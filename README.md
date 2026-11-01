# Mission OS

A personal productivity and habit-tracking app built around sustainable execution, not endless task completion.

## Product vision

Mission OS helps turn meaningful goals into repeatable daily actions. It combines habit tracking, task prioritization, focus sessions, energy awareness, learning systems, and regular reviews in one calm, game-inspired workspace.

The app should help users answer four questions:
1. What matters most right now?
2. What is the next small action?
3. How is my energy and focus changing?
4. Am I making meaningful progress over time?

## Product principles

- **Impact over activity:** prioritize a small number of meaningful outcomes (Pareto principle and MITs).
- **Sustainable consistency:** support routines without shame, punishment, or streak anxiety.
- **Energy-aware planning:** let users identify their own high-energy windows rather than assuming everyone has the same schedule.
- **Deep work by design:** make focused sessions, single-tasking, and distraction reduction easy.
- **Small actions:** break large goals into clear next steps and make starting friction low.
- **Learning that sticks:** support active recall, spaced repetition, deliberate practice, and reflection.
- **Measure what matters:** track outcomes, consistency, focus, and energy without turning life into a scoreboard.
- **Privacy and user control:** keep personal tracking data transparent and under the user's control.

## Core app areas

1. **Today / Mission Control**
   - Three most important tasks
   - Today's habits
   - Focus session launcher
   - Energy check-in
   - Quick capture for ideas and tasks

2. **Habits**
   - Flexible schedules and frequencies
   - Simple completion tracking
   - Notes and optional reminders
   - Streaks as optional context, never as punishment

3. **Goals and Projects**
   - Long-term goals, milestones, projects, and next actions
   - Impact-versus-effort prioritization
   - Progress reviews

4. **Focus**
   - Pomodoro and custom focus sessions
   - Single-task session notes
   - Distraction parking lot
   - Session history

5. **Energy and Rhythm**
   - Lightweight energy ratings at configurable times
   - Sleep and rest notes (optional)
   - Personal pattern visualizations after enough data is collected

6. **Learning**
   - Learning goals and practice plans
   - Active-recall prompts
   - Spaced-review scheduling

7. **Reviews and Insights**
   - Daily shutdown
   - Weekly review
   - Monthly and quarterly goal reviews
   - Trends that explain progress, not just count activity

## Suggested build sequence

### Phase 0: Foundation
- Choose the stack and visual direction.
- Establish app shell, navigation, and responsive layout.
- Define data models and persistence strategy.

### Phase 1: Useful MVP
- Today dashboard
- Habit creation and completion
- Three daily priorities
- Local persistence
- Basic daily and weekly review

### Phase 2: Focus and energy
- Focus timer and session history
- Energy check-ins
- Personal productivity pattern charts

### Phase 3: Goals and learning
- Goals, projects, milestones, and next actions
- Learning plans and spaced review

### Phase 4: Insights and refinement
- Weekly/monthly insights
- Optional reminders and export/backup
- Accessibility, polish, and testing

## MVP success criteria

A user can:
- Create, edit, pause, and complete a habit.
- Set up to three daily priorities.
- See today's progress at a glance.
- Record an energy check-in.
- Complete a short daily review.
- Close and reopen the app without losing locally saved data.

## Product guardrails

- Do not reward sleep deprivation, overwork, or excessive exercise.
- Avoid guilt-based streak mechanics and punitive notifications.
- Keep the default experience lightweight; advanced frameworks should be optional.
- Never present productivity scores as a measure of personal worth.
- Make data collection minimal and explain what is stored.

## Open decisions

- Frontend stack and deployment target
- Visual direction and name/branding details
- Local-first versus account-backed sync
- Whether to ship as a responsive web app/PWA first

---

*Mission OS is a working product concept. Features and implementation choices should be validated incrementally, starting with the smallest useful daily workflow.*
