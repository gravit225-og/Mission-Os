# Mission OS Screen Layouts

**Version:** 1.0  
**Status:** Proposed screen specification  
**Design system:** [DESIGN_SYSTEM.md](DESIGN_SYSTEM.md)

These layouts describe hierarchy and behavior, not pixel-perfect implementation. Mobile is the primary design target; desktop uses the same information hierarchy with more room.

## 1. Global navigation and shell

### Desktop shell

~~~text
┌──────────────────────┬──────────────────────────────────────────────────────────────┐
│ MISSION OS           │  Page title                         Search   + Capture      │
│                      │                                                              │
│ TODAY                │  Main content area                                          │
│  Today               │                                                              │
│  Missions             │                                                              │
│  Habits               │                                                              │
│  Focus                │                                                              │
│                      │                                                              │
│ PERSONAL             │                                                              │
│  Goals & Projects     │                                                              │
│  Trackers             │                                                              │
│  Journal              │                                                              │
│                      │                                                              │
│ REFLECTION           │                                                              │
│  Reviews              │                                                              │
│  Activity Timeline    │                                                              │
│  Achievements         │                                                              │
│                      │                                                              │
│ ACCOUNT              │                                                              │
│  Profile              │                                                              │
│  Settings             │                                                              │
│                      │                                                              │
│ Account / Sign out    │                                                              │
└──────────────────────┴──────────────────────────────────────────────────────────────┘
~~~

### Mobile shell

~~~text
┌──────────────────────────────┐
│ MISSION OS            Search │
│                              │
│          Page content        │
│                              │
│                              │
│                              │
│                              │
├──────────────────────────────┤
│ Today  Journal  +  Insights  Manage │
└──────────────────────────────┘
~~~

The central + opens Quick Capture. Manage is a hub, not an overloaded drawer.

## 2. Today / Mission Control

Purpose: show what matters today and make starting easy.

### Desktop layout

~~~text
┌─────────────────────────────────────────────────────────────────────────────┐
│ MONDAY, 16 NOVEMBER                                  [Recovery Mode] [Profile]│
│ Good morning, [Name].                                                        │
│ Your day, your pace.                                                         │
├─────────────────────────────────────────────────────────────────────────────┤
│ LEVEL 3      160 XP                                  TO LEVEL 4: 240 XP      │
│ [====================-------------------------------]                       │
├─────────────────────────────────────────────────────────────────────────────┤
│ TODAY'S BRIEFING                                                             │
│ [Your main mission is ready to start.                            Dismiss ×] │
│ [One scheduled habit is still waiting for a log.                  Dismiss ×] │
├────────────────────────────────────────────┬────────────────────────────────┤
│ MAIN MISSION                               │ HABITS DUE TODAY                │
│                                            │                                │
│ Build the next Celestscape feature         │ [ ] Reading / study             │
│ Product / Celestscape                      │ [ ] Stretch / movement          │
│ Next step: sketch the empty state          │ [ ] Custom habit                │
│ 25 min · reward preview                    │                                │
│ [Start 2-minute step] [Complete]           │ [View all habits]               │
├────────────────────────────────────────────┴────────────────────────────────┤
│ QUICK ACTIONS                                                                │
│ [+ Quick Capture] [Start Focus] [Add Task] [Log Habit] [Add Measurement]     │
├────────────────────────────────────────────┬────────────────────────────────┤
│ RECENT MILESTONES                          │ RECENT ACTIVITY                 │
│ • Project milestone completed              │ • Journal note saved            │
│ • Goal checkpoint reached                  │ • Focus session recorded        │
│ [View milestones]                          │ [Open timeline]                 │
└────────────────────────────────────────────┴────────────────────────────────┘
~~~

### Mobile order
1. Greeting/date and profile/recovery action.
2. Compact XP/level panel.
3. Main Mission card.
4. Today's Briefing (collapsed to a short list if needed).
5. Habits Due Today.
6. Quick Actions (horizontal scroll or 2-column grid).
7. Recent Milestones / Activity.

The main mission should appear before lower-priority informational sections on phone.

### Key behavior
- Greeting uses local time and optional display name.
- If there is no Main Mission, show empty state with Create Task and Choose Existing Task.
- XP card displays lifetime XP and progress to next level; it never implies lost progress.
- Briefing is based on app data and deterministic rules, with one-tap dismissal for that day.
- Recovery Mode is a user-selected presentation mode, never an automatically inferred health state.
- Habits Due Today only includes habits scheduled for the current local date.
- Recent Milestones links to the original project/goal record.

## 3. Quick Capture

Purpose: save an idea or event before it disappears from memory.

### Mobile bottom sheet

~~~text
┌──────────────────────────────┐
│             ━                │
│ Quick Capture                │
│ [Note] [Task] [Idea] [Event] │
│                              │
│ What's on your mind?         │
│ ┌──────────────────────────┐ │
│ │                          │ │
│ │ Write freely...          │ │
│ │                          │ │
│ └──────────────────────────┘ │
│                              │
│ [ + Details (optional) ]     │
│                              │
│ [Save note]                  │
└──────────────────────────────┘
~~~

Rules:
- Default type is Note.
- Save requires only content.
- Metadata remains collapsed initially.
- Autosave drafts.
- Show pending/synced/failed status.
- After save, keep user in context and show a small confirmation.

## 4. Journal

Purpose: searchable personal memory and life history.

### Timeline page

~~~text
┌──────────────────────────────────────────────────────────────────────┐
│ Journal                                     [Search] [ + New Entry ]  │
│ Your notes, ideas, experiences and decisions.                        │
│ [All] [Notes] [Ideas] [Events] [Learning] [Reflections]               │
│ [Date range] [Tags] [Project]                                         │
├──────────────────────────────────────────────────────────────────────┤
│ TODAY                                                                  │
│ 10:42  IDEA                                                            │
│        Celestscape telescope mode idea                                  │
│        A short preview of the thought...               [tag] [project] │
│                                                                        │
│ 08:15  DAILY NOTE                                                      │
│        Morning notes                                                   │
│        A short preview of the entry...                                  │
│                                                                        │
│ YESTERDAY                                                              │
│ 18:20  LEARNING                                                        │
│        Notes from today's programming workshop                         │
│                                                                        │
│ [Load more]                                                            │
└──────────────────────────────────────────────────────────────────────┘
~~~

### Entry editor
- Title optional.
- Main writing canvas is the largest element.
- Type, tags, date/time, links and connector privacy live in optional Details.
- Save state appears unobtrusively.
- Provide explicit back navigation and draft recovery.
- Connector privacy flag is clearly explained, not hidden.

## 5. Missions / Tasks

Purpose: turn goals into clear next actions.

Main views: Today, Inbox, Upcoming, Waiting, Completed.

Task list row:
- Checkbox/status control.
- Task title and next action.
- Optional project, due date, estimate and priority.
- Overflow menu for edit, defer, archive.

Task detail:
- Description.
- Next action.
- Linked goal/project.
- Estimate and due date.
- Session history.
- Related journal entries.
- Start Focus and Complete actions.

Main Mission is a selected task reference, not a duplicate task object.

## 6. Focus

Purpose: remove friction between intention and action.

### Active focus screen

~~~text
┌──────────────────────────────┐
│ Focus                  Exit  │
│                              │
│ PROJECT X-RAY                │
│ Read the next section        │
│                              │
│           02:00              │
│         STARTER              │
│                              │
│       [Pause] [End]          │
│                              │
│ Keep distractions parked.   │
└──────────────────────────────┘
~~~

After the starter timer:
- Stop here. The start counts.
- Continue for 10 minutes.
- Choose a longer focus duration.

Do not force a longer session. Session history stores actual time and outcome.

## 7. Habits

Purpose: quick logging with a useful history.

Top controls:
- Today / Week / Calendar.
- Add Habit.
- Optional schedule filter.

Habit card:
- Name and optional description.
- Scheduled days and target, if relevant.
- Large completion/log control.
- Optional quantity input.
- History opens a calendar or list.
- Edit/pause from a secondary menu.

Avoid streak-loss warnings. Missing log is displayed as not recorded, not as failure.

## 8. Goals and Projects

Purpose: connect daily work with longer-term outcomes.

Goals page:
- Active goals grouped first.
- Each goal shows why it matters, optional target date, and linked projects.
- Progress is derived from explicit milestones or user-selected progress method.

Project detail:
- Project title, status, short description.
- Next milestone and next action.
- Milestone list.
- Linked tasks.
- Project journal / activity history.
- Related ideas and decisions.

Do not require all personal tasks to be assigned to a project.

## 9. Trackers

Purpose: define what matters and record it consistently.

Trackers overview:
- Search and filter.
- Add Tracker.
- Cards show name, unit/type, latest value, and compact history preview.

Create tracker flow:
1. Name.
2. Value type: number, duration, boolean, rating, text.
3. Unit/scale and optional range.
4. Preferred cadence (optional).
5. Save.

Log entry:
- Typed field matched to tracker.
- Occurred-at date/time defaulted to now.
- Optional note.
- Save.

Tracker detail:
- Latest entries.
- Chart when values are chartable.
- Date range.
- Edit tracker.
- Export this tracker.

## 10. Reflections and Reviews

Purpose: make looking back easy without forcing journaling.

Reviews home:
- Daily closeout.
- Weekly review.
- Monthly review.
- Quarterly review.
- Previous reviews.

Review editor:
- Period label.
- Optional prompts.
- Large free-form area.
- Save partial / Complete review.
- Related tasks, projects and milestones are linkable.
- Calculated activity counts are factual context, separate from user-written interpretation.

## 11. Insights and Activity Timeline

Insights:
- Date range selector: 7 days, 30 days, 90 days, custom.
- Habit completion chart.
- Focus history.
- Task completion.
- Tracker trends.
- Journal activity.
- Goal/project milestones.

Activity Timeline:
- Chronological events from app records.
- Filter by entity type and date.
- Open source record from each event.
- Avoid duplicating journal content in event stream.

All charts show a no-data state and a textual summary. Do not invent missing values.

## 12. Manage hub and Settings

Manage hub groups:
- Missions / Tasks
- Habits
- Focus
- Goals & Projects
- Trackers
- Reviews
- Activity Timeline
- Achievements

Settings:
- Profile and timezone.
- Appearance and text scaling.
- Dashboard customization.
- Privacy and connector access.
- Export and backup.
- Notifications.
- Account deletion.

## 13. Recovery Mode

Entry point: a quiet button on Today.

When enabled:
- Keep one chosen priority visible.
- Show only user-selected essential habits.
- Offer a simple defer action for other tasks.
- Reduce visual emphasis of XP, charts and milestone widgets.
- Keep capture and journal fully available.
- Preserve all data and XP history.
- No automatic fatigue inference or AI detection.

## 14. Global state and edge-case screens

Design each major view for:
- First-use empty state with one clear next action.
- Loading state that preserves layout.
- Offline state with locally saved/pending indicator.
- Sync failure with retry and no data disappearance.
- Expired session with safe reauthentication.
- Validation error adjacent to the affected field.
- Delete confirmation with clear item identity.
- Search with no results and clear-filter action.
- Long content, long titles and small-phone widths.

## 15. Build order from these layouts

1. App shell, navigation, tokens.
2. Quick Capture sheet/dialog.
3. Journal timeline, editor and entry detail.
4. Today dashboard with honest empty states.
5. Tasks and Main Mission.
6. Habits.
7. Focus session.
8. Goals and Projects.
9. Flexible Trackers.
10. Reviews.
11. Insights, Activity Timeline and Achievements.
12. Settings, export, and connector controls.

The first usable release should prioritize capture and journal reliability, even though Today is the visual home.
