# Security policy

This policy applies to every repository under
[`bundu-labs`](https://github.com/bundu-labs) unless that repository
ships its own `SECURITY.md` with different terms. It mirrors the
canonical Nyuchi Africa security policy at
[`nyuchi/.github → SECURITY.md`](https://github.com/nyuchi/.github/blob/main/SECURITY.md).

## Reporting a vulnerability

**Do not** open a public issue, pull request, or discussion for a
suspected vulnerability. Public disclosure before a fix is available
puts users at risk.

You have two private channels. Use whichever you prefer.

### 1. Email (preferred for first contact)

Send a report to **<security@nyuchi.com>**.

### 2. GitHub Private Security Advisory

On the affected repository: **Security** tab → _Report a
vulnerability_. Only that repo's security maintainers will see the
report.

## What to include

A good report lets us reproduce and assess quickly:

- A descriptive title.
- Repository, package, version, and commit SHA affected.
- A clear description of the vulnerability and its impact.
- Steps to reproduce, ideally with a minimal proof of concept.
- Logs, screenshots, or traffic captures that help.
- Your suggested remediation, if you have one.
- Whether you would like to be credited in the advisory.

## Our commitments

When you report a vulnerability in good faith, we commit to:

- Acknowledging receipt within **3 business days**.
- Initial assessment within **10 business days**.
- Keeping you informed about remediation progress.
- Coordinating disclosure timing with you and crediting you in the
  published advisory unless you ask us not to.

## Scope

In scope:

- Every repository under [`bundu-labs`](https://github.com/bundu-labs).
- Production deployments of those repositories
  (e.g. `nyuchi.com`, `bundu.family`).

Out of scope (report to the relevant team):

- Repositories under [`nyuchi`](https://github.com/nyuchi) — see
  [`nyuchi/.github → SECURITY.md`](https://github.com/nyuchi/.github/blob/main/SECURITY.md).
- Sister-brand surfaces (Mukoko, Shamwari, Nhimbe) operated outside
  this org.
- Third-party services we integrate with — please report directly.

## Repositories that ship their own SECURITY.md

Some repositories add stricter terms (response SLAs, additional scope,
specific environment notes). The repo-level file always wins for that
repo — but it cannot relax the terms here.
