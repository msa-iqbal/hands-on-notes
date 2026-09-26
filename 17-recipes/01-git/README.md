# Git

Practical, step-by-step **Git recipes** for recovering lost work, rewriting history, managing commits and branches, resolving conflicts, creating tags, and migrating repositories.

## Contents

|#|Topic|What You'll Learn|
|---|---|---|
|01|Recover Deleted Branch|Recover a deleted local or remote branch using Git reflog and commit references|
|02|Recover Lost Commit|Find and restore commits that are no longer reachable from a branch or tag|
|03|Rewrite Last Commit|Amend the latest commit message, content, author information, or staged changes|
|04|Squash Commits|Combine multiple commits into a single clean commit using interactive rebase|
|05|Remove Sensitive File From History|Remove accidentally committed secrets or sensitive files from Git history and clean affected references|
|06|Undo Pushed Commit|Safely undo commits that have already been pushed using revert or history rewriting when appropriate|
|07|Resolve Merge Conflict|Identify conflicting changes, resolve conflicts, stage resolutions, and complete the merge|
|08|Create and Push Tag|Create annotated or lightweight tags and push tags to a remote repository|
|09|Migrate Repository|Move a Git repository between remotes while preserving branches, tags, and repository history|

## Structure

The recipes progress from **recovering Git data → modifying commit history → managing changes → resolving conflicts → tagging → repository migration**.

The first sections focus on recovering lost work, including deleted branches and unreachable commits. The next recipes cover modifying recent history through commit amendments and interactive rebasing.

Sensitive-file removal and pushed-commit reversal address situations where changes have already entered shared or remote history. Merge-conflict resolution then provides a structured workflow for reconciling changes from different branches.

The final recipes cover creating and publishing Git tags and migrating repositories while preserving important repository references and history.

Each recipe follows an **action-oriented workflow** with commands, expected results, verification steps, and important safety considerations.

## Focus

- Recovering deleted branches and lost commits
- Amending and rewriting commit history
- Squashing commits with interactive rebase
- Removing sensitive files from history
- Undoing pushed commits safely
- Resolving merge conflicts
- Creating and publishing Git tags
- Migrating repositories while preserving history

## Goal

> Build a practical Git recipe knowledge base that makes common recovery, history management, conflict resolution, tagging, and repository migration procedures easy to follow, verify, and reproduce.
