# Ambient Life Dashboard

A calm, always-on display: today's events, current weather, and what's on
your mind, set against a Smoky Mountain ridgeline that shifts through dawn,
day, dusk, and night with the real hour.

- **One file.** Everything lives in `index.html` — no server, no build step.
- **Weather** comes from Open-Meteo (free, no key), set to Maynardville, TN.
- **Events & tasks** are added right on the page (pencil button, bottom-right)
  and sync through Firebase when signed in with Google — same account on
  every device means every device shows the same day.
- **Google Calendar** events for today show up automatically once you sign
  in — read-only, tagged "Google" — alongside anything added on the page.
- **Signed out**, it still works; changes just stay on that one device.

Live at: https://emuhleej.github.io/ambient-dashboard/

See DEPLOY.md for setup.
