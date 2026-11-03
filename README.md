# GUI skills

Agent skills for [GUI](https://gui.now) — HTML in, URL out.

One directory per skill. Copy the one you want into your agent's skills
directory, or clone the whole repo in place.

| Skill | What it does |
|-------|--------------|
| [`gui-now/`](gui-now/) | Turn generated HTML, Markdown or Mermaid into a shareable live URL instead of dumping it in chat |

## Install

**Claude Code plugin** (the skill plus the gui-now MCP server), in Claude Code:

```
/plugin install gui-now --marketplace gui-now/skills
```

On Claude Code before 2.1.275, add the marketplace first:

```
/plugin marketplace add gui-now/skills
/plugin install gui-now@gui-now
```

The plugin asks for an optional Pro API key when it's enabled. Leave it empty
for the free tier.

**Any agent that supports skills** (Claude Code, Cursor, Codex and others):

```bash
npx skills add gui-now/skills --skill gui-now -g
```

**Claude Code, skill only** — project-scoped:

```bash
mkdir -p .claude/skills
cp -r gui-now .claude/skills/
```

Or user-scoped, to make it available everywhere:

```bash
mkdir -p ~/.claude/skills
cp -r gui-now ~/.claude/skills/
```

## Pro keys

The skill reads `GUI_NOW_API_KEY` when it is set, which lifts the free tier's
limits (2 MB → 10 MB, 24h → up to 30 days, 3 edits → unlimited, 5 → 100
creates/hr) and enables password-protected canvases.

`GUI_NEW_API_KEY` is the pre-rename name and is still read as a fallback, so
keys exported before the move to gui.now keep working.

In OpenClaw the key is injected from `skills.entries.gui-now.apiKey`.

## License

MIT — see [LICENSE](LICENSE).
