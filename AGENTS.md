# AGENTS.md

This file provides guidance to AI coding agents when working with code in this repository.

## Repository Overview

A collection of skills for AI coding agents (OpenCode, Claude, Cursor, etc.). Skills are packaged instructions that extend the agent's capabilities.

## Creating a New Skill

### Directory Structure

```
skills/
  {skill-name}/           # kebab-case directory name
    SKILL.md              # Required: skill definition
    scripts/              # Optional: executable scripts
    references/           # Optional: cached data or reference docs
    evals/                # Optional: test cases
    .gitignore            # Optional: skill-specific ignores
```

### Naming Conventions

- **Skill directory**: `kebab-case` (e.g., `fmhy-lookup`, `my-new-skill`)
- **SKILL.md**: Always uppercase, always this exact filename
- **Scripts**: `kebab-case.sh`
- **Evals**: `evals/evals.json`

### SKILL.md Format

```markdown
---
name: {skill-name}
description: {One sentence describing when to use this skill. Include trigger phrases.}
---

# {Skill Title}

{Brief description of what the skill does.}

## When to Use

{List of trigger phrases and contexts}

## Workflow

{Numbered list explaining the skill's workflow}

## Output Format

{Show example output format}
```

### Best Practices

- **Keep SKILL.md under 500 lines** — put detailed reference material in separate files
- **Write specific descriptions** — helps the agent know exactly when to activate the skill
- **Use progressive disclosure** — reference supporting files that get read only when needed
- **Cache external data in references/** — if the skill fetches data from the web, cache it locally

### Git Ignore

- Root `.gitignore`:通用忽略 (`node_modules/`, `*.pyc`, etc.)
- Each skill can have its own `.gitignore`: for skill-specific cache/workspace

### End-User Installation

```bash
npx skills add xykbear/skills
```

Or manually:

```bash
git clone https://github.com/xykbear/skills.git
cp -r skills/{skill-name} ~/.claude/skills/
```
