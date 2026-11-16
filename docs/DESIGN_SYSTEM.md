# Mission OS Design System

**Version:** 1.0  
**Status:** Proposed visual system for implementation  
**Design direction:** Midnight Workshop  
**Primary target:** Android phone PWA, adaptive desktop

## 1. Design identity

Mission OS should feel like a calm personal command center: focused enough to help a person begin, warm enough to invite reflection, and quiet enough to hold years of personal information.

The visual language borrows the clarity and usefulness of the supplied OFFSEASON dashboard reference, not its brand, exact layout, wording, or identity.

### Design keywords
- Calm, not sterile.
- Focused, not militaristic.
- Personal, not social.
- Data-rich, not dashboard-noisy.
- Rewarding, not manipulative.
- Editorial warmth with precise utility.

### Brand expression
- Wordmark: MISSION OS, uppercase with generous tracking.
- Optional mark: simple abstract compass/orbit or intersecting path symbol. Avoid a mascot or trophy as the primary logo.
- Primary accent: warm rose / ember.
- Supporting colors: mineral teal for positive state, warm amber for milestones, muted blue-violet for secondary chart series.
- Default theme: dark graphite. A light theme can be added after the first useful release.
- Avoid pure black as the only background and avoid bright accent fills on every card.

## 2. Color tokens

Color values below are initial design tokens. Verify rendered contrast in actual components and test on real Android screens before release.

### Dark theme

| Token | Hex | Use |
|---|---|---|
| --color-bg | #101113 | Main application background |
| --color-sidebar | #0B0D0F | Desktop navigation |
| --color-surface-1 | #17191C | Default cards and inputs |
| --color-surface-2 | #1E2125 | Raised/interactive surfaces |
| --color-surface-3 | #262A2F | Hover and selected secondary surface |
| --color-text-primary | #F2F0F1 | Main readable text |
| --color-text-secondary | #B9B4B7 | Supporting text |
| --color-text-muted | #A8A3A6 | Metadata and helper copy |
| --color-border | #343238 | Dividers and card borders |
| --color-border-strong | #504A50 | Focused/active outlines |
| --color-accent | #D97889 | Accent icon, active state, progress |
| --color-accent-strong | #9C4F61 | Primary filled button |
| --color-accent-container | #352027 | Selected accent surfaces |
| --color-on-accent | #FFFFFF | Text on primary filled button |
| --color-success | #73C4B0 | Completed/synced/positive state |
| --color-warning | #E6B86A | Due/attention state |
| --color-danger | #E28B86 | Destructive/error state |
| --color-info | #91ACDE | Neutral informational state |

Use semantic roles, not raw colors scattered through components. Components should use roles such as surface, text-primary, border, and success rather than choosing hex values ad hoc.

### Chart series
Use a consistent series order: rose, teal, amber, blue, violet. Always include a label, legend, tooltip, marker, or line pattern so chart meaning is not conveyed by color alone.

### Contrast guardrails
- Normal text target: at least 4.5:1 against its rendered background.
- Large text target: at least 3:1.
- Important component boundaries and focus indicators should remain clearly perceivable.
- Do not use muted text tokens for essential instructions unless the actual pairing passes contrast testing.
- Verify accent buttons with their paired on-accent text rather than assuming the brand color is accessible.

## 3. Typography

### Font roles
- UI and body: Inter, ui-sans-serif, system-ui, -apple-system, BlinkMacSystemFont, Segoe UI, sans-serif.
- Display headings: Georgia or a system serif as restrained editorial contrast for page greeting and Main Mission title only.
- Numbers: use tabular numerals for XP, durations, counts, dates, and chart labels.

Avoid loading remote font files as a hard dependency. System fallbacks must render well offline.

### Type scale

| Token | Size / line height | Weight | Use |
|---|---|---|---|
| Display | 36 / 44 px | 600 | Dashboard greeting on wide screens |
| H1 | 30 / 38 px | 650 | Page title |
| H2 | 22 / 30 px | 600 | Main section/card title |
| H3 | 18 / 26 px | 600 | Card subheading |
| Body large | 16 / 24 px | 400 | Main reading text |
| Body | 14 / 21 px | 400 | Default interface text |
| Label | 13 / 18 px | 550 | Buttons, form labels |
| Caption | 12 / 17 px | 450 | Metadata; not critical instructions |
| Eyebrow | 11 / 16 px | 650 | Short uppercase section labels |

Use uppercase, letter-spaced eyebrow labels sparingly. Do not render long sentences in all caps. Respect browser zoom and user text scaling.

## 4. Spacing and layout tokens

Base spacing unit: 4 px.

| Token | Value |
|---|---:|
| space-1 | 4 px |
| space-2 | 8 px |
| space-3 | 12 px |
| space-4 | 16 px |
| space-5 | 20 px |
| space-6 | 24 px |
| space-8 | 32 px |
| space-10 | 40 px |
| space-12 | 48 px |
| space-16 | 64 px |

### Radius
- Small control: 8 px.
- Input and compact card: 10 px.
- Standard card: 14 px.
- Feature card/dialog: 18 px.
- Pill: 999 px.

### Responsive layout
- Phone: 320–767 px. Single column, 16 px page gutters, bottom navigation.
- Tablet: 768–1199 px. Compact rail or top navigation; 24 px gutters.
- Desktop: 1200 px and up. 240 px sidebar, content max width 1080 px, 32–48 px page gutters.
- Wide desktop: keep content capped; do not stretch journal text or forms across the entire screen.
- Main reading column: aim for 680–760 px maximum width.
- Dashboard can use a wider two-column content grid.

## 5. Navigation system

### Desktop sidebar
Grouped navigation:
- **Today:** Today, Missions, Habits, Focus.
- **Personal:** Goals & Projects, Trackers, Journal.
- **Reflection:** Reviews, Activity Timeline, Achievements.
- **Account:** Profile, Settings.

Sidebar width: 240 px. Keep labels visible, show active item using a soft accent-container background and accent icon. User/account block anchors the bottom. Sidebar can collapse to an icon rail.

Do not add Community or Leaderboard. Planning cycles are a future optional view, not an MVP dependency.

### Tablet
Use a 72 px icon rail when there is room, with accessible tooltips and visible page title. Otherwise use compact top navigation.

### Mobile
Use a fixed bottom navigation with safe-area padding:
1. Today
2. Journal
3. Central Capture action
4. Insights
5. Manage

Manage opens a structured hub for Missions/Tasks, Habits, Focus, Goals & Projects, Trackers, Reviews, Activity Timeline, Achievements, and Settings. Focus should also be reachable from the Main Mission card. Do not force a desktop-style sidebar onto a phone.

## 6. Core components

### App shell
- Responsive sidebar/bottom navigation.
- Main scroll area with page title and optional top actions.
- Global search entry point.
- Quick Capture entry point always reachable.
- Respect device safe areas and browser viewport changes.

### Buttons
- Primary: solid accent-strong background, on-accent text.
- Secondary: surface background with border.
- Tertiary: text/icon only.
- Destructive: danger treatment only for destructive actions.
- Icon-only buttons require accessible labels and comfortable hit area.
- Primary action should be singular within a card; do not make every action equally bright.

### Cards
- Surface-1 fill, subtle 1 px border, 14 px radius.
- Feature card may use a slightly higher surface level.
- Avoid heavy shadows and gradients as the default.
- Card action area should be visually distinct but compact.

### Inputs
- Label above field.
- Visible focus ring.
- Clear error text beside/under the field.
- Save state for autosaving editors.
- Avoid placeholder-only labels.
- Support keyboard types appropriate to values (number, decimal, date).

### Chips and badges
- Use for status, category, filters and tags.
- Neutral by default; semantic color only when state conveys meaning.
- Never use a badge as the only way to communicate a critical state.

### Progress
- XP progress bar is thin and calm.
- Always pair bar with numeric text.
- Charts include text summaries and empty states.

### Briefing item
- One-line or two-line message plus source/action.
- Dismiss control has accessible label.
- Dismissal persists for current local day.
- No animated urgency or red warning unless there is a real error.

### Main Mission card
- Mission title and optional project/category.
- One sentence for the next action.
- Estimate and reward preview as secondary metadata.
- Start Focus and Complete as separate actions with clear semantics.
- User can change the selected mission.

### Habit row
- Habit name and optional unit/target.
- One easy completion/log control.
- Optional progress state.
- No shaming copy or punitive streak flame.

### Quick Capture
- Opens a bottom sheet on mobile and centered dialog or side panel on desktop.
- Defaults to free-form note.
- Provide actions for Note, Task, Idea, Event, Measurement, Habit.
- Save text immediately; optional metadata is progressively disclosed.

### Timeline entry
- Date/time, type icon, title or first line, short preview, tags.
- Full entry opens on tap.
- Do not force a card for every short note; timeline can be a clean list.

### Focus session
- Strong typographic focus on current task and remaining time.
- Low-distraction controls.
- Pause/resume/end with clear state.
- Backgrounding must not corrupt elapsed time.
- After starter session: Stop here, Continue 10 min, or Choose focus length.

## 7. Interaction principles

- One visually dominant action per region.
- Prefer direct actions over nested menus for common daily actions.
- Advanced metadata lives behind an optional Details section.
- Empty states teach one next step.
- Use confirmation for destructive deletion, not every harmless tap.
- Undo can be offered for reversible logging/completion.
- No confetti on routine interactions; reserve small milestone animation for significant achievements and respect reduced motion.
- Never make XP collection require extra app navigation.

## 8. Accessibility and input

- Target WCAG 2.2 AA as the design baseline.
- Meet WCAG 2.2 AA target-size rules; design common mobile controls at approximately 44–48 px where practical.
- Provide visible keyboard focus and logical focus order.
- Do not convey completion/error solely through color.
- Support reduced motion and browser zoom.
- Label icon-only actions.
- Use semantic headings and landmarks.
- Ensure text remains readable on dark surfaces.
- Drag/reorder actions must have a button-based alternative.

## 9. States and feedback

Every data-driven screen defines:
- Initial loading/skeleton.
- Empty state.
- Normal state.
- Offline/pending sync state.
- Sync failure/retry state.
- Validation error.
- Permission/authentication error.
- Destructive confirmation.
- Search with no results.
- Successful save feedback.

Use small, non-blocking toasts for routine saves. Keep unsynced records visible with a clear status indicator.

## 10. Motion

- Most transitions: 120–180 ms.
- Sheet/dialog: 180–240 ms.
- Avoid parallax and continuous looping motion.
- Honor prefers-reduced-motion.
- XP progress can animate once after a confirmed award, but must not flash or demand attention.

## 11. Token implementation starter

~~~css
:root {
  color-scheme: dark;
  --color-bg: #101113;
  --color-sidebar: #0B0D0F;
  --color-surface-1: #17191C;
  --color-surface-2: #1E2125;
  --color-surface-3: #262A2F;
  --color-text-primary: #F2F0F1;
  --color-text-secondary: #B9B4B7;
  --color-text-muted: #A8A3A6;
  --color-border: #343238;
  --color-border-strong: #504A50;
  --color-accent: #D97889;
  --color-accent-strong: #9C4F61;
  --color-accent-container: #352027;
  --color-on-accent: #FFFFFF;
  --color-success: #73C4B0;
  --color-warning: #E6B86A;
  --color-danger: #E28B86;
  --color-info: #91ACDE;

  --space-1: 4px;
  --space-2: 8px;
  --space-3: 12px;
  --space-4: 16px;
  --space-5: 20px;
  --space-6: 24px;
  --space-8: 32px;
  --space-10: 40px;
  --space-12: 48px;
  --space-16: 64px;

  --radius-sm: 8px;
  --radius-control: 10px;
  --radius-card: 14px;
  --radius-feature: 18px;
  --radius-pill: 999px;

  --sidebar-width: 240px;
  --content-max: 1080px;
  --reading-max: 740px;
}
~~~

## 12. Design validation checklist

Before implementation is visually accepted:
- Check 320 px, 360 px, 390 px, 768 px, 1024 px, and 1440 px widths.
- Check primary, secondary, disabled, hover, active, focused, loading, and error states.
- Verify text contrast with actual rendered colors.
- Test large text and browser zoom.
- Test touch target comfort on a physical Android phone.
- Test dashboard with no tasks, no habits, no XP, and no milestones.
- Test long titles, long journal entries, and large numbers.
- Test keyboard-only navigation.
- Test reduced-motion mode.
- Verify no dashboard section requires fake placeholder content.

## 13. Design references

The system uses role-based color and typography tokens, following the general theming approach documented by Android's Material Design 3 resources. It uses WCAG 2.2 AA as the accessibility baseline.

- [W3C WCAG 2.2](https://www.w3.org/TR/WCAG22/)
- [Android Developers: Material Design 3](https://developer.android.com/develop/ui/compose/designsystems/material3)
- [Android Developers: Create an accessible and personalized theme with Material Design 3](https://developer.android.com/codelabs/m3-design-theming)
