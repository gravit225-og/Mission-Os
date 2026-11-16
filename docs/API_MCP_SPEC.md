# Mission OS API and MCP Integration Specification

Status: Planned integration contract
Version: 1.0
Updated: 2026-11-16

## 1. Purpose

Mission OS is the source of truth for personal data. ChatGPT is an external intelligence client. Integration lets ChatGPT retrieve user-authorized records and later create/update selected records with permission.

There is no AI runtime inside Mission OS.

## 2. Architecture

- PWA uses authenticated app/backend access.
- PostgreSQL remains canonical storage.
- Separate HTTPS MCP server exposes narrow tools.
- MCP server authenticates owner and enforces user scope.
- MCP server calls approved application services; it does not expose raw SQL or unrestricted database access.
- ChatGPT receives only records required for a request.
- Connector authorization can be revoked independently of Mission OS account.

## 3. Requirements

- Read-only tools are implemented/security-tested before write tools.
- Every query scoped to authenticated user.
- Every result respects private_from_connector.
- List/search tools support pagination and bounded date ranges.
- Outputs use stable IDs and ISO 8601 timestamps.
- Responses distinguish missing data from empty results.
- Writes validate input, enforce ownership, and use idempotency.
- Destructive operations excluded from first MCP release.
- Bulk writes require explicit confirmation and are deferred.
- External writes recorded in audit log.
- Never include credentials/secrets or records outside requested scope.

## 4. Initial read tools

### get_today
Input: optional local date; defaults to current date in configured timezone.
Output: daily priorities, habit statuses, tasks due/selected, user-entered check-ins, recent non-private journal metadata. Do not return full bodies in dashboard summary.

### search_journal
Input: query; optional from/to dates, entry types, tag IDs, limit (default 20, max 50), cursor.
Output: matching non-private entries with ID, title, type, occurred_at, short excerpt, tags, pagination cursor. Broad search never returns full bodies.

### get_journal_entry
Input: entry UUID.
Output: full entry only if owned by authenticated user and not private from connector.

### get_activity_history
Input: from/to, optional entity type, limit, cursor.
Output: bounded activity events with event type, entity reference, timestamp, minimal payload.

### get_habits
Input: optional from/to/habit ID.
Output: habit definitions and logs in bounded range.

### get_tasks_and_projects
Input: optional status, project ID, date range, pagination.
Output: tasks, project metadata, milestones. Avoid unrelated journal bodies.

### get_metrics
Input: optional metric IDs, from/to, limit, cursor.
Output: tracker definitions and values, including units and timestamps.

### get_reflections
Input: period type, from/to, limit, cursor.
Output: user-written reflection content and period metadata, excluding connector-private entries.

### get_weekly_summary_data
Input: bounded date range.
Output: structured raw counts and linked record references. It returns data, not interpretations or recommendations. ChatGPT performs analysis externally.

## 5. Later write tools

### create_journal_entry
Required: body. Optional: title, entry type, occurred_at, tags, related entity IDs, private_from_connector. Returns created ID and saved timestamp.

### create_task
Required: title. Optional: description, priority, due date, project ID, next action. Returns created task.

### log_habit
Required: habit ID, local date, status. Optional: quantity and note. Idempotency key required.

### create_metric_entry
Required: metric ID, occurred_at, one correctly typed value. Optional note. Validate against tracker definition.

### save_reflection
Required: period type, period start/end, user-approved answers/body. Clearly indicate content is being saved. Do not save AI interpretation as user-authored reflection without approval; record provenance when AI-assisted.

## 6. Provenance and audit

External writes record actor, tool/action, target entity ID, timestamp, request/idempotency ID, and result status.

Where relevant, content carries provenance metadata such as created_via=chatgpt_connector. This identifies origin, not correctness.

## 7. Privacy controls

- private_from_connector entries excluded from every retrieval path, including search excerpts, summaries, related-record traversal, and activity feeds that might leak content.
- No bypass through another tool.
- User can revoke connector access.
- Token scopes separate read and write permissions.
- Default scope read-only.
- No account-wide export through MCP in first release.
- No raw SQL, arbitrary file paths, or arbitrary URL fetch tools.

## 8. Security and protocol notes

- Use current stable MCP specification and maintained TypeScript SDK at implementation time; pin versions and record compatibility.
- For remote HTTP MCP, implement authorization and protected-resource discovery required by selected transport/spec.
- Use short-lived credentials, least-privilege scopes, secure token storage, revocation.
- Validate origin/redirect behavior; do not accept arbitrary callback URLs.
- Apply rate limits, request-size limits, pagination caps, and timeouts.
- Never ship Supabase service-role key or database password to browser or ChatGPT.
- Treat tool arguments and retrieved journal text as untrusted user data, not executable instructions.
- Require explicit confirmation before writes where supported; otherwise use narrow tools and user-configured scopes.
- No delete/archive-all tool in first release.

## 9. Rollout

1. Document and test application API internally.
2. Create read-only MCP server against test dataset.
3. Verify ownership isolation and private-entry exclusion.
4. Connect to ChatGPT where account plan and client support custom MCP connectors.
5. Test realistic questions against seeded data and verify record references.
6. Add narrow write tools with separate scopes.
7. Add audit view and revocation.
8. Expand only when read/write behavior is reliable.

## 10. Example: weekly review

User asks ChatGPT to review last week.

ChatGPT retrieves weekly summary data, habit logs, relevant tasks/projects, reflection, and journal matches for the date range. ChatGPT analyzes records, distinguishes facts from possible explanations, and proposes changes. Nothing is written back unless the user approves a specific save action.
