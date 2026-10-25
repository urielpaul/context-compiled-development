# Verification

Verification establishes whether the delivered behavior satisfies the declared requirements and constraints. **Implementation is not proof of correctness.** Evidence is useful only when it can establish the property being claimed.

```text
Rule / requirement
        ↓
Acceptance criteria
        ↓
Required evidence
        ↓
Verification
```

The Issue specifies task-level evidence; product TESTING defines the applicable policy and available verification levels. Use the actual risk of the change to identify important gaps. Product-specific technologies, environments, and mandatory scenarios belong in TESTING or the compiled Issue rather than this generic methodology.

## Select sufficient evidence

Use the **smallest verification level that can actually prove the property**. A larger test is not automatically better, and a smaller test is insufficient if it cannot represent the relevant behavior.

| Property | Possible evidence |
| --- | --- |
| Pure state transition or calculation | Focused behavioral test |
| Shared boundary or external interaction | Contract or integration check representing that boundary faithfully |
| Observable user interaction | Component or system scenario appropriate to the behavior |
| Runtime characteristic | Measurement under representative conditions |
| Subjective experience or product judgment | Reproducible human evaluation when needed |
| Documentation change | Consistency, meaning, and reference review |

These are options, not a required stack or a list of levels every product needs. A test double can isolate a useful property but cannot prove behavior that it replaces with an assumption. Use the actual technology or a sufficiently faithful environment when the property depends on it.

Scale evidence with impact, likelihood of failure, reversibility, and difficulty of detecting a defect. A low-impact local change rarely needs the same verification as a change to a critical shared boundary. Preserve existing protections when they remain valid; do not generate redundant tests at every level.

For each acceptance criterion, identify automated evidence, a reproducible manual check, or a justified reason that a proposed check is inapplicable. Unverified criteria are gaps rather than implicit successes. Documentation edits do not need artificial implementation tests.

## Test the actual requirement

Assertions should establish behavior rather than reproduce the implementation's internal steps. A successful return value does not prove that all required state changed, that protected data was preserved, or that forbidden behavior was rejected.

For the illustrative document-library rule:

```text
DOC-001 [REQUIRED]
Document.public_visible iff Document.status == Published
```

Evidence covers both sides of the condition: Published documents are accessible publicly; Draft and Archived documents are not. If public access has discovery and direct-access paths, check the relevant paths rather than assuming one proves the other.

Test negative behavior when relevant: invalid states, prohibited transitions, rejected operations, disallowed access, and meaningful failure conditions. When a requirement promises preservation, compare the relevant state before and after the operation. A successful happy path cannot establish the absence of a forbidden outcome.

For a reproducible bug, add a meaningful regression check that fails before the correction and passes afterward when feasible. The check should capture the violated requirement, not incidental details of the repair.

Coverage and test count are supporting signals. High coverage can coexist with incorrect assumptions, missing negative scenarios, or assertions about the wrong property. Choose any thresholds from the product's observed risks and representative suite rather than copying arbitrary targets.

## Make verification reproducible

Identify the revision and conditions under which evidence was produced. Control relevant inputs, time, randomness, ordering, and external state. Tests create the fixtures they need and isolate mutable state according to project policy.

Manual evidence describes setup, actions, expected result, and observed result clearly enough for another person or agent to repeat it. “Tried it and it works” does not identify a verifiable claim.

Keep verification practical: use focused checks during development and complete the checks required for acceptance. Each level should contribute evidence rather than repeat equivalent scenarios without benefit. Record material environmental limitations instead of presenting partial verification as complete.

## Independent review

Tests written by the Implementing Agent are valuable, but can share the same mistaken interpretation as the implementation. Independent review is useful when another perspective can challenge that interpretation, inspect critical boundaries, or assess evidence quality. Do not systematically duplicate entire suites or require another reviewer for every low-risk change.

A Reviewer Agent normally loads this document, the Issue, the diff, verification evidence, and relevant rules/context. Inspect additional code or global sources when a concrete ambiguity or risk requires it. A global reconstruction of the product is not the default review task.

Evaluate whether:

- the behavior and scope match the Issue;
- relevant rule semantics, constraints, and shared contracts remain valid;
- acceptance criteria have evidence that actually establishes them;
- important negative scenarios and plausible regressions are addressed;
- no unauthorized architectural, security, or global behavior change is hidden in the implementation.

Challenge assumptions, not just syntax. Where necessary, derive an independent scenario from the requirement or compare the compiled context with its owning source. If the Issue itself is deficient or stale, return that problem to planning rather than accepting an implementation of the wrong requirement.

Classify findings by impact. A violation of behavior, acceptance, a required boundary, or evidence policy can block acceptance. An optional improvement or personal style preference does not become a new requirement. Reviewers preserve agent implementation freedom and do not expand scope or invent constraints.

## Human review

Use human review for valuable judgment or a policy requirement, following the [human-intervention boundaries](planning.md#human-intervention). Subjective product quality, high-impact unresolved decisions, and changes requiring accountable human judgment are appropriate reasons.

Evidence should let people assess the relevant behavior or decision without mentally reconstructing every line of implementation. Repeated manual checking of an automatable, well-defined property is a signal to improve the evidence process. Human review is not a universal prerequisite for acceptance.

## CI and failures

CI executes the checks agreed by the project. Required failures remain visible and block acceptance under that policy until resolved or handled through an explicitly authorized, recorded exception. An exception records its reason, impact, and condition for restoring the check; it is not a passing result.

Investigate whether a failure is a regression, an incorrect or outdated expectation, an environmental problem, or a pre-existing defect. Change a test when its expectation is demonstrably wrong or an authorized requirement changed, not merely to obtain a green result.

Do not hide failures by deleting tests, weakening assertions, arbitrary exclusions, unexamined retries, or CI-specific bypasses. Flakiness is a defect in the evidence process: diagnose it and use retries only for a known, documented reason. Passing on a retry does not explain a previous failure.

Report what ran, what it established, the result, and any limitation. Retain useful reports or references without pasting complete logs into every Issue or PR. Review and evidence outcomes feed the [Definition of Done](execution.md#definition-of-done).

## Learn from failures

When a bug escapes or an unexpected intervention is needed, locate the failure:

| Origin | Question |
| --- | --- |
| Specification | Was the required behavior missing, ambiguous, or contradictory? |
| Context compilation | Was necessary meaning omitted, distorted, or stale in the Issue? |
| Implementation | Did the change violate an adequately specified requirement? |
| Verification | Was evidence missing, unrepresentative, or aimed at the wrong property? |

Several causes can apply. Correct the owning source or process, propagate affected context through planning, and add targeted regression protection where useful. Another test is not always the right or sufficient response.

Use escaped defects, rework, reviewer findings, flakiness, check duration, and human review effort as supporting observations. The [primary optimization metric](principles.md#optimize-human-intervention-time) remains human intervention time per delivered functionality with acceptable quality.
