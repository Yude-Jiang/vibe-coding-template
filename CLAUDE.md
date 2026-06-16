# CLAUDE.md

## Project Overview

<!-- TODO: Fill in a 2-3 sentence description of what this project does. -->

## General Constraints

### Code Style
- Prefer editing existing files over creating new ones.
- Default to writing no comments; only add one when the WHY is non-obvious.
- Do not add error handling for scenarios that cannot happen.
- No backwards-compatibility hacks for clearly unused code.

### Security
- Never introduce command injection, XSS, SQL injection, or other OWASP Top 10 vulnerabilities.
- Validate only at system boundaries (user input, external APIs).
- Never commit secrets, credentials, or `.env` files.

### Git
- Commit messages should be concise and explain the "why", not the "what".
- Never force-push to main/master.
- Never skip pre-commit hooks (`--no-verify`).

### Testing
- Run the test suite before reporting a task complete.
- Type checking and linters verify correctness; manual testing verifies behavior.

---

## Project-Specific Constraints

<!-- TODO: Fill in constraints specific to this project. Examples:
- Supported Node/Python/Go version
- Required environment variables
- Database migration rules
- Off-limits files or directories
-->

## Architecture

<!-- TODO: Describe the high-level architecture.
- Key directories and their purpose
- Data flow overview
- External dependencies and why they were chosen
-->

## Common Commands

<!-- TODO: Fill in the commands developers run day-to-day. Examples:

```bash
# Install dependencies
npm install

# Start dev server
npm run dev

# Run tests
npm test

# Lint
npm run lint
```
-->

## Key Files

<!-- TODO: List the most important files a new contributor should read first. -->

## Domain Knowledge

See [DOMAIN.md](./DOMAIN.md) for business-domain terminology and rules.

## Progress Tracking

See [PROGRESS.md](./PROGRESS.md) for current session state and task history.
