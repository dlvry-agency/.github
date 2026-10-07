## Change

Explain the problem and resulting behavior. Link the issue when relevant.

## Verification

List the checks performed and their results. Include screenshots for visible changes.

## Merge route

- [ ] Ordinary change: work branch → `develop`, squash merge.
- [ ] Release: `develop` → `main`, merge commit; follow with a sync PR.
- [ ] Hotfix: `hotfix/*` → `main`, merge commit; follow with a sync PR.
- [ ] Synchronization: `main` → `develop`, merge commit.

Select the applicable route. Preserve both permanent branches.

## Ready for review

- [ ] Relevant automated checks pass; the latest remote CI result is green.
- [ ] Documentation and meaningful regression coverage are updated where needed.
- [ ] Secrets, generated artifacts and customer data are absent from the diff.
- [ ] Risks, migration and rollback steps are explained when applicable.
- [ ] An independent reviewer has approved the latest changes before merge.
