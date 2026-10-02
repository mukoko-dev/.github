# Contributing

This file is the **organisation-wide default** for the
[`mukoko-dev`](https://github.com/mukoko-dev) GitHub org, home of
Mukoko, the consumer surface of the Bundu ecosystem. Every repository
inherits it unless it ships its own `CONTRIBUTING.md`.

The canonical engineering working agreement is at
[`nyuchi/.github → CONTRIBUTING.md`](https://github.com/nyuchi/.github/blob/main/CONTRIBUTING.md).
The terms below mirror the relevant sections; downstream
`mukoko-dev` repos may add **stricter** rules but cannot **relax** them.

If you are reading this for the first time, also read:

- [`CODE_OF_CONDUCT.md`](./CODE_OF_CONDUCT.md) — how we treat each
  other.
- [`SECURITY.md`](./SECURITY.md) — how to report vulnerabilities.
- [`SUPPORT.md`](./SUPPORT.md) — where to get help.
- [`AGENTS.md`](https://github.com/nyuchi/.github/blob/main/AGENTS.md) in `nyuchi/.github` — rules for
  AI-assisted contributions.

## Quick start

1. **Fork** the repo (external) or **branch** it (member).
2. Work on a branch that matches our [branch-naming rules](#branch-naming).
3. Open a PR with a [Conventional Commits][cc] title, signed commits,
   and a DCO sign-off. Green CI and resolved review threads, and we'll
   merge it. Required approving reviews are **0** during the
   solo-developer phase.

## Commit conventions

Every commit and every PR title on every repo in the org must follow
[Conventional Commits 1.0][cc]:

```
<type>(<optional scope>)<!>: <short imperative summary>

<optional body, wrapped at ~72 chars>

<optional footers>
```

### Allowed types

| Type       | Use for                                      |
| ---------- | -------------------------------------------- |
| `feat`     | A user-visible new feature.                  |
| `fix`      | A user-visible bug fix.                      |
| `perf`     | A change that improves performance.          |
| `refactor` | Code change, neither feature nor fix.        |
| `docs`     | Documentation only.                          |
| `test`     | Adding or correcting tests.                  |
| `build`    | Build system, packaging, dependencies.       |
| `ci`       | CI configuration, workflows, GitHub Actions. |
| `chore`    | Maintenance not covered above.               |
| `revert`   | Reverting a previous commit.                 |
| `style`    | Formatting only; no logic change.            |

PR title lint runs in repositories that call the shared
[`reusable-pr-title-lint.yml`](https://github.com/nyuchi/.github/blob/main/.github/workflows/reusable-pr-title-lint.yml)
workflow.

## Signed commits (required)

Every commit landing on `main` must show **Verified** on GitHub —
either GPG or SSH signed. This is policy, not a ruleset rule: neither
the enterprise nor the org ruleset requires signatures, so reviewers
check it.

## DCO sign-off (required)

Every commit must carry a `Signed-off-by:` trailer, added by
`git commit -s`. This is the
[Developer Certificate of Origin](https://developercertificate.org/);
it asserts the commit is yours to contribute under the project licence.

## Branch naming

`<type>/<short-kebab-description>`. Allowed prefixes:

| Prefix      | Use                 |
| ----------- | ------------------- |
| `feat/`     | new functionality   |
| `fix/`      | bug fix             |
| `perf/`     | performance         |
| `refactor/` | internal cleanup    |
| `docs/`     | documentation only  |
| `test/`     | test changes        |
| `build/`    | build/dependency    |
| `ci/`       | CI config           |
| `chore/`    | maintenance         |
| `redesign/` | design or IA change |

For AI agents:

| Prefix     | Agent                    |
| ---------- | ------------------------ |
| `claude/`  | Claude Code              |
| `cursor/`  | Cursor                   |
| `copilot/` | GitHub Copilot Workspace |

## Linting and formatting

Lint is required org-wide. The org ruleset runs
[`.github/workflows/org-lint.yml`](https://github.com/mukoko-dev/.github/blob/main/.github/workflows/org-lint.yml)
from this repository on every pull request in every `mukoko-dev`
repository, and requires its five checks: `lint / actionlint`,
`lint / JSON validity`, `lint / prettier`, `lint / markdownlint` and
`lint / yamllint`.

A repository needs **no `lint.yml` and no lint config files** of its
own. Where it has a `.prettierrc`, `.prettierignore`,
`.markdownlint.jsonc` or `.yamllint.yaml` at its root, the shared
lint uses it; where it does not, the canonical copy in
[`nyuchi/.github`](https://github.com/nyuchi/.github) applies. Add one
only when the repository genuinely needs to differ.

## Code of conduct

By participating you agree to abide by our
[Code of Conduct](./CODE_OF_CONDUCT.md). Disagreement is welcome;
disrespect is not.

## Reporting security issues

**Do not** open a public issue. See [`SECURITY.md`](./SECURITY.md).

## AI-assisted contributions

If you're using an AI agent (Claude Code, Cursor, Copilot, Aider, …),
read [`AGENTS.md`](https://github.com/nyuchi/.github/blob/main/AGENTS.md) before submitting.

[cc]: https://www.conventionalcommits.org/en/v1.0.0/
