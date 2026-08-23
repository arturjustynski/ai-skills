# Runtime adapters

Use this reference only when Routewise needs to translate abstract capability/reasoning requirements into an executable runtime route.

## General adapter rules

1. Prefer runtime introspection over stale assumptions when model availability, allowed reasoning levels, effective model substitutions, or observable resource signals can be inspected reliably.
2. Respect organization allowlists, provider/data boundaries, managed settings, permissions, user ceilings, and runtime credit/budget controls.
3. Never claim a worker used a requested model or reasoning level if the runtime substituted something else; use the actual effective configuration when observable.
4. If exact model or reasoning control is unavailable, fall back to advisory mode for that dimension rather than pretending it is controllable.
5. Context-isolated workers are optional. They are especially useful for reconnaissance or adversarial review, but must never require manual user context transfer.
6. Capability approval does not authorize a provider change, broader data exposure, or additional tool permissions.

## Codex / GPT-5.6

Default capability map when GPT-5.6 family models are available:

- economical -> `gpt-5.6-luna`
- intermediate -> `gpt-5.6-terra`
- strongest -> `gpt-5.6-sol`

Treat reasoning/thinking level as a separate runtime-observed axis. A particular Codex surface may expose only a subset of levels, and labels can differ between surfaces.

When the current surface exposes labels such as `low`, `medium`, `high`, `extra high`, `max`, or an additional `ultra` level on selected models, preserve those exact runtime labels in user-facing recommendations. Do not assume any level exists unless the active runtime or UI confirms it.

A reasoning increase inside an already approved model tier does **not** require a new capability approval. It may still consume materially more credits or latency, so Routewise should choose the smallest useful increase rather than mechanically walking every level.

If the runtime exposes actual credit usage or remaining allocation, use it as resource evidence. Do not reverse-engineer or invent hidden credit multipliers. An available allocation is a ceiling/resource budget, not a quality target to spend down.

Current Codex multi-agent configuration may support a default spawned-agent model and reasoning effort, with explicit spawn settings taking precedence. Multi-agent collaboration may expose spawn/send/resume/wait/close primitives. When those primitives are available, keep the primary thread as controller and spawn bounded workers with the selected model/reasoning configuration.

Do not bypass Routewise approval gates merely because Codex can technically spawn a stronger model.

### Codex routing pattern

Start routine orchestration cheaply:

```text
controller: gpt-5.6-luna / medium
```

For a reasoning gap, stay on the approved capability tier and choose one materially stronger useful thinking level supported by that model, for example:

```text
planning worker: gpt-5.6-luna / extra high or max (when supported)
```

Do not automatically try `high -> extra high -> max` as a ladder. The point is to test whether additional reasoning resolves the bottleneck without buying more base capability.

For a confirmed capability gap in a bounded architectural decision:

```text
controller: gpt-5.6-luna / medium
approval requested: gpt-5.6-terra / high
Terra scope: resolve the load-bearing architectural decision only
remaining implementation: gpt-5.6-luna / medium when sufficient
```

If Terra is already approved and the runtime offers a higher reasoning setting such as `max` or `ultra` on Terra, increasing Terra's thinking level remains a reasoning-axis decision; it is not Sol approval. Use it only when deeper processing of known information is the bottleneck and the added resource cost is justified.

If Terra later exposes a separate capability gap requiring Sol, request a new Sol approval. The same rule applies inside Sol: higher thinking levels do not justify keeping Sol for routine stages after the load-bearing problem is resolved.

## Claude Code

Default capability map when the standard Claude families are available:

- economical -> `haiku`
- intermediate -> `sonnet`
- strongest -> `opus`

Claude Code subagents can use family aliases, full model IDs, or inherit the main model. Runtime allowlists may substitute a requested model; treat the actual effective model as authoritative when visible.

Subagent `effort` can be configured independently in subagent definitions, with supported values depending on the model/runtime. Per-invocation model selection may be available even when per-invocation effort selection is not. Therefore:

- use the exact model dynamically when supported;
- use an existing compatible effort-configured subagent/profile when available;
- otherwise use the closest supported effort inherited/configured by the runtime;
- if the required reasoning level cannot be controlled and that difference is material, explain it and fall back to advisory routing for that step.

Claude Code subagents run in their own context windows, which is useful for reconnaissance and adversarial review. This should be invisible to the user as normal orchestration; never ask them to open another chat for ordinary Routewise operation.

### Claude examples

```text
controller: current economical model / medium-equivalent effort
recon: economical subagent
plan: economical subagent / higher effort if controllable
hard bounded decision: request approval -> intermediate subagent
routine implementation: economical subagent
```

## Unknown or future runtimes

When model names, ordering, reasoning controls, or resource accounting change:

1. identify the cheapest practical coding model, a materially stronger middle tier, and the strongest available tier;
2. identify supported reasoning/effort controls separately;
3. inspect organization/provider/data boundaries before selecting an execution path;
4. use observable credits/usage when available instead of invented cost ratios;
5. preserve the Routewise approval sequence across capability tiers;
6. keep the routing core unchanged.