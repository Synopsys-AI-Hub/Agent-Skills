# Synopsys AI Skills

> Reusable instruction sets for AI coding assistants working on Synopsys projects — brand guidelines, Python coding style, and more.

Built on the open [Agent Skills](https://agentskills.io) standard. Compatible with **Claude Code**, **GitHub Copilot**, **Cursor**, **Windsurf**, **Cline**, and any AI tool that reads markdown instruction files.

---

## What Are Skills?

Skills are folders containing a `SKILL.md` file — structured markdown instructions that AI assistants load on demand for specialised tasks. Instead of repeating context in every prompt, skills give the model the right standards, conventions, and constraints automatically.

Each skill in this repository encodes Synopsys-specific knowledge: brand colors and design tokens, PyAnsys Python coding conventions, and formatting tool configurations. They ensure that AI-generated code and content stays on-brand and on-spec across teams and tools.

**Learn more:**
- [Agent Skills specification](https://agentskills.io)
- [What are skills?](https://docs.anthropic.com/en/docs/agents-and-tools/claude-code/skills)
- [Creating custom skills](https://docs.anthropic.com/en/docs/agents-and-tools/claude-code/skills#creating-custom-skills)

## Skills in This Repository

| Skill | Description |
|---|---|
| [`synopsys-brand`](./skills/synopsys-brand/) | Synopsys brand colors, typography, gradients, design tokens, and usage rules for any visual output. |
| [`pyansys-python-style`](./skills/pyansys-python-style/) | PyAnsys Python coding conventions — PEP 8 rules, naming, Ruff/pre-commit config, docstrings, and deprecation patterns. |

Each skill has a `SKILL.md` file with YAML frontmatter (`name`, `description`) followed by detailed markdown instructions.

> **Note:** These skills encode Synopsys internal conventions and should be reviewed by your team before adoption. Test them in your workflow before relying on them in production.

## Repository Structure

```
.
├── README.md
├── skills/
│   ├── synopsys-brand/
│   │   └── SKILL.md
│   └── pyansys-python-style/
│       └── SKILL.md
└── template/
    └── SKILL.md
```

## Using These Skills

### Claude Code (CLI)

Place the skill folders in your project's `.claude/skills/` directory or in `~/.claude/skills/` for global use:

```bash
# Project-level
cp -r skills/synopsys-brand .claude/skills/

# Global (applies to all projects)
cp -r skills/synopsys-brand ~/.claude/skills/
```

Claude loads matching skills automatically based on the task context.

### Claude.ai (Web & Desktop)

Skills can be added to your Claude.ai account under **Settings → Skills**. Upload the `SKILL.md` files or use the skill creator to paste the content. Once saved, Claude applies them when relevant keywords or tasks match the skill description.

### VS Code — GitHub Copilot

GitHub Copilot reads instruction files from your repository. Convert skills to Copilot's format:

```bash
# Create the instructions directory
mkdir -p .github/copilot

# Copy skill content as a Copilot instruction file
cp skills/pyansys-python-style/SKILL.md .github/copilot/pyansys-python-style.md
cp skills/synopsys-brand/SKILL.md .github/copilot/synopsys-brand.md
```

Then reference them in `.github/copilot-instructions.md`:

```markdown
## Coding Standards
Follow the PyAnsys Python coding style defined in `.github/copilot/pyansys-python-style.md`.

## Brand Guidelines
Follow the Synopsys brand guidelines defined in `.github/copilot/synopsys-brand.md`.
```

Copilot will include these instructions in its context for Chat, Edits, and code completions.

### VS Code — Cursor

Cursor reads project rules from a `.cursor/rules/` directory:

```bash
mkdir -p .cursor/rules

cp skills/pyansys-python-style/SKILL.md .cursor/rules/pyansys-python-style.mdc
cp skills/synopsys-brand/SKILL.md .cursor/rules/synopsys-brand.mdc
```

Cursor loads `.mdc` files automatically as project context. You can also add rules globally via **Cursor Settings → Rules**.

### VS Code — Windsurf

Windsurf reads a `.windsurfrules` file in your project root:

```bash
# Append skill content to the rules file
cat skills/pyansys-python-style/SKILL.md >> .windsurfrules
cat skills/synopsys-brand/SKILL.md >> .windsurfrules
```

### VS Code — Cline

Cline reads a `.clinerules` file in your project root:

```bash
cat skills/pyansys-python-style/SKILL.md >> .clinerules
cat skills/synopsys-brand/SKILL.md >> .clinerules
```

### VS Code — Continue

Continue reads a `.continuerules` file or config in `.continue/`:

```bash
mkdir -p .continue

cp skills/pyansys-python-style/SKILL.md .continue/pyansys-python-style.md
cp skills/synopsys-brand/SKILL.md .continue/synopsys-brand.md
```

### Any Other AI Tool

The `SKILL.md` files are standard markdown. For any AI assistant that accepts system instructions or context files, paste or reference the skill content directly. The frontmatter (`name`, `description`) helps tools route the right skill to the right task.

## Creating a New Skill

A skill is a folder with a `SKILL.md` file. The file starts with YAML frontmatter followed by markdown instructions:

```markdown
---
name: my-new-skill
description: One sentence describing when this skill should be used.
---

# My New Skill

## Overview

Instructions the AI assistant should follow...
```

**Required frontmatter fields:**
- `name` — A kebab-case identifier for the skill.
- `description` — When to apply this skill. This is what AI tools match against to decide whether to load it.

Use the [`template/SKILL.md`](./template/SKILL.md) as a starting point.

**Tips for writing effective skills:**
- Be specific and concrete — include exact values (hex codes, line lengths, config blocks).
- Include code examples for patterns the AI should follow.
- Organise with clear headings so the model can find relevant sections quickly.
- Keep the description focused — a skill that tries to cover everything matches nothing well.

## Multi-Model Compatibility

These skills are written as plain markdown, which makes them model-agnostic. They work with:

| AI Tool | Model(s) | Integration Method |
|---|---|---|
| Claude Code | Claude | `.claude/skills/` directory |
| Claude.ai | Claude | Settings → Skills |
| GitHub Copilot | GPT-4o, Claude, Gemini | `.github/copilot/` instructions |
| Cursor | Claude, GPT-4o, custom | `.cursor/rules/` directory |
| Windsurf | Claude, GPT-4o | `.windsurfrules` file |
| Cline | Claude, GPT-4o, custom | `.clinerules` file |
| Continue | Any OpenAI-compatible | `.continue/` directory |
| Anthropic API | Claude | Skills API parameter |

The Agent Skills standard ensures portability. Write once, use across any AI-assisted workflow.

## Contributing

1. Fork this repository.
2. Create a new skill folder under `skills/` with a `SKILL.md` file.
3. Follow the existing skills as a reference for structure and depth.
4. Open a pull request with a description of what the skill covers and when it should trigger.

## License

Copyright © 2024–2026 Synopsys, Inc. All rights reserved.

The brand guidelines skill contains Synopsys proprietary brand information. The Python coding style skill is derived from the publicly available [PyAnsys Developer's Guide](https://dev.docs.pyansys.com/).
