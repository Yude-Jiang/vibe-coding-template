# Skill: spec

Generate or update a functional specification for a feature.

## Trigger

Use when the user says: "write a spec for X", "spec out X", or "draft a spec".

## Behavior

1. Read `DOMAIN.md` and `CLAUDE.md` to understand domain rules and constraints.
2. **Identify reusable assets.** Scan the exploration file(s) in `docs/explorations/`,
   `DOMAIN.md`, and the user's instructions for references to existing code, skills,
   modules, or libraries that this feature is meant to **integrate rather than rewrite**.
   Examples: "复用 X skill 的 builder"、"基于 Y 模块构建"、"把 Z 转化成 Agent"、
   "wrap the existing API"。If any are found, list them in the spec's
   `## Hard Architectural Constraints` section (see Output Structure below) with:
   - The exact source file/path/module where the code lives
   - Which specific functions/classes/constants must be imported or extracted
   - An explicit statement that reimplementing equivalent functionality is **not allowed**
     — if integration has engineering obstacles (e.g. code is in markdown, not a .py module),
     the correct action during BUILD is to report the obstacle and wait for confirmation,
     not to silently write a substitute.
   If no reusable assets are identified, include the section header with "None identified"
   so BUILD-phase agents know they checked rather than forgot.
3. Ask clarifying questions if the feature scope is ambiguous (max 3 questions).
4. Write the spec to `docs/explorations/<feature-slug>.md` using the structure below.
5. Summarize what was written and what open questions remain.

## Output Structure

```markdown
# Spec: <Feature Name>

## Problem
<!-- One paragraph: what pain does this solve and for whom? -->

## Goals
- [ ] Goal 1
- [ ] Goal 2

## Non-Goals
- What this feature explicitly does NOT do.

## Hard Architectural Constraints
<!-- Step 2 output. List existing assets that MUST be integrated, not reimplemented.
     For each asset:
     - Source: exact file path or module (e.g. `st-ppt-brand/references/pptx-implementation.md`)
     - What to use: specific functions/classes/constants (e.g. `add_cards_row`, `ST_DARK_BLUE`)
     - Integration note: if the source is not directly importable (e.g. code in markdown,
       separate repo, different language), state this explicitly so BUILD knows to extract/
       convert first, not skip.
     If none: write "None identified — this feature is built from scratch." -->

## Proposed Solution
<!-- Narrative description + key design decisions. -->

## Data Model Changes
<!-- New or modified entities, fields, relations. -->

## API / Interface Changes
<!-- New endpoints, events, or UI surfaces. -->

## Open Questions
- [ ] Question 1
```

## Notes

- Keep specs in `docs/explorations/` until the feature is shipped.
- Reference the spec from the relevant GitHub issue if one exists.
- The `Hard Architectural Constraints` section exists to prevent a specific failure mode:
  BUILD-phase agents treating "reuse X" as "reimplement X's functionality from scratch"
  when they encounter integration friction. By listing exact file paths and function names,
  this section makes the difference between "integrate" and "reimplement" unambiguous.
  If BUILD encounters an obstacle integrating a listed asset, the correct action is to
  report it and wait — not to silently substitute a reimplementation.
