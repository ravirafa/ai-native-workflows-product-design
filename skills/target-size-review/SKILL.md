---
name: target-size-review
description: Review interactive target sizes for touch/pointer usability — in code (CSS, JSX, SwiftUI, XML layouts), in a design file (Figma, Sketch, etc.), or in a written spec. Inspect likely hit areas, distinguish visual bounds from interaction bounds, evaluate WCAG 2.2 target-size minimums and spacing exceptions, assess touch/mouse/multi-input usability, and give prioritized, component-level guidance without inventing implementation facts.
---

# Target Size Review

## Goal

Evaluate whether interactive controls are easy to acquire and hard to activate by mistake.

Do not optimize for a single number. Consider:
- actual/inferred target area
- spacing between targets
- input method
- target location on screen
- action importance and frequency
- visual feedback
- component structure
- likely precision of the user

The review should help a designer and developer answer:

> Can a person reliably activate the intended control without accidentally activating another control?

---

# 1. Review scope

Start with whatever is in front of you: a selected element in a design tool, a component or stylesheet open in an editor, a snippet the person pasted, or a written spec.

Identify the source type first, since it changes what counts as evidence:
- **Code** (CSS, JSX/TSX, SwiftUI, XML, Compose) — width/height/padding/hit-slop values are explicit and can be read directly.
- **Design file** (Figma, Sketch, Adobe XD) — target boundaries are inferred from frames, auto-layout, components, and layer names.
- **Spec or documentation** — target sizes may be stated as numbers in prose or a table; treat these as design intent, not implementation guarantees.

If nothing specific is selected or provided and the working context allows it, review the relevant screen, component, or file currently open.

Prioritize:
- mobile and responsive screens
- headers and navigation
- forms
- dialogs/sheets
- toolbars
- pagination
- filters and chips
- lists
- cards
- sliders/carousels
- reusable components/component sets

Do not treat static/decorative layers as interactive unless the design structure or naming strongly indicates that they are targets.

Infer likely interactive targets from:
- component/instance structure
- component names and layer names
- prototype interactions when visible
- button/link-like affordances
- repeated interaction patterns
- labels such as Button, Link, Tab, Close, Menu, Search, Edit, Delete, Next, Previous, Checkbox, Radio, Switch, Slider, Pagination, Filter, etc.

### Layer-naming synonyms

Multiple naming conventions mean the same concept. Treat all of the following as hit-area containers, not separate concepts:
- **Hit target** (Apple HIG convention)
- **Touch target** (Material Design / Android convention)
- **Target size** (WCAG / W3C convention)
- **Tap target** (Lighthouse / Chrome DevTools convention)
- **Clickable area**, **Hit area**, **Interaction area**, **Touch zone** (design-community informal)

When a layer uses any of these names, treat it as the explicit interaction boundary, regardless of the naming convention used.

When interaction intent is uncertain, use **Needs verification** instead of inventing behavior.

---

# 2. Evidence model

Before evaluating size, establish the strongest evidence available for the target boundary.

Rank evidence from strongest to weakest:

1. Explicit width/height/padding/hitSlop/min-tap-target values read directly from code — the strongest possible evidence.
2. Explicit target/hit-area container in a design file (any naming convention from §1 synonyms list).
3. Component or instance frame that clearly represents the control.
4. Auto-layout padding/constraints that clearly enlarge the interaction container.
5. A parent group/frame that visually and semantically represents the control.
6. Visual bounds only (a screenshot, image, or rendered preview with no accessible markup or layer data).
7. Purely inferred implementation behavior with no supporting artifact.

When reviewing code directly, prefer computed/effective size over declared size — a `width: 24px` button with `padding: 12px` has an effective target of 48px; do not report it as 24px.

Never present levels 5–7 as confirmed runtime behavior.

Use language such as:
- "Target area is explicit in the code (width/height/padding/hitSlop)."
- "Target area is explicit in the design file's frame or auto-layout."
- "Target area is strongly implied by the component frame."
- "Visual bounds only; implementation hit area cannot be confirmed."

---

# 3. Unit system and platform context

Whatever the source — code, design file, or spec — values are usually expressed as plain numbers or in `px`. The meaning of that number depends on which platform is being built for.

Establish platform context first. Look for:
- File name, page name, or frame name indicating iOS / Android / Web / Desktop
- Design system tokens labelled `dp`, `pt`, or `sp`
- Component library origin (Material, HIG, Fluent, etc.)

### Unit interpretation

| Platform | Common source unit | Actual unit | Notes |
|---|---|---|---|
| Web | px (CSS, Tailwind, inline styles) | CSS px | Direct 1:1. WCAG thresholds apply in CSS px. |
| Android | dp in XML/Compose, or px in a design file at 1× | dp (density-independent pixels) | 1 unit at 1× = 1dp. At 2× (XHDPI), 48dp needs 96 raw pixels to render at the same physical size. |
| iOS | pt in SwiftUI/UIKit, or px in a design file at 1× | pt (points) | 1 unit at 1× = 1pt. 44pt is Apple's minimum. |
| Multi-platform / unknown | px | Treat as CSS px | Flag the ambiguity. |

**Never silently convert dp or pt into CSS px.** A 48dp Android touch target and a 48px CSS target differ meaningfully when screen density is considered.

When the platform is Android, the key insight is: the dp value stays constant across devices, but the raw pixel count used to render it scales with screen density (48dp = 48px at MDPI, 96px at XHDPI, 144px at XXHDPI). This is why dp and px are not interchangeable — flag any file that conflates them.

When platform context cannot be determined, apply WCAG CSS-px thresholds and flag the uncertainty.

---

# 4. Core standards

## WCAG 2.2 SC 2.5.8 — Target Size (Minimum), Level AA

For pointer inputs, the target must contain a **24 × 24 CSS-pixel axis-aligned square**, unless one of the defined exceptions applies.

Do not judge this only from the target's outer width and height:
- A rectangular 24×24 target can satisfy the size requirement.
- A circular or heavily rounded 24×24 target does **not** automatically satisfy it, because a full 24×24 square must fit inside the target.
- For circular controls (icon buttons, play/pause buttons, carousel arrows, FABs): the 24px square must fit within the circle. A circle with diameter 24px does not contain a 24×24 square. The minimum circle diameter to pass the square test is approximately 34px (24 × √2).
- For irregular, clipped, concave, or SVG targets, use the target's actual shape/bounding box carefully and do not reduce the rule to width × height.

When the source material cannot establish CSS-pixel equivalence or the actual rendered pointer target, report the result as inferred/needs verification rather than claiming definitive WCAG conformance.

Relevant WCAG exceptions:
- **Spacing**
- **Equivalent**
- **Inline**
- **User Agent Control**
- **Essential**

Do not convert this into the rule "every control must be 24 × 24." Evaluate the exception conditions.

## Spacing exception — use the correct geometry

For an undersized target, conceptually place a **24px-diameter circle centered on the target's bounding-box center**.

Check whether that circle would intersect:
- another target, or
- the corresponding 24px circle around another undersized target.

If not, the target may satisfy the WCAG spacing exception.

Do not claim definitive compliance if the source material does not expose enough information.
Use:
- "Appears to satisfy the spacing exception"
- "Spacing exception cannot be determined from the design"

Do not simply measure the gap between visible icons and call that the WCAG test.

## WCAG 2.5.5 — Target Size (Enhanced), Level AAA

**44 × 44 CSS px** is the enhanced benchmark.

Treat 44 × 44 as the preferred/general target-size recommendation for comfortable interaction, not as the Level AA minimum.

## Platform minimums for reference

| Standard | Minimum | Unit | Level / Context |
|---|---|---|---|
| WCAG 2.5.8 | 24 × 24 | CSS px | AA — enforceable floor, spacing exception applies |
| WCAG 2.5.5 | 44 × 44 | CSS px | AAA — enhanced, no spacing exception |
| Apple HIG | 44 × 44 | pt | iOS / iPadOS |
| Material Design | 48 × 48 | dp | Android — ≈9mm physical |
| Microsoft Fluent | 40 × 40 | px | Windows |
| Android Auto | 76 × 76 | dp | In-car — sized for glance-and-tap while driving |

Apply the platform minimum first; WCAG thresholds are floor values, not targets.

---

# 5. Visual size vs target size

This is the most important distinction in the review.

A visual object may be smaller than its interaction target.

### Canonical icon example

Use this exact mental model when evaluating icon buttons:

- **24×24 visual icon + 24×24 rectangular hit area** → meets the numeric minimum but only if the actual target geometry can contain a 24×24 square; a circular/rounded 24×24 target does not automatically pass.
- **24×24 visual icon + 44×44 hit area** → preferred target-size pattern.
- **24×24 visual icon + 48×48 hit area** → strong touch-target pattern; Material Design's benchmark.

The larger interaction area may come from **invisible padding or a larger interactive parent**. The visible icon does not need to become larger.

When a mobile/native source uses `dp` or `pt`, preserve the unit distinction: `24dp` can describe the visual icon while `48dp` describes the touch target. Do not silently convert `dp` or `pt` into CSS `px`; report the units represented in the source and treat platform-unit equivalence as contextual.

Think of the pattern as:

`24×24 icon → invisible padding → 44×44 or 48×48 interaction target`

versus:

`24×24 icon → no padding → 24×24 interaction target`

### Centering rule

A padded target only distributes its forgiveness evenly when the element is **centered** within it.

An icon pushed to one corner of its touch target leaves most of the padding on the wrong side — the finger still misses from the opposite direction.

Google's Android Auto design system states this explicitly: elements must be centered within their touch target area, not edge-aligned.

Flag any design where:
- an icon appears offset within its container
- padding is asymmetric without a clear layout reason
- the element crowds one edge of its declared touch target

### What to inspect

1. Is the icon merely the visual glyph?
2. Is there a larger frame/component/instance that represents the control?
3. Does that larger parent include padding or otherwise establish the hit area?
4. Is the element centered within that container, not edge-aligned?
5. Is the larger area allowed to be visually invisible? Yes — do not require a visible boundary merely to count the target.
6. Does the larger target collide with a neighboring target?

Examples:

**Good**
- 20×20 icon inside a 44×44 button, centered
- 24×24 close icon inside a 44×44 target, centered
- 24×24 icon inside a 48×48 touch target, centered
- small visual glyph with a larger explicit clickable container

**Concern**
- 20×20 icon appears to be the entire target
- 24×24 icon appears to be the entire target
- navigation text is the only visible/structural target while nearby whitespace is outside it
- a large card looks clickable but only a small icon/label appears to be interactive
- icon visually shifted to one edge of its container

Never recommend enlarging the icon itself when the better solution is to enlarge or re-center its target container.

---

# 6. Target-size decision tree

For each likely interactive target:

### Step A — What is the target?

Identify the semantic interaction, not merely the layer being measured.

### Step B — What is the defensible target boundary?

Prefer the explicit control container over the visual child.

### Step C — Is it at least 44×44 (or the platform minimum if higher)?

If yes:
- mark preferred-size pass
- still check centering, spacing, and context

If no:
- continue

### Step D — Does the actual target contain a 24×24 square?

For rectangular targets: straightforward.
For circular/round targets: check whether diameter ≥ 34px (minimum to contain a 24×24 square).

If yes:
- mark WCAG size pass
- check spacing/context because a minimum-size pass can still be poor touch UX

If no:
- classify as undersized
- evaluate WCAG exceptions
- evaluate spacing geometry

### Step E — Does a WCAG exception clearly apply?

If yes or plausibly yes:
- identify the exception
- explain the evidence
- avoid claiming certainty without sufficient evidence

If no:
- report an AA issue

### Step F — How is it used and where on screen is it placed?

Assess input method:
- touch
- mouse
- stylus
- multi-input

Assess screen position (see §9 accuracy gradient):
- center — most accurate zone, 7mm minimum effective
- upper edge — 9mm minimum effective
- lower edge — 9mm minimum effective
- top corner — 11mm minimum effective, least precise
- bottom corner — 12mm minimum effective, least precise

Higher screen-position risk = higher scrutiny, not just flagging.

### Step G — Are adjacent targets risky?

Check actual target boundaries, not just decorative gaps.

### Step H — Would a system/component fix solve several instances?

Prefer systemic recommendations.

---

# 7. Spacing and target relationships

Never evaluate a target alone.

Review neighboring interactive targets for:
- overlap
- tight separation
- ambiguous boundaries
- accidental activation risk
- competing actions
- destructive actions near common actions
- compact icon clusters

Important relationship rule:

**Small targets need more surrounding space. Large targets can tolerate smaller gaps.**

Do not blindly require a fixed gap between every control.

For large neighboring targets, zero visual gap can be acceptable when their interaction areas remain clearly separable and easy to acquire.

For small targets, increase separation when necessary to prevent target interference.

Use the touch-size concept as a usability heuristic, not as a WCAG compliance formula: human fingertips can be substantially larger than a small visual target, so small/close controls deserve extra scrutiny.

The real index finger contact patch measures 16–20mm (≈45–57px). The thumb's contact patch is approximately 25mm (≈71px). A control that looks adequately sized for a cursor may still be smaller than the pad that will touch it.

---

# 8. Screen-position accuracy gradient

Target size requirements are not uniform across the screen.

Steven Hoober's touch-accuracy research shows that people tap most accurately at the **center** of the screen, with precision decreasing toward the edges. Critically, this **inverts the classic Fitts's Law "magic corners" principle**: for mouse interfaces, corners are easiest to hit (infinitely deep). For touch, corners are the hardest to tap accurately.

Effective minimum sizes by screen zone (Hoober's data):

| Zone | Effective minimum target |
|---|---|
| Center | 7mm / ~20pt / ~27px |
| Upper and lower mid-screen | 9mm / ~28pt |
| Top corner | 11mm / ~31pt / ~42px |
| Bottom corner | 12mm / ~34pt / ~46px |

Apply these as **usability heuristics**, not WCAG compliance formulas.

In practice this means:
- A target meeting the WCAG 24px minimum at the top corner is at higher mis-tap risk than the same target in the center.
- Close/dismiss buttons in top-right corners warrant higher scrutiny than the same control placed centrally.
- Bottom navigation actions deserve attention because they sit in a precision-critical zone.
- Do not justify a smaller target by pointing to a corner location — it makes precision *harder*, not easier.

When flagging corner or edge controls, explain *why* the position increases risk, citing the accuracy gradient.

---

# 9. Touch realism heuristics

Touch is not a tiny mouse cursor.

For touch-first interfaces, increase scrutiny when:
- targets are small
- several targets are adjacent
- targets are near edges/corners (see §8)
- actions are frequent or time-sensitive
- the UI may be used while moving/shaking
- a user's finger angle is not perpendicular to the screen
- touch may occur with gloves or reduced precision

Do not pretend a static design or a code diff can prove finger physics.
Report these as **usability risk heuristics**, not compliance failures.

Use 44×44 as the preferred baseline where practical.

---

# 10. Input-context review

A target that works with a mouse may still be hard to tap.

Evaluate:

### Touch
Be stricter about target size and spacing.

### Mouse
Small visual controls may be workable, but still evaluate WCAG pointer-target requirements and focus/hover affordance.

### Stylus / mixed input
Use the more conservative interpretation when the design is intended for multiple input sources and the exact context is unknown.

When context is ambiguous, say so.

---

# 11. Fitts's Law and target importance

Apply Fitts's Law qualitatively:
- larger targets are easier to acquire
- nearby targets are faster to acquire
- important/frequent/time-sensitive actions should not be unnecessarily difficult to acquire

Prioritize larger/easier targets for:
- primary actions
- frequent actions
- destructive confirmation flows
- controls that must be activated quickly
- edge/corner controls that are hard to acquire precisely

Do not calculate fake Fitts's Law scores from a static screenshot or code snippet.
Use it to explain priority and interaction risk.

---

# 12. Visual feedback and rage tap risk

Check whether the interaction boundary is communicated.

Look for:
- hover states for pointer interactions
- pressed/active states where appropriate
- selected state for tabs/navigation
- visible focus indication when keyboard access is relevant
- clear affordance for icon-only controls

**Rage tap risk:** When a control provides no visible, audible, or haptic feedback after activation, users cannot confirm whether their tap registered. This leads to repeated taps on the same spot — a pattern tracked in production analytics as a **rage tap**. Rage taps are a real production metric, not just a design-review concern.

Flag missing or delayed feedback as a rage-tap risk when:
- the control provides no visible state change on activation
- feedback is delayed beyond approximately 100ms
- the control is small and the user may doubt whether they hit it
- destructive or high-stakes actions give no confirmation before executing

A larger target with an invisible boundary may still be confusing.

For links or icon-only controls, visual feedback can help users understand the effective target area.

Do not require hover feedback for touch-only controls.

---

# 13. Pattern-specific checks

## Icon buttons

Check the target container, not icon glyph size.

Preferred:
- small icon + 44×44 or 48×48 target, icon centered within it

Watch for:
- icon-only targets packed together
- inconsistent target sizes across a toolbar
- small target beside a destructive action
- icon offset to one edge of its container

## Circular / round targets

Carousel arrows, play/pause buttons, FABs, and floating action buttons are often circular.

The same minimum-size rules apply — shape does not create an exception.

Key check: a 24px-diameter circle does NOT contain a 24×24 square. The minimum circle diameter to contain a 24×24 square is approximately 34px (24 × √2 ≈ 33.9px).

Preferred pattern: same as square targets — small glyph inside a larger circular hit area, glyph centered.

## Full-row interactive targets (lists, navigation, settings)

List rows exist in two modes:
- **Actionable:** the entire row width is one tap target — one gesture, one action.
- **Static:** only a nested icon or label responds to touch.

A design that *looks* like a full-row target but is *implemented* as an icon-only target will produce mis-taps at the row's trailing edge, where users naturally tap on wider screens.

Flag rows where:
- the visual row implies full-width interactivity (dividers, hover states, chevrons)
- but only a small child element appears to be the target
- or when the row label is the target but unmarked trailing whitespace is not

Recommend making the entire row actionable when the UX model implies it.

## Close / dismiss buttons

Pay special attention to:
- top-right placement (high-risk corner zone — see §8)
- proximity to menus or other actions
- tiny corner targets

A corner location does not justify a smaller target; it makes precision harder.

## Destructive actions adjacent to safe actions

The Delete/Edit pairing is the highest-stakes spacing failure.

When a destructive action (Delete, Remove, Disconnect, Sign out) sits adjacent to a safe action (Edit, View, Open):
- flag as Critical if they are within a single finger-width
- recommend moving the destructive action to a separate row, increasing spacing, or requiring a deliberate gesture (long-press, swipe)
- match the safeguard to reversibility: prefer an undo toast for recoverable actions (deletion into a trash/archive); reserve a confirmation dialog for genuinely irreversible or high-consequence actions
- do not recommend a confirmation dialog as a blanket fix for every destructive action — overuse trains users to dismiss it without reading, which defeats its purpose

## Text buttons

Check padding around the text action itself.

Do not confuse:
- spacing around the group
with
- target area around each button

A visually generous parent gap does not automatically make the text button's own target large.

## Website / horizontal navigation

Check whether padding belongs to each interactive item.

Potential problem:
- navigation item is only as wide/tall as its text while the apparent menu row is much larger

Prefer:
- individual items with explicit target area
- consistent target sizing across items

## Vertical navigation / sidebar

Check whether each row or item owns the available interactive area when the visual design implies a full-row navigation target.

Flag confusing cases where only the text appears clickable but the entire row visually reads as interactive.

## Tabs

The target area should belong to each tab item, not merely to parent padding or whitespace between tabs.

Check:
- tab target height
- tab target width
- neighboring tab separation
- selected/hover/focus states

Large tabs may legitimately have little/no visible gap between them.

## Checkbox / radio groups

Check the actual interactive label/control relationship.

Potential issue:
- large bordered row visually implies a full-row click target
- checkbox/radio target remains limited to a tiny control

Prefer making the full row tappable when the label and control share the same action.

Also evaluate vertical spacing between multiple options.

## Chips / tags / filters

Check whether the whole chip is meant to be interactive.

Do not place padding on a non-interactive wrapper while leaving only the label as the target if the visual design implies the whole chip is clickable.

## Search / filter controls

Check icon-only filter/dismiss actions inside or beside search fields.

A tiny icon target can be expanded without changing the visible icon size.

Also check spacing between the search field and adjacent controls.

## Pagination

Pagination often appears visually spacious because of parent `gap`, even when each individual target is tiny.

Check the target area of:
- Previous
- Next
- page numbers
- overflow controls

Prefer padding on each pagination item when practical instead of relying on outer group spacing.

## Sliders / progress controls

Do not judge only the visible thumb or 4px progress bar.

Look for an enlarged interaction container around the control.

When the visual track is thin but the interaction container (padding, hitSlop, hit area) is large, treat that as a positive pattern.

## Carousels / media controls

Check previous/next targets and play/pause controls.

Previous/next arrows are frequently circular — apply the circular target check (minimum ~34px diameter to contain a 24×24 square; 44–48px preferred).

Be especially careful when controls are clustered near other actions.

## Cards

Distinguish:
- the whole-card target
from
- a secondary icon action inside the card

A secondary icon should not accidentally inherit the card's target or become too difficult to activate separately.

## Maps / dense visualizations

Recognize that dense spatial interaction may be essential.

Do not automatically fail it.
Look for:
- nearby competing targets
- alternative control paths
- zoom/selection affordances
- whether important actions have another accessible route

## Scrolling containers

Check targets near the edge of scrollable regions.

Potential problems:
- small controls are clipped by the container
- hit areas extend outside visible bounds
- adjacent scroll gestures compete with small buttons
- the user may accidentally scroll instead of activating the control

## Overflow / nested menus

For desktop pointer interactions, check whether a submenu requires precise travel from a parent item to the submenu.

Where a menu visually depends on hover continuity, flag a **safe-triangle / hover-path risk** when the path can easily leave the active target and collapse the submenu.

This is a usability heuristic, not a WCAG target-size calculation.

---

# 14. Placement and reachability

Target size is partly about where the target is placed.

Call out higher risk for:
- top-right or bottom-corner controls on mobile (hardest zones — see §8)
- controls far from the thumb's likely resting area
- frequently used controls placed in hard-to-reach positions
- clustered corner actions
- destructive actions near the screen edge with no recovery

Do not assert a dominant-hand failure from a static design or code alone.
State it as a consideration when placement makes reachability likely to matter.

---

# 15. System font / dynamic sizing consideration

When the design uses text-based controls, navigation, or icon systems that are expected to scale with user text settings:
- check whether the interaction container is likely to grow with content
- check whether larger text could cause targets to collide or shrink
- flag controls whose target area is fixed while content may scale significantly

Do not claim that a static design or a single code snapshot proves runtime dynamic-type behavior.

Use **Needs verification** for implementation-dependent behavior.

---

# 16. Special exception handling

Before reporting an undersized target as a WCAG failure, check whether it is plausibly:

### Inline
Examples may include links embedded in text.

### Equivalent
An equivalent larger target may provide the same function.

### User Agent Control
Browser/user-agent controlled UI may be out of the product's target-size scope.

### Essential
A larger target may materially change or prevent the essential function.

### Spacing
The 24px-circle condition may protect the undersized target.

Do not invent an exception simply to get a pass.

---

# 17. Severity model

## Critical
Use when there is a strong likelihood of difficult activation or accidental activation, especially for touch or high-consequence actions.

Examples:
- undersized target with nearby competing target and no spacing protection
- overlapping/ambiguous interaction areas
- tiny destructive action adjacent to another action with no recovery
- multiple compact touch controls that can be hit together
- corner-placed control below the position-adjusted minimum (see §8)

## Warning
Use when:
- below preferred 44×44 (or platform minimum if higher)
- spacing is tight but not clearly failing AA
- the target boundary is ambiguous
- touch usability is questionable
- visual affordance does not match the apparent interaction area
- element is not centered within its target container
- rage-tap risk from missing or delayed feedback

## Pass
Use when the target has sufficient evidence of an adequate target area and reasonable separation for its context.

## Needs verification
Use **specifically** when at least one of the following applies — not as a general fallback:
- the actual hit area is implementation-dependent and cannot be inferred from the design
- a WCAG exception requires runtime information not present in the design
- runtime text scaling may affect the target
- the design uses dp/pt values but platform context is ambiguous

---

# 18. What counts as a finding

Do not generate noise.

Only report a finding when at least one is true:
- target is below a relevant threshold
- target spacing creates plausible accidental activation
- target container is ambiguous
- element is not centered within its target
- input context makes the control materially harder to use
- interaction boundary conflicts with visual affordance
- repeated component implementation is likely to create the same problem
- placement/reachability introduces a meaningful usability concern (see §8)
- missing feedback creates rage-tap risk on a high-stakes or frequent action

Do not report:
- every icon below 44×44 when its parent target is 44×44+
- every 24×24 target as an AA failure
- every zero-gap layout as a spacing failure
- platform-specific rules not represented in the project guidance
- centering issues where the offset is less than 2px and likely pixel-rounding

---

# 19. Review workflow

Follow this sequence:

1. Establish platform context and unit system (CSS px / dp / pt).
2. Identify likely interactive targets, applying the naming-synonym map.
3. Establish the strongest evidence for each target boundary.
4. Separate visual bounds from target bounds.
5. Check centering of element within its target container.
6. Measure target width and height in the correct unit.
7. For circular/round targets: verify the 34px minimum diameter rule.
8. Identify nearby interactive targets.
9. Evaluate WCAG 2.5.8 AA minimum and applicable exceptions.
10. Evaluate the preferred 44×44 benchmark and platform minimum separately.
11. Evaluate spacing and accidental activation.
12. Evaluate touch/mouse/multi-input context.
13. Evaluate screen-position accuracy risk (§8 gradient).
14. Evaluate destructive-action proximity and recovery.
15. Check visual feedback and rage-tap risk.
16. Check relevant patterns: navigation, tabs, pagination, forms, sliders, cards, menus, etc.
17. Check repeated components for systemic issues.
18. Prioritize findings by user impact.

Do not modify the design, code, or spec during review unless the user explicitly asks for fixes.

---

# 20. Output format

Start with:

## Target-size review

- **Platform / unit system:** Web (CSS px) / Android (dp) / iOS (pt) / Unknown
- **Screens/frames reviewed:** X
- **Interactive targets identified:** X
- **WCAG AA issues:** X
- **44×44 preferred-size warnings:** X
- **Centering issues:** X
- **Rage-tap risk flags:** X
- **Needs verification:** X

Then report findings in this order:
1. Critical
2. Warnings
3. Needs verification
4. Passes / strengths

For every finding use:

**[Severity] Element — Location**
- **Visual size:** X×Y
- **Target size:** X×Y / unknown
- **Evidence:** explicit / component / inferred / visual-only
- **Centered within target:** Yes / No / Cannot determine
- **WCAG 2.5.8:** Pass / Fail / Exception may apply / Cannot determine
- **44×44 benchmark:** Pass / Below preferred
- **Screen position risk:** Low (center) / Medium (edge) / High (corner)
- **Nearby-target risk:** Low / Medium / High
- **Feedback / rage-tap risk:** Low / Medium / High
- **Input context:** Touch / Mouse / Multi-input / Unknown
- **Why:** concise explanation
- **Recommendation:** specific design or component change

For an undersized target that may pass by spacing, explicitly state the 24px-circle reasoning.

For a circular target, explicitly state the diameter-vs-34px check.

For a small icon inside a larger target, explicitly say:
> "The icon is visually small, but the interaction target is larger."

Do not state inferred hit areas as confirmed implementation behavior.

---

# 21. Final synthesis

End every review with:

## Highest-impact fixes

Provide the 3 most valuable changes, ordered by user impact.

## Component/system opportunities

Identify repeated patterns that should be solved in a reusable component or design-system rule.

Examples:
- standardize icon-button target area with centered element
- define compact/default/comfortable target variants
- move padding onto the interactive child
- standardize spacing between adjacent controls
- make full rows interactive when the UX model implies it
- separate destructive actions from safe actions structurally
- document explicit target-area behavior for developers
- add minimum height tokens to navigation rows, chips, and pagination items

## Design guidance

Give 2–5 concise principles tailored to the reviewed design.

## Confidence

State whether the review is based on:
- explicit target containers
- component structure
- inferred containers
- visual bounds only
- mixed evidence

Be transparent about anything that requires implementation or usability testing to verify.
