# pascal-skills

Claude Code skills I actually use, published in case they are useful to someone else. MIT.

| Skill | What it does |
| --- | --- |
| [`simplify-product`](plugins/simplify-product) | Reviews a feature, screen or flow by first principles — remove, automate, combine, default and defer, and only then redesign. Starts by asking whether the screen should exist at all. |

## Install

As a plugin, which is one command per skill and updates with the repo:

```
/plugin marketplace add mohammedpascal/pascal-skills
/plugin install simplify-product@pascal-skills
```

Or just take the file — a skill is a single Markdown file and nothing about it needs a
plugin:

```sh
git clone https://github.com/mohammedpascal/pascal-skills.git
cp -r pascal-skills/plugins/simplify-product/skills/simplify-product ~/.claude/skills/
```

Put it in a project's `.claude/skills/` instead of `~/.claude/skills/` to scope it to that
repo.

## Using simplify-product

Ask for it by name, or describe the problem and let it trigger:

```
/simplify-product
this settings screen has grown to fourteen rows, have a look
```

It takes a screenshot, a description, a PRD, a wireframe, a list of controls — anything
that describes what the user is faced with. It answers with the real user goal, the flow
underneath the UI, what to remove/automate/combine/default/defer, and only then a proposed
screen.

It is deliberately willing to tell you the feature should not exist.

## What this isn't

These are personal skills, published because they might be useful — not a supported
product. No release schedule, no compatibility promise. Fork it and make it yours; that is
what the licence is for.

## Adding a skill here

Notes to self, mostly:

- Only skills I wrote. Anything installed by another tool belongs to whoever wrote it —
  link upstream instead of copying it in.
- Frontmatter is `name`, `description` and `license`. Nothing else; Claude Code does not
  read the other keys that float around, and putting them in teaches people wrong.
- No client names, internal hostnames, absolute paths, or examples that give away
  unreleased work.
- Self-contained: no references to my other skills or local setup.
- `claude plugin validate .`, then install it from a clean checkout before pushing.

## Licence

MIT — see [LICENSE](LICENSE).
