# CQ Claude Toolkit

Shared Claude Code skills for the CQ Designs NY team.

Skills stored in `~/.claude/skills/` only exist on the machine that created them. A shared
Claude account shares login and billing, not files. This repo is a Claude Code **plugin
marketplace**, which is the mechanism that actually moves a skill between computers.

## Install (run once per machine)

In Claude Code, run:

```
/plugin marketplace add https://github.com/cqdesignsny/cq-claude-toolkit.git
```

```
/plugin install cq@cq-toolkit
```

Restart Claude Code. `/cq:startup` will appear in the slash command list.

Use the full `https://` URL, not the `cqdesignsny/cq-claude-toolkit` shorthand. The shorthand
makes Claude Code clone over SSH when the machine has an SSH key, which fails for anyone
whose key is not registered with GitHub. The HTTPS URL works on every machine, with or
without SSH, because the repo is public.

The same two commands work from a terminal without opening Claude Code:

```
claude plugin marketplace add https://github.com/cqdesignsny/cq-claude-toolkit.git && claude plugin install cq@cq-toolkit
```

## What's in it

| Command | What it does |
| --- | --- |
| `/cq:startup` | Reads the README and handoff doc in the current folder, checks git history for drift, posts a short "where we left off" briefing, then waits for direction. |

## Getting updates

When this repo changes, each machine picks it up with:

```
/plugin update cq@cq-toolkit
```

## Adding a skill to this plugin

1. Create `plugins/cq/skills/<name>/SKILL.md` with YAML frontmatter (`name`, `description`).
   The `description` is what Claude matches against, so write it as trigger conditions, not
   as a summary.
2. Optionally add `plugins/cq/commands/<name>.md` so it shows up in the `/` menu. Keep it
   thin: it should just tell Claude to invoke the skill.
3. Bump `version` in `plugins/cq/.claude-plugin/plugin.json`.
4. Commit and push. Teammates run `/plugin update cq@cq-toolkit`.

Anything the skill references must live in this repo. Absolute paths like
`/Users/<someone>/...` will break on every machine but the author's.

## Layout

```
.claude-plugin/marketplace.json     marketplace manifest, lists the plugins
plugins/cq/
  .claude-plugin/plugin.json        plugin manifest
  skills/startup/SKILL.md           the skill itself
  commands/startup.md               slash command entry point
```
