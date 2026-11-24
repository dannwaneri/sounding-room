# Rules for every task

- If a requirement is unclear or missing, list the gaps and stop. Don't guess.
- No AI or network calls in `src/core/`. The numeric core is deterministic.
- Never guess a curve type. If no rule fits, return "unclassified".
- Iterative methods have a fixed iteration cap and at most one restart.
  No loop-until-converged.
- Reject bad input rows one by one, with a reason. Load the rest.
- Never edit files in `data/basiri/`. They are source data under CC BY 4.0.
- Keep each source file under 200 lines.
- Every new function in `src/core/` gets a test. Every invariant gets a
  property-based test.
