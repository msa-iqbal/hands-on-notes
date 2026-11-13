# GitHub Pull Requests

Pull Requests (PRs) are one of the most important collaboration features of GitHub.

They allow developers to propose changes to a repository and ask other team members to review those changes before they become part of another branch.

Pull Requests are commonly used for:

* Code reviews.
* Feature development.
* Bug fixes.
* Collaboration between developers.
* Automated testing and quality checks.
* Discussing proposed changes.
* Maintaining code quality.

A pull request connects Git branching workflows with GitHub's collaboration and review features.

# 🗁 What Is a Pull Request?

A **Pull Request** is a request to merge changes from one branch into another branch.

For example, suppose your repository has a `main` branch:

```text
main
```

You create a separate branch for a new feature:

```text
feature/user-authentication
```

After implementing the feature, you push the branch to GitHub and open a pull request asking to merge it into `main`.

```text
main
  |
  |------------------------------+
                                 |
feature/user-authentication      |
  |                              |
  +------------------------------+
                 |
          Pull Request
                 |
          Review and Merge
                 |
                main
```

A pull request provides a dedicated place to:

* Review code changes.
* Discuss implementation details.
* Request modifications.
* Run automated checks.
* Approve changes.
* Merge changes into the target branch.

**Important:** Creating a pull request does not immediately merge the changes. It starts a review and integration process.

# 🗁 Why Pull Requests Are Used

Pull requests improve collaboration and reduce the risk of introducing problematic changes.

### 🖆 Code Review

Other developers can inspect your changes before they are merged.

### 🖆 Quality Control

Reviewers can identify:

* Bugs.
* Security problems.
* Performance issues.
* Incorrect logic.
* Poor coding practices.
* Missing tests.

### 🖆 Team Collaboration

Developers can discuss implementation decisions directly alongside the relevant code changes.

### 🖆 Automated Testing

GitHub Actions and other CI systems can automatically run:

* Unit tests.
* Integration tests.
* Build processes.
* Linters.
* Security checks.

### 🖆 Change History

Pull requests provide a record of:

* What changed.
* Why it changed.
* Who proposed it.
* Who reviewed it.
* Whether checks passed.
* When it was merged.

# 🗁 Pull Request vs Issue vs Branch

These three GitHub concepts serve different purposes.

| Feature | Purpose |
| --- | --- |
| Branch | Provides an independent line of development |
| Issue | Tracks a task, bug, feature request, or discussion |
| Pull Request | Proposes changes and requests review and merging |

Example:

* **Issue:** Add user authentication.
* **Branch:** `feature/user-authentication`.
* **Pull Request:** Merge the authentication implementation into `main`.

A common workflow connects all three:

```text
Issue
  |
  v
Create Branch
  |
  v
Implement Changes
  |
  v
Push Branch
  |
  v
Create Pull Request
  |
  v
Review and Test
  |
  v
Merge Pull Request
  |
  v
Close Related Issue
```

# 🗁 Pull Request Workflow

A typical pull request workflow looks like this:

```text
main
  |
  +-------------------------------+
                                  |
                         Create Feature Branch
                                  |
                                  v
                     feature/user-authentication
                                  |
                           Make Changes
                                  |
                            Git Commits
                                  |
                            Push to GitHub
                                  |
                           Create Pull Request
                                  |
                         Automated Checks
                                  |
                           Code Review
                                  |
                         Address Feedback
                                  |
                         Approve Pull Request
                                  |
                           Merge into main
                                  |
                         Delete Feature Branch
```

The workflow can vary depending on team policies, repository permissions, and branch protection rules.

# 🗁 Understanding Base and Compare Branches

When creating a pull request, GitHub asks you to select two branches.

* **Base branch:** The branch that will receive the changes.
* **Compare branch:** The branch containing the changes you want to merge.

Example:

```text
Base:     main
Compare:  feature/login
```

This means:

> Merge the changes from `feature/login` into `main`.

Another example:

```text
Base:     develop
Compare:  feature/payment
```

This means:

> Merge the changes from `feature/payment` into `develop`.

The direction matters. Always verify the base and compare branches before creating a pull request.

# 🗁 Creating a Pull Request

You can create pull requests through the GitHub web interface.

### 🖆 Prepare Your Feature Branch

Start with an existing repository:

```bash
git clone https://github.com/username/project.git

cd project
```

Update your local `main` branch:

```bash
git switch main

git pull origin main
```

Create a feature branch:

```bash
git switch -c feature/user-authentication
```

### 🖆 Make Changes and Commit

Modify your project files.

Check the changes:

```bash
git status
```

Stage the changes:

```bash
git add .
```

Create a commit:

```bash
git commit -m "Add user authentication"
```

### 🖆 Push the Branch

Push your feature branch to GitHub:

```bash
git push -u origin feature/user-authentication
```

### 🖆 Open GitHub

Navigate to your repository on GitHub.

GitHub may display a notification suggesting that you create a pull request for your recently pushed branch.

Select **Compare & pull request**.

Alternatively:

* Open the **Pull requests** tab.
* Select **New pull request**.
* Choose the base branch.
* Choose the compare branch.

### 🖆 Complete the Pull Request

Provide:

* A descriptive title.
* A detailed description.
* Relevant issue references.
* Reviewers, if needed.
* Labels, if applicable.

Select **Create pull request**.

Your pull request is now open.

# 🗁 Creating a Pull Request with GitHub CLI

GitHub CLI (`gh`) allows you to create and manage pull requests from the terminal.

First, authenticate:

```bash
gh auth login
```

Check authentication:

```bash
gh auth status
```

Create a pull request:

```bash
gh pr create
```

Create a pull request with a title and body:

```bash
gh pr create \
  --base main \
  --head feature/user-authentication \
  --title "Add user authentication" \
  --body "Implements the user authentication feature."
```

Create a draft pull request:

```bash
gh pr create --draft
```

Open the pull request in your browser:

```bash
gh pr create --web
```

# 🗁 Pull Request Title and Description

A good pull request title and description help reviewers understand the purpose of the changes.

### 🖆 Title

Use a short and descriptive title.

Good examples:

```text
Add user authentication
Fix login validation error
Improve API response handling
Update database connection configuration
```

Avoid unclear titles:

```text
Update
Changes
Fix
New feature
```

### 🖆 Description

A useful description explains:

* What changed?
* Why was the change necessary?
* How was it implemented?
* How can it be tested?
* Are there any related issues?

Example:

```markdown
# 🗁 Summary

Implement user authentication using JWT.

# 🗁 Changes

- Add login endpoint.
- Add registration endpoint.
- Implement JWT token generation.
- Add authentication middleware.
- Add validation for login credentials.

# 🗁 Testing

- Tested successful login.
- Tested invalid credentials.
- Tested protected API endpoints.

# 🗁 Related Issues

Closes #24
```

A clear description reduces unnecessary review discussions.

# 🗁 Draft Pull Requests

A **Draft Pull Request** is an incomplete pull request that is not yet ready for formal review or merging.

Draft pull requests are useful when:

* Development is still in progress.
* You want early feedback.
* You need to share implementation ideas.
* You want to discuss an approach before completing it.

Create a draft pull request using GitHub CLI:

```bash
gh pr create --draft
```

On GitHub, you can also select the draft option when creating a pull request.

When the work is ready, select **Ready for review**.

A draft pull request normally cannot be merged until it is marked ready for review.

# 🗁 Reviewers and Assignees

Pull requests support assigning people to different responsibilities.

### 🖆 Reviewers

Reviewers inspect the proposed changes and provide feedback.

They can:

* Comment on changes.
* Approve the pull request.
* Request changes.

### 🖆 Assignees

Assignees are responsible for handling the pull request.

For example:

| Role | Responsibility |
| --- | --- |
| Author | Implements and submits changes |
| Reviewer | Inspects and evaluates changes |
| Assignee | Takes responsibility for the pull request |
| Maintainer | Manages repository integration and policies |

You can request reviewers from the pull request page.

With GitHub CLI:

```bash
gh pr edit 42 --add-reviewer developer1,developer2
```

Assign a pull request:

```bash
gh pr edit 42 --add-assignee developer1
```

Replace `42` with the actual pull request number.

# 🗁 Reviewing Pull Requests

Code review is an important part of the pull request process.

A reviewer should examine the proposed changes before approving them.

### 🖆 Review Process

Open the pull request and select the **Files changed** tab.

Inspect:

* Modified files.
* Added files.
* Deleted files.
* Code quality.
* Application logic.
* Security implications.
* Test coverage.
* Potential regressions.

If necessary, add comments to specific lines.

### 🖆 Reviewing with GitHub CLI

View a pull request:

```bash
gh pr view 42
```

View its changes:

```bash
gh pr diff 42
```

Check out the pull request locally:

```bash
gh pr checkout 42
```

This allows you to test the proposed changes in your local environment.

# 🗁 Review Decisions

GitHub supports different review outcomes.

### 🖆 Comment

The reviewer provides feedback without explicitly approving or requesting changes.

### 🖆 Approve

The reviewer approves the proposed changes.

### 🖆 Request Changes

The reviewer identifies problems that should be addressed before merging.

Example workflow:

```text
Pull Request Created
        |
        v
    Code Review
        |
        +-------------------+
        |                   |
        v                   v
     Approved          Changes Requested
        |                   |
        v                   v
     Ready to           Author Updates
      Merge                 |
                            v
                       New Review
                            |
                            v
                         Approved
```

Repository rules may require approvals from one or more reviewers before merging.

# 🗁 Review Comments and Conversations

Reviewers can leave comments on individual lines of code or participate in general discussions.

### 🖆 Inline Comments

Inline comments are attached to specific lines in the diff.

Example:

```text
File: src/auth.js

Line: 42

Comment:
Consider handling the case where the token is missing.
```

### 🖆 Review Threads

Related comments can form a conversation thread.

The author can respond, update the implementation, and resolve the conversation when the issue has been addressed.

### 🖆 Resolving Conversations

A reviewer or authorized participant can mark a conversation as resolved.

This helps keep the review focused on unresolved issues.

**Best practice:** Do not resolve a conversation simply to remove it from view. Make sure the underlying concern has actually been addressed.

# 🗁 Files Changed and Diff Review

The **Files changed** tab displays the differences between the base and compare branches.

GitHub uses diff notation:

```diff
 function greet(name) {
-  return "Hello";
+  return `Hello, ${name}`;
 }
```

Interpretation:

* Lines beginning with `-` were removed.
* Lines beginning with `+` were added.
* Unchanged lines provide surrounding context.

Reviewers can inspect individual files, add comments, and review the complete change set.

You can also inspect changes locally:

```bash
git diff main...feature/user-authentication
```

The three-dot comparison shows changes introduced by the feature branch relative to the common ancestor of the two branches.

# 🗁 Pull Request Checks

GitHub can run automated checks against pull requests.

These checks commonly include:

* Unit tests.
* Integration tests.
* Build verification.
* Linting.
* Type checking.
* Security scanning.
* Deployment validation.

For example, a GitHub Actions workflow might run when a pull request targets `main`:

```yaml
name: Pull Request Checks

on:
  pull_request:
    branches:
      - main

jobs:
  test:
    runs-on: ubuntu-latest

    steps:
      - name: Checkout repository
        uses: actions/checkout@v4

      - name: Setup Node.js
        uses: actions/setup-node@v4
        with:
          node-version: 22

      - name: Install dependencies
        run: npm ci

      - name: Run tests
        run: npm test

      - name: Build project
        run: npm run build
```

The pull request displays the status of checks after they run.

Common statuses include:

| Status | Meaning |
| --- | --- |
| Success | Check completed successfully |
| Failure | Check failed |
| Pending | Check is still running |
| Skipped | Check did not run |
| Neutral | Check completed without a success or failure conclusion |

Repositories can configure required status checks through branch protection rules or repository rulesets.

A pull request may be blocked from merging until required checks pass.

# 🗁 Linking Pull Requests to Issues

GitHub supports automatically closing issues when a pull request is merged.

Use closing keywords in the pull request description or a qualifying commit message.

Common keywords include:

```text
Closes #24
Fixes #24
Resolves #24
```

Example:

```markdown
# 🗁 Summary

Implement user authentication.

# 🗁 Changes

- Add login API.
- Add registration API.
- Add JWT authentication middleware.

# 🗁 Related Issues

Closes #24
```

When the pull request is merged into the repository's default branch, the referenced issue is automatically closed.

Other supported keywords include:

```text
close
closes
closed

fix
fixes
fixed

resolve
resolves
resolved
```

The closing reference must point to the appropriate issue, and the pull request must be merged into the default branch for automatic closure.

# 🗁 Updating a Pull Request

A pull request automatically reflects new commits pushed to its compare branch.

Suppose you have an open pull request from:

```text
feature/user-authentication
```

You discover a problem and make another commit:

```bash
git add .

git commit -m "Fix authentication validation"

git push origin feature/user-authentication
```

GitHub automatically updates the existing pull request.

You do not need to create another pull request.

This is one of the main advantages of using branches for pull request workflows.

# 🗁 Handling Merge Conflicts

A merge conflict occurs when Git cannot automatically combine changes from different branches.

For example:

* `main` modifies a particular line.
* `feature/login` modifies the same line differently.
* Git cannot determine which version should be retained.

You can resolve conflicts locally.

Update your local base branch:

```bash
git switch main

git pull origin main
```

Switch to your feature branch:

```bash
git switch feature/login
```

Merge the updated base branch into your feature branch:

```bash
git merge main
```

If conflicts occur, Git marks the affected files.

Example:

```text
<<<<<<< HEAD
const message = "Login successful";
=======
const message = "Authentication successful";
>>>>>>> main
```

The conflict markers represent:

* `<<<<<<< HEAD`: The current branch's version.
* `=======`: Separates the conflicting versions.
* `>>>>>>> main`: The incoming branch's version.

Edit the file and keep the correct implementation.

After resolving conflicts:

```bash
git add .

git commit
```

Push the updated feature branch:

```bash
git push origin feature/login
```

The pull request will update automatically.

**Alternative:** You can rebase your feature branch onto the updated base branch instead of merging it. Choose the strategy according to your team's workflow.

# 🗁 Merging a Pull Request

Once the required reviews and checks are complete, an authorized user can merge the pull request.

Open the pull request and select the available merge option.

GitHub commonly supports three merge methods:

* Create a merge commit.
* Squash and merge.
* Rebase and merge.

The available options depend on repository settings.

# 🗁 Merge Methods

### 🖆 Create a Merge Commit

This method combines the branches using a merge commit.

Command-line equivalent:

```bash
git switch main

git merge feature/login
```

Example:

```text
       A---B---C  feature/login
      /         \
A---D---E-------M  main
```

The merge commit `M` joins the histories of both branches.

Advantages:

* Preserves individual commits.
* Preserves the branch's development history.
* Clearly records the merge operation.

Disadvantages:

* Can produce a more complex commit graph.
* Many small feature commits may clutter the main branch history.

### 🖆 Squash and Merge

This method combines all pull request changes into a single commit on the base branch.

Example:

```text
A---B---C  feature/login

A---D---S  main
```

Here, `S` represents one commit containing the combined changes.

Advantages:

* Keeps the main branch history clean.
* Combines multiple development commits into one.
* Useful for feature branches with many temporary commits.

Disadvantages:

* Individual pull request commits are not preserved as separate commits in the base branch history.
* Original commit-level development history is not represented in the target branch's commit sequence.

### 🖆 Rebase and Merge

This method applies the pull request commits onto the base branch without creating a merge commit.

Example:

```text
Before:

A---B---C  main
     \
      D---E  feature/login


After:

A---B---C---D'---E'  main
```

The commits are replayed onto the base branch.

The resulting commits have new commit identities because their parent relationships change.

Advantages:

* Produces a linear history.
* Avoids an additional merge commit.
* Preserves individual changes as separate commits.

Disadvantages:

* Rewrites commit identities.
* Does not preserve the original branch topology in the target branch history.

### 🖆 Merge Method Comparison

| Method | History | Merge Commit | Commit Structure |
| --- | --- | --- | --- |
| Merge commit | Preserves branch topology | Yes | Multiple original commits |
| Squash and merge | Simplified | No | One combined commit |
| Rebase and merge | Linear | No | Individual rebased commits |

**Important:** GitHub's Rebase and merge option is not exactly the same as manually rebasing a local branch and then merging it. GitHub applies the pull request commits onto the base branch as new commits.

# 🗁 Auto-Merge

Auto-merge allows GitHub to merge a pull request automatically when the repository's required conditions are satisfied.

For example:

* Required approvals are received.
* Required status checks pass.
* Required conversations are resolved.
* Other repository rules are satisfied.

Enable auto-merge from the pull request interface when the feature is available.

Using GitHub CLI:

```bash
gh pr merge 42 --auto --squash
```

This requests automatic squash merging when the required conditions are met.

Auto-merge must be enabled for the repository, and the pull request must satisfy the repository's applicable requirements.

# 🗁 Deleting the Branch After Merging

After merging a pull request, the feature branch may no longer be needed.

GitHub provides an option to delete the branch after a successful merge.

You can also delete it manually.

Delete the remote branch:

```bash
git push origin --delete feature/login
```

Delete the local branch:

```bash
git branch -d feature/login
```

If the local branch has unmerged changes and you intentionally want to remove it:

```bash
git branch -D feature/login
```

Use the force-delete command carefully because it removes the local branch reference even when Git considers its changes unmerged.

# 🗁 Reverting a Pull Request

Sometimes a merged pull request introduces a bug or unwanted behavior.

GitHub provides a **Revert** option for eligible merged pull requests.

Reverting creates a new change that reverses the changes introduced by the original pull request.

You can also revert the merge commit manually.

For a merge commit:

```bash
git revert -m 1 <merge-commit-hash>
```

The `-m 1` option selects the first parent as the mainline.

For a squash-merged pull request, revert the squash commit:

```bash
git revert <squash-commit-hash>
```

After reverting locally, push the resulting commit:

```bash
git push origin main
```

**Important:** Reverting does not erase the original pull request or rewrite repository history. It creates a new commit that reverses previous changes.

# 🗁 Pull Request Templates

A pull request template provides a standard structure for descriptions.

GitHub supports repository-level pull request templates.

Create a directory:

```text
.github/
```

Create a file:

```text
.github/pull_request_template.md
```

Example:

```markdown
# 🗁 Summary

Describe the changes introduced by this pull request.

# 🗁 Changes

- Change A
- Change B
- Change C

# 🗁 Testing

Describe how the changes were tested.

- [ ] Unit tests pass.
- [ ] Integration tests pass.
- [ ] Build succeeds.

# 🗁 Checklist

- [ ] Code follows project conventions.
- [ ] Documentation has been updated.
- [ ] No unnecessary files were added.
- [ ] Changes have been self-reviewed.

# 🗁 Related Issues

Closes #
```

GitHub can use this template when contributors create pull requests.

For multiple templates, create files inside:

```text
.github/PULL_REQUEST_TEMPLATE/
```

For example:

```text
.github/
└── PULL_REQUEST_TEMPLATE/
    ├── feature.md
    ├── bugfix.md
    └── documentation.md
```

# 🗁 Working with Forks

Fork-based workflows are commonly used in open-source projects.

A fork is a separate GitHub repository created from another repository.

Contributors can make changes in their own fork without having direct write access to the original repository.

Example:

```text
Original Repository
       |
       v
Fork Repository
       |
       v
Clone Fork Locally
       |
       v
Create Feature Branch
       |
       v
Make Changes
       |
       v
Push to Fork
       |
       v
Create Pull Request
       |
       v
Original Repository
```

### 🖆 Fork Workflow

Fork the repository using GitHub.

Clone your fork:

```bash
git clone https://github.com/your-username/project.git

cd project
```

Add the original repository as an upstream remote:

```bash
git remote add upstream https://github.com/original-owner/project.git
```

Check remotes:

```bash
git remote -v
```

Create a feature branch:

```bash
git switch -c feature/new-feature
```

Make changes and commit:

```bash
git add .

git commit -m "Add new feature"
```

Push to your fork:

```bash
git push -u origin feature/new-feature
```

Open a pull request from your fork's feature branch to the original repository's target branch.

### 🖆 Keep Your Fork Updated

Fetch changes from the original repository:

```bash
git fetch upstream
```

Update your local main branch:

```bash
git switch main

git merge upstream/main
```

Push the updated branch to your fork:

```bash
git push origin main
```

You can also use GitHub's fork synchronization interface.

# 🗁 Managing Pull Requests with GitHub CLI

GitHub CLI provides commands for managing pull requests without opening a browser.

### 🖆 List Pull Requests

```bash
gh pr list
```

List all pull requests:

```bash
gh pr list --state all
```

List open pull requests:

```bash
gh pr list --state open
```

### 🖆 View a Pull Request

```bash
gh pr view 42
```

Open a pull request in the browser:

```bash
gh pr view 42 --web
```

### 🖆 Create a Pull Request

```bash
gh pr create
```

### 🖆 Check Out a Pull Request

```bash
gh pr checkout 42
```

### 🖆 View Pull Request Changes

```bash
gh pr diff 42
```

### 🖆 Review a Pull Request

Approve:

```bash
gh pr review 42 --approve
```

Request changes:

```bash
gh pr review 42 --request-changes \
  --body "Please improve the input validation."
```

Submit a general comment:

```bash
gh pr review 42 --comment \
  --body "The implementation looks good overall."
```

### 🖆 Merge a Pull Request

Merge with a merge commit:

```bash
gh pr merge 42 --merge
```

Squash and merge:

```bash
gh pr merge 42 --squash
```

Rebase and merge:

```bash
gh pr merge 42 --rebase
```

### 🖆 Close a Pull Request

```bash
gh pr close 42
```

Reopen a pull request:

```bash
gh pr reopen 42
```

### 🖆 Check Pull Request Status

```bash
gh pr checks 42
```

### 🖆 Mark a Draft as Ready

```bash
gh pr ready 42
```

### 🖆 Edit a Pull Request

```bash
gh pr edit 42 --title "Improve authentication"

gh pr edit 42 --body "Updated pull request description."
```

Replace `42` with the relevant pull request number.

# 🖆 Pull Request Best Practices

### 🖆 Keep Pull Requests Small

Prefer focused pull requests that solve one problem.

Small pull requests are easier to:

* Review.
* Test.
* Understand.
* Debug.
* Merge.

### 🖆 Use Descriptive Titles

A pull request title should clearly communicate its purpose.

Example:

```text
Add JWT authentication middleware
```

### 🖆 Explain the Reason for Changes

Describe the problem being solved and the approach used.

### 🖆 Include Tests

Add or update tests whenever application behavior changes.

### 🖆 Review Your Own Changes

Before creating a pull request, inspect the diff:

```bash
git diff main...HEAD
```

Remove:

* Debugging statements.
* Unnecessary files.
* Temporary changes.
* Unrelated modifications.

### 🖆 Keep Feature Branches Updated

Regularly synchronize your feature branch with the target branch to reduce integration conflicts.

### 🖆 Respond to Review Feedback

Address review comments carefully and explain any implementation decisions that need clarification.

### 🖆 Avoid Unnecessary Force Pushes

Force-pushing can rewrite shared branch history and may complicate reviews.

If rewriting history is necessary on your own feature branch, prefer:

```bash
git push --force-with-lease
```

This is safer than `git push --force` because it checks whether the remote branch has changed unexpectedly.

### 🖆 Use Automated Checks

Configure CI workflows to run tests and builds on pull requests.

### 🖆 Protect Important Branches

Use branch protection rules or repository rulesets to enforce:

* Required reviews.
* Required status checks.
* Conversation resolution.
* Restricted direct pushes.
* Other repository policies.

# 🖆 Common Problems and Troubleshooting

### 🖆 Pull Request Cannot Be Created

Possible causes:

* The branch has not been pushed.
* The compare branch does not exist remotely.
* There are no changes to propose.
* The user does not have the necessary repository access.

Check remote branches:

```bash
git branch -r
```

Push your feature branch:

```bash
git push -u origin feature/login
```

### 🖆 Pull Request Has No Commits

Possible causes:

* The wrong base branch was selected.
* The feature branch contains no new commits.
* Changes have already been merged into the base branch.

Inspect the branches:

```bash
git log --oneline main..feature/login
```

### 🖆 Merge Conflicts Exist

Update the feature branch with the latest base branch:

```bash
git fetch origin

git switch feature/login

git merge origin/main
```

Resolve conflicts and push the updated branch.

### 🖆 Required Checks Are Failing

Inspect the failed workflow or status check.

For GitHub Actions:

* Open the **Checks** tab.
* Select the failed workflow.
* Inspect the logs.
* Fix the underlying problem.
* Push the corrected changes.

### 🖆 Approval Is Required

Some repositories require one or more approvals before merging.

Request a review from an authorized maintainer or reviewer.

### 🖆 Pull Request Is Out of Date

The base branch may have received new commits.

Update your feature branch:

```bash
git fetch origin

git switch feature/login

git merge origin/main
```

Push the updated branch:

```bash
git push origin feature/login
```

### 🖆 Changes Are Missing from the Pull Request

Check that the correct branch was pushed:

```bash
git branch --show-current

git status

git log --oneline
```

Push the expected branch:

```bash
git push origin feature/login
```

### 🖆 Pull Request Cannot Be Merged

Check:

* Required reviews.
* Required status checks.
* Merge conflicts.
* Unresolved conversations.
* Branch protection rules.
* Repository permissions.
* Whether the pull request is still a draft.

# 🖆 Practical Exercises

### 🖆 Exercise: Create Your First Pull Request

* Create a GitHub repository.
* Clone it locally.
* Create a feature branch.
* Add a new file.
* Commit your changes.
* Push the feature branch.
* Create a pull request targeting `main`.
* Review the changes.
* Merge the pull request.

### 🖆 Exercise: Work with Pull Request Reviews

* Create a feature branch.
* Make a code change.
* Open a pull request.
* Ask another developer to review it.
* Add a review comment.
* Request changes.
* Update the code.
* Push the changes.
* Complete the review.

### 🖆 Exercise: Use GitHub CLI

Practice these commands:

```bash
gh pr create

gh pr list

gh pr view

gh pr diff

gh pr checkout

gh pr checks

gh pr review

gh pr merge
```

### 🖆 Exercise: Build a Fork-Based Workflow

* Fork an existing repository.
* Clone your fork.
* Add the original repository as `upstream`.
* Create a feature branch.
* Implement a change.
* Push the branch to your fork.
* Open a pull request against the original repository.

### 🖆 Exercise: Configure a Pull Request Template

* Create `.github/pull_request_template.md`.
* Add summary and testing sections.
* Add a checklist.
* Open a new pull request.
* Verify that GitHub uses the template.

# 🖆 Interview Questions

**What is a pull request?**

A pull request is a GitHub feature that allows developers to propose changes from one branch to another, request code reviews, run automated checks, and merge approved changes.

**What is the difference between a pull request and a merge?**

A pull request is the collaboration and review process. A merge is the operation that integrates changes from one branch into another.

**What are base and compare branches?**

The base branch receives the changes, while the compare branch contains the changes being proposed.

**What is a draft pull request?**

A draft pull request indicates that proposed changes are not yet ready for formal review or merging.

**What are the three common pull request merge methods?**

* Create a merge commit.
* Squash and merge.
* Rebase and merge.

**What is the difference between squash and merge and rebase and merge?**

Squash and merge combines the pull request changes into one commit. Rebase and merge applies the individual pull request commits onto the base branch as new commits.

**What happens when you push new commits to a pull request branch?**

The existing pull request automatically updates with the new commits and changes.

**Can you create a pull request without direct write access to the original repository?**

Yes. You can fork the repository, push changes to your fork, and open a pull request against the original repository.

**What happens when a pull request is merged?**

The proposed changes are integrated into the target branch using the selected merge method. Related issues may also close automatically if supported closing keywords are used.

**What is auto-merge?**

Auto-merge allows GitHub to merge a pull request automatically once the repository's required conditions are satisfied.

**How can you resolve merge conflicts in a pull request?**

Update the feature branch with the latest base branch, resolve conflicts locally, commit the resolution if needed, and push the updated branch.

**Why are code reviews important?**

Code reviews help identify defects, improve maintainability, enforce standards, share knowledge, and reduce the risk of problematic changes entering important branches.

# 🖆 RESOURCES & REFERENCES

> Explore the documentation, tools, guides, and references behind the topic.

**Official References**

* [GitHub Pull Requests Documentation](https://docs.github.com/en/pull-requests)
* [About Pull Requests](https://docs.github.com/en/pull-requests/collaborating-with-pull-requests/proposing-changes-to-your-work-with-pull-requests/about-pull-requests)
* [Creating a Pull Request](https://docs.github.com/en/pull-requests/collaborating-with-pull-requests/creating-a-pull-request)
* [Reviewing Changes in Pull Requests](https://docs.github.com/en/pull-requests/collaborating-with-pull-requests/reviewing-changes-in-pull-requests)
* [Merging a Pull Request](https://docs.github.com/en/pull-requests/collaborating-with-pull-requests/merging-a-pull-request)
* [GitHub CLI: Pull Requests](https://cli.github.com/manual/gh_pr)
* [Git Documentation](https://git-scm.com/doc)
