---
name: skill-name-here
description: What this skill does, when Claude should use it, and likely trigger phrases. This is the ONLY part Claude sees before deciding whether to load the full skill — be specific.
---

# Skill: [Name]

## Purpose
One or two sentences — what real task does this help accomplish?

## When to use
Concrete situations where this applies. Note when NOT to use it, too.

## Inputs
What the user needs to provide (context, files, data, constraints).

## Expected outputs
What this produces, and in what form.

## Instructions
Step-by-step logic to follow. This is the actual prompt Claude runs when the skill is invoked. Be specific enough that someone unfamiliar with the process could follow it.

## Constraints
Known limitations, edge cases, things this should refuse or flag.

## Example
A real (or realistic) input → output pair.

## Related skills
Links to other skills this composes well with.

## Portability
- **Claude Code / Claude.ai**: drop this folder into `~/.claude/skills/` (personal) or a project's `.claude/skills/` — auto-detected by description match.
- **Other tools (Cursor, Copilot, etc.)**: no native SKILL.md support. Copy the Instructions section into that tool's rules/instructions file, or paste directly as a prompt.
