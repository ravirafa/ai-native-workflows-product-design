# Prompts

Standalone, reusable prompts for situations where a full skill is unnecessary overhead.

## What belongs here

- A single reusable instruction you'd otherwise retype or reconstruct each time.
- Something genuinely portable — plain text/Markdown, no frontmatter, works in Claude, ChatGPT, Cursor, or anywhere else.

## What does not belong here

- Anything that needs auto-detection or structured inputs/outputs — that belongs in `../skills/`.
- Multi-step sequences — that belongs in `../agents/`.
- If a prompt keeps growing more specific and gains real structure (defined inputs, expected outputs, edge cases), graduate it to a skill.

## Structure

```
prompts/
├── README.md
└── design-critique-opener.md
```

Each prompt is a single Markdown file, named descriptively in kebab-case.

## Format

Use [`../templates/prompt-template.md`](../templates/prompt-template.md).

## Example (abbreviated)

```markdown
# Prompt: Design Critique Opener

## Use case
Kicking off a structured critique of a design artifact without leading with personal opinion.

## Prompt
Look at this design. Before giving any opinion, list only what you observe:
layout, hierarchy, content, and interaction patterns. Do not evaluate yet —
just describe what's there.

## Notes
Forces evidence-before-assumption; pairs well with the "structured critique" pattern.
```

## Available prompts

_(none yet — this section grows as prompts are added)_
