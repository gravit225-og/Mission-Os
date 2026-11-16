# Mission OS Product Requirements Document

Status: Product direction approved; implementation-ready draft
Version: 1.0
Updated: 2026-11-16

## 1. Product definition

Mission OS is a private, mobile-first personal life and productivity data system. It helps its owner capture life, organize work, track meaningful activities, preserve personal history, and review progress over time.

Mission OS is the system of record. It stores and presents data, performs deterministic calculations, and runs user-configured rules. It does not contain a built-in AI chatbot, AI coach, AI-generated insights engine, or AI reflection generator. ChatGPT is an optional external intelligence layer connected later through a permission-controlled API/MCP interface.

Product promise:
- Capture information quickly without organizing it immediately.
- Make starting meaningful work easier and positively rewarding.
- Keep a reliable, searchable history of thoughts, events, decisions, and progress.
- Make tracking flexible enough to cover new interests without code changes.
- Make personal data portable and controlled by its owner.
- Work usefully even when ChatGPT is unavailable.

## 2. Locked product decisions

| Decision | Choice |
|---|---|
| Platform | Mobile-first installable PWA |
| Journal | One unified chronological timeline with categories, tags, filters, and links |
| Tracking | Flexible user-defined trackers and fields |
| Privacy | Private account with secure cloud storage |
| Planning | Complete specification before implementation |
| AI | No AI features inside Mission OS; ChatGPT is external |
| Initial audience | Single-owner personal use, with user-scoped architecture |

## 3. Product principles

1. Capture first, organize later.
2. Easy starts beat elaborate plans.
3. Reward real initiation and progress, not button tapping.
4. Progress without punishment: no lost XP, shame, or broken-streak penalties.
5. Data before interpretation: show recorded facts and transparent calculations.
6. Flexible, not complicated: advanced fields and views are optional.
7. One source of truth: avoid duplicate copies of the same fact.
8. Private by default: no public profiles, social feed, or sharing by default.
9. Portable by design: complete export and tested backup/restore.
10. AI-independent: core workflows work without an AI service or connector.

## 4. Primary user jobs

- Record something important in under 20 seconds.
- Write a long journal entry without being forced through a form.
- See today's priorities and begin one with minimal friction.
- Log habits with minimal navigation.
- Create a custom tracker for a new area of life.
- Understand what happened on a particular day or during a project.
- Review a week or month using recorded entries and measurements.
- Export personal information in usable formats.
- Later, let ChatGPT retrieve selected records and perform approved write actions.

## 5. Information architecture

Primary navigation:
1. Today: daily dashboard and start-work flow.
2. Journal: unified timeline, search, filters, entry details.
3. Capture: global quick-capture action.
4. Insights: deterministic charts, summaries, review history.
5. Manage: habits, tasks, goals, projects, trackers, settings.

Use compact mobile bottom navigation. Capture should be prominent but not obstructive.

### 5.1 Today / Mission Control

- Show selected local date and concise greeting.
- Show up to three user-selected daily priorities.
- Show today's habit checklist and completion state.
- Show a clear next action for active tasks.
- Provide global Quick Capture.
- Provide optional energy/mood check-ins, never required.
- Show lightweight progress based on transparent counts.
- Start flow: choose task, choose or write a tiny first action, start a short starter session, then stop, continue, or focus.
- Allow hiding dashboard sections.
- Empty states explain how to begin and never show fake data.

### 5.2 Unified Journal and Life Timeline

Entry types: daily journal, quick note/thought, idea, event, learning log, decision, project update, reflection, milestone, other.

Requirements:
- Free-form body is the primary input.
- Title, type, tags, linked project/task/goal, mood/energy are optional metadata.
- Automatic created timestamp and editable occurred-at timestamp.
- Draft/autosave for longer writing.
- Timeline grouped by date.
- Search title and body.
- Filter by type, tag, date range, and linked project.
- Pin/favorite entries.
- Edit, archive, permanently delete.
- Link entries to records without copying content.
- Private-from-connector flag for entries excluded from ChatGPT.
- Quick capture does not require choosing a type; default to general note.

### 5.3 Habits

- Create, edit, pause, archive, delete.
- Schedules: daily, selected weekdays, days per week, or custom cadence.
- Track completion, skipped/not applicable, and optional quantity.
- Optional note per log.
- Backfill a previous date.
- Show calendar/history and simple completion rates.
- Streaks are optional context only; missed days never remove earned points.
- Interpret schedules in the configured timezone.

### 5.4 Tasks and Focus

Task fields: title, optional description, status, priority, due date, estimated duration, linked goal/project, tags, next action.

Statuses: inbox, next, in progress, waiting, done, cancelled.

- Support daily top-three selection without limiting full task list.
- Quick task creation from anywhere.
- Complete, reschedule, defer, or archive without guilt language.
- Focus sessions can optionally link to a task.
- Starter session default: 2 minutes, configurable.
- Continue options may include 10-minute momentum and a configurable longer focus block.
- Store planned duration, actual duration, start/end, outcome, optional note.
- Calculate elapsed time from timestamps when app returns from background.
- User can stop early and still receive initiation credit after the meaningful-start condition.

### 5.5 Goals, Projects, Milestones

- Goal has outcome, why it matters, optional target date, status, progress method.
- Project can belong to a goal and contain tasks, milestones, notes, linked journal entries.
- Milestones record meaningful checkpoints.
- Show project activity history.
- Allow projects to be paused or archived.
- Tasks need not belong to a goal.

### 5.6 Flexible Trackers

Users can create a tracker without developer intervention.

Initial value types:
- Number (integer/decimal)
- Duration
- Boolean
- Rating (configurable scale)
- Short text

Tracker configuration: name, description, unit, value type, optional min/max, decimal precision, preferred cadence, optional tags, optional target/threshold.

- Log value with occurred-at timestamp and optional note.
- Support multiple entries per day.
- Edit and delete entries.
- Show list history and suitable charts for compatible types.
- Do not force fixed tracker categories.
- Health-related tracking is optional and does not provide diagnosis or medical recommendations.

### 5.7 Reflections and Reviews

Templates are optional prompts, not mandatory questionnaires.

- Daily closeout: meaningful action, learning, challenge, next step.
- Weekly review: wins, friction, project progress, what to change.
- Monthly review: progress, neglected areas, decisions, next-month focus.
- Quarterly review: goal relevance, direction, major changes.
- User can write freely, skip prompts, save partial review.
- Review is a journal-linked record with period start/end.
- Review history is searchable/exportable.
- App may calculate factual counts but does not generate AI interpretations.

### 5.8 Analytics

Initial deterministic analytics:
- Habit completion by period.
- Tasks completed by period and project.
- Focus duration by day/week and task.
- Metric history and trend charts.
- Journal entry counts and timeline density.
- Goal/project milestone history.
- Daily/weekly activity overview.

Every statistic defines numerator, denominator, date range, and treatment of skipped/missing values. Avoid a single overall life score. Charts distinguish no data from zero.

### 5.9 Settings and Data Ownership

- Profile display name (optional), timezone, date format, week start, theme, dashboard preferences.
- Export all data in JSON; CSV for tabular records.
- Documented and tested backup/restore.
- Account/data deletion controls.
- Connector permissions and audit history when integration is added.
- Notifications opt-in and configurable.

## 6. Quick Capture requirements

Global capture options:
- Note/journal
- Idea
- Task
- Event
- Measurement
- Habit log
- Reflection

Fastest path is a text box with Save. Type, tags, project links, and metadata can be added later.

Acceptance targets:
- Short note saved in no more than two interactions after opening capture.
- Capture works offline and syncs later.
- Draft content survives accidental navigation or reload.
- Show pending, synced, and failed sync states.

Voice transcription is not part of MVP. Future voice capture may store audio or accept an external transcript; no built-in AI transcription is planned.

## 7. Start and reward system

Reward model reduces starting friction and reinforces meaningful effort, not app engagement.

Event types:
- meaningful_start
- focus_progress
- task_completed
- habit_logged
- review_completed
- milestone_reached

Rules:
- Start reward is awarded once per meaningful task/session start, not per button press.
- User may stop after starter session without losing start reward.
- Completion rewards are separate from initiation rewards.
- Rapid repeated start/stop events cannot farm unlimited points.
- No negative XP, decay, or loss for rest days.
- Points are feedback, not personal-worth scores.
- Values are configurable and tuned after real use.
- Each event uses an idempotency key to prevent duplicate rewards after retries.

Illustrative starting values, not final: meaningful start 10 XP, completed focus block 20 XP, task completion 25 XP, saved reflection 10 XP, milestone 40 XP.

## 8. Non-functional requirements

Usability:
- Mobile-first narrow Android screens.
- Suitable touch targets and typography.
- Short capture/logging flows.
- Accessible labels, keyboard navigation, contrast, reduced motion.

Reliability:
- No silent data loss.
- Autosave drafts.
- Offline capture queue.
- Visible sync status and retry.
- Idempotent sync.
- Version-controlled database migrations.

Performance:
- Fast first useful render on mobile connection.
- Paginated timeline/search.
- Do not load all journal entries into memory.
- Query aggregated date ranges for charts.

Security/privacy:
- HTTPS in production.
- RLS on every user-owned table.
- Never expose service-role credentials to browser.
- No public sharing by default.
- Connector excludes private entries.
- Minimal telemetry.
- Validate writes and rate-limit API/MCP endpoints.

Portability:
- Export stable IDs, timestamps, timezone, relationships, tracker definitions.
- Include schema version.
- CSV for common tables, full-fidelity JSON for backup.

## 9. Explicit non-goals

- No AI chatbot, AI agent, AI-generated reflection, or automatic AI interpretation inside app.
- No social feed, public profile, leaderboard.
- No invasive background monitoring or screen-time surveillance.
- No medical diagnosis/advice.
- No complex calendar scheduler in first release.
- No native Android app until PWA usage validates need.
- No multi-user collaboration in first release.

## 10. MVP acceptance criteria

Owner can:
1. Sign in securely and access only own records.
2. Install PWA on Android.
3. Create a quick note in seconds.
4. Create, edit, search, filter, delete journal entries.
5. View chronological journal timeline.
6. Create/log a habit, including a past date.
7. Create tasks and choose up to three daily priorities.
8. Start a short focus session linked to a task.
9. Create a custom numeric or text tracker and record values.
10. Save a daily reflection.
11. View basic habit, task, metric history.
12. Reopen app without losing synced data.
13. Capture offline and sync after reconnect.
14. Export data in JSON.

## 11. Success measures

Measure usefulness, not compulsive engagement:
- Time/actions required to capture a note.
- Successful capture rate.
- Sync failure and duplicate rates.
- Completion of key phone workflows.
- User-reported usefulness/friction.
- Voluntary return because the app is useful.

Do not optimize for screen time, notification opens, streak length, or raw event count.

## 12. Technical choices to verify

The product decisions are locked. Verify framework, database hosting region/provider, auth method, offline sync library, hosting, and MCP SDK/OAuth implementation during repository setup.


## 13. Today dashboard design refinement

The Today page is the daily launchpad. It should make the next useful action obvious while keeping the full system one tap away.

Recommended desktop information order:
1. Date and personalized greeting.
2. Level and lifetime XP progress panel.
3. Today's Briefing: a short, dismissible list of useful prompts.
4. Main Mission: the selected top priority, with category, estimate, next action, reward preview, Start Focus and Complete controls.
5. Habits Due Today: only habits scheduled for the current local date, with inline logging.
6. Quick Actions: Quick Capture, Start Focus, Add Task, Log Habit, Add Measurement, New Journal Entry.
7. Recent Milestones: recent goal/project milestones and achievements.

On mobile, reorder and collapse sections to preserve a short path to Quick Capture and Main Mission. Users can hide or rearrange nonessential dashboard sections.

### Today's Briefing rules
- Briefing items are derived from actual app records and deterministic rules.
- Examples: selected priority is not started; a scheduled habit is awaiting a log; a review is due; a project milestone is approaching.
- Each item can be dismissed for the day.
- No AI-generated advice or vague motivational notifications.
- Do not repeatedly surface the same dismissed item during that local day.

### Main Mission rules
- User selects the Main Mission from their tasks or daily priorities.
- If none is selected, show a helpful empty state to choose one or create a task.
- Display its next action, estimated duration, project/category context, and available reward preview.
- Start Focus opens the starter-session flow.
- Complete requires the normal task completion action; it must not be confused with merely starting.
- User may change the Main Mission at any time.

### Level, achievements, and milestones
- Level is calculated from lifetime XP using a documented progression curve.
- XP is never lost through inactivity, rest, or missed habits.
- Achievements are earned from transparent events and can be reviewed in a private achievements/history page.
- Recent Milestones surfaces existing project/goal milestones; it must not create duplicate milestone records.

### Recovery Mode
- User-controlled mode for low-energy or overloaded days.
- When enabled, dashboard prioritizes one minimum viable task, essential scheduled habits selected by the user, and a gentle option to defer other tasks.
- It may hide nonessential dashboard modules and reduce visual urgency.
- It does not change historical records, erase goals, reduce earned XP, or automatically mark tasks complete.
- No AI detection of fatigue and no medical or psychological claims.

### Explicitly rejected reference features
- No built-in AI assistant (ChatGPT remains external).
- No leaderboard, public profile, or community competition.
- Planning cycles/seasons are a possible later enhancement, not an MVP dependency.


## 14. Visual design system and screen layouts

The visual specification is maintained separately:
- [Design System](DESIGN_SYSTEM.md): colors, typography, spacing, navigation, components, interaction, accessibility, and tokens.
- [Screen Layouts](SCREEN_LAYOUTS.md): desktop/mobile layouts and behaviors for Today, Capture, Journal, Tasks, Focus, Habits, Goals/Projects, Trackers, Reviews, Insights, Settings, and Recovery Mode.

The Today dashboard adopts a level/XP panel, dismissible deterministic briefing, Main Mission card, Habits Due Today, Quick Actions, and Recent Milestones. It does not adopt an AI assistant, leaderboard, or community features.
