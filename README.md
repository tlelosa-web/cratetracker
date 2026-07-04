# CrateTracker — F1 Clash

A single-purpose, offline-first Progressive Web App that predicts the premium crate
cycle in the mobile game **F1 Clash**. Ported from the community spreadsheet
"Track & find Premium Crates" by TR The Flash.

- **Offline-first** — after the first load, it runs with no internet at all (service worker caches the full app shell).
- **Installable** — "Add to Home Screen" launches it full-screen with its own icon, like a native app.
- **Private** — all data (baseline, race log, active step) stays in the browser via `localStorage`. Nothing leaves the device.

---

## Live app

Once GitHub Pages is enabled (see below), the app is served at:

```
https://<your-username>.github.io/<repo-name>/
```

Open that URL in Safari (iOS) or Chrome (Android) → share/menu → **Add to Home Screen**.

---

## Deploying to GitHub Pages

1. Create a new GitHub repository and push these files to the `main` branch,
   keeping the folder layout exactly as-is (see **Structure** below).
2. In the repo: **Settings → Pages**.
3. Under **Build and deployment**, set **Source: Deploy from a branch**.
4. Select **Branch: `main`**, **Folder: `/ (root)`**, then **Save**.
5. Wait ~1 minute. The live URL appears at the top of the Pages settings screen.

The included `.nojekyll` file disables Jekyll processing so all files (including
the `icons/` folder) are served verbatim.

---

## Structure

```
.
├── index.html            # The app (single-file HTML/CSS/JS)
├── manifest.json         # PWA manifest — name, icons, standalone launch
├── service-worker.js     # Offline cache-first service worker
├── .nojekyll             # Tell GitHub Pages to skip Jekyll
├── icons/
│   ├── icon-180.png      # apple-touch-icon (iOS home screen)
│   ├── icon-192.png      # Android
│   ├── icon-512.png      # Splash / install
│   └── icon-maskable-512.png  # Android adaptive/circular masking
└── README.md
```

---

## Updating the app

The service worker caches aggressively for offline use, so browsers will keep
serving the old version until the cache is invalidated. **After editing
`index.html` (or any cached file):**

1. Open `service-worker.js`.
2. Bump the version string, e.g. `cratetracker-v1` → `cratetracker-v2`.
3. Commit and push.

Installed devices will pick up the new version on their next launch with a
network connection. Without the version bump, the old cache persists.

---

## The crate model

The per-race crate pattern for all 8 steps is read directly from the source
spreadsheet's conditional formatting and encoded in `index.html`
(`STEP_GOLDS` / `STEP_RC`). Each step lists its Golden-crate races; every other
race up to the premium race is Standard; the premium (Platinum or Legendary)
lands at the step's `STEP_RC` distance.

Two steps carry advisory notes surfaced in-app rather than auto-applied, because
they depend on live in-game timing the app can't observe:

- **Step 1** — Platinum may land on race 13 or 14.
- **Step 3** — every third cycle adds a bonus Golden at race 9, shifting later crates one race later.

---

## Credits

Crate-cycle data: "F1 Clash Track & find Premium Crates" by TR The Flash.
This app is an unofficial fan tool and is not affiliated with F1 Clash or its publishers.
