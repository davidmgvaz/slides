# isla-bd-django52

Slidev deck — **Django 5.2: From Database to Application**.

A 4-hour theory + practice session introducing Django 5.2 to the **BD
(Databases)** class — LICE, ISLA Gaia, English. Tailored to students who
have designed a relational database and want the *application-integration
bonus* (up to 10%) on their individual project.

## Run locally

```sh
npm install
npm run dev        # opens http://localhost:3030
```

## Export

```sh
npm run build          # static site -> dist/
npm run export:pdf     # PDF handout
```

## Theme

ISLA-branded theme (`style.css` + `global-bottom.vue`), derived from the
`Isla.potx` "Facet Isla" colour scheme — wine `#5B1F2E`, crimson `#A40624`,
mauve `#BA969A`, cream — on the Slidev `seriph` base. The logo
(`isla-logo-white.png`) sits next to `slides.md`.

## Companion code

The hands-on practice uses the Django **polls app** on PostgreSQL — the
`isla-db-polls` repo (`github.com/davidmgvaz/isla-db-polls`):

- `main` branch — the starter students code along with
- `solution` branch — the completed reference

## Adding to the `davidmgvaz/slides` repo

Copy this whole folder into the repo root, add a card in `index.html`, and
push — the deploy workflow auto-discovers any folder containing a `slides.md`.
