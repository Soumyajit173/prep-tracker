# Prep Tracker

A daily consistency tracker for product-company interview prep — DSA, Java/Spring, revision, and project work — built as an installable web app (PWA). Works fully offline once installed, with all data stored locally on your device.

## Features

- **Weekly view** — each day shows a checklist of study tasks, plus any fixed commitments (coaching, training) that block out part of that day
- **Full CRUD** — add, edit, and delete both tasks and fixed commitments directly in the app
  - Tap a task's text to edit it inline
  - Tap **+ Add a commitment** on any open day, or **Edit** / **Remove** on an existing one
  - Fixed commitments are set per weekday (e.g. every Thursday), not per date, so editing one updates all future weeks
- **Streak tracking** — counts consecutive fully-completed days
- **10-week heatmap** — a GitHub-style contribution graph showing consistency over time
- **Per-day progress ring** — each day card shows a small ring that fills as tasks are completed
- **Offline-first** — a service worker caches the app shell, and all data is stored locally in the browser via **IndexedDB**, so it works with no internet connection after first load
- **Installable on Android** — via Chrome's "Add to Home screen" / "Install app," it runs as a standalone app with its own icon, no browser bar

## Tech stack

Plain HTML, CSS, and JavaScript — no build step, no dependencies, no framework. Data persistence uses the browser's native **IndexedDB**. Offline support and installability come from a **Web App Manifest** (`manifest.json`) and a **Service Worker** (`service-worker.js`).

## File structure

```
.
├── index.html        # The entire app (markup, styles, and logic)
├── manifest.json      # PWA manifest — name, icons, display mode
├── service-worker.js  # Caches app files for offline use
├── icon-192.png        # App icon (192×192, used for install/home screen)
├── icon-512.png        # App icon (512×512, used for install/splash)
├── favicon.ico          # Browser tab icon (multi-resolution)
├── favicon-16.png       # Browser tab icon (16×16)
└── favicon-32.png       # Browser tab icon (32×32)
```

All internal paths are relative with no folder prefix, so this repo can be served from the root of a domain or from a subpath (e.g. GitHub Pages project sites) without any changes.

## Running it locally

No build step needed. Any static file server works, for example:

```bash
python3 -m http.server 8000
```

Then open `http://localhost:8000` in a browser. Note: IndexedDB and the service worker require `http://localhost` or `https://` — opening `index.html` directly via `file://` will not fully work.

## Deploying with GitHub Pages (free)

1. Push all the files in this repo to GitHub (already done if you're reading this from the repo).
2. Go to **Settings → Pages**.
3. Under **Source**, select the `main` branch and the root folder (`/`).
4. Save, and wait about a minute for it to publish at `https://<your-username>.github.io/<repo-name>/`.

## Installing on Android

1. Open the deployed URL in **Chrome** on your Android phone.
2. Tap the **Install app** banner if it appears, or open the ⋮ menu and choose **Add to Home screen**.
3. The app now runs standalone from your home screen, with offline support.

## Data & privacy

All data (tasks, completion status, custom commitments) is stored **only on your device**, inside the browser's IndexedDB. Nothing is sent to a server — there is no backend. This also means:

- Data does not sync across devices or browsers. Installing the app on a second phone starts with a fresh, empty tracker.
- Clearing your browser's site data for this app (or uninstalling it) will erase your history and streak.
- Uninstalling and reinstalling from the *same* URL on the *same* device/browser generally preserves data, but this isn't guaranteed — export/backup isn't currently supported.

Use the **Reset all saved progress** button at the bottom of the app to manually clear everything and start over.

## Customizing

- **Default daily tasks**: edit the `DEFAULT_TASKS` array near the top of the `<script>` block in `index.html`.
- **Default fixed commitments**: edit the `DEFAULT_FIXED` object in the same place — keyed by weekday (`0` = Sunday … `6` = Saturday). These are just the *starting* defaults; commitments can also be edited or removed entirely from within the app itself.
