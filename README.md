# `.github` — KETIV organization defaults

This is the **organization profile repository** for [KETIV-Technologies-Inc](https://github.com/Ketiv-Technologies-Inc). It is intentionally meta: nothing here ships to customers. It defines what shows up on our public org page and what GitHub auto-applies to every repo in the org that does not override it.

## What lives here

```
.github/
├── profile/
│   └── README.md              # Renders at github.com/Ketiv-Technologies-Inc
├── ISSUE_TEMPLATE/
│   ├── bug_report.yml         # Default bug-report form
│   ├── feature_request.yml    # Default feature-request form
│   └── config.yml             # Disables blank issues + adds support links
├── PULL_REQUEST_TEMPLATE.md   # Default PR description scaffold
└── workflow-templates/
    ├── dotnet-ci.yml          # .NET 8 build + test
    ├── dotnet-ci.properties.json
    ├── node-ci.yml            # Node + TypeScript build + test + lint
    ├── node-ci.properties.json
    ├── codeql.yml             # CodeQL security scanning
    └── codeql.properties.json
```

## How GitHub uses each piece

| Path | Behavior |
| --- | --- |
| `profile/README.md` | Rendered on the org's public landing page at `github.com/<org>`. Only visible if the file exists in a repo literally named `.github`. |
| `ISSUE_TEMPLATE/` | Applied as the **default issue templates** to any repo in the org that does not define its own `.github/ISSUE_TEMPLATE/` folder. |
| `PULL_REQUEST_TEMPLATE.md` | Applied as the **default PR template** to any repo in the org that does not define its own. |
| `workflow-templates/` | Appears in the "New workflow" picker of every repo in the org. Templates are starting points — once added to a repo, they are owned by that repo. |

## How to change the org landing page

1. Edit `profile/README.md`.
2. Open a PR. CODEOWNERS routes it to the marketing/dev-ex reviewers.
3. On merge, the change is live within seconds at [github.com/Ketiv-Technologies-Inc](https://github.com/Ketiv-Technologies-Inc).

The landing page is **public**. Treat it as marketing surface area — copy passes the same review bar as ketiv.com homepage changes.

## How to add a new workflow template

1. Drop a `your-template.yml` plus a `your-template.properties.json` into `workflow-templates/`.
2. The `properties.json` defines what shows up in the picker UI (name, description, categories, icon).
3. See [GitHub's docs](https://docs.github.com/en/actions/using-workflows/creating-starter-workflows-for-your-organization) for the full spec.

## Things this repo deliberately does **not** include

The user-selected scope skipped these — add them later if needed:

- `CODE_OF_CONDUCT.md` — org-wide code of conduct default.
- `CONTRIBUTING.md` — org-wide contribution guidelines default.
- `SUPPORT.md` — pointers to support channels.
- `FUNDING.yml` — sponsor button config.

## License

The contents of this repository (templates, README copy) are © KETIV Technologies, Inc. They are intended for use within the KETIV organization on GitHub. The workflow templates themselves are MIT-licensed scaffolding — feel free to adapt them.
