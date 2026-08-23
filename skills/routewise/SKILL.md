---
name: routewise
description: Adaptive model routing for software-engineering work under cost constraints. Use when the user wants to choose or orchestrate the cheapest model + reasoning strategy for a coding task, bug investigation, plan, refactor, architecture decision, or review without silently lowering quality. Routewise can split work across bounded stages, validate cheap-model results, and escalate capability only when evidence shows it is needed; every stronger model tier requires user approval.
compatibility: Best with runtimes that can spawn model-selectable subagents; degrades to precise recommendation-only routing when they cannot.
---

# Routewise

Route software-engineering work to the least expensive model-and-reasoning strategy that can complete the **whole task correctly**.

A stronger model may be economical when used briefly for a load-bearing plan, decomposition, diagnosis, or architectural decision that makes the remaining work safe for a cheaper model. Never save cost by silently lowering the required quality.

## Language

Continue in the user's language unless they ask otherwise. This skill being written in English must not force an English response.

## Non-negotiable rules

1. **Route the task, not just the prompt.** The result may be one configuration or a sequence of configurations for bounded stages.
2. **Model capability and reasoning effort are separate axes.** Higher reasoning on a cheaper model may beat a stronger model at moderate reasoning when reasoning depth is the bottleneck.
3. **Use evidence, not self-confidence scores.** Do not route from claims like “82% confidence.” Name the unresolved facts, contradictions, assumptions, or validation gaps.
4. **Escalate capability only for capability problems.** Missing context, poor decomposition, insufficient reasoning, and weak validation have cheaper remedies.
5. **Escalation buys bounded work, not ownership of the task.** Downgrade after the bottleneck is resolved.
6. **No human-maintained benchmark dependency.** Never require developers to provide model scorecards, historical routing data, or repeated A/B runs.
7. **Be cost-aware, not falsely cost-predictive.** Do not invent future token counts, total credits, or unpublished reasoning-cost multipliers.
8. **Every stronger model tier requires explicit user approval.** Increasing reasoning inside an already approved tier does not.
9. **Context isolation is optional.** Use it when available and valuable; never require the user to open a new chat.
10. **No retry loops.** Same failure + same evidence + same approach is not a new attempt.

## Private routing state

Track only what matters:

- goal and success condition;
- explicit user model constraints;
- runtime orchestration capabilities;
- verified facts versus open assumptions;
- task dependencies and blocked branches;
- risk, reversibility, and strength of available validation;
- current route: stage -> capability -> reasoning -> validation;
- highest approved model tier;
- branches executed under explicit risk acceptance.

Treat the route as a dependency graph, not a checklist. Keep it private unless showing part of it helps the user decide.

## Capability-first core

Reason first in abstract tiers:

- **economical** — cheapest useful coding/reasoning model available;
- **intermediate** — materially stronger and materially more expensive;
- **strongest** — highest practical capability available.

Reasoning/effort is separate. Use only levels the selected runtime/model actually supports, conceptually ordered from low through medium/high to the highest available level.

The runtime adapter maps these requirements to real model names. At approval time always show the real model and real reasoning/effort value.

For current adapter guidance, read [references/runtime-adapters.md](references/runtime-adapters.md) only when runtime-specific routing or execution is needed.

## Controller posture

When the user can choose the starting configuration, recommend the economical model at **medium** reasoning as the controller.

The controller should spend little reasoning on routine orchestration. If routing/decomposition itself becomes difficult, first delegate that bounded analysis to the economical model at higher reasoning before requesting a stronger capability tier.

Do not force a session restart to satisfy this default. Use the current agent as controller when necessary.

## Runtime mode

### Orchestrated mode

Use when the harness can spawn workers/subagents with suitable model and reasoning controls.

The controller may route, for example:

```text
reconnaissance       -> economical / medium
load-bearing plan    -> economical / highest useful reasoning
hard global decision -> intermediate / high
implementation       -> economical / medium
validation           -> objective checks + economical reviewer if needed
```

Do not change the primary conversation's model merely because one bounded stage needs a stronger worker.

### Advisory mode

Use when the harness cannot execute the desired route itself.

Return:

- recommended actual model + reasoning;
- bounded work to perform on it;
- concrete reason the current configuration is insufficient;
- minimum context/evidence to carry forward;
- a concise resume point.

Never pretend an unsupported model switch or spawn occurred.

## Routing workflow

### 1. Frame the outcome

Determine what the user wants done and what would count as success.

Do not turn a trivial, bounded, objectively testable edit into a routing ceremony. Route cheaply and proceed.

### 2. Apply user constraints

Treat explicit model instructions as constraints, not evidence about difficulty.

Examples:

- “only Luna” -> economical ceiling;
- “do not use Sol” -> intermediate ceiling;
- “use Sol” -> strongest tier is already approved, but cheaper stages remain allowed;
- “use Sol for the entire task” -> honor it and do not optimize those stages downward.

A model ceiling is **not** advance acceptance of risks that have not yet been discovered.

### 3. Detect execution capabilities

Determine whether the runtime can:

- spawn/select a worker model;
- control worker reasoning/effort;
- isolate worker context;
- inspect code/repo/logs/docs;
- run tests/build/static checks;
- resume after worker completion.

Choose orchestrated or advisory mode. Do not ask the user for configuration facts the runtime can inspect itself.

### 4. Initial task fingerprint

Inspect only factors that can change routing:

- local versus cross-cutting scope;
- number and interaction of constraints;
- specified versus inferred behavior;
- whether global interactions must be reasoned about together;
- consequence and detectability of error;
- reversibility/rollback cost;
- quality of objective validation;
- whether the work is mainly execution, diagnosis, planning, synthesis, architecture, or review.

Do not route from category labels alone. “Migration,” “auth,” “refactor,” or “concurrency” are not automatic model assignments.

### 5. Adaptive reconnaissance

Inspect repository context only while new evidence could plausibly change:

- model capability;
- reasoning effort;
- decomposition;
- validation strategy;
- escalation decision.

Before asking the user for a fact available from code, tests, logs, documentation, git state, or connected tools, inspect it.

Useful reconnaissance may include relevant implementation/call sites, tests, dependency versions, similar repo patterns, boundaries, diffs, logs, or reproduction steps.

Stop when more exploration has low expected routing value.

### 6. Diagnose the bottleneck

Before changing model tier, classify the current limiting factor.

**Context gap** — material facts are missing.  
-> Gather the smallest useful evidence.

**Reasoning gap** — facts are available but deeper constraint tracking, synthesis, alternatives, or inference are needed.  
-> Increase reasoning on the current approved tier when useful.

**Scope/decomposition gap** — the work is too broad or entangled and can be split without losing required global interactions.  
-> Decompose and re-route the parts.

**Validation gap** — a plausible result exists but available evidence cannot establish correctness.  
-> Strengthen objective checks or adversarial verification.

**Capability gap** — context is sufficiently known, scope is sensible, adequate reasoning was tried, and a load-bearing problem still cannot be resolved or defended with evidence.  
-> Recommend the next model tier, subject to approval.

Do not buy capability to compensate for missing facts, poor decomposition, or insufficient effort.

### 7. Choose the route

When materially relevant, compare shapes such as:

- one configuration for the whole task;
- stronger planning/decomposition -> cheaper implementation;
- cheap reconnaissance -> bounded stronger diagnosis;
- independent cheap workers -> synthesis;
- one stronger global worker when decomposition would destroy essential interactions.

Orchestration overhead is itself a cost. Do not split work merely because subagents exist.

### 8. Select reasoning versus model capability

Ask what the real bottleneck is.

Use more reasoning when deeper processing of already-understood information is missing.

Use a stronger model when evidence indicates a base capability ceiling.

Never assume a universal ordering such as `economical/max < intermediate/medium`. Evaluate the pair `(capability, reasoning)` for the bounded stage.

If useful reconnaissance still leaves a boundary case between adjacent configurations, choose the safer adjacent configuration. Do not jump directly to the strongest tier just because classification is uncertain.

### 9. Approval gate

A move to the next capability tier requires explicit approval unless the user already approved that tier for this task.

Approval is tier-by-tier:

- economical -> intermediate: ask;
- intermediate -> strongest: ask again.

Approval of intermediate does not imply approval of strongest.

Do not ask approval for reasoning increases inside an already approved tier.

Use this compact shape:

```text
Recommended escalation: <actual model> / <reasoning>
Bounded purpose: <what it will resolve>
Why the current tier is insufficient: <concrete evidence>
Expected route afterward: <downgrade/next stage if known>
```

Do not re-ask a denied escalation for the same reason unless materially new evidence creates a different escalation need.

### 10. Execute bounded workers

Give a stronger worker only the context needed to solve its bottleneck:

- exact goal;
- verified evidence;
- unresolved load-bearing questions;
- constraints/non-goals;
- required output artifact;
- validation expectations;
- assumptions it must not make.

After it returns, re-route. Downgrade whenever the remaining work no longer requires the expensive capability or effort.

## Verification

Validation is adaptive. Prefer objective evidence first; use adversarial checking when a plausible wrong answer could survive cheap checks.

For the full verification protocol, including plan/root-cause/implementation checks, read [references/verification.md](references/verification.md) only when a non-trivial artifact needs verification.

Core rules:

1. Prefer tests, build/typecheck/lint, reproduction, invariants, static checks, executable examples, and repository contracts.
2. For plans, root-cause analyses, architecture, or other weakly executable artifacts, freeze the result and attack its load-bearing claims instead of asking “does this look good?”
3. Criticism without evidence is not an escalation signal.
4. A review with no material findings is valid. Do not invent objections.
5. When verification finds a material flaw, diagnose whether it is context, reasoning, decomposition, validation, or capability before escalating.

## Retry policy

For the same material problem on the same model tier, allow at most **one directed repair/reasoning retry** after diagnosing the failure.

Another attempt is justified only by materially new evidence, a meaningfully different decomposition, or a new validation result.

Otherwise stop retrying and either re-route or escalate.

## If escalation is denied

A denied escalation creates a model ceiling. Recompute the route under it.

Try safe remaining options:

- targeted reconnaissance;
- higher useful reasoning on the allowed model;
- safer decomposition or narrower scope;
- stronger validation;
- completion of independent branches.

Do not silently lower the quality floor.

If a branch remains blocked, explain the unresolved issue and why it matters. Dependent branches remain blocked; independent safe work may continue.

## Risk override

If no safe route remains under the ceiling, offer:

1. approve the recommended escalation;
2. explicitly accept the concrete residual risks and continue best-effort on the lower tier;
3. stop the blocked branch.

State **task-specific risks**, not generic warnings.

After explicit risk acceptance:

- mark the branch **risk-accepted / provisional**;
- automatically choose the strongest reasoning level on the allowed model that is likely to materially help;
- strengthen validation;
- preserve unresolved assumptions explicitly;
- keep downstream dependent results provisional;
- never later call the branch verified unless new evidence resolves the uncertainty.

Risk acceptance permits best effort. It does not permit fabricated certainty.

## Risk and reversibility

Risk raises the required margin of quality but is not an automatic model trigger.

Consider together:

- consequence of error;
- blast radius;
- detectability;
- reversibility/rollback cost;
- test and observability strength;
- uncertainty of underlying facts.

A high-consequence but simple, strongly testable change may remain cheap. A less dramatic but poorly observable and irreversible decision may justify stronger reasoning or capability.

## Cost posture

Use reliable runtime or organization pricing when available, but do not require it.

Never invent:

- exact future input/output/cache usage;
- exact total credits for a task;
- an unpublished reasoning multiplier;
- an assumption that higher reasoning is free because unit token price is unchanged.

Optimize expected whole-task value, not fake precision.

## User-facing behavior

Keep routing overhead proportional to the task.

For simple work, proceed without narrating the framework.

For a non-trivial route, show it only when it helps with cost, approval, or dependencies, for example:

```text
Route
- reconnaissance: Luna / medium
- plan: Luna / max
- implementation: Luna / medium
- validation: tests + targeted self-check
```

When advisory mode requires a manual switch, provide the exact next configuration and resume point.

## Completion states

Use only when materially relevant:

- **verified/completed** — available evidence supports the result at the required quality floor;
- **completed with residual risks** — known non-blocking uncertainty remains;
- **risk-accepted/provisional** — user explicitly accepted work below the recommended capability route;
- **partially completed/blocked** — safe work is done but one or more branches remain blocked.

Do not produce a ceremonial model-by-model report unless the user asks.

## Compact decision rule

1. Can cheap evidence resolve the uncertainty? -> gather it.
2. Is reasoning depth the bottleneck? -> increase effort within the approved tier.
3. Can safe decomposition reduce difficulty without losing essential global context? -> decompose and re-route.
4. Can objective or adversarial validation settle correctness? -> validate.
5. Does a material capability gap remain? -> request approval for the next tier.
6. If denied, recompute under the ceiling; if still blocked, offer explicit risk acceptance or stop that branch.
7. After a stronger worker resolves its bounded problem, downgrade again when possible.

The goal is not to prove the cheapest model can do everything. Spend expensive capability only where it materially improves the probability of a correct outcome.
