# slides

David's presentation decks, built with [Slidev](https://sli.dev) and published to GitHub Pages.

**Live:** https://davidmgvaz.github.io/slides/

## Decks

| Folder | Deck | Live |
|--------|------|------|
| `django52-pitch-deck/` | Vendlive — Dev Workflow Improvements | https://davidmgvaz.github.io/slides/django52-pitch-deck/ |

## Running a deck locally

```sh
cd django52-pitch-deck
npm install
npm run dev        # opens http://localhost:3030
```

## How deployment works

`.github/workflows/deploy.yml` runs on every push to `main`. It builds each
deck with Slidev and publishes the combined output to GitHub Pages:

- the landing page (`index.html`) is served at `/slides/`
- each deck is served at `/slides/<folder>/`

Each deck is built with `--base /slides/<folder>/` so its asset URLs
resolve under the Pages sub-path.

## Adding a new deck

1. Create a new top-level folder (e.g. `npm create slidev@latest my-deck`, or
   copy an existing deck folder as a starting point).
2. Add a build step to `.github/workflows/deploy.yml`:

   ```yaml
   - name: Build my-deck
     working-directory: my-deck
     run: |
       npm install
       npx slidev build --base /slides/my-deck/ --out ../dist/my-deck
   ```
3. Add a card for it in `index.html`.
4. Push to `main` — Pages redeploys automatically.

## One-time setup

In the repo's **Settings → Pages**, set **Source** to **GitHub Actions**.
