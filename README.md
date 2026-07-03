# claude-plugins-work

A Claude plugin marketplace of software-delivery tooling. It packages an end-to-end SDLC delivery pipeline as composable, natural-language-triggered skills that run inside Claude Code and Cowork.

The design goal is a single, versioned source of truth for AI-assisted delivery work: the skills that turn an idea into shipped, tracked work, with every source-of-truth document co-located with the code it describes.

## Design principles

The pipeline is built around a small set of deliberate constraints rather than an accumulation of features:

- **One source of truth: local markdown in the repo.** Project-management tools (Linear, GitHub Issues) are sync targets, never the origin. State lives where the code lives.
- **A document is worth writing only if its "why" can't be recovered from the code.** That single test gates every decision record — it keeps documentation from drifting into noise.
- **Don't reinvent formats that already have standards.** Decisions use MADR-style records; agent context uses `AGENTS.md`; nothing bespoke where a convention already exists.
- **One skill, one trigger domain.** Skills with genuinely different trigger conditions stay separate rather than being merged into a monolith; shared doctrine lives as reference files, not as its own triggerable skill.

## The pipeline

```
PRODUCT workflow  (new product)
  product-brief → domain-doc → product-backlog

FEATURE workflow  (per backlog item)
  feature-request → feature-brief → feature-design → feature-plan → feature-tickets → feature-publish
      intake          idea→PRD        PRD→spec         spec→plan       plan→files       files→Linear/GitHub

decision-record is available throughout, at any point a costly, hard-to-reverse decision is made.
```

A brand-new product runs the product workflow first, then pushes each backlog item through the feature workflow. A new feature on an existing product skips straight to `feature-brief`.

## Skills

| Skill | Does |
|---|---|
| `product-brief` | Idea → Product Brief + MoSCoW-prioritised backlog |
| `domain-doc` | Bootstraps and maintains the glossary + domain model |
| `feature-request` | Intake triage — the front door before a PRD |
| `feature-brief` | Feature idea → PRD |
| `feature-design` | PRD → Technical Specification (runs a virtual dev team of specialist lenses) |
| `feature-plan` | PRD + Tech Spec → sequenced Delivery Plan |
| `feature-tickets` | Delivery Plan → local Epic + Story ticket files |
| `feature-publish` | Local ticket files → Linear or GitHub Issues |
| `decision-record` | MADR-style decision record with a significance gate |

## Repository structure

```
.claude-plugin/marketplace.json      marketplace catalog
plugins/
└── delivery-design/
    ├── .claude-plugin/plugin.json    plugin manifest (versioned)
    └── skills/                       one directory per skill, each with SKILL.md + templates
docs/                                 design and build notes (not shipped in the plugin)
```

Skills are auto-discovered by walking `skills/*/SKILL.md`; they are not declared in the manifest. Each skill carries its own templates and, where relevant, subagent prompt files.

## Install

```
/plugin marketplace add <owner>/claude-plugins-work
/plugin install delivery-design
```

Skills then trigger on natural language — see each skill's `SKILL.md` for its trigger phrases.

## Status

The nine skills above ship and are in active use. The pipeline is evolving as the practice matures; design rationale for larger decisions lives in `docs/`.
