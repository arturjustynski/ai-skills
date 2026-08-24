# Routewise Plan regression fixtures

These cases protect the contract of `routewise-plan`.

The suite was derived from real planning-session retrospectives, then generalized so it contains no project-specific or private repository details.

The main invariants are:

- show a routing gate before deep planning;
- remain read-only and never implement;
- distinguish recommendation from actual runtime execution;
- verify or label assumptions instead of silently treating them as facts;
- shape the plan so downstream stages can be executed by cheaper models where practical;
- spend stronger planning capability only when the leverage can amortize its cost.
