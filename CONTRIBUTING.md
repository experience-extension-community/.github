# Contributing to the Experience Extension Community

Thanks for your interest in contributing! This document covers org-wide expectations. Each individual extension repo may have additional, repo-specific guidelines in its own `CONTRIBUTING.md`.

## Code of Conduct

By participating, you agree to abide by our [Code of Conduct](./CODE_OF_CONDUCT.md). Treat everyone with respect.

## Ways to Contribute

- **Report bugs** in an extension's issue tracker (use the bug report template).
- **Request features** in an extension's issue tracker.
- **Improve documentation** — typos, clarity, missing examples. Docs PRs are always welcome.
- **Submit code** for an existing extension.
- **Propose a new extension** by opening an Extension Proposal issue in [this repo](https://github.com/experience-extension-community/.github/issues/new/choose).
- **Build a new extension** — start from the [template repo](https://github.com/experience-extension-community/experience-extension-template).
- **Review pull requests** — even non-maintainers can leave helpful review comments.
- **Answer questions** in [Discussions](https://github.com/orgs/experience-extension-community/discussions).

## Submitting a Pull Request

1. **Fork** the affected repository (or create a branch if you have collaborator access).
2. **Create a feature branch**: `git checkout -b feat/short-description` or `fix/short-description`.
3. **Make your changes** following the extension's coding standards (linting and tests are enforced by CI).
4. **Add tests** where appropriate.
5. **Add a changeset**: `npx changeset` — describe the change and choose patch / minor / major.
6. **Commit** using [Conventional Commits](https://www.conventionalcommits.org/): `feat:`, `fix:`, `docs:`, `chore:`, `refactor:`, `test:`, `ci:`.
7. **Sign off** your commits with the [Developer Certificate of Origin](https://developercertificate.org/) — use `git commit -s` to add the `Signed-off-by` line. By signing off, you certify that you have the right to contribute the code.
8. **Push** and open a PR against `main`.
9. **Fill out the PR template** completely — it isn't busywork; it speeds review significantly.
10. **Pass CI** — lint, type-check, tests, build, and accessibility checks must all be green.
11. **Address review feedback** — at least one approving review is required before merge.

PRs are merged via squash or rebase only — no merge commits. The merging maintainer authors a clean conventional-commit message for the squashed commit.

## Coding Standards

Each extension repo enforces standards via ESLint, Prettier, jest-axe, and CI. The org-wide expectations are:

- **Functional React components only.** Hooks rules strictly followed.
- **Built from `@ellucian/react-design-system/core`** wherever possible. Custom components must be justified.
- **Material Symbols Outlined** for icons (not Path icons).
- **WCAG 2.2 AA accessibility** for every published extension. axe-core suite must pass.
- **No hardcoded brand colors** outside `src/utils/branding/tokens.js`. No hardcoded credentials, URLs, or institution-specific strings anywhere in source.
- **Configuration documented** — every env var and every Experience-side config knob appears in the README's Configuration Reference table.
- **SPDX license header** on every `.js`/`.jsx` file: `// SPDX-License-Identifier: Apache-2.0`.
- **Tests** for every component (render + a11y), every hook (behavior), every util (unit).
- **Tests written with Jest + React Testing Library + jest-axe.**

See [ARCHITECTURE.md](./ARCHITECTURE.md) for the complete standards and rationale.

## Reviewing Pull Requests

Even non-maintainers can leave helpful review comments. When reviewing, look for:

- Does the change do what the PR description says?
- Are there tests proportional to the change?
- Does it pass CI?
- Are there any accessibility regressions?
- Are any new configuration knobs documented?
- Are there any hardcoded brand values, credentials, or institution-specific strings?
- Is the code readable, with comments where logic is non-obvious?

## Recognition

Contributors are recognized in:

- The contributors list on each repo
- The release notes for each extension version
- The org's annual community recap

## Institutional contributors with their own internal standards

Some contributors come from institutions with their own internal engineering frameworks — Florida Polytechnic University, for instance, maintains an enterprise framework covering its DevOps, security, and integration standards. The Experience Extension Community framework does not replace or override those internal standards. They compose:

- Follow the community framework for everything contributed to a public repo in this organization (conventions, accessibility, branding-token discipline, security checklist, license, DCO sign-off).
- If your institution's internal standards are stricter on a given dimension (e.g., your institution requires CLAUDE.md / AGENTS.md / `.claude/` configuration files for AI-assisted development; your institution requires more elaborate per-repo runbooks), follow those in your local working copy. Just don't impose them on other contributors via the community standards.
- The community framework intentionally omits AI-coding-tool-specific configuration files (CLAUDE.md, AGENTS.md, `.cursorrules`, etc.) from the template, so contributors using any AI tool — or none — can work without friction. If your institution requires these files, add them to your fork; just don't commit them to the community template or to extensions you contribute upstream unless adoption becomes broad.

## Questions?


Ask in [Discussions](https://github.com/orgs/experience-extension-community/discussions) — there are no dumb questions, and your asking probably helps the next person too.
