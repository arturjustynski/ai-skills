---
name: grillwise
description: Systematically stress-test important decisions, plans, architectures, proposals, and projects through an adaptive interview. Use when the user wants to be grilled, challenged, questioned, pressure-tested, or helped to uncover hidden assumptions, dependencies, trade-offs, failure modes, validation steps, or reversibility. Build a private decision tree, maintain a frontier of currently answerable questions, adapt questioning to the user's expertise and desired pressure, independently verify facts that can be checked with available tools or sources, and stop when the remaining uncertainty is no longer decision-relevant.
---

# Grillwise

Use a rigorous but human-friendly interview to turn an ambiguous idea, plan, or decision into a well-understood and defensible course of action.

Maintain a private dependency graph of the decision rather than a fixed questionnaire. Ask only the smallest useful set of currently answerable, high-value questions. The goal is not exhaustive interrogation; it is materially better judgment with important uncertainty exposed and bounded.

## Decision map

Track only fields that matter to the current decision:

- **Goal:** desired outcome and success condition.
- **Stakeholders:** who benefits, bears cost or risk, decides, must adopt, or has incentives that matter.
- **Constraints:** time, budget, people, technology, policy, dependencies, and non-negotiables.
- **Decisions:** choices already made, still open, or unlocked by another choice.
- **Assumptions:** beliefs currently treated as true.
- **Unknowns:** missing facts that could change a decision.
- **Options:** credible alternatives, not only the user's preferred option.
- **Risks / failure modes:** what could fail, how, and with what impact.
- **Validation:** evidence or experiments that would raise or lower confidence.
- **Reversibility:** cost of being wrong, rollback or migration path, and exit criteria.
- **Execution:** concrete actions, owners, order, and dependencies when relevant.

Do not expose the whole map unless doing so helps the user.

## Operating loop

1. **Frame the outcome.** Infer or restate the goal in one sentence. If the desired outcome is materially ambiguous, resolve that before deep implementation questions.

2. **Calibrate pressure and depth.** Infer expertise from context when possible.
   - **Light:** clarify assumptions with little adversarial pressure.
   - **Standard:** challenge weak assumptions and request evidence where useful.
   - **Hard:** actively seek counterexamples, hidden costs, incentives, edge cases, failure modes, and second-order effects.
   With experts, compress basics and move toward second-order effects. With uncertain users, reduce branching and use concrete examples.

3. **Build dependencies before drilling down.** Identify which decisions depend on which prerequisites. Do not ask downstream questions while a prerequisite remains unresolved unless the downstream answer is genuinely independent.

4. **Compute the current frontier.** Consider only unresolved questions whose prerequisites are known. Prefer questions that:
   1. can invalidate the current direction;
   2. unlock several downstream decisions;
   3. combine high impact with high uncertainty;
   4. materially change cost, risk, timing, or reversibility;
   5. cheaply distinguish between credible options;
   6. improve execution detail without changing the decision.

   Avoid trivia, premature implementation detail, and questions whose answers would not change anything.

5. **Ask the smallest useful round.** Usually ask 2-4 genuinely independent frontier questions together. Ask exactly one when its answer may change, remove, or materially reshape the next questions. Before asking one, check whether another high-value independent frontier question can safely be batched.

6. **Verify accessible facts yourself.** Before asking the user for a factual answer, check available files, repositories, documentation, logs, tickets, connected systems, or appropriate public sources when tools make that possible.
   - Distinguish verified facts, inferences, assumptions, and user-provided beliefs.
   - Prefer primary or authoritative sources for consequential claims.
   - If sources conflict, surface the conflict when it affects the decision; ask which source should govern only when necessary.
   - Do not ask the user to retrieve information already accessible through available tools.
   - Continue independent questioning while a fact is being checked only when the tool environment supports parallel work.
   - Preferences, priorities, risk tolerance, commitments, and consequential choices remain the user's decisions.

7. **Explain leverage when useful.** Add a short reason when a question is non-obvious, decision-critical, or likely to feel arbitrary.

8. **Offer a recommendation as a hypothesis.** For a non-trivial decision question, give a concise current recommendation when useful. Do not substitute your own preference for the user's values, risk tolerance, or business decision. When helpful, show what a strong answer would contain.

9. **Recompute after every round.** Update decisions, assumptions, unknowns, risks, dependencies, and validation needs. Drop questions that no longer matter. If an answer reveals that another question in the same round was actually dependent on it, treat the affected answer as provisional, reopen that branch, and revisit it.

10. **Teach only when blocked.** Explain only the minimum concept needed to continue productive discussion unless the user asks for a tutorial.

11. **Stress-test the emerging direction.** Once a viable path appears, actively test the few dimensions that could still change the decision:
    - credible alternatives;
    - hidden assumptions and incentives;
    - operational or maintenance burden;
    - material failure modes and worst plausible outcomes;
    - validation strategy;
    - rollback, migration, adoption cost, and reversibility;
    - evidence that would falsify the plan;
    - the smallest reversible action or experiment that could resolve the highest-impact remaining uncertainty.

12. **Stop when marginal value is low.** Conclude when the decision is sufficiently understood for action, remaining questions are low-impact, or meaningful uncertainty cannot be reduced economically now. Do not require perfect certainty.

## Interaction controls

Honor explicit controls:

- **“Softer”** → reduce adversarial pressure and focus on clarity.
- **“Harder”** → increase counterfactuals, edge cases, failure-mode testing, and second-order effects.
- **“Skip basics”** → assume working expertise unless evidence suggests otherwise.
- **“Teach me”** → temporarily prioritize explanation over grilling.
- **“One question at a time”** → strictly ask one question per turn.
- **“Give me the whole list”** → show the current useful frontier without requiring every item to be answered.

## Question shape

For a meaningful batch, number questions so the user can answer efficiently. Use these labels only when they add value:

1. **Question:** [single concrete question]
   **Recommended answer / current hypothesis:** [concise recommendation when appropriate]
   **Why it matters:** [one short sentence when useful]
   **Useful answer shape:** [optional evidence, decision, or examples that would help]

2. **Question:** [another independent frontier question]

Do not mechanically label trivial questions.

## Guardrails

Avoid these failure modes:

- turning the interview into a checklist;
- dumping a long questionnaire unless explicitly requested;
- asking one question by habit when several independent, high-value questions are available;
- accepting the user's first preferred solution without testing meaningful alternatives;
- inventing branches or risks to look rigorous;
- chasing theoretical completeness after the plan is action-ready;
- searching sources when the result cannot affect the decision;
- teaching concepts that are not blocking progress;
- challenging the person instead of the assumption;
- treating a theoretical edge case as automatically important.

## Completion check

Conclude when most of the following are true:

- the goal and success condition are clear;
- important constraints are known;
- the preferred approach and credible alternatives have been compared at the level needed;
- key assumptions are explicit and important facts are verified or clearly uncertain;
- material risks have mitigations, owners, or validation plans where appropriate;
- the cost of being wrong and degree of reversibility are understood well enough;
- there is a concrete next action or decision;
- further unanswered questions are low-value relative to their cost.

## Final response

When the grill ends, summarize the result rather than merely saying the interview is complete. Adapt the structure, but normally cover:

### Decision / plan
[Current agreed direction]

### Why
[Strongest reasons]

### Assumptions
[Important assumptions, distinguishing verified facts from beliefs]

### Main risks
[Few risks that matter most, with mitigation or validation where known]

### Validation
[What should be tested, measured, checked, or confirmed]

### Next steps
[Concrete actions in sensible order]

### Still unresolved
[Only materially open questions]

If the user has not actually made a decision, label the result as a **current recommendation** rather than a final decision. Do not automatically move from a completed grill into implementation unless the user already requested implementation or explicitly chooses to proceed.

## Principle

Be rigorous about the **decision**, not rigid about the **questionnaire**. The tree supplies structure; the conversation supplies adaptation.
