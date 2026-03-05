# AgentDock Community Registry Examples

This folder contains example capabilities and agents that demonstrate how to create content for AgentDock registries. These are reference examples and do not affect the app build.

## Structure

```
community-registry/
└── registry/
    ├── agents/                    # AI agents for specific tasks
    │   ├── fullstack-developer.md
    │   ├── code-reviewer.md
    │   └── api-designer.md
    └── capabilities/              # Reusable capabilities organized by type
        ├── hooks/                 # File change triggers
        │   └── auto-test-runner.json
        ├── mcps/                  # Model Context Protocol servers
        │   ├── github-mcp.json
        │   └── postgres-mcp.json
        ├── plugins/               # IDE plugins (empty)
        ├── rules/                 # Coding rules (empty)
        └── skills/                # Development skills
            └── react-component-generator.json
```

## Capabilities

### MCP Servers
- **GitHub MCP**: Repository access, issues, PRs, and code search
- **PostgreSQL MCP**: Database queries and schema inspection

### Skills
- **React Component Generator**: TypeScript + Tailwind CSS components

### Hooks
- **Auto Test Runner**: Automatically runs tests on file changes

## Agents

### Development Agents
- **Full-Stack Developer**: End-to-end web development
- **Code Reviewer**: Quality, security, and best practices review
- **API Designer**: RESTful and GraphQL API architecture

## Adding New Content

See [CONTRIBUTING.md](CONTRIBUTING.md) for guidelines on how to add new capabilities and agents to this private registry.

## Usage

These are reference examples. To use them:

1. Copy the JSON/MD files to your actual registry
2. Or use them as templates to create your own capabilities and agents
3. The actual registry sync happens through the configured GitHub repository in AgentDock settings

## Purpose

This folder serves as:
- Reference examples for capability and agent formats
- Templates for creating new content
- Documentation of the expected structure
- Local examples that don't affect the app build