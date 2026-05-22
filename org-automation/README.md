# `org-automation/` — Manifest for the Ketiv GitHub Automation system

This directory is the **source of truth** for what the `Ketiv.GitHub.Automation` Azure Function applies to every repo in `Ketiv-Technologies-Inc`.

When you merge a PR that touches this directory, [`.github/workflows/sync-manifest.yml`](../.github/workflows/sync-manifest.yml) runs:

1. Validates `manifest.json` against `manifest.schema.json`.
2. Scans `files/**` for accidentally committed secrets (TruffleHog).
3. Authenticates to Azure via OIDC (federated identity, gated by the `production` environment on this repo).
4. PUTs the manifest spec to App Configuration key `Ketiv:GitHub:Automation:Manifest`, tagged with the commit SHA.

The function reads from App Config (60s cache); changes go live across all function instances within a minute of the workflow completing.

## How to edit

1. Open a PR that modifies `manifest.json`.
2. Wait for PR validation (schema check + secret scan).
3. Get a CODEOWNERS-required review.
4. Merge. Approve the `production` environment deployment when prompted. The workflow runs and pushes to App Config.

Removing or disabling a tool does **not** unapply it from existing repos — the function only ever pushes, never reverts. See PLAN.md §2 (Non-Goals).

## Tool kinds

| `kind` | Purpose |
|---|---|
| `workflowFile` | A file under `.github/workflows/` (e.g. TruffleHog, CodeQL) |
| `configFile` | Any other repo file (`dependabot.yml`, `.editorconfig`, `.coderabbit.yaml`) |
| `repoSetting` | Repo-level settings via `PATCH /repos/{o}/{r}` — currently merge-button defaults |
| `appCheck` | Verifies a GitHub App is installed; opens an Issue if not. Read-only. |

## Selector syntax (`appliesTo`)

- `*` — all repos
- `name:foo-*` — glob on repo name
- `language:csharp,typescript` — primary-language match (OR within the list)
- `topic:has:public` — repo topic filter
- `visibility:public` / `visibility:private`
- Multiple expressions = AND. Multiple values within one expression = OR.

## Source of files

- `source.type: "githubRepo"` — pulled from another repo at runtime (e.g., `workflow-templates/` in this repo).
- `source.type: "gitFile"` — pulled from `files/` in this same `org-automation/` tree.

File *content* is **not** inlined into the App Config blob. App Config holds the spec only (which tools, which selectors, where the files live); the function reads file content from GitHub at orchestration time, with a short in-memory cache. This keeps the App Config blob under the 10 KB per-value ceiling.

## What lives here

```
org-automation/
├── manifest.json          ← edit this to add/remove/tune tools
├── manifest.schema.json   ← schema validating manifest.json (CI gate)
├── README.md              ← this file
└── files/                 ← inline content files referenced via source.type "gitFile"
```

## Schema version policy

`schemaVersion` is on the contract between this manifest and the function. If you bump it, deploy a function update that understands both N and N+1 first; bump the manifest in a separate PR; then drop N support in a subsequent function deploy. Never bump in the same change as a function deploy.

## Related

- [PLAN.md](https://github.com/Ketiv-Technologies-Inc/github-automation/blob/main/docs/PLAN.md) — full architecture (rev 6)
- [Phase 0B manual setup runbook](https://github.com/Ketiv-Technologies-Inc/github-automation/blob/main/docs/phase-0b-manual-setup-runbook.md) — how this seed got here
