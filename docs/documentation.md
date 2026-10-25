# Documentation

Product documentation preserves decisions that agents need to plan and verify work. It stays separate from this generic methodology and avoids duplicating details expressed more precisely by executable sources.

## Source ownership

| Source | Owns |
| --- | --- |
| PRODUCT | Intent, actors, capabilities, scope, and product behavior |
| DOMAIN | Entities, relations, states, transitions, and invariants |
| ARCHITECTURE | Stable technical boundaries and architectural decisions |
| TESTING | Evidence required to trust changes |
| Project PLANNING | Project-specific planning rules |
| SPRINT | The current increment |
| ISSUE | Compiled execution context for a bounded task |
| AGENTS | Repository-local operational instructions |
| CODE / CONFIG | Concrete executable implementation details |

These sources are not a simple authority hierarchy. Each owns a different type of decision. An Issue can select behavior within the product's scope, but cannot silently redefine a domain invariant. Existing code shows current behavior without automatically establishing that the behavior is intended.

Keep one authoritative definition of each rule. Other global sources can refer to it. Including its semantics in a compiled Issue is intentional derivation for execution, not a second authoritative definition.

When sources materially conflict, identify the kind of decision involved and resolve it in its owning source. Do not select whichever document is more local, more recent, or more convenient without establishing that its change was authorized. Cross-cutting changes can require coordinated updates to several owners.

## Product document set

A typical product maintains:

```text
product-docs/
├── PRODUCT.md
├── DOMAIN.md
├── ARCHITECTURE.md
├── TESTING.md
├── PLANNING.md
└── sprints/
    ├── 001-initial-increment.md
    └── 002-next-increment.md
```

Global knowledge has one maintained home independent of individual technical tasks. That home can serve multiple technical repositories; the methodology prescribes neither a monorepo nor a particular repository topology. Create repository-specific architectural documentation only when distinct, stable responsibilities justify it.

### PRODUCT.md

Use a hybrid format: short prose explains purpose and value; structured notation describes actors, surfaces, capabilities, exclusions, and strong product rules.

For example, a document library helps editors maintain material that readers can discover without seeing unfinished work:

```text
PRODUCT DocumentLibrary

ACTOR Editor
ACTOR Reader

SURFACE EditorialWorkspace
CAPABILITY MaintainDocuments
- Editor creates, edits, publishes, and archives documents.

SURFACE PublicLibrary
CAPABILITY DiscoverDocuments
- Reader searches and reads published documents.

RULE UX-001 [FORBIDDEN]
An unsuccessful edit is presented as saved.

EXCLUDED
- paid subscriptions
- collaborative editing
```

Describe the problem, intended experience, included capabilities, and meaningful exclusions. Keep domain invariants in DOMAIN and technical mechanisms in ARCHITECTURE. PRODUCT does not need endpoints, storage structures, internal class names, or a copy of the domain model.

### DOMAIN.md

Use a more strongly declarative format to describe valid states and behavior independently of implementation. Declare conceptual entities, meaningful relations and cardinalities, states, significant transitions, and invariants. Model domain concepts rather than every internal code type.

```text
ENTITY Collection
ENTITY Document

RELATION
Collection -> Document [0..N]
Document -> Collection [exactly 1]

ENUM DocumentStatus
- Draft
- Published
- Archived

STATE Archived
Retained for later use and hidden from public readers.

TRANSITION
Draft -> Published [allowed]
Published -> Archived [allowed]
Archived -> Published [allowed]

RULE DOC-001 [REQUIRED]
Document.public_visible iff Document.status == Published

RULE DOC-002 [FORBIDDEN]
Archiving a Document deletes its stored content or descriptive metadata.
```

In this example, descriptive metadata comprises the title, summary, and labels; lifecycle status is separate.

Give rules stable IDs. Use one main condition per rule and canonical names for concepts and states. A rule ID continues to identify the same concern as its wording evolves; use a new ID for a different concern and preserve any useful replacement reference. IDs alone do not identify a rule's revision.

Keep implementation mechanisms out of the domain model. A rule can strongly constrain an implementation without prescribing a framework, transport, storage engine, or internal directory structure.

### Declarative notation

The examples are **structured pseudocode inside Markdown**, intentionally not a formal DSL. Constructs such as `PRODUCT`, `ACTOR`, `SURFACE`, `CAPABILITY`, `ENTITY`, `RELATION`, `ENUM`, `STATE`, `TRANSITION`, `RULE`, and `EXCLUDED` are writing conventions, not a closed grammar.

Use these explicit categories for strong rules:

- `[REQUIRED]`: a condition that is required to hold.
- `[FORBIDDEN]`: an invalid state or behavior, stated directly.

For instance, “Archiving a Document deletes its stored content” directly names the invalid behavior. Avoid placing a permissive sentence under `[FORBIDDEN]` and expecting the reader to negate it mentally.

Use natural language whenever it is clearer than a symbolic expression. Be explicit about ambiguous terms such as visibility, preservation, or completion. The notation exists to compress meaning, not to make writing resemble code.

Traceability connects:

```text
Rule → Sprint → Issue → Implementation → Verification
```

No parser, compiler, schema, custom extension, or formal grammar is required or provided. Consider formalization only after repeated use across multiple products naturally reveals a stable language and a concrete benefit.

### ARCHITECTURE.md

Record decisions that constrain multiple tasks or would be costly to rediscover: component responsibilities, dependency boundaries, shared contracts, strategic technologies, security boundaries, and relevant operational constraints.

Distinguish required boundaries from illustrative designs. Leave reversible local choices to the Implementing Agent. Exact dependency versions, commands, and concrete configuration usually belong in executable sources unless a particular choice is itself a lasting constraint.

### TESTING.md

Define the product's evidence policy: available verification levels, critical properties, required integrated scenarios, limits of test doubles, reproducibility expectations, CI gates, and any required reviews.

Product-specific technologies and risks belong here. The generic [verification model](verification.md) explains how to select sufficient evidence; TESTING applies it to the actual system. The Planning Agent compiles the relevant policy into each Issue.

### Project PLANNING.md and Sprints

Project PLANNING adapts [the planning process](planning.md) to local needs: Issue granularity, repository coordination, decision authority, workflow conventions, and traceability. Keep generic methodology guidance in this repository rather than copying it into every product.

A Sprint declares an increment's objective, included and excluded capabilities, relevant constraints, dependencies, and expected outcome. It can span repositories. An Issue is an executable unit within that increment. Future Sprints can describe intent and boundaries until execution approaches.

### ISSUE and AGENTS.md

The Issue carries task-specific semantics and evidence as defined in [planning](planning.md#compile-an-issue). It does not become the permanent home of a global decision.

A small local `AGENTS.md` provides commands, repository conventions, operational restrictions, contribution steps, and pointers to executable configuration. Avoid copying PRODUCT, DOMAIN, ARCHITECTURE, or TESTING into it. Relevant global knowledge reaches the Implementing Agent through the Issue.

### Code and configuration

Use code, manifests, configuration, and executable workflows as the source of concrete implementation facts. Avoid maintaining a prose inventory that merely repeats them. Document the stable intent or constraint when those executable facts do not explain it adequately.

Existing tests preserve known expectations, but can also be outdated. Conflicts between implementation, tests, and intended behavior require interpretation within the appropriate source responsibility.

## Methodology version adoption

A product pins or explicitly declares the methodology version it follows, for example in project PLANNING:

```text
methodology: context-compiled-development@v0.1.0-alpha.1
```

Adopting a new version is an explicit project change. Review changed conventions and their effect on planned or active work; a newly published version does not automatically invalidate every existing Issue. Preliminary versions are experimental and can evolve substantially through use.

Issue provenance records both the methodology version and the product source revisions. [Planning owns the provenance and stale-context process](planning.md#provenance-and-stale-compiled-context), including propagation after source changes. Version references support reproducibility without requiring every Implementing Agent to reload the original sources.
