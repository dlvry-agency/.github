# DLVRY branch policy

Version 1.0 — 2026-10-07. Company baseline for DLVRY repositories.

## Branches and pull requests

| Branch        | Purpose                                 | Allowed sources                                                      | Merge method                                                     |
| ------------- | --------------------------------------- | -------------------------------------------------------------------- | ---------------------------------------------------------------- |
| `main`        | Stable, releasable code; default branch | `develop`, or an urgent `hotfix/*` based on `main`                   | Merge commit                                                     |
| `develop`     | Integrated work for the next release    | Short-lived work branches; `main` to synchronize a release or hotfix | Squash for work branches; merge commit for `main`                |
| Work branches | One focused change                      | Start from up-to-date `develop`                                      | Pull request into `develop`                                      |
| `hotfix/*`    | Urgent correction to the stable branch  | Start from up-to-date `main`                                         | Pull request into `main`, then synchronize `main` into `develop` |

Use `feat/<short-description>`, `fix/<short-description>`, `chore/<short-description>`, `docs/<short-description>`, `refactor/<short-description>`, `test/<short-description>`, or `ci/<short-description>`. `codex/<short-description>` is also supported for existing tooling. Use lowercase kebab-case. Branch names and source-to-target routing are conventions reviewed in the pull request; the baseline rulesets do not enforce their syntax or routing.

Keep `main` and `develop` permanent. Do not commit directly, force-push, rewrite, rename, or delete either stable branch during normal work. Keep `main` as the repository default and explicitly choose `develop` as the base of ordinary pull requests.

## Required review and verification

- Every change to a stable branch goes through a pull request.
- Require one approval from another eligible reviewer. A second login belonging to the author is not an independent review.
- Dismiss stale approvals when new changes are pushed. The latest reviewable push must be approved by someone other than its pusher.
- Resolve review conversations before merging.
- Require the repository's real CI check to pass with the branch up to date. In `dlvry-platforms`, the check is `verify` from the Verify workflow, provided by GitHub Actions.
- No permanent bypass actors, including administrators or automation. Emergency exceptions require an owner decision, a recorded reason and prompt restoration of the policy.
- Never merge a failing or still-running check. A green local run is not proof of a green remote job, including cleanup steps.

Rulesets apply only where the GitHub plan supports enforcement. GitHub Free does not enforce these protections on private organization repositories. A saved ruleset or a successful CI run is not a substitute for server-side enforcement. A private repository is not protection-ready until the organization uses a supporting plan and enforcement has been verified.

## Merge and release discipline

Use Conventional Commit pull request titles, for example `feat(catalog): add collection search`. Squash short-lived branches into `develop`, using the pull request title for the resulting commit. Keep merge commits enabled and rebase merging disabled at repository level.

Release with a pull request from `develop` to `main`, using a merge commit. Then open a synchronization pull request from `main` to `develop`, also using a merge commit. This preserves shared ancestry. Never squash or rebase merges between the two permanent branches. If a release needs conflict resolution, merge the current `main` into `develop` through a reviewed synchronization branch and pull request before retrying the release.

Hotfixes enter `main` by pull request, with the same review and CI requirements. Follow immediately with a `main` to `develop` synchronization pull request. Do not leave the fix only on `main`.

Delete short-lived branches after merge when no other pull request uses them. Keep automatic head-branch deletion off while protection is unenforced, because a release pull request uses `develop` as its head. It can be enabled after deletion protection on both permanent branches is confirmed.

Create release tags only from an approved `main` commit. This policy does not deploy software or grant access to environments. Deployment approvals and release-tag protections must be configured separately before production releases.

## Working locally

```bash
git fetch origin --prune
git switch develop
git pull --ff-only origin develop
git switch -c feat/short-description
# Make and verify the change.
git push -u origin feat/short-description
# Open a pull request with base: develop.
```

Do not discard local changes or use a hard reset to update a branch. Resolve divergence explicitly. Prefer fresh short-lived branches and small pull requests.

## Adopt this policy in another repository

1. Establish CI and a successful default-branch run first. Confirm its exact check name and GitHub App source.
2. Create `develop` from the current, verified `main` commit without rewriting history.
3. Import the JSON templates from `governance/rulesets/` in the company `.github` repository. Replace the `verify` check only when the target repository uses a different real CI gate. Never require a nonexistent check.
4. Require PRs, one independent approval, resolved conversations, current CI, no deletion and no force-push on both stable branches. Keep the bypass list empty.
5. Permit only merge commits into `main`; permit squash and merge into `develop` for the distinct flows above. Do not require linear history: it conflicts with the permanent-branch merge strategy.
6. Keep the repository private unless its owner explicitly approves publication. Do not change visibility to obtain free protection.
7. Verify the resulting rules and plan entitlement; test an unapproved pull request and a failing CI result are blocked before calling the repository protection-ready.
8. Use this contribution guide and the company pull request template. Repository-specific templates override inherited ones.

Company-wide automatic enforcement requires an organization ruleset targeting the intended repositories and a supporting GitHub plan. Repository imports do not automatically protect future repositories. For a small profile-only or documentation repository, an owner can explicitly choose a documented single-branch exception with protected `main` and an appropriate CI gate.
