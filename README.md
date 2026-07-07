# zeroone-marketplace

Repositório pessoal de plugins, skills e MCPs customizados para o ecossistema Zeroone.gg.

**Filosofia**: 
- Plugins **universais** que funcionam em múltiplos CLIs (Grok Build, Claude Code, Codex, Antigravity/AGY etc.)
- Plugins **específicos** otimizados para uma ferramenta em particular quando necessário.

## Como adicionar este marketplace

### Grok Build

```bash
grok plugin marketplace add Felipenogue91/zeroone-marketplace
```

Depois instale os plugins:

```bash
grok plugin install agents-md-management --trust
```

Ou pela TUI:
- `/plugins` → aba Marketplace → `a` → `Felipenogue91/zeroone-marketplace`

### Claude Code

```bash
/plugin marketplace add Felipenogue91/zeroone-marketplace
/plugin install agents-md-management
```

### Outras ferramentas (Codex, AGY, etc.)

A maioria suporta adicionar via git ou caminho local apontando para este repositório.

## Categorias de Plugins

```
zeroone-marketplace/
├── universal/          # Funcionam em múltiplos CLIs
├── grok/               # Otimizados para Grok Build
├── claude-code/        # Mais específicos para Claude Code
├── codex/              # Específicos para Codex
├── agy/                # Específicos para Antigravity
└── mcps/               # Servidores MCP (geralmente mais portáteis)
```

## Plugins atuais

### Universal
- **agents-md-management** — Versão customizada focada em AGENTS.md (CLAUDE.md tratado apenas como ponte de compatibilidade).

## Estrutura de cada plugin

```
plugin-name/
├── .claude-plugin/
│   └── plugin.json
├── skills/
│   └── skill-name/
│       └── SKILL.md
├── commands/
│   └── meu-comando.md
├── README.md
└── (opcional) .mcp.json
```

## Como contribuir / adicionar novo plugin

1. Escolha a categoria certa (`universal/` é preferível quando possível).
2. Copie a estrutura do template em `template/`.
3. Crie um bom `README.md` dentro do plugin explicando:
   - O que faz
   - Quais CLIs são suportados
   - Como usar
4. Atualize este README principal.
5. Abra PR ou envie o commit.

## Notas de Compatibilidade

- **Skills puras** (SKILL.md) são as mais portáteis entre Grok, Claude Code, Codex e AGY.
- Hooks complexos e agentes específicos geralmente funcionam melhor em um CLI só.
- MCPs costumam ser mais independentes da ferramenta.
- Sempre declare a compatibilidade no README do plugin.

## Licença

Uso livre para projetos pessoais e da equipe Zeroone.
