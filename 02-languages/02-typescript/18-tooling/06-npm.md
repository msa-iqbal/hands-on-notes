# npm

npm (Node Package Manager) is the default package manager distributed with Node.js.

It allows developers to install, manage, update, and publish JavaScript and TypeScript packages.

npm also provides tools for managing project dependencies, executing scripts, and maintaining reproducible development environments.

# 🗁 What Is npm?

npm consists of three important parts:

* **npm CLI:** The command-line tool used to manage packages and projects.
* **npm Registry:** An online repository containing publicly available JavaScript packages.
* **npm Website:** The platform for discovering packages, viewing documentation, and managing published packages.

A typical npm workflow:

```text
Developer
    |
    v
 npm CLI
    |
    v
npm Registry
    |
    v
Download Packages
    |
    v
node_modules/
    |
    v
Project Development
```

# 🗁 Installing Node.js and npm

npm is installed automatically when you install Node.js using the official Node.js distribution.

Verify Node.js:

```bash
node --version
```

Verify npm:

```bash
npm --version
```

Display the Node.js installation path:

```bash
which node
```

On Windows:

```powershell
where.exe node
```

# 🗁 Creating a New npm Project

Create a project directory:

```bash
mkdir my-project
cd my-project
```

Initialize npm:

```bash
npm init
```

The command starts an interactive process that asks for project information.

Example:

```text
package name: (my-project)
version: (1.0.0)
description: My Node.js application
entry point: (index.js)
```

After initialization, npm creates:

**package.json**

```json
{
  "name": "my-project",
  "version": "1.0.0",
  "description": "My Node.js application",
  "main": "index.js",
  "scripts": {
    "test": "echo \"Error: no test specified\" && exit 1"
  }
}
```

You can also initialize a project without interactive questions:

```bash
npm init -y
```

This generates a `package.json` file using default values.

# 🗁 Understanding package.json

`package.json` is the central configuration file of an npm project.

It contains project metadata, dependencies, scripts, and other package configuration.

Example:

```json
{
  "name": "my-app",
  "version": "1.0.0",
  "description": "A TypeScript application",
  "main": "dist/index.js",
  "scripts": {
    "dev": "tsx watch src/index.ts",
    "build": "tsc",
    "start": "node dist/index.js",
    "test": "vitest"
  },
  "dependencies": {
    "express": "^5.0.0"
  },
  "devDependencies": {
    "typescript": "^5.0.0",
    "tsx": "^4.0.0",
    "vitest": "^3.0.0"
  }
}
```

Important properties:

| Property | Purpose |
| --- | --- |
| `name` | Project or package name |
| `version` | Current package version |
| `description` | Project description |
| `main` | Traditional package entry point |
| `type` | Determines Node.js module interpretation for `.js` files |
| `scripts` | Defines reusable npm commands |
| `dependencies` | Packages required at runtime |
| `devDependencies` | Packages used during development |
| `peerDependencies` | Packages expected from the consuming project |
| `optionalDependencies` | Dependencies that may be installed optionally |
| `engines` | Declares supported runtime versions |

# 🗁 Installing Packages

Install a package:

```bash
npm install express
```

Short form:

```bash
npm i express
```

npm installs the package into `node_modules/` and records it in `dependencies`.

Install multiple packages:

```bash
npm install express cors dotenv
```

Install a specific version:

```bash
npm install express@5.1.0
```

Install the latest version allowed by the package registry:

```bash
npm install express@latest
```

# 🗁 Development Dependencies

Development dependencies are packages needed during development, testing, linting, or building but not normally required as direct runtime dependencies.

Install a development dependency:

```bash
npm install --save-dev typescript
```

Short form:

```bash
npm i -D typescript
```

Install multiple development dependencies:

```bash
npm install -D eslint prettier vitest
```

Example:

```json
{
  "dependencies": {
    "express": "^5.0.0"
  },
  "devDependencies": {
    "typescript": "^5.0.0",
    "eslint": "^9.0.0",
    "prettier": "^3.0.0"
  }
}
```

# 🗁 Installing Global Packages

Global packages are installed at the system or user environment level rather than inside a specific project.

Install globally:

```bash
npm install --global typescript
```

Short form:

```bash
npm i -g typescript
```

Check globally installed packages:

```bash
npm list -g --depth=0
```

Remove a global package:

```bash
npm uninstall -g typescript
```

Global installation is useful for command-line tools that you want to access across multiple projects.

For project-specific tooling, local development dependencies are generally preferable.

# 🗁 Removing Packages

Remove a dependency:

```bash
npm uninstall express
```

Remove a development dependency:

```bash
npm uninstall -D typescript
```

Remove a global package:

```bash
npm uninstall -g typescript
```

npm updates the relevant dependency declarations in `package.json` and the lockfile.

# 🗁 Updating Packages

Check outdated dependencies:

```bash
npm outdated
```

Update dependencies within the version ranges declared in `package.json`:

```bash
npm update
```

Update a specific package:

```bash
npm update express
```

Install a specific newer version:

```bash
npm install express@latest
```

The difference is important:

* `npm update` generally respects the existing version ranges.
* `npm install package@latest` explicitly requests the latest published version and updates the dependency declaration accordingly.

# 🗁 Understanding Semantic Versioning

npm packages commonly follow Semantic Versioning (SemVer).

Version format:

```text
MAJOR.MINOR.PATCH
```

Example:

```text
5.2.3
```

| Component | Meaning |
| --- | --- |
| MAJOR | Breaking changes |
| MINOR | Backward-compatible functionality |
| PATCH | Backward-compatible bug fixes |

Common version range operators:

| Operator | Meaning | Example |
| --- | --- | --- |
| `^` | Allows compatible updates without changing the major version | `^5.2.3` |
| `~` | Allows patch updates within the specified minor version | `~5.2.3` |
| `>=` | Greater than or equal to a version | `>=5.2.3` |
| `>` | Greater than a version | `>5.2.3` |
| `<=` | Less than or equal to a version | `<=5.2.3` |
| `*` | Any version | `*` |

For example:

```json
{
  "dependencies": {
    "express": "^5.2.3",
    "axios": "~1.7.9"
  }
}
```

The exact installed versions are recorded in `package-lock.json`.

# 🗁 Understanding package-lock.json

`package-lock.json` is an automatically generated lockfile that records the exact dependency tree installed by npm.

It contains information such as:

* Exact package versions.
* Dependency relationships.
* Resolved package locations.
* Integrity hashes.
* Dependency metadata.

Example:

```json
{
  "name": "my-project",
  "lockfileVersion": 3,
  "packages": {
    "": {
      "name": "my-project",
      "dependencies": {
        "express": "^5.0.0"
      }
    },
    "node_modules/express": {
      "version": "5.1.0"
    }
  }
}
```

The example is simplified; actual lockfiles contain additional metadata.

**Why is package-lock.json important?**

Imagine two developers install the same project.

Without a lockfile, compatible version ranges may resolve to different package versions at different times.

With a lockfile, npm can reproduce the recorded dependency tree.

Best practices:

* Commit `package-lock.json` to version control.
* Do not manually edit the lockfile.
* Use `npm ci` in automated environments.
* Keep the lockfile synchronized with `package.json`.

# 🗁 Installing Dependencies from Existing Projects

When joining an existing project, dependencies are usually already declared in `package.json`.

Install them:

```bash
npm install
```

npm reads the project manifest and lockfile and installs the required packages.

If a compatible lockfile exists, npm uses it to guide dependency resolution.

# 🗁 npm ci

`npm ci` is designed for clean, reproducible dependency installation.

Run:

```bash
npm ci
```

Important characteristics:

* Requires an existing `package-lock.json` or compatible npm lockfile.
* Removes the existing `node_modules/` directory.
* Installs the dependency tree represented by the lockfile.
* Does not update the lockfile.
* Fails if the lockfile and `package.json` are inconsistent.

**npm install vs npm ci**

| Feature | npm install | npm ci |
| --- | --- | --- |
| Installs dependencies | Yes | Yes |
| Uses lockfile | Yes | Yes |
| Updates lockfile when needed | Yes | No |
| Removes existing node_modules | Not normally | Yes |
| Requires lockfile | No | Yes |
| Suitable for CI/CD | Yes | Preferred |

# 🗁 npm Scripts

npm scripts allow developers to define reusable commands inside `package.json`.

Example:

```json
{
  "scripts": {
    "dev": "tsx watch src/index.ts",
    "build": "tsc",
    "start": "node dist/index.js",
    "lint": "eslint .",
    "format": "prettier --write .",
    "typecheck": "tsc --noEmit"
  }
}
```

Execute a script:

```bash
npm run dev
```

Run other scripts:

```bash
npm run build
npm run lint
npm run format
npm run typecheck
```

Some scripts have dedicated npm commands:

```bash
npm start
npm test
```

You can list available scripts by inspecting `package.json` or running:

```bash
npm run
```

# 🗁 Executing Package Binaries with npx

`npx` allows developers to execute package binaries without manually specifying their installation paths.

Example:

```bash
npx tsc --version
```

Run ESLint:

```bash
npx eslint .
```

Run Prettier:

```bash
npx prettier --write .
```

For packages already installed locally, `npx` can execute their binaries from the project's dependencies.

Modern npm also provides the `npm exec` command:

```bash
npm exec -- eslint .
```

Use locally installed tools when possible to maintain consistent versions across projects.

# 🗁 Listing Installed Packages

List top-level dependencies:

```bash
npm list --depth=0
```

List all installed dependencies:

```bash
npm list
```

List production dependencies:

```bash
npm list --omit=dev
```

List globally installed packages:

```bash
npm list -g --depth=0
```

Check whether a package is installed:

```bash
npm list express
```

# 🗁 Auditing Dependencies

npm provides built-in commands for identifying known security vulnerabilities in dependencies.

Run:

```bash
npm audit
```

Attempt automatic fixes where compatible updates are available:

```bash
npm audit fix
```

Display detailed vulnerability information:

```bash
npm audit --json
```

Be careful with forced updates:

```bash
npm audit fix --force
```

The `--force` option can introduce breaking dependency updates.

Review the proposed changes and test the application before accepting them.

# 🗁 npm Cache

npm maintains a local cache to reduce repeated downloads and improve installation performance.

Verify the cache:

```bash
npm cache verify
```

Display the cache location:

```bash
npm config get cache
```

npm manages cache integrity automatically. Clearing the cache is rarely necessary.

# 🗁 npm Configuration

npm supports configuration through command-line options, environment variables, project-level configuration, and user-level configuration.

Display npm configuration:

```bash
npm config list
```

Get a specific configuration value:

```bash
npm config get registry
```

Set the registry:

```bash
npm config set registry https://registry.npmjs.org/
```

Remove a configuration value:

```bash
npm config delete registry
```

npm configuration can also be stored in an `.npmrc` file.

Example:

**.npmrc**

```text
save-exact=true
fund=false
```

This configuration saves exact dependency versions when installing packages and disables funding messages.

Avoid committing authentication tokens or other secrets in project-level configuration files.

# 🗁 npm Workspaces

npm Workspaces allow multiple related packages to be managed inside a single repository.

They are useful for monorepos.

Example directory structure:

```text
my-monorepo/
├── package.json
├── package-lock.json
├── node_modules/
└── packages/
    ├── api/
    │   └── package.json
    ├── web/
    │   └── package.json
    └── shared/
        └── package.json
```

Root `package.json`:

```json
{
  "name": "my-monorepo",
  "private": true,
  "workspaces": [
    "packages/*"
  ]
}
```

Install dependencies for all workspaces:

```bash
npm install
```

Run a script in a specific workspace:

```bash
npm run build --workspace=api
```

Install a package into a specific workspace:

```bash
npm install express --workspace=api
```

npm Workspaces simplify dependency management and script execution across related packages.

# 🗁 Publishing Packages to npm

npm allows developers to publish reusable packages to the npm Registry.

First, create an npm account and authenticate:

```bash
npm login
```

Check the current authenticated user:

```bash
npm whoami
```

Prepare a package:

**package.json**

```json
{
  "name": "my-example-package",
  "version": "1.0.0",
  "description": "An example npm package",
  "main": "index.js"
}
```

Publish:

```bash
npm publish
```

For scoped packages intended to be publicly available:

```bash
npm publish --access public
```

Publishing a package requires a valid package name and version. A version already published under the same package name cannot simply be overwritten.

# 🗁 Useful npm Commands

| Command | Purpose |
| --- | --- |
| `npm init` | Initialize a project |
| `npm init -y` | Initialize with default values |
| `npm install` | Install dependencies |
| `npm install package` | Install a package |
| `npm install -D package` | Install a development dependency |
| `npm uninstall package` | Remove a package |
| `npm update` | Update dependencies within allowed ranges |
| `npm outdated` | Display outdated dependencies |
| `npm list` | List installed packages |
| `npm ci` | Perform a clean lockfile-based installation |
| `npm run` | List available scripts |
| `npm audit` | Check dependency vulnerabilities |
| `npm cache verify` | Verify the npm cache |
| `npm config list` | Display configuration |
| `npm login` | Authenticate with npm |
| `npm publish` | Publish a package |

# 🗁 Best Practices

* Use `package.json` to manage project metadata and dependencies.
* Commit `package-lock.json` to version control.
* Prefer local development dependencies for project tooling.
* Use `npm ci` in CI/CD environments.
* Avoid installing unnecessary global packages.
* Review dependency updates before upgrading major versions.
* Run `npm audit` regularly.
* Avoid exposing authentication tokens in source control.
* Use npm Workspaces when managing related packages in a monorepo.
* Keep Node.js and npm versions consistent across development environments.
* Use semantic versioning when publishing reusable packages.

# 🗁 Summary

npm is the package manager used to manage JavaScript and TypeScript project dependencies.

Its main capabilities include:

* Initializing and managing projects.
* Installing and removing packages.
* Managing dependency versions.
* Running project scripts.
* Maintaining reproducible installations with lockfiles.
* Auditing dependency vulnerabilities.
* Managing monorepos with Workspaces.
* Publishing reusable packages to the npm Registry.

Understanding npm is essential for building, maintaining, and distributing modern JavaScript and TypeScript applications.
