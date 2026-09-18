# simplify-product

A Claude Code skill that simplifies a product feature, screen or user flow by first
principles.

It does not open by redesigning what you showed it. It asks what the user is actually
trying to accomplish, rebuilds the minimum flow without any UI, and runs a fixed pass —
**remove, automate, combine, default, defer, clarify** — before it is allowed to propose a
screen. A prettier version of a cluttered screen counts as a failed pass.

Give it a screenshot, a screen description, a PRD, a user story, a wireframe, or just a
list of the controls on a screen.

## Install

```
/plugin marketplace add mohammedpascal/pascal-skills
/plugin install simplify-product@pascal-skills
```

Or copy [`skills/simplify-product`](skills/simplify-product) into `~/.claude/skills/`.

## What comes back

The user goal in one sentence, the mental model the current design demands, the flow
underneath the UI, where the complexity comes from, the remove/automate/combine/default/
defer lists, a simplified experience, a proposed minimum screen, a before → after, and a
test to challenge whatever complexity survived.

MIT. Part of [pascal-skills](https://github.com/mohammedpascal/pascal-skills).
