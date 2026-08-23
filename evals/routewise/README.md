# Routewise regression fixtures

These cases protect Routewise's routing-policy contract while the skill evolves.

They are behavioral fixtures, not golden-response tests. A model may produce different wording while still preserving the required routing decisions.

## Status

- `baseline` — behavior required by the current Routewise skill and expected to remain stable.
- `target` — a known hardening requirement intentionally documented before the policy change lands. Promote it to `baseline` when the corresponding policy is implemented and the controlled regression check passes.

## Current method

For an instruction-only change:

1. Review or run the current skill against every `baseline` case.
2. Check that every `must` behavior is present and every `must_not` behavior is avoided.
3. Apply the candidate instruction.
4. Repeat the same cases without changing the rubric.
5. Reject or revise the candidate if a baseline invariant regresses.
6. For `target` cases, record whether the planned hardening change now satisfies the fixture, then promote it to `baseline`.

Controlled dry-run reports are stored under [`results/`](results/). They record the source revision, method, per-case result, and limitations.

The suite is intentionally lightweight. It is suitable for controlled regression and design work, not for claiming statistically independent benchmark results.

A future evaluation harness can execute the same cases repeatedly against real runtimes and record model tier, reasoning effort, worker count, usage, credits, latency, escalation decisions, and task outcomes.