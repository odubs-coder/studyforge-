# StudyForge — deploying to GitHub Pages

Everything in this folder is the website. No build step, no npm, no framework.

```
index.html              the whole app
manifest.webmanifest    makes it installable as an app
sw.js                   service worker — lets it work offline
.nojekyll               stops GitHub mangling the files
icons/                  homescreen icons
```

---

## 1. Create the repository

1. Go to **github.com/new**
2. Repository name: `studyforge` (this becomes part of your URL)
3. Set it to **Public** — GitHub Pages needs Public on a free account
4. Do **not** tick "Add a README", since you already have one
5. Click **Create repository**

## 2. Upload the files

Easiest route, no command line:

1. On the new empty repo page, click **uploading an existing file**
2. Drag in `index.html`, `manifest.webmanifest`, `sw.js`, `README.md`
3. Drag the whole `icons` folder in as well — keep it as a folder
4. Scroll down, click **Commit changes**

`.nojekyll` is a hidden file and drag-and-drop often skips it. Add it manually:

1. Click **Add file → Create new file**
2. Name it exactly `.nojekyll`
3. Leave the contents empty
4. **Commit changes**

## 3. Turn on Pages

1. In the repo, go to **Settings** (top row) → **Pages** (left sidebar)
2. Under **Source**, choose **Deploy from a branch**
3. Branch: **main**, folder: **/ (root)**
4. **Save**

Wait two or three minutes. Refresh the Pages settings screen and your URL appears at the top:

```
https://<your-username>.github.io/studyforge/
```

## 4. Connect the AI

Card generation, quizzes and marking need an Anthropic API key once the app is on your own domain.

1. Open the site, create your account
2. Go to **You → AI connection**
3. Paste a key from **console.anthropic.com**

**Read this before you do.** The key is stored in your own browser only, and is never committed to
GitHub. That is fine for you testing on your own phone. It is **not** fine once other people use the
site, because the key travels from the browser and can be read by anyone using it. Before a single
student touches this, the API call needs to move behind a small server-side proxy so the key never
reaches the browser. Ask me and it is a short job.

**Never paste your key into `index.html` and commit it.** Public repo means public key, and it will
be scraped within hours.

## 5. Add it to your homescreen

**iPhone / iPad (Safari — it must be Safari, not Chrome)**
1. Open your Pages URL in Safari
2. Tap the **Share** button (square with an arrow)
3. Scroll down, tap **Add to Home Screen**
4. Tap **Add**

**Android (Chrome)**
1. Open your Pages URL in Chrome
2. Tap the **⋮** menu
3. Tap **Install app** or **Add to Home screen**
4. Tap **Install**

It launches full screen with no browser bar, has its own icon, and works offline.

---

## Updating it later

Upload the new `index.html` over the old one, **and** bump the cache version in `sw.js`:

```js
const CACHE = 'studyforge-v2';   // was v1
```

If you skip that, the service worker keeps serving the old version and you will think your changes
did not deploy.

---

## Things worth knowing

- **Your data lives in the browser, not the cloud.** Decks, streak and account are stored on the
  device. A different phone means a different set of decks, and clearing browsing data wipes them.
  Real sync needs a backend.
- **Public repo, public site.** Anyone with the link can open it. There is no way to restrict a free
  GitHub Pages site.
- **HTTPS comes free** with Pages, which is required for the service worker and for the password
  hashing to work properly.
- **Custom domain** is optional: Settings → Pages → Custom domain, then point a CNAME at
  `<your-username>.github.io` with your registrar.
- **Still pre-launch.** Replace "StudyForge AI" in the footer with your registered company name,
  number and address before this goes anywhere near real users, and get the privacy notice and DPIA
  finished before any pupil account is created.
