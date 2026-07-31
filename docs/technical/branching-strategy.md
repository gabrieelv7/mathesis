## Description

Define and document the branching strategy used in the Mathesis repository.

The strategy should establish how development branches are created, named, reviewed, merged, and deleted. The `main` branch must remain stable and should only receive changes through reviewed pull requests.

## Proposed strategy

The project will use a simplified GitHub Flow:

1. Create an issue for the work.
2. Create a branch from the latest `main`.
3. Implement and commit the changes.
4. Push the branch to GitHub.
5. Open a pull request targeting `main`.
6. Wait for automated checks and review.
7. Merge the pull request.
8. Delete the branch after the merge.

## Branch naming convention

Branches must include the issue number and a short description.

```text
feature/<issue-number>-<description>
fix/<issue-number>-<description>
docs/<issue-number>-<description>
refactor/<issue-number>-<description>
test/<issue-number>-<description>
chore/<issue-number>-<description>