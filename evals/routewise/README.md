# Routewise entry-point regression fixtures

These cases protect the post-split contract of the `routewise` entry point.

`routewise` is intentionally recommendation-only. It selects the next Routewise workflow and a provisional model/reasoning route, but it does not create the implementation plan or execute the implementation.

The historical dry-run reports in `results/` describe the earlier monolithic Routewise design and are retained as project history. New behavior is evaluated against the split workflow suites:

- `evals/routewise/` — recommendation-only entry point;
- `evals/routewise-plan/` — execution-optimized planning;
- `evals/routewise-implementation/` — ready-plan routing and optional execution.

The fixtures are behavioral, not golden-response tests. They protect decisions and guardrails rather than exact wording.
