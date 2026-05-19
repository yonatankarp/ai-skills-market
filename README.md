# ai-skills-market

An open-source [Claude Code](https://docs.claude.com/en/docs/claude-code) skill marketplace. Skills are authored as language-agnostic knowledge units and grouped into plugins by topic.

## Plugins

| Plugin | Purpose |
| ------ | ------- |
| **`ddd-core`** | Language-agnostic Domain-Driven Design. Four mode entry points (`ddd-design`, `ddd-review`, `ddd-refactor`, `ddd-implement`) plus thirteen pattern primitives covering Evans's strategic and tactical patterns. |
| **`ddd-kotlin`** | Kotlin idiom adapter. Translates DDD pattern decisions into idiomatic Kotlin. |

More plugins (other languages, adjacent patterns like CQRS / event sourcing / hexagonal architecture) will live as siblings under `plugins/`.

## Skill index

### `ddd-core` (17 skills)

**Mode skills (entry points):**

- [`ddd-design`](plugins/ddd-core/skills/ddd-design/SKILL.md) — guide a new domain model from brief to draft
- [`ddd-review`](plugins/ddd-core/skills/ddd-review/SKILL.md) — PR-style audit against the DDD anti-pattern catalog
- [`ddd-refactor`](plugins/ddd-core/skills/ddd-refactor/SKILL.md) — sequence small steps toward a target DDD shape
- [`ddd-implement`](plugins/ddd-core/skills/ddd-implement/SKILL.md) — bridge pattern decisions to code via a language adapter

**Strategic patterns:**

- [`ddd-ubiquitous-language`](plugins/ddd-core/skills/ddd-ubiquitous-language/SKILL.md)
- [`ddd-bounded-context`](plugins/ddd-core/skills/ddd-bounded-context/SKILL.md)
- [`ddd-context-mapping`](plugins/ddd-core/skills/ddd-context-mapping/SKILL.md)
- [`ddd-subdomain-classification`](plugins/ddd-core/skills/ddd-subdomain-classification/SKILL.md)

**Tactical patterns:**

- [`ddd-entity`](plugins/ddd-core/skills/ddd-entity/SKILL.md)
- [`ddd-value-object`](plugins/ddd-core/skills/ddd-value-object/SKILL.md)
- [`ddd-aggregate`](plugins/ddd-core/skills/ddd-aggregate/SKILL.md)
- [`ddd-domain-event`](plugins/ddd-core/skills/ddd-domain-event/SKILL.md)
- [`ddd-domain-service`](plugins/ddd-core/skills/ddd-domain-service/SKILL.md)
- [`ddd-repository`](plugins/ddd-core/skills/ddd-repository/SKILL.md)
- [`ddd-factory`](plugins/ddd-core/skills/ddd-factory/SKILL.md)

**Cross-cutting:**

- [`ddd-anti-patterns`](plugins/ddd-core/skills/ddd-anti-patterns/SKILL.md) — catalog cited by `ddd-review` and `ddd-refactor`
- [`ddd-deeper-insight`](plugins/ddd-core/skills/ddd-deeper-insight/SKILL.md) — Evans Part III techniques

### `ddd-kotlin` (1 skill)

- [`ddd-kotlin-idioms`](plugins/ddd-kotlin/skills/ddd-kotlin-idioms/SKILL.md) — pattern → idiomatic Kotlin (stdlib only, no framework opinions)

## Installing

```bash
# Inside Claude Code
/plugin marketplace add yonatankarp/ai-skills-market   # once published
/plugin install ddd-core@ai-skills-market
/plugin install ddd-kotlin@ai-skills-market
```

Or, for local development:

```bash
/plugin marketplace add ~/Projects/ai-skills-market
```

## How the skills compose

Mode skills are *entry points* triggered by user intent ("help me design a domain", "review this for DDD smells", etc.). They orchestrate, then reference pattern primitive skills.

Pattern primitives are *knowledge units* — one DDD concept each. They define what the pattern is, when to apply it, when not to, and the decisions involved. They never commit to a programming language.

Language adapter plugins like `ddd-kotlin` never duplicate concepts. They translate pattern decisions into idiomatic code for one language.

This means you can install `ddd-core` alone to get DDD guidance in any language, and add `ddd-kotlin` (or future `ddd-typescript`, `ddd-rust`, …) for language-specific scaffolding.

## Repository layout

```
ai-skills-market/
├── .claude-plugin/marketplace.json     # Marketplace definition
├── plugins/
│   ├── ddd-core/
│   │   ├── .claude-plugin/plugin.json
│   │   └── skills/                     # One folder per skill, each with SKILL.md
│   └── ddd-kotlin/
│       ├── .claude-plugin/plugin.json
│       └── skills/
├── docs/
│   ├── specs/                          # Design specs per major change
│   └── CONTRIBUTING.md                 # Skill authoring conventions
├── references/                         # Shared reference material (book citations, diagrams)
└── README.md
```

## Contributing

See [`docs/CONTRIBUTING.md`](docs/CONTRIBUTING.md) for skill authoring conventions and how to add a new plugin.

## License

[MIT](LICENSE)

## Acknowledgements

The `ddd-core` plugin is grounded in *Domain-Driven Design: Tackling Complexity in the Heart of Software* by Eric Evans (2003). Skills paraphrase and apply Evans's concepts; they do not reproduce the book.
