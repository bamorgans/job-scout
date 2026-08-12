# Scout — standalone install

Scout is a single-page web app. There's no build step and no framework install.
This folder is everything needed to run it on the web and install it on an iPhone.

```
index.html            the whole app
manifest.webmanifest  makes it installable
sw.js                 offline support (service worker)
icons/                app icons (home-screen, maskable, apple-touch)
```

## Where it runs

| Surface | Works? | Notes |
| :-- | :-- | :-- |
| Desktop browser (Chrome, Safari, Firefox, Edge) | Yes | Open the hosted URL, or the file directly. |
| Mobile Safari / Chrome (iPhone, Android) | Yes | Open the hosted URL. |
| Installed to iPhone home screen (standalone, full-screen) | Yes | Needs to be hosted over **https** (see below). |
| Offline (view cached data) | Yes | After first load on a hosted https URL. |

Cached data (jobs, meetups, intel, settings) is stored **on the device** in `localStorage`,
so it persists between visits with no account and no server.

## 1. Host it (required for iPhone install + offline)

Home-screen install and the service worker only work over **https** (or `http://localhost`).
Any static host works. Easiest is GitHub Pages:

```bash
# in a repo that publishes to Pages
cp -r index.html manifest.webmanifest sw.js icons/ .
git add . && git commit -m "Scout PWA" && git push
```

Enable Pages (Settings → Pages → deploy from branch). Your URL will be like
`https://bamorgans.github.io/job-scout/`. Netlify / Vercel / Cloudflare Pages: just drag this folder in.

Opening `index.html` straight off disk (file://) also works for a quick look, but the
service worker and home-screen install are disabled on file:// — that's a browser rule, not a bug.

## 2. Install on iPhone

1. Open the hosted https URL in **Safari**.
2. Tap the **Share** button.
3. **Add to Home Screen** → Add.
4. Launch it from the icon — it opens full-screen with no browser chrome, like a native app.

(Android/Chrome: the browser shows an **Install app** prompt, or use ⋮ → *Install app*.)

## 3. Turn on live data

Scanning job boards / meetups / company intel calls the Anthropic API, which a browser
can't call directly (CORS). Pick one, in **Setup → Live data connection**:

- **Proxy URL** — run the separate `scout-proxy` service and paste its URL. Key stays on the server. Best for a hosted app.
- **API key** — paste your own Anthropic key; stored only on the device. Fine for personal use; don't ship a public page with a key.

Without either, Scout still runs fully as a local tracker — you can add, edit, sort, search,
bulk-email, and export/import everything; only the live *scan* buttons need the connection.

## Updating

Bump the cache name in `sw.js` (`scout-v1` → `scout-v2`) whenever you change `index.html`,
so installed copies pull the new version instead of serving the old cached shell.
