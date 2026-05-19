# Evans, *Domain-Driven Design* — Chapter Map

Pins which sections of Evans's *Domain-Driven Design: Tackling Complexity in the Heart of Software* (2003) each skill cites, so skill authors don't need to re-skim the whole book.

The book has four parts plus a glossary and bibliography. Chapter numbers below are stable across printings; PDF page numbers vary by edition (the version in `~/Downloads/` totals 324 pages).

## Part overview

| Part | Chapters | Topic | Cited by skills |
| ---- | -------- | ----- | --------------- |
| I. Putting the Domain Model to Work | 1–3 | Crunching knowledge, communication & language, binding model to implementation | `ddd-design`, `ddd-ubiquitous-language` |
| II. The Building Blocks of a Model-Driven Design | 4–7 | Layered architecture; entities, value objects, services, modules; aggregates, repositories, factories; lifecycle | `ddd-entity`, `ddd-value-object`, `ddd-domain-service`, `ddd-aggregate`, `ddd-repository`, `ddd-factory`, `ddd-implement` |
| III. Refactoring Toward Deeper Insight | 8–13 | Breakthrough, implicit concepts explicit, supple design, declarative design, model integrity | `ddd-deeper-insight`, `ddd-refactor` |
| IV. Strategic Design | 14–17 | Bounded contexts, context mapping, distillation (core/supporting/generic), large-scale structure | `ddd-bounded-context`, `ddd-context-mapping`, `ddd-subdomain-classification` |

## Per-chapter quick reference

### Part I — Putting the Domain Model to Work

- **Ch. 1. Crunching Knowledge** — Knowledge crunching with domain experts; producing a useful model is iterative dialogue, not requirements gathering. Cited by `ddd-design`.
- **Ch. 2. Communication and the Use of Language** — Ubiquitous Language: one language for experts, developers, code, tests, within one bounded context. Cited by `ddd-ubiquitous-language`.
- **Ch. 3. Binding Model and Implementation** — Model-Driven Design; hands-on modelers; the link between concept and code. Cited by `ddd-design`, `ddd-deeper-insight`.

### Part II — The Building Blocks of a Model-Driven Design

- **Ch. 4. Isolating the Domain** — Layered architecture (UI / Application / Domain / Infrastructure). Cited by `ddd-review` (leaky layering).
- **Ch. 5. A Model Expressed in Software** — Entities, Value Objects, Domain Services, Modules. The canonical definitions. Cited by `ddd-entity`, `ddd-value-object`, `ddd-domain-service`.
- **Ch. 6. The Life Cycle of a Domain Object** — Aggregates, Repositories, Factories. The lifecycle trio. Cited by `ddd-aggregate`, `ddd-repository`, `ddd-factory`.
- **Ch. 7. Using the Language: An Extended Example** — A worked cargo-shipping example tying together Part II. Useful as a reference example when authoring skill bodies.

### Part III — Refactoring Toward Deeper Insight

- **Ch. 8. Breakthrough** — Modeling breakthroughs are gradual. Cited by `ddd-deeper-insight`.
- **Ch. 9. Making Implicit Concepts Explicit** — Specifications, policies, named domain rules. Cited by `ddd-deeper-insight`, `ddd-refactor`.
- **Ch. 10. Supple Design** — Intention-revealing interfaces, side-effect-free functions, assertions, conceptual contours, standalone classes, closure of operations. Cited by `ddd-deeper-insight`.
- **Ch. 11. Applying Analysis Patterns** — Borrowing Fowler-style analysis patterns. Background reading.
- **Ch. 12. Relating Design Patterns to the Model** — GoF patterns in DDD context (Strategy, Composite as domain patterns). Background.
- **Ch. 13. Refactoring Toward Deeper Insight** — How to recognize and act on the urge to remodel. Cited by `ddd-refactor`.

### Part IV — Strategic Design

- **Ch. 14. Maintaining Model Integrity** — Bounded Context, Context Map, and the seven context-mapping patterns: Shared Kernel, Customer-Supplier, Conformist, Anticorruption Layer, Open Host Service, Published Language, Separate Ways (plus Big Ball of Mud as the anti-state). Cited by `ddd-bounded-context`, `ddd-context-mapping`.
- **Ch. 15. Distillation** — Core Domain, Generic Subdomains, Domain Vision Statement, Highlighted Core, Cohesive Mechanisms, Segregated Core, Abstract Core. Cited by `ddd-subdomain-classification`.
- **Ch. 16. Large-Scale Structure** — Evolving Order, System Metaphor, Responsibility Layers, Knowledge Level, Pluggable Component Framework. Background; rarely cited directly.
- **Ch. 17. Bringing It All Together** — Synthesis. Background.

## What Evans 2003 does *not* cover well

Some patterns the DDD community now treats as canonical are absent or only briefly mentioned in the 2003 text:

- **Domain Events** — Mentioned in passing; the canonical treatment came later (Vernon's *Implementing DDD*, 2013). `ddd-domain-event` cites Evans Ch. 5 for adjacency to Domain Service and notes the later canonicalization.
- **CQRS, Event Sourcing, Hexagonal Architecture** — Out of scope for this book; not cited by any `ddd-core` skill in v0.1.0.

## How skills should cite

Format: `Evans, *Domain-Driven Design* (2003), Ch. N` — chapter granularity. Page numbers are optional and only worth including when the citation is to a specific definition or diagram. Do not cite chapter ranges casually ("Chapters 5–7") — pick the chapter that *defines* the concept.
