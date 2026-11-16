# Mission OS Master Specification v2

**Product:** Mission OS  
**Specification:** Master Specification v2.0  
**Status:** Product direction and target architecture  
**Date:** 2026-11-16  
**Owner:** Personal product, initially single-owner  
**Companion documents:** WORKSPACE_DATA_MODEL.md, RESEARCH_BENCHMARKS.md, IMPLEMENTATION_DECISIONS.md, DESIGN_SYSTEM.md, SCREEN_LAYOUTS.md

---

## 1. Executive definition

Mission OS is a private, modular, all-in-one personal workspace and life-management system. It combines a flexible knowledge workspace, personal information management, planning and execution, journaling, tracking, reflection, and historical records in one connected environment.

It is designed to become the owner's primary place to:
- Capture thoughts, ideas, events, notes, files, and tasks.
- Create pages, documents, databases, and reusable templates.
- Organize knowledge into areas, projects, and collections.
- Plan goals, tasks, habits, routines, and focus sessions.
- Record personal activity, learning, decisions, and milestones.
- Track user-defined measurements.
- Review progress and personal history.
- Retrieve and export personal data.
- Allow external tools such as ChatGPT to access explicitly authorized information.

### Product promise

**Your life and work, organized in one connected system, without forcing your life into a rigid system.**

Mission OS must be useful without AI. It contains no built-in AI chatbot, AI coach, AI-written reflection feature, AI semantic search, or AI insight generator. ChatGPT is an optional external intelligence layer connected through a secure API/MCP interface when the user's account and client support it.

### Product hierarchy

1. **Workspace:** pages, documents, databases, files, templates, links.
2. **Life management:** goals, projects, tasks, calendar, habits, routines, focus.
3. **Personal memory:** journal, notes, ideas, events, decisions, learning, milestones.
4. **Tracking and reflection:** custom metrics, reviews, deterministic analytics.
5. **External intelligence:** authorized ChatGPT access, separate from the app runtime.

These are connected views over shared data, not unrelated mini-apps.

## 2. Product decisions already locked

| Decision | Product choice |
|---|---|
| Platform | Mobile-first installable PWA |
| Primary device | Android phone, with adaptive desktop/tablet |
| Ownership | Private personal workspace, initially one owner |
| Journal | Unified timeline with types, tags, filters, and links |
| Tracking | Flexible, user-created trackers and typed values |
| Cloud | Private authenticated storage with a portable export |
| Offline | Reliable local capture and queued sync are required |
| Built-in AI | None |
| External AI | ChatGPT via separately authorized API/MCP |
| Visual system | Midnight Workshop, dark graphite with warm rose accent |
| Product scope | All-in-one life and productivity workspace, built in controlled phases |

Technical providers and editor library remain provisional until the compatibility spike and prototype described in this specification are complete.

## 3. Product principles

1. **Capture first, organize later.** The default capture path asks for the least information possible.
2. **One source of truth.** One task, note, or project can appear in multiple views without being copied.
3. **Flexible structure.** Users can create their own pages, properties, databases, tags, areas, and templates.
4. **Clear structure when useful.** Specialized records such as tasks, habits, events, and measurements use validated fields rather than being free-form text only.
5. **Easy starts.** Make it obvious how to begin the next meaningful action.
6. **Progress without punishment.** No negative XP, lost rewards, shame, forced streaks, or penalties for rest.
7. **Personal data belongs to the owner.** Export, backup, deletion, privacy, and connector permissions are first-class features.
8. **Portable records.** Use stable IDs, documented schemas, and open export formats.
9. **AI-independent core.** The app remains fully useful if ChatGPT is unavailable.
10. **Power underneath, simplicity on the surface.** Advanced database and planning controls should not clutter basic capture and daily use.
11. **Truthful analytics.** Missing data is not zero; estimates and interpretations are not facts.
12. **No compulsive engagement design.** Do not optimize for screen time, app opens, notification clicks, or endless streaks.

## 4. Primary user jobs

Mission OS should make these jobs easy:

- Save a quick thought before it is forgotten.
- Write a detailed document or journal entry.
- Organize pages under folders, parent pages, or custom areas.
- Create a database to manage a collection of information.
- View the same database as a table, board, calendar, list, gallery, or timeline.
- Connect records across projects, pages, tasks, goals, and journal entries.
- Find a page, record, or phrase from anywhere in the workspace.
- See today's meaningful priorities without browsing the entire workspace.
- Turn a large goal into a project and a project into next actions.
- Track habits without being punished for missing a day.
- Record custom metrics without needing a developer to add a feature.
- Keep a searchable personal history of events, learning, and decisions.
- Reflect on a day, week, month, or quarter.
- Use the app with weak or no internet for core capture workflows.
- Export and restore the full workspace.
- Let ChatGPT analyze selected records with permission.

## 5. Information architecture

### 5.1 Primary navigation

**Today**
- Daily dashboard
- Main Mission
- Daily priorities
- Habits due today
- Quick Actions
- Recent milestones/activity

**Workspace**
- Workspace home
- Favorites
- Recently opened
- Page tree / folders
- Areas of Life
- Databases
- Templates
- Files

**Missions**
- Task inbox
- Today
- Upcoming
- Projects
- Goals
- Milestones
- Focus sessions

**Journal**
- Unified timeline
- Daily entries
- Notes and thoughts
- Ideas
- Events
- Learning logs
- Decisions
- Reflections

**Track & Review**
- Habits
- Routines
- Custom trackers
- Calendar
- Daily/weekly/monthly/quarterly reviews
- Analytics
- Activity history

**Manage**
- Search and tags
- Achievements
- Settings
- Privacy
- Export and backup
- External connections

On phones, use a compact bottom navigation for Today, Workspace, Capture, Journal, and Manage. Secondary modules are accessible from their relevant hubs. On desktop, use a grouped sidebar and page tabs/breadcrumbs. Do not simply shrink the desktop sidebar onto a phone.

### 5.2 Areas of Life

Areas are user-defined long-lived categories that help organize pages, projects, goals, and databases. A starter workspace may offer examples such as Learning, Personal, Creative Work, Projects, Science, and Health & Wellbeing, but users can rename, remove, or create their own.

Areas are organizational containers, not mandatory classification fields. A record can belong to more than one area through explicit links.

### 5.3 Workspace tree

A workspace contains:
- Pages
- Nested pages
- Databases
- Collections / folders
- Templates
- Uploaded files
- Favorites
- Archived and deleted items

A database may be a full page or an embedded view within another page. Database rows open as pages with their own document body.

## 6. Workspace engine

The workspace engine is the foundation of the product. Life-management modules must be able to reuse its page, record, relation, search, and permission infrastructure.

### 6.1 Pages

Every page has:
- Stable page ID.
- Title.
- Optional icon and cover.
- Optional parent page.
- Page type (regular page, database page, database item, system page, or template).
- Optional area/project association.
- Created and modified timestamps.
- Favorite/archive/trash state.
- Optional page-level privacy restriction.
- Editable document content.

Required behaviors:
- Create, rename, move, duplicate, archive, restore, and delete pages.
- Nest pages.
- Breadcrumb navigation.
- Back/forward page history.
- Favorites and recently opened pages.
- Page links and backlinks.
- Page-level search.
- Open in full page; optional side panel on wide screens.
- Safe deletion with recoverable trash before permanent deletion.

### 6.2 Block editor

The editor is block-based. Each block has a stable ID, type, attributes, and content/children where appropriate.

Initial block set:
- Paragraph
- Heading 1, 2, 3
- Bulleted list
- Numbered list
- Checklist / to-do
- Toggle / collapsible section
- Quote
- Callout
- Divider
- Code block with language
- Image
- File attachment
- Link / bookmark
- Simple table
- Page link / mention
- Embedded database view
- Date/reference block where useful

Later block types:
- Columns
- Equation
- Advanced table
- Audio attachment
- Video/embed
- Synced blocks
- Custom widgets

Editor requirements:
- Slash menu for inserting blocks.
- Drag/reorder blocks, with non-drag alternatives.
- Keyboard shortcuts and mobile-friendly contextual menus.
- Undo/redo.
- Paste from web and other documents with safe sanitization.
- Autosave and visible save/sync status.
- Draft recovery after navigation or reload.
- Stable links to other pages/records.
- Block-level selection and deletion.
- Accessible editing and keyboard focus.

Canonical content storage is versioned structured JSON, not rendered HTML. Markdown and HTML are import/export formats. Because editor conversions can be lossy, full-fidelity backup must preserve the canonical structured document format and its schema version.

### 6.3 Databases

A database is a collection of records. Each record has a stable identity and can open as a full page with its own body content.

Database requirements:
- Create, rename, duplicate, archive, and delete a database.
- Add, rename, reorder, hide, and remove properties.
- Configure property type and validation.
- Create multiple saved views of the same records.
- Link a database view into another page.
- Open a record in side peek, center dialog, or full page.
- Filter, sort, and group records.
- Bulk update supported properties.
- Search within the database.
- Create records from templates.
- Export database data with schema and relation metadata.

### 6.4 Property types

Initial core types:
- Title
- Text / rich text
- Number
- Select
- Multi-select
- Status
- Date / date range
- Checkbox
- URL
- Email
- Relation
- File / media
- Created time
- Last edited time

Later / advanced:
- Person (only if sharing is introduced)
- Formula
- Rollup
- Button/action
- Created by / edited by
- Progress calculation

Property type changes must validate existing values and warn before destructive conversion. A user must be able to undo or restore a previous property schema when possible.

### 6.5 Database views

Initial views:
- Table
- List
- Board / Kanban
- Calendar
- Gallery

Later views:
- Timeline
- Chart
- Form
- Custom dashboard view

Each saved view has its own:
- Name and icon.
- Layout type.
- Filters.
- Sort order.
- Grouping/subgrouping.
- Visible properties.
- Card preview/layout preferences.
- Default open behavior.

A view is a saved query and presentation of the same records. It must not make copies of database rows.

### 6.6 Relations and rollups

Relations connect records across databases or to pages/projects/goals. A relation should support one-way or reciprocal display where appropriate, with ownership and target validation.

Initial relation capabilities:
- Link records.
- Show linked record title and type.
- Open linked record.
- Search/pick linked record.
- Show backlinks.
- Prevent links across users/workspaces without an explicit sharing model.

Rollups are a later milestone. Initial rollup candidates:
- Count related tasks.
- Count completed tasks.
- Sum a numeric property.
- Average a numeric property.
- Earliest/latest date.
- Show a selected related value.

Formula evaluation is not MVP. When added, formulas must use a restricted, documented expression language, not arbitrary JavaScript execution.

### 6.7 Templates

Template types:
- Page template.
- Database record template.
- Project template.
- Meeting / event template.
- Daily / weekly review template.
- Habit / routine template.
- Custom user template.

Templates may contain page blocks, default property values, and optional linked structures. Repeated templates and scheduled creation are later enhancements.

### 6.8 Search and navigation

Global search should search:
- Page titles and body text.
- Journal entries.
- Database record titles and searchable properties.
- Tasks and project names.
- Tags.
- Decisions and learning logs.

Search requirements:
- Results grouped by entity type.
- Recent searches.
- Filters by type, area, project, date.
- Keyboard shortcut on desktop and easy access on mobile.
- Search results open the canonical record.
- Private-from-connector is a separate API permission, not a reason to hide content from the owner.
- Initial search can use PostgreSQL full-text search; advanced fuzzy or semantic search is not part of the built-in app.

### 6.9 Links, backlinks, and related records

Support:
- Internal page links.
- Mentions of pages/records.
- Backlinks.
- Related records panel.
- Explicit relationships such as related_to, part_of, documents, depends_on, and follows_up.
- A graph visualization is optional and should not replace a practical list of related records.

## 7. Life management system

### 7.1 Tasks

Task fields:
- Title and description.
- Status: inbox, next, in progress, waiting, done, cancelled.
- Priority.
- Due date/time.
- Optional start date.
- Estimate and actual duration.
- Next action.
- Project/goal/area relations.
- Tags.
- Created/completed timestamps.

Views:
- Inbox
- Today
- Upcoming
- Board by status
- Calendar
- Project task list
- Completed / archive

Requirements:
- Quick add from anywhere.
- Daily top three / Main Mission selection.
- Subtasks and checklists.
- Optional dependencies later.
- Defer/reschedule without guilt language.
- Task is one record displayed in many places.

### 7.2 Goals

Goal contains:
- Outcome statement.
- Why it matters.
- Optional target date.
- Status (active, paused, achieved, abandoned).
- Progress method.
- Related projects and milestones.
- Review history.

Do not equate every personal desire with a measurable numerical goal. Allow qualitative goals.

### 7.3 Projects

Projects connect goals, tasks, documents, journal entries, decisions, trackers, and milestones.

Project page should show:
- Overview and purpose.
- Status and next milestone.
- Main next action.
- Linked tasks.
- Project notes and documentation.
- Decisions and experiments.
- Milestones.
- Timeline/activity history.
- Relevant trackers.

### 7.4 Calendar and events

Calendar brings together dated items without turning the app into a complex scheduling product too early.

Initial calendar supports:
- User-created events.
- Task due dates.
- Project milestones.
- Habit schedule overview.
- Review periods.
- Day, week, month, and agenda views.

Later:
- Time-blocking.
- Recurring events.
- External calendar integrations.
- Conflict detection and schedule suggestions.

Events are records with start/end, timezone, title, location/meeting link optional, notes, and relations. Calendar views must render a shared event/task source, not separate copies.

### 7.5 Habits and routines

Habits:
- Flexible frequency and selected weekdays.
- Binary or quantity-based log.
- Optional note.
- Backfill.
- History/calendar.
- Completion rate with clear denominator.
- Missed or absent logs are not automatically failure.

Routines:
- Named reusable sequence of steps.
- Optional schedule or trigger.
- Steps may link to tasks/habits.
- User starts, skips, or stops a routine without penalty.
- Routine completion does not imply every underlying item is complete unless logged.

### 7.6 Focus sessions and easy starts

- Short starter session, default two minutes.
- Optional 10-minute momentum step.
- Configurable longer focus session.
- Session can link to a task or project.
- Store timestamps, planned/actual duration, outcome, and notes.
- Timer recalculates from timestamps after backgrounding.
- Allow stop, pause, resume, interruption and completion.
- Do not force a longer session after the starter.

### 7.7 Motivation and rewards

Reward meaningful behavior:
- Meaningful task start.
- Completed focus session.
- Task completion.
- Habit log.
- Review completion.
- Project milestone.

Rules:
- Starting and finishing are separate kinds of progress.
- No XP for opening the app or pressing a button only.
- Idempotent rewards; retries cannot duplicate points.
- No negative points, streak decay, or loss after inactivity.
- Rewards are optional and can be disabled.
- Level curve is documented and versioned.
- Achievements are private and based on transparent events.
- Recovery Mode does not remove progress or modify records.

### 7.8 Recovery Mode

User-controlled mode for low-energy or overloaded days:
- Keep one chosen priority visible.
- Show only user-selected essential habits.
- Reduce visual emphasis of XP and charts.
- Offer defer/park actions for other work.
- Keep Quick Capture and Journal available.
- Do not infer health state, diagnose fatigue, or automatically activate.

## 8. Personal memory and life log

### 8.1 Unified journal

Journal is one chronological system with entry types:
- Daily journal
- Quick note / thought
- Idea
- Event
- Learning log
- Decision
- Project update
- Reflection
- Milestone
- Other

Entry body is free-form. Title, type, tags, date, project/goal links, and privacy options are optional metadata. Quick capture defaults to a general note.

### 8.2 Personal timeline

A chronological view can combine:
- Journal entries.
- Task completion.
- Focus sessions.
- Habit logs.
- Project milestones.
- Goal changes.
- Events.
- Measurements.

This timeline is generated from canonical records and activity events. It should not copy full journal bodies into a separate log.

### 8.3 Learning and knowledge capture

A learning log can store:
- Topic.
- Source/reference.
- Notes.
- Key concepts.
- Questions to investigate.
- Related pages/projects.
- Optional practice session.

It must remain usable as a normal page/database record; it is not a separate walled garden.

### 8.4 Decision log

A decision record contains:
- Decision.
- Context.
- Options considered (optional).
- Reasoning.
- Expected consequences.
- Review date (optional).
- Outcome / status.
- Linked project and related journal pages.

## 9. Flexible tracking and analytics

### 9.1 Custom trackers

Users can define trackers without code changes.

Initial value types:
- Number.
- Duration.
- Boolean.
- Rating with configurable scale.
- Short text.

Tracker configuration:
- Name and description.
- Value type and unit.
- Min/max and precision where relevant.
- Optional target.
- Preferred cadence.
- Optional tags and area/project links.

Support multiple records per day and historical editing. Do not turn a missing measurement into zero.

### 9.2 Analytics

Deterministic calculations only:
- Habit completion.
- Task status and completion.
- Focus time.
- Tracker values and trends.
- Journal volume/timeline density.
- Milestone history.
- Project activity.
- Time ranges and comparisons.

Every chart/metric defines its period, numerator, denominator, missing-data behavior, and timezone. No overall score should claim to measure a person's worth or entire life quality.

## 10. Quick Capture and daily workflows

### 10.1 Universal Quick Capture

Available from every major screen, with a keyboard shortcut where appropriate.

Capture options:
- Note
- Idea
- Task
- Event
- Measurement
- Habit log
- Journal entry

Fast path:
1. Open capture.
2. Type/paste content or value.
3. Save.

Classification and metadata are optional and can be added later. Voice capture may be added as an input path later, but transcription is not built-in AI.

### 10.2 Daily loop

1. Open Today to see priorities and relevant context.
2. Choose a Main Mission.
3. Define a tiny next action and start.
4. Capture thoughts and events through the day.
5. Log habits/trackers only when useful.
6. Close the day with an optional reflection.
7. Use weekly/monthly reviews to adjust.

Do not force users to complete every stage.

## 11. Deterministic automation

Mission OS may include rule-based automation without AI.

Examples:
- A task is completed, so a linked project progress count updates.
- A due date is approaching, so a reminder appears.
- A weekly review record is created from a template.
- A saved quick capture is categorized only when the user chooses a type.
- A recurring habit schedule creates the expected log slots for display, not fake completion records.

Rules:
- User can inspect, enable/disable, and delete automations.
- No hidden modifications to content.
- Avoid automation loops.
- Log important automated actions.
- Notifications are opt-in and frequency-limited.
- Do not promise background notifications will always run; browser and device lifecycle restrictions apply.

## 12. External ChatGPT integration

ChatGPT is external. It is not a native Mission OS module or runtime dependency.

### Read capabilities (first)
- Get Today dashboard data.
- Search workspace pages and journal.
- Read a page.
- Read database records and properties.
- Get tasks/projects/goals.
- Get habits and logs.
- Get tracker definitions and metric entries.
- Get reflections and activity history.
- Retrieve bounded summaries as raw data, without AI interpretation generated by Mission OS.

### Write capabilities (later, separately scoped)
- Create a page/note.
- Create a task.
- Log a habit.
- Create a metric entry.
- Save an approved reflection.
- Update an explicitly selected record.

Security:
- OAuth or equivalent authorization.
- Separate read and write scopes.
- Read-only by default.
- User-scoped queries.
- Explicit consent for writes.
- Destructive and bulk writes excluded initially.
- Idempotency keys and audit log.
- Respect page/entry connector privacy flags and workspace access rules.
- No raw SQL, arbitrary filesystem, or unrestricted database tool.
- Treat retrieved user content as untrusted data; page text must never grant tool permissions or override connector policies.
- Revoke connector access independently.
- Use bounded results and pagination.

The app must remain useful without ChatGPT. The API contract should remain provider-neutral so the data is not locked to a single AI product.

## 13. Privacy, security, and ownership

- Private workspace by default.
- HTTPS in production.
- Authentication required for private content.
- Row-level security on every user-owned database table.
- Grants and RLS policies both configured and tested.
- Service-role/admin secrets only server-side.
- File storage access scoped to owner.
- No public page sharing in the initial release.
- Connector privacy flag on pages and journal entries.
- Clear export, backup, restore, and deletion controls.
- Minimal telemetry; no sale or advertising use of personal content.
- Audit trail for external writes and privileged operations.
- Database backups plus user-controlled exports; backups are not a substitute for export.
- Retention and permanent deletion rules clearly documented.

## 14. Import, export, and portability

### Import
Initial import targets:
- Markdown files and folders.
- HTML exports.
- CSV databases.
- JSON backup format.
- Notion export archive as a best-effort import path.

Import must provide:
- Preview before committing.
- File/record count.
- Mapping summary.
- Unsupported block/property report.
- Duplicate detection.
- Rollback or delete-imported-batch option.
- Preservation of original files where possible.

Do not promise perfect round-trip import of proprietary export formats. Relation and rollup reconstruction must be validated from available export data.

### Export
- Full-fidelity JSON with schema version, stable IDs, relationships, property definitions, pages, blocks, and timestamps.
- Markdown/HTML export for readable pages.
- CSV export for database rows.
- Original file downloads for attachments.
- Export by workspace, area, database, project, or date range.
- Tested restore procedure from a full JSON archive.

## 15. Technical architecture

### Recommended baseline

| Layer | Provisional choice |
|---|---|
| Frontend | React + TypeScript + Vite |
| UI styling | CSS variables/design tokens, with accessible component primitives |
| Block editor | Prototype BlockNote first; compare with Tiptap before locking |
| Data validation | Zod |
| Server data state | TanStack Query |
| Primary database | PostgreSQL |
| Hosted backend candidate | Supabase |
| Authentication | Supabase Auth or equivalent |
| File storage | Private object storage |
| Local persistence | IndexedDB |
| PWA | Manifest + service worker |
| Search | PostgreSQL full-text initially |
| API | Versioned REST/JSON or secure RPC |
| External AI | Separate MCP server |
| Testing | Unit, integration, RLS, sync, and browser end-to-end tests |

### Architecture layers

1. PWA presentation and editor.
2. Domain/application service layer.
3. Local IndexedDB drafts and offline operation queue.
4. Authenticated API/backend.
5. PostgreSQL canonical records and object storage.
6. Search/indexing and deterministic analytics.
7. Separate external MCP adapter.

The browser must never receive database passwords or service-role keys.

### Editor selection

BlockNote is the leading prototype candidate because it is a React-focused block editor designed for a Notion-like editing experience and supports custom block types. Its Markdown import is explicitly lossy, so canonical structured JSON is required.

Tiptap is the alternative if the prototype proves that its headless schema/extension model gives better control over custom content, mobile editing, or Yjs integration. Its current Markdown extension is documented as beta. Do not lock the editor based on screenshots alone.

### Offline and synchronization

Initial offline requirements:
- App shell available after first successful load/install.
- Quick Capture drafts saved locally.
- Journal drafts saved locally.
- Queue safe creates/updates while offline.
- Show pending/synced/failed status.
- Retry on reconnect or next app foreground.
- Idempotent operation IDs prevent duplicates.
- Never cache private API responses in a shared service-worker cache.

Multi-device conflict strategy:
- Start with record version numbers and explicit conflict resolution for edits.
- Prototype Yjs + IndexedDB for page documents if reliable offline editing across multiple devices is a high-priority requirement.
- Do not introduce CRDT complexity for every table or structured record by default.
- No claim of conflict-free synchronization until tested with two devices and interrupted connections.

### PWA limitations
A service worker can be stopped by the browser and is not a continuously running background process. Background sync and notifications must be treated as best-effort. The UI should sync while open/foregrounded and clearly expose pending work.

## 16. Non-functional requirements

### Usability
- Core flows comfortable at 320–430 px phone widths.
- Touch targets generally 44–48 px where practical.
- Quick Capture in seconds.
- Main Mission visible without deep navigation.
- Short onboarding; useful empty states.
- Configurable sections and dashboard.
- Avoid modal stacks and unnecessary forms.

### Performance
- Paginate pages, search and database rows.
- Virtualize long lists where needed.
- Lazy-load heavy editor features and attachments.
- Do not load entire workspace at startup.
- Cache safe static shell assets.
- Measure bundle size and slow-device performance.

### Accessibility
- WCAG 2.2 AA as baseline.
- Keyboard and touch support.
- Visible focus, semantic headings, labels and landmarks.
- Contrast validation.
- Reduced motion.
- Do not rely on color alone.
- Provide non-drag alternatives for reordering.

### Reliability
- Autosave and draft recovery.
- No silent write failures.
- Idempotent sync.
- Schema migrations.
- Backup and restore tests.
- Explicit conflict handling.
- Graceful handling of expired authentication.

## 17. Scope and roadmap

The target product is all-in-one. The development strategy is staged so the app becomes genuinely usable before the full system exists.

### Phase 0: Technical validation
- Inspect repository and deployment workflow.
- Prototype BlockNote versus Tiptap on Android.
- Test editor JSON round-trip and Markdown export/import.
- Test Supabase/PostgreSQL authentication, grants and RLS.
- Test IndexedDB draft persistence.
- Decide document save/version/conflict model.
- Record architecture decisions.

Exit: chosen stack proven in a small vertical slice; no broad feature implementation yet.

### Phase 1: Workspace foundation
- PWA shell and authentication.
- Workspace, sidebar/navigation, page tree.
- Page CRUD, editor, autosave, drafts.
- Favorites, recent pages, archive/trash.
- Basic tags and page links.
- Initial global page search.
- Export pages to JSON/Markdown.

Exit: owner can create, organize, edit, find, and export real pages every day.

### Phase 2: Capture and personal memory
- Universal Quick Capture.
- Unified journal timeline.
- Entry types, tags, backlinks, decisions, ideas, events, learning logs.
- Attachments.
- Search and filters.
- Daily journal page/template.

Exit: personal notes and life history can be stored and retrieved reliably.

### Phase 3: Productivity engine
- Tasks and priorities.
- Goals and projects.
- Main Mission and Today's Briefing.
- Habits and routines.
- Focus sessions and starter flow.
- XP/rewards and achievements.
- Calendar basics.

Exit: daily planning and execution work from shared records.

### Phase 4: Custom database engine
- Database definitions and property types.
- Database rows as pages.
- Table, list and board views.
- Filters, sorts, grouping.
- Relations and backlinks.
- Templates.
- Calendar/gallery views.
- Embedded linked database views.

Exit: owner can create and adapt a useful database without code changes.

### Phase 5: Tracking, reflection, and analytics
- Custom trackers.
- Daily/weekly/monthly/quarterly reviews.
- Deterministic analytics.
- Activity timeline.
- Rollups for common calculations.
- Full JSON/CSV/Markdown export and restore.

Exit: system provides a useful longitudinal record of work, interests, habits and personal progress.

### Phase 6: External ChatGPT access
- Stable API.
- Read-only MCP server.
- OAuth and scopes.
- Connector privacy filtering.
- Audit events.
- Carefully scoped write tools.

Exit: ChatGPT can access only authorized data and perform approved writes safely.

### Phase 7: Refinement and advanced workspace
- Advanced database views/formulas.
- Advanced editor blocks.
- Import tooling.
- Additional offline conflict support.
- Optional external calendar integrations.
- Performance/accessibility polish.
- Personalization and module customization.

## 18. MVP definition

The first meaningful alpha is not the whole all-in-one product. It is the smallest real system that can become the owner's daily workspace.

Alpha 0.1 must include:
- Installable PWA shell.
- Private authentication and user-scoped data.
- Workspace and page tree.
- Block editor with core text/list/checklist/heading/quote/code/image blocks.
- Autosave and draft recovery.
- Quick Capture.
- Unified journal/timeline.
- Tags and internal page links.
- Page search.
- Basic JSON/Markdown export.
- Offline capture with visible sync state.
- Empty/loading/error states and phone usability tests.

Custom database views, advanced formulas, MCP writes, and broad integrations are explicitly not part of Alpha 0.1.

## 19. Alpha acceptance tests

1. Create a page, refresh, and confirm content persists.
2. Create nested pages and navigate by breadcrumb.
3. Add, reorder, and delete supported block types.
4. Paste text and links without breaking the document.
5. Close/reopen the app and recover a saved draft.
6. Capture a journal note in a few interactions.
7. Capture offline, reconnect, and confirm exactly one saved record.
8. Search for a phrase in a page and open the correct page.
9. Link two pages and navigate via link/backlink.
10. Export a page and restore its structured JSON content.
11. Verify one user cannot access another user's page or file.
12. Verify private connector flags are honored by API tests.
13. Test small phone widths and keyboard navigation.
14. Test long content and error/retry behavior.

## 20. Risks and mitigations

| Risk | Mitigation |
|---|---|
| Scope becomes too large | Stage the product and enforce Alpha 0.1 boundaries |
| Editor choice becomes expensive to change | Prototype editor and define a versioned canonical document format |
| Markdown conversion loses content | Preserve canonical JSON; test import/export |
| Offline edits overwrite one another | Versioning and explicit conflict resolution; evaluate Yjs for documents |
| Custom database engine grows complex | Start with typed property subset and a few views |
| Generic data model loses validation | Use typed domain tables for tasks, habits, events, metrics |
| Privacy leaks through connectors/search | User-scoped queries, RLS, connector privacy tests, audit |
| App becomes noisy | Customizable dashboard, progressive disclosure, minimal default widgets |
| Too many rewards create gaming | Reward meaningful events, idempotency, optional XP |
| Vendor lock-in | Open exports, documented schema, stable IDs, separate MCP adapter |

## 21. Definition of success

Mission OS succeeds when the owner:
- Naturally uses it to capture and find information.
- Can manage real projects and daily work without maintaining duplicate systems.
- Can create new databases and trackers as life changes.
- Can review their own history without reconstructing it from memory.
- Trusts that records are private, portable, and not silently lost.
- Can use the app without AI, while external ChatGPT integration adds useful analysis when permitted.

The measure is lasting usefulness, not maximum screen time or maximum number of features.
