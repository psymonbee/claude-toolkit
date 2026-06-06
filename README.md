# claude-toolkit

### Simple tools for newbie vibe coders

I can't write code, but I can run Claude — and I've found these really helpful to keep my crazy
brain on track.

My aim is creating tools that help me track and maintain all the bits I don't really know.

enjoy 🙂

---

## ...the boring bit follows

These are [Claude Code](https://claude.com/claude-code) plugins. Add the catalogue once, then
install whichever ones you want.

### Add the catalogue

```
/plugin marketplace add psymonbee/claude-toolkit
```

Then browse and install:

```
/plugin                              # open the plugin menu to browse
/plugin install orient@claude-toolkit
```

Update later with `/plugin marketplace update claude-toolkit`.

### What's in here

| Plugin | What it does |
|---|---|
| [orient](plugins/orient) | Keeps a note, per project, of where you left each thing you were working on — so when you come back days later you remember what was in your head, instead of figuring it all out again. Used as `/orient:orient`. |

> Plugin commands get a name prefix, so it's `/orient:orient`, not plain `/orient`. If you'd
> rather just type `/orient`, copy `plugins/orient/commands/orient.md` into your own project's
> `.claude/commands/` folder instead — same tool, no install.

---

### For me (maintainer notes)

Each plugin is a folder under `plugins/`:

```
plugins/<name>/
├── .claude-plugin/plugin.json     # name, description, version, author
├── commands/<name>.md             # a slash command  (and/or)
├── skills/<name>/SKILL.md         # a skill
└── README.md
```

Then add one line to [`.claude-plugin/marketplace.json`](.claude-plugin/marketplace.json):

```json
{ "name": "<name>", "source": "./<name>", "description": "..." }
```

(`metadata.pluginRoot` is `./plugins`, so `source` is relative to that.)

Commit and push — anyone who's added the catalogue gets it on their next
`/plugin marketplace update`.

Keep it general. Anything codebase-specific or niche doesn't belong here.
