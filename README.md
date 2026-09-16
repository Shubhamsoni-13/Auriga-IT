# Daymark — Habit Tracker

Daymark is a lightweight, browser-based habit tracker designed around a simple daily flow: see what is due today, tick habits off, and protect your streaks. It includes flexible schedules, current and best streaks, habit search, archiving, per-habit progress history and graphs, and a 75-day challenge progress card.

## Features

### Daily habit tracking

- A **Today** view that shows only active habits scheduled for the selected date.
- One-click completion controls; click a completed habit again to undo that day's log.
- Previous/next date navigation for reviewing or backfilling days, plus a one-click return to today.
- A daily completion count and progress ring showing completed habits out of all habits due that day.
- A supportive daily message that changes when all scheduled habits are complete.
- A morning check-in banner listing the current profile's habits that are still unlogged today.
- Optional browser notifications: select **Enable reminder alerts** in the check-in banner to receive one morning reminder at or after 8:00 AM for the outstanding habits while the app is open.
- Each habit with a reminder time sends one additional alert at its chosen time if that habit is still unchecked. Alerts are tracked independently for every profile, habit, and date.

### Flexible routines

- Create habits with a custom name, icon, and colour.
- Choose any combination of days of the week for each habit, including every day, weekdays, weekends, or an individual schedule.
- Set an optional reminder time for each habit; the time appears in the daily and All Habits lists.
- Clear schedule labels such as **Every day**, **Weekdays**, or the selected day names.
- Edit an existing habit at any time without losing its completion history.

### Streaks and history

- A **current streak** for every habit, based on consecutive completed scheduled days.
- A **best-ever streak** shown alongside each habit.
- Non-scheduled days do not break a streak.
- Completion history is recorded as local calendar dates, avoiding time-zone-related day shifts.

### Search and habit management

- Global habit search from the top bar, including the `Ctrl+K` / `Cmd+K` shortcut.
- An **All habits** view with a dedicated search field.
- Archive habits to remove them from daily tracking without deleting their logs or streak history.
- Restore archived habits whenever they become relevant again.
- Permanently delete a habit and its logs from **All habits**; the app asks for confirmation before removing it.
- Separate **Active** and **Archived** filters.

### Multiple profiles

- Add a separate profile for another person from the profile control at the bottom of the sidebar.
- Switch instantly between profiles from the same menu.
- Every profile has fully separate habits, completion logs, current/best streaks, archived habits, analytics, and 75-day challenge start date.
- A new profile begins with an empty habit list so each person can build their own routine.
- Profiles are stored only in the current browser's local storage; they are private to that browser and are not synced to another device.

### Insights and progress graphs

- Weekly completion percentage across all active, scheduled habits.
- Longest current streak across all active habits.
- All-time completion total.
- A seven-day overall completion bar chart.
- A dedicated 14-day progress card for every active habit, containing:
  - a day-by-day status calendar;
  - green check marks for completed days;
  - soft red dashes for missed scheduled days;
  - gray cells for days that were not scheduled;
  - the habit's current streak and best-ever streak;
  - a 14-day completion percentage; and
  - an individual bar graph showing daily completion history.

### 75-day challenge and usability

- A sidebar card that displays the current day and completion progress of a 75-day challenge.
- Starter data so the app feels immediately usable on the first visit.
- Responsive design for desktop and mobile screens.
- Persistent browser storage: habits and logs survive refreshes on the same browser and device.
- No account, backend, database, or build step required.

## Requirements

There is no build system, package manager, database, or backend. You only need a modern browser (Chrome, Edge, Firefox, or Safari).

The app stores data in the browser's `localStorage`, so data remains on the same browser and device after refreshes. It is intentionally a static prototype and does not sync between devices.

> **Reminder limitation:** the in-app check-in appears whenever Daymark is opened. Browser alerts can be delivered only while this static app is open in a browser tab. Sending a notification while the browser is completely closed requires a production push-notification service and backend.

## Project structure

```text
.
├── README.md
└── outputs/
    ├── index.html    # Page structure and accessible controls
    ├── styles.css    # Responsive visual design
    └── app.js        # Habits, schedules, streak logic, and local storage
```

## Run locally

### Fastest option

Open `outputs/index.html` directly in your browser. Double-click the file in File Explorer, or use your browser's **Open file** command.

### Recommended: use a local web server

Running a local server more closely matches how the app will behave when deployed and makes browser debugging easier.

From the project root in PowerShell, run either option below.

Using Python (available on many Windows installations):

```powershell
py -m http.server 8080 --directory outputs
```

Then visit `http://localhost:8080`.

Using Node.js without adding project files:

```powershell
npx serve outputs
```

The command prints the local URL to open. Stop either server with `Ctrl+C` in its terminal.

## How to use the app

1. On **Today**, click the circle beside a habit to mark it complete. Click it again to undo the entry for that date.
2. Use the arrows in the top bar to review or fill in previous days; **Today** returns to the current date.
3. Select **New habit** to add a name, icon, colour, and the days it should be due.
4. Select **All habits** to search, edit, archive, or restore habits. Archiving hides a habit from the daily list but keeps its completion and streak history.
5. Select **Insights** for weekly completion, total completions, and the longest active streak. The **Habit-by-habit progress** section shows the last 14 days for each active habit: check marks mean completed, dashes mean a scheduled habit was missed, and gray cells are days it was not scheduled. Each card includes a 14-day completion percentage and a bar graph.

## Development notes

- `app.js` contains the app state. On the very first visit, it creates sample habits and saves them as `daymark-habits` in `localStorage`.
- Each habit has a `days` array using JavaScript day indexes (`0` = Sunday through `6` = Saturday) and a `logs` array of local calendar dates in `YYYY-MM-DD` format.
- Streak values are calculated from scheduled days only. A non-scheduled day does not break a streak.
- The UI is responsive at `760px`; use your browser's device toolbar to test narrow layouts.
- Google Fonts are loaded from Google. The app otherwise has no network dependency.

## Debugging

### Open browser developer tools

In Chrome or Edge, press `F12` or `Ctrl+Shift+I`.

- Use the **Console** tab to see JavaScript errors.
- Use the **Elements** tab to inspect layout and test CSS rules.
- Use **Application** → **Local Storage** to inspect saved habits.

### Reset the app to its sample data

This removes only Daymark's saved data in the current browser, then reloads the page:

```js
localStorage.removeItem('daymark-habits');
localStorage.removeItem('daymark-start');
location.reload();
```

Run the snippet in the browser Console while Daymark is open.

### Check JavaScript syntax from the terminal

With Node.js installed, run:

```powershell
node --check outputs\app.js
```

No output and exit code `0` means the file has valid JavaScript syntax.

### Common issues

| Symptom | Likely cause | Fix |
| --- | --- | --- |
| Changes disappear after refresh | Private/incognito browsing, cleared site data, or a different browser profile | Use a normal browser profile and avoid clearing local site data. |
| Sample habits do not reappear | Existing Daymark data is already saved | Use the reset snippet above. |
| Font looks different/offline | Google Fonts could not load | The tracker remains functional; connect to the internet or replace the font import with locally hosted fonts. |
| `py` or `npx` is not recognized | Python or Node.js is not installed or is not on `PATH` | Open `index.html` directly, or install the relevant runtime. |
| Habit is missing from Today | It is archived or not scheduled for the selected date | Check **All habits**, restore it if necessary, or edit its scheduled days. |

## Deployment

Because this is a static site, deploy the contents of `outputs/` to any static host (for example GitHub Pages, Netlify, Vercel, or an internal web server). Set `index.html` as the site entry point. No environment variables or build command are needed.

> Before production use, add a real backend or export/sync feature if users need backup, multi-device access, or shared data.
