# Kiro build log

One entry per action: what I asked Kiro, what it did, what I changed.
This log is the source for the "How Kiro was used" write-up.

## 2026-11-24

- **Setup.** Repo, MIT license, Basiri data and a placeholder page made
  before Kiro (no app code). Kiro IDE 1.4.236 installed.
- **Sign-in.** GitHub sign-in failed: the token request returned 401
  (ref f4c8c498-7031-7019-502b-bce8aacf2b2a). AWS Builder ID worked.
  Skipped the VS Code import, to keep Kiro clean.
- **Steering (Vibe mode).** Asked Kiro to write `product.md`, `tech.md`
  and `structure.md` from the README, the data README and a short list of
  facts. Took 1m 51s, about 0.42 credits.
- **Steering review.** The draft was good, but 9 points needed a fix
  before any spec:
  1. "no modern alternative" was false (pyGIMLi, ResIPy exist).
  2. "closed-source, no longer maintained" was not checked.
  3. It called the Basiri `.DAT` a WinResist format. Only `.RST` is.
  4. It named "forward model reciprocity" as a test invariant. Replaced
     with real VES invariants (flat curve for half-space, round trip,
     positive values, increasing depths).
  5. It planned exact regression tests against the `.RST` models. Those
     would fail by design (station 26 has two valid models). Replaced
     with a validation report plus a baseline check.
  6. It left the UI framework open. Chose React.
  7. It said Pages builds on push. Not set up; we deploy with wrangler.
  8. It claimed "offline-capable" with no service worker. Removed.
  9. No rules file. Added `rules.md` ("list the gaps and stop",
     "unclassified", iteration cap, row-by-row rejection).
  I fixed 1, 2, 4, 5 and 7 by hand. Claude (Opus 5.5) fixed the rest and
  wrote `rules.md` from our planning notes.
