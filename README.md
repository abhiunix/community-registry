# AgentDock Community Registry

A curated collection of capabilities, skills, and agents for AI coding assistants. Used by [AgentDock](https://github.com/abhijeetsingh/agentdock) to deploy configurations to Claude Code, Cursor, and Windsurf.

## Repository structure

```
community-registry/
├── registry.json          # Master index with totals and categories
├── schemas/               # JSON Schema definitions
│   └── metadata.schema.json
├── skills/                # SKILL.md files with metadata
│   ├── index.json
│   └── development/
│       ├── git-commit-reviewer/
│       ├── api-endpoint-scaffolder/
│       └── react-component-generator/
├── mcps/                  # MCP server definitions
│   ├── index.json
│   └── devtools/
│       ├── github-mcp.json
│       └── postgres-mcp.json
├── rules/                 # Rule definitions
│   ├── index.json
│   └── code-quality/
│       └── general-code-review.json
├── hooks/                 # Hook definitions
│   ├── index.json
│   └── testing/
│       └── auto-test-runner.json
└── agents/                # Agent definitions (.md + .json pairs)
    ├── index.json
    ├── code-review/
    │   └── code-reviewer
    ├── architecture/
    │   └── api-designer
    └── development/
        └── fullstack-developer
```

## Entry types

| Type | Description | Format |
|------|-------------|--------|
| **Skills** | Domain-specific instructions for AI agents | `SKILL.md` + `metadata.json` |
| **MCPs** | Model Context Protocol server configurations | Single `.json` file |
| **Rules** | Lightweight text directives injected into agent context | Single `.json` file |
| **Hooks** | Commands triggered by events (file save, pre-commit) | Single `.json` file |
| **Agents** | AI sub-agents with system prompts and configuration | `.md` + `.json` pair |

## How it works

1. AgentDock fetches `registry.json` to discover available entries.
2. Each type folder has an `index.json` with summary data for browsing.
3. Individual metadata files contain full configuration for deployment.
4. AgentDock translates entries into IDE-specific formats (Claude Code, Cursor, Windsurf).

## Skills

| Skill | Category | Description |
|-------|----------|-------------|
| [Git Commit Reviewer](skills/development/git-commit-reviewer/) | development | Reviews staged changes for bugs, security issues, and code quality |
| [API Endpoint Scaffolder](skills/development/api-endpoint-scaffolder/) | development | Scaffolds REST API endpoints with validation, error handling, and tests |
| [React Component Generator](skills/development/react-component-generator/) | development | Generates React components with TypeScript, Tailwind, accessibility, and tests |

## MCP Servers

| Name | Category | Description |
|------|----------|-------------|
| [GitHub MCP](mcps/devtools/github-mcp.json) | devtools | GitHub repository access, issues, PRs, and code search |
| [PostgreSQL MCP](mcps/devtools/postgres-mcp.json) | devtools | PostgreSQL database queries and schema inspection |

## Rules

| Name | Category | Description |
|------|----------|-------------|
| [General Code Review](rules/code-quality/general-code-review.json) | code-quality | Lightweight checklist for consistent code reviews |

## Hooks

| Name | Category | Description |
|------|----------|-------------|
| [Auto Test Runner](hooks/testing/auto-test-runner.json) | testing | Runs relevant tests automatically when files are modified |

## Agents

| Name | Category | Description |
|------|----------|-------------|
| [Code Reviewer](agents/code-review/) | code-review | Reviews code for quality, security, and best practices |
| [API Designer](agents/architecture/) | architecture | Designs RESTful and GraphQL APIs |
| [Full Stack Developer](agents/development/) | development | End-to-end web development with React, Node.js, and databases |

## Contributing

See [CONTRIBUTING.md](CONTRIBUTING.md) for how to add your own skills, capabilities, and agents.

## License

All entries in this registry are individually licensed. See each entry's `license` field. The registry structure itself is MIT licensed.
