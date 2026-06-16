# Vibe Coding Template

Template for Loop Engineering workflows: CI quality gates, KPI weekly reporting, deploy smoke checks, and rollback runbook.

## Quick Start

```bash
git init
npm install
npm run quality:gate
```

## Included assets

- `.github/workflows/ci.yml`
- `docs/loop-kpi.schema.json`
- `docs/deploy-runbook.md`
- `scripts/contract-check.ts`
- `scripts/weekly-status-report.ts`
- `scripts/post-deploy-smoke.ts`
- `tests/template.test.ts`

## Core Commands

```bash
npm run lint
npm run test
npm run coverage
npm run contract:check
npm run quality:gate
npm run kpi:report
npm run smoke -- --url=https://<service-url>
```

## Notes

- `contract:check` validates `metadata.json` and `firebase-blueprint.json`.
- `kpi:report` writes markdown reports into `reports/`.
- `smoke` checks homepage and optional `/healthz` endpoint.
