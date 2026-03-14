[English](./README.md) | [中文](./README.zh-CN.md)

---

# xykbear-skills

Personal AI Agent Skills collection for AI coding assistants.

## Installation

```bash
npx skills add xykbear/skills
```

Or manually:

```bash
git clone https://github.com/xykbear/skills.git
cp -r skills/{skill-name} ~/.agents/skills/
```

## Available Skills

### fmhy-lookup

Search FMHY (Free Media Heck Yeah) wiki for free tools, software, and site recommendations.

**Use when:**
- Finding free tools, software, apps, sites, or resource recommendations
- Looking for video downloaders, VPNs, ad blockers, password managers, proxy tools, download utilities, etc.

**Examples:**
- "Find me a free video downloader"
- "What are some good password managers?"
- "Recommend some free proxy tools"

### prompt-creator

Help users create, optimize, and document system prompts (AGENTS.md).

**Use when:**
- User wants to document/persist conversation experience into rules
- Asking about refining system prompts
- Wanting to optimize/review existing agents.md configuration

**Examples:**
- "Document this session's learnings into my agents.md"
- "Review my current AGENTS.md for improvements"
- "What can be extracted from this conversation?"

## Usage

Skills are automatically available once installed. The agent will use them when relevant tasks are detected.

Simply describe your needs naturally - no special commands required.

## License

MIT License - See [LICENSE.txt](./LICENSE.txt)
