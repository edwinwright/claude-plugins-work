# claude-plugins-work — Claude Context

A Claude plugin marketplace of software-delivery tooling, intended to be **public** as a portfolio/CV artifact demonstrating AI-engineering practice. This repo is the single source of truth for these plugins; installed copies elsewhere should be treated as stale.

> [!warning] Public repo — no personal data
> This repo is intended for GitHub and a CV. Never commit PII, personal preferences, private vault structure, client names, or credentials. Personal-flavoured content belongs in `claude-plugins-personal`. Review diffs for leakage before every push.

---

## Repo Shape

```
.claude-plugin/marketplace.json      catalog — lists every plugin (fixed path)
plugins/
└── delivery-design/                 end-to-end SDLC delivery pipeline
    ├── .claude-plugin/plugin.json
    └── skills/{product-brief, domain-doc, feature-request, feature-brief,
                feature-design, feature-plan, feature-tickets,
                feature-publish, decision-record}/
docs/                                design + build notes (not shipped in the plugin)
```

The `delivery-design` skills form a pipeline: `product-brief → domain-doc → feature-request → feature-brief → feature-design → feature-plan → feature-tickets → feature-publish`, with `decision-record` available throughout.

---

## How Plugins Work (facts, not assumptions)

- **One marketplace per repo.** `marketplace.json` lives at the fixed path `.claude-plugin/marketplace.json`. There is no mechanism for multiple marketplaces in one repo.
- **Every plugin needs `.claude-plugin/plugin.json`** with a required `version` field (used to detect updates). Missing plugin.json or missing marketplace entry = won't install.
- **Skills are auto-discovered** by walking `skills/*/SKILL.md`. They are **not** declared in `plugin.json`. Same for `commands/`.
- **`category`** is a freeform string; `productivity` is used here.
- Plugins are not surface-specific — the same plugin works in Claude Code and Cowork. Hooks and sub-agents only run in Cowork.
- MCP servers can be bundled in a plugin, but verify the exact mechanism (`plugin.json` key vs. `.mcp.json`) against current docs before adding one.

## Keep descriptions honest and in sync

The plugin description in `marketplace.json` and in the plugin's own `plugin.json` must match and must reflect what the skills actually do. When you add or remove a skill, update both descriptions and bump `version`.

## Conventions for developing skills here

- One skill = one clear trigger domain; keep genuinely different triggers as separate skills.
- Shared doctrine lives as a `references/`, `prompts/`, or `templates/` file inside the skill, not as its own triggerable skill.
- Direct and concise skill instructions; British English; no personal or client-identifying content.
