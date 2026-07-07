# zeroone-marketplace

Personal marketplace with custom plugins, skills, and MCPs for Zeroone.gg.

This repo is meant to be added as a marketplace source in Grok Build, Claude Code, and compatible tools.

## How to add this marketplace

### Grok Build

```bash
grok plugin marketplace add Felipenogue91/zeroone-marketplace
```

Then install plugins:

```bash
grok plugin install agents-md-management --trust
```

Or use the TUI:
- `/plugins` → Marketplace tab → `a` to add → `Felipenogue91/zeroone-marketplace`

### Claude Code

```bash
/plugin marketplace add Felipenogue91/zeroone-marketplace
/plugin install agents-md-management
```

## Plugins included

- `agents-md-management` — Custom version focused on maintaining AGENTS.md (with CLAUDE.md treated only as compatibility bridge).

## Structure

Each plugin follows the standard format:

```
plugin-name/
├── .claude-plugin/
│   └── plugin.json
├── skills/
│   └── skill-name/
│       └── SKILL.md
├── commands/
│   └── some-command.md
└── README.md
```

## Contributing / Using

Feel free to use these in your own setups. If you want to fork or suggest improvements, open an issue or PR.
