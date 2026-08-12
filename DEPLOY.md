# Deploy Scout to GitHub Pages

You run these on your machine (they need your GitHub login, which stays with you).
Everything is already structured for a Pages subpath — relative paths, service-worker scope,
and manifest all resolve correctly under `https://bamorgans.github.io/job-scout/`.

Put this whole folder's contents at the root of the new repo:

```
index.html  manifest.webmanifest  sw.js  .nojekyll  README.md  icons/
```

---

## Option A — GitHub CLI (fastest, one shot)

Requires the `gh` CLI, logged in (`gh auth login`).

```bash
cd job-scout                       # this folder
git init -b main
git add .
git commit -m "Scout: local-first opportunity radar (PWA)"

# creates the repo under your account and pushes in one step
gh repo create job-scout --public --source=. --remote=origin --push

# enable Pages from the main branch, root folder
gh api -X POST repos/bamorgans/job-scout/pages -f "source[branch]=main" -f "source[path]=/"
```

Give it ~1 minute, then open: **https://bamorgans.github.io/job-scout/**

---

## Option B — GitHub website + git

1. Go to https://github.com/new → name it **job-scout** → Public → **Create repository**
   (don't add a README; this folder has one).
2. In this folder:

```bash
cd job-scout
git init -b main
git add .
git commit -m "Scout: local-first opportunity radar (PWA)"
git remote add origin https://github.com/bamorgans/job-scout.git
git push -u origin main
```

3. On GitHub: **Settings → Pages** → Source: *Deploy from a branch* →
   Branch: **main**, Folder: **/ (root)** → Save.
4. Wait ~1 min, then open **https://bamorgans.github.io/job-scout/**.

---

## Install on your iPhone

1. Open **https://bamorgans.github.io/job-scout/** in **Safari**.
2. **Share** → **Add to Home Screen** → Add.
3. Launch from the icon — full-screen, no browser chrome.

## Updating later

Edit files, then bump the cache in `sw.js` (`scout-v1` → `scout-v2`) so installed copies
fetch the new version, and:

```bash
git add . && git commit -m "update" && git push
```

Pages redeploys automatically in about a minute.

## Notes

- Live scanning still needs a connection (proxy URL or your own key) set in **Setup**.
  The app itself — tracking, sorting, search, bulk email, export/import — works with neither.
- To serve at the **root** (`https://bamorgans.github.io/`) instead of `/job-scout/`, the repo must be
  named `bamorgans.github.io`. That's your portfolio repo, so a `job-scout` project repo (subpath) is
  the clean choice and needs no path changes.
