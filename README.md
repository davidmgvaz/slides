# slides

David's presentation decks, built with [Slidev](https://sli.dev) and published to GitHub Pages.

**Live:** https://davidmgvaz.github.io/slides/

## Decks

| Folder | Deck | Live |
|--------|------|------|
| `boostinc-django52-pitch-deck/` | Vendlive — Dev Workflow Improvements | https://davidmgvaz.github.io/slides/boostinc-django52-pitch-deck/ |

## Running a deck locally

```sh
cd boostinc-django52-pitch-deck
npm install
npm run dev        # opens http://localhost:3030
```

## How deployment works

`.github/workflows/deploy.yml` runs on every push to `main`. It
**auto-discovers decks** — every top-level folder containing a `slides.md` is
built with Slidev — and publishes the combined output to GitHub Pages:

- the landing page (`index.html`) is served at `/slides/`
- each deck is served at `/slides/<folder>/`

Each deck is built with `--base /slides/<folder>/` so its asset URLs
resolve under the Pages sub-path.

## Adding a new deck

1. Create a new top-level folder with a Slidev deck in it — i.e. containing a
   `slides.md` (e.g. `npm create slidev@latest my-deck`, or copy an existing
   deck folder as a starting point).
2. Add a card for it in `index.html`.
3. Push to `main`.

No workflow edit needed — the deploy job picks up any folder with a `slides.md`
automatically. The only manual touch is the `index.html` card (so it shows on
the landing page).

## One-time setup

In the repo's **Settings → Pages**, set **Source** to **GitHub Actions**.
(The workflow also enables Pages itself via `configure-pages`, so this is
belt-and-braces.)
