# DDD Skill Suite — Design Spec

**Date:** 2026-05-19
**Status:** Draft, awaiting review
**Repo:** `ai-skills-market`

## Goal

Ship a Claude Code plugin marketplace whose first two plugins teach and apply Domain-Driven Design (DDD). The marketplace must scale — additional plugins (other languages, adjacent patterns) drop in as siblings with no central refactor.

## Constraints

- **Language-agnostic core.** DDD is not a language concept. Core skills describe patterns in prose and decisions, never in language-specific code.
- **Kotlin first as an adapter.** Kotlin idioms ship as a separate plugin that *references* core pattern skills. The same shape supports future language adapters.
- **Open-source.** MIT license, public-facing README, contributor guide.
- **Marketplace shape, not flat collection.** Repo follows the standard Claude Code marketplace layout (`.claude-plugin/marketplace.json` + per-plugin folders) so users can `/plugin install ddd-core@ai-skills-market`.

## Non-goals

- Reproducing Eric Evans's book. The book is a *reference* for grounding; skills paraphrase and apply, not transcribe.
- Spring / Arrow / JPA opinions in `ddd-kotlin`. Those become their own opt-in plugins if/when needed.
- DDD-adjacent patterns (event sourcing, CQRS, hexagonal architecture). They are explicitly out of scope for this spec and will be authored as future plugins.

## Architecture: mode + pattern hybrid

Two layers in `ddd-core`:

### Layer 1 — Mode entry-point skills (4)

Triggered by user *intent*. They orchestrate; they do not contain the bulk of the knowledge.

| Skill | Triggers on |
| ----- | ----------- |
| `ddd-design` | "help me design a domain", "model this", "what's the bounded context for X" |
| `ddd-review` | "review this for DDD smells", "is this code DDD-clean", PR review framing |
| `ddd-refactor` | "refactor to use aggregates", "extract value object", "split this aggregate" |
| `ddd-implement` | "scaffold an entity", "give me code for X aggregate" — hands off to the language adapter for the actual code |

Mode skills are short. Each:
1. Asks 2–4 clarifying questions to scope the work.
2. Names which pattern primitives apply.
3. Hands off to the relevant pattern skill(s).
4. For `ddd-implement`, also hands off to a language adapter (e.g., `ddd-kotlin-idioms`).

### Layer 2 — Pattern primitive skills (13)

One per DDD concept. Each is a self-contained knowledge unit answering:

- **What is this pattern?**
- **When does it apply?** (decision heuristics, indicator phrases)
- **When does it NOT apply?** (common over-application pitfalls)
- **Trade-offs and variants.**
- **A small worked example in prose/pseudocode** — no language commitment.
- **Common anti-patterns and code smells** that signal misuse.

**Strategic patterns (4):**

| Skill | Maps to Evans |
| ----- | ------------- |
| `ddd-ubiquitous-language` | Ch. 2 |
| `ddd-bounded-context` | Ch. 14 |
| `ddd-context-mapping` | Ch. 14 (Shared Kernel, Customer-Supplier, Conformist, ACL, OHS, Published Language, Separate Ways, Big Ball of Mud) |
| `ddd-subdomain-classification` | Ch. 15 (Core / Supporting / Generic) |

**Tactical patterns / building blocks (7):**

| Skill | Maps to Evans |
| ----- | ------------- |
| `ddd-entity` | Ch. 5 |
| `ddd-value-object` | Ch. 5 |
| `ddd-aggregate` | Ch. 6 |
| `ddd-domain-event` | (Evans 2003 lite; standard DDD extension) |
| `ddd-domain-service` | Ch. 5 |
| `ddd-repository` | Ch. 6 |
| `ddd-factory` | Ch. 6 |

**Cross-cutting patterns (2):**

| Skill | Maps to Evans |
| ----- | ------------- |
| `ddd-anti-patterns` | Cross-cutting (anemic model, primitive obsession, leaky repository, smart UI, etc.) |
| `ddd-deeper-insight` | Part III (Chs 8–13) — making implicit concepts explicit, supple design, intention-revealing interfaces, side-effect-free functions, assertions, conceptual contours, standalone classes, closure of operations |

### Layer 3 — Language adapter (`ddd-kotlin`)

One skill (for now):

| Skill | Purpose |
| ----- | ------- |
| `ddd-kotlin-idioms` | Translates each pattern decision into idiomatic Kotlin (data class / value class / sealed class / object / Result / require/check / extension functions). Cross-references the relevant pattern skill. |

Future adapter skills (`ddd-kotlin-spring`, `ddd-kotlin-arrow`, `ddd-typescript-idioms`) ship as sibling skills or sibling plugins without affecting `ddd-core`.

## How skills compose at runtime

Example user prompt: *"Help me model a refund flow for the payments domain."*

1. `ddd-design` triggers on intent.
2. `ddd-design` asks scoping questions (bounded context, existing model, integrations).
3. `ddd-design` cites and loads `ddd-ubiquitous-language`, `ddd-bounded-context`, `ddd-aggregate` as the active pattern primitives.
4. The user gets a structured walkthrough that ends in a domain sketch.
5. If the user then asks "now write the Kotlin code", `ddd-implement` triggers, which in turn invokes `ddd-kotlin-idioms`.

## File layout

```
ai-skills-market/
├── .claude-plugin/marketplace.json
├── plugins/
│   ├── ddd-core/
│   │   ├── .claude-plugin/plugin.json
│   │   └── skills/
│   │       ├── ddd-design/SKILL.md
│   │       ├── ddd-review/SKILL.md
│   │       ├── ddd-refactor/SKILL.md
│   │       ├── ddd-implement/SKILL.md
│   │       ├── ddd-ubiquitous-language/SKILL.md
│   │       ├── ddd-bounded-context/SKILL.md
│   │       ├── ddd-context-mapping/SKILL.md
│   │       ├── ddd-subdomain-classification/SKILL.md
│   │       ├── ddd-entity/SKILL.md
│   │       ├── ddd-value-object/SKILL.md
│   │       ├── ddd-aggregate/SKILL.md
│   │       ├── ddd-domain-event/SKILL.md
│   │       ├── ddd-domain-service/SKILL.md
│   │       ├── ddd-repository/SKILL.md
│   │       ├── ddd-factory/SKILL.md
│   │       ├── ddd-anti-patterns/SKILL.md
│   │       └── ddd-deeper-insight/SKILL.md
│   └── ddd-kotlin/
│       ├── .claude-plugin/plugin.json
│       └── skills/
│           └── ddd-kotlin-idioms/SKILL.md
├── docs/
│   ├── specs/2026-05-19-ddd-skill-suite-design.md
│   └── CONTRIBUTING.md
├── references/
│   └── evans-chapter-map.md             # Cite-able page/chapter index
├── LICENSE                              # MIT
├── README.md
└── .gitignore
```

**Total skill count for v0.1.0: 18** (4 mode + 13 pattern + 1 Kotlin adapter).

## Skill description discipline

Each skill's `description:` field is load-bearing — Claude uses it to decide whether to invoke. Conventions:

- **Mode skills** mention concrete user phrases ("review this for DDD", "model a domain", "refactor an aggregate").
- **Pattern skills** mention *both* the pattern name AND the symptoms that should pull it in (e.g., `ddd-value-object` triggers on "value object", "primitive obsession", "wrap this primitive into a type").
- **Adapter skills** mention the source pattern + the language (e.g., `ddd-kotlin-idioms` triggers on "kotlin DDD", "kotlin entity", "kotlin value class").

## Risks and mitigations

| Risk | Mitigation |
| ---- | ---------- |
| **Trigger collisions** — 17 ddd-core skills with overlapping vocabulary may all fire on "ddd". | Tight description fields. Mode skills explicitly list "use me first; I'll route to pattern skills". Pattern skills explicitly say "do not invoke me directly for general DDD questions; the mode skills will route". |
| **Skill bloat** — long SKILL.md files lose Claude's attention. | Target 150–400 words per SKILL.md body. Detailed worked examples go in adjacent `examples/` files referenced from the SKILL.md. |
| **Generic content** — risk of reproducing Evans verbatim. | Each skill must include at least one *decision* heuristic (when to apply / not apply) and at least one *anti-pattern* warning — concrete authoring discipline, not just summary. |
| **Maintenance** — 18 skills is a lot to keep current. | `docs/CONTRIBUTING.md` formalizes the structure. A short skill-template file in `references/` for new authors. |

## Out of scope (deferred)

- `ddd-kotlin-spring` / `ddd-kotlin-arrow` adapter sub-skills.
- Other language adapter plugins.
- CQRS, event sourcing, hexagonal architecture plugins.
- Evaluation harness for skill triggering quality.

## Open questions

None known at draft time. Will surface during implementation.

## Acceptance criteria

- Repo installs cleanly via `/plugin marketplace add ~/Projects/ai-skills-market`.
- Both plugins install via `/plugin install`.
- All 18 SKILL.md files validate (have required frontmatter, are not empty).
- Smoke test: a "help me design a payments domain" prompt routes through `ddd-design` → at least one pattern skill is referenced.
- README, LICENSE, CONTRIBUTING are in place.
- A first commit lands on `main`.
