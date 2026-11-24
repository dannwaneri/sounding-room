# Tech — Sounding Room

## Stack

| Concern | Choice |
|---|---|
| Language | TypeScript (strict) |
| UI | React |
| Bundler / dev server | Vite |
| Testing | Vitest |
| Property-based tests | fast-check |
| Charts / plots | Plain SVG, hand-authored — no chart library |
| Deployment | Cloudflare Pages, serving `dist/` |
| Runtime | Browser only — no Node server, no edge functions |

## Key constraints

- **Static site.** The build output in `dist/` is a set of static files. There is no server process, no serverless function, and no backend API.
- **No external runtime dependencies for math.** `src/core/` must have zero DOM imports and zero network calls. It is pure TypeScript functions that could run in Node or a web worker without change.
- **No chart library.** All plots are drawn as SVG elements constructed in `src/ui/`. This keeps the bundle small and gives full control over log-log axis scaling and overlay rendering.
- **User data stays in the browser.** Nothing is uploaded. LocalStorage or IndexedDB may be used for persistence within the same device.

## File formats handled

| Format | Description |
|---|---|
| CSV | Generic AB/2 + apparent-resistivity columns |
| `.DAT` (Basiri, tab-separated) | Tab-separated, CRLF, header on line 1, no MN/2 column, duplicate AB/2 values are valid overlap readings |
| `.RST` (WinResist result) | Layer model + smoothed data, fixed layout described in `data/basiri/README.md` |

## Testing rules

- Unit and property tests live in `tests/`, mirroring the `src/` tree (e.g. `tests/core/` covers `src/core/`).
- Core math functions must have property-based tests via fast-check wherever an algebraic invariant exists (e.g. homogeneous half-space gives a flat curve equal to ρ; round-trip recovery of a known model; resistivities and thicknesses > 0; depths strictly increasing; misfit ≥ 0).
- A validation report (not a pass/fail test) compares our fit with each .RST model: % difference per layer and RMS misfit of both models against the field data. A test fails only if the report gets worse than the last saved baseline.
- Tests must pass in Vitest's browser-free environment (jsdom or node pool). No test may open a real browser.

## Build and deploy

```
npm run dev      # Vite dev server
npm run build    # outputs to dist/
npm run test     # Vitest
npm run preview  # preview dist/ locally
```

Deployed with `wrangler pages deploy dist --project-name sounding-room`. No environment variables are required at build time.
