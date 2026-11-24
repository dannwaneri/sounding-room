# Structure — Sounding Room

## Directory layout

```
sounding-room/
├── src/
│   ├── core/          # Pure math — no DOM, no network, no AI
│   ├── io/            # File importers
│   └── ui/            # Screens, components, SVG plots
├── tests/             # Mirrors src/
│   ├── core/
│   ├── io/
│   └── ui/
├── data/
│   └── basiri/        # Read-only validation dataset (CC BY 4.0)
├── dist/              # Vite build output — not committed
├── .kiro/
│   └── steering/      # These docs
└── public/            # Static assets served as-is
```

## `src/core/` — pure math

Contains the geophysical algorithms. Has **no imports from the DOM, from `src/ui/`, or from any AI service**.

| Module (planned) | Responsibility |
|---|---|
| `forward.ts` | Compute theoretical apparent-resistivity curve from layer model (Ghosh filter / Hankel transform) |
| `inversion.ts` | Fit a layer model to measured data (iterative least squares or similar) |
| `curveType.ts` | Classify a sounding curve as A, K, H, Q, or compound type |
| `model.ts` | TypeScript types for layer models, sounding data, and fit results |

Rules:
- Every exported function is pure (same input → same output, no side effects).
- Numerical invariants (misfit ≥ 0, resistivities > 0, thicknesses > 0) are enforced at the type level or with runtime assertions.
- Property-based tests in `tests/core/` cover algebraic invariants.

## `src/io/` — importers

Parses raw file content (string or ArrayBuffer) into the data types defined in `src/core/model.ts`.

| Module (planned) | Responsibility |
|---|---|
| `csv.ts` | Generic two-column CSV (AB/2, ρ_a) |
| `dat.ts` | Basiri `.DAT` format (tab-separated AB/2, ρa) |
| `rst.ts` | WinResist `.RST` result format |

Rules:
- Importers return `Result<T, ParseError>` (or equivalent discriminated union) — they never throw on bad input.
- No DOM access. Importers receive a string; the UI layer handles `FileReader`.

## `src/ui/` — screens and plots

React components that wire `src/core/` and `src/io/` to the DOM. All SVG plot rendering lives here.

Rules:
- UI components own no business logic. They call `src/core/` functions and pass the results to SVG rendering helpers.
- Log-log axis scaling, tick placement, and curve overlay are computed in helper functions that can be tested without a DOM (pure coordinate transforms).

## `tests/` — test tree

Mirrors `src/`. Each test file covers the module at the matching path.

- `tests/core/` — unit and property-based tests for math functions.
- `tests/io/` — parser tests, including round-trip tests against the Basiri `.DAT` and `.RST` files.
- `tests/ui/` — component tests (jsdom), focused on coordinate-transform helpers and importer wiring.

## `data/basiri/` — read-only validation data

Holds 30 `.DAT` and 31 `.RST` files from the Basiri survey. Nothing in `src/` or `tests/` may modify these files. Tests read them as fixtures.

See `data/basiri/README.md` for the file format details and known gaps in the dataset.
