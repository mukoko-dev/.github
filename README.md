# mukoko-dev/.github

> Org-wide defaults for `mukoko-dev`: the shared CI wiring, the lint
> configuration every repository inherits, and the notes explaining why both
> are shaped the way they are.

[![Lint](https://github.com/mukoko-dev/.github/actions/workflows/lint.yml/badge.svg)](https://github.com/mukoko-dev/.github/actions/workflows/lint.yml)

**Workflow library:** [`nyuchi/.github`](https://github.com/nyuchi/.github/tree/main/.github/workflows)
| **Rulesets:** `enterprise-main-protection` and `org-wide-main-protection` | **Merge method:** squash or rebase

## The org profile and the Mukoko Manifesto

| Path                                                                             | Purpose                                                                                                                                         |
| -------------------------------------------------------------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------- |
| [`profile/README.md`](profile/README.md)                                         | The page shown at <https://github.com/mukoko-dev>.                                                                                              |
| [`profile/canonical/MUKOKO_MANIFESTO.md`](profile/canonical/MUKOKO_MANIFESTO.md) | **The Mukoko Manifesto v5.0.0** (October 2026), the Founder's text, unedited. Exempt from Prettier and markdownlint so it is never reformatted. |

The Manifesto is one of three canonical documents. The other two live in
their owners' `.github` repositories:
[the Nyuchi Architecture](https://github.com/nyuchi/.github/blob/main/profile/canonical/NYUCHI_ARCHITECTURE.md)
(which wins wherever documents disagree) and
[the Bundu Order](https://github.com/bundu-labs/.github/blob/main/profile/canonical/BUNDU_ORDER.md).

## Community-health defaults

`CODE_OF_CONDUCT.md`, `CONTRIBUTING.md`, `SECURITY.md`, `SUPPORT.md`, the PR
template and the issue forms in `.github/` apply to every `mukoko-dev`
repository that does not ship its own. `.github/CODEOWNERS` and
`.github/dependabot.yml` apply to this repository only.

## CI is not written here

The reusable workflow library lives in [`nyuchi/.github`][hub] and is shared
across the whole estate. That repository is **public**, and a reusable
workflow in a public repository can be called from any repository in any
organisation — private callers included. So `mukoko-dev` repositories call
those workflows directly rather than keeping a second copy of them.

This is already how `bundu-labs` and `mzizi-dev` consume the library. Copying
the workflows into this org would double the maintenance surface and let the
two copies drift apart silently, which is the failure this arrangement is
meant to avoid.

Only add a workflow file here when the behaviour genuinely differs from the
shared one. If you ever do copy a reusable workflow into this org, say so in
the PR and record why, so the divergence is deliberate and visible. The
shift-left security workflows below are the one such case so far.

What this repository does hold, in `.github/workflows/`, is the org-required
lint workflow, the shift-left security reusables, and thin callers that also
apply to this repository itself:

| File                             | Name                           | Trigger                                                       |
| -------------------------------- | ------------------------------ | ------------------------------------------------------------- |
| `org-lint.yml`                   | `Org lint`                     | Every pull request in every `mukoko-dev` repository           |
| `lint.yml`                       | `Lint`                         | Pull requests, and pushes to `main`/`master`/`scaffold`       |
| `pr-title-lint.yml`              | `PR title`                     | `pull_request_target` — opened, edited, reopened, synchronize |
| `stale.yml`                      | `Stale`                        | Daily at 01:23 UTC, and `workflow_dispatch`                   |
| `security.yml`                   | `Security`                     | This repository: PRs, pushes, weekly                          |
| `reusable-dependency-review.yml` | `Reusable / Dependency review` | `workflow_call`                                               |
| `reusable-dependency-audit.yml`  | `Reusable / Dependency audit`  | `workflow_call`                                               |
| `reusable-codeql.yml`            | `Reusable / CodeQL`            | `workflow_call`                                               |

[hub]: https://github.com/nyuchi/.github/tree/main/.github/workflows

## Shift-left security

The CI layer of the shift-left plan in
[mzizi-dev/mzizi#62](https://github.com/mzizi-dev/mzizi/issues/62). Three
reusable workflows, each with least-privilege permissions, actions pinned to
a commit, no secrets, and tool binaries checked against a SHA-256 pinned in
the workflow:

| Workflow                         | What it answers                                                                                                                                                | Inputs                                                                                                      |
| -------------------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------- | ----------------------------------------------------------------------------------------------------------- |
| `reusable-dependency-review.yml` | Does this pull request **add** a dependency with a known advisory? Every ecosystem in GitHub's dependency graph at once. Passes with a notice on other events. | `fail-on-severity` (default `low`), `fail-on-scopes`, `allow-ghsas`, `config-file`                          |
| `reusable-dependency-audit.yml`  | Is what the repository depends on **today** acceptable? `rust`: `cargo deny check` per manifest. `js`: `osv-scanner` per lockfile. Fails if it finds nothing.  | `ecosystems`, `rust-manifests`, `deny-config`, `cargo-deny-checks`, `js-paths`, tool pins                   |
| `reusable-codeql.yml`            | Static analysis, one job per language, results in the Security tab.                                                                                            | `languages` (e.g. `rust, javascript-typescript, actions`), `build-mode`, `paths`, `paths-ignore`, `queries` |

Each file's header documents its inputs in full. The minimal caller, which
also appears as **Shift-left security** under Actions → New workflow in
every `mukoko-dev` repository, is
[`workflow-templates/security.yml`](workflow-templates/security.yml):

```yaml
jobs:
  dependency-review:
    uses: mukoko-dev/.github/.github/workflows/reusable-dependency-review.yml@main
  audit:
    uses: mukoko-dev/.github/.github/workflows/reusable-dependency-audit.yml@main
    with:
      ecosystems: rust
  codeql:
    uses: mukoko-dev/.github/.github/workflows/reusable-codeql.yml@main
    permissions:
      actions: read
      contents: read
      security-events: write
    with:
      languages: rust, actions
```

Run it on `pull_request`, `merge_group`, `push` to the default branch and
`staging`, and a weekly `schedule` (the template does all four). Before the
first run, switch CodeQL **default** setup off in the repository, or GitHub
rejects the results; a private repository also needs GitHub Code Security
for dependency review and CodeQL.

**The commit layer.** [`.pre-commit-config.yaml`](.pre-commit-config.yaml)
runs gitleaks, actionlint, yamllint and markdownlint on staged files before
each commit (`pre-commit install` once per clone). Linter versions match
the ones CI pins. Any repository can copy it and add hooks for its own
languages. It is a local convenience; the CI checks above stay the gate.

**Who can call them.** This repository is **public**, so any repository in
any organisation can call these workflows, private callers included:
`mukoko-dev`, `mzizi-dev`, `bundu-labs` and `nyuchi` alike. What does
**not** cross organisations is enforcement: a `mukoko-dev` org ruleset can
require these checks only in `mukoko-dev` repositories. To require them in
`mzizi-dev`, that org's own ruleset must name them, or an enterprise ruleset
must.

**Why here and not in `nyuchi/.github`.** The shared library already has
`reusable-codeql.yml`, `reusable-dependency-review.yml`, and the
enterprise-required `dependency-review.yml` (diff-aware review plus
`npm`/`pnpm`/`cargo audit`/`pip-audit` on changed lockfiles). These differ on
purpose: the review needs only `contents: read` (no PR comment, so it works
on forks) and fails from `low` severity; the audit checks the **whole**
dependency tree on push and schedule, not just changed lockfiles, with
`cargo deny` (licences, bans and sources too, not only advisories) and
`osv-scanner`; CodeQL takes a plain language list, per-language build modes,
`paths`/`paths-ignore`, and drops `packages: read`. If they prove out, the
next step is to upstream them to `nyuchi/.github` and turn these into thin
pointers, as `mzizi-dev/mzizi-registry` did for Vite+.

## Lint is required org-wide

The org ruleset `org-wide-main-protection` has a "Require workflows to pass"
rule naming
[`.github/workflows/org-lint.yml`](.github/workflows/org-lint.yml) in this
repository, so GitHub runs it on every pull request in every `mukoko-dev`
repository. Its job is named `lint` and calls `nyuchi/.github`'s
`reusable-lint.yml`, so it publishes the five status checks the same ruleset
requires:

```text
lint / actionlint
lint / JSON validity
lint / prettier
lint / markdownlint
lint / yamllint
```

A repository therefore needs **no `lint.yml` of its own and no lint config
files**. A repository that already has a `lint.yml` caller may keep it; the
org-required run is what the ruleset counts.

Those names are not free-form. A job that calls a reusable workflow publishes
its checks as `<caller job> / <called job>`. A matrix, or five ordinary
top-level jobs, would publish different names, and a required context that
never reports leaves every pull request pending forever.

## Per-repository CI

Beyond lint, pick the reusable workflow that matches the project and call it
from a workflow in that repository:

| project shape                          | reusable workflow                                       |
| -------------------------------------- | ------------------------------------------------------- |
| Next.js app or pnpm/Turborepo monorepo | `reusable-ci-nextjs-monorepo.yml`                       |
| TypeScript application                 | `reusable-ci-typescript.yml`                            |
| Published TypeScript package           | `reusable-ci-typescript-lib.yml`                        |
| Python monorepo                        | `reusable-ci-python-monorepo.yml`                       |
| Rust monorepo                          | `reusable-ci-rust-monorepo.yml`                         |
| Docs site with `.mdx`                  | `reusable-ci-docs-mdx.yml`                              |
| Docker image or container              | `reusable-ci-docker.yml`, `reusable-ci-container.yml`   |
| Terraform or OpenTofu                  | `reusable-ci-terraform.yml`, `reusable-ci-opentofu.yml` |
| Solidity                               | `reusable-ci-solidity.yml`                              |
| any repository                         | `reusable-codeql.yml`, `reusable-dependency-review.yml` |

Also in the library, for repositories that want them: `reusable-release.yml`,
`reusable-sbom.yml`, `reusable-slsa-provenance.yml`,
`reusable-openssf-scorecard.yml`, `reusable-pr-title-lint.yml` and
`reusable-stale.yml`. The list above is not exhaustive — read
[the directory][hub] rather than trusting this table to stay complete.

## Lint configuration

`.prettierrc`, `.prettierignore`, `.markdownlint.jsonc`, `.yamllint.yaml` and
`.editorconfig` in this repository are this repository's own copies. A
repository does not need them: where a repository has its own `.prettierrc`,
`.prettierignore`, `.markdownlint.jsonc` or `.yamllint.yaml` at its root, the
reusable workflow uses it; where it does not, it falls back to the canonical
copy in [`nyuchi/.github`](https://github.com/nyuchi/.github). Add one only
when a repository genuinely needs to differ.

Two adjustments come up often:

- **`.yamllint.yaml` must ignore your lockfile.** `pnpm-lock.yaml` has lines
  far past any sane width, and the reusable runs `yamllint -s`, which
  promotes warnings to errors.
- **Prettier and `.mdx` do not mix.** Prettier rewrites MDX2 expression
  comments — `{/* … */}` becomes `{/_ … _/}` — corrupting the file. If a
  repository has `.mdx` content, give it a `.prettierignore` that excludes
  it.

## Merge method and branch protection

The branch-protection standard is in
[`nyuchi/.github` → `ORG_SETTINGS.md`](https://github.com/nyuchi/.github/blob/main/ORG_SETTINGS.md).
Two rulesets apply to the default branch of every repository here except
`sandbox-*` and `archive-*`.

**`enterprise-main-protection`**, from the `bundu-labs` enterprise, is
**active** in every org: changes land by pull request with **0** required
approvals, linear history, no force pushes, no deletion. It carries no status
checks.

**`org-wide-main-protection`**, this org's own ruleset, repeats that floor and
adds the checks:

| Rule                      | Effect                                                                                                                                                                                                              |
| ------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `deletion`                | The default branch cannot be deleted                                                                                                                                                                                |
| `non_fast_forward`        | No force pushes                                                                                                                                                                                                     |
| `required_linear_history` | No merge commits reach the default branch                                                                                                                                                                           |
| `pull_request`            | Changes land by PR, with **squash or rebase**. Review threads must be resolved. `required_approving_review_count` is **0**; stale reviews are not dismissed on push and unattributed changes need no extra approval |
| `required_status_checks`  | The five lint checks above. Strict — the branch must be up to date with the base                                                                                                                                    |
| `workflows`               | `mukoko-dev/.github/.github/workflows/org-lint.yml` must run and pass on every pull request                                                                                                                         |

Organisation admins can bypass both. A repository may add one ruleset of its
own, named `repo-ci`, holding only its own CI checks; it never repeats these
rules or narrows the merge methods. Classic branch protection is not used.

Repository settings still decide which merge buttons appear: `campfire` and
`mukoko-openapi` have squash merging switched off, so use rebase there.

There is **no `required_signatures` rule.** Commits do not need to be signed
to merge.

## Licence

No `LICENSE` file is committed to this repository. Until one is added the
contents are under exclusive copyright.

© Nyuchi Africa (Pvt) Ltd.
