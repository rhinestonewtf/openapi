# openapi

Published OpenAPI specs for Rhinestone's public HTTP APIs. A specs-only repo — no package
manifest, no build, no runtime code.

The orchestrator publishes **one file per API version**, under `orchestrator/`, named for
that version's dated codename — `2026-04.blanc` lives in `blanc.json`. Versions are
published side by side rather than replacing each other, so pick deliberately: a client
pinned to an older version should read that version's file, not the newest one. Each
spec's own `info.version` is the authoritative version string.

The deposit service publishes a single unversioned `deposit-service.json`.

## Specs are generated — never hand-edit them

Every JSON file here is a build artifact copied in from its source repo (`orchestrator`,
`deposit-service-processor`). Editing a spec in this repo fixes nothing: the next bot PR
overwrites it. Change the endpoint definition upstream instead.

Each source repo regenerates its spec on push to `main`, force-pushes a fixed branch, and
opens or updates a rolling PR here. **Never base work on those branches** — they are
force-pushed. The review of that PR's diff *is* the API change review.

If you consume a spec from this repo by URL, point at a specific version file. A
top-level aggregate file existed historically and has been removed, so URLs built on that
assumption will 404.

## CI

Pull requests are linted with `@redocly/cli lint --extends minimal` over every spec.

## Where to go next

[AGENTS.md](./AGENTS.md) — the full source-repo and generator table, the rolling branch
names, and commands.
