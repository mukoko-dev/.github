# mukoko-dev/.github

> Org-wide defaults for `mukoko-dev`: the shared CI wiring, the lint
> configuration every repository inherits, and the notes explaining why both
> are shaped the way they are.

[![Lint](https://github.com/mukoko-dev/.github/actions/workflows/lint.yml/badge.svg)](https://github.com/mukoko-dev/.github/actions/workflows/lint.yml)

**Workflow library:** [`nyuchi/.github`](https://github.com/nyuchi/.github/tree/main/.github/workflows)
| **Active ruleset:** `org-wide-main-protection` | **Merge method:** rebase only

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
the PR and record why, so the divergence is deliberate and visible.

What this repository does hold, in `.github/workflows/`, are three thin
callers that also apply to this repository itself:

| File                | Name       | Trigger                                                       |
| ------------------- | ---------- | ------------------------------------------------------------- |
| `lint.yml`          | `Lint`     | Pull requests, and pushes to `main`/`master`/`scaffold`       |
| `pr-title-lint.yml` | `PR title` | `pull_request_target` — opened, edited, reopened, synchronize |
| `stale.yml`         | `Stale`    | Daily at 01:23 UTC, and `workflow_dispatch`                   |

[hub]: https://github.com/nyuchi/.github/tree/main/.github/workflows

## Every repository needs `lint.yml`

The org ruleset `org-wide-main-protection` requires five status checks on the
default branch of every repository:

```text
lint / actionlint
lint / JSON validity
lint / prettier
lint / markdownlint
lint / yamllint
```

Those names are not free-form. A job that calls a reusable workflow publishes
its checks as `<caller job> / <called job>`, so the five strings above are
produced by a job named exactly `lint` calling `reusable-lint.yml`, whose own
jobs are named `actionlint`, `JSON validity`, and so on.

Defining the five as ordinary top-level jobs does **not** work. They then
report as bare `actionlint`, `JSON validity`, … , the five required contexts
never report at all, and a required context that never reports is
permanently pending — every pull request in the repository is blocked
forever, with every visible check green. Copy
[`.github/workflows/lint.yml`](.github/workflows/lint.yml) verbatim.

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
`.editorconfig` in this repository are the org baseline. The lint tools
auto-discover them from the repository root, so copy them into each
repository rather than pointing at these.

Two adjustments come up often:

- **`.yamllint.yaml` must ignore your lockfile.** `pnpm-lock.yaml` has lines
  far past any sane width, and the reusable runs `yamllint -s`, which
  promotes warnings to errors.
- **Prettier and `.mdx` do not mix.** Prettier still parses `.mdx` with its
  legacy MDX1 parser and rewrites MDX2 expression comments — `{/* … */}`
  becomes `{/_ … _/}` — corrupting the file. If a repository has `.mdx`
  content, pass a `prettier-glob` that excludes it.

## Merge method and branch protection

**Rebase only.** `org-wide-main-protection` sets
`allowed_merge_methods: ["rebase"]`, and the repository settings agree with it:
every repository in this org has `allow_squash_merge: false`,
`allow_merge_commit: false`, `allow_rebase_merge: true` and
`allow_auto_merge: true`. Squash and merge-commit are off in both places.

Alongside the five required checks, the ruleset applies:

| Rule                      | Effect                                                                                                                                                                                |
| ------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `deletion`                | The default branch cannot be deleted                                                                                                                                                  |
| `non_fast_forward`        | No force pushes                                                                                                                                                                       |
| `required_linear_history` | No merge commits reach the default branch                                                                                                                                             |
| `pull_request`            | Changes land by PR. Stale reviews are dismissed on push, review threads must be resolved, and unattributed changes need an extra approval. `required_approving_review_count` is **0** |
| `required_status_checks`  | Strict — the branch must be up to date with the base                                                                                                                                  |

It applies to the default branch of every repository except `sandbox-*` and
`archive-*`.

There is **no `required_signatures` rule.** An earlier version of this
document said there was; there is not, and commits do not need to be signed
to merge.

A second ruleset, `enterprise-main-protection`, arrives from the `bundu-labs`
enterprise. It is in **evaluate** mode — it reports what it would have done
and blocks nothing.

## Licence

No `LICENSE` file is committed to this repository. Until one is added the
contents are under exclusive copyright.

© Nyuchi Africa (Pvt) Ltd.
