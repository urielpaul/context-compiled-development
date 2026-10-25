# Execution

The Implementing Agent turns a Ready Issue into a complete, verifiable change.

> The implementing agent has broad freedom over HOW, but little freedom to reinterpret WHAT.

## Establish execution context

Start with the compiled Issue, repository `AGENTS.md` when present, relevant code/configuration, and established contracts. Read local operational instructions before modifying the repository. Inspect comparable behavior and available evidence rather than assuming the current system's state.

The Issue supplies the task's necessary global semantics. Load additional sources only for a concrete gap, referenced detail, or contradiction. Do not reconstruct the whole product by default. If necessary meaning is absent, ask for the Issue to be completed or recompiled.

Confirm that the work is Ready under the project's process, its blocking dependencies are satisfied, and no known source change invalidates it. Use available provenance and change notices to identify potentially stale context; [planning defines its revalidation](planning.md#provenance-and-stale-compiled-context). Provenance is not a requirement to reload every source revision.

Sources retain their responsibilities during execution. Existing code and tests can be wrong; the Issue can also be incomplete or stale. Resolve material contradictions through the owning decision process rather than silently choosing one interpretation.

## Exercise implementation autonomy

Choose the simplest implementation that satisfies the behavior, relevant rules, acceptance criteria, and required evidence. Internal names, algorithms, helpers, local organization, and proportionate refactoring normally remain agent decisions.

Observe how equivalent problems are solved in the repository. Reuse suitable conventions and established abstractions when they remain compatible with current constraints. Depart from an existing pattern for a concrete reason, such as a conflict with intended behavior or a simpler local solution. A change to global architecture requires explicit resolution.

Preserve shared contracts and the product's security constraints. Local freedom does not authorize changing intent, business rules, security boundaries, architectural responsibilities, or contracts on which other work depends. Apply the [decision process](planning.md#decisions) when a proposed change exceeds those boundaries.

New dependencies and abstractions need a concrete benefit. A reversible local choice can remain autonomous; a strategic dependency or new infrastructure is a broader decision. Avoid speculative generality, future capabilities, and mechanisms introduced only because they might later be useful.

## Keep scope bounded

Implement the requested outcome and supporting changes needed to deliver it. A small refactor, affected test update, or local repair can belong in the Issue when necessary and compatible with its boundaries.

Keep unrelated cleanup, future features, broad renames, and independent improvements separate. Refactoring remains proportional to the task; crossing shared boundaries or substantially expanding scope requires resolution before proceeding.

When parallel work exists, preserve its stable contracts and avoid unnecessary edits to shared areas. Coordinate required overlap through the project's workflow rather than making unilateral changes that invalidate another Issue's context.

Reference implementations are starting points. Verify their fit with current behavior and constraints, and preserve useful origin information when adapting material. Copying a prior solution does not establish correctness for this product.

## Handle unexpected problems

Classify the problem before choosing a response:

| Situation | Response |
| --- | --- |
| Local, reversible implementation choice | Decide and continue within the Issue |
| Missing or contradictory execution semantics | Request clarification or recompilation of the affected context |
| Necessary change beyond delegated authority | Record the decision, impact, and options; stop the dependent portion |
| Defect blocking the authorized outcome | Make a minimal in-scope repair, or escalate if the repair crosses boundaries |
| Relevant defect or improvement outside this task | Record separate work without expanding the current change |

Before escalating, consult available code, contracts, Issues, and decisions. Explain the specific uncertainty, why it cannot be resolved locally, and which work depends on the answer. Do not reopen a settled decision without material new evidence.

Save safe progress and use the project's blocked-work process while awaiting a necessary decision. Continue only genuinely independent, already authorized work. After resolution, use the updated Issue and assess whether earlier changes remain valid.

## Verify and hand off

The Implementing Agent supplies and runs the evidence required by the Issue and project policy, adding targeted checks when an actual uncovered risk appears. [Verification](verification.md) defines evidence selection, failure handling, and review; passing compilation alone does not establish task completion.

Review the final change for behavior, scope, preserved rules/contracts, and unwanted temporary material. Keep commits coherent. A PR corresponds to the Issue and briefly states the resulting behavior, meaningful local decisions, verification results, and material limitations. Link retained evidence rather than copying complete logs or repeating the Issue.

Keep permanent decisions in their owning sources. Update global documentation only when authorized scope or an explicitly resolved decision requires it; ordinary local choices do not require additional documents.

## Definition of Done

An Issue is Done when:

- the requested behavior is implemented and acceptance criteria are satisfied;
- relevant rules and shared contracts are preserved;
- required evidence exists, is accessible, and required checks pass;
- blocking findings and decisions are resolved;
- the change stays within authorized scope and records material limitations;
- applicable repository integration and review conditions are met.

Independent review and human review apply according to risk and policy. A blanket human sign-off is not part of this definition.
