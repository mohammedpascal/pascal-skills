---
name: simplify-logic
description: Simplify code logic by first principles — find workarounds, legacy and backward-compatibility paths, duplicate states or concepts, dead flags and over-abstraction, and ask the developer what the code is meant to achieve before changing anything. Use when reviewing a module, domain model, state machine or a feature's logic, when a codebase has drifted over time and leftovers have piled up, when two statuses or fields seem to mean the same thing, or when asked to simplify logic, remove legacy, untangle a model, or challenge whether code should exist.
license: MIT
---

# Simplify Logic

## Prime directive

**Do not assume the existing code should continue to exist.**

Don't ask how to make this code cleaner. First ask what the developer was trying to achieve,
and whether the system needs this logic at all to achieve it.

Code drifts. A status is added that duplicates an old one. A workaround outlives the bug it
patched. A compatibility path stays for a version nobody runs. A flag finishes rolling out and
its `if` stays behind. None of it was wrong when written; all of it is cost now.

The goal is not fewer lines. The goal is fewer concepts a reader must hold in their head to
predict what the system does. A sophisticated system may still run underneath; the model it
exposes to the next developer should be the smallest one that serves the business.

Never open by refactoring. A tidier version of the same tangled model is a failed pass.

---

## Inputs

Any combination of: a file, module, directory, feature, domain concept ("order status",
"retries"), a diff, a schema, a bug that keeps coming back, or a vague "this area feels
messy".

When the scope is a concept rather than a file, find everywhere it lives first — types,
schema, writers, readers, UI, tests, docs.

---

## Core question

Before evaluating anything, answer in one sentence: **what business need does this code
serve?**

Describe the outcome, never the implementation.

- Bad: "Orders have nine statuses and a cancellation flag."
- Better: "Staff need to know whether an order still needs something from them."

Output as:

```
REAL INTENT:
<one sentence>
```

If you can't write it from the code, that is the first question for the developer.

---

## Evidence before claims

Every finding is backed by something you read, cited as `file:line`.

- **Callers** — grep every reader and writer. A value nothing branches on is not a concept.
- **Data** — which values actually occur? A query, fixture, seed or log beats a guess. A
  legacy branch with zero rows reaching it is dead.
- **History** — `git log -S '<symbol>'` and `git blame` tell you *why* something was added.
  The commit that introduced a workaround usually names the bug it worked around.
- **Tests** — a test pinning odd behavior is a claim someone needed it. Find out who.

Keep facts and assumptions visibly apart. "Merge these" on a guess is how real distinctions
get destroyed.

---

## Core philosophy

- Simplicity is understanding the problem well enough that the obvious model appears.
- Every concept must earn its place. A status, flag, table, option or layer that changes no
  behavior is noise someone will have to read.
- Fix causes, not symptoms. Each workaround is a bug report nobody filed.
- Replace, don't accumulate. A new concept should retire an old one completely.
- Ask the developer what they meant. Intent is cheaper to ask than to reverse-engineer.

---

## The fourteen principles

Apply every one.

### 1. Solve the real problem

- What business outcome is this code for?
- Is it implementing the need, or a solution someone once asked for?
- Could the same outcome come from logic that already exists?

### 2. Rebuild from first principles

Write the minimum model with no code: entities, their states, the allowed transitions, and
the invariants that must always hold.

- If we wrote this today from zero, would this exist?
- Which pieces are inherited from an earlier version of the product, not from the need?

### 3. One concept, one representation

Two statuses, enum values, flags, fields or tables that behave the same are one concept.

Test it: list every place each is written and read. If every branch treats them alike, they
are duplicates. If one branch differs, find out whether that difference is a business rule
or an accident.

### 4. Find the fossils

- Support for old versions, old clients, old payload shapes, old URLs
- Compatibility shims, adapters and dual reads (`new ?? legacy`)
- Feature flags that are on everywhere, or off everywhere
- Migrations that already ran, still guarded at runtime
- Fallbacks for data that no longer exists
- `TODO`, `HACK`, `FIXME`, "temporary", "for now", "until"

For each: who still reaches this path? Prove it with code and data, not with a feeling.

### 5. A workaround marks a bug

Retries, sleeps, swallowed errors, `try/catch` around nothing in particular, defensive checks
for states that should be impossible, special cases keyed on one ID or one customer, ordering
hacks, double-writes.

Find what it works around. Fix that. Delete the patch.

### 6. Make illegal states unrepresentable

- Boolean combinations (`isActive`, `isCancelled`, `isArchived`) become one enum.
- Implicit state spread across fields becomes an explicit state machine with the allowed
  transitions written down in one place.
- Nullable fields that are only set in one state belong to that state.

### 7. Single source of truth

Derive rather than store. Two fields that can disagree eventually will. A cached count, a
copied name, a status that mirrors another status — derive it, or make one the owner.

### 8. Reduce choices in code

- Parameters every caller passes the same value for
- Config options nobody changes
- An interface or strategy with one implementation
- An abstraction with one caller
- A generic layer for a case that never generalised

Inline them. Abstract on the third real use, not the first imagined one.

### 9. Validate at the boundary

Check inputs once, where they enter the system. The core trusts its types. Re-validating in
every layer hides which layer is actually responsible and buries the real invariant.

### 10. Names match the business

When a concept's meaning has shifted, rename it everywhere — code, schema, API, UI, docs.
An old word carrying a new meaning is a trap for every future reader.

### 11. Remove fully

When a concept is replaced, migrate the data and delete the old path in the same change.
No dual-read, no fallback "just in case", no deprecated-but-still-working branch.

If keeping a legacy path really seems necessary, don't decide alone — ask, and name the
date or condition under which it goes.

### 12. Prevent future complexity

- Does this shape force every new case to add another status, branch or flag?
- Will the next feature copy this workaround?
- Can the model gain capabilities without gaining conditionals?

```
BAD:    if (type === 'a') … else if (type === 'b') … else if (type === 'c') …  in 6 files
BETTER: one table of per-type behavior, read in one place
```

### 13. Structure follows the domain, not the org chart

Find structure that exists because of how the team, the history or the framework was
organised — not because the domain has two things:

- Two services, modules or tables for one concept
- One table or code path per integration, where the domain has one thing with a source
- DTO → model → entity copies carrying the same fields
- Pass-through layers that only forward calls
- One workflow split across jobs because two people built the halves

Ask: *would the domain care that these are separate?* If not, unify them. The code absorbs
organisational complexity rather than mirroring it.

### 14. Protect the product core

State the promise: *"This system lets ______ do ______."*

Simplification changes the shape of the code, not what the product does. Every behavior
that users, customers or integrations rely on — API responses, webhooks, exports, emails,
what a user sees — must survive the pass.

- A distinction that looks duplicated in code may be a real business rule. Separate the
  **promised** behavior from the **accidental** behavior before merging anything.
- If a merge or removal would change something observable from outside, it is a product
  decision, not a cleanup. Ask; don't decide. When the real question is whether the feature
  should exist at all, that is simplify-product's job.
- A simpler system that does a different job is a failed pass.

---

## Ask the developer

The code tells you *what*. Only the developer knows *why*. For every finding whose intent
the evidence can't settle, ask an implementation question framed around the business need:

- "`completed` and `done` are handled identically in A, B and C. Is there a business
  difference meant here, or can they merge?"
- "The `legacy_state` fallback in X has no rows reaching it since the March migration. Can
  it go, or does an external system still write it?"
- "The 2-second sleep before reading the webhook was added in <commit> for <bug>. Is the
  race still possible, or was the cause fixed elsewhere?"

Rules:

- Batch questions — at most four per round, recommended answer first, with the evidence that
  makes it the recommendation.
- Never ask what the code, data or history already answers.
- Any change observable from outside the code — a user, customer or integration would
  notice — is always a question, never an assumption.
- **Change no code until intent is confirmed.** Then offer to apply, smallest safe change
  first.

---

## The simplification pass

Run in this order. Never start at step 7.

1. **REMOVE** — what can be deleted outright? Dead paths, fossils, unused options.
2. **MERGE** — which duplicate concepts become one?
3. **DERIVE** — which stored values can be computed instead?
4. **INLINE** — which abstractions, parameters and layers earn nothing?
5. **FIX ROOT CAUSE** — which workarounds go once the real bug is fixed?
6. **RENAME** — which names no longer say what the thing is?
7. **RESTRUCTURE** — only now.

---

## Output

```
## Real intent
<one sentence>

## Current model
What the code makes a reader understand — every concept, status and path they must hold.

## Minimum model
Entities, states, transitions, invariants. No code.

## Complexity detected
Each source of complexity, with file:line evidence and why it exists (from history).

## Remove
## Merge
## Derive
## Inline
## Fix root cause
## Rename

## Questions for you
Only intent questions the evidence can't answer.

## Migration
Data to migrate, order of steps, what is deleted and when. No leftover legacy path.
External contracts touched (API, webhooks, exports, UI) and how each is preserved.

## Before → After
Before: <concepts / states / branches>
After:  <concepts / states / branches>

## The test
- Could a new developer predict the behavior from the minimum model alone?
- Does the product still keep its promise to every user and integration, with only the
  code's shape changed?
- Can you delete this branch / field / status and still pass the business need? Delete it.
- "If we removed ______, the system could still ______ because ______."
```

Use the final test to challenge whatever complexity survived.

---

## Worked example

**Input:** an order module. Statuses `new, pending, awaiting_payment, paid, processing,
shipped, completed, done, cancelled`. Also an `is_cancelled` boolean, a `legacy_state`
column read as a fallback when `status` is null, and a retry-with-sleep around the payment
webhook handler.

A weak review renames the enum values, extracts a `StatusHelper`, and adds tests. Every
concept survives. That is a failed pass.

The expected review:

**Real intent:** staff need to know what an order still needs — payment, shipping, or
nothing.

```
EVIDENCE   pending / awaiting_payment: every reader treats them alike (orders.ts:40, ui/badge.tsx:12)
           completed / done: same; `done` written only by an old import job (git log -S done → 2022)
           processing: written by the worker, read only by a log line
           is_cancelled: always set together with status = cancelled; 3 readers check either
           legacy_state: 0 rows with status IS NULL; fallback added for a 2021 migration
           sleep(2000): added for "webhook arrives before order row commits" (commit abc123)

REMOVE     legacy_state column and its fallback (after confirming no external writer)
MERGE      pending + awaiting_payment → awaiting_payment
           completed + done → completed
           paid + processing → paid
DERIVE     is_cancelled → status === 'cancelled'
FIX CAUSE  commit the order before publishing the payment intent; delete retry + sleep
QUESTION   "Is `new` different from `awaiting_payment` for the business — e.g. a draft
            the customer hasn't confirmed — or can it merge too?"
```

If the answer is "no difference":

```
awaiting_payment → paid → shipped → completed
        └──────────────┴──→ cancelled
```

Nine statuses, a boolean, a legacy column and a sleep become five states with transitions
written down in one place. The business behavior is unchanged; the model a reader must
learn shrank by half.
