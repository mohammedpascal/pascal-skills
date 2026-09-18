---
name: simplify-product
description: Simplify a product feature, screen, or user flow by first principles — remove, automate, combine, default and defer before redesigning. Use for a product design or UX review of a screen, feature, PRD, screenshot, wireframe or user flow, when a UI feels cluttered or needs decluttering, when a feature list keeps growing, or when asked to simplify, cut scope, or challenge whether something should exist.
license: MIT
---

# Simplify Product

## Prime directive

**Do not assume the existing screen should continue to exist.**

Don't ask how to make the feature simpler. First ask whether the user should have to deal
with the feature at all.

The goal is not a visually minimal UI. The goal is less thinking, deciding, configuring,
remembering and manual work for the user. A sophisticated system may run underneath while
the experience stays extremely simple.

Never open by redesigning the current UI. A prettier version of a cluttered screen is a
failed pass.

---

## Inputs

Any combination of: screenshot, screen description, feature description, user story, PRD,
existing flow, wireframe, list of controls/actions, product goal, user goal.

When information is missing, infer reasonable possibilities — but mark assumptions clearly
and keep them separate from what is known.

---

## Core question

Before evaluating anything, answer in one sentence: **what is the user actually trying to
accomplish?**

Describe the desired outcome, never the current UI.

- Bad: "The user wants to tag support tickets by category."
- Better: "The user wants the right person to pick up this ticket quickly."

Output as:

```
REAL USER NEED:
<one sentence>
```

---

## Core philosophy

- Simplicity isn't removing things — it's understanding the problem deeply enough that the
  obvious solution appears.
- Say no to 1,000 things. Focus is deciding what *not* to build.
- Start from the experience, work backwards to the technology.
- If a feature needs explaining, the design failed.
- Make the default the right answer for 90% of people. Never ship a preference to dodge a
  decision.

---

## The nine principles

Apply every one.

### 1. Solve the real problem

- Why does the user need this?
- What outcome are they reaching for?
- Is this solving the need, or implementing a requested solution?
- Could the same outcome be reached without this feature?

### 2. Rebuild from first principles

Ignore how competing products solve it. Break the experience into fundamental requirements.

- What absolutely must happen?
- Which assumptions are inherited from other products rather than from the need?
- If we designed this today from zero, would this screen exist?
- Does the user have to understand the system's internal structure? They shouldn't.

Write the minimum flow. No UI yet. Example: `Audio captured → content understood →
important information extracted → available when needed`.

### 3. Remove friction

For every tap, field, confirmation, screen, setting, choice, modal, navigation step,
permission and manual organization step, ask: *why must the user do this?*

Classify each one: **KEEP / REMOVE / AUTOMATE / DEFAULT / DEFER**.

Prefer removing an interaction over making it prettier.

### 4. Reduce choices

Find the decisions being pushed onto the user — templates, folders, AI actions, output
formats, defaults to configure, which processing operation to run.

- Can context determine this automatically?
- Can one strong default handle most cases?
- Can advanced choices appear only when relevant?
- Can several choices collapse into one intelligent action?

Never expose a system capability merely because it exists.

### 5. Design for human psychology

Ask: *what does the user's attention go to first?* There should normally be one obvious
primary action.

Evaluate against these (Apple HIG):

- **Clarity** — legible text, obvious affordances, one clear focal point per screen.
- **Deference** — the UI gets out of the way; content is the interface, chrome is minimal.
- **Depth** — hierarchy and motion communicate where you are, instead of labels and
  instructions.
- **Consistency** — reuse familiar patterns so people transfer knowledge instead of
  relearning.
- **Feedback** — every action gets an immediate, visible response.
- **Forgiveness** — undo over confirm; make mistakes cheap rather than preventing them
  with dialogs.

Also check: information hierarchy, progressive disclosure, visual noise, cognitive load,
perceived speed, confidence, predictability, wording. Explain state changes with motion
and hierarchy before adding explanatory text.

### 6. Protect the product vision

State the core promise: *"We help users ______ without having to ______."*

Example: "We help support teams answer customers well without having to read the whole
history first."

- Does this feature strengthen the promise?
- Is it merely technically impressive?
- Does it contradict the product philosophy?
- Is it better omitted?

A feature can be useful and still not belong in the product.

### 7. Make language extremely clear

Replace technical terms, internal system vocabulary, abstract labels and long button text
with what the user already understands.

- "Semantic knowledge retrieval" → "Ask your notes"
- "Initiate audio capture" → "Record"
- "Generate AI summary" → "Summary"

Verbs for actions, familiar nouns for destinations.

### 8. Reduce implementation-driven UX

Find where technical or organizational architecture leaks into the experience: separate
services becoming separate screens, database entities becoming nav items, different
backend processes becoming different buttons.

Ask: *would the user care that these are separate systems?* If not, unify them. The
product absorbs technical complexity rather than exposing it.

### 9. Prevent future complexity

- Will this create another menu?
- Will every future feature add another tab?
- Will users eventually need to configure this?
- Is this a pattern that will produce clutter?
- Can the architecture gain capabilities without gaining UI?

```
BAD:  Summary · Tasks · Translate · Quiz · Mindmap · Email · Slides · Flashcards · Rewrite
BETTER: Ask AI, with contextual suggestions.
```

---

## Applied to features

- One job per feature. If it does two things, it's two features — or it's the wrong feature.
- Kill before you add. A new feature should replace something, not accumulate beside it.
- Integrate rather than append: fold the capability into an existing flow instead of adding
  a new entry point.
- Progressive disclosure: power available, not visible. Advanced options live one layer down.
- No settings toggle to resolve an internal disagreement about the default.

## Applied to screens

- One primary action per screen, visually dominant. Everything else is secondary or hidden.
- Remove every element that doesn't help the user do the thing or understand the state.
- Reduce steps before reducing pixels — flow simplification beats visual decluttering.
- Don't ask for what you can infer, remember, or defer.
- Empty states teach; they don't apologize.
- Text is UI: rewrite labels until they're short and unambiguous, then design around them.

---

## The simplification pass

Run in this order. Never start at step 7.

1. **REMOVE** — what can disappear completely?
2. **AUTOMATE** — what can the system infer or do instead?
3. **COMBINE** — which separate concepts become one?
4. **DEFAULT** — which decision can be made for most users?
5. **DEFER** — what only needs to appear when relevant?
6. **CLARIFY** — what language or hierarchy can become obvious?
7. **REDESIGN** — only now.

---

## Output

```
## User goal
<one sentence>

## Current mental model
What the current design expects the user to understand.

## Fundamental flow
The experience independent of UI. e.g. Capture → Understand → Remember → Act

## Complexity detected
The main sources of complexity.

## Remove
## Automate
## Combine
## Default
## Defer

## Simplified experience
Step by step.

## Proposed screen
Minimum UI: primary action, secondary actions, visible information, hidden/contextual actions.

## Before → After
Before: <current>
After:  <simplified>

## The test
- Can a first-time user complete the main task without reading anything?
- Can you delete this element / step / screen and still ship? Then delete it.
- "If we removed ______, the user could still accomplish ______ because ______."
```

Use the final test to challenge whatever complexity survived.

---

## Worked example

**Input:** a support ticket detail screen with tabs for Conversation, Summary, Suggested
Reply, Sentiment, Translate, Related Tickets, Macros and Custom Prompt.

A weak review rearranges the tabs. This is the expected behavior instead:

**Real goal:** understand this customer's problem and answer it well.

```
REMOVE    permanent Sentiment / Related Tickets / Macros / Custom Prompt tabs
AUTOMATE  summary written on arrival; the customer's ask detected automatically
COMBINE   Suggested Reply + Macros → one draft you can edit
DEFAULT   Conversation + Summary always ready
DEFER     Translation appears when the customer wrote in another language
          Related tickets appear when one is actually similar
```

```
Refund not received — Dana Okoye
Open · 2 days · 4 messages

Summary
<summary>

Draft reply
<draft>

Ask anything about this ticket…
```

The system may still support thirty AI capabilities. The interface exposes three things.
Capabilities stay; surfaces collapse.
