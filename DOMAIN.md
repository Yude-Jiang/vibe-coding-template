# DOMAIN.md — Business Domain Reference

This file captures domain terminology, business rules, and invariants that Claude
and contributors must understand to work correctly on this codebase.

---

## Glossary

<!-- TODO: Define the key nouns in your domain. One term per entry.

| Term | Definition |
|------|-----------|
| User | A registered account holder who can log in and create resources. |
| Workspace | An isolated environment owned by a User or Organisation. |
-->

| Term | Definition |
|------|-----------|
| _Example_ | _Replace this row with real domain terms._ |

---

## Core Business Rules

<!-- TODO: List invariants that must always hold true in the system.
Phrase them as assertions: "A X must always have Y" or "Z can only happen when W."

- An Order must always have at least one LineItem.
- Payment can only be captured after an Order is in `confirmed` state.
- A User cannot belong to more than one primary Organisation.
-->

- _Add business rules here._

---

## State Machines

<!-- TODO: For entities with lifecycle states, describe valid transitions.

### Order States
pending → confirmed → shipped → delivered
pending → cancelled
confirmed → cancelled (only before payment captured)
-->

---

## External Systems & Integrations

<!-- TODO: List third-party services and what role they play.

| System | Purpose | Docs |
|--------|---------|------|
| Stripe | Payment processing | https://stripe.com/docs |
| SendGrid | Transactional email | https://docs.sendgrid.com |
-->

---

## Data Ownership & Privacy

<!-- TODO: Describe which data is PII, retention policies, and deletion rules. -->

---

## Filling Guide

1. Replace every `<!-- TODO: ... -->` block with real content.
2. Keep entries short — a sentence or two per term is enough.
3. Update this file whenever a new domain concept is introduced in code.
4. Reference this file from `CLAUDE.md` so AI assistants load it automatically.
