# Routewise Implementation regression fixtures

These cases protect the contract of `routewise-implementation`.

The suite focuses on failures observed in real implementation-routing sessions:

- choosing an intermediate model by intuition instead of comparing `(model, reasoning)` pairs;
- failing to distinguish a first attempt from conditional fallback;
- treating plan weakness as model weakness;
- overlooking when a bounded re-plan can amortize itself across many cheaper execution stages;
- executing when the user asked only for a recommendation.

The fixtures are behavioral and intentionally avoid exact response wording.
