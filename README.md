# openapi

Published OpenAPI specs for Rhinestone's public HTTP APIs. A specs-only repo — no package
manifest, no build, no runtime code.

| File | API | Version |
|---|---|---|
| `orchestrator/blanc.json` | Rhinestone Orchestrator | `2026-04.blanc` |
| `orchestrator/alps.json` | Rhinestone Orchestrator | `2026-01.alps` |
| `deposit-service.json` | Deposit Service | `1.0.0` |

## Specs are generated — never hand-edit them

Every JSON file here is a build artifact copied in from its source repo (`orchestrator`,
`deposit-service-processor`). Editing a spec in this repo fixes nothing: the next bot PR
overwrites it. Change the endpoint definition upstream instead.

Each source repo regenerates its spec on push to `main`, force-pushes a fixed branch, and
opens or updates a rolling PR here. **Never base work on those branches** — they are
force-pushed. The review of that PR's diff *is* the API change review.

## CI

Pull requests are linted with `@redocly/cli lint --extends minimal` over every spec.

## Where to go next

[AGENTS.md](./AGENTS.md) — the full source-repo and generator table, the rolling branch
names, and commands.
