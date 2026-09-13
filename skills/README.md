# Skills

Reusable, composable AI capabilities. A skill is a self-contained folder that Claude Code can auto-detect and load based on its description — not just a document describing an idea.

## What belongs here

- A capability that solves one clear, repeatable task (e.g. "synthesize interview notes into themes," not "help with UX research" — too broad).
- Something you've actually used, not a theoretical idea.
- Instructions specific enough that Claude can follow them without additional clarification.

## What does not belong here

- Multi-step roles that chain several capabilities together — that's an **agent** (see `../agents/`).
- One-off wording that doesn't need frontmatter or auto-detection — that's a **prompt** (see `../prompts/`).
- Vague or aspirational capabilities without a working instruction set.

## Structure

Each skill is its own folder, named in kebab-case, containing a `SKILL.md`:

```
skills/
├── README.md
└── usability-test-analysis/
    └── SKILL.md
```

If a skill needs supporting detail too long for the main file, add a `references/` subfolder inside that skill's folder and point to it explicitly from `SKILL.md`. Keep `SKILL.md` itself under ~500 lines.

## Format

Use [`../templates/skill-template.md`](../templates/skill-template.md). Every skill requires YAML frontmatter with `name` and `description` — the description is what Claude Code scans to decide whether to load the skill, so it should state what the skill does, when to use it, and likely trigger phrases.

## Example (abbreviated)

```markdown
---
name: usability-test-analysis
description: Analyzes usability test notes or transcripts into structured findings with severity ratings. Use when the user has raw usability test data and needs synthesized, actionable findings.
---

# Skill: Usability Test Analysis

## Purpose
Turns raw usability test notes/transcripts into structured, severity-rated findings.

## Instructions
1. Group observations by task/flow.
2. Identify friction points and successes.
3. Rate each finding by severity (Critical/Major/Minor).
4. Summarize into a findings table with recommendations.
```

## Available skills

_(none yet — this section grows as skills are added)_
