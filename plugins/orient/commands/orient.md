---
description: Per-thread cold-start orientation — snapshot or recall where you left a workstream (set | get | done | <topic>)
argument-hint: "[<topic> | set <topic> | done <topic> | get]   (no arg = index)"
allowed-tools: Read, Edit, Write, Bash(git status:*), Bash(git log:*), Bash(git diff:*), Bash(ls:*)
---

You manage this project's **orientation entries** — frozen snapshots of the user's headspace
when they walk away from a workstream, so days or weeks later they can step back into *that
specific thread* without reconstructing it from scratch.

A project has many threads going at once (e.g. a feature, a refactor, a migration). Each thread
gets its own snapshot. This is deliberately NOT the same as asking "what's the state of the
project now" — the user wants to recover the *mindset they left with*, not a fresh analysis
filtered through today's intentions.

## Layout

- `orientation/<slug>.md` — one entry per workstream. The frozen headspace snapshot.
- `ORIENTATION.md` (repo root) — the **index**: any shared project basics (how to run, gotchas,
  key paths) written once, plus an auto-maintained list of every entry with status +
  last-updated + a one-line summary. You keep this list in sync whenever you `set` or `done`.

Each entry has frontmatter:
```
---
slug: pricing
title: Layer 2 — Pricing
status: parked          # active | parked | done
updated: 2025-01-15
summary: one-liner shown in the index
---
```

The user invoked: `/orient $ARGUMENTS`

Parse `$ARGUMENTS` (case-insensitive first word):

- empty, `get`, `list`, `index`  → **INDEX mode**
- `set [topic]`                  → **SET mode** (topic = the rest of the args)
- `done [topic]`                 → **DONE mode**
- anything else                  → **RECALL mode** (the whole arg string is the topic query)

Use today's date (it's provided in your context) wherever an entry needs `updated` set or
staleness judged. Don't hardcode a date.

---

## INDEX mode

Show the user what threads they have parked.

1. Read `ORIENTATION.md`. If it doesn't exist, say so and offer to start the first entry with
   `/orient set <topic>` (SET mode will scaffold the index + `orientation/` folder).
2. Present the index conversationally — lead with the freshest / `active` thread so they
   re-orient fast, then list the rest with status and how stale each is (relative to today).
   Don't dump full entries — this is the menu, not the meal.
3. Remind them they can pull any one back with `/orient <topic>`.

## RECALL mode

The user wants one specific thread's snapshot back.

1. `ls orientation/` and read `ORIENTATION.md`'s index to see what entries exist.
2. Fuzzy-match the topic query against entry slugs, titles, and summaries.
   - **One clear match** → read `orientation/<slug>.md` and replay it conversationally.
     Lead with **where they left it** (the headspace), then the next physical step, then open
     questions. Note the `updated` date so they know how stale it is. If the entry depends on
     another thread, mention it and point to that entry.
   - **Several plausible matches** → list them briefly and ask which.
   - **No match** → say so plainly and show the index so they can pick.
3. Do NOT start doing the next-step work unless the user asks — recall is for re-orienting only.

## SET mode

Freeze the current chat's thread into its own entry. The goal: future-them reads it cold and
recovers exactly the mindset they're leaving with right now.

1. Determine the thread:
   - If a topic was given, use it. Match it to an existing `orientation/<slug>.md` to UPDATE,
     or create a new entry if none fits.
   - If no topic, infer the workstream from *this chat*. If genuinely ambiguous, ask which
     thread this snapshot is for before writing.
2. Gather real state — don't guess:
   - Read the existing entry if updating (preserve good content; this is an update).
   - Run `git status`, `git log --oneline -10`, `git diff --stat` for commits / uncommitted work.
   - Reflect on what *this chat* actually did, decided, and deliberately parked — that
     conversational context is the part git can't recover and is the whole point.
3. Write `orientation/<slug>.md` with the frontmatter block, then a skimmable, honest body —
   adapt headings to what's true, but cover:
   - **Where I left it** — the headspace: what you were mid-thought on, why you stopped here.
   - **Done in this thread (don't redo)** — so future-you doesn't repeat finished work.
   - **Next step (pick up here)** — the very next action, with breadcrumbs: files to open,
     commands to run, decisions still open.
   - **Open questions / decisions hanging** — what you hadn't resolved yet.
   - **Depends on / blocked by** — other threads this waits on; link them as `orientation/<slug>.md`.
   Set `updated` to today and `status` (active|parked|done).
4. Update `ORIENTATION.md`'s index: add or refresh this entry's line (status, updated, summary).
   Keep any shared "project basics" section intact — only touch the entry list. If `ORIENTATION.md`
   doesn't exist yet, create it with a short index table and (optionally) a project-basics section.
5. Give a 2–3 line summary of what the snapshot captured and confirm the file path.

## DONE mode

Mark a thread finished so it drops off the active list.

1. Match the topic to an entry (as in RECALL). If ambiguous/missing, ask or show the index.
2. Set that entry's `status: done` and refresh `updated`. Leave the body as the record.
3. Update its line in `ORIENTATION.md`'s index. Confirm in one line.

---

Be accurate over comprehensive — a wrong orientation is worse than a short one. If unsure
whether something was finished vs parked, say so in the entry rather than asserting it's done.
