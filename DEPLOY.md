# Deploy — GitHub Pages (web UI, no command line)

## 1. Create the repo

1. Go to github.com and sign in.
2. Click the **+** (top right) → **New repository**.
3. Name it: `ambient-dashboard`
4. Keep it **Public** (required for free GitHub Pages).
5. Click **Create repository**.

## 2. Upload the files

1. On the new repo page, click **uploading an existing file**.
2. Drag in: `index.html` and `.nojekyll`
   (If `.nojekyll` doesn't show up in your unzipped folder, it's because
   Windows hides files that start with a dot — in File Explorer, go to
   View → Show → Hidden items.)
3. Click **Commit changes**.

## 3. Turn on GitHub Pages

1. In the repo, go to **Settings** → **Pages** (left sidebar).
2. Under "Branch", choose **main** and **/ (root)**, then **Save**.
3. Wait a minute or two, then visit:
   https://emuhleej.github.io/ambient-dashboard/

## 4. Firebase — two quick checks

The page is already wired to your existing Firebase project
(emergency-hub-b19b2). Two things to verify in the Firebase console:

**a) Authorized domain** — Authentication → Settings → Authorized domains.
`emuhleej.github.io` should already be listed (your Emergency Hub uses it).
If it's there, you're done with this step.

**b) Firestore rules** — Firestore Database → Rules. The dashboard stores
data under `users/{your-uid}/events` and `users/{your-uid}/tasks`. Make
sure your rules include a block like this (add it inside the existing
`match /databases/{database}/documents` block, without removing what's
already there):

    match /users/{userId}/{document=**} {
      allow read, write: if request.auth != null && request.auth.uid == userId;
    }

This means: each signed-in person can only read and write their own data.

## 4b. Turn on real Google Calendar sync

Signing in now also asks for read-only access to your Google Calendar, so
today's actual events show up next to anything you add by hand. Two things
to check in the Google Cloud project behind your Firebase project
(console.cloud.google.com, same project as `emergency-hub-b19b2`):

**a) Enable the API** — APIs & Services → Library → search "Google Calendar
API" → Enable.

**b) OAuth consent screen** — APIs & Services → OAuth consent screen.
- Add the scope `.../auth/calendar.readonly`.
- If the app is in "Testing" mode (normal for a personal project), add your
  Google account under **Test users** — otherwise Google will block the
  calendar permission at sign-in.

That's it — no new API key needed, the existing Firebase Google sign-in
button now requests calendar access too. Events are pulled from **all** of
your visible calendars (family, shared, subscribed), not just the primary
one. Google's access tokens expire after about an hour; the page refreshes
automatically every 5 minutes while the tab is open, and if it lapses
(e.g. the display was on overnight), the last-synced events stay on screen
and a **Refresh Google Calendar** button in the edit panel reconnects with
one click.

Fetched Google events are also shared through Firebase (stored under
`users/{your-uid}/gcal`, which the rules in step 4 already cover), so a
device that can't run the Google sign-in popup — like the iPad home-screen
app — still shows calendar events as long as any of your signed-in devices
has synced recently.

Google Calendar events show with a small "Google" tag and can't be deleted
from the dashboard (edit them in Google Calendar itself); events you add on
the dashboard work as before.

## 5. On the iPad

1. Open Safari → https://emuhleej.github.io/ambient-dashboard/
2. Tap the pencil (bottom right) → **Sign in with Google**.
3. Share button → **Add to Home Screen** (the moon icon comes with it).
4. Settings → Display & Brightness → Auto-Lock → Never, if you want it
   to stay on as an ambient display.

Sign in with the same Google account on your PC's browser and the iPad —
that's what makes them show the same events and tasks.

## Updating later

Any time you get a new `index.html`: open the repo on github.com, click
`index.html` → the pencil icon (Edit) → paste the new contents → Commit.
Or delete the old file and upload the new one. Pages refreshes itself in
a minute or two.
