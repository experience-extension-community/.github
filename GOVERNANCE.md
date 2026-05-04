# Governance

This document defines roles, decision-making, and the path for evolving governance as the community grows.

## Mission

To provide a discoverable, well-maintained, openly-licensed catalog of Ellucian Experience extensions built and shared by higher-education institutions.

## Roles

### Org Lead (currently: 1 person, Michael Leys — Florida Polytechnic University)

- Holds **Owner** permission on the GitHub organization.
- Has tiebreaker authority on org-wide decisions (template changes, governance updates, license changes, organization-level branding, the catalog inclusion criteria).
- Approves Code of Conduct enforcement actions.
- Manages org-level secrets and integrations.
- Recruits co-owner(s) from peer institutions before public launch.

### Co-Owner(s)

- Holds **Owner** permission on the GitHub organization.
- Required: at least one co-owner from a peer institution must be in place before the organization is publicly announced. **No single-owner public launch.**
- Shares all org-lead responsibilities. Operates as a peer to the lead.

### Per-Repo Maintainers

- Listed in each extension repo's `CODEOWNERS` file.
- Have **Maintain** or **Admin** permission on their repo(s).
- Final authority on decisions within their repo: code review, releases, breaking-change choices, deprecations.
- Typically employed by the institution that originated the extension, but this is not a requirement.

### Contributors

- Anyone who submits an issue, PR, comment, or Discussion.
- No formal process to become a contributor — just contribute.

## Decisions

### In an extension repo

Maintainers decide by lazy consensus on issues and PRs. If maintainers disagree among themselves, the most senior maintainer breaks the tie. If contention persists, escalate to the org lead.

### Org-wide

The org lead and any co-owners must agree. If they cannot agree, the org lead has tiebreaker authority **for now** — see Transition below.

### Architectural Decision Records (ADRs)

For non-trivial cross-cutting decisions (e.g., "should the shared utils package use TypeScript?"), file an ADR in this repo's `docs/adr/` folder. Format:

```
# ADR-NNN: Short Title

**Status:** Proposed | Accepted | Deprecated | Superseded by ADR-MMM
**Date:** YYYY-MM-DD
**Decided by:** [names]

## Context
What's the situation?

## Decision
What did we choose?

## Consequences
What changes? What are the trade-offs?
```

ADRs are append-only; superseded ADRs are not deleted, just marked.

## Transition Trigger: Steering Committee

When the organization reaches **three institutions actively contributing** (defined as: at least one merged pull request from a contributor employed by that institution, in two consecutive quarters), governance transitions automatically:

1. The org lead's tiebreaker authority dissolves.
2. A Steering Committee forms with one seat per actively contributing institution (capped at 7 to avoid coordination overhead).
3. Org-wide decisions are made by simple majority of the Steering Committee.
4. The previous org lead remains an Owner and has one vote on the Steering Committee, no more, no less.

This trigger is automatic and intentionally formulaic — it removes the awkwardness of asking "is it time to share power yet?" The transition is announced in Discussions and the new committee is recorded in this document via a PR.

## Becoming a Maintainer of an Extension

The existing maintainers of an extension may add a new maintainer when:

- The candidate has had at least 3 substantive PRs merged into that extension.
- The candidate has shown consistent good judgment in code review or issue triage.
- An existing maintainer formally proposes the addition in an issue.
- No existing maintainer objects within 7 days.

## Adding a New Extension to the Organization

See the **Promotion-to-Org Checklist** in [ARCHITECTURE.md](./ARCHITECTURE.md) Section 16. The org lead (or, post-transition, the Steering Committee) approves promotion after the checklist is complete.

## Removing an Extension

An extension may be archived (read-only) when:

- The maintainers declare it deprecated.
- It has had no substantive activity for 18 months and no maintainer responds to a "still maintained?" issue within 30 days.
- It violates the Code of Conduct, license terms, or trademark policy.

Archived extensions remain in the catalog with `Archived` status; they are not deleted.

## Amending This Document

Changes to this `GOVERNANCE.md` follow the org-wide decision rules above: org-lead approval pre-transition; majority of Steering Committee post-transition. Amendments are discussed publicly via PR.
