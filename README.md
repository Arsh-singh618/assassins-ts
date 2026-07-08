# Assassins T/S — Installable PWA

This folder turns your single-file training simulator into an installable
Progressive Web App: an icon on your phone's home screen, opens full-screen
(no browser bar), and keeps working with no internet connection after the
first load — because all of the app's data already lives in `localStorage`
on the device.

## What's in here
- `index.html` — your original app, with Claude's artifact-preview bridge
  script stripped out (it isn't needed outside claude.ai) and PWA tags added.
- `manifest.json` — tells the phone the app's name, icon, and that it should
  open in standalone (app-like) mode.
- `service-worker.js` — caches the app shell so it loads and works offline.
- `icon-192.png`, `icon-512.png`, `icon-180.png` — icons generated from your
  existing favicon, at the sizes Android/iOS expect.

## One requirement: HTTPS
Service workers (what makes the app installable and offline-capable) only
work over **HTTPS** or on `localhost` — never over plain `http://` on a LAN
IP, and never from a `file://` path opened directly. So you need to host
these 6 files somewhere with HTTPS. Two free options that fit how you
already deploy things:

### Option A — GitHub Pages (free, matches your existing GitHub use)
1. Create a new repo (or a folder in an existing one), e.g. `assassins-ts`.
2. Push these 6 files to it, unchanged, all in the repo root.
3. Repo Settings → Pages → set source to the `main` branch, root folder.
4. GitHub gives you a URL like `https://<username>.github.io/assassins-ts/`.

### Option B — Netlify Drop (fastest, zero setup)
1. Go to https://app.netlify.com/drop
2. Drag this whole folder onto the page.
3. You get an HTTPS URL immediately (e.g. `https://random-name.netlify.app`).

### Option C — Railway static site
Since you already use Railway for AussieMeals/the invoice app, you can serve
this as a static site there too (any static-file buildpack works) if you'd
rather keep everything under one host.

## Installing it on your phone
Once it's hosted somewhere HTTPS:

**Android (Chrome):**
1. Open the URL in Chrome.
2. Tap the **Install app** button that appears bottom-right (or Chrome's
   own "Add to Home screen" prompt / the ⋮ menu → "Install app").
3. It now behaves like a normal app: its own icon, opens full-screen, works
   with airplane mode on.

**iPhone/iPad (Safari):**
1. Open the URL in Safari (must be Safari, not Chrome, for this to work on iOS).
2. Tap the Share icon → **Add to Home Screen**.
3. Launch it from the home screen icon — it'll open full-screen.

## Notes
- All your data (users, content, settings, layout edits) is stored in the
  browser's `localStorage` on that device, exactly like before — nothing
  changed there. It's per-device: installing on a second phone starts fresh.
- If you ever edit `index.html`, bump the version string at the top of
  `service-worker.js` (`assassins-ts-v1` → `assassins-ts-v2`) so installed
  phones actually pick up the update instead of serving the old cached copy.
- The Google Fonts used by the page will fail to load the very first time
  you're offline before they've been cached — after that first successful
  load, the service worker caches them and it's fine offline from then on.
