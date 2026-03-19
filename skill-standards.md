# Skill Development Standards

## Overview

This document defines the standards for skill development in this repository.

## Reference

For template format, see: @skill-template.md

---

## Why This Matters

### SKILL.md Reference Loading

Using `@` in SKILL.md triggers **greedy loading**:
- All `@` references are loaded when skill triggers
- This can cause unnecessary context bloat

Using `[text](path)` in SKILL.md enables **lazy loading**:
- Model decides when to load based on workflow phase
- Only relevant references are loaded when needed

### Layer/Grade Impact on Structure

| Layer | Structure | References |
|-------|-----------|------------|
| Atomic | Single file | None needed |
| Composite | Entry + references/ | By class |
| Workflow | Multi-layer | Per phase |

---

## Classification, Layering & Grading

### Layer (分层) - By Complexity

| Layer | Definition | Use Case |
|-------|------------|----------|
| Atomic | Single operation | File reading, formatting, simple transforms |
| Composite | Multiple atomic operations | MCP server, tool sets |
| Workflow | Multi-step with branching | Complex business logic |

### Class (分类) - By Function Type

| Class | Description |
|-------|-------------|
| `data-extraction` | Data extraction, parsing |
| `file-transform` | File format conversion |
| `code-generation` | Code generation, refactoring |
| `analysis` | Analysis, review, evaluation |
| `workflow` | Workflow orchestration |

### Grade (分级) - By Complexity Level

| Grade | Criteria |
|-------|----------|
| Grade 1 | Single function, minimal config, no variants |
| Grade 2 | 2-3 variants/frameworks, moderate complexity |
| Grade 3 | Multiple variants, edge cases, high complexity |

---

## File Reference Standards

### In SKILL.md

| Format | Loading Mode | Description |
|--------|--------------|-------------|
| `@file.md` | Greedy | All references loaded when skill triggers |
| `[text](path)` | Lazy (Recommended) | Model decides based on workflow phase |
| `plain-path` | No trigger | Text reference only |

### Recommended Pattern

**✅ Embedded Lazy Loading**
```markdown
## Phase 1: Research
When you need best practices: [📋 Best Practices](../refs/best-practices.md)

## Phase 2: Implementation
When you need code examples: [⚡ TypeScript Guide](../refs/typescript.md)
```

**❌ Not Recommended: Top-level Declaration**
```markdown
## References
- @refs/api.md    # Causes all references to load when skill triggers
- @refs/ts.md
```

---

## Directory Structure

### By Layer

```
# Atomic/Grade 1 - Simple Structure
skill-name/
├── SKILL.md              # Direct inclusion
└── (no references/)

# Composite/Grade 2 - Standard Structure
skill-name/
├── SKILL.md              # Entry + workflow
└── references/           # Grouped by class

# Workflow/Grade 3 - Layered Structure
skill-name/
├── SKILL.md              # Top-level coordination
├── layer-1/
│   ├── SKILL.md
│   └── references/
├── layer-2/
│   ├── SKILL.md
│   └── references/
└── shared/               # Shared resources
```

### Standard Structure

```
skill-name/
├── SKILL.md              # Entry point + workflow
├── scripts/              # Executable scripts
├── references/           # Reference documents
└── evals/               # Test cases
```

---

## Frontmatter Extension

```yaml
---
name: {skill-name}
description: {Trigger description with keywords and use cases}
layer: atomic|composite|workflow
class: data-extraction|file-transform|...
grade: 1|2|3
---
```

---

## Best Practices

1. **Keep SKILL.md under 500 lines** - Detailed content in references/
2. **Write specific descriptions** - Ensures correct skill triggering
3. **Use progressive disclosure** - Reference files loaded only when needed
4. **Cache external data in references/** - Local cache for web data

---

## Development Workflow

When creating or modifying skills:

1. Determine layer/class/grade
2. Follow the appropriate directory structure
3. Use embedded references in workflow sections
4. Test with realistic prompts
