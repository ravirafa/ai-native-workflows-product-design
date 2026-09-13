# Contributing

Thanks for considering a contribution. This repo favors small, tested, real contributions over large speculative ones.

## Before you contribute

Ask: has this actually helped you accomplish a real design task with AI? If it's untested or purely theoretical, it's probably not ready yet.

## What to contribute

- **A skill** — a reusable capability. Use [`templates/skill-template.md`](./templates/skill-template.md).
- **An agent** — a role that composes existing skills into an end-to-end job. Use [`templates/agent-template.md`](./templates/agent-template.md).
- **A prompt** — a standalone reusable instruction too small for a full skill. Use [`templates/prompt-template.md`](./templates/prompt-template.md).

## How to submit

1. Fork the repo.
2. Copy the relevant template into the correct top-level folder (`skills/`, `agents/`, or `prompts/`).
3. For skills, create a new folder named in kebab-case (e.g. `usability-test-analysis/`) containing your `SKILL.md`.
4. Fill in every section of the template — incomplete templates will be sent back for revision.
5. Open a pull request with a short description of the real task this resource helps with.

## Quality bar

- Instructions should be specific enough that someone unfamiliar with your process can follow them.
- Include at least one concrete example — not just abstract description.
- Skills must include valid YAML frontmatter (`name` + `description`) so Claude Code can detect them automatically.
- No duplicate capability — if a skill already covers this, extend it or reference it instead of forking the idea.

## Style

Keep it technical, concise, and practical. No marketing language, no unsupported claims.
