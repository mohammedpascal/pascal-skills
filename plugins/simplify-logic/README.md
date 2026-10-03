# simplify-logic

A Claude Code skill that simplifies code logic by first principles.

Projects drift. Two statuses end up meaning the same thing, a workaround outlives its bug,
a compatibility path stays behind for a version nobody runs. This skill does not open by
refactoring what you showed it. It asks what the code is actually for, writes the minimum
model (states, transitions, invariants) without code, backs every finding with callers,
data and git history, and asks you the intent questions it can't answer itself. Then it
runs a fixed pass — **remove, merge, derive, inline, fix root cause, rename** — before it
is allowed to restructure anything. A tidier version of a tangled model counts as a failed
pass.

It changes no code until you've confirmed what the code is meant to do.

Give it a file, a module, a domain concept ("order status"), a schema, a diff, or a bug
that keeps coming back.

## Install

```
/plugin marketplace add mohammedpascal/pascal-skills
/plugin install simplify-logic@pascal-skills
```

Or copy [`skills/simplify-logic`](skills/simplify-logic) into `~/.claude/skills/`.

## What comes back

The real intent in one sentence, the current model and the minimum model, the sources of
complexity with `file:line` evidence, the remove/merge/derive/inline/fix/rename lists,
questions for you, a migration plan that leaves no legacy path behind, a before → after,
and a test to challenge whatever complexity survived.

Sibling of [simplify-product](../simplify-product), which does the same for the user
experience.

MIT. Part of [pascal-skills](https://github.com/mohammedpascal/pascal-skills).
