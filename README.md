# AgentDock Community Registry

Community-contributed capabilities, skills, and agents for [AgentDock](https://github.com/abhijeetsingh/agentdock).

## Structure

```
community-registry/
├── skills/                          # Skills (agentskills.io spec)
│   ├── react-component-generator/
│   │   ├── SKILL.md                 # Entry point with frontmatter
│   │   └── references/              # Supporting files
│   │       └── patterns.md
│   ├── api-endpoint-scaffolder/
│   │   └── SKILL.md
│   └── git-commit-reviewer/
│       └── SKILL.md
├── registry/
│   ├── capabilities/                # Other capabilities (JSON format)
│   │   ├── mcps/                    # MCP server definitions
│   │   │   ├── github-mcp.json
│   │   │   └── postgres-mcp.json
│   │   ├── rules/                   # Coding rules
│   │   │   └── general-code-review.json
│   │   └── hooks/                   # Event-triggered automation
│   │       └── auto-test-runner.json
│   └── agents/                      # Agent definitions (.md)
│       ├── code-reviewer.md
│       ├── api-designer.md
│       └── fullstack-developer.md
├── CONTRIBUTING.md
└── README.md
```

## Skills

Skills follow the [agentskills.io](https://agentskills.io/specification) specification. Each skill is a directory with a `SKILL.md` entry point.

| Skill | Description |
|-------|-------------|
| [react-component-generator](skills/react-component-generator/) | Generates React components with TypeScript, Tailwind, accessibility, and tests |
| [api-endpoint-scaffolder](skills/api-endpoint-scaffolder/) | Scaffolds REST API endpoints with validation, error handling, and tests |
| [git-commit-reviewer](skills/git-commit-reviewer/) | Reviews staged changes for bugs, security issues, and code quality |

## MCP Servers

| Name | Description |
|------|-------------|
| [github-mcp](registry/capabilities/mcps/github-mcp.json) | GitHub repository access, issues, PRs, and code search |
| [postgres-mcp](registry/capabilities/mcps/postgres-mcp.json) | PostgreSQL database queries and schema inspection |

## Rules

| Name | Description |
|------|-------------|
| [general-code-review](registry/capabilities/rules/general-code-review.json) | Lightweight checklist for consistent code reviews |

## Hooks

| Name | Description |
|------|-------------|
| [auto-test-runner](registry/capabilities/hooks/auto-test-runner.json) | Runs relevant tests automatically when files are modified |

## Agents

| Name | Description |
|------|-------------|
| [code-reviewer](registry/agents/code-reviewer.md) | Reviews code for quality, security, and best practices |
| [api-designer](registry/agents/api-designer.md) | Designs RESTful and GraphQL APIs |
| [fullstack-developer](registry/agents/fullstack-developer.md) | End-to-end web development with React, Node.js, and databases |

## Contributing

See [CONTRIBUTING.md](CONTRIBUTING.md) for how to add your own skills, capabilities, and agents.
