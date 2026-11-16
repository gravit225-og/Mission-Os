# Mission OS Complete Build Roadmap

**Version:** 1.0  
**Date:** 2026-11-16  
**Source of truth:** [Master Specification v2](MASTER_SPEC_V2.md)

This is the dependency-aware execution plan for building Mission OS. It is milestone-based, not calendar-based: each stage is complete only when its exit tests pass.

## Build strategy

Build in vertical slices: data model → validation/API → interface → tests → documentation. Keep a working deployable version at each milestone. Do not build mock dashboards disconnected from real data, and do not start all modules in parallel.

| Stage | Milestone | Outcome |
|---|---|---|
| 0 | Product and repo setup | Scope, backlog, environments and acceptance criteria ready |
| 1 | Technical validation | PWA, editor, backend security and offline approach proven |
| 2 | Secure app foundation | Installable app, authentication, database and protected routes |
| 3 | Workspace core | Pages, nested tree, block editor, autosave and links |
| 4 | Capture and journal | Unified personal memory usable daily |
| 5 | Search and portability | Search, export, backup and restore |
| 6 | Productivity engine | Tasks, Today, habits, focus and rewards |
| 7 | Goals and projects | Long-term planning connected to daily execution |
| 8 | Custom databases | Properties, views, relations and templates |
| 9 | Tracking and reflection | Custom trackers, reviews and analytics |
| 10 | ChatGPT integration | Read-only MCP first, then approved writes |
| 11 | Release hardening | Security, accessibility, performance and deployment |
| 12 | Advanced evolution | Import, advanced sync, formulas and integrations |

---

## Stage 0 — Product, repository and delivery setup

**Tasks**
- Audit repository contents and reconcile older docs with Master Specification v2.
- Confirm private, single-owner initial model and target Android browser.
- Create a prioritized GitHub issue backlog with dependencies.
- Add issue templates and milestone labels.
- Define development, preview and production environments.
- Decide hosting and deployment workflow.
- Define secrets handling; never commit environment files or credentials.
- Define acceptance tests before implementation.

**Deliverables:** linked product docs, prioritized backlog, environment/deployment decision, decision log.

**Exit gate:** the project can be run locally by following its setup instructions, and the next approved issue is unambiguous.

## Stage 1 — Technical validation spike

Build disposable prototypes to test expensive decisions before the main implementation.

### 1A. PWA and frontend
- Scaffold a minimal React + TypeScript + Vite app.
- Test responsive layouts on narrow phone, typical Android and desktop widths.
- Test installation, app-shell caching, safe areas, keyboard resizing and touch behavior.

### 1B. Editor comparison
Prototype the same sample in BlockNote and Tiptap:
- Paragraphs, headings, lists, checklists, quote, code, divider and tables.
- Image/file attachments and internal page-link placeholder.
- Paste from web and plain text.
- Undo/redo and block reordering.
- Structured JSON save/load.
- Markdown import/export and known conversion losses.
- Android keyboard, selection and scrolling behavior.
- Extensibility, accessibility and bundle-size comparison.

Choose an editor only after this test. Store versioned structured JSON as canonical content; Markdown is an interchange format.

### 1C. Backend and security
- Create a development PostgreSQL/Supabase project.
- Test authentication and one owner-scoped table.
- Enable RLS and explicitly configure grants.
- Test allowed owner access and denied second-user access.
- Verify service-role secrets never enter browser code or bundles.
- Test migrations from a clean database.

### 1D. Offline proof
- Save a draft in IndexedDB and recover it after reload.
- Queue a create operation offline and sync exactly once after reconnect.
- Test retry, duplicate submission, expired authentication and server failure.
- Decide whether record versions are sufficient initially or whether document collaboration needs Yjs.

**Deliverables:** working spike, editor comparison, security proof, offline test report, updated architecture decisions.

**Exit gate:** no unresolved critical data-loss, editor, security or target-device risk.

## Stage 2 — Secure application foundation

**Build**
- React, TypeScript, Vite, linting, formatting, type checks and tests.
- Feature-oriented project structure.
- Midnight Workshop design tokens and accessible UI primitives.
- Routing, protected app shell and responsive navigation.
- Sign-in, sign-out, session restoration and auth-expiry handling.
- Workspace creation and owner membership.
- Database migrations, RLS policies and RLS tests.
- PWA manifest, icons and service worker.
- IndexedDB local store and safe static asset caching.
- Loading, empty, error and retry states.
- CI checks and preview deployment.

**Screens:** authentication, first-run setup, Today shell, Workspace shell, Settings.

**Exit gate:** app installs on the target Android browser; protected routes work; RLS proves one user cannot read or modify another user's records; no secrets are exposed.

## Stage 3 — Workspace and page foundation

**Data**
- Workspace/entity identity.
- Page metadata and parent-child relationships.
- Versioned page documents.
- Draft/revision recovery.
- Favorites, archive, trash and stable ordering.

**Features**
- Create, rename, move, duplicate, archive, restore and delete pages.
- Nested page tree and breadcrumbs.
- Favorites and recent pages.
- Page title, icon and optional cover.
- Block editor with approved core blocks.
- Slash menu, contextual insertion, keyboard shortcuts.
- Drag reorder plus accessible non-drag alternative.
- Autosave and visible save/sync state.
- Undo/redo and draft recovery.
- Internal page links and backlinks.
- Desktop side peek; mobile full-page navigation.

**Tests:** nested page operations, JSON round-trip, refresh recovery, simulated failures, undo/redo, link integrity and schema migration.

**Exit gate:** the owner can create, edit, organize, link and reliably reopen real pages.

## Stage 4 — Universal capture and unified journal

**Build**
- Global Quick Capture.
- Capture types: note, idea, journal, event, task, measurement.
- Fast save with title/body only; metadata optional.
- Journal entry types: daily journal, quick note, idea, event, learning, decision, reflection, project update, milestone and other.
- Unified chronological timeline.
- Filters by type, tag, date and linked project.
- Daily journal template.
- Offline capture queue and sync status.
- Private file attachments.
- Connector privacy flag for pages/entries.

**UX rules:** capture from anywhere; save before categorization; unsynced state is visible; timeline opens canonical records.

**Tests:** quick phone capture, offline save/reconnect, no duplicate sync, timeline filters, edit old entries, connector privacy exclusion.

**Exit gate:** Mission OS becomes useful daily as a personal capture and journal system.

## Stage 5 — Search, export, backup and restore

**Search**
- Global search across page titles/body, journal entries, tags and indexed record titles.
- Group results by entity type.
- Filters by date, area, project and type.
- Recent searches and desktop shortcut.
- Results open canonical records.

**Portability**
- Full-fidelity workspace JSON with schema version and stable IDs.
- Markdown/HTML page export.
- CSV export for tabular records.
- Original attachments and manifest.
- Export by workspace, area, project or date range.
- Restore into a clean test workspace.
- Document unsupported/lossy conversions.

**Backups**
- Automated database backup strategy.
- Separate user-triggered export.
- Restore drill before real daily use.
- Clear deletion and retention policy.

**Exit gate:** owner can find information and export/restore without depending on the live service.

## Stage 6 — Daily productivity engine

### Tasks
- Inbox, next, in-progress, waiting, done and cancelled statuses.
- Priority, due date, estimate, tags, description and subtasks.
- Project/goal relations.
- Today, Upcoming, Board and calendar-ready views.
- Defer/reschedule actions.

### Today dashboard
- Main Mission and top three priorities.
- Habits due today.
- Quick actions.
- Recent milestones/activity.
- Configurable sections and ordering.
- Helpful empty states, not a wall of widgets.

### Habits and routines
- User-defined cadence.
- Binary or quantity logs.
- Backfill and history.
- Optional routines with steps.
- No penalty for missed days.
- Correct timezone/local-date behavior.

### Focus and easy start
- Two-minute starter.
- Optional 10-minute momentum step.
- Configurable longer sessions.
- Link session to task/project.
- Pause, stop, resume and interruption states.
- Timestamp-based timer behavior after backgrounding.

### Rewards
- Idempotent reward ledger.
- XP for meaningful starts, completed focus, tasks, habits and milestones.
- No XP for app opening or button tapping alone.
- No negative XP, streak decay or lost progress.
- Rewards can be disabled.

**Exit gate:** user can plan a day, start meaningful work, log habits/focus and see truthful progress without punitive mechanics.

## Stage 7 — Goals, projects, milestones and calendar

**Goals:** qualitative or quantitative outcome, why it matters, optional target date, status, progress method, linked projects/milestones and review history.

**Projects:** overview, purpose, status, next milestone/action, linked tasks, documents, journal entries, decisions, trackers and activity timeline.

**Milestones:** title, date, status, notes and project relation; completion appears in personal timeline.

**Calendar:** real events with timezone; render task due dates and milestones from canonical records; day/week/month/agenda views; avoid duplicate calendar records for tasks. Add recurrence only after single-event behavior is stable.

**Exit gate:** long-term goals/projects connect to daily actions and personal notes without duplicated data.

## Stage 8 — Custom database engine

Build only after page identity, search, relations and permissions are stable.

**Sequence**
1. Database pages and database item records.
2. Property definitions and type validation.
3. Property values and select options.
4. Table view.
5. Filters, sorts and saved views.
6. List view.
7. Board view and grouping.
8. Calendar view.
9. Gallery view.
10. Database item opens as a page.
11. Embedded linked database views.
12. Relation properties and related-record picker.
13. Record templates.
14. Bulk editing and CSV export.

**Initial properties:** title, text, number, select, multi-select, status, date, checkbox, URL, relation, file, created/updated time. Formula and rollup are later.

**Guardrails:** no arbitrary SQL or executable formulas; views are query configurations, not copies; property type changes validate existing values; relation targets must belong to the authorized workspace.

**Exit gate:** owner can create a useful custom database and multiple views without code changes.

## Stage 9 — Flexible tracking, reflection and analytics

**Trackers**
- User-defined tracker name, type, unit, precision, optional target and cadence.
- Values: number, duration, boolean, rating and short text.
- Multiple entries per day where allowed.
- Historical edits and notes.
- Links to areas/projects.
- Missing values remain missing, never silently converted to zero.

**Reviews**
- Daily, weekly, monthly and quarterly templates.
- Optional, editable and configurable prompts.
- Partial reviews are valid.
- Link reviews to journal, goals, projects and trackers.

**Analytics**
- Habit completion with explicit denominator.
- Task throughput/status.
- Focus time.
- Metric trends.
- Project milestone history.
- Journal/timeline activity.
- Date-range comparisons and data export.

**Exit gate:** personal history and trends are reliable, with transparent calculations and no invented interpretations.

## Stage 10 — ChatGPT integration

The connector is a separate service/package. Mission OS itself contains no AI features.

### 10A. Stable API
- Versioned authenticated API.
- User-scoped queries.
- Pagination and bounded responses.
- Stable record IDs.
- Read models for Today, pages, journal, tasks, projects, habits, metrics and reviews.
- Rate limits and audit events.

### 10B. MCP read-only release
Initial tools:
- get_today
- search_workspace
- get_page
- list_journal_entries
- get_task / list_tasks
- get_project
- list_habits
- get_tracker / list_metric_entries
- list_reviews
- get_activity_history

Respect connector-private flags across search, lists, relations, backlinks and summaries.

### 10C. Scoped writes, only after read security passes
- create_note
- create_task
- log_habit
- add_metric_entry
- save_reflection
- update_selected_record

Require separate read/write scopes, validation, idempotency keys, audit log, narrow schemas and revocation. No arbitrary SQL, bulk deletion or destructive actions in early releases.

**Tests:** cross-user denial, private-entry exclusion, prompt injection in retrieved notes cannot grant permissions, pagination, rate limits, duplicate-write prevention and revoked-token denial.

**Exit gate:** ChatGPT can access only approved records and perform narrow authorized actions.

## Stage 11 — Release hardening

- Unit tests for domain logic and validation.
- Integration tests for API/database behavior.
- RLS tests for every exposed table.
- Browser end-to-end tests for core journeys.
- Target Android device tests.
- WCAG 2.2 AA accessibility review.
- Performance profiling on modest hardware/network.
- Large workspace and search tests.
- Minimal-data error monitoring.
- Dependency and secret scanning.
- Test expired auth while offline, interrupted uploads, duplicate sync, backup restore, migrations, deletion and connector revocation.
- Production environment, HTTPS, backups, monitoring, privacy notice, export/deletion instructions, release notes and rollback plan.

**Exit gate:** critical acceptance tests pass and no high-severity privacy, data-loss or authorization defects remain.

## Stage 12 — Advanced evolution

Only add these when real usage demonstrates a need:
- Notion export importer with preview/mapping.
- Advanced blocks and synced blocks.
- Timeline/chart database views.
- Restricted formula language and rollups.
- Yjs/CRDT multi-device document editing if justified.
- External calendar integrations.
- Optional reminders and notifications.
- Voice capture as an input method.
- Additional data integrations.
- Multiple workspaces.
- Collaboration/sharing only if product direction changes.

Do not add social feeds, public profiles, leaderboards or engagement loops by default.

---

## First 20 implementation issues

1. Audit repository and reconcile documentation.
2. Add GitHub issue templates and milestone labels.
3. Scaffold React + TypeScript + Vite.
4. Add lint, type-check, test and production-build scripts.
5. Create Midnight Workshop design tokens.
6. Build responsive app shell and navigation.
7. Configure PWA manifest and icons.
8. Set up backend project and environment separation.
9. Add authentication and protected routes.
10. Create workspace/entity migrations.
11. Add RLS policies and cross-user denial tests.
12. Prototype BlockNote editor.
13. Prototype Tiptap comparison sample.
14. Record editor decision.
15. Create pages and page_documents tables.
16. Implement page CRUD and nested page tree.
17. Implement editor JSON persistence and autosave.
18. Add IndexedDB draft recovery.
19. Build Quick Capture and journal records.
20. Add unified journal timeline and basic search.

Each issue must include purpose, dependencies, acceptance criteria, tests and documentation updates.

## Definition of done for every feature

A feature is complete only when:
- Data model and migrations are committed.
- Validation exists at the appropriate boundary.
- Permissions and RLS are tested.
- Loading, empty, error and success states exist.
- Phone and desktop behavior are checked.
- Offline behavior is explicit.
- Search and export implications are considered.
- Tests pass in CI.
- Documentation is updated.
- It works with real persisted data.

## Development operating rules

- Maintain one prioritized backlog.
- Build small vertical slices: schema → API → UI → tests → docs.
- Do not build mock dashboards disconnected from real data.
- Do not add AI features inside Mission OS.
- Do not build MCP before stable APIs and permissions.
- Do not build advanced databases before pages and relations work.
- Keep a working deployable version at every major milestone.
- Use the app at each daily-use milestone; turn friction into issues.
- Reprioritize from observed friction, not feature excitement.

## Immediate next step

Start Stage 1 with a technical spike, not the full feature build. Inspect the actual repository and local development workflow, scaffold the smallest PWA, then test editor, backend and offline choices. Once those gates pass, build the real foundation in Stage 2.
