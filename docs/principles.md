# Principles

These commitments govern the methodology. Operational details belong in [planning](planning.md), [execution](execution.md), and [verification](verification.md); source ownership belongs in [documentation](documentation.md).

## Declare before implementing

State the desired behavior, invariants, constraints, contracts, and evidence before starting the affected implementation. Preserve human authority over intent and high-impact decisions while giving agents responsibility for implementation.

Describe implementation details only when they express a necessary constraint or a stable architectural decision. Internal names, helpers, and reversible local designs usually belong in code.

## Keep global sources of truth

Give each enduring decision one authoritative home. Sources have authority by responsibility, not by their position in a document hierarchy. Resolve a contradiction in the source that owns the decision and propagate its consequences to affected work.

Conversations and completed Issues can preserve rationale, but they do not replace an updated global source when a lasting decision changes.

## Compile context

Planning selects and transforms relevant global knowledge into a task-specific execution context. The goal is **maximum precision with minimum sufficient context**.

Sufficiency includes the meaning of every rule needed for execution. Traceability references help identify a rule but cannot substitute for that meaning. Compiled context is derived, versioned knowledge and can become stale when its sources change.

## Use the Issue as the execution unit

An Issue expresses one bounded outcome that can be implemented and verified coherently. It gives the agent enough context to proceed without reconstructing the entire product.

Prefer functional or behavioral slices where reasonable. Issue count, file count, and lines of code are poor measures of an appropriate execution boundary.

## Grant autonomy inside explicit boundaries

Agents resolve local, reversible implementation decisions. Changes to the assigned intent or shared constraints require explicit resolution through the [decision process](planning.md#decisions).

Avoid both silent assumptions about important behavior and escalation of ordinary implementation choices. Readiness and acceptance can be established without routine human inspection of every task, subject to the product's policies.

## Specify progressively

Keep enduring product knowledge clear while allowing future increments to remain broad. Compile detailed execution work when it is reasonably close to being built.

Learning changes plans. Early exhaustive backlogs create speculative decisions and stale context rather than reliable autonomy.

## Parallelize through stable boundaries

Parallel work depends on stable contracts, understood dependencies, and manageable overlap. Conceptual independence alone does not establish execution independence.

Optimize the division of work to reduce interference, coordination, and rework. Increasing the number of active agents is useful only when it improves delivery under those conditions.

## Seek independent confidence

Implementation is not proof of correctness. Verification connects requirements to evidence and challenges the assumptions behind the implementation.

Tests written by the Implementing Agent are useful but may share its interpretation errors. Independent checks or review add value when they examine those assumptions; they need not duplicate every check or introduce a human gate on every delivery.

## Keep documentation simple

Documentation earns its maintenance cost by preserving stable knowledge, reducing ambiguity, or preventing repeated intervention. Use the smallest document set and the least detail that accomplish those purposes.

Prefer executable sources for concrete implementation facts. Add a document, template, abstraction, tool, or process only for an observed need. The preliminary methodology remains documentation-only.

## Evolve from observed failures and interventions

Use unexpected human intervention, recurring ambiguity, agent conflicts, rework, and escaped defects to identify weaknesses. Determine whether the problem came from specification, context compilation, boundaries, implementation, or verification.

Choose the smallest corrective change that prevents recurrence. An intervention does not automatically justify another rule, document, or review step.

## Optimize human intervention time

The primary metric is **human intervention time per delivered functionality with acceptable quality**. Establish the quality threshold through acceptance criteria and evidence; reducing review effort while increasing escaped defects does not satisfy the objective.

Human time includes necessary clarification, decisions, review, and correction. When measuring, record actual contributions once, distinguish human time from agent time, and avoid double counting across Issues or contributors. Pause active-work timers while waiting; do not infer human minutes from agent execution time.

Use rework, escaped defects, autonomous completion, agent cost, and the reasons for escalation as supporting signals. Compare changes in the process with comparable work and quality expectations. These measures improve the methodology rather than rank individual productivity or reward Issue throughput.
