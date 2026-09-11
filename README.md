# mukoko-dev/.github

Org-wide defaults for `mukoko-dev`: the shared CI wiring, the lint
configuration every repository inherits, and the notes explaining why both
are shaped the way they are.

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
| any repository                         | `reusable-codeql.yml`, `reusable-dependency-review.yml` |

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

## Merge method

The org ruleset allows **squash merges only**, alongside
`required_linear_history` and `required_signatures`. Note that the
repository-level settings in this org still leave merge-commit and rebase
switched on; the ruleset is what actually decides, and it permits squash
alone.
