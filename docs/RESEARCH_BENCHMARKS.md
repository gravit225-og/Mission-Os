# Mission OS Research and Product Benchmark Notes

**Date:** 2026-11-16  
**Purpose:** Record product patterns and technical evidence used in Master Specification v2.

This document separates observed product capabilities from Mission OS recommendations. It is not a claim that one competitor is universally best.

## 1. Notion: structured workspace patterns

Official Notion help documents describe:
- Databases as collections of pages, where an item can open as a full page.
- Database properties for adding structured context to records.
- Multiple views of the same database, including table, board, timeline, calendar, list, gallery, and chart.
- Per-view filters, sorting, grouping, and property visibility.
- Relations to connect database items and rollups to aggregate related properties.
- Database templates that reuse page structures and default property values.

Mission OS lesson:
- A database row should be a full record/page, not a dead spreadsheet cell.
- Views must be saved presentations of the same records, not duplicated lists.
- Relations are core infrastructure, not a later cosmetic feature.
- Templates reduce repetitive setup.
- Database and page content should share the same workspace identity and linking model.

Official sources:
- https://www.notion.com/help/what-is-a-database
- https://www.notion.com/help/views-filters-and-sorts
- https://www.notion.com/help/database-properties
- https://www.notion.com/help/guides/using-relation-and-rollup-properties
- https://www.notion.com/help/database-templates

## 2. Obsidian: ownership and plain-file portability

Obsidian's official help explains that notes are Markdown-formatted plain text files stored in a local vault folder, which can be edited by other tools. It also documents properties in notes.

Mission OS lesson:
- Personal content should not be trapped in one opaque export.
- Full-fidelity JSON can preserve advanced structures, while Markdown/HTML remain human-readable export options.
- Stable IDs and an explicit schema version are required for relations and restore.
- Users should be able to retrieve and export their content even if they leave the service.

Official sources:
- https://obsidian.md/help/data-storage
- https://obsidian.md/help/properties

## 3. Anytype: local-first ownership

Anytype's published documentation repo describes a local-first philosophy, offline work, syncing across a user's devices, and user control over personal data.

Mission OS lesson:
- Capture should happen locally before sync.
- Offline state must be visible and recoverable.
- The app should treat export and user control as product capabilities.
- Local-first reliability is a design direction, but it still needs a realistic conflict model and backup strategy.

Source:
- https://github.com/anyproto/docs

## 4. Block editors: BlockNote and Tiptap

BlockNote describes itself as a React-oriented block editor with an out-of-the-box experience, custom block types, and customizable UI.

Its Markdown import documentation warns that conversion can be lossy and recommends storing the editor's native structured block format for non-lossy persistence.

Tiptap describes itself as a headless, schema-based editor with modular extensions. Its current Markdown extension documentation labels the feature beta. Its ecosystem documents Yjs-backed collaboration and IndexedDB persistence for offline editing.

Mission OS lesson:
- Prototype BlockNote first for faster block-editor UX.
- Compare Tiptap if custom schemas, extension control, or collaboration needs are stronger.
- Store a versioned canonical structured document format.
- Markdown is an import/export format, not the only database source of truth.
- Test paste, tables, task lists, images, links, and mobile editing before locking editor dependency.

Sources:
- https://www.blocknotejs.org/docs
- https://www.blocknotejs.org/docs/features/import/markdown
- https://tiptap.dev/docs
- https://tiptap.dev/docs/editor/core-concepts/schema
- https://tiptap.dev/docs/editor/markdown
- https://tiptap.dev/docs/guides/offline-support

## 5. Offline PWA and document sync

MDN's PWA documentation explains that service workers support offline and background patterns, but browsers can stop a service worker when inactive. Yjs documents an IndexedDB persistence provider for local document updates; Tiptap documents an offline editing approach based on Yjs and y-indexeddb.

Mission OS lesson:
- A service worker caches the application shell; IndexedDB stores actual offline data.
- Never assume a service worker runs continuously.
- The app should sync when foregrounded/reconnected, with best-effort background sync.
- Use idempotency keys for queued operations.
- Start with versioned records and explicit conflict handling.
- Prototype Yjs for rich page documents if cross-device offline editing justifies CRDT complexity. Do not apply CRDT to every structured database table by default.

Sources:
- https://developer.mozilla.org/en-US/docs/Web/Progressive_web_apps/Guides/Offline_and_background_operation
- https://docs.yjs.dev/getting-started/allowing-offline-editing
- https://tiptap.dev/docs/guides/offline-support

## 6. Security and database authorization

Supabase's current RLS documentation explains that row-level security and database grants work together. Policies do not automatically revoke grants. It also warns that views can bypass RLS by default unless created with appropriate security settings.

Mission OS lesson:
- Every exposed user-owned table needs explicit grants and RLS policies.
- Add tests for allow and deny paths.
- Review views/materialized views separately.
- Service-role keys must remain server-side.
- Connector access must have additional scope and privacy checks.

Source:
- https://supabase.com/docs/guides/database/postgres/row-level-security

## 7. Habit and behavior-change evidence

A 2024 systematic review of 41 digital behavior-change studies found common use of self-monitoring, goal setting, prompts/cues, descriptive feedback, and positive reinforcement.

Mission OS lesson:
- A useful habit system combines self-set goals, lightweight logging, feedback and cues.
- Rewards should reinforce meaningful action rather than app opening or repeated tapping.
- Missed logs should not automatically imply failure.
- Do not create an app that pressures continuous tracking.

Peer-reviewed source:
- https://www.jmir.org/2024/1/e54375

## 8. ChatGPT and MCP integration constraints

OpenAI's current help documentation describes custom MCP connections as remote-server integrations, says connection management is web-only, and documents plan/feature availability differences. It also explains that write actions can trigger confirmations depending on permissions and action risk.

Mission OS lesson:
- Do not make Mission OS depend on custom connector availability in a particular ChatGPT plan or mobile client.
- Build a provider-neutral, authenticated API first.
- Add a remote MCP adapter as a separate integration.
- Start read-only, then add narrow scoped writes.
- Treat page/journal text retrieved by tools as untrusted user data, not tool instructions.
- Re-check ChatGPT plan and connector support at the time integration is implemented.

Source:
- https://help.openai.com/en/articles/12584461-connecting-custom-mcp-servers-in-chatgpt

## 9. Product synthesis

The product patterns combine into these Mission OS requirements:

| Pattern | Mission OS implementation |
|---|---|
| Notion pages | Workspace pages and nested page tree |
| Notion databases | Custom databases where each item opens as a page |
| Notion views | Saved table/list/board/calendar/gallery views |
| Notion relations and rollups | Shared relation model and later rollups |
| Notion templates | Page/database/project/review templates |
| Obsidian local files | Portable Markdown/HTML export and stable IDs |
| Anytype local-first idea | Local capture, offline queue, explicit sync state |
| BlockNote block editor | Prototype for block-first editor UX |
| Tiptap schemas/extensions | Alternative if deeper custom editor control is needed |
| Yjs/IndexedDB | Candidate for page-document offline synchronization |
| Habit research | Self-monitoring, goals, cues, descriptive feedback, positive reinforcement |
| MCP | External ChatGPT read/write access with authorization and audit |

No competitor's full product should be copied. The goal is a coherent personal system with a simpler daily surface and a trustworthy data foundation.
