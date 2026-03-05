# Contributing to AgentDock Registry

This guide explains how to contribute capabilities and agents to the community registry.

## Directory Structure

```
registry/
├── capabilities/
│   ├── mcps/         # MCP server definitions
│   ├── rules/        # Rule definitions
│   ├── hooks/        # Hook definitions
│   ├── skills/       # Skill definitions
│   └── plugins/      # Plugin definitions
└── agents/           # Agent templates
```

## Capability Schema

### MCP Server

```json
{
  "id": "octocat/github-mcp",
  "name": "GitHub MCP Server",
  "description": "Clear description of what this MCP server does",
  "author": "octocat",
  "version": "1.0.0",
  "visibility": "public",
  "mcp": {
    "command": "npx",
    "args": ["-y", "@modelcontextprotocol/server-github"],
    "env": [
      {
        "key": "GITHUB_TOKEN",
        "label": "GitHub Personal Access Token",
        "required": true
      }
    ]
  },
  "tags": ["github", "vcs", "api"],
  "adapters": ["claude-code", "cursor", "windsurf"]
}
```

### Rule

```json
{
  "id": "johndoe/typescript-strict",
  "name": "TypeScript Strict Mode",
  "description": "Enforces strict TypeScript coding standards",
  "author": "johndoe",
  "version": "1.0.0",
  "visibility": "public",
  "rule": {
    "content": "Always use strict TypeScript...",
    "globs": ["**/*.ts", "**/*.tsx"]
  },
  "tags": ["typescript", "style", "strict"],
  "adapters": ["claude-code", "cursor"]
}
```

## Agent Schema

Agent files are Markdown with YAML frontmatter:

```markdown
---
id: janedev/code-reviewer
name: Code Reviewer
description: Reviews code for quality, security, and best practices
author: janedev
version: 1.0.0
model: sonnet
color: purple
memory: project
tags: [code-review, quality, security]
---

# Code Reviewer Agent

You are a code reviewer. When reviewing code:
- Check for bugs and logical errors
- Verify security best practices
- Suggest performance improvements
- Ensure code follows project conventions
```

## Submission Process

1. Fork the repository
2. Add your capability/agent in the appropriate directory
3. Test locally by running AgentDock
4. Submit a Pull Request with:
   - Description of the capability/agent
   - How you tested it
   - Any dependencies or requirements

## Naming Conventions

### ID Format (Required)

All capabilities and agents must use the `github_username/name` format:

- **author**: Your GitHub username (required, must match your GitHub account)
- **name**: A unique, descriptive kebab-case name for your capability

**Examples:**
- `octocat/github-mcp` (for user "octocat")
- `johndoe/react-component-generator` (for user "johndoe")

**Why GitHub username?** Using your GitHub username as the author segment:
- Prevents naming conflicts between contributors
- Provides clear attribution and ownership
- Allows for accountability and contact

### Other Conventions

- **Files**: Match the capability name (e.g., `github-mcp.json`)
- **Tags**: Use lowercase, common terms
- **Names**: Use descriptive, human-readable names

## Environment Variables

For secrets, use the `${VAR_NAME}` syntax:

```json
{
  "env": {
    "API_KEY": "${MY_API_KEY}"
  }
}
```

Users will be prompted to enter these values during deployment.

## Quality Guidelines

1. **Test thoroughly**: Verify the capability works with all listed adapters
2. **Clear descriptions**: Help users understand what they're deploying
3. **Minimal permissions**: Only request necessary env vars
4. **Proper tagging**: Use relevant, searchable tags

## Examples

See existing capabilities in the `registry/` directory for reference implementations.
