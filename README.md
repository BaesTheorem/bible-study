# Study Bible

A self-contained study app for the [Skeptic's Annotated Bible](https://www.skepticsannotatedbible.com) (KJV, all 66 books) and the Skeptic's Annotated Book of Mormon (all 15 books). Static files, no build tooling, no server-side code. Installs to a phone or desktop as a PWA and works offline once the data is cached.

## Features

- **Split screen.** Two independent reading panes. Each picks its own collection, book, and chapter, so you can put Genesis 1 next to 1 Nephi 1, or two chapters of the same book side by side.
- **SAB annotations.** Steve Wells' verse notes, section summaries, endnotes, and category markings (Absurdity, Injustice, Contradiction, Science and History, Plagiarism, and the rest) render alongside the text. Toggle them off per pane for a plain reading view. Cross-reference links navigate inside the app; contradiction pages and other articles open on the SAB site.
- **Highlights and notes.** Tap a verse for five highlight colors and a personal note editor. Everything lands in a "My study" list with jump links.
- **Search** across both collections, or scoped to one.
- **Real backup, not just browser cache.** Highlights and notes persist in IndexedDB, with two ways out of it:
  - one-tap export/import of a JSON backup file
  - optional sync to a **private GitHub Gist**, which also keeps multiple devices on the same data (newest edit per verse wins)
- Light/dark theme, adjustable text size.

## Running it

Any static file server works. Locally:

```bash
python3 -m http.server 8080
```

Then open http://localhost:8080. For phone install, serve it over HTTPS and use the browser's "Add to Home Screen". Service workers require HTTPS or localhost.

If `data/` is missing or incomplete, rebuild it from the source site (stdlib-only scraper, polite 0.4s delay between requests, about 20 minutes for everything):

```bash
python3 build-data.py            # everything
python3 build-data.py --sample   # 7 books, for a quick look
python3 build-data.py --book gen --book 1ne   # specific books
```

Partial builds merge into the existing manifest.

## Gist sync (optional)

Settings → GitHub Gist sync. Create a token at GitHub → Settings → Developer settings with **only the gist scope**, paste it in, hit Sync now. First sync creates a private gist named `study-bible-backup.json`; every device with the token converges on the same highlights and notes. The token stays in the browser's localStorage.

## Content and credit

Scripture text (KJV and the 1830 Book of Mormon) is public domain. The annotations, notes, and category markings are the work of Steve Wells at [skepticsannotatedbible.com](https://www.skepticsannotatedbible.com), who also publishes them as a print book worth buying.

## Files

| File | What |
|---|---|
| `build-data.py` | Scraper/builder. Fetches skepticsannotatedbible.com, writes `data/manifest.json` plus one JSON per book. |
| `index.html`, `css/app.css`, `js/app.js` | The whole app. Vanilla JS, no dependencies. |
| `sw.js` | Service worker: offline app shell, caches scripture data as you read it. |
| `manifest.webmanifest`, `icons/` | PWA install metadata. |
