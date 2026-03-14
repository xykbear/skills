# AGENTS.md

This file provides guidance to AI coding agents when working with code in this repository.

## Repository Overview

A collection of skills for AI coding agents (OpenCode, Claude, Cursor, etc.). Skills are packaged instructions that extend agent capabilities.

## Publishing to skills.sh

### Installation

```bash
npx skills add xykbear/skills
```

Or manually:

```bash
git clone https://github.com/xykbear/skills.git
# Copy specific skill to agent's skills directory
cp -r skills/{skill-name} ~/.agents/skills/
```

### Publishing Checklist

Before releasing a new skill:

1. **Update README.md** - Add new skill to "Available Skills" section
2. **Update README.zh-CN.md** - Add Chinese translation
3. Test skill locally
4. Commit and push

### Skill Template

Read @skill-template.md when:
- Creating a new skill
- Modifying existing skill structure
- Having questions about the standards

### License

MIT License - See LICENSE.txt

### Git Ignore

- Root `.gitignore`: common ignores (`node_modules/`, `*.pyc`, etc.)
- Each skill can have its own `.gitignore`: for skill-specific cache/workspace
