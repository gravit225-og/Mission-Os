# Mission OS Workspace Data Model

**Version:** 1.0  
**Status:** Logical model for Master Specification v2  
**Date:** 2026-11-16

This document extends the original productivity data model with the page/workspace/database engine. It is a logical specification, not production SQL. Constraints, indexes, migrations, and RLS policies must be implemented and tested before live data is stored.

## 1. Shared identity model

Use a shared entity identity for records that can be linked, searched, opened, or shown in activity history.

### workspaces
- id UUID PK
- owner_user_id UUID FK to auth user
- name
- description nullable
- created_at, updated_at
- archived_at nullable

Initial release creates one private workspace for the owner. Do not implement collaboration or invites in the first release, but keep workspace ownership explicit so the schema can evolve.

### workspace_entities
- id UUID PK
- workspace_id UUID FK
- user_id UUID FK
- entity_type (page, database, task, goal, project, habit, routine, event, metric, reflection, decision, file, template, area)
- display_title
- created_at, updated_at
- archived_at nullable
- deleted_at nullable

Every domain record gets one stable entity ID. Specialized tables hold validated fields; the shared identity table supports global links/search/activity.

## 2. Pages and documents

### pages
- entity_id UUID PK/FK to workspace_entities
- workspace_id UUID
- parent_page_id UUID nullable FK to pages
- page_type (regular, database, database_item, area_home, template, system)
- icon nullable
- cover_asset_id UUID nullable
- position_key text / sortable ordering value
- is_favorite boolean default false
- is_pinned boolean default false
- is_archived boolean default false
- created_at, updated_at

### page_documents
- page_id UUID PK/FK
- document_format (blocknote_json or other versioned editor format)
- schema_version integer
- content_json JSONB
- plain_text_cache text
- revision integer
- updated_at
- updated_by UUID

Canonical page content is structured document JSON with schema version. Do not use rendered HTML as the canonical source of truth.

### page_revisions
- id UUID PK
- page_id UUID FK
- revision integer
- content_json JSONB or immutable object-storage snapshot
- title_snapshot nullable
- created_at
- created_by
- change_reason nullable

Revision retention policy and snapshot frequency are implementation decisions; avoid storing a full snapshot on every keystroke.

### page_links
- id UUID PK
- workspace_id UUID
- source_page_id UUID FK
- target_entity_id UUID FK
- link_type (inline_link, mention, embed, relation)
- source_block_id nullable
- created_at
- unique source/target/type/block relationship where applicable

Use this table to power backlinks and related-page navigation. The document remains the source for the visible inline link; this table is an index.

## 3. Block editor content

Blocks are stored in the page's canonical structured JSON document. Each block contains:
- Stable block ID.
- Type.
- Type-specific attributes.
- Inline content or child blocks.
- Optional links to workspace entities or assets.

BlockNote is the first prototype candidate, not a locked dependency. If selected, store its native JSON with an explicit schema_version. If the editor changes later, write a versioned migration/adapter instead of silently rewriting old content.

Required block categories: text, heading, list, checklist, toggle, quote, callout, divider, code, image/file, link, simple table, page mention, embedded database view.

Avoid a separate SQL row per paragraph/block in the first version unless search, granular permissions, or sync tests prove it necessary. Extract searchable text and page links into indexes/tables.

## 4. Databases

### workspace_databases
- entity_id UUID PK/FK to workspace_entities
- database_page_id UUID FK to pages
- description nullable
- default_view_id UUID nullable
- created_at, updated_at

A database is represented as a workspace page. It can be opened full-page or embedded in another page.

### database_properties
- id UUID PK
- database_id UUID FK to workspace_databases
- name
- property_type (title, text, number, select, multi_select, status, date, checkbox, url, email, relation, file, created_time, updated_time; future formula, rollup)
- config JSONB for validation and display settings
- position integer
- is_required boolean default false
- created_at, updated_at

### database_select_options
- id UUID PK
- property_id UUID FK
- label
- color_token nullable
- position integer
- archived_at nullable

### database_items
- entity_id UUID PK/FK to workspace_entities
- database_id UUID FK
- page_id UUID FK to pages
- position_key text
- created_at, updated_at

Every database item is a workspace entity and has a page body. It may be displayed as a row/card in many views without duplication.

### database_property_values
- id UUID PK
- item_entity_id UUID FK to database_items
- property_id UUID FK to database_properties
- value_text nullable
- value_number nullable
- value_boolean nullable
- value_date nullable
- value_json JSONB nullable (for supported structured types only)
- updated_at
- unique item/property

API/database validation must ensure the value column matches the property's declared type. Select/multi-select values use option IDs, not fragile label strings. Relation values use a dedicated join table.

### database_relation_values
- property_id UUID FK to database_properties
- source_item_entity_id UUID FK to database_items
- target_entity_id UUID FK to workspace_entities
- position integer
- created_at
- composite/unique constraint to prevent duplicate relation rows

Validate that source and target are in an authorized workspace and that the property is a relation.

### database_views
- id UUID PK
- database_id UUID FK
- name
- view_type (table, list, board, calendar, gallery; future timeline, chart, form)
- config JSONB containing filters, sorts, groupings, visible properties, card layout, open behavior
- position integer
- created_at, updated_at

Filters and sorts must be validated against property definitions. Do not accept raw SQL or executable expressions from view configuration.

## 5. Templates

### templates
- entity_id UUID PK/FK to workspace_entities
- template_type (page, database_item, project, event, review, habit, routine)
- target_database_id UUID nullable
- document_content_json JSONB
- default_properties JSONB
- created_at, updated_at

Templates may have associated relation defaults, but relation IDs must be validated at creation time. Repeating/scheduled templates are later scope.

## 6. Areas and collections

### areas
- entity_id UUID PK/FK to workspace_entities
- description nullable
- icon nullable
- color_token nullable
- created_at, updated_at

### entity_area_links
- entity_id UUID FK to workspace_entities
- area_id UUID FK to areas
- created_at
- composite primary key

Areas are optional organizational categories; records can be linked to multiple areas.

### collections
- entity_id UUID PK/FK to workspace_entities
- description nullable
- collection_type (manual, saved_filter)
- config JSONB nullable
- created_at, updated_at

Collections can group links to pages/entities. A saved-filter collection stores a validated query definition, not arbitrary SQL.

## 7. Files and attachments

### file_assets
- entity_id UUID PK/FK to workspace_entities
- storage_key
- original_filename
- mime_type
- size_bytes
- checksum
- alt_text nullable
- created_at
- deleted_at nullable

Rules:
- Private storage bucket by default.
- Signed access URLs with short lifetime.
- Validate file size and allowed content types.
- Do not trust client-supplied MIME type alone.
- File references in page blocks use asset entity IDs.
- Export includes original attachments or a manifest with downloadable files.

## 8. Relations across all entities

### entity_relations
- id UUID PK
- workspace_id UUID
- source_entity_id UUID FK
- target_entity_id UUID FK
- relation_type (related_to, part_of, documents, caused_by, depends_on, follows_up, references, parent_of)
- created_at
- created_by
- unique workspace/source/target/relation_type

Keep directed versus reciprocal relation semantics explicit. Backlinks are derived from incoming relations. A graph visualization is optional; related-record lists are the core UX.

## 9. Search index

### entity_search
May be a materialized view or maintained index containing:
- entity_id
- workspace_id
- entity_type
- title
- text_content
- tags
- updated_at
- source_page_id nullable

Use PostgreSQL full-text search initially. Search indexing must respect user scope. Private connector restrictions apply at the API layer even when owner search includes the content.

## 10. Calendar and activity

### calendar_events
- entity_id UUID PK/FK to workspace_entities
- title
- starts_at, ends_at timestamptz
- timezone
- all_day boolean
- location nullable
- notes_page_id nullable FK
- recurrence_rule nullable (later)
- created_at, updated_at

Tasks/milestones may appear in calendar views through their dates; do not copy them into calendar_events unless the user creates a real event.

### activity_events
- id UUID PK
- workspace_id UUID
- user_id UUID
- entity_id UUID nullable FK
- event_type
- occurred_at
- payload JSONB containing safe metadata only
- idempotency_key nullable
- created_at

Do not duplicate journal bodies inside activity_events.

## 11. Original productivity records

The existing logical model remains valid for specialized tables such as:
- tasks
- goals
- projects
- milestones
- habits
- habit_logs
- focus_sessions
- metrics
- metric_entries
- reflections
- decisions
- reward_events

Each specialized record references the shared workspace_entities identity. Use real typed fields for values that need constraints, sorting, aggregation, or reliable analytics.

## 12. Security and data integrity

- Every owned table has workspace_id and/or user_id as appropriate.
- RLS enabled on every exposed user-owned table.
- Grants and policies are both explicitly configured.
- Service-role credentials never reach browser.
- Cross-table relation validation ensures both sides belong to the same authorized workspace.
- Private connector flags are enforced in every retrieval route, including backlinks and summaries.
- Views/materialized views must be reviewed for RLS behavior before exposure.
- File storage policies enforce the same ownership rules.
- Migrations and tests live in Git.

## 13. Indexes and constraints

Initial indexes:
- workspace_entities(workspace_id, entity_type, updated_at)
- pages(parent_page_id, position_key)
- page_links(target_entity_id)
- database_items(database_id, position_key)
- database_property_values(property_id, value_number) where useful
- database_property_values(property_id, value_date) where useful
- entity_relations(source_entity_id, relation_type)
- entity_relations(target_entity_id, relation_type)
- calendar_events(starts_at, ends_at)
- activity_events(workspace_id, occurred_at DESC)
- full-text index over entity_search title/content

Add indexes based on query plan measurements, not speculation.

## 14. Implementation order

1. Workspace and entity identity.
2. Pages and structured page documents.
3. Page tree, links, backlinks, and search.
4. File assets and templates.
5. Database definitions, properties, items, and views.
6. Relation values and generic entity relations.
7. Calendar events and activity history.
8. Specialized productivity records and analytics.
9. Import/export and connector-safe read models.
