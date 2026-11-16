# Mission OS Architecture Decision Record Set

**Date:** 2026-11-16  
**Status:** Provisional decisions and required validation

## ADR-001: PWA first

**Decision:** Build a mobile-first installable PWA before a native Android app.

**Reasoning:** One responsive codebase can serve phone and desktop. The first user needs reliable capture, pages, and productivity features more than native-only capabilities.

**Validation:** Check installation, keyboard behavior, safe areas, storage persistence, app-shell cache, upload and offline sync on the target Android browser.

**Revisit if:** The PWA cannot meet practical needs for notifications, background processing, file access, or performance.

## ADR-002: PostgreSQL as canonical structured database

**Decision:** Use PostgreSQL as the canonical server-side record store, with Supabase as the leading hosted candidate.

**Reasoning:** Pages, properties, tasks, projects, habits, metrics, relations and permissions require structured relationships and transactions.

**Security rule:** Enable RLS and set grants together on every exposed table. Test read/write allow and deny paths. Review views for security behavior. Never expose service-role credentials in the browser.

**Validation:** Build a small authenticated prototype and test cross-user access before storing personal data.

## ADR-003: BlockNote first, subject to editor spike

**Decision:** Prototype BlockNote first; compare Tiptap before final dependency lock.

**Reasoning:** BlockNote provides a block-first React editor with a faster out-of-the-box UX. Tiptap offers deeper schema/extension control and a wider modular editor model.

**Canonical format:** Versioned structured JSON. Markdown/HTML are export/interchange formats and may be lossy.

**Required spike:** Test headings, lists, checklists, tables, image/file attachment, page links, database embeds, paste from web, undo/redo, mobile keyboard behavior, editor performance, export, and schema migration.

## ADR-004: Typed domain records plus flexible custom databases

**Decision:** Use specialized validated tables for core concepts and a generic property engine for user-created databases.

**Reasoning:** Tasks, habits, events and measurements require validation and reliable aggregation. A custom database engine provides flexibility for information the product team cannot predict.

**Rule:** Do not store every feature as arbitrary JSON. Use JSON for flexible view configuration and page document structure, with typed validation for values that need sorting, filtering, and aggregation.

## ADR-005: Local capture first, not unbounded local-first replication

**Decision:** Persist drafts and offline creates in IndexedDB. Sync using idempotent operations and record versions. Add document CRDT only if prototype proves it is needed.

**Reasoning:** The first product is private and single-owner. Reliable capture is more important than complex real-time collaboration.

**Validation:** Two-device offline edit tests, duplicate-submit tests, expired-session tests, and conflict resolution tests.

**Revisit if:** Users frequently edit the same page across multiple devices while offline and explicit conflict resolution proves inadequate.

## ADR-006: ChatGPT integration is a separate adapter

**Decision:** Build the product API first, then a separate MCP server.

**Reasoning:** This avoids dependence on one model vendor or on current ChatGPT plan availability.

**Security:** Read-only first. Separate read/write scopes. User-scoped access. Connector-private content excluded. Narrow tools only. Audit all external writes. No destructive/bulk writes in early releases.

## ADR-007: MVP priorities

**Decision:** First alpha is Workspace + Pages + Block Editor + Quick Capture + Journal + Search + Export + offline drafts.

**Reasoning:** This is the smallest useful foundation that can become a daily personal workspace. The all-in-one product is the target, not the Alpha 0.1 scope.

**Do not include in Alpha 0.1:** full custom database engine, formula engine, social features, native app, AI assistant, MCP write tools, complicated calendar scheduling.

## ADR-008: Visual identity

**Decision:** Keep Midnight Workshop as the initial design system: dark graphite surfaces, warm rose accent, restrained teal/amber semantic colors, accessible type and mobile-first responsive layout.

**Revisit:** After a real interactive prototype has been tested on the user's phone.
