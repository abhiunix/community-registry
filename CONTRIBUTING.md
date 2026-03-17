# Contributing to the AgentDock Community Registry

Thank you for contributing. This guide explains how to add new entries to the registry.

## General rules

- One entry per pull request.
- All JSON files must be valid and use 2-space indentation.
- Metadata must conform to `schemas/metadata.schema.json`.
- Use kebab-case for file and folder names.
- Include a clear, concise description (under 200 characters).
- Tag entries with relevant keywords for discoverability.

## Adding a skill

1. Pick or create a category folder under `skills/` (e.g., `skills/development/`).
2. Create a folder named after your skill: `skills/<category>/<skill-name>/`.
3. Add your `SKILL.md` file with YAML frontmatter (`name`, `description`, `license`, `allowed-tools`).
4. Add a `metadata.json` file following the schema. Required fields: `name`, `display_name`, `description`, `category`, `version`, `tags`, `compatible_adapters`, `license`.
5. If your skill has reference files, place them in a `references/` subfolder.
6. Update `skills/index.json` to include your entry.

### SKILL.md format

```markdown
---
name: your-skill-name
description: "What this skill does and when to use it."
license: MIT
allowed-tools: Read Glob Grep Edit Write
---

# Your Skill Name

Instructions for the AI agent go here.

## Steps
1. First, do this...
2. Then, do that...

## Rules
- Constraints and guardrails
```

## Adding an MCP server

1. Pick or create a category folder under `mcps/` (e.g., `mcps/devtools/`).
2. Create a `<name>.json` file with the full MCP configuration:

```json
{
  "name": "my-mcp",
  "display_name": "My MCP Server",
  "description": "What this MCP server provides",
  "category": "devtools",
  "author": "yourname",
  "author_github": "yourgithub",
  "version": "1.0.0",
  "tags": ["relevant", "tags"],
  "compatible_adapters": ["claude-code", "cursor", "windsurf"],
  "transport": "stdio",
  "command": "npx",
  "args": ["-y", "@scope/package-name"],
  "env": {
    "API_KEY": {
      "type": "secret",
      "label": "${API_KEY}",
      "required": true
    }
  },
  "source": {
    "repo": "org/repo",
    "url": "https://github.com/org/repo"
  },
  "stats": {
    "github_stars": 0,
    "updated_at": "2026-01-01"
  },
  "license": "MIT"
}
```

3. Update `mcps/index.json` to include your entry.

## Adding a rule

1. Pick or create a category folder under `rules/` (e.g., `rules/code-quality/`).
2. Create a `<name>.json` file with `scope` and `content` fields:

```json
{
  "name": "my-rule",
  "display_name": "My Rule",
  "description": "What this rule enforces",
  "category": "code-quality",
  "author": "yourname",
  "version": "1.0.0",
  "tags": ["quality"],
  "compatible_adapters": ["claude-code", "cursor", "windsurf"],
  "scope": "project",
  "content": "## Rule Title\n\n- Rule point 1\n- Rule point 2",
  "source": { "repo": "", "url": "" },
  "stats": { "github_stars": 0, "updated_at": "2026-01-01" },
  "license": "MIT"
}
```

3. Update `rules/index.json` to include your entry.

## Adding a hook

1. Pick or create a category folder under `hooks/` (e.g., `hooks/testing/`).
2. Create a `<name>.json` file with `trigger` and `command` fields:

```json
{
  "name": "my-hook",
  "display_name": "My Hook",
  "description": "What this hook does and when it triggers",
  "category": "testing",
  "author": "yourname",
  "version": "1.0.0",
  "tags": ["automation"],
  "compatible_adapters": ["claude-code", "cursor"],
  "trigger": "file_save",
  "command": "your-command --args",
  "source": { "repo": "", "url": "" },
  "stats": { "github_stars": 0, "updated_at": "2026-01-01" },
  "license": "MIT"
}
```

3. Update `hooks/index.json` to include your entry.

## Adding an agent

1. Pick or create a category folder under `agents/` (e.g., `agents/code-review/`).
2. Create a `<name>.json` metadata file with `model`, `color`, `memory`, and other fields.
3. Create a `<name>.md` file with the agent's description and system prompt.
4. Update `agents/index.json` to include your entry.

## Updating registry.json

After adding any entry, update the root `registry.json` to reflect the new totals and categories.

## Metadata schema

All metadata files should conform to `schemas/metadata.schema.json`. Key required fields:

| Field | Type | Description |
|-------|------|-------------|
| `name` | string | Unique kebab-case identifier |
| `display_name` | string | Human-readable name |
| `description` | string | Short description (max 200 chars) |
| `category` | string | Category folder name |
| `version` | string | Semantic version (e.g., `1.0.0`) |
| `tags` | array | Searchable keywords |
| `license` | string | SPDX license identifier |

## Submission process

1. Fork the repository.
2. Add your entry in the appropriate directory.
3. Update the relevant `index.json` and `registry.json`.
4. Test it locally with AgentDock.
5. Submit a pull request describing what you added and how you tested it.

## Quality guidelines

- Test thoroughly with all listed adapters.
- Write clear descriptions so users understand what they are deploying.
- Request only necessary environment variables and tool permissions.
- Use relevant, searchable tags.
