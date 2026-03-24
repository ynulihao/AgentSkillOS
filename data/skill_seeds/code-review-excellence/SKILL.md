---
name: code-review-excellence
description: "Use when reviewing pull requests, establishing code review standards, or mentoring developers through reviews. Provides systematic analysis covering correctness, security, performance, and maintainability while giving constructive, actionable feedback that fosters knowledge sharing."
---

# Code Review Excellence

Systematic code review that transforms gatekeeping into knowledge sharing through constructive feedback and collaborative improvement.

## Workflow

1. **Gather context** (2-3 min) - Read PR description, linked issue, check CI status, understand business requirement
2. **High-level review** (5-10 min) - Architecture fit, file organization, testing strategy
3. **Line-by-line review** (10-20 min) - Logic, security, performance, maintainability
4. **Summarize and decide** (2-3 min) - Key concerns, praise, clear verdict

## Review Scope

**Review manually**: Logic correctness, edge cases, security vulnerabilities, performance, test quality, error handling, API design, architectural fit

**Automate instead**: Formatting (Prettier/Black), import ordering, linting, simple typos

## Feedback Principles

- Be **specific and actionable**, not vague
- Focus on **code, not the person**
- **Educate** rather than judge
- **Praise** good patterns alongside concerns
- **Prioritize**: distinguish blocking issues from nits

### Severity Labels

Use labels so authors know what matters:
- `[blocking]` - Must fix before merge
- `[important]` - Should fix; discuss if disagree
- `[nit]` - Nice to have, not blocking
- `[suggestion]` - Alternative approach to consider
- `[praise]` - Good work worth highlighting

### Feedback Style

Ask questions instead of commanding:
- "What happens if `items` is empty?" (not "This will fail on empty lists")
- "Have you considered the Repository pattern here?" (not "Extract this into a service")
- "How should this behave if the API call fails?" (not "Add error handling")

## Security Checklist

- [ ] User input validated and sanitized
- [ ] SQL queries parameterized
- [ ] Auth/authz checks on all protected actions
- [ ] No hardcoded secrets
- [ ] Error messages don't leak internals
- [ ] File uploads restricted (size, type)
- [ ] CSRF protection on state-changing operations

## Performance Checklist

- [ ] No N+1 queries
- [ ] Database queries use indexes
- [ ] Large lists paginated
- [ ] Expensive operations cached
- [ ] No blocking I/O in hot paths

## Common Language-Specific Issues

### Python
- Mutable default arguments (`def f(items=[])` - use `None` instead)
- Bare `except:` catching everything (catch specific exceptions)
- Mutable class attributes shared across instances

### TypeScript/JavaScript
- Using `any` defeats type safety (define proper interfaces)
- Unhandled async errors (wrap in try/catch, check `response.ok`)
- Mutating props in React components

## PR Size Guidelines

- Ideal: 200-400 lines
- Over 400 lines: ask to split into focused PRs
- Large features: review in stages (interfaces first, then implementation, then integration)

## Summary Template

```markdown
## Summary
[Brief overview of what was reviewed]

## Strengths
- [Good patterns or approaches]

## Required Changes
[blocking] [Issue description]

## Suggestions
[suggestion] [Improvement idea]

## Verdict
Approve / Comment / Request Changes
```

## Best Practices

- Review within 24 hours, ideally same day
- Time-box reviews to 60 minutes; take breaks for large PRs
- Offer to pair on complex issues
- Know when to let go - working code with minor style differences is fine
- Avoid scope creep ("while you're at it...")
