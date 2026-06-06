# claude-toolkit

A growing catalogue of general-purpose [Claude Code](https://claude.com/claude-code) commands
and skills — the ones that aren't tied to any single codebase. Install the marketplace once,
then pick the plugins you want.

## Install the catalogue

```
/plugin marketplace add psymonbee/claude-toolkit
```

Then browse and install:

```
/plugin                              # open the plugin UI to browse
/plugin install orient@claude-toolkit
```

Update later with `/plugin marketplace update claude-toolkit`.

## Plugins

| Plugin | What it does |
|---|---|
| [orient](plugins/orient) | Per-thread cold-start orientation — snapshot and recall the headspace you left each workstream with, so you can step back into one thread cold. Invoked as `/orient:orient`. |

## Adding a new plugin to the catalogue

Each plugin is a self-contained folder under `plugins/`:

```
plugins/<name>/
├── .claude-plugin/plugin.json     # manifest: name, description, version, author
├── commands/<name>.md             # a slash command  (and/or)
├── skills/<name>/SKILL.md         # a skill
└── README.md
```

Then add one entry to [`.claude-plugin/marketplace.json`](.claude-plugin/marketplace.json):

```json
{ "name": "<name>", "source": "./<name>", "description": "..." }
```

(`metadata.pluginRoot` is set to `./plugins`, so `source` is relative to that.)

Commit and push — anyone who has added this marketplace gets the new plugin on their next
`/plugin marketplace update`.

## Notes

- Plugin commands are **namespaced** by plugin name, so they're invoked as
  `/<plugin>:<command>` (e.g. `/orient:orient`).
- Anything codebase-specific or niche does **not** belong here — keep this catalogue general.
