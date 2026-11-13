# GitHub Actions

GitHub Actions is GitHub's automation and CI/CD platform.

It allows developers to automate tasks directly inside GitHub repositories.

Common automation tasks include:

* Running tests whenever code is pushed.
* Checking code quality.
* Building applications.
* Deploying applications.
* Publishing packages.
* Automating repository maintenance.
* Creating release workflows.
* Running scheduled jobs.

GitHub Actions is based on workflow files written in YAML.

These files define when automation should run, which machines should execute it, and what commands or actions should be performed.

# 🗁 What Is GitHub Actions?

**GitHub Actions** is a CI/CD and workflow automation service integrated into GitHub.

It allows you to define automated workflows that respond to repository events.

For example, whenever a developer pushes code to the `main` branch, GitHub Actions can automatically:

* Install project dependencies.
* Run automated tests.
* Check code formatting.
* Build the application.
* Deploy the application if all required checks pass.

Example workflow:

```yaml
name: Node.js CI

on:
  push:
    branches:
      - main

jobs:
  build:
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

      - name: Build application
        run: npm run build
```

This workflow automatically runs when code is pushed to `main`.

# 🗁 Why Use GitHub Actions?

GitHub Actions helps eliminate repetitive manual tasks.

### 🖆 Continuous Integration

Automatically test code whenever changes are pushed.

### 🖆 Continuous Delivery

Automatically prepare and validate application releases.

### 🖆 Continuous Deployment

Automatically deploy applications when predefined conditions are satisfied.

### 🖆 Code Quality

Run tools such as:

* ESLint.
* Prettier.
* TypeScript.
* SonarQube.
* Security scanners.

### 🖆 Repository Automation

Automate tasks such as:

* Labeling issues.
* Closing stale issues.
* Creating release notes.
* Publishing packages.
* Updating dependencies.

### 🖆 Developer Productivity

Developers can focus on implementation while GitHub handles repetitive workflows.

# 🗁 GitHub Actions Architecture

GitHub Actions uses several connected components.

```text
GitHub Repository
       |
       v
Workflow File
(.github/workflows/*.yml)
       |
       v
Workflow Trigger
(push, pull_request, schedule)
       |
       v
Workflow
       |
       v
Jobs
       |
       v
Runner
       |
       v
Steps
       |
       +----------------------+
       |                      |
       v                      v
    Commands               Actions
       |                      |
       +----------+-----------+
                  |
                  v
          Workflow Results
                  |
                  v
          GitHub Actions UI
```

The workflow defines the automation.

The trigger determines when it runs.

Jobs contain steps.

Runners execute the jobs.

Actions provide reusable functionality.

# 🗁 Core Concepts

Understanding these terms is essential before creating workflows.

| Concept | Description |
| --- | --- |
| Workflow | Automated process defined in a YAML file |
| Event | An occurrence that triggers a workflow |
| Job | A group of steps executed on a runner |
| Step | An individual task inside a job |
| Action | A reusable unit of automation |
| Runner | Machine that executes a job |
| Artifact | File or collection of files produced by a workflow |
| Secret | Encrypted configuration value |
| Variable | Repository, organization, or environment configuration value |
| Environment | Deployment target with optional protection rules |

Example:

```text
Workflow
  |
  +-- Job: Test
  |     |
  |     +-- Step: Checkout code
  |     +-- Step: Install dependencies
  |     +-- Step: Run tests
  |
  +-- Job: Build
        |
        +-- Step: Checkout code
        +-- Step: Install dependencies
        +-- Step: Build application
```

# 🗁 Workflow Files

GitHub Actions workflows are stored inside:

```text
.github/workflows/
```

Example repository structure:

```text
project/
│
├── src/
├── tests/
├── package.json
│
└── .github/
    └── workflows/
        ├── ci.yml
        ├── deploy.yml
        └── release.yml
```

Each YAML file defines a separate workflow.

For example:

```text
.github/workflows/ci.yml
```

A repository can contain multiple workflow files.

You can organize workflows according to their responsibilities:

* `ci.yml` — Continuous integration.
* `deploy.yml` — Application deployment.
* `release.yml` — Release automation.
* `security.yml` — Security scanning.

Workflow filenames are not restricted to these names.

# 🗁 Workflow Syntax

A GitHub Actions workflow uses YAML syntax.

Basic structure:

```yaml
name: Workflow Name

on:
  push:
    branches:
      - main

permissions:
  contents: read

jobs:
  job-name:
    runs-on: ubuntu-latest

    steps:
      - name: Step Name
        run: echo "Hello GitHub Actions"
```

### 🖆 name

Defines the workflow name displayed in GitHub.

```yaml
name: Node.js CI
```

### 🖆 on

Defines the events that trigger the workflow.

```yaml
on:
  push:
```

### 🖆 permissions

Controls the access token permissions available to the workflow.

```yaml
permissions:
  contents: read
```

### 🖆 jobs

Defines the jobs that belong to the workflow.

```yaml
jobs:
  build:
```

### 🖆 runs-on

Specifies the runner environment.

```yaml
runs-on: ubuntu-latest
```

### 🖆 steps

Defines the individual tasks executed by a job.

```yaml
steps:
  - name: Print message
    run: echo "Workflow started"
```

# 🗁 Workflow Triggers and Events

Events determine when workflows execute.

GitHub Actions supports many event types.

### 🖆 push

Runs when commits are pushed to a repository.

```yaml
on:
  push:
```

Restrict execution to a branch:

```yaml
on:
  push:
    branches:
      - main
```

### 🖆 pull_request

Runs when pull request activity occurs.

```yaml
on:
  pull_request:
```

You can specify which pull request activities trigger the workflow:

```yaml
on:
  pull_request:
    types:
      - opened
      - synchronize
      - reopened
```

Here:

* `opened` — A pull request is opened.
* `synchronize` — New commits are pushed to its source branch.
* `reopened` — A closed pull request is reopened.

### 🖆 workflow_dispatch

Allows users to start a workflow manually.

```yaml
on:
  workflow_dispatch:
```

### 🖆 schedule

Runs a workflow according to a cron schedule.

```yaml
on:
  schedule:
    - cron: "0 0 * * *"
```

This schedule runs daily at midnight UTC.

### 🖆 release

Runs when release-related events occur.

```yaml
on:
  release:
    types:
      - published
```

### 🖆 issues

Runs when issue activity occurs.

```yaml
on:
  issues:
    types:
      - opened
      - closed
```

### 🖆 workflow_run

Runs a workflow after another workflow completes.

```yaml
on:
  workflow_run:
    workflows:
      - Node.js CI
    types:
      - completed
```

### 🖆 Multiple Events

A workflow can respond to multiple events:

```yaml
name: CI

on:
  push:
    branches:
      - main

  pull_request:
    branches:
      - main

  workflow_dispatch:
```

This workflow runs when:

* Code is pushed to `main`.
* A pull request targets `main`.
* A user manually starts the workflow.

# 🗁 Running Workflows on Specific Branches

You can control which branches trigger a workflow.

### 🖆 Include Specific Branches

```yaml
on:
  push:
    branches:
      - main
      - develop
```

### 🖆 Exclude Specific Branches

```yaml
on:
  push:
    branches-ignore:
      - experimental
      - archive
```

### 🖆 Include or Exclude Paths

You can also filter by changed file paths.

```yaml
on:
  push:
    paths:
      - "src/**"
      - "package.json"
```

Exclude specific paths:

```yaml
on:
  push:
    paths-ignore:
      - "docs/**"
      - "*.md"
```

Branch and path filters help avoid running unnecessary workflows.

**Important:** GitHub applies branch and path filters together when both are configured for the same event. A workflow runs only when the applicable filters match.

# 🗁 Jobs

A job is a group of steps executed on a runner.

Example:

```yaml
jobs:
  test:
    runs-on: ubuntu-latest

    steps:
      - name: Run tests
        run: npm test

  build:
    runs-on: ubuntu-latest

    steps:
      - name: Build application
        run: npm run build
```

By default, jobs can run in parallel.

### 🖆 Job Dependencies

Use `needs` to specify that a job depends on another job.

```yaml
jobs:
  test:
    runs-on: ubuntu-latest

    steps:
      - run: echo "Running tests"

  build:
    needs: test
    runs-on: ubuntu-latest

    steps:
      - run: echo "Building application"

  deploy:
    needs: build
    runs-on: ubuntu-latest

    steps:
      - run: echo "Deploying application"
```

Execution order:

```text
Test
  |
  v
Build
  |
  v
Deploy
```

If a required job fails or is skipped, dependent jobs are normally skipped unless their conditions explicitly allow them to run.

### 🖆 Job-Level Conditions

Use `if` to control whether a job executes.

```yaml
jobs:
  deploy:
    if: github.ref == 'refs/heads/main'
    runs-on: ubuntu-latest

    steps:
      - run: echo "Deploying main branch"
```

# 🗁 Steps

Steps are individual operations inside a job.

A step can:

* Execute shell commands.
* Use an existing action.
* Set environment variables.
* Upload artifacts.
* Download artifacts.

### 🖆 Using run

The `run` keyword executes shell commands.

```yaml
steps:
  - name: Display message
    run: echo "Hello World"

  - name: Check Node version
    run: node --version

  - name: Install dependencies
    run: npm ci
```

### 🖆 Multiple Commands

```yaml
steps:
  - name: Build project
    run: |
      npm ci
      npm run lint
      npm test
      npm run build
```

The pipe character `|` allows multiple commands in a YAML block.

### 🖆 Working Directory

You can define a working directory for a step:

```yaml
steps:
  - name: Run frontend tests
    run: npm test
    working-directory: frontend
```

### 🖆 Shell

Specify which shell executes a command:

```yaml
steps:
  - name: Run Bash command
    shell: bash
    run: |
      echo "Running Bash"
      pwd
```

# 🗁 Actions

An action is a reusable unit of automation.

Instead of writing every operation manually, you can use actions created by GitHub, the community, or your organization.

Example:

```yaml
steps:
  - name: Checkout repository
    uses: actions/checkout@v4
```

This action checks out the repository code into the runner's workspace.

### 🖆 Common GitHub Actions

| Action | Purpose |
| --- | --- |
| `actions/checkout` | Checks out repository code |
| `actions/setup-node` | Configures Node.js |
| `actions/setup-python` | Configures Python |
| `actions/setup-java` | Configures Java |
| `actions/upload-artifact` | Uploads workflow artifacts |
| `actions/download-artifact` | Downloads workflow artifacts |
| `actions/cache` | Caches dependencies and build data |

Example:

```yaml
steps:
  - name: Checkout code
    uses: actions/checkout@v4

  - name: Setup Node.js
    uses: actions/setup-node@v4
    with:
      node-version: 22
```

The `with` keyword passes input parameters to an action.

### 🖆 Pinning Actions

Actions are referenced using a version or Git reference.

Example:

```yaml
uses: actions/checkout@v4
```

For stronger supply-chain security, organizations can pin actions to verified full-length commit SHAs.

Example format:

```yaml
uses: owner/action@FULL_40_CHARACTER_COMMIT_SHA
```

A full commit SHA is more immutable than a movable version tag.

# 🗁 Runners

A runner is the machine that executes a GitHub Actions job.

GitHub provides hosted runners, and you can also configure self-hosted runners.

### 🖆 GitHub-Hosted Runners

GitHub provides managed virtual machines with supported operating systems.

Common runner labels include:

```yaml
runs-on: ubuntu-latest
```

```yaml
runs-on: windows-latest
```

```yaml
runs-on: macos-latest
```

The available operating system images and hardware depend on GitHub's runner offerings and your plan.

### 🖆 Self-Hosted Runners

A self-hosted runner is a machine that you configure and manage yourself.

It can be:

* A physical server.
* A virtual machine.
* A cloud instance.
* An on-premises machine.

Example:

```yaml
jobs:
  build:
    runs-on: self-hosted

    steps:
      - run: echo "Running on a self-hosted runner"
```

You can also target custom runner labels:

```yaml
runs-on: [self-hosted, linux, x64]
```

### 🖆 GitHub-Hosted vs Self-Hosted Runners

| Feature | GitHub-Hosted | Self-Hosted |
| --- | --- | --- |
| Infrastructure | Managed by GitHub | Managed by you |
| Setup | Minimal | Requires configuration |
| Maintenance | Managed by GitHub | Your responsibility |
| Custom hardware | Limited to offered options | Greater flexibility |
| Network access | Hosted environment restrictions | Can access configured internal networks |
| Cost | Depends on plan and usage | Infrastructure and maintenance costs |

Self-hosted runners require careful security management, especially when running workflows from untrusted pull requests.

# 🗁 Environment Variables

Environment variables provide configuration values to workflow steps and jobs.

Use `env` to define them.

### 🖆 Workflow-Level Variables

```yaml
name: Environment Example

on:
  push:

env:
  APP_NAME: my-application

jobs:
  build:
    runs-on: ubuntu-latest

    steps:
      - name: Display application name
        run: echo "$APP_NAME"
```

### 🖆 Job-Level Variables

```yaml
jobs:
  build:
    runs-on: ubuntu-latest

    env:
      NODE_ENV: production

    steps:
      - run: echo "$NODE_ENV"
```

### 🖆 Step-Level Variables

```yaml
steps:
  - name: Run command
    env:
      API_VERSION: v1
    run: echo "$API_VERSION"
```

### 🖆 Environment Variable Scope

| Scope | Availability |
| --- | --- |
| Workflow | All jobs and steps in the workflow |
| Job | All steps in that job |
| Step | Only that step |

For cross-platform workflows, use the `env` context or environment files rather than assuming a particular shell's variable syntax.

# 🗁 Contexts and Expressions

GitHub Actions provides contexts that expose information about workflow execution.

Common contexts include:

| Context | Description |
| --- | --- |
| `github` | Information about the workflow run and repository |
| `env` | Environment variables |
| `vars` | Configuration variables |
| `secrets` | Available secrets |
| `steps` | Outputs and results from previous steps |
| `job` | Current job information |
| `jobs` | Outputs from reusable workflow jobs |
| `runner` | Runner environment information |
| `matrix` | Current matrix configuration |
| `needs` | Results and outputs from dependent jobs |
| `inputs` | Workflow or action inputs |

### 🖆 Using the github Context

```yaml
steps:
  - name: Display repository
    run: echo "${{ github.repository }}"
```

### 🖆 Using Expressions

Expressions are written inside:

```text
${{ expression }}
```

Example:

```yaml
if: ${{ github.ref == 'refs/heads/main' }}
```

GitHub also supports certain expression contexts directly in `if` conditions without explicitly writing `${{ }}`.

### 🖆 Common Functions

```yaml
if: contains(github.event.head_commit.message, 'release')
```

Other useful functions include:

* `contains()`
* `startsWith()`
* `endsWith()`
* `format()`
* `join()`
* `toJSON()`
* `fromJSON()`
* `hashFiles()`

Example:

```yaml
steps:
  - name: Check changed package files
    if: ${{ hashFiles('package-lock.json') != '' }}
    run: echo "Package lock file exists"
```

**Security note:** Do not directly interpolate untrusted event data into shell commands. Use environment variables and appropriate input validation to reduce script injection risks.

# 🗁 Secrets and Variables

GitHub supports secrets and configuration variables for managing workflow configuration.

### 🖆 Secrets

Secrets store sensitive values such as:

* API tokens.
* Deployment credentials.
* Cloud provider keys.
* Database passwords.
* Private service credentials.

Secrets are encrypted and can be managed at repository, organization, and environment levels, subject to permissions and plan availability.

Access a secret:

```yaml
steps:
  - name: Deploy application
    env:
      DEPLOY_TOKEN: ${{ secrets.DEPLOY_TOKEN }}
    run: ./deploy.sh
```

### 🖆 Variables

Variables store non-sensitive configuration values.

Examples:

* Application environment.
* Deployment region.
* API endpoint.
* Feature configuration.
* Project identifiers.

Access a variable:

```yaml
steps:
  - name: Display deployment region
    run: echo "${{ vars.DEPLOY_REGION }}"
```

### 🖆 Secrets vs Variables

| Feature | Secrets | Variables |
| --- | --- | --- |
| Sensitive information | Yes | No |
| Typical use | Tokens and passwords | Configuration |
| Access context | `secrets` | `vars` |
| Storage | Encrypted | Configuration value |
| Intended for public output | No | Depends on value |

**Important:** Never print secrets in workflow logs. Masking is a useful safeguard, but it does not make unsafe handling acceptable.

# 🗁 Artifacts

Artifacts allow workflows to preserve and share files produced during execution.

Common examples:

* Build outputs.
* Test reports.
* Coverage reports.
* Log files.
* Compiled applications.
* Deployment packages.

### 🖆 Upload Artifacts

```yaml
steps:
  - name: Build application
    run: npm run build

  - name: Upload build output
    uses: actions/upload-artifact@v4
    with:
      name: production-build
      path: dist/
```

### 🖆 Download Artifacts

A different job can download an artifact from the same workflow run.

```yaml
jobs:
  build:
    runs-on: ubuntu-latest

    steps:
      - name: Create build
        run: |
          mkdir dist
          echo "Build output" > dist/index.html

      - name: Upload artifact
        uses: actions/upload-artifact@v4
        with:
          name: production-build
          path: dist/

  deploy:
    needs: build
    runs-on: ubuntu-latest

    steps:
      - name: Download artifact
        uses: actions/download-artifact@v4
        with:
          name: production-build

      - name: Inspect downloaded files
        run: ls -la
```

Artifacts are useful when jobs run on separate machines and need to share generated files.

# 🗁 Caching Dependencies

Dependency installation can consume significant workflow execution time.

Caching allows workflows to reuse previously downloaded dependencies or generated data.

Example for Node.js:

```yaml
steps:
  - name: Checkout repository
    uses: actions/checkout@v4

  - name: Setup Node.js
    uses: actions/setup-node@v4
    with:
      node-version: 22
      cache: npm

  - name: Install dependencies
    run: npm ci
```

The `setup-node` action supports dependency caching.

### 🖆 Using actions/cache

You can also configure caching directly:

```yaml
steps:
  - name: Cache npm dependencies
    uses: actions/cache@v4
    with:
      path: ~/.npm
      key: ${{ runner.os }}-npm-${{ hashFiles('package-lock.json') }}
      restore-keys: |
        ${{ runner.os }}-npm-
```

The cache key identifies a cache entry.

If the dependency lock file changes, the hash changes and GitHub can create a new cache entry.

**Cache vs Artifact**

| Cache | Artifact |
| --- | --- |
| Reuses data between workflow runs | Preserves workflow output |
| Improves execution speed | Shares or stores generated files |
| Can be replaced or evicted | Has an artifact retention period |
| Example: dependency cache | Example: compiled application |

# 🗁 Workflow Dependencies

Jobs can depend on other jobs.

Use `needs` to control execution order.

Example:

```yaml
jobs:
  test:
    runs-on: ubuntu-latest
    steps:
      - run: echo "Testing"

  build:
    needs: test
    runs-on: ubuntu-latest
    steps:
      - run: echo "Building"

  deploy:
    needs: build
    runs-on: ubuntu-latest
    steps:
      - run: echo "Deploying"
```

Execution:

```text
Test
  |
  v
Build
  |
  v
Deploy
```

Multiple jobs can depend on the same job:

```yaml
jobs:
  test:
    runs-on: ubuntu-latest
    steps:
      - run: echo "Testing"

  build:
    needs: test
    runs-on: ubuntu-latest
    steps:
      - run: echo "Building"

  security:
    needs: test
    runs-on: ubuntu-latest
    steps:
      - run: echo "Security scanning"

  deploy:
    needs:
      - build
      - security
    runs-on: ubuntu-latest
    steps:
      - run: echo "Deploying"
```

The build and security jobs can run in parallel after the test job succeeds.

# 🗁 Conditional Execution

The `if` keyword controls whether a job or step executes.

### 🖆 Branch Condition

```yaml
steps:
  - name: Deploy to production
    if: github.ref == 'refs/heads/main'
    run: echo "Production deployment"
```

### 🖆 Event Condition

```yaml
steps:
  - name: Handle pull request
    if: github.event_name == 'pull_request'
    run: echo "Pull request workflow"
```

### 🖆 Failure Condition

```yaml
steps:
  - name: Run tests
    run: npm test

  - name: Display failure message
    if: failure()
    run: echo "Tests failed"
```

### 🖆 Always Run

```yaml
steps:
  - name: Cleanup
    if: always()
    run: echo "Cleanup step"
```

Common status-check functions:

| Function | Purpose |
| --- | --- |
| `success()` | All required previous steps or jobs succeeded |
| `failure()` | A previous step or job failed |
| `always()` | Evaluates true even after cancellation or failure |
| `cancelled()` | Workflow or job was cancelled |

Use `always()` carefully for cleanup and reporting tasks. It does not guarantee that a runner can continue executing after every possible cancellation or infrastructure failure.

# 🗁 Matrix Builds

Matrix builds allow the same job to run with multiple configurations.

For example, test an application against multiple Node.js versions.

```yaml
name: Node.js Matrix Testing

on:
  push:

jobs:
  test:
    runs-on: ubuntu-latest

    strategy:
      matrix:
        node-version: [20, 22, 24]

    steps:
      - name: Checkout repository
        uses: actions/checkout@v4

      - name: Setup Node.js
        uses: actions/setup-node@v4
        with:
          node-version: ${{ matrix.node-version }}

      - name: Install dependencies
        run: npm ci

      - name: Run tests
        run: npm test
```

GitHub creates separate job executions for each matrix value.

```text
Test Job
   |
   +-- Node.js 20
   |
   +-- Node.js 22
   |
   +-- Node.js 24
```

### 🖆 Multiple Matrix Dimensions

```yaml
strategy:
  matrix:
    os: [ubuntu-latest, windows-latest]
    node-version: [20, 22]
```

This creates four combinations:

* Ubuntu with Node.js 20.
* Ubuntu with Node.js 22.
* Windows with Node.js 20.
* Windows with Node.js 22.

Matrix strategies can also use `include` and `exclude` to customize combinations.

# 🗁 Reusable Workflows

Reusable workflows allow one workflow to call another workflow.

This helps avoid duplicating CI/CD configuration across repositories or workflow files.

The called workflow must use `workflow_call`.

Example:

```yaml
# .github/workflows/reusable-test.yml

name: Reusable Test Workflow

on:
  workflow_call:

jobs:
  test:
    runs-on: ubuntu-latest

    steps:
      - name: Checkout repository
        uses: actions/checkout@v4

      - name: Run tests
        run: npm test
```

Call the reusable workflow:

```yaml
# .github/workflows/ci.yml

name: CI

on:
  push:

jobs:
  test:
    uses: ./.github/workflows/reusable-test.yml
```

Reusable workflows can accept inputs, secrets, and outputs.

They are useful when several workflows need the same job structure.

# 🗁 Composite Actions

Composite actions package multiple steps into a reusable action.

Unlike reusable workflows, composite actions are used inside a job's steps.

Example directory structure:

```text
.github/
└── actions/
    └── setup-project/
        └── action.yml
```

Create `action.yml`:

```yaml
name: Setup Project
description: Install Node.js dependencies

runs:
  using: composite
  steps:
    - name: Setup Node.js
      uses: actions/setup-node@v4
      with:
        node-version: 22

    - name: Install dependencies
      shell: bash
      run: npm ci
```

Use the composite action:

```yaml
steps:
  - name: Checkout repository
    uses: actions/checkout@v4

  - name: Setup project
    uses: ./.github/actions/setup-project
```

### 🖆 Reusable Workflow vs Composite Action

| Feature | Reusable Workflow | Composite Action |
| --- | --- | --- |
| Purpose | Reuse complete workflows or jobs | Reuse a collection of steps |
| Invocation | Job-level `uses` | Step-level `uses` |
| Can define jobs | Yes | No |
| Can run independently | Through workflow invocation | No |
| Best suited for | Shared CI/CD pipelines | Shared setup or task sequences |

# 🗁 GitHub Actions for CI

Continuous Integration (CI) is the practice of frequently integrating code changes and automatically validating them.

A typical CI workflow performs:

```text
Code Push
   |
   v
Checkout Code
   |
   v
Install Dependencies
   |
   v
Run Linter
   |
   v
Run Tests
   |
   v
Build Application
   |
   v
CI Result
```

Example Node.js CI workflow:

```yaml
name: Node.js CI

on:
  push:
    branches:
      - main
      - develop

  pull_request:
    branches:
      - main
      - develop

permissions:
  contents: read

jobs:
  ci:
    runs-on: ubuntu-latest

    steps:
      - name: Checkout repository
        uses: actions/checkout@v4

      - name: Setup Node.js
        uses: actions/setup-node@v4
        with:
          node-version: 22
          cache: npm

      - name: Install dependencies
        run: npm ci

      - name: Run lint
        run: npm run lint

      - name: Run tests
        run: npm test

      - name: Build application
        run: npm run build
```

This workflow validates changes before they are merged.

Your `package.json` must define the scripts used by the workflow.

# 🗁 GitHub Actions for CD

Continuous Delivery and Continuous Deployment are related practices.

**Continuous Delivery** ensures that changes are automatically built, tested, and prepared for release. Deployment to production may require manual approval.

**Continuous Deployment** automatically releases changes to production when all required conditions pass.

A deployment workflow commonly follows this structure:

```text
Code Push
   |
   v
Run Tests
   |
   v
Build Application
   |
   v
Upload Build Artifact
   |
   v
Deploy to Server
   |
   v
Verify Deployment
```

Example deployment workflow structure:

```yaml
name: Deploy Application

on:
  push:
    branches:
      - main

permissions:
  contents: read

jobs:
  deploy:
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

      - name: Build application
        run: npm run build

      - name: Deploy application
        env:
          DEPLOY_TOKEN: ${{ secrets.DEPLOY_TOKEN }}
        run: ./scripts/deploy.sh
```

This example assumes the repository contains a deployment script and a configured secret.

The actual deployment process depends on your hosting provider and infrastructure.

# 🗁 Deployment Environments

GitHub Environments allow you to define deployment targets such as:

* Development.
* Staging.
* Production.

Each environment can have its own:

* Secrets.
* Variables.
* Deployment protection rules.
* Required reviewers.
* Deployment history.

Example:

```yaml
jobs:
  deploy:
    runs-on: ubuntu-latest

    environment: production

    steps:
      - name: Deploy application
        run: echo "Deploying to production"
```

Before running the job, GitHub applies the protection rules configured for the selected environment.

For example, production deployment can require approval from an authorized reviewer.

# 🗁 Permissions and Security

GitHub Actions workflows can access repository resources using the automatically provided `GITHUB_TOKEN`.

The token has permissions determined by repository or organization settings and workflow-level configuration.

Use the minimum permissions required.

Example:

```yaml
permissions:
  contents: read
```

For a workflow that needs to create repository contents:

```yaml
permissions:
  contents: write
```

Avoid granting write access when read access is sufficient.

### 🖆 Job-Level Permissions

You can define permissions for individual jobs:

```yaml
jobs:
  test:
    runs-on: ubuntu-latest

    permissions:
      contents: read

    steps:
      - uses: actions/checkout@v4
      - run: npm test
```

### 🖆 Important Security Practices

* Never commit secrets to workflow files.
* Use GitHub Secrets for sensitive credentials.
* Grant only necessary token permissions.
* Review third-party actions before using them.
* Prefer full-length commit SHA pinning for critical actions.
* Avoid executing untrusted pull request code with privileged credentials.
* Protect deployment environments.
* Avoid printing tokens or sensitive values in logs.
* Review permissions for workflows triggered by external contributors.
* Keep dependencies and actions updated.

### 🖆 Pull Request Security

Workflows triggered by `pull_request` events from forks generally receive restricted permissions and do not receive repository secrets.

Be especially careful with `pull_request_target`.

This event runs in the base repository's security context. Combining it with checkout and execution of untrusted pull request code can expose privileged tokens or secrets.

Use it only when you understand the security implications.

# 🗁 Scheduled Workflows

The `schedule` event allows workflows to run automatically at specific times.

It uses cron syntax.

Example:

```yaml
name: Daily Maintenance

on:
  schedule:
    - cron: "0 0 * * *"

jobs:
  maintenance:
    runs-on: ubuntu-latest

    steps:
      - name: Run maintenance
        run: echo "Daily maintenance task"
```

Cron format:

```text
* * * * *
| | | | |
| | | | +-- Day of week
| | | +---- Month
| | +------ Day of month
| +-------- Hour
+---------- Minute
```

Examples:

| Cron | Schedule |
| --- | --- |
| `0 0 * * *` | Every day at midnight UTC |
| `0 6 * * *` | Every day at 06:00 UTC |
| `0 0 * * 1` | Every Monday at midnight UTC |
| `0 0 1 * *` | First day of every month |
| `*/15 * * * *` | Every 15 minutes |

Scheduled workflows use UTC.

They run against the latest commit on the default branch. GitHub may delay scheduled runs during periods of high load, and scheduled workflows can be disabled in certain inactive public repositories.

# 🗁 Manual Workflow Execution

Use `workflow_dispatch` to allow users to trigger workflows manually.

Basic example:

```yaml
name: Manual Deployment

on:
  workflow_dispatch:

jobs:
  deploy:
    runs-on: ubuntu-latest

    steps:
      - name: Deploy application
        run: echo "Manual deployment started"
```

### 🖆 Manual Inputs

You can define inputs:

```yaml
name: Manual Deployment

on:
  workflow_dispatch:
    inputs:
      environment:
        description: Deployment environment
        required: true
        type: choice
        options:
          - staging
          - production

jobs:
  deploy:
    runs-on: ubuntu-latest

    steps:
      - name: Display selected environment
        run: echo "Deploying to ${{ inputs.environment }}"
```

The user can select the deployment environment when starting the workflow.

**Important:** The workflow file must exist on the default branch for the manual workflow to be available through GitHub's workflow interface.

# 🗁 Managing Workflows with GitHub CLI

GitHub CLI supports workflow management from the terminal.

### 🖆 List Workflows

```bash
gh workflow list
```

### 🖆 View Workflow Details

```bash
gh workflow view ci.yml
```

### 🖆 Run a Workflow Manually

```bash
gh workflow run ci.yml
```

Provide an input:

```bash
gh workflow run deploy.yml -f environment=staging
```

### 🖆 List Workflow Runs

```bash
gh run list
```

### 🖆 View a Workflow Run

```bash
gh run view RUN_ID
```

### 🖆 View Logs

```bash
gh run view RUN_ID --log
```

### 🖆 Watch a Running Workflow

```bash
gh run watch RUN_ID
```

### 🖆 Download Artifacts

```bash
gh run download RUN_ID
```

### 🖆 Re-run a Workflow

```bash
gh run rerun RUN_ID
```

### 🖆 Cancel a Workflow Run

```bash
gh run cancel RUN_ID
```

### 🖆 Disable a Workflow

```bash
gh workflow disable ci.yml
```

### 🖆 Enable a Workflow

```bash
gh workflow enable ci.yml
```

Replace `RUN_ID` with the actual workflow run identifier.

# 🖆 Monitoring and Troubleshooting

GitHub provides a dedicated **Actions** tab for monitoring workflow execution.

You can inspect:

* Workflow runs.
* Job execution.
* Step results.
* Execution logs.
* Failed commands.
* Artifacts.
* Execution duration.

### 🖆 Workflow Does Not Start

Possible causes:

* Incorrect YAML syntax.
* Workflow file is outside `.github/workflows/`.
* Trigger configuration does not match the event.
* Branch or path filters exclude the change.
* Workflow is disabled.
* Required workflow permissions or repository settings are not satisfied.

Verify the workflow file location:

```text
.github/workflows/ci.yml
```

Validate YAML formatting and review the event configuration.

### 🖆 YAML Syntax Error

Example incorrect YAML:

```yaml
jobs:
  build:
  runs-on: ubuntu-latest
```

Correct version:

```yaml
jobs:
  build:
    runs-on: ubuntu-latest
```

YAML indentation is significant.

### 🖆 Command Not Found

Example:

```text
npm: command not found
```

Possible solution: configure the required runtime before executing commands.

```yaml
- name: Setup Node.js
  uses: actions/setup-node@v4
  with:
    node-version: 22
```

### 🖆 Permission Denied

Possible causes:

* Missing file permissions.
* Insufficient `GITHUB_TOKEN` permissions.
* Repository or organization restrictions.
* Incorrect deployment credentials.

Check the workflow's `permissions` configuration and the repository settings.

### 🖆 Secret Is Empty

Possible causes:

* Incorrect secret name.
* Secret not configured.
* Secret unavailable to the triggering event.
* Environment protection or configuration issue.

Check the secret configuration without printing its value.

### 🖆 Job Is Skipped

Possible causes:

* A failed dependency job.
* An `if` condition evaluated to false.
* Branch or event conditions did not match.

Inspect the job dependencies and conditions.

### 🖆 Artifact Not Found

Possible causes:

* Upload step failed.
* Incorrect artifact name.
* Artifact belongs to a different workflow run.
* Artifact has expired.

Verify the upload and download steps use matching artifact names.

### 🖆 Cache Not Restored

Possible causes:

* Cache key changed.
* Cache was never created.
* Cache was evicted.
* Restore key does not match an available cache.

Inspect the cache action logs and key configuration.

# 🖆 Best Practices

### 🖆 Keep Workflows Focused

Create separate workflows for unrelated responsibilities.

For example:

```text
.github/workflows/
├── ci.yml
├── deploy.yml
├── security.yml
└── release.yml
```

### 🖆 Use Minimal Permissions

Start with read-only permissions and grant additional access only when required.

### 🖆 Cache Dependencies

Use dependency caching to reduce repeated installation time.

### 🖆 Use Reusable Workflows

Avoid duplicating the same workflow configuration across multiple repositories.

### 🖆 Separate CI and CD

Keep testing and deployment responsibilities logically separated.

### 🖆 Protect Production Deployments

Use GitHub Environments and required reviewers for sensitive deployments.

### 🖆 Pin Third-Party Actions

Use verified full-length commit SHAs where supply-chain security requires immutable references.

### 🖆 Avoid Hardcoding Secrets

Use GitHub Secrets and environment-specific configuration.

### 🖆 Keep Workflows Maintainable

Use meaningful workflow, job, and step names.

### 🖆 Test Workflows Before Relying on Them

Verify workflow behavior on pull requests and non-production branches before using it for production deployments.

### 🖆 Monitor Workflow Usage

Review execution time, failed runs, and unnecessary workflow triggers.

# 🖆 Practical Exercises

### 🖆 Exercise: Create Your First Workflow

* Create a GitHub repository.
* Add a `.github/workflows/` directory.
* Create `hello.yml`.
* Configure a `push` event.
* Add a job using `ubuntu-latest`.
* Add a step that prints a message.
* Push the workflow file.
* Inspect the execution in the Actions tab.

### 🖆 Exercise: Build a Node.js CI Pipeline

* Create a Node.js project.
* Add a `package.json`.
* Configure test and build scripts.
* Create a CI workflow.
* Configure Node.js using `actions/setup-node`.
* Install dependencies with `npm ci`.
* Run tests.
* Build the application.

### 🖆 Exercise: Configure Pull Request Checks

* Create a feature branch.
* Add a pull request workflow.
* Trigger it using `pull_request`.
* Configure tests and linting.
* Open a pull request.
* Inspect the check results.

### 🖆 Exercise: Use Secrets

* Create a test secret in repository settings.
* Access it through `${{ secrets.SECRET_NAME }}`.
* Pass it to a step using `env`.
* Verify that the workflow does not expose the secret in logs.

### 🖆 Exercise: Work with Artifacts

* Create a build directory.
* Generate a sample file.
* Upload it using `actions/upload-artifact`.
* Download it in another job.
* Verify that the file exists.

### 🖆 Exercise: Create a Matrix Workflow

* Configure multiple Node.js versions.
* Run the same tests against each version.
* Inspect the separate matrix job executions.

### 🖆 Exercise: Create a Deployment Workflow

* Configure a staging environment.
* Add a deployment script.
* Store required credentials in GitHub Secrets.
* Configure the deployment job.
* Add environment protection rules.
* Test the deployment process.

# 🖆 Interview Questions

**What is GitHub Actions?**

GitHub Actions is GitHub's automation and CI/CD platform that allows developers to execute workflows in response to repository events.

**What is a workflow?**

A workflow is an automated process defined in a YAML file inside `.github/workflows/`.

**What is a runner?**

A runner is a machine that executes GitHub Actions jobs.

**What is the difference between a job and a step?**

A job is a group of steps executed on a runner. A step is an individual task inside a job.

**What is an action?**

An action is a reusable unit of automation that can be used inside workflow steps.

**What is the difference between GitHub-hosted and self-hosted runners?**

GitHub-hosted runners are managed by GitHub. Self-hosted runners are machines configured and maintained by the repository owner or organization.

**What is the purpose of `needs`?**

The `needs` keyword defines dependencies between jobs and controls their execution order.

**What is a matrix build?**

A matrix build executes a job across multiple configuration combinations, such as different operating systems or runtime versions.

**What is the difference between artifacts and caches?**

Artifacts preserve and share generated workflow outputs. Caches reuse data, such as dependencies, to improve workflow execution speed.

**What is `GITHUB_TOKEN`?**

`GITHUB_TOKEN` is an automatically provided token that allows workflows to interact with GitHub resources according to its configured permissions.

**What is the purpose of GitHub Secrets?**

Secrets store sensitive configuration values such as API tokens and deployment credentials.

**What is the difference between reusable workflows and composite actions?**

Reusable workflows share complete workflows or jobs. Composite actions package multiple steps into a reusable action.

**What is `workflow_dispatch`?**

It is an event that allows a workflow to be started manually.

**What is a scheduled workflow?**

A scheduled workflow executes automatically according to a cron expression.

**What is the difference between Continuous Integration and Continuous Deployment?**

Continuous Integration automatically validates code changes through processes such as testing and building. Continuous Deployment automatically releases validated changes to the target environment.

**Why should workflow permissions be restricted?**

Restricting permissions follows the principle of least privilege and reduces the potential impact of compromised workflows or credentials.

# 🖆 RESOURCES & REFERENCES

> Explore the documentation, tools, guides, and references behind the topic.

**Official References**

* [GitHub Actions Documentation](https://docs.github.com/en/actions)
* [Understanding GitHub Actions](https://docs.github.com/en/actions/about-github-actions/understanding-github-actions)
* [Workflow Syntax](https://docs.github.com/en/actions/reference/workflows-and-actions/workflow-syntax)
* [Events That Trigger Workflows](https://docs.github.com/en/actions/reference/workflows-and-actions/events-that-trigger-workflows)
* [GitHub-Hosted Runners](https://docs.github.com/en/actions/using-github-hosted-runners)
* [Self-Hosted Runners](https://docs.github.com/en/actions/hosting-your-own-runners)
* [Using Secrets in GitHub Actions](https://docs.github.com/en/actions/how-tos/write-workflows/choose-what-workflows-do/use-secrets)
* [Workflow Artifacts](https://docs.github.com/en/actions/using-workflows/storing-workflow-data-as-artifacts)
* [Dependency Caching](https://docs.github.com/en/actions/using-workflows/caching-dependencies-to-speed-up-workflows)
* [Reusable Workflows](https://docs.github.com/en/actions/sharing-automations/reusing-workflows)
* [Creating Composite Actions](https://docs.github.com/en/actions/tutorials/create-actions/create-a-composite-action)
* [GitHub Actions Security](https://docs.github.com/en/actions/security-for-github-actions)
* [GitHub CLI Manual](https://cli.github.com/manual/gh_workflow)
