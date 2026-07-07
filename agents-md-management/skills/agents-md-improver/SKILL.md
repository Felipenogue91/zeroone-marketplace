---
name: agents-md-improver
description: Audit and improve AGENTS.md files in repositories. Use when user asks to check, audit, update, improve, or fix AGENTS.md (or CLAUDE.md, which is treated as a compatibility bridge to AGENTS.md). Scans for all AGENTS.md/AGENTS.local.md files, evaluates quality against templates, outputs quality report, then makes targeted updates. Also use when the user mentions "AGENTS.md maintenance" or "project memory optimization".
tools: Read, Glob, Grep, Bash, Edit
---

# AGENTS.md Improver

Audit, evaluate, and improve AGENTS.md files across a codebase to ensure agentic coding tools have optimal project context.

**This skill can write to AGENTS.md files.** After presenting a quality report and getting user approval, it updates AGENTS.md files with targeted improvements.

**CLAUDE.md is never edited by this skill.** In this ecosystem, `CLAUDE.md` exists only as a compatibility bridge for Claude Code — it typically contains `@AGENTS.md` or otherwise points at AGENTS.md. AGENTS.md is always the canonical, operational memory file.

## Workflow

### Phase 1: Discovery

Find all AGENTS.md files in the repository:

```bash
find . -name "AGENTS.md" -o -name "AGENTS.local.md" 2>/dev/null | head -50
```

**File Types & Locations:**

| Type | Location | Purpose |
|------|----------|---------|
| Project root | `./AGENTS.md` | Primary project context (checked into git, shared with team) — canonical, operational memory |
| Local overrides | `./AGENTS.local.md` | Personal/local settings (gitignored, not shared) |
| Package-specific | `./packages/*/AGENTS.md` | Module-level context in monorepos |
| Subdirectory | Any nested location | Feature/domain-specific context |

**Note:** Also check for `CLAUDE.md` bridge files, but only to validate them — never to edit them:

```bash
find . -name "CLAUDE.md" 2>/dev/null | head -50
```

For each `CLAUDE.md` found:
- If it contains `@AGENTS.md` (an import/pointer), just confirm that `AGENTS.md` exists alongside it. Report the pointer as valid or broken — do not write to the file.
- If it's a symlink to `AGENTS.md`, resolve it and treat the target `AGENTS.md` as the file to audit/edit — still edit through the real path, not the symlink.
- If a `CLAUDE.md` exists with no pointer to AGENTS.md at all, flag it in the report as "bridge missing" — a note for the user to fix, not something this skill fixes automatically.

**Note on multi-tool memory files:** Claude Code also has its own global preference file (`~/.claude/CLAUDE.md`) for user-wide settings unrelated to any single project. That file is Claude Code's own configuration mechanism, not a project AGENTS.md, and is out of scope for this skill.

### Phase 2: Quality Assessment

For each AGENTS.md file, evaluate against quality criteria. See [references/quality-criteria.md](references/quality-criteria.md) for detailed rubrics.

**Quick Assessment Checklist:**

| Criterion | Weight | Check |
|-----------|--------|-------|
| Commands/workflows documented | High | Are build/test/deploy commands present? |
| Architecture clarity | High | Can the agent understand the codebase structure? |
| Non-obvious patterns | Medium | Are gotchas and quirks documented? |
| Conciseness | Medium | No verbose explanations or obvious info? |
| Currency | High | Does it reflect current codebase state? |
| Actionability | High | Are instructions executable, not vague? |

**Quality Scores:**
- **A (90-100)**: Comprehensive, current, actionable
- **B (70-89)**: Good coverage, minor gaps
- **C (50-69)**: Basic info, missing key sections
- **D (30-49)**: Sparse or outdated
- **F (0-29)**: Missing or severely outdated

### Phase 3: Quality Report Output

**ALWAYS output the quality report BEFORE making any updates.**

Format:

```
## AGENTS.md Quality Report

### Summary
- Files found: X
- Average score: X/100
- Files needing update: X
- CLAUDE.md bridges found: X (valid: X, missing/broken: X)

### File-by-File Assessment

#### 1. ./AGENTS.md (Project Root)
**Score: XX/100 (Grade: X)**

| Criterion | Score | Notes |
|-----------|-------|-------|
| Commands/workflows | X/20 | ... |
| Architecture clarity | X/20 | ... |
| Non-obvious patterns | X/15 | ... |
| Conciseness | X/15 | ... |
| Currency | X/15 | ... |
| Actionability | X/15 | ... |

**Issues:**
- [List specific problems]

**Recommended additions:**
- [List what should be added]

**Bridge status:** `./CLAUDE.md` present and points to AGENTS.md via `@AGENTS.md` (valid) / missing / broken

#### 2. ./packages/api/AGENTS.md (Package-specific)
...
```

### Phase 4: Targeted Updates

After outputting the quality report, ask user for confirmation before updating.

**Update Guidelines (Critical):**

1. **Propose targeted additions only** - Focus on genuinely useful info:
   - Commands or workflows discovered during analysis
   - Gotchas or non-obvious patterns found in code
   - Package relationships that weren't clear
   - Testing approaches that work
   - Configuration quirks

2. **Keep it minimal** - Avoid:
   - Restating what's obvious from the code
   - Generic best practices already covered
   - One-off fixes unlikely to recur
   - Verbose explanations when a one-liner suffices

3. **Show diffs** - For each change, show:
   - Which AGENTS.md file to update
   - The specific addition (as a diff or quoted block)
   - Brief explanation of why this helps future sessions

**Diff Format:**

```markdown
### Update: ./AGENTS.md

**Why:** Build command was missing, causing confusion about how to run the project.

```diff
+ ## Quick Start
+
+ ```bash
+ npm install
+ npm run dev  # Start development server on port 3000
+ ```
```
```

### Phase 5: Apply Updates

After user approval, apply changes using the Edit tool, targeting AGENTS.md (or AGENTS.local.md) directly. Preserve existing content structure. Do not write to CLAUDE.md under any circumstances — if a CLAUDE.md bridge is missing or broken, report it and let the user decide how to fix it.

## Templates

See [references/templates.md](references/templates.md) for AGENTS.md templates by project type.

## Common Issues to Flag

1. **Stale commands**: Build commands that no longer work
2. **Missing dependencies**: Required tools not mentioned
3. **Outdated architecture**: File structure that's changed
4. **Missing environment setup**: Required env vars or config
5. **Broken test commands**: Test scripts that have changed
6. **Undocumented gotchas**: Non-obvious patterns not captured
7. **Broken CLAUDE.md bridge**: A `CLAUDE.md` that doesn't point to AGENTS.md (missing `@AGENTS.md` import or equivalent)

## User Tips to Share

When presenting recommendations, remind users:

- **`#` key shortcut**: During a Claude Code session, pressing `#` auto-captures a learning — if the project's `CLAUDE.md` is just a pointer (`@AGENTS.md`), make sure the captured note lands in AGENTS.md, not duplicated into the bridge file.
- **Keep it concise**: AGENTS.md should be human-readable; dense is better than verbose
- **Actionable commands**: All documented commands should be copy-paste ready
- **Use `AGENTS.local.md`**: For personal preferences not shared with team (add to `.gitignore`)
- **Claude Code global defaults**: User-wide Claude Code preferences (not project memory) still live in `~/.claude/CLAUDE.md` — that's a separate mechanism from project AGENTS.md files

## What Makes a Great AGENTS.md

**Key principles:**
- Concise and human-readable
- Actionable commands that can be copy-pasted
- Project-specific patterns, not generic advice
- Non-obvious gotchas and warnings

**Recommended sections** (use only what's relevant):
- Commands (build, test, dev, lint)
- Architecture (directory structure)
- Key Files (entry points, config)
- Code Style (project conventions)
- Environment (required vars, setup)
- Testing (commands, patterns)
- Gotchas (quirks, common mistakes)
- Workflow (when to do what)
