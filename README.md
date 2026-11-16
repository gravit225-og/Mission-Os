# Mission OS

**Personal Life & Productivity Data System**

Mission OS is a private, mobile-first personal data system for capturing life, organizing work, tracking meaningful activity, preserving personal history, and reviewing progress.

## Core architecture

- **Mission OS:** the system of record. It captures, stores, organizes, tracks, and displays personal data.
- **ChatGPT:** the optional external intelligence layer for analysis, reasoning, research, and recommendations.
- **API / MCP:** a secure, permission-controlled bridge between ChatGPT and Mission OS.

Mission OS itself contains **no built-in AI chatbot, AI coach, AI-generated reflections, or AI interpretation engine**. Core features must work without ChatGPT.

## Locked product decisions

- Mobile-first installable PWA.
- One unified chronological journal timeline with categories, tags, filters, and links.
- Flexible user-created trackers and measurement types.
- Private account with secure cloud storage.
- Complete product specification before implementation.
- Personal single-owner use initially, with user-scoped data security.

## Product modules

1. **Today / Mission Control:** daily briefing, main mission, level/XP progress, habits due today, quick actions, recent milestones, and easy-start flow.
2. **Journal:** free-form entries, ideas, events, learning, decisions, reflections, and a searchable timeline.
3. **Habits:** flexible schedules and non-punitive tracking.
4. **Tasks & Focus:** next actions, daily top three, starter sessions, focus history.
5. **Goals & Projects:** milestones, linked tasks, and project history.
6. **Flexible Trackers:** user-defined numeric, duration, boolean, rating, and text measurements.
7. **Reflections:** optional daily, weekly, monthly, and quarterly reviews.
8. **Analytics:** transparent deterministic charts and historical summaries.

## Product principles

- Capture first, organize later.
- Make starting meaningful work easy.
- Reward initiation and progress, never button tapping.
- No punishment for missed days or rest.
- Data before interpretation.
- Privacy, portability, and user control.
- No dependence on AI for core app functionality.

## Documentation

- [Product Requirements Document](docs/PRODUCT_REQUIREMENTS.md)
- [Data Model](docs/DATA_MODEL.md)
- [Implementation Blueprint](docs/IMPLEMENTATION_PLAN.md)
- [API & MCP Integration Specification](docs/API_MCP_SPEC.md)
- [Design System](docs/DESIGN_SYSTEM.md)
- [Screen Layouts](docs/SCREEN_LAYOUTS.md)

## Recommended technical direction

React + TypeScript + Vite PWA, PostgreSQL/Supabase, authentication, row-level security, IndexedDB for reliable offline capture, and a separate MCP server for future ChatGPT integration. These are implementation recommendations to validate with a small technical spike before committing to the full stack.

## Development order

1. Validate repository, framework, hosting, database, authentication, and offline strategy.
2. Build the PWA foundation and security model.
3. Build Quick Capture and Journal first. This is the first daily-use milestone.
4. Add habits, tasks, focus sessions, and non-punitive rewards.
5. Add goals, projects, and flexible trackers.
6. Add reflections, deterministic analytics, and complete export/restore.
7. Add read-only API/MCP access, then carefully scoped write tools.
8. Test reliability, privacy, accessibility, and mobile usability.

## First milestone

A working, installable app that lets its owner securely capture, search, edit, and review journal entries, including reliable offline capture and later sync.

See the documentation above for detailed requirements and acceptance criteria.


## Dashboard inspiration review

Mission OS adopts a focused Today dashboard with a visible level/XP panel, dismissible Today's Briefing, one Main Mission card with Start Focus and Complete actions, Habits Due Today, Quick Actions, Recent Milestones, and an optional user-controlled Recovery Mode. It does not adopt the reference app's built-in AI assistant, leaderboard, or community features. Optional planning cycles may be explored later without duplicating goals and projects.


## Visual design direction

The current direction is Midnight Workshop: graphite dark surfaces, warm rose accent, restrained teal/gold semantic colors, clear typography, and responsive mobile-first layouts. The dashboard uses a level/XP panel, dismissible briefing, Main Mission, Habits Due Today, Quick Actions, and Recent Milestones. See the design system and screen layout specifications.
