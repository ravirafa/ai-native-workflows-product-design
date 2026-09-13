# Agents

Task-oriented AI roles that compose multiple existing skills into an end-to-end job.

## What belongs here

- A defined role (e.g. "Accessibility Reviewer") that runs a sequence of steps, each backed by a skill from `../skills/`.
- Something that produces a complete output from a real starting input — not a single atomic action.

## What does not belong here

- Logic that duplicates an existing skill instead of referencing it. If an agent needs a capability, point to the skill — don't re-describe it inline.
- A single-step capability — that belongs in `../skills/` instead.

## Structure

```
agents/
├── README.md
└── accessibility-reviewer.md
```

Agents are single Markdown files (not folders) since they primarily document composition and sequence rather than standalone instructions.

## Format

Use [`../templates/agent-template.md`](../templates/agent-template.md). Every agent should explicitly list which skills it composes and in what order.

## Example (abbreviated)

```markdown
# Agent: Accessibility Reviewer

## Role
Reviews a UI artifact end-to-end for accessibility gaps and produces a prioritized fix list.

## Composed skills
1. `contrast-check` (skills/contrast-check)
2. `semantic-structure-review` (skills/semantic-structure-review)
3. `aria-audit` (skills/aria-audit)

## Workflow
1. Run contrast-check on all visible text/UI elements.
2. Run semantic-structure-review on the markup/component tree.
3. Run aria-audit on interactive elements.
4. Merge findings into one prioritized list (Critical/Major/Minor).
```

## Available agents

_(none yet — this section grows as agents are added)_
