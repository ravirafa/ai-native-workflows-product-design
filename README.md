# AI-Native Workflows for Product Design

A curated, open-source collection of reusable AI-native workflows, skills, agents, and prompts for people working in product design, UX, UI, research, and design systems.

This is not a prompt library. The primary unit here is a **reusable capability or way of working** — something that helps you accomplish a real design task with AI, not just a clever one-off instruction.

## Why this exists

Most AI resources for designers are either generic prompt collections or one-off tips that don't compose into anything larger. This repo takes a different approach: every resource is built to be reused, combined, and improved — skills feed into agents, agents support workflows, and patterns document the thinking that makes all of it work well in practice.

## Who it's for

- Product designers and UX/UI designers integrating AI into daily work
- UX researchers using AI to accelerate synthesis and analysis
- Design systems and accessibility practitioners
- Anyone doing design-adjacent work who wants tested, composable AI practices instead of ad-hoc prompting

## What you'll find here

| Resource | Purpose |
|---|---|
| **Skills** | Reusable, composable AI capabilities (Claude Skills format) |
| **Agents** | Task-oriented AI roles that compose multiple skills into an end-to-end job |
| **Prompts** | Standalone reusable instructions for cases too small for a full skill |
| **Templates** | Starting points for contributing new skills, agents, or prompts |

## How the repository is structured

```
/
├── skills/         Reusable capabilities Claude can load and run
├── agents/         Task-oriented roles composed from skills
├── prompts/        Standalone reusable prompts
└── templates/      Fill-in-the-blank starting points for contributors
```

Each directory has its own README explaining what belongs there, what doesn't, and how to structure a new contribution.

## Quick start

1. Browse `skills/` for a capability that matches your task.
2. If you're using Claude Code, copy the relevant skill folder into your `.claude/skills/` directory — it will be auto-detected based on its description.
3. If you're using another tool, open the skill's `SKILL.md` and adapt the instructions section directly.

## How to use a skill

Each skill lives in its own folder under `skills/` and contains a `SKILL.md` file. In Claude Code, skills are detected automatically based on their description — no manual invocation needed. In other tools, copy the instructions section into your tool's prompt or rules file.

## How to use an agent

Agents in `agents/` describe a role built from one or more existing skills, run in a defined sequence. Read the agent's file to see which skills it composes and in what order, then either run it in Claude Code (which can chain skill invocations) or follow the steps manually.

## How to contribute

See [CONTRIBUTING.md](./CONTRIBUTING.md). In short: use the templates in `templates/`, keep resources practical and tested (not theoretical), and open a PR.

## Roadmap

This repository is intentionally small at launch. Planned additions as real, tested contributions come in:
- `workflows/` — end-to-end documented ways of working
- `patterns/` — reusable AI + design working patterns
- `examples/` — real before/after examples of AI-assisted design work

## License

[MIT](./LICENSE)
