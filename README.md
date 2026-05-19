# ai-skills-market

An open-source [Claude Code](https://docs.claude.com/en/docs/claude-code) skill marketplace. Skills are authored as language-agnostic knowledge units and grouped into plugins by topic.

## Plugins

| Plugin | Purpose |
| ------ | ------- |
| **`ddd-core`** | Language-agnostic Domain-Driven Design. Four mode entry points (`ddd-design`, `ddd-review`, `ddd-refactor`, `ddd-implement`) plus thirteen pattern primitives covering Evans's strategic and tactical patterns. |
| **`ddd-kotlin`** | Kotlin idiom adapter. Translates DDD pattern decisions into idiomatic Kotlin. |

More plugins (other languages, adjacent patterns like CQRS / event sourcing / hexagonal architecture) will live as siblings under `plugins/`.

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
