# Planning

The Planning Agent **compiles context** into small, executable, verifiable Issues. It defines **WHAT** is required and preserves necessary constraints, while leaving unnecessary **HOW** to the Implementing Agent.

## Inputs and planning horizon

Consume relevant portions of:

- PRODUCT, DOMAIN, ARCHITECTURE, and TESTING;
- project PLANNING and the current Sprint;
- repository `AGENTS.md`, established contracts, and actual code/configuration state;
- existing Issues, dependencies, and recorded decisions.

Read these sources according to their responsibility. [Source ownership](documentation.md#source-ownership) is the reference when that distinction needs clarification; reading the entire methodology is not a prerequisite for each planning task.

Plan only authorized scope. Future Sprints can remain descriptions of intent and boundaries. Deeply compile work reasonably close to execution; if an entire MVP is the immediate increment, planning that increment completely is appropriate. Do not turn speculative later stages into an exhaustive backlog.

The planner does not implement features, invent missing business behavior, or silently change a global source. Identify contradictions and missing decisions before making dependent work executable.

## Compile an Issue

For each bounded outcome:

1. Establish the observable result within the Sprint's scope and the current implementation state.
2. Select the relevant rules, constraints, decisions, and shared contracts.
3. Resolve or explicitly expose missing decisions; identify affected dependencies.
4. Define acceptance criteria and the evidence needed to establish them under TESTING.
5. Include enough semantic context to execute, record provenance, and validate readiness.

An Issue can contain the following sections. Omit headings that add no information; the result and its verification still need to be clear.

| Section | Content |
| --- | --- |
| Objective | The outcome to deliver |
| Expected behavior | Observable behavior and meaningful state changes |
| Relevant rules | Necessary product, domain, architecture, security, and contract semantics |
| Acceptance criteria | Specific, verifiable conditions for acceptance |
| Verification | Required evidence, relevant level, and important negative scenarios |
| Out of scope | Nearby behavior that could otherwise be mistaken for part of the task |
| Dependencies | Explicit blocking work, contracts, and sequencing constraints |
| Decisions | Resolved constraints and any blocking questions |

Avoid vague acceptance criteria such as “works correctly” or “handles errors.” State what happens under a defined condition and what remains true afterward. An acceptance criterion describes the outcome; Verification explains how evidence will establish it.

Do not prescribe class names, helper methods, internal patterns, interchangeable libraries, or exact files unless a real constraint makes them necessary. Stabilizing an externally shared contract can require precise technical detail; designing every consumer's internals does not.

### Self-contained means semantically sufficient

**If understanding a global rule is necessary to execute an Issue, include the semantic content of that rule, not only its ID.** This applies equally to product behavior, domain invariants, architecture boundaries, security constraints, and evidence requirements.

Insufficient execution context:

```text
Relevant rules:
- DOC-001
- DOC-002
```

Sufficient rule context:

```text
DOC-001 [REQUIRED]
Document.public_visible iff Document.status == Published

DOC-002 [FORBIDDEN]
Archiving a Document deletes its stored content or descriptive metadata.
```

Define terms, exceptions, and related contract details when needed to interpret those rules correctly. A link can provide provenance or optional background; it does not substitute for necessary meaning. Do not send the agent to several global documents to reconstruct a local objective.

Self-contained does not mean exhaustive. Exclude irrelevant rules, entire documents, conversation transcripts, and discarded alternatives. Preserve stable IDs and source revisions for traceability while compiling only the knowledge the task actually needs.

### Compact example

The following is an illustrative Issue compiled from the document-library example:

```text
Objective
Allow an editor to archive a published document without losing its data.

Expected behavior
A published document can become Archived. It leaves public discovery
and direct public access, remains editable in the editorial workspace,
and can later be published again.

Relevant rules
DOC-001 [REQUIRED]
Document.public_visible iff Document.status == Published

DOC-002 [FORBIDDEN]
Archiving a Document deletes its stored content or descriptive metadata.
Descriptive metadata comprises the title, summary, and labels;
lifecycle status is separate.

Acceptance criteria
- Archiving a Published document changes its status to Archived.
- Public discovery and direct public access no longer expose it.
- Its content, title, summary, and labels remain unchanged and available to the editor.
- Publishing it again restores public access with the same content.

Verification
- A focused transition test verifies the new status and preserved data.
- A scenario through the real public access boundary verifies that both
  discovery and direct access exclude the archived document.
- The same scenario verifies access after publication is restored.
Use the project's existing test levels and isolated fixtures.

Out of scope
Permanent deletion, revision history, and notifications.

Dependencies
Existing document publication and public access capabilities.

Decisions
no-decision: no unresolved human/global decision blocks this behavior.

Compiled from (illustrative references)
product-docs@4fa812c
methodology@v0.1.0-alpha.1
```

The example assumes the product has authorized this behavior. A planner cannot introduce editability, republication, or public-access rules merely because they seem convenient.

## Granularity and repository boundaries

Prefer a functional or behavioral slice that produces a coherent, verifiable result. Avoid splitting one capability into arbitrary entity, storage, service, and interface tasks when those changes belong together. A slice does not need to touch every technical layer.

Split an Issue when it combines independent outcomes, unrelated rule sets, separately blocking decisions, or an unreviewable change. Combine fragments when they have no independently verifiable result or require most of another Issue's context. There is no universal file-count or line-count limit.

Place work in the repository where it will primarily execute. A Sprint can coordinate several repositories through linked Issues and stable contracts. Separate genuinely independent repository changes instead of creating a large cross-repository Issue; preserve the shared functional outcome at Sprint level.

Previous products and implementations can inform planning, but do not become authoritative automatically. Record a useful reference and evaluate it against the current product's rules and architecture before proposing reuse.

## Decisions

Use exactly one decision label per Issue:

| Label | Meaning |
| --- | --- |
| `no-decision` | No human/global decision is needed before execution; remaining choices are within agent autonomy |
| `requires-decision` | At least one necessary decision exceeds the agent's authority and remains unresolved |
| `decision-resolved` | Previously pending decisions have been explicitly resolved and incorporated into the context |

These labels classify decision readiness, not difficulty, implementation progress, or approval. A large implementation can be `no-decision`; a three-line change can be `requires-decision`. `decision-resolved` does not by itself make an Issue Ready.

Local, reversible implementation choices remain with the Implementing Agent. First consult existing decisions and available context before escalating. Do not silently resolve changes affecting product, domain, architecture, security, shared contracts, scope, or important commercial behavior.

For a decision beyond agent authority:

1. Record the concrete question, why it matters, affected behavior/work, and useful alternatives with consequences. A recommendation can help but does not authorize its adoption.
2. Apply `requires-decision` and keep dependent work outside Ready. Identify direct and transitive dependents. Isolate a genuinely independent portion only if its scope and contract are already clear.
3. Obtain resolution from the authorized decision maker. Human intervention is appropriate for the cases described below; do not assume an answer because waiting is inconvenient.
4. Record the resolution and its reference. Update the owning global source for a lasting decision, then recompile affected pending or active Issues.
5. Replace the label with `decision-resolved` when all required decisions are resolved. Revalidate dependencies and readiness; an ambiguous or partial answer leaves dependent work blocked.

Centralize a shared decision and link its consumers rather than requesting the same decision repeatedly. Preserve known decisions unless new evidence reveals a material conflict, risk, or impossibility. Review available answers when selecting or resuming work; this process does not presume continuous comment monitoring.

### Human intervention

Human intervention is required primarily for:

- unresolved high-impact decisions outside delegated agent authority;
- subjective product judgment that cannot be established by existing criteria;
- high-risk or security-sensitive changes when policy requires human involvement;
- global architectural, product, or domain changes;
- uncertainty that agents cannot resolve safely within their authority.

This does not require a person to manually inspect every Issue or PR. Authorized scope can be validated by another agent or automated checks. Existing explicit authorization can satisfy scope authorization without requesting it again. Product policy can add targeted human gates where justified.

## Dependencies and parallelization

Identify what blocks execution, what can proceed after a contract is settled, and what is merely a useful ordering preference. Use precise Issue references, including repository identity, or native dependency links where available. “After the other component” is not a sufficient dependency description.

Stabilize shared inputs, outputs, semantics, and failure behavior before parallel consumers rely on them. A stable contract does not require the provider's complete implementation, but its consumers need a reliable boundary and a plan to verify eventual integration.

Evaluate likely file overlap, shared state, coupled components, unresolved decisions, and integration order. Mark parallel work only where these constraints allow it. Conceptually different features can still interfere; independent files can still depend on an unsettled contract.

The objective is less coordination and rework, not the largest possible number of simultaneous Issues.

## Scope validation and Ready

An Issue is Ready when an Implementing Agent can begin within the agreed boundaries without a new global decision. Validate that:

- the outcome belongs to authorized near-term scope and is bounded;
- necessary rule semantics and established decisions are present;
- acceptance criteria and required evidence are specific;
- blocking dependencies are satisfied and needed contracts are stable;
- provenance is recorded and known source changes have been assessed;
- no unresolved decision blocks the work.

An agent or automated validation can perform this review, escalating only the parts that need human judgment. Readiness leaves ordinary implementation decisions open.

A supported flow is:

```text
Planning
    ↓
Agent/automated scope validation
    ↓
Ready
    ↓
Implementation
    ↓
Automated verification
    ↓
Independent agent review when useful
    ↓
Human review when valuable or required
```

Review stages are conditional on risk, evidence, and product policy. This is not a mandatory sequence of human approvals. Assign work when execution is being taken up, rather than preassigning an entire planned backlog. Boards and operational states remain project-local.

## Provenance and stale compiled context

An Issue is conceptually a compiled artifact: it was produced from particular source revisions and a methodology version. Record a small provenance section, for example:

```text
Compiled from:
product-docs@4fa812c
methodology@v0.1.0-alpha.1
```

Use resolvable references in real work: an identifiable repository with a commit, tag, or equivalent immutable revision. When inputs span repositories, record each relevant source revision, including an important contract or code baseline when it shaped the compilation. The example's short names assume the product identifies the repositories unambiguously.

Provenance explains what knowledge produced the Issue, supports reproducible planning, and helps assess whether work has become stale. It does **not** ask the Implementing Agent to reread those original sources. Products [declare their adopted methodology version](documentation.md#methodology-version-adoption); an Issue records the version actually used to compile it.

When PRODUCT, DOMAIN, ARCHITECTURE, TESTING, project PLANNING, a Sprint, or another relevant source changes:

1. Update the authoritative source and identify the semantic change and affected rules/contracts.
2. Review affected pending and active Issues. A changed source revision signals possible staleness; unrelated changes do not require recompiling everything.
3. Recompile or update the affected semantics, acceptance criteria, evidence, decisions, dependencies, and provenance as needed. Do not merely replace the revision reference while leaving outdated meaning.
4. For active work, notify the responsible agent and assess the existing implementation. Suspend the affected portion if its assumptions are no longer valid; revalidate readiness before resuming. Do not silently replace acceptance criteria during execution.
5. Keep completed Issues as historical records. If the change requires corrective behavior, create or link corrective work rather than retrospectively rewriting every completed Issue.

Maintain any significant change explanation and decision reference compactly. Reproduction requires the recorded sources and relevant decisions, not an identical generated wording. This preliminary version defines the concept only; it provides no stale-detection tooling or automation.
