# Mission OS Data Model

Status: Proposed logical schema for implementation
Version: 1.0
Updated: 2026-11-16

## 1. Principles

- PostgreSQL relational model is recommended source of truth.
- Every user-owned row includes user_id.
- UUID primary keys generated securely.
- Store timestamps in UTC (timestamptz); store user's IANA timezone in profile.
- Use occurred_at for when something happened and created_at for when recorded.
- Keep current state and historical events distinct.
- Use JSONB for flexible configuration, not as a replacement for core relational fields.
- Foreign keys and indexes support common relationships and searches.

## 2. Core tables

### profiles
user_id UUID primary key / auth user FK; display_name nullable; timezone default Asia/Kolkata but editable; week_starts_on; date_format; theme; dashboard_preferences JSONB (visible sections, order); recovery_mode_enabled BOOLEAN default false; created_at; updated_at.

### journal_entries
id UUID PK; user_id UUID; entry_type text (daily, note, idea, event, learning, decision, project_update, reflection, milestone, other); title nullable; body required; occurred_at; created_at; updated_at; is_pinned; is_archived; private_from_connector; metadata JSONB; deleted_at nullable.

Indexes: user_id + occurred_at descending; user_id + entry_type + occurred_at descending; full-text index on title/body scoped by user.

### tags
id UUID PK; user_id UUID; name; normalized_name; color nullable; created_at; unique user_id + normalized_name.

### journal_entry_tags
journal_entry_id FK; tag_id FK; composite primary key.

### habits
id UUID PK; user_id UUID; name; description nullable; schedule_type (daily, weekdays, days_per_week, custom); schedule_config JSONB; tracking_type (boolean, quantity); unit nullable; status (active, paused, archived); created_at; updated_at; archived_at nullable.

### habit_logs
id UUID PK; user_id UUID; habit_id FK; local_date DATE; status (completed, skipped, not_applicable); quantity_value nullable; note nullable; created_at; updated_at; unique user_id + habit_id + local_date for one daily log per habit in MVP.

### tasks
id UUID PK; user_id UUID; title; description nullable; status (inbox, next, in_progress, waiting, done, cancelled); priority nullable; due_at nullable; estimated_minutes nullable; completed_at nullable; goal_id nullable FK; project_id nullable FK; next_action nullable; created_at; updated_at; archived_at nullable.

### daily_priorities
id UUID PK; user_id UUID; local_date DATE; task_id FK; position 1-3; created_at; unique user_id + local_date + position; unique user_id + local_date + task_id.

### focus_sessions
id UUID PK; user_id UUID; task_id nullable FK; session_type (starter, momentum, focus, custom); planned_seconds nullable; started_at; ended_at nullable; actual_seconds nullable; outcome (progress, completed, stopped, interrupted); note nullable; created_at; updated_at.

### goals
id UUID PK; user_id UUID; title; description nullable; why_it_matters nullable; status (active, paused, achieved, abandoned); target_date nullable; created_at; updated_at; achieved_at nullable.

### projects
id UUID PK; user_id UUID; goal_id nullable FK; name; description nullable; status (active, paused, completed, archived); started_at nullable; target_date nullable; completed_at nullable; created_at; updated_at.

### milestones
id UUID PK; user_id UUID; project_id nullable FK; goal_id nullable FK; title; description nullable; target_date nullable; completed_at nullable; created_at; updated_at.

### metrics
id UUID PK; user_id UUID; name; description nullable; value_type (number, duration, boolean, rating, text); unit nullable; config JSONB (scale, min/max, precision, preferred cadence, optional target); status (active, archived); created_at; updated_at.

### metric_entries
id UUID PK; user_id UUID; metric_id FK; occurred_at; numeric_value nullable; duration_seconds nullable; boolean_value nullable; text_value nullable; rating_value nullable; note nullable; created_at; updated_at. A constraint/trigger must ensure exactly one value column is populated and matches metric's configured type.

### reflections
id UUID PK; user_id UUID; period_type (daily, weekly, monthly, quarterly, custom); period_start DATE; period_end DATE; journal_entry_id nullable FK; answers JSONB; created_at; updated_at; unique user_id + period_type + period_start + period_end.

### decisions
id UUID PK; user_id UUID; title; context nullable; decision; reasoning nullable; review_date nullable; status (active, superseded, reversed); journal_entry_id nullable FK; created_at; updated_at.

### activity_events
id UUID PK; user_id UUID; entity_type; entity_id; event_type; occurred_at; payload JSONB containing small metadata only; idempotency_key nullable; created_at; unique user_id + idempotency_key when key exists.

### reward_events
id UUID PK; user_id UUID; event_type; source_entity_type; source_entity_id; points integer; idempotency_key required; created_at; unique user_id + idempotency_key.

### entry_links
id UUID PK; user_id UUID; source_type; source_id; target_type; target_id; relationship (related_to, caused_by, documents, part_of, etc.); created_at; unique user_id + source/target/relationship tuple.

### sync_operations (only if required by selected sync design)
id UUID PK; user_id UUID; client_operation_id UUID; entity_type; entity_id; operation (create, update, delete); status (pending, applied, conflict, failed); created_at; processed_at nullable; unique user_id + client_operation_id.

## 3. Relationships

- One user owns many records.
- Journal entry has many tags and links.
- Habit has many habit logs.
- Task can link to project and/or goal.
- Project contains tasks and milestones.
- Goal contains projects and milestones.
- Metric has many metric entries.
- Reflection can reference journal entry.
- Activity events reference entities without copying their content.
- Reward events are derived from meaningful events and idempotent.

## 4. Date/time rules

- Store event timestamps as UTC timestamptz.
- Store user timezone as IANA timezone string, initially Asia/Kolkata but editable.
- Store habit completion local_date explicitly.
- Date-only concepts (due date, review period) use DATE.
- Render local time using profile timezone.
- Missing habit log does not mean habit failed.

## 5. Flexible tracker validation

Use relational columns for common value types. Use JSONB for tracker configuration only. Enforce value-type consistency in database/API. Do not store every measurement as arbitrary JSON because aggregation and constraints become difficult.

## 6. Security

- Enable RLS on every user-owned table.
- Policies scope SELECT, INSERT, UPDATE, DELETE to auth.uid() = user_id.
- Configure grants as well as policies.
- Service-role credentials remain server-side only.
- Validate foreign-key links so records cannot cross users.
- MCP queries apply same user scope and connector privacy filter.
- private_from_connector entries are excluded from connector search, fetch, summaries, and related-record traversal.
- Do not expose raw SQL or unrestricted table access to MCP.

## 7. Indexing

- B-tree indexes on user_id plus common date/order fields.
- Full-text index on journal title/body.
- Habit logs by user_id + local_date and habit_id + local_date.
- Metric entries by user_id + metric_id + occurred_at.
- Paginate timeline/search.
- Consider partitioning only after measuring actual scale.

## 8. Export

JSON export includes schema_version, exported_at, timezone, entity arrays, stable IDs, timestamps, relationship IDs, and tracker definitions.

CSV export includes separate files for journal entries, habits, habit logs, tasks, focus sessions, goals, projects, metrics, metric entries, and reflections.

## 9. Schema implementation order

1. Profiles and user security.
2. Journal entries, tags, entry links.
3. Habits and habit logs.
4. Tasks, daily priorities, focus sessions.
5. Goals, projects, milestones.
6. Metrics and metric entries.
7. Reflections, decisions, activity events, reward events.
8. Export/import and MCP-safe read views.


## 10. Dashboard and rewards data notes

- Level is derived from the sum of reward_events.points using a versioned progression curve; do not store a second mutable XP total unless a verified cache is required.
- Achievements should initially be derived from reward/activity events and milestone records. Add a dedicated achievements table only if users need custom achievement definitions or manually managed unlocks.
- Today's Briefing dismissal can use a small user-scoped briefing_dismissals table keyed by local_date and deterministic briefing_item_key, or a JSON preference only if the list remains very small. Prefer a table for auditability and idempotency.
- Main Mission can reuse daily_priorities with a selected task reference or add a nullable main_mission_task_id to a daily dashboard state table if it needs to differ from the top three. Do not duplicate task content.
- Recovery Mode is a user preference/state, not a diagnosis. Persist enabled state and optionally the date it applies to; it must not rewrite task or habit history.
