---
name: anti-slop-ui
description: Helps designers and developers avoid generic AI-generated interfaces when creating or reviewing a landing page, website, app screen, dashboard, component, UI mockup, or redesign. Establishes design intent, replaces generic defaults with product-specific details, and reviews the result against evidence.
---

# Anti-slop UI

AI-generated interfaces often rely on familiar patterns that look polished but could belong to any product. This skill grounds UI work in product-specific intent, avoids generic defaults, and checks the result against the actual interface or code.

## Purpose

Guide interface generation and review so the layout, language, and visual identity express this product and support its users' main task.

## When to use

Use when creating or reviewing a landing page, website, app screen, dashboard, component, or UI mockup. Use it even when the user does not mention generic AI design.

Do not use it for non-UI work or as a substitute for a specialized usability or accessibility audit. It can identify accessibility issues, but does not establish full conformance by itself.

## Inputs

Look for these six design-intent fields in the conversation, project files, or brief:

```text
DESIGN INTENT
Product: [name and what it does]
Users: [who, and what they care about]
Feels like: [3 words]
Never feels like: [3 words, e.g. generic SaaS]
One signature detail: [what only this product would do]
Reference: [a real product or place the user admires]
```

If intent is missing or thin, ask up to five concise questions, combining related fields where useful. Do not start from vague direction such as "modern and clean." If the user prefers to proceed without answering, state the assumptions in three lines at the top of the response.

## Expected outputs

- The requested UI, or a review grounded in the supplied UI/code.
- For reviews, an evidence table covering product specificity, defaults, signature detail, task hierarchy, action-label consistency, contrast, and keyboard access.
- A short list titled **Defaults I used without a reason.**
- Three decisions that need a human designer's judgment.

## Instructions

### 1. Establish design intent

Use the six fields in Inputs. Treat existing project files and the user's brief as evidence; do not invent product facts or claim a reference was provided when it was not.

### 2. Avoid generic defaults

Do not use these unless the design intent explicitly calls for them:

- Purple-to-blue (or any) gradient hero, floating glow blobs, or mesh backgrounds
- A pill badge above the headline (for example, "AI-powered" or "Next-gen")
- Headlines built as "Elevate / Unlock / Transform / Reimagine / Supercharge + Your + noun"
- Three feature cards with circle icons and vague titles such as "Seamless," "Powerful," or "Secure"
- Glassmorphism as the default container
- Fake social proof, including invented stars, logos, testimonials, or user counts
- A gradient "Get started" pill paired with a ghost "Learn more" button
- Identical dashboard stat tiles without a task-based reason
- Emoji used as icons
- Filler marketing words such as "journey," "experience," "solutions," "streamline," "empower," "innovation," "next-gen," or "cutting-edge"

### 3. Replace defaults with product specifics

- Make each headline say something factual and specific to this product. Apply the swap test: replace the product name with a competitor's; if the headline still works unchanged, make it more specific.
- Name real features, numbers, or constraints instead of adjectives. For example, "Table for four by 7pm, no deposit" is more specific than "Seamless reservations." Do not invent claims to make copy feel specific.
- Derive palette, type, and imagery from the product's world, users, and references rather than generic labels such as "modern" or "clean."
- Choose the layout for the user's main task, not a default hero / three-cards / CTA sequence.
- Include the signature detail identified in the design intent.

### 4. Use a coherent, accessible system

- Follow provided design tokens, type scale, spacing rules, and component library. If none are provided, define a small set before building: a type scale, spacing rules, two brand colors, and neutrals.
- Use semantic elements: `button` for actions, `a` for navigation, and `label` for inputs. Do not make a `div` clickable when a native control fits.
- Check body-text contrast against WCAG 2.2 SC 1.4.3: 4.5:1 for normal text and 3:1 for large text. Do not claim a pass without measuring or otherwise establishing the colors.
- Provide visible focus states and keyboard operation for interactive controls.
- Use the same label for the same action across screens.

### 5. Review with evidence

After generating or inspecting the UI, quote evidence from the actual UI, design, or code. Mark anything that cannot be verified as unknown rather than claiming it passes.

| Check | Pass / Fail | Evidence |
|---|---|---|
| Swap test: headline is product-specific | | Quote the headline and explain what makes it specific. |
| No unjustified defaults | | List any found. |
| Signature detail present | | Say where it appears, or that it is missing. |
| Hierarchy and spacing support the main task | | Name the task and the supporting element. |
| Same action, same label on every screen | | List the labels or identify inconsistencies. |
| Contrast and keyboard baseline met | | Cite measured contrast and keyboard/focus evidence, or list what could not be verified. |

Mark any element you chose without a stated reason as [DEFAULT], and do not explain why.

### 6. Hand judgment back

Finish with three decisions for a human designer, such as brand voice, signature moment, or a hierarchy trade-off. Do not describe the result as finished: implementation does not replace design judgment.

## Constraints

- Never invent product facts, testimonials, logos, metrics, references, or evidence.
- A missing design-intent field is not permission to assume it; ask or clearly state assumptions.
- A visual review cannot confirm runtime behavior, keyboard support, or measured contrast unless the available artifact exposes that evidence.
- Treat the banned patterns as defaults to challenge, not universal prohibitions; explicit product intent can justify one.

## Example

**Input:** A neighborhood bicycle repair shop wants a booking page. Customers care about knowing when they can collect a repaired bike. The shop confirms that standard tune-ups are ready the same afternoon when dropped off before 10am. Its signature detail is a hand-marked repair ticket shown with each booking.

**Specific direction:** Use the confirmed cutoff as the headline fact, organize the page around booking a tune-up and choosing a drop-off time, and show the repair ticket as the signature detail. Do not invent prices, customer counts, or service guarantees. In review, verify the actual headline, booking flow, ticket treatment, and keyboard/contrast evidence rather than assuming they are present.

## Related skills

- [Target Size Review](../target-size-review/SKILL.md) — assess interactive target sizes in UI code, designs, or specs.

## Portability

- **Claude Code / Claude.ai:** install this folder in the appropriate skills directory and invoke it when its description matches the task.
- **Other tools:** adapt the Instructions section to the tool's rules or prompt format; do not assume it loads `SKILL.md` natively.