# Context-Compiled Development

**Experimental and evolving · First preliminary public version: `v0.1.0-alpha.1`**

Context-Compiled Development is a methodology for turning product intent into verifiable software through autonomous agents.

> Humans define what must be true. Agents decide how to implement it.

Agents often spend substantial effort reconstructing product context, rediscovering decisions, or asking people to resolve ambiguity. Giving every agent every document adds noise and repeated work. This methodology separates enduring decisions, task preparation, implementation, and verification so each agent receives the minimum sufficient context for its responsibility.

The primary optimization target is **human intervention time per delivered functionality with acceptable quality**. Quality is established through explicit acceptance criteria and verification evidence.

## How it works

```text
Desired state
+ constraints
+ required evidence
        ↓
context compilation
        ↓
self-contained execution context
        ↓
autonomous implementation
        ↓
verification
```

Humans establish product intent, important constraints, and decisions that exceed agent authority. A Planning Agent selects the relevant knowledge and compiles small, executable Issues. An Implementing Agent chooses the implementation within those boundaries. Verification evaluates the resulting behavior against the requirements.

An Issue is the execution unit: it carries the objective, necessary rule semantics, acceptance criteria, and required evidence. A list of rule IDs or links to whole documents is insufficient when the agent needs their meaning to execute the task. Provenance records which source revisions and methodology version produced the Issue; relevant changes can make that compiled context stale.

Scope validation and verification can be performed by agents and automated checks. Independent agent review adds confidence where useful. Human review is used when valuable or required, including unresolved high-impact decisions and subjective product judgment; it is not a gate on every Issue or PR.

## Sources and boundaries

Each product keeps its own `PRODUCT.md`, `DOMAIN.md`, `ARCHITECTURE.md`, `TESTING.md`, project `PLANNING.md`, and `sprints/`. These sources own different kinds of decisions rather than forming a simple authority hierarchy. Local `AGENTS.md` files provide repository operations; code and configuration express concrete implementation details. See the [source ownership model](docs/documentation.md#source-ownership).

Context compilation transfers the relevant semantics into an Issue without making the Issue a competing global source of truth. The Implementing Agent has broad autonomy over local, reversible implementation choices and escalates decisions outside its boundaries. Future Sprints can remain high-level; detailed Issues are prepared close to execution, as described in [planning](docs/planning.md).

This model applies to SaaS applications, games, CLIs, libraries, desktop applications, infrastructure projects, and other software systems. Products choose their own architectures and evidence policies.

## Repository and selective reading

```text
.
├── README.md
└── docs/
    ├── principles.md
    ├── documentation.md
    ├── planning.md
    ├── execution.md
    └── verification.md
```

| Document | Responsibility |
| --- | --- |
| [Principles](docs/principles.md) | Foundational commitments, optimization, and evolution |
| [Documentation](docs/documentation.md) | Source ownership, notation, and methodology adoption |
| [Planning](docs/planning.md) | Issue compilation, decisions, readiness, dependencies, and stale context |
| [Execution](docs/execution.md) | Implementing Agent autonomy, scope, escalation, and completion |
| [Verification](docs/verification.md) | Evidence selection, independent review, and learning from failures |

Read by role, then expand only for a concrete need:

| Role | Normal working context |
| --- | --- |
| Planning Agent | `docs/planning.md`, relevant product documentation, current Sprint, relevant repository/code state and existing decisions |
| Implementing Agent | Compiled Issue, repository `AGENTS.md`, relevant code and established contracts |
| Reviewer Agent | `docs/verification.md`, Issue, diff, relevant rules/context and supporting evidence |

README is a human entry point, not mandatory agent execution context. `principles.md` and `documentation.md` are normative references for humans, methodology maintainers, and agents that need those concepts; they are not automatically loaded for every task. `execution.md` defines the execution behavior carried into Issues and local instructions, and is available when that guidance is needed.

This publication contains documentation only. Validate the methodology through actual use and evolve it from observed failures and interventions before considering specialized tooling.
