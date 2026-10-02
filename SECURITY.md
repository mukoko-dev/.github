# Security policy

This policy applies to every repository under
[`mukoko-dev`](https://github.com/mukoko-dev), the Mukoko super app and
its mini-apps, unless that repository ships its own `SECURITY.md` with
different terms. It mirrors the canonical Nyuchi Africa security policy
at
[`nyuchi/.github → SECURITY.md`](https://github.com/nyuchi/.github/blob/main/SECURITY.md).

## Reporting a vulnerability

**Do not** open a public issue, pull request, or discussion for a
suspected vulnerability. Public disclosure before a fix is available
puts users at risk.

You have two private channels. Use whichever you prefer.

### 1. GitHub private vulnerability reporting

On the affected repository: **Security** tab → _Report a
vulnerability_. Only that repository's maintainers see the report.
This is available on the public repositories in this organisation.

### 2. Email

Send a report to **<security@nyuchi.com>**, the address the Mukoko
repositories already publish. Use email for a private repository, or
when you are not sure which repository is affected.

## What to include

A good report lets us reproduce and assess quickly:

- A descriptive title.
- Repository, package, version, and commit SHA affected.
- The affected Mukoko app and URL, if it is a live deployment.
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

- Every repository under [`mukoko-dev`](https://github.com/mukoko-dev).
- Production deployments of those repositories under `mukoko.com`
  (for example `accounts.mukoko.com`, `events.mukoko.com`,
  `weather.mukoko.com`, `kweli.mukoko.com`).

Out of scope (report to the relevant organisation):

- Repositories under [`nyuchi`](https://github.com/nyuchi), including
  Mukoko News and the Nyuchi API — see
  [`nyuchi/.github → SECURITY.md`](https://github.com/nyuchi/.github/blob/main/SECURITY.md).
- The Mzizi design system in [`mzizi-dev`](https://github.com/mzizi-dev)
  and the Bundu Foundation's repositories in
  [`bundu-labs`](https://github.com/bundu-labs) — use the Security tab
  on the affected repository there.
- Third-party services we integrate with — please report directly.

## Repositories that ship their own SECURITY.md

Some repositories add stricter terms (response SLAs, additional scope,
specific environment notes). The repo-level file always wins for that
repo — but it cannot relax the terms here.
