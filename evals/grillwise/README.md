# Grillwise regression fixtures

These cases protect Grillwise's behavioral contract while its instruction is simplified.

They are deliberately **behavioral**, not golden-response tests. A model does not need to produce identical wording before and after a refactor. It should preserve the decision-making invariants captured by each case.

## Current method

For an instruction-only refactor:

1. Run or review the baseline skill against every case.
2. Record whether each `must` behavior is present and each `must_not` behavior is avoided.
3. Apply the candidate instruction.
4. Repeat the same cases without changing the rubric.
5. Reject or revise the candidate if a material invariant regresses.

The initial suite is intentionally lightweight. It is suitable for controlled manual/dry-run regression, not for claiming statistically independent benchmark results.

A future automated harness can run each case multiple times through an API and use an independent judge or deterministic checks where possible.
