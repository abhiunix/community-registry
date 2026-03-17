---
name: git-commit-reviewer
description: "Reviews staged git changes and provides feedback on code quality, potential bugs, and security issues before committing. Use when asked to review changes or before creating a commit."
license: MIT
allowed-tools: Read Glob Grep Bash
---

# Git Commit Reviewer

When asked to review changes or before a commit, follow this process:

## Step 1: Gather Changes

Run `git diff --cached --stat` to see which files are staged, then `git diff --cached` for the full diff.

If nothing is staged, check `git diff` for unstaged changes and let the user know.

## Step 2: Analyze Each File

For each changed file, check:

### Code Quality
- Are variable/function names descriptive?
- Is there duplicated logic that should be extracted?
- Are functions small and focused?
- Is error handling present where needed?

### Potential Bugs
- Off-by-one errors in loops or array access
- Null/undefined not handled
- Race conditions in async code
- Missing `await` on async calls
- Incorrect comparison operators (`==` vs `===`)

### Security
- No secrets, API keys, or tokens in the diff
- User input is validated before use
- No SQL injection or XSS vectors introduced
- File paths are sanitized if user-provided

### Dependencies
- New dependencies are intentional and from trusted sources
- No unnecessary dependency additions
- Lock file is updated consistently

## Step 3: Provide Feedback

Structure your review as:

```
## Summary
One-line overview of the changes.

## Issues Found
### Critical (must fix)
- ...

### Suggestions (nice to have)
- ...

## Looks Good
- Positive observations about the changes
```

## Rules

- Be constructive — explain why something is an issue, not just that it is
- If no issues found, say so clearly and briefly
- Don't nitpick style if the project has a formatter configured
- Focus on logic and correctness over formatting
- If the diff is large (>500 lines), summarize by file group rather than line-by-line
