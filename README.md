# `.github` — Experience Extension Community Hub Repository

> **What this repo is:** the org-level "hub" repository for the [Experience Extension Community](https://github.com/experience-extension-community). It hosts org-wide community files, governance documentation, the extensions catalog, and templates that GitHub auto-applies to every repo in the org that doesn't override them.
>
> **Looking for code?** This repo intentionally contains no code — only docs and metadata. To browse extensions, see [**CATALOG.md**](./CATALOG.md). To start a new extension, use the [**experience-extension-template**](https://github.com/experience-extension-community/experience-extension-template) repo.

---

## How GitHub uses this repo

GitHub treats a repo named `.github` in an organization specially. Files here apply org-wide unless individual repos override them.

| File / folder | Where it appears |
|---|---|
| `profile/README.md` | The org's public landing page at `github.com/experience-extension-community` |
| `CODE_OF_CONDUCT.md` | Default Code of Conduct for any repo without its own |
| `CONTRIBUTING.md` | Surfaced when contributors open a PR or issue, if their repo has none |
| `SECURITY.md` | Linked from each repo's "Security" tab |
| `SUPPORT.md` | Linked from the issue/discussion creation flows |
| `ISSUE_TEMPLATE/` | Default issue forms for repos without their own |
| `PULL_REQUEST_TEMPLATE.md` | Default PR template for repos without their own |
| `workflows/reusable-*.yml` | Reusable GitHub Actions workflows referenced by extension repos |

---

## File index

```
.github/
├── README.md                      ← You are here
├── profile/
│   └── README.md                  ← Org public landing page
├── CATALOG.md                     ← Curated index of all extensions in the org
├── CODE_OF_CONDUCT.md             ← Contributor Covenant 2.1
├── CONTRIBUTING.md                ← How to contribute to any repo in the org
├── GOVERNANCE.md                  ← Roles, decision-making, transition triggers
├── SECURITY.md                    ← Vulnerability reporting policy
├── SUPPORT.md                     ← Where to get help
├── LICENSE                        ← Apache License 2.0 (org default)
├── ARCHITECTURE.md                ← (Phase 1 deliverable) Master architecture plan
├── ISSUE_TEMPLATE/
│   ├── config.yml                 ← Issue template chooser config
│   ├── bug_report.yml             ← Bug report form
│   ├── feature_request.yml        ← Feature request form
│   └── extension_proposal.yml     ← Proposing a new extension to add to the org
├── PULL_REQUEST_TEMPLATE.md       ← Default PR template
└── workflows/
    └── reusable-ci.yml            ← Reusable CI workflow used by extension repos
```

---

## Maintenance notes

- **Org-wide policy changes** (Code of Conduct, governance, security policy, license) are decisions for the org lead, per [GOVERNANCE.md](./GOVERNANCE.md). PRs for these files require lead approval before merge.
- **CATALOG.md** is updated whenever a new extension is promoted to the org per the promotion checklist (see [ARCHITECTURE.md](./ARCHITECTURE.md), Section 16).
- **Reusable workflows** in `workflows/` are referenced by every extension repo's CI. Changes here propagate to every repo's next workflow run — test changes carefully on a single repo before merging.

## Maintainers

| Role | Person | Institution | Contact |
|---|---|---|---|
| Org lead | Michael Leys | Florida Polytechnic University | mike@leys.dev / [@mleys](https://github.com/mleys) |
| Co-maintainer | _seeking peer-institution co-owner before public launch_ | — | — |

## License

This repository's documentation is licensed under [Apache License 2.0](./LICENSE), the same as every other repo in the org.
