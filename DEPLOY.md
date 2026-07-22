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
