# CLAUDE.md

Guidance for AI assistants (and humans) working in this repository.

## What this is

A single-file **workout tracking dashboard** — "Ibrahim Workout Tracker". The user
pastes their lifts week by week and the app analyses progress: per-exercise tables,
progression charts, muscle-group volume, a body heatmap, and auto-generated coaching
notes. No backend; everything runs in the browser and persists to `localStorage`.

## Repository structure

```
.
├── index.html    # The entire app: HTML + CSS + JS in one file (open in a browser)
├── README.md     # Project placeholder
└── CLAUDE.md     # This file
```

There is **no build step, package manager, or test suite**. To run it, open
`index.html` in any modern browser. Two external resources load from CDNs:
Google Fonts (Barlow Condensed + Inter) and Chart.js 4.x. The app degrades
gracefully offline — charts show a "needs internet" note, everything else works.

## How the app works (mental model)

- **State** is an array of weekly sessions in `localStorage` under key `iwt_sessions_v1`:
  `[{ week, date, exercises: [{ name, sets: [{ w, r }] }] }]`. Raw sets only — all
  analytics are derived on the fly, never stored.
- **Input parsing** (`parseInput`): a text line starting with a letter is an exercise
  name; a line starting with a digit is a set parsed as `weight reps` (also accepts
  `x`/`*`/`×` separators; a lone number = bodyweight reps). Multiple exercises per paste.
- **Muscle mapping** (`classify` + `RULES`): keyword rules map an exercise name to a
  primary group (`Chest/Shoulders/Arms/Back/Abs/Legs`), a specific label, and secondary
  muscles. Order matters — **specific rules come first, first match wins**. All volume
  math uses *primary* attribution; secondaries are informational tags only.
- **Stats** (`exStats`): total sets/reps, volume = Σ(weight×reps), best set (highest
  est. 1RM), and estimated 1RM via the **Epley formula** `w × (1 + reps/30)`.
- **Render pipeline**: `render()` is the single entry point called after every state
  change. It fans out to `renderWeekSelector / renderSummary / renderTable /
  renderHeatmap / renderBreakdown / renderInsights / renderCharts`.

## Design system (follow this — don't reinvent)

The visual identity is **thermal**: a cold-steel dark UI where trained muscles and
stats glow with "ignition" heat. Keep new work consistent with it.

- **Palette** is defined as CSS custom properties in `:root`. Ground `#0E141B`,
  text `#E7ECF3`, accent (ignition orange) `#FF6A2B`, secondary (steel cyan) `#4CC2FF`.
  Each muscle group has a fixed categorical colour (`--c-chest` … `--c-legs`).
- **Two colour languages, kept separate**: the body heatmap uses a single-hue thermal
  ramp (`heatColor()`, steel→ember→ignition→amber) to mean *intensity*; charts and the
  muscle breakdown use the categorical group colours to mean *category*. Don't mix them.
- **Type**: Barlow Condensed for display/headings/big numbers; Inter for body and tables
  (use `tabular-nums` for figures).
- **Body diagram** is a geometric, segmented SVG (`FRONT_SVG` / `BACK_SVG`). Each muscle
  shape carries `class="muscle" data-group="<Group>"`; JS recolours by `data-group`.
  To add/adjust regions, edit those template strings and keep the `data-group` values
  in sync with `GROUPS`.

## Conventions for AI assistants

1. **Keep it one file, no backend, no build.** All changes go in `index.html`. Don't
   introduce a bundler, framework, or server unless the user explicitly asks.
2. **Single render path.** After mutating `state`, call `save()` then `render()` —
   don't hand-patch the DOM out of band.
3. **Charts are recreated, not mutated.** Use `makeChart(id, cfg)`, which destroys the
   prior instance; keep instances in the `CH` map.
4. **Extending muscle coverage** = add a rule to `RULES` (specific first) and, if a new
   group is ever introduced, update `GROUPS`, `GROUP_COLOR`, `SUGGEST`, and both SVGs.
5. **Respect the palette and the heat/category split** described above.
6. **Accessibility & motion**: keep visible focus styles and honour
   `prefers-reduced-motion` (already wired for CSS transitions and Chart.js animation).
7. **Verify before claiming done.** There's no browser in the dev environment here, so
   at minimum run `node --check` on the inline script and sanity-check tag balance and
   referenced element IDs. Ideally open `index.html` in a real browser to eyeball it.

## Development workflow

- Default/integration branch: `main`. Work on a feature branch; never commit to `main`.
- Branch naming in use: `claude/<short-description>-<suffix>`.
- Commit messages: imperative mood, descriptive. Open a PR only when explicitly asked.

## Build / test / lint commands

_None configured (intentionally — static single-file app)._ Quick local check:

```bash
# syntax-check the inline JS (extract it first, or paste into node --check)
node --check <(extracted-inline-script)
```
