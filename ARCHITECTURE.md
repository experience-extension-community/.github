<!--
SPDX-License-Identifier: Apache-2.0
-->

# Architecture

The authoritative technical rules for the **Experience Extension Community**
organization. If [CONTRIBUTING.md](./CONTRIBUTING.md) handles *process* (how to
open a PR, how to ask for review, how to behave), this document handles
*substance* (what the code, repo structure, tests, and docs must look like
to be acceptable in the org).

When CONTRIBUTING, GOVERNANCE, CATALOG, README, or SUPPORT say "see
ARCHITECTURE.md", they mean this file.

---

## 1. Purpose

The Experience Extension Community organization exists to provide a
**vendor-neutral, openly-licensed catalog of Ellucian Experience extensions**
shared across higher-ed institutions. The goal is concrete: any institution
should be able to browse the [catalog](./CATALOG.md), pick an extension,
edit a small surface of config to match their tenant, and ship — without
forking institution-specific code.

This document is the canonical source for the technical bar an extension
must meet to live in this org and the rules contributors are expected to
follow. Process and people questions (how decisions are made, how
maintainers are added, how disputes are resolved) belong in
[GOVERNANCE.md](./GOVERNANCE.md). Etiquette and the PR mechanics belong in
[CONTRIBUTING.md](./CONTRIBUTING.md).

Changes to this document follow the org-wide decision rules in
[GOVERNANCE.md](./GOVERNANCE.md#decisions): org-lead approval pre-transition,
Steering Committee majority post-transition.

## 2. Repo strategy

**One repo per sample.** Each published extension lives in its own
repository inside the org, with its own issue tracker, CODEOWNERS, CI,
versioning, and maintainer set. The org **does not** run a monorepo.

Reasons:

- **Adoption.** "Use this template" produces a clean fork focused on a
  single sample. A monorepo would force adopters to extract a sub-tree
  and lose history and CI wiring.
- **Independent maintenance.** Different institutions maintain different
  samples on different cadences. A monorepo couples release schedules
  unnecessarily.
- **Per-repo governance.** [GOVERNANCE.md](./GOVERNANCE.md) defines
  per-repo maintainers via `CODEOWNERS`. The model is already per-repo.

If a single sample has *internal* multi-package structure (a card + a
companion Data Connect pipeline + docs), that's fine — see Section 3.
That's not a monorepo; it's a single sample with multiple folders.

## 3. Repo internal layout

Two layouts are accepted. Pick based on what the extension actually
ships.

### Nested layout (recommended when Data Connect is involved)

```
<repo>/
├── README.md
├── LICENSE
├── .github/
│   ├── CODEOWNERS
│   └── workflows/ci.yml
├── docs/
│   ├── architecture.md
│   ├── adoption-guide.md
│   └── images/
├── extension/                 ← the Experience extension itself
│   ├── package.json
│   ├── extension.js
│   ├── webpack.config.js
│   ├── eslint.config.mjs
│   ├── .nvmrc
│   ├── jest.config.js
│   └── src/
│       ├── cards/
│       ├── components/
│       ├── config/
│       ├── page/
│       └── utils/
└── dataconnect/               ← Ethos DC pipeline definitions
    ├── README.md
    └── <COMMUNITY-resource>_v1.0.0.json
```

The canonical example of this layout is
[`user-information-ids`](https://github.com/experience-extension-community/user-information-ids).

### Flat layout (recommended when there is no Data Connect counterpart)

```
<repo>/
├── README.md
├── LICENSE
├── package.json
├── eslint.config.mjs
├── .nvmrc
├── jest.config.js
└── src/
    ├── cards/
    ├── components/
    └── …
```

Use this when the entire extension is a `package.json`-rooted JavaScript
project. The org's [reusable CI workflow](./.github/workflows/reusable-ci.yml)
assumes this layout by default.

Either layout is acceptable. Mixed conventions (some files at root, some
nested) are not — pick one.

## 4. Licensing

**Apache License 2.0 only.** No GPL, no MIT, no proprietary, no
"all rights reserved" content in source. Every repo in the org ships
under Apache-2.0 and includes a `LICENSE` file with the full text at the
repo root.

Why Apache-2.0:

- Patent grant. Institutions contributing in good faith should not also
  be exposing themselves to downstream patent claims.
- Compatible with re-use inside other Apache-2.0 projects, MIT projects,
  BSD projects, and proprietary forks.
- Already the license of every existing org repo; consistency matters.

## 5. SPDX headers and file conventions

**Every** `.js`, `.jsx`, `.ts`, `.tsx`, `.mjs`, `.cjs` source file in the
repo MUST start with:

```js
// SPDX-License-Identifier: Apache-2.0
```

Markdown files (`*.md`) do not require an SPDX header — the surrounding
LICENSE file covers them. If a markdown file is intended to ship standalone
(detached from the LICENSE), prepend an HTML-comment header:

```md
<!--
SPDX-License-Identifier: Apache-2.0
-->
```

Other conventions:

- **Line endings:** LF in committed files. Repos with mixed editors set
  `.gitattributes` to normalize on checkout.
- **Encoding:** UTF-8.
- **Indentation:** 4 spaces in JavaScript/JSX. 2 spaces in YAML. Project
  may enforce via ESLint and `.editorconfig` if desired.
- **File names:** kebab-case for repo names; PascalCase for React component
  files (`UserInformationCard.jsx`); camelCase for non-component modules.

## 6. Branding & theming tokens

**No hardcoded brand colors, fonts, or institution-specific visual tokens
outside a single tokens module.** The recommended path is
`src/utils/branding/tokens.js`. The tokens module exports a structured
object covering primary/secondary/text/surface/status/focus.

Why this matters: adopters change their brand by editing one file, not
by grep-and-replace across the codebase. A reviewer can verify
"institution-agnostic" with one file diff.

Fonts follow the same rule. If a web font is required, load it via a
runtime helper that reads from the institution config (e.g.,
`institution.typekit.href`) and no-ops when unset, so adopters without a
licensed font get the system default with zero edits.

## 7. Parameterization surface

Every institution-specific value lives in a small, clearly-named "config
surface" — a handful of files an adopter is expected to edit. Each sample
documents its surface in its own `docs/architecture.md` and its
`README.md` Configuration Reference table.

Recommended file names (use unless you have a strong reason not to):

| Concern | File |
|---|---|
| Identity (publisher, name, DC pipeline name, email domains, font) | `src/config/institution.js` |
| Domain entities the sample iterates over | `src/config/<thing>Types.js` |
| Role / authorization vocabulary | `src/config/roles.js` |
| Visual tokens | `src/utils/branding/tokens.js` |

Source files outside this surface MUST NOT contain institution-specific
literals (email domains, brand colors, hostnames, role strings, etc.).
Reviewers should grep for the originating institution's name, brand
slug, and brand colors in every PR — zero hits is the bar.

The canonical example is the five-file surface in
[user-information-ids](https://github.com/experience-extension-community/user-information-ids/tree/main/extension/src/config).

## 8. Accessibility

**WCAG 2.2 AA is the minimum.** Zero `jest-axe` violations is the bar
in tests. Every interactive component MUST have an `axe`-clean assertion
covering its loaded state.

Concrete rules:

- Every interactive control is keyboard-reachable. **No mouse-only
  interactions.** Click handlers also bind to `Enter` and `Space` where
  appropriate.
- Visible focus state on every interactive element — use a 3px outline
  with a token-defined focus color. Never use `outline: none` without an
  equally-visible replacement.
- Color contrast: text ≥ 4.5:1 on its background; UI elements ≥ 3:1
  (WCAG 2.2 AA). Brand-token defaults shipped in the org's reference
  samples already meet AA; adopters who change tokens must re-verify
  contrast.
- Respect `prefers-reduced-motion`: animations must collapse to ≤ 0.01ms
  duration when the user prefers reduced motion.
- ARIA live regions for ephemeral status (copied-to-clipboard, error
  messages, loading announcements).
- Semantic landmarks: `<section aria-labelledby="...">`, headings in a
  sensible order, `<main>` / `<nav>` / etc. where applicable.

Manual passes (NVDA or VoiceOver) are required before a sample is
promoted from 🟡 Beta to 🟢 Stable — see Section 16.

## 9. Testing

Baseline: **Jest + React Testing Library + jest-axe**, no exceptions.

Every component MUST have at minimum:

- A **render** test (the component mounts without throwing under the
  default mocked hooks).
- A **role-based-branching** test (different `useUserInfo()` outputs,
  different data shapes, error states).
- An **axe** test (`expect(await axe(container)).toHaveNoViolations()`).

Every hook MUST have a behavior test.

Every utility module MUST have a unit test.

Coverage thresholds are not numerically enforced yet — that is a follow-up
once we have enough samples to set a meaningful number. The current rule
is "every component, hook, and util has at least one test, and no test is
skipped without a tracked issue."

Third-party libraries from Ellucian (`@ellucian/react-design-system/*`,
`@ellucian/ds-icons/*`, `@ellucian/experience-extension-utils`) are mocked
at the module-resolution layer in tests. A reference set of mocks lives in
[user-information-ids/extension/test/mocks/](https://github.com/experience-extension-community/user-information-ids/tree/main/extension/test/mocks);
copy and adapt.

## 10. CI

Every extension repo MUST have a GitHub Actions workflow that, on every
PR and push, runs (in order): **lint → test → build**. The workflow must
fail on any non-zero exit.

There are two acceptable shapes:

### Option A — call the org's reusable workflow (flat layout)

```yaml
# .github/workflows/ci.yml
name: CI
on: [pull_request, push]
jobs:
  ci:
    uses: experience-extension-community/.github/.github/workflows/reusable-ci.yml@main
    with:
      node-version-matrix: '["20", "22"]'
```

Requires `package.json` at the repo root and the standard script names
the reusable workflow expects (`lint`, `test:ci`, `test:a11y`, `build`).
Updates to the reusable workflow propagate to every adopting repo on the
next workflow run.

### Option B — inline workflow (nested layout)

When `package.json` lives under `extension/` instead of the repo root,
the reusable workflow's `working-directory: .` assumptions don't hold.
Use an inline workflow that sets `defaults.run.working-directory` and
points to the nested `package-lock.json`:

```yaml
# .github/workflows/ci.yml
name: CI
on: [pull_request, push]
jobs:
  build:
    runs-on: ubuntu-latest
    defaults:
      run:
        working-directory: extension
    steps:
      - uses: actions/checkout@v4
      - uses: actions/setup-node@v4
        with:
          node-version-file: 'extension/.nvmrc'
          cache: 'npm'
          cache-dependency-path: 'extension/package-lock.json'
      - run: npm ci
      - run: npm run lint
      - run: npm test
      - run: npm run build-prod
```

The
[`user-information-ids` repo](https://github.com/experience-extension-community/user-information-ids/blob/main/.github/workflows/ci.yml)
is the canonical example.

## 11. Code quality and commit standards

### ESLint

- **ESLint v9+ with the flat config** format (`eslint.config.mjs`).
- Required plugins: `eslint-plugin-react`, `eslint-plugin-react-hooks`,
  `eslint-plugin-jsx-a11y`, `eslint-plugin-import`.
- Recommended rule sets from each plugin, plus the project's overrides.
- Lint must run as `npm run lint` and pass on every PR (enforced by CI).

The legacy `.eslintrc.json` shape is not accepted for new code.

### Conventional Commits

Every commit message MUST use a [Conventional Commits](https://www.conventionalcommits.org/)
prefix: `feat:`, `fix:`, `docs:`, `chore:`, `refactor:`, `test:`, `ci:`,
`build:`, or `style:`. Squash-merge commit messages on `main` must
follow the same shape.

### DCO sign-off

Every commit MUST be signed off via `git commit -s` (Developer
Certificate of Origin). This is enforced by the org-wide DCO check.
Unsigned-off commits will block merge.

### Squash or rebase only

Pull requests are merged via squash or rebase. No merge commits on
`main`. The merging maintainer authors a clean conventional-commit
message for the squashed commit.

## 12. Dependencies and supply chain

- **Pin Ellucian SDK versions** to the exact CDN tarball URL referenced
  by `@ellucian/*` dependencies in `package.json`. Don't use unversioned
  `latest` URLs. Bumping the SDK is a discrete PR, not a side effect.
- **Pin major + minor for other dev deps**; allow patch updates. Use
  `^x.y.z` for normal deps, exact versions for anything security-sensitive.
- **Commit `package-lock.json`.** CI depends on reproducible installs via
  `npm ci`.
- **Run `npm audit` quarterly.** Document any accepted findings (typically
  transitive-from-SDK) in the repo's `SECURITY.md` or in a tracked issue.
- **No accidentally-public secrets.** Sample `.env` files (`sample.env`,
  `.env.example`) ship with placeholder values only. Real `.env` files are
  in every `.gitignore`.

## 13. Versioning and changesets

Every extension repo uses [Changesets](https://github.com/changesets/changesets)
(`@changesets/cli`) to track user-facing changes and version bumps.

- Every PR with a user-facing impact adds a changeset entry
  (`.changeset/<descriptive-name>.md`) declaring patch / minor / major and
  describing the impact in user-facing terms.
- Maintainers run `npm run changeset:version` periodically (or on a
  release cadence) to consume pending entries and bump the package
  `version`.
- Extensions ARE NOT published to npm. Versioning serves the CHANGELOG
  and the catalog status — not a registry.
- Tag releases with `vX.Y.Z` matching `package.json`. GitHub Releases are
  optional but recommended for samples with adopters in production.

## 14. Documentation conventions

Every repo MUST contain:

| File | Purpose |
|---|---|
| `README.md` | What it does, quickstart, **Configuration Reference table** covering every adopter-editable knob. |
| `LICENSE` | Apache-2.0 full text. |
| `.github/CODEOWNERS` | Initial maintainer(s). |
| `docs/architecture.md` | Data flow, parameterization surface rationale, why split this way. |
| `docs/adoption-guide.md` | Step-by-step "make this yours" walkthrough. |
| `docs/images/` | At least the `card-preview.png` and `page-preview.png` listed in the README. May start as a `README.md` placeholder pending real screenshots; **screenshots are required before promotion to 🟢 Stable** (Section 16). |

Optional but recommended:

| File | Purpose |
|---|---|
| `CLAUDE.md` / `AGENTS.md` | Project context for AI assistants and human contributors (see Section 15). |
| `SECURITY.md` | Repo-specific reporting (inherits from org default if absent). |
| `CONTRIBUTING.md` | Repo-specific contributing guidance (inherits from org default if absent). |

## 15. AI-tool configuration files

This section clarifies a distinction that has caused friction.

**Not committed** (per-contributor, often containing private state):

- `.cursor/`, `.cursorrules`, `.cursorignore`, `.aider*` and other AI-tool
  workspace configuration directories
- `.continue/`, `.windsurf/`, similar IDE-AI configs
- `mcp.json` / MCP server configurations that contain API keys, private
  endpoints, or per-machine secrets
- API keys, tokens, or secrets of any kind
- Private agent / sub-agent definitions or personalized prompts intended
  for a single user or a single institution's internal use

These belong in `.gitignore` and in each contributor's personal
environment, not in the shared repo.

**Allowed and encouraged** (documentation that happens to be auto-loaded
by AI tools):

- `CLAUDE.md` at the repo root — project context, conventions, build /
  test commands, parameterization-surface rules. Read by Anthropic's
  Claude Code CLI automatically and equally useful as a reference for
  human contributors.
- `AGENTS.md` at the repo root — equivalent for tools that look for that
  filename.
- Any other AI-readable markdown helper that is **content**, not
  **configuration**.

The distinction is **content vs. configuration**. A markdown file
describing the codebase is content — it has the same value as
`docs/architecture.md` and the same review surface. A JSON file
configuring a tool, or a prompt baked for a specific user, is
configuration — it varies per contributor and may contain secrets.

If you are unsure whether a file belongs, ask: *"If a brand-new
contributor cloned this repo and never used the same AI tool I do, would
this file be useful to them or just clutter?"* Useful → commit. Clutter
or harmful → ignore.

## 16. Promotion-to-Org Checklist

This is the gate a sample passes to move from 🟡 **Beta** (initial
contribution, "useful as reference") to 🟢 **Stable** (production-ready,
listed without caveats) in [CATALOG.md](./CATALOG.md).

The org lead (or, post-transition, the Steering Committee — see
[GOVERNANCE.md](./GOVERNANCE.md#transition-trigger-steering-committee))
approves the status change after every checkbox below is satisfied.

- [ ] **Production deployment.** At least one institution has deployed
  this extension to a *production* Experience tenant (not a dev or test
  tenant). The deploying institution is recorded in the sample's README
  or release notes.
- [ ] **Screenshots.** `docs/images/` contains at least `card-preview.png`
  and, if the sample has a page view, `page-preview.png`. Screenshots
  use neutral test data — no real IDs, names, or emails.
- [ ] **Manual a11y pass.** A human ran either an axe browser-extension
  sweep or a screen-reader pass (NVDA or VoiceOver) against a live
  instance of the extension. Findings are either resolved or documented
  in the README's accessibility section.
- [ ] **Two-institution review.** At least one PR against the repo has
  been approved by reviewers from **two different institutions**. This
  guards against single-institution dependencies in code or design.
- [ ] **Maintainer responsiveness.** The repo's listed maintainers have
  acknowledged new issues within 14 days for the prior calendar quarter.
  An unresponsive maintainer set is grounds for blocking promotion
  regardless of code quality.
- [ ] **CI green on main.** Lint, tests, and build all pass on the
  `main` branch's latest commit at promotion time.
- [ ] **Configuration Reference complete.** The README's Configuration
  Reference table covers every adopter-editable env var, manifest field,
  and code-side knob. Reviewers verify by grep-and-cross-check.
- [ ] **No institution-specific literals.** Grepping for the originating
  institution's name, brand slug, and brand colors returns zero hits
  outside `docs/` provenance notes.

A sample failing any single check stays at 🟡 Beta. A sample that
*regresses* on any check (e.g., maintainer stops responding) may be
moved back from 🟢 Stable to 🟡 Beta or ⚫ Archived — see
[GOVERNANCE.md § Removing an Extension](./GOVERNANCE.md#removing-an-extension).

The checklist is intentionally short. Add items here only by amendment
to this document via PR.

---

## References

- [CONTRIBUTING.md](./CONTRIBUTING.md) — how to contribute to any repo in the org.
- [GOVERNANCE.md](./GOVERNANCE.md) — roles, decision-making, transition triggers.
- [CATALOG.md](./CATALOG.md) — curated index of all extensions.
- [README.md](./README.md) — what the `.github` hub repo is and how it works.
- [SECURITY.md](./SECURITY.md) — vulnerability reporting policy.
- [SUPPORT.md](./SUPPORT.md) — where to get help.
- [user-information-ids](https://github.com/experience-extension-community/user-information-ids) — canonical example extension. Its `extension/src/config/`, `extension/src/utils/branding/tokens.js`, `extension/test/mocks/`, and `.github/workflows/ci.yml` illustrate Sections 6–10 in working code.
