# Git

Practical, structured **Git troubleshooting guides** for diagnosing merge conflicts, repository-state problems, lost commits, branch issues, push failures, authentication errors, remote problems, accidental commits, and repository corruption.

## Contents

| #   | Topic                  | What You'll Learn                                                                                                                                                            |
| --- | ---------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| 01  | Merge Conflict         | Identify conflicting changes, inspect conflict markers, resolve conflicts safely, stage resolved files, and complete the merge                                               |
| 02  | Detached HEAD          | Understand detached HEAD states, identify the current commit, preserve work, create or switch to a branch, and return to normal branch-based development                     |
| 03  | Lost Commit            | Locate unreachable or missing commits using reflog and repository history, recover the required commit, and verify the recovered work                                        |
| 04  | Deleted Branch         | Recover deleted local or remote branches, locate the branch tip, recreate the branch reference, and verify its history                                                       |
| 05  | Rejected Push          | Diagnose push rejection, inspect local and remote history, synchronize changes safely, resolve conflicts when required, and retry the push                                   |
| 06  | Non-Fast-Forward       | Understand non-fast-forward errors, compare local and remote histories, integrate remote changes, and update the remote branch safely                                        |
| 07  | Authentication Failure | Diagnose Git authentication problems, verify credentials, SSH keys, tokens, credential helpers, repository permissions, and remote configuration                             |
| 08  | Remote Not Found       | Diagnose missing or incorrect remote repositories, inspect remote URLs, verify repository access, correct the remote configuration, and test connectivity                    |
| 09  | Wrong Branch           | Identify the current and intended branch, preserve uncommitted or committed work, move changes safely, and restore the correct branch state                                  |
| 10  | Accidental Commit      | Identify unintended commits, determine whether they are local or already pushed, undo or rewrite history safely, and verify the resulting repository state                   |
| 11  | Repository Corruption  | Identify signs of repository corruption, inspect Git objects and references, run repository integrity checks, recover available data, and restore a healthy repository state |

## Structure

The guides progress from **working-tree and merge problems → repository-state problems → commit and branch recovery → push and history synchronization → authentication and remote issues → accidental changes → repository corruption**.

The first sections focus on common Git workflow problems. Merge conflicts and detached HEAD states are handled first because they frequently occur during normal branching, merging, rebasing, and commit operations.

Commit and branch recovery then covers lost commits and deleted branches. These guides use Git history and reflog information to locate otherwise difficult-to-find objects and restore the required repository references.

Push and synchronization troubleshooting covers rejected pushes and non-fast-forward errors. These guides explain how local and remote histories differ and how to integrate changes without unnecessarily overwriting existing work.

Authentication and remote troubleshooting then addresses failures caused by credentials, SSH keys, access permissions, repository URLs, or remote configuration. The wrong-branch and accidental-commit guides cover mistakes made during everyday Git operations and provide safe recovery approaches based on whether changes have been committed or pushed.

The final guide addresses repository corruption and focuses on integrity checks, object and reference inspection, recovery possibilities, and verification of the repaired repository.

Each troubleshooting guide follows a **problem-oriented workflow** covering symptoms, possible causes, diagnosis, solution, verification, prevention, and related issues.

## Focus

- Merge conflicts and branch synchronization
- Detached HEAD and repository-state problems
- Lost commits and deleted branches
- Rejected pushes and non-fast-forward errors
- Git authentication and access failures
- Remote URL and repository configuration
- Wrong-branch and accidental-commit recovery
- Repository integrity and corruption
- Reflog, history, references, and Git diagnostics
- Safe recovery and verification procedures

## Goal

> Build a practical Git troubleshooting knowledge base that makes common repository, branch, commit, synchronization, authentication, remote, recovery, and integrity problems easier to identify, diagnose, resolve, verify, and prevent.
