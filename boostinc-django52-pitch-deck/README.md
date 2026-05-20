# Vendlive — Django 5.2 dev workflow pitch deck

[Slidev](https://sli.dev) deck pitching the developer-workflow improvements
that rode in with the Django 4.2 → 5.2 upgrade — the work merged to `master`
via the four `INTERNAL-4719-django-52` PRs.

## Run it locally

Requires Node 18+ (any modern version works — same as the Vue frontend).

```sh
cd django52-pitch-deck
npm install
npm run dev          # opens http://localhost:3030
```

Slidev reloads on save. Press `f` for fullscreen, `o` for slide overview,
`?` for the full keymap.

## Export

```sh
npm run export:pdf   # → slides-export.pdf
npm run export:pptx  # → slides-export.pptx
npm run build        # static site in ./dist/ for GitHub Pages, etc.
```

PDF/PPTX export uses Playwright under the hood; the first run will prompt
to install the browser binary.

## Edit

Everything lives in `slides.md`. Slides are separated by `---`.
[Slidev docs](https://sli.dev/guide/syntax.html) cover the syntax,
layouts (`layout: two-cols`, `layout: section`, etc.), and component
embedding.

The deck targets a fellow-developer audience and an informal-but-persuasive
tone — keep that in mind if you tweak it.

## Source for the pitch content

- Branch: [`INTERNAL-4719-django-52`](https://github.com/aeguana/vendlive/tree/INTERNAL-4719-django-52)
- Upgrade brief: [INTERNAL-4719: Cost & Complexity Analysis](https://aeguana.atlassian.net/wiki/spaces/SD/pages/2540437516)
- Inventory of changes: `git log --no-merges master..HEAD`
