# DAD MODE – The 12 Week Reset

A personal fitness and weight-loss web app. Everything is stored on your phone (localStorage). No accounts, no paid services.

## Put it on your iPhone (GitHub Pages, ~5 minutes)
1. On GitHub, create a new public repository called `dad-mode`.
2. Upload every file in this folder (index.html, manifest.json, sw.js and the three PNG icons) to the repo root.
3. Repo → Settings → Pages → Source: "Deploy from a branch", branch `main`, folder `/ (root)` → Save.
4. Wait a minute, then open `https://<your-username>.github.io/dad-mode/` in **Safari** on your iPhone.
5. Tap the Share button → **Add to Home Screen** → Add.
6. Open DAD MODE from the home screen. It runs full screen and works offline.

## Your data
- Stored on this phone, in the home-screen app. Safari and the home-screen app keep separate storage on iOS, so always use the home-screen icon.
- Back up from Settings (gear icon) → Back up data. Restore the same way.
- iOS can clear storage for web apps that go unused for several weeks, so take a backup now and then.

## Adding cloud sync later
1. Create a free Supabase project and a table `dadmode_state (user_id uuid primary key, data jsonb, updated_at timestamptz)` with row-level security tied to `auth.uid()`.
2. Add Supabase Auth (magic link email) to the app.
3. In `save()`, after writing localStorage, upsert `{user_id, data: S, updated_at: now()}`.
4. On load, fetch the row and keep whichever copy has the later `updated_at`.

## Garmin steps, sleep and heart rate
Web apps can't read Apple Health directly, so a Shortcut copies your data and DAD MODE imports it.
1. Garmin Connect app: More → Settings → Connected Apps → Apple Health. Turn on steps, sleep, heart rate and calories.
2. Build the "DAD MODE" Shortcut. The full action list is in the app under Today → From your Garmin → How to set it up.
3. Daily: run the Shortcut, open DAD MODE, tap Import → Paste and import.

## Automatic sync (optional, free)
In the app: Settings (gear) → Cloud sync. It walks you through:
1. A free Supabase project, plus the setup SQL (copy button in the app).
2. Pasting your Project URL and anon key. The app makes a private sync code.
3. A "DAD MODE Sync" Shortcut that posts your Garmin numbers to Supabase, run by Time of Day automations (12:00, 17:30, 21:30).
DAD MODE then pulls the last 14 days of Garmin data every time it opens, and backs up your diary to the cloud.

## Phone nudges
Settings → Phone nudges has four ready-made Shortcut automations (2:45pm snack nudge, morning check-in, weigh-in mornings, wind-down) with copyable messages.

## Barcode scanning
Food → Scan barcode uses your camera (or a photo, or typed number) and the free Open Food Facts database. It needs an internet connection.

## Progress photos
Stored on this phone only (IndexedDB). They are not in backups or cloud sync.
