# plugin-template

**Template oficial** do zeroone-marketplace.

Copie esta pasta inteira e renomeie para criar um novo plugin.

## Estrutura recomendada

```
meu-plugin/
├── .claude-plugin/
│   └── plugin.json
├── skills/
│   └── nome-da-skill/
│       └── SKILL.md
├── commands/
│   └── meu-comando.md
├── README.md
└── (opcional) .mcp.json
```

## Dicas

- Prefira skills puras para máxima compatibilidade entre CLIs.
- Documente claramente a compatibilidade (Grok, Claude Code, Codex, AGY).
- Mantenha o foco em AGENTS.md quando possível.
