# AI Skills

A growing collection of practical, reusable AI skills by **Artur Justyński**.

These skills are designed as focused workflows rather than one-off prompts. They follow the open Agent Skills format and aim to stay portable across compatible AI clients wherever the underlying runtime allows it.

## Skills

| Skill | What it does |
| --- | --- |
| **[Grillwise](skills/grillwise/)** | Stress-tests decisions, plans, architectures, proposals, and projects through an adaptive interview that exposes assumptions, risks, trade-offs, validation needs, and reversible next steps. |
| **[Routewise](skills/routewise/)** | Recommendation-only entry point that chooses the next Routewise workflow and a provisional model + reasoning route without planning or implementing the task. |
| **[Routewise Plan](skills/routewise-plan/)** | Builds execution-optimized implementation plans that move load-bearing reasoning into planning so downstream stages can use the least expensive sufficient capability. |
| **[Routewise Implementation](skills/routewise-implementation/)** | Routes ready plans to the least expensive sufficient `(model, reasoning)` pair, validates execution, and recommends a bounded re-plan when plan quality is what makes implementation expensive. |

The Routewise skills are intentionally separated by phase. `routewise` is a read-only recommendation gate, `routewise-plan` shapes work for economical execution, and `routewise-implementation` routes or executes work that is already sufficiently specified.

## Installation

For now, installation is intentionally simple and manual:

1. Clone or download this repository.
2. Choose a directory under [`skills/`](skills/).
3. Install or copy that complete skill directory into a skills location supported by your AI client.

The skill directory should be kept intact, including any `references/`, `scripts/`, or other bundled resources it contains.

For the complete Routewise workflow, install all three Routewise directories: `routewise`, `routewise-plan`, and `routewise-implementation`.

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
│   ├── routewise/
│   │   ├── README.md
│   │   ├── cases.yaml
│   │   └── results/
│   │       ├── 2026-08-24-hardening-dry-run.md
│   │       └── 2026-08-24-simplification-dry-run.md
│   ├── routewise-plan/
│   │   ├── README.md
│   │   └── cases.yaml
│   └── routewise-implementation/
│       ├── README.md
│       └── cases.yaml
└── skills/
    ├── grillwise/
    │   └── SKILL.md
    ├── routewise/
    │   └── SKILL.md
    ├── routewise-plan/
    │   └── SKILL.md
    └── routewise-implementation/
        └── SKILL.md
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
