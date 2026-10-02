# Contributing

This file is the **organisation-wide default** for the
[`bundu-labs`](https://github.com/bundu-labs) GitHub org. Every
repository inherits it unless it ships its own `CONTRIBUTING.md`.

The canonical engineering working agreement is at
[`nyuchi/.github → CONTRIBUTING.md`](https://github.com/nyuchi/.github/blob/main/CONTRIBUTING.md).
The terms below mirror the relevant sections; downstream
`bundu-labs` repos may add **stricter** rules but cannot **relax** them.

If you are reading this for the first time, also read:

- [`CODE_OF_CONDUCT.md`](./CODE_OF_CONDUCT.md) — how we treat each
  other.
- [`SECURITY.md`](./SECURITY.md) — how to report vulnerabilities.
- [`SUPPORT.md`](./SUPPORT.md) — where to get help.
- [`AGENTS.md`](./AGENTS.md) — rules for AI-assisted contributions.

## Quick start

1. **Fork** the repo (external) or **branch** it (member).
2. Work on a branch that matches our [branch-naming rules](#branch-naming).
3. Open a PR with a [Conventional Commits][cc] title, signed commits,
   and a DCO sign-off. Green CI + at least one approving review and
   we'll merge it.

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

PR title lint runs on every PR via the org-wide
[`reusable-pr-title-lint.yml`](https://github.com/nyuchi/.github/blob/main/.github/workflows/reusable-pr-title-lint.yml)
workflow.

## Signed commits (required)

Every commit landing on `main` must show **Verified** on GitHub —
either GPG or SSH signed. Branch-protection enforces this.

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

Each consumer repo ships its own
[`.editorconfig`](./.editorconfig),
[`.prettierrc`](./.prettierrc),
[`.markdownlint.jsonc`](./.markdownlint.jsonc), and
[`.yamllint.yaml`](./.yamllint.yaml) — copy them from this repo as a
starting point. Run the org-wide reusable lint workflow to enforce
them in CI:

```yaml
jobs:
  lint:
    uses: nyuchi/.github/.github/workflows/reusable-lint.yml@main
```

## Code of conduct

By participating you agree to abide by our
[Code of Conduct](./CODE_OF_CONDUCT.md). Disagreement is welcome;
disrespect is not.

## Reporting security issues

**Do not** open a public issue. See [`SECURITY.md`](./SECURITY.md).

## AI-assisted contributions

If you're using an AI agent (Claude Code, Cursor, Copilot, Aider, …),
read [`AGENTS.md`](./AGENTS.md) before submitting.

[cc]: https://www.conventionalcommits.org/en/v1.0.0/
