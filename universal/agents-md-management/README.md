# AGENTS.md Management Plugin

Tools to maintain and improve AGENTS.md files - audit quality, capture session learnings, and keep project memory current.

`CLAUDE.md` is treated only as a compatibility bridge (typically an `@AGENTS.md` pointer or a symlink to `AGENTS.md`). This plugin never writes to CLAUDE.md — AGENTS.md is always the canonical file.

## What It Does

Two complementary tools for different purposes:

| | agents-md-improver (skill) | /revise-agents-md (command) |
|---|---|---|
| **Purpose** | Keep AGENTS.md aligned with codebase | Capture session learnings |
| **Triggered by** | Codebase changes | End of session |
| **Use when** | Periodic maintenance | Session revealed missing context |

## Usage

### Skill: agents-md-improver

Audits AGENTS.md files against current codebase state:

```
"audit my AGENTS.md files"
"check if my AGENTS.md is up to date"
```

<img src="agents-md-improver-example.png" alt="AGENTS.md improver showing quality scores and recommended updates" width="600">

### Command: /revise-agents-md

Captures learnings from the current session:

```
/revise-agents-md
```

<img src="revise-agents-md-example.png" alt="Revise command capturing session learnings into AGENTS.md" width="600">

## Author

Zeroone Team
