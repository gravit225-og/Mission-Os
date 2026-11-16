# Mission OS Implementation Blueprint

Status: Approved product direction; phased build plan
Version: 1.0
Updated: 2026-11-16

## 1. Recommended technical baseline

These are recommended defaults, subject to a small compatibility spike before the first implementation commit.

| Layer | Recommendation | Reason |
|---|---|---|
| Frontend | React + TypeScript + Vite | Familiar ecosystem, fast PWA build |
| UI | Tailwind CSS + accessible primitives | Responsive mobile-first interface |
| Validation | Zod | Shared client/API validation |
| Server state | TanStack Query | Caching, retries, mutation state |
| Local/offline store | IndexedDB via Dexie | Durable drafts and queued writes |
| Backend/database | Supabase PostgreSQL | Relational data and hosted backend |
| Auth | Supabase Auth | User identity and sessions |
| Security | PostgreSQL RLS + least-privilege grants | Enforce user-level isolation |
| PWA | Manifest + service worker | Installability and app-shell caching |
| API | Versioned REST/JSON or secure RPC | Stable external integration contract |
| MCP | Separate TypeScript MCP server | ChatGPT integration without app-embedded AI |

Do not add a complex state-management framework unless actual complexity requires it.

## 2. Target repository structure

- README.md
- docs/PRODUCT_REQUIREMENTS.md
- docs/DATA_MODEL.md
- docs/IMPLEMENTATION_PLAN.md
- docs/API_MCP_SPEC.md
- src/app, components, features, lib, services, styles, types
- public/icons and manifest
- supabase/migrations and supabase/tests
- mcp-server as a separate deployable integration
- tests and package manifest

This is a target structure, not a requirement to create empty folders before code exists.

## 3. Phases and exit criteria

### Phase 0 — Repository and architecture validation
Tasks:
- Confirm repository contents and development workflow.
- Verify React/Vite PWA build on narrow Android viewport.
- Verify Supabase project setup and migrations.
- Prototype authenticated read/write with RLS.
- Test offline draft persistence in IndexedDB.
- Record decisions in architecture decision records.

Exit:
- Blank installable PWA builds and deploys.
- Authenticated user can create/read own test record.
- No service-role key in client code.
- Offline draft survives refresh.

### Phase 1 — Foundation and shell
Tasks:
- Create React/TypeScript/Vite app.
- Add mobile navigation and design tokens.
- Configure manifest, icons, service worker, install flow.
- Configure Supabase client using public client configuration only.
- Implement sign-in/out and protected routes.
- Add migrations, RLS policies, RLS tests.
- Add loading, error, and empty states.

Exit:
- App installs on Android browser.
- Authenticated routes work.
- RLS tests prove user A cannot read or mutate user B records.
- App shell loads after first successful install, subject to browser storage availability.

### Phase 2 — Capture and Journal (first daily-use milestone)
Tasks:
- Global Quick Capture.
- Journal create/edit/archive/delete.
- Autosaved drafts.
- Unified chronological timeline.
- Type/tag/date/project filters.
- Full-text search.
- IndexedDB offline create queue and sync status.
- Journal export.

Exit:
- Short note captured in seconds.
- Offline note syncs exactly once after reconnect.
- User can find/edit older entry.
- No note lost during normal navigation/reload tests.
- Owner begins using this milestone daily.

### Phase 3 — Habits and daily execution
Tasks:
- Habit CRUD and schedules.
- Habit logging and backfill.
- Task inbox, statuses, priorities, daily top three.
- Starter timer and focus sessions.
- Reward event ledger with idempotency.
- Dashboard sections and visibility settings.
- Level/XP panel derived from reward events.
- Today's Briefing with deterministic, dismissible items.
- Main Mission card with next action, Start Focus and Complete actions.
- Habits Due Today, Quick Actions, and Recent Milestones sections.
- User-controlled Recovery Mode.

Exit:
- Habit logs use correct local date.
- Missed days do not remove points/progress.
- Start XP only follows meaningful start.
- Retried sync cannot award duplicate XP.
- Stopping starter session causes no penalty.

### Phase 4 — Goals, projects, flexible trackers
Tasks:
- Goal/project/milestone CRUD.
- Link tasks and journal entries to projects.
- Tracker definition builder.
- Typed metric validation.
- Numeric/duration/rating charts and text history.
- Project timeline.

Exit:
- User creates tracker without code changes.
- Invalid value types rejected.
- Historical entries stay attached to tracker.
- Project history searchable.

### Phase 5 — Reflection and deterministic analytics
Tasks:
- Daily, weekly, monthly, quarterly review templates.
- Free-form and partial reflections.
- Habit/task/focus/metric aggregations.
- Date-range filters.
- Explain chart definitions and missing-data handling.
- JSON and CSV export.

Exit:
- Reviews optional and editable.
- Analytics match tested fixture calculations.
- Missing data is not represented as zero.
- Full export includes relationships and schema version.

### Phase 6 — API and ChatGPT MCP integration
Tasks:
- Finalize versioned API contract.
- Implement read-only MCP tools first.
- Add connector privacy filter and user scope.
- Implement authentication/authorization appropriate to selected MCP HTTP transport/spec.
- Add write tools only after read tools are secure.
- Require explicit confirmation for destructive/bulk changes.
- Add audit history.
- Test with limited non-sensitive dataset first.

Exit:
- MCP cannot access another user's data.
- Connector-private entries never returned.
- Tools paginated and size-limited.
- Writes idempotent and audited.
- Revoking authorization blocks access.

### Phase 7 — Reliability and polish
Tasks:
- Test slow network, offline, reconnect, duplicate sync, expired session.
- Test mobile viewport and keyboard behavior.
- Test export/restore.
- Accessibility review.
- Privacy and deletion review.
- Optional reminders only after user testing.
- Minimal telemetry and performance review.

Exit:
- Critical flows pass on target Android browsers.
- No known critical security or data-loss defects.
- Restore from export tested.
- PWA update process does not strand users on incompatible cached assets.

## 4. Offline and sync design

PWA should prioritize reliable capture, not make every feature offline in v1.

Offline scope:
- App shell and previously cached static assets.
- Create/edit journal drafts and quick notes.
- Queue selected simple writes (journal note, habit log, metric entry).
- Show pending, synced, conflict, and failed states.

Sync rules:
- Assign UUIDs client-side to offline-created records.
- Assign unique client_operation_id to each queued operation.
- Server applies operations idempotently.
- Use updated_at plus version/conflict handling for edits.
- Do not silently overwrite concurrent edits.
- Initial single-owner conflict UI shows local and server versions.
- Never cache private API responses in a shared service-worker cache.
- Cache only static app-shell assets and explicitly safe public resources.

## 5. Quality strategy

Test layers:
- Unit: date calculations, schedule rules, XP calculations, tracker validation.
- Component: forms, capture, habit controls, timer states.
- Integration: database CRUD, RLS, sync queue, export.
- End-to-end: capture note, create habit, set priority, start focus, log metric, complete reflection.
- Security: cross-user access, missing auth, connector-private entries, malformed input.
- PWA: install, reload, offline app shell, offline note queue, reconnect.

Critical edge cases:
- User changes timezone.
- Daylight-saving transition for users outside India.
- App backgrounded during focus timer.
- Double save.
- Network drops during write.
- Session expires with queued write.
- Same note edited on two devices.
- Tracker type changed after entries exist.
- Habit paused or schedule changed mid-week.
- Entity deleted while linked records exist.

## 6. Delivery discipline

- Implement one vertical slice at a time.
- Every phase ends with a working feature and acceptance test.
- Keep database migrations in Git.
- No manual production schema changes without migration.
- Use preview/development environment before production.
- Avoid dependencies without clear need.
- Keep secrets in environment configuration, never committed.
- Update docs when product decision changes.
- Do not start MCP work before auth, RLS, export, and data privacy are stable.

## 7. First implementation ticket

Ticket: Establish PWA foundation and prove secure persistence.

Definition of done:
1. React + TypeScript + Vite app runs.
2. Mobile-first layout renders.
3. PWA manifest and app-shell service worker configured.
4. Supabase connection uses public client configuration only.
5. Initial profile/test table migration exists.
6. RLS enabled and policies tested.
7. Authenticated create/read works for own test record.
8. Cross-user access denied in tests.
9. Local IndexedDB draft survives refresh.
10. Setup instructions and environment variable names documented.

Do not implement all product modules in this ticket.


## 8. Dashboard implementation notes

Build the Today dashboard from existing domain records rather than introducing parallel copies:
- Main Mission references an existing task/daily priority.
- Habits Due Today queries active habits against the user's local date and schedule.
- Recent Milestones queries existing milestone records.
- Level and achievements derive from reward/activity events.
- Briefing items are deterministic rules and can be dismissed for the current local day.
- Recovery Mode changes presentation and suggested scope only; it does not mutate task, habit, or XP history.

Desktop may use a grouped sidebar similar in information hierarchy to the reference: Main (Today, Missions/Tasks, Habits, Focus), Personal (Goals/Projects, Trackers, Journal), Reflection (Reviews, Timeline, Achievements), Account (Profile, Settings). On mobile, use a compact bottom navigation and a prominent Quick Capture action. Do not reproduce the reference's brand, exact visual styling, AI assistant, or social features.
