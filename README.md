# AI Skills

A growing collection of practical, reusable AI skills by **Artur Justyński**.

These skills are designed as focused workflows rather than one-off prompts. They follow the open Agent Skills format and aim to stay portable across compatible AI clients wherever the underlying runtime allows it.

## Skills

| Skill | What it does |
| --- | --- |
| **[Grillwise](skills/grillwise/)** | Stress-tests decisions, plans, architectures, proposals, and projects through an adaptive interview that exposes assumptions, risks, trade-offs, validation needs, and reversible next steps. |
| **[Routewise](skills/routewise/)** | Routes software-engineering work to the least expensive sufficient model-and-reasoning strategy for each stage, escalating capability only when evidence shows it is needed. |

## Installation

For now, installation is intentionally simple and manual:

1. Clone or download this repository.
2. Choose a directory under [`skills/`](skills/).
3. Install or copy that complete skill directory into a skills location supported by your AI client.

The skill directory should be kept intact, including any `references/`, `scripts/`, or other bundled resources it contains.

Client-specific locations and installation flows vary. A provider-aware installer may be added later; until then, this repository avoids hard-coding paths that are not portable across clients.

## Repository structure

```text
ai-skills/
├── README.md
├── LICENSE
├── .github/
│   └── workflows/
│       └── validate-skills.yml
├── evals/
│   ├── grillwise/
│   │   ├── README.md
│   │   └── cases.yaml
│   └── routewise/
│       ├── README.md
│       ├── cases.yaml
│       └── results/
│           └── 2026-08-24-hardening-dry-run.md
└── skills/
    ├── grillwise/
    │   └── SKILL.md
    └── routewise/
        ├── SKILL.md
        └── references/
            ├── runtime-adapters.md
            └── verification.md
```

## Design principles

- **Useful before impressive.** A skill should solve a real workflow problem, not exist to look sophisticated.
- **Progressive disclosure.** Keep the core behavior in `SKILL.md`; move optional detail into supporting resources only when it genuinely helps.
- **Evidence over ceremony.** Validation, escalation, and complexity should be justified by the task rather than added by default.
- **Portable where practical.** Keep skill logic vendor-neutral unless runtime-specific behavior is actually required.

## Validation

Every push and pull request that changes skills, evals, or the validation workflow runs lightweight checks for required `SKILL.md` files, valid YAML frontmatter, naming constraints, directory/name consistency, description limits, local skill references, and regression-fixture structure. Long `SKILL.md` files also receive a warning so they can be reviewed for progressive disclosure opportunities.

Behavioral regression fixtures for instruction refactors live under [`evals/`](evals/). They protect behavior rather than exact response wording and are intentionally lightweight until an automated multi-run evaluation harness becomes worthwhile.

## License

MIT. See [LICENSE](LICENSE).

## Author

**Artur Justyński**  
GitHub: [@arturjustynski](https://github.com/arturjustynski)