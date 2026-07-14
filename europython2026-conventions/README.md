# I Am a Sucker for Conventions

EuroPython 2026 talk — 30 min, Chamber Hall B (S3B), Kraków.
**15 July 2026, 14:30–15:00 CEST** (13:30 Lisbon).

> Why Django's defaults work, until they don't.

**Status: 37 slides, clocked at 24:05** (practice-calibrated; ~6 min Q&A
in the 30-min slot; four slides are 10-second "history repeats"
flip-cards). Full outline, content decisions,
and open TODOs live in [`HANDOFF.md`](./HANDOFF.md) — read that first.

> ⚠️ **Embargo:** slide 2 contains the first public announcement of
> DjangoCon Europe 2027 (Innsbruck). Do **not** push to `main` (GH Pages
> auto-deploys) or share exports before the talk.

## Run locally

```sh
npm install
npm run dev        # opens http://localhost:3030
```

## Theme & style

[`seriph`](https://github.com/slidev-community/slidev-theme-seriph) +
**"Patch Notes" custom style** (`style.css`, direction 1b — Django greens on
warm paper, mono/terminal/diff voice; class cookbook in `README_style.md`,
application notes in `HANDOFF.md` §7). Code panels need the dark shiki
tokens from `setup/shiki.ts`. The penguin-theme history is in `HANDOFF.md`
§7; the two no-op leftovers (`global-top.vue`, `vite.config.ts`) are safe to
delete.

## Structure (3 acts)

| Act | Sections | ≈ time |
|---|---|---:|
| Hook + thesis | intro · whoami · confession · trade-offs (1–9) | 5:35 |
| The tour + the story | official hatches (10–15) · 2010 monkey-patch (16–20) | 12:30 |
| The negotiation + landing | flip-cards (22–25) · Carlton · bend don't break · aside · takeaways | 10:50 |

## Before the talk

- Rehearse to time (scripted 28:55 — buffer is thin). Practice the fast flip
  through slides 22–25: two stories slow, four cards fast.
- Visual QA after the restyle: title slide (cursor + footer logos), slide 2
  (kv grid + announce), slide 7 & 31 (diff rows), slides 22–25 (cards),
  slide 26 (quote scale), slide 33 (Boost logo), and one dark-green code
  panel for token contrast.
- Optional: real Boost 5.2 code snippet for the aside; speaker portrait.
- After the talk: regenerate repo-root landing card + README row, then push.
