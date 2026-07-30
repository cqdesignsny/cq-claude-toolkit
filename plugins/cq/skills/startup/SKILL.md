---
name: startup
description: Universal session catch-up shortcut. In whatever folder the session is open, reads the README, the handoff doc if one exists (HANDOFF.md or similar), and recent git history, then posts a short "where we left off" briefing and waits for direction. Use whenever the user types /startup or /cq:startup, and also when they open a session asking to get caught up, resume where things left off, or read the README and the handoff.
---

# Startup: get acclimated, brief, wait

The user runs this at the start of a session to get Claude fully caught up on whatever project lives in the current folder. The output is a short briefing, not work. Do not start building anything until the user responds to the briefing.

## Find the docs

Work from the current working directory.

1. README: look for `README.md` (any capitalization). If there is no README, this folder is not set up for /startup. Say so, list the top-level contents so the user can orient, and stop.
2. Handoff: look for `HANDOFF.md` at the root first, then any top-level file with "handoff" in the name (case-insensitive). Projects using this pattern keep a running session log there. It is optional; the README alone is enough to brief from.

## Read them efficiently

- Read the README in full. If it is unusually large (over roughly 50KB), read the headings first, then the sections covering status and how to run the project.
- For the handoff, read the first 20 lines before anything else. These projects keep a resume pointer near the top (a "Resuming? Read ..." note naming the current recap and queued-task sections). If a pointer exists, read only the sections it names: find their line numbers with `grep -n "^## " <handoff file>`, then Read with offset and limit. If there is no pointer, read the whole file when it is small (under roughly 30KB), or just the final two sections when it is not. Handoffs grow every session; reading one end to end wastes context the actual work will need.

## Check for drift

If the folder is a git repository, run `git log --oneline -15` and `git status`. Commits newer than the handoff's last-updated date, or uncommitted changes sitting in the tree, mean the docs are stale. Say so in the briefing instead of trusting them blindly. Skip this step quietly if there is no repo.

## Brief, then wait

Keep it under about 250 words, plain prose or short bullets:

- **Where we left off:** what the docs say happened most recently, one or two lines.
- **Queued next:** whatever the handoff queues, with the key constraint attached (a target it has to hit, a file it plugs into, whatever gates it).
- **Blocked or open:** items waiting on someone else, briefly.
- **Drift or surprises:** uncommitted changes, commits after the last recap, anything that contradicts the docs. Leave this section out when there is nothing to report.

If exactly one task is queued, end by proposing to start it. Otherwise end by asking what to pick up.

## Style

No emojis, no em dashes, no hype words. Write like a teammate leaving a note.
