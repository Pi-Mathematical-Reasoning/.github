# Rules for coding agents

This file restates the repository conventions for coding agents. The
human-facing version, with setup details, is [CONTRIBUTING.md](CONTRIBUTING.md).

## Non-negotiable

- **Never push to `main`.** Every change reaches `main` through a pull request.
  Direct pushes are blocked by a branch ruleset; do not try to work around it.
- **Commits are a single line**: `type(scope): subject`, at most 100 characters, with a lowercase imperative subject and no trailing period. Types: `feat`, `fix`, `perf`, `revert`, `docs`, `refactor`, `test`, `build`, `ci`, `style`, `chore`; never invent another. Scope: exactly one of `SCOPES` in `commitlint.config.mjs` (the Scopes table in CONTRIBUTING.md); never invent one, and if none fits, ask. The only thing allowed after the header is a blank line and `Co-authored-by: Name <email>` trailers. No body text. Put details in the PR description.
- **PR titles are valid commit headers**, scope included; squash merges use the title as the commit message.
- **PR bodies use the template's sections, in order, every one filled in**; write "n/a" rather than deleting one:

  ```sh
  gh pr create --title "type(scope): subject" --body-file <file>
  ```
- **Merges are squash or rebase only.** History stays linear.
- **Releases are made by merging the Release Please pull request**, which is
  authored by `pi-release-bot[bot]`, the organization's own app. Do
  not create tags or releases by hand, and do not edit `CHANGELOG.md` or
  `version.txt`.
- **Run the checks before opening a pull request:**

  ```sh
  npm test
  ```
