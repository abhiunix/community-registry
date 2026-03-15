---
name: react-component-generator
description: "Generates production-ready React components with TypeScript, Tailwind CSS, proper accessibility, and test scaffolding. Use when asked to create a new React component."
license: MIT
allowed-tools: Read Glob Grep Edit Write
---

# React Component Generator

When asked to create a React component, follow this process:

## Step 1: Understand Requirements

Before generating code, clarify:
- Component name and purpose
- Props the component should accept
- Whether it needs internal state
- If it should fetch data or receive it via props

## Step 2: Generate Component

Create a functional component with:
- TypeScript interface for all props
- Tailwind CSS for styling (no CSS modules)
- Semantic HTML elements
- ARIA attributes for accessibility
- JSDoc comment describing the component

### Component Template

```typescript
interface ${ComponentName}Props {
  /** Description of each prop */
}

/** Brief description of what this component does */
export function ${ComponentName}({ ...props }: ${ComponentName}Props) {
  return (
    <div className="...">
      {/* Implementation */}
    </div>
  );
}
```

## Step 3: Add Tests

Generate a co-located test file (`ComponentName.test.tsx`) with:
- Render test (component mounts without errors)
- Props test (renders correctly with different props)
- Interaction test (click handlers, form inputs work)

## Rules

- Always use `function` declarations, not arrow functions, for components
- Always export components as named exports, not default
- Props interface must be exported separately
- Use `clsx` or template literals for conditional classes, not ternaries in className
- Never use `any` type — define proper types or use `unknown`
- Include `data-testid` attributes on interactive elements
