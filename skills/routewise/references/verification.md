# Adaptive verification protocol

Use this reference when a non-trivial result cannot be established cheaply by objective checks alone.

## Verification ladder

### 1. Objective evidence first

Prefer evidence such as:

- tests;
- build/typecheck/lint;
- reproduction of the original failure;
- property or invariant checks;
- static analysis;
- executable examples;
- repository contracts and documented behavior.

When objective evidence strongly establishes the result, do not run a heavyweight adversarial review by habit.

### 2. Adversarial artifact check

Use for weakly executable artifacts such as plans, root-cause analyses, architectural decisions, or interpretations of system behavior.

Freeze the artifact. Review the artifact and evidence, not the author's desire for it to be correct.

Extract only the few load-bearing claims or decisions. For each material one ask:

- What direct evidence supports it?
- What observation would falsify it?
- Does repository reality contradict it?
- Is a necessary caller, dependency, state transition, interface, or failure path missing?
- Is a hard decision hidden behind vague wording?
- Is the artifact very detailed on routine mechanics but vague on the risky parts?
- Is there an obvious alternative that changes cost, risk, reversibility, or scope?
- Can the claimed success be observed or tested?

Classify findings by consequence:

- **holds** — evidence supports the claim;
- **minor** — low-impact weakness or polish;
- **major** — material weakness affecting quality, risk, or rework;
- **blocker** — the route/result must not proceed as if verified.

A finding without evidence is not an escalation signal. Zero major findings is legitimate; do not manufacture criticism.

### 3. Optional isolated same-tier review

If the runtime can cheaply create an isolated same-tier worker, it may receive:

- frozen artifact;
- verified evidence;
- unresolved questions;
- review instructions;

but not the reasoning that produced the artifact.

Use this only when independence has enough value to justify another call. It is never mandatory and never requires a new user chat.

### 4. Diagnose before escalating

If verification finds a major or blocker, classify the cause:

- context gap -> gather evidence;
- reasoning gap -> one directed higher-effort repair;
- scope/decomposition gap -> re-scope;
- validation gap -> strengthen checks;
- capability gap -> recommend next capability tier.

Do not equate failed self-review with automatic model escalation.

## Planning check

Plans deserve stronger verification because they can be fluent and detailed while leaving implementation-critical decisions unresolved.

Check whether:

- the difficult decisions are actually decided rather than renamed;
- interfaces and data/control boundaries are concrete enough for execution;
- failure behavior is specified where it matters;
- sequencing has observable completion/verification points;
- risky or irreversible steps have rollback/escape paths;
- detail is concentrated on risky parts rather than routine mechanics;
- the implementer can execute bounded stages without inventing missing architecture;
- an obvious alternative was considered when it could materially change the route.

If the plan holds, continue. Do not escalate solely to obtain a second opinion.

## Root-cause check

For a debugging hypothesis:

- Does it explain every important observed symptom?
- What observation would contradict it?
- Are multiple plausible causes still alive?
- Can code, logs, instrumentation, or a targeted test distinguish them?
- Does the proposed fix address the cause rather than only the symptom?

A reproducible failing test that turns green for the right reason is stronger evidence than a persuasive narrative.

## Implementation check

After meaningful code changes verify:

- required behavior is covered;
- relevant callers/contracts were not missed;
- material failure and edge paths were considered;
- implementation still matches any validated load-bearing plan decisions;
- relevant tests/build/typecheck/static analysis were run when available;
- provisional upstream assumptions remain explicitly provisional.

Do not automatically buy a stronger code reviewer when objective evidence is already strong.

## Risk-override verification

After the user accepts risk to remain below the recommended capability tier:

- use the strongest reasoning level on the allowed tier that is likely to help materially;
- strengthen objective validation where possible;
- run adversarial checking on load-bearing claims;
- keep unresolved assumptions explicit;
- mark dependent output provisional;
- do not let passing superficial tests erase an uncertainty those tests cannot actually cover.
