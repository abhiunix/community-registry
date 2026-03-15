---
name: api-endpoint-scaffolder
description: "Scaffolds REST API endpoints with route handler, validation, error handling, and tests. Use when asked to create a new API endpoint or route."
license: MIT
allowed-tools: Read Glob Grep Edit Write Bash
---

# API Endpoint Scaffolder

When asked to create a new API endpoint, follow this process:

## Step 1: Detect Framework

Read the project to determine the framework in use:
- **Express/Fastify** — look for `app.get()`, `router.post()`, `package.json` deps
- **Next.js App Router** — look for `app/api/` directory
- **Next.js Pages Router** — look for `pages/api/` directory
- **Hono/Elysia** — check `package.json`

Match the existing patterns and conventions in the codebase.

## Step 2: Scaffold the Endpoint

Generate these files for each endpoint:

### Route Handler
- Parse and validate request body/params/query using Zod or the project's validation library
- Return proper HTTP status codes (200, 201, 400, 404, 500)
- Include error handling with descriptive messages
- Add TypeScript types for request and response

### Validation Schema
```typescript
import { z } from "zod";

export const createUserSchema = z.object({
  name: z.string().min(1).max(100),
  email: z.string().email(),
});

export type CreateUserInput = z.infer<typeof createUserSchema>;
```

### Response Format
Always use a consistent response envelope:
```json
{
  "success": true,
  "data": { ... }
}
```
```json
{
  "success": false,
  "error": { "message": "...", "code": "VALIDATION_ERROR" }
}
```

## Step 3: Add Tests

Create a test file covering:
- Happy path (valid request returns expected response)
- Validation errors (invalid input returns 400)
- Not found (missing resource returns 404)
- Authentication (if endpoint is protected)

## Rules

- Never hardcode secrets or connection strings
- Always validate all user input before processing
- Use parameterized queries for database operations, never string concatenation
- Log errors but don't expose internal details to the client
- Follow the existing project conventions for file placement and naming
