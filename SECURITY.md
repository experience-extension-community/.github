# Security Policy

## Reporting a Vulnerability

If you discover a security vulnerability in any extension or repository within this organization, **please do not open a public issue.** Public disclosure before a fix is available puts every adopting institution at risk.

Instead, report privately by emailing **mike@leys.dev** with:

- Repository name and affected version(s)
- Description of the vulnerability
- Steps to reproduce
- Potential impact
- Any proposed mitigation (optional)

You may also use [GitHub's private vulnerability reporting](https://docs.github.com/en/code-security/security-advisories/guidance-on-reporting-and-writing/privately-reporting-a-security-vulnerability) on the affected repository if it is enabled.

## What to Expect

- **Acknowledgment** within 5 business days
- **Initial assessment** within 10 business days
- **Coordinated disclosure timeline** agreed with the reporter, typically 30–90 days depending on severity
- **Credit** in the security advisory (unless you request anonymity)

## Scope

This policy covers all repositories in the `experience-extension-community` organization. It does not cover vulnerabilities in:

- Ellucian's own products and services (report those directly to Ellucian)
- Third-party dependencies (report to upstream maintainers; we will track and update)
- An institution's own deployment environment

## Coordinated Disclosure

Once a fix is available, we will:

1. Release a patched version of the affected repository.
2. Publish a GitHub Security Advisory with CVE if applicable.
3. Notify maintainers of dependent extensions in the org.
4. Notify community members via GitHub Discussions.

## Reporters' Hall of Fame

Reporters who follow this policy in good faith are recognized in our security acknowledgments (with permission).
