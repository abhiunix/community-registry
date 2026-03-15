# Contributing to AgentDock Community Registry

## Adding a Skill

Skills follow the [agentskills.io](https://agentskills.io/specification) specification. Each skill is a directory under `skills/`.

### Directory Structure

```
skills/
└── your-skill-name/
    ├── SKILL.md              # Required — entry point
    ├── scripts/              # Optional — helper scripts
    ├── references/           # Optional — reference docs
    └── assets/               # Optional — templates, data files
```

### SKILL.md Format

```markdown
---
name: your-skill-name
description: "What this skill does and when to use it. Be specific."
license: MIT
allowed-tools: Read Glob Grep Edit Write
---

# Your Skill Name

Instructions for the AI agent go here. Write clear, step-by-step
guidance for what the agent should do when this skill is invoked.

## When to Use
Describe the trigger conditions.

## Steps
1. First, do this...
2. Then, do that...

## Rules
- Constraints and guardrails
```

### Frontmatter Fields

| Field | Required | Description |
|-------|----------|-------------|
| `name` | Yes | Lowercase, hyphens only, max 64 chars. Must match directory name. |
| `description` | Yes | What the skill does AND when to use it. Max 1024 chars. |
| `license` | No | License name (e.g., `MIT`, `Apache-2.0`). |
| `allowed-tools` | No | Space-separated list of tools the skill can use. |

### Naming Rules

- Directory name must match the `name` field in SKILL.md
- Use lowercase letters, numbers, and hyphens only (`a-z`, `0-9`, `-`)
- No leading, trailing, or consecutive hyphens
- 1-64 characters

### Good Example

See [skills/react-component-generator/](skills/react-component-generator/) for a multi-file skill with references.

---

## Adding a Capability (MCP, Rule, Hook)

Capabilities are JSON files under `registry/capabilities/{type}/`.

### MCP Server

```json
{
  "id": "yourname/my-mcp",
  "name": "My MCP Server",
  "description": "What this MCP server provides",
  "version": "1.0.0",
  "author": "yourname",
  "tags": ["relevant", "tags"],
  "visibility": "public",
  "adapters": ["claude-code", "cursor", "windsurf"],
  "mcp": {
    "command": "npx",
    "args": ["-y", "@scope/package-name"],
    "env": [
      {
        "key": "API_KEY",
        "label": "Description shown to user",
        "required": true
      }
    ]
  }
}
```

### Rule

```json
{
  "id": "yourname/my-rule",
  "name": "My Rule",
  "description": "What this rule enforces",
  "version": "1.0.0",
  "author": "yourname",
  "tags": ["quality"],
  "visibility": "public",
  "adapters": ["claude-code", "cursor", "windsurf"],
  "rule": {
    "scope": "project",
    "content": "## Rule Title\n\n- Rule point 1\n- Rule point 2"
  }
}
```

### Hook

```json
{
  "id": "yourname/my-hook",
  "name": "My Hook",
  "description": "What this hook does and when it triggers",
  "version": "1.0.0",
  "author": "yourname",
  "tags": ["automation"],
  "visibility": "public",
  "adapters": ["claude-code", "cursor"],
  "hook": {
    "trigger": "file_save",
    "command": "your-command --args"
  }
}
```

---

## Adding an Agent

Agents are Markdown files under `registry/agents/`.

```markdown
---
id: yourname/my-agent
name: My Agent
description: What this agent specializes in
author: yourname
version: 1.0.0
model: sonnet
color: blue
memory: project
tags: [tag1, tag2]
---

System prompt goes here. Describe the agent's role, expertise,
and how it should behave.
```

---

## ID Format

All contributions use the `github-username/name` format:

- `octocat/github-mcp`
- `johndoe/react-component-generator`

Your GitHub username as the author prevents naming conflicts and provides attribution.

## Submission Process

1. Fork the repository
2. Add your skill/capability/agent in the appropriate directory
3. Test it locally with AgentDock
4. Submit a Pull Request with a description of what you added and how you tested it

## Quality Guidelines

- **Test thoroughly**: Verify it works with all listed adapters
- **Clear descriptions**: Help users understand what they're deploying
- **Minimal permissions**: Only request necessary env vars and tools
- **Proper tagging**: Use relevant, searchable tags
