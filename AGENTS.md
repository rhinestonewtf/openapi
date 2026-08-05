# openapi

Published OpenAPI specs for Rhinestone's public HTTP APIs. Specs-only repo: no
package manifest, no build, no runtime code.

## Specs are generated, never hand-edited

Every JSON file here is a build artifact copied in from its source repo. Editing
a spec in this repo fixes nothing — the next bot PR overwrites it. Change the
endpoint definition upstream instead:

| File                                        | Source repo                 | Generator                             |
| ------------------------------------------- | --------------------------- | ------------------------------------- |
| `orchestrator/blanc.json` (`2026-04.blanc`) | `orchestrator`              | `pnpm generate-openapi`               |
| `orchestrator/alps.json` (`2026-01.alps`)   | `orchestrator`              | `pnpm generate-openapi`               |
| `deposit-service.json`                      | `deposit-service-processor` | `bun run scripts/generate-openapi.ts` |

Each source repo's `.github/workflows/openapi.{yml,yaml}` regenerates on push to
`main`, force-pushes a fixed branch (`update/orchestrator-v1`,
`update/deposit-processor`), and opens or updates a rolling PR here. Never base
work on those branches — they are force-pushed. The review of that PR's diff
_is_ the API change review.

## Commands

- `npx --yes @redocly/cli lint --extends minimal orchestrator/*.json deposit-service.json` — same check CI runs (`.github/workflows/validate.yml`, PRs into `main`)

`--extends minimal` means findings are warnings, not failures. The specs emit a
large standing pile of them — mostly `security-defined`, since neither spec
declares `securitySchemes` — so a clean run is not the bar. Only structural
invalidity fails CI.

## Consumers

`main` is the live source for the docs site, so merging here ships immediately.

- `docs` — Mintlify pulls `orchestrator/blanc.json` and `deposit-service.json` from raw `main` (`docs/docs.json`)
- `sdk` — generates `wire.gen.ts` from `orchestrator/blanc.json` at the commit pinned in `sdk/src/clients/orchestrator/.openapi-ref`, bumped hourly by its `sync-wire-types` workflow
- `mockestrator` — `@hey-api/openapi-ts` codegen from `orchestrator/blanc.json`

## Gotchas

- Every orchestrator spec lives under `orchestrator/`, one file per API version. A root-level `orchestrator.json` (a stale alps copy, unlinted and no longer published) existed until it was removed; external links to that path are dead, not broken publishing.
- Orchestrator specs are OpenAPI 3.0.0 with inlined schemas (no `components`); `deposit-service.json` is 3.1.0 with `components.schemas`. Tooling must handle both.
- alps and blanc are separate wires with different paths, not aliases (`/intents/route` vs `/quotes`, `/intent-operations` vs `/intents`). See `orchestrator/docs/api.md` for the versioning model.
