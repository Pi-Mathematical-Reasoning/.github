# Contributing

This repository holds the `Pi-Mathematical-Reasoning` organization profile:
`profile/README.md` is what [github.com/Pi-Mathematical-Reasoning](https://github.com/Pi-Mathematical-Reasoning)
shows. It has no build step and deploys nothing. Coding agents should read
[AGENTS.md](AGENTS.md).

Because it is the organization's public `.github` repository, GitHub also
uses its community health files (this `CONTRIBUTING.md`, and the pull
request templates in `.github/`) as defaults for any organization repository
that has none of its own. Every Pi repository ships its own, as the
conventions require.

## Local development setup

Requirements: Node.js 24, [pre-commit](https://pre-commit.com) and the GitHub CLI (`gh`).

```sh
git clone https://github.com/Pi-Mathematical-Reasoning/.github.git pi-mathematical-reasoning-github
cd pi-mathematical-reasoning-github
npm ci                                         # commitlint
pre-commit install --hook-type commit-msg      # local commit-msg hook
```

The `commit-msg` hook runs commitlint locally. It is a convenience only;
**CI is the authoritative check**.

Run the CI checks locally before opening a pull request:

```sh
npm test
```

## Commits

Each commit message is a **single-line** Conventional Commit with a type and a scope:

```
type(scope): subject
```

- At most 100 characters, with an imperative, lowercase subject and no trailing period.
- Types: `feat`, `fix`, `perf`, `revert`, `docs`, `refactor`, `test`, `build`, `ci`, `style`, `chore`: the same set in every repository.
- The scope is required: exactly one of the scopes below. A change that spans two is two pull requests, or takes the scope it is mostly about.
- Breaking changes are marked with `!`, for example `feat(profile)!: drop the repository table`.
- The **only** content allowed after the header is a blank line followed by `Co-authored-by: Name <email>` trailers, in any letter case. No body text. Details belong in the pull request description.

commitlint enforces this: `@commitlint/config-conventional` plus three local rules in `tools/commitlint/`. `only-coauthor-trailers` fails on any line after the header that is not blank or a `Co-authored-by` trailer, `single-scope` fails on a list such as `feat(a,b)`, and `subject-lowercase` requires a lowercase subject except in Dependabot's fixed `chore(deps): Bump ...`. The fixtures in `tests/commitlint/` show accepted and rejected messages; CI checks them, and lints the pull request title through the same config. The standard behind it is the [repository conventions](https://github.com/jwallace145/repository-template/blob/main/repository-conventions.md).

### Scopes

Every commit names exactly one of this repository's scopes. They are `SCOPES` in `commitlint.config.mjs`; this table mirrors it, in the same order, and `npm test` fails if they drift. Adding, renaming or removing a scope goes through a pull request like any other change.

| Scope | Covers |
| --- | --- |
| `profile` | `profile/README.md`, the organization's profile page |
| `templates` | The default pull request template and issue forms |
| `repo` | README, CONTRIBUTING, AGENTS, commitlint and repository tooling |
| `deps` | Dependency updates, mostly Dependabot's |
| `release` | Release Please's release pull requests and its config |

## Pull requests

- After the initial scaffold commit, **every change reaches `main` through a
  pull request**. Direct pushes are blocked.
- Merges are **squash or rebase only**. No merge commits; history is linear.
- The **title must be a valid commit header**, scope included, because a squash merge
  uses it as the commit message. Release Please's pull requests are titled
  `chore(release): release X.Y.Z`, and Dependabot's `chore(deps): Bump ...`.
- Fill in every section of the one template, `.github/pull_request_template.md`
  (Summary, Why, Changes, Verification, Checklist). Write "n/a" rather than
  deleting one. For a fix, Why names the bug, its root cause and how to reproduce it.

CI runs these jobs on every pull request, and all of them are required on
`main`:

| Job | Checks |
|---|---|
| `commitlint` | Every commit in the range `base..head`, so rebase-merged commits are all checked |
| `pr-title` | The title passes the same commitlint config as a commit, scope included |
| `commitlint-rules` | Unit tests for the local rules, the scopes table, and the fixture messages |

## Issues

Open issues from the forms: **Bug**, **Feature** or **Task**. Blank issues are off.

## Releases and versioning

Releases are managed by [Release Please](https://github.com/googleapis/release-please).

- One version for the whole repository (`release-type: simple`), tagged
  `vX.Y.Z`. Release Please owns `CHANGELOG.md` and `version.txt`; nobody edits
  them by hand.
- On every push to `main`, the `release-please` workflow opens or updates a
  release pull request authored by `pi-release-bot[bot]`, a GitHub
  App owned by the `Pi-Mathematical-Reasoning` organization (Contents and Pull requests,
  read and write). The organization secrets `RELEASE_BOT_CLIENT_ID` and
  `RELEASE_BOT_PRIVATE_KEY` are its credentials. CI runs on it like any other
  pull request.
- **Merging the Release Please pull request is the deliberate, human-reviewed
  act that releases a version.** Nobody creates tags by hand.
- The profile is live as soon as a change merges to `main`; a release only
  records it in the changelog.

| Change | Commit type | Bump |
|---|---|---|
| Removing a profile section or a default community health file | `feat!` / `fix!` | major (minor while `0.x`) |
| A new profile section or default file | `feat` | minor |
| A correction to the profile | `fix` | patch |
| `docs`, `chore`, `ci`, `refactor`, `test`, `style`, `build` | no release on their own |

## Repository settings

`scripts/apply-repo-settings.sh` applies the settings through the GitHub API:

- Squash and rebase merges only; squash commits use the pull request title and
  a blank body. Head branches are deleted after merge.
- The `bug`, `enhancement` and `task` labels the issue forms apply.
- A ruleset on `main` requires a pull request, squash or rebase merges, every
  CI job as a status check and linear history, and blocks force pushes and
  deletion, with no bypass actors.
