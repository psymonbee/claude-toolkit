# orient

A `/orient` slash command for Claude Code that keeps a **per-thread cold-start orientation**
for a project.

## The problem it solves

You work on something, get part-way, then have to go do another thing — because things depend
on other things. Days later you come back and can't remember what was in your head when you
left. Asking "what's the state of this?" gives you a *fresh* answer through the lens of today's
intentions. What you actually want is the **headspace you walked away with**, for that one
specific thread.

`orient` stores one frozen snapshot per workstream so you can step back into a single thread
cold — what you were mid-thought on, the next physical step, the questions you hadn't resolved.

It is deliberately **separate from any project memory / build log**. Memory is searchable
history. `orient` is "where was my head when I left *this* thread."

## Install

```
/plugin marketplace add psymonbee/claude-toolkit
/plugin install orient@claude-toolkit
```

Once installed it's invoked as **`/orient:orient`** (plugin commands are namespaced by the
plugin name).

> Prefer plain `/orient` with no install step? Copy `commands/orient.md` from this folder into
> your project's `.claude/commands/orient.md` (or `~/.claude/commands/` for all projects). Same
> command, no namespace.

## Usage

| Command | What it does |
|---|---|
| `/orient:orient` | The **index** — every thread you have going, freshest first, with status + how stale each is. |
| `/orient:orient <topic>` | Pull back **one thread's snapshot** — replayed as "where you left it / next step / open questions". |
| `/orient:orient set [topic]` | **Freeze** the current thread into its own entry. Omit the topic and it infers it from the chat. |
| `/orient:orient done <topic>` | Mark a thread finished so it drops off the active list (kept as a record). |

## What it writes

In whatever repo you run it (nothing is created until your first `set`):

```
ORIENTATION.md            ← the index: thread table + any shared project basics
orientation/
  <slug>.md               ← one frozen snapshot per workstream
```

Each entry carries `status` (active | parked | done), an `updated` date, and a one-line
summary, plus a body: **Where I left it → Done (don't redo) → Next step → Open questions →
Depends on** (which links other threads, for when work blocks on work).

Commit these files and they travel with the repo — so the same orientation is there on your
laptop, your phone, or the web, anywhere the repo is checked out.
