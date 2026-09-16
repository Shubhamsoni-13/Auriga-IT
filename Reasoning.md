The app is built around a “today first” workflow: users open it, see only habits due that day, and tick them off immediately. This keeps the main screen focused instead of overwhelming users with their full habit list.
Each habit stores:
- Its name, icon, colour, scheduled days, optional reminder time, completion dates, and archive status.
- Completion dates are stored as local calendar dates, which makes streak calculations reliable across time zones.
- Current streaks count consecutive completed scheduled days; non-scheduled days never break a streak.
- Best streaks scan the habit’s saved completion history.
Profiles are modeled as separate containers. Each profile owns its own habits, logs, archives, analytics, and 75-day challenge start date. Switching profiles changes the active data set, preventing one user’s habits from affecting another’s.
The reminder system has two layers:
- An in-app morning banner identifies everything still unlogged for today.
- Browser notifications can be enabled once, then the app checks every 30 seconds for habits whose reminder time has passed and are still incomplete. It records sent alerts by profile, habit, and date so users do not receive duplicate notifications.
The Insights screen turns raw check-ins into useful feedback: overall weekly progress plus an individual 14-day calendar and graph for every active habit. This makes missed days, consistency patterns, current streaks, and improvement easy to spot.
The app uses browser localStorage because it keeps setup simple—no account, server, database, or build tools. The tradeoff is that data stays in that browser profile and notifications require the app/browser to remain open.

