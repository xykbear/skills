# Skill Template Reference

## Directory Structure

```
skills/
  {skill-name}/           # kebab-case directory name
    SKILL.md              # Required: skill definition
    .gitignore            # Optional: skill-specific ignores
    scripts/              # Optional: executable scripts
    references/           # Optional: cached data or reference docs
    evals/                # Optional: test cases
```

## Naming Conventions

- **Skill directory**: `kebab-case` (e.g., `fmhy-lookup`, `my-new-skill`)
- **SKILL.md**: Always uppercase, always this exact filename
- **Scripts**: `kebab-case.sh`
- **Evals**: `evals/evals.json`

## SKILL.md Format

```markdown
---
name: {skill-name}
description: {One sentence describing when to use this skill. Include trigger phrases.}
license: MIT              # Optional
---

# {Skill Title}

{Brief description of what the skill does.}

## How It Works

{Numbered list explaining the skill's workflow}

## Usage

{Show common usage patterns}

## Output Format

{Show example output format}
```

## Best Practices

- **Keep SKILL.md under 500 lines** — put detailed reference material in separate files
- **Write specific descriptions** — helps the agent know exactly when to activate the skill
- **Use progressive disclosure** — reference supporting files that get read only when needed
- **Cache external data in references/** — if the skill fetches data from the web, cache it locally

## Description Guidelines (Pushy)

Write "pushy" descriptions to ensure high trigger rate:
- Include common trigger phrases users might say
- Mention specific use cases
- Be specific about when this skill should activate

## Git Ignore

- Root `.gitignore`: common ignores (`node_modules/`, `*.pyc`, etc.)
- Each skill can have its own `.gitignore`: for skill-specific cache/workspace

## Development Patterns

- External data/cache stored in `references/` (excluded via skill's .gitignore)
- File/path resolution must use Glob tool
