# Runtime adapters

Use this reference only when Routewise needs to translate abstract capability/reasoning requirements into an executable runtime route.

## General adapter rules

1. Prefer runtime introspection over stale assumptions when model availability or allowed reasoning levels can be inspected reliably.
2. Respect organization allowlists, managed settings, provider substitutions, permissions, and user ceilings.
3. Never claim a worker used a requested model if the runtime substituted another model; use the actual model when observable.
4. If exact model or reasoning control is unavailable, fall back to advisory mode for that dimension rather than pretending it is controllable.
5. Context-isolated workers are optional. They are especially useful for reconnaissance or adversarial review, but must never require manual user context transfer.

## Codex / GPT-5.6

Default capability map when GPT-5.6 family models are available:

- economical -> `gpt-5.6-luna`
- intermediate -> `gpt-5.6-terra`
- strongest -> `gpt-5.6-sol`

GPT-5.6 exposes configurable reasoning in the API, but a particular Codex surface may expose only a subset. Always use the real level supported by that surface.

Current Codex multi-agent configuration supports a default spawned-agent model and reasoning effort, with explicit spawn settings taking precedence. Multi-agent collaboration may expose spawn/send/resume/wait/close primitives. When those primitives are available, keep the primary thread as controller and spawn bounded workers with the selected model/reasoning configuration.

Do not bypass Routewise approval gates merely because Codex can technically spawn a stronger model.

### Codex examples

A difficult planning task with no capability evidence beyond reasoning depth:

```text
controller: gpt-5.6-luna / medium
planning worker: gpt-5.6-luna / highest useful supported reasoning
implementation: gpt-5.6-luna / medium
```

A confirmed capability gap in a bounded architectural decision:

```text
controller: gpt-5.6-luna / medium
approval requested: gpt-5.6-terra / high
Terra scope: resolve the load-bearing architectural decision only
remaining implementation: gpt-5.6-luna / medium when sufficient
```

If Terra later exposes a separate capability gap requiring Sol, request a new Sol approval.

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

When model names or ordering change:

1. identify the cheapest practical coding model, a materially stronger middle tier, and the strongest available tier;
2. identify supported reasoning/effort controls separately;
3. preserve the Routewise approval sequence across capability tiers;
4. do not infer exact price ratios unless reliable current rates are available;
5. keep the routing core unchanged.
