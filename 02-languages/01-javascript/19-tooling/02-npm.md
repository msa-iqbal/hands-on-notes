# npm (Node Package Manager)

**npm** is the default package manager distributed with Node.js.

It allows developers to install, manage, update, remove, and publish JavaScript packages.

npm consists of three main parts:

* **npm CLI:** Command-line tool used to manage packages and execute scripts.
* **npm Registry:** Online database containing published JavaScript packages.
* **npm Website:** Platform for discovering packages, managing accounts, and publishing software.

Official website: <https://www.npmjs.com/>

Documentation: <https://docs.npmjs.com/>

# 🗁 What Is a Package?

A package is a collection of JavaScript code and related resources distributed for reuse.

Packages can provide:

* Utility functions.
* Frameworks and libraries.
* Development tools.
* Build systems.
* Testing utilities.
* Command-line applications.
* Type definitions.
* Application components.

Examples of popular npm packages:

| Package | Purpose |
| --- | --- |
| `express` | Web application framework |
| `react` | User interface library |
| `lodash` | JavaScript utility functions |
| `axios` | HTTP client |
| `vite` | Frontend build tool |
| `typescript` | TypeScript compiler |
| `vitest` | Testing framework |
| `prettier` | Code formatter |
| `eslint` | JavaScript linter |

# 🗁 Checking npm Installation

npm is normally installed alongside Node.js.

Check the Node.js version:

```bash
node --version
```

Check the npm version:

```bash
npm --version
```

Display npm help:

```bash
npm help
```

Display available commands:

```bash
npm help
```

# 🗁 Initializing an npm Project

The `npm init` command creates a `package.json` file for a project.

Create a project directory:

```bash
mkdir my-project
cd my-project
```

Initialize npm:

```bash
npm init
```

npm asks several questions:

* Package name.
* Version.
* Description.
* Entry point.
* Test command.
* Git repository.
* Keywords.
* Author.
* License.

After completing the prompts, npm generates:

```json
{
  "name": "my-project",
  "version": "1.0.0",
  "description": "My JavaScript project",
  "main": "index.js",
  "scripts": {
    "test": "echo \"Error: no test specified\" && exit 1"
  },
  "license": "ISC"
}
```

For a faster initialization with default values:

```bash
npm init -y
```

# 🗁 Understanding package.json

`package.json` is the central configuration file for an npm project.

It stores project metadata, scripts, dependencies, and package configuration.

Example:

```json
{
  "name": "my-app",
  "version": "1.0.0",
  "description": "A JavaScript application",
  "type": "module",
  "main": "index.js",
  "scripts": {
    "start": "node index.js",
    "dev": "node --watch index.js",
    "test": "vitest run"
  },
  "dependencies": {
    "express": "^5.0.0"
  },
  "devDependencies": {
    "vitest": "^4.0.0"
  }
}
```

Important properties:

| Property | Purpose |
| --- | --- |
| `name` | Package name |
| `version` | Package version |
| `description` | Package description |
| `main` | Legacy package entry point |
| `type` | Determines how `.js` files are interpreted |
| `scripts` | Defines executable project commands |
| `dependencies` | Runtime dependencies |
| `devDependencies` | Development dependencies |
| `peerDependencies` | Dependencies expected from consuming projects |
| `optionalDependencies` | Dependencies whose installation failures may be tolerated |
| `engines` | Declares supported runtime versions |
| `license` | Package license |

# 🗁 Installing Packages

The `npm install` command installs packages from the npm registry or other supported package sources.

Install a package:

```bash
npm install express
```

The shorter form is:

```bash
npm i express
```

npm installs the package inside `node_modules` and records it in `dependencies` by default.

Install multiple packages:

```bash
npm install express axios lodash
```

Install a package globally:

```bash
npm install --global typescript
```

Global packages are generally used for command-line tools that should be available outside a particular project.

For most project dependencies, prefer local installation.

# 🗁 Installing Development Dependencies

Development dependencies are packages needed during development, testing, or building but not normally required by the application's runtime.

Install a development dependency:

```bash
npm install --save-dev vitest
```

Short form:

```bash
npm install -D vitest
```

Examples of development dependencies:

* ESLint.
* Prettier.
* Vitest.
* Jest.
* TypeScript.
* Vite.
* Build tools.

The package is recorded in `devDependencies`.

# 🗁 Installing Production Dependencies

Runtime dependencies are required by the application when it executes.

Example:

```bash
npm install express
```

This adds Express to `dependencies`.

In modern npm versions, `--save` is unnecessary because saving dependencies is the default behavior.

Install only production dependencies:

```bash
npm install --omit=dev
```

This is useful when preparing a production deployment.

# 🗁 Installing Specific Package Versions

npm allows developers to install particular package versions.

Install the latest version:

```bash
npm install lodash
```

Install an exact version:

```bash
npm install lodash@4.17.21
```

Install a major version:

```bash
npm install lodash@4
```

Install a specific version as a development dependency:

```bash
npm install -D vitest@4.0.0
```

Version selection is important when maintaining compatibility between project dependencies.

# 🗁 Installing Packages from Different Sources

npm supports package installation from more than just the public registry.

**From the npm registry**

```bash
npm install express
```

**From a Git repository**

```bash
npm install github:user/repository
```

**From a Git URL**

```bash
npm install git+https://github.com/user/repository.git
```

**From a local directory**

```bash
npm install ../shared-library
```

**From a package archive**

```bash
npm install ./packages/library.tgz
```

Git and local package installations are useful when developing internal libraries or testing unpublished packages.

# 🗁 Understanding node_modules

`node_modules` is the directory where npm installs project packages and their dependencies.

Example project structure:

```text
my-project/
├── node_modules/
│   ├── express/
│   ├── lodash/
│   └── vitest/
├── package.json
├── package-lock.json
└── index.js
```

Important characteristics:

* Contains installed packages.
* Can become large.
* May contain deeply nested dependencies.
* Is normally excluded from Git.
* Can be recreated from package metadata and the lockfile.

A typical `.gitignore` entry:

```gitignore
node_modules/
```

Do not manually edit files inside `node_modules`. Manage dependencies through npm instead.

# 🗁 Understanding package-lock.json

`package-lock.json` records the exact dependency tree resolved during installation.

It helps ensure that developers and deployment environments install consistent dependency versions.

Example:

```json
{
  "name": "my-project",
  "lockfileVersion": 3,
  "packages": {
    "": {
      "name": "my-project",
      "version": "1.0.0",
      "dependencies": {
        "lodash": "^4.17.21"
      }
    },
    "node_modules/lodash": {
      "version": "4.17.21"
    }
  }
}
```

Important points:

* Records resolved package versions.
* Includes dependency metadata.
* Helps reproduce installations.
* Supports consistent CI and deployment workflows.
* Should normally be committed to Git for applications.

The lockfile is especially important when multiple developers work on the same project.

# 🗁 Installing Dependencies from Existing Configuration

If a project already contains `package.json`, npm can install its declared dependencies.

```bash
npm install
```

npm resolves the dependencies and creates or updates `node_modules` and the lockfile.

For reproducible installations in automated environments, use:

```bash
npm ci
```

`npm ci`:

* Requires a compatible lockfile.
* Installs the exact dependency tree represented by the lockfile.
* Removes the existing `node_modules` directory before installation.
* Does not modify `package.json` or the lockfile.
* Fails when the lockfile and package manifest are inconsistent.

Use `npm ci` in CI pipelines and clean deployment environments.

# 🗁 Updating Packages

npm provides commands for checking and updating installed dependencies.

Check outdated packages:

```bash
npm outdated
```

Example output:

```text
Package   Current   Wanted   Latest
express   4.21.0    4.21.2   5.0.0
lodash    4.17.20   4.17.21  4.17.21
```

Update dependencies within the permitted version ranges in `package.json`:

```bash
npm update
```

Update a specific package:

```bash
npm update express
```

Install a particular newer version:

```bash
npm install express@latest
```

The `npm update` command generally respects the version ranges declared in `package.json`. Installing `@latest` explicitly can move a dependency beyond its existing range.

Review breaking changes before upgrading major versions.

# 🗁 Removing Packages

Remove an installed package:

```bash
npm uninstall express
```

Short form:

```bash
npm remove express
```

Remove a development dependency:

```bash
npm uninstall vitest
```

npm removes the package from the appropriate dependency declaration and updates the lockfile.

Remove a globally installed package:

```bash
npm uninstall --global typescript
```

# 🗁 Listing Installed Packages

List top-level dependencies:

```bash
npm list --depth=0
```

List all installed dependencies:

```bash
npm list
```

List globally installed packages:

```bash
npm list --global --depth=0
```

Check whether a particular package is installed:

```bash
npm list express
```

# 🗁 Running npm Scripts

npm allows developers to define reusable commands in `package.json`.

Example:

```json
{
  "scripts": {
    "start": "node index.js",
    "dev": "node --watch index.js",
    "test": "vitest run",
    "build": "vite build"
  }
}
```

Run a script:

```bash
npm run dev
```

For scripts named `start`, `test`, `stop`, or `restart`, npm also provides dedicated command forms where applicable.

Examples:

```bash
npm start
npm test
```

Run a package binary directly through npm:

```bash
npm exec -- eslint .
```

The `npx` command is also commonly used to execute package binaries.

# 🗁 Understanding npx

`npx` is an npm-provided command for executing package binaries.

It is useful when running tools without manually specifying their installation paths.

Example:

```bash
npx vite
```

Run a specific package version:

```bash
npx vite@latest
```

Run a locally installed package binary:

```bash
npx eslint .
```

Modern npm also provides `npm exec`:

```bash
npm exec -- vite
```

For project tooling, installing the package locally and executing its binary is generally more predictable than relying on an arbitrary package fetched during execution.

# 🗁 npm Cache

npm maintains a cache to reduce repeated downloads and improve installation performance.

Check cache information:

```bash
npm cache ls
```

Verify cache integrity:

```bash
npm cache verify
```

Clean the cache:

```bash
npm cache clean --force
```

Cache cleaning is rarely necessary because npm manages its cache automatically.

Prefer `npm cache verify` when troubleshooting potential cache problems.

# 🗁 npm Configuration

npm supports configuration through command-line options, environment variables, configuration files, and defaults.

View npm configuration:

```bash
npm config list
```

View a specific setting:

```bash
npm config get registry
```

Change the registry:

```bash
npm config set registry https://registry.npmjs.org/
```

Remove a configuration value:

```bash
npm config delete registry
```

Configuration files can exist at different levels:

* Project-level `.npmrc`.
* User-level `.npmrc`.
* Global configuration.
* Built-in npm configuration.

Example `.npmrc`:

```ini
registry=https://registry.npmjs.org/
save-exact=true
```

Do not commit authentication tokens or private credentials in project configuration files.

# 🗁 npm Registry

The npm registry is an online package repository where JavaScript packages are published and distributed.

Developers can search for packages:

<https://www.npmjs.com/>

Install a package by name:

```bash
npm install express
```

npm retrieves package metadata and downloads the package from the configured registry.

Developers can also configure private registries for internal organizational packages.

# 🗁 Publishing npm Packages

npm allows developers to publish reusable packages to the registry.

A typical publishing workflow:

**Create a package**

```bash
mkdir my-library
cd my-library
npm init -y
```

**Prepare the package**

```javascript
// index.js

export function greet(name) {
  return `Hello, ${name}!`;
}
```

**Configure package metadata**

```json
{
  "name": "my-example-library",
  "version": "1.0.0",
  "type": "module",
  "main": "./index.js",
  "exports": {
    ".": "./index.js"
  }
}
```

**Authenticate**

```bash
npm login
```

**Publish**

```bash
npm publish
```

Package names must be available and valid for the registry.

For scoped packages, public access may need to be specified:

```bash
npm publish --access public
```

Publishing requires an appropriate npm account and permissions.

Before publishing, inspect the package contents:

```bash
npm pack --dry-run
```

This helps identify files that should not be included.

# 🗁 npm Semantic Versioning

npm packages commonly follow Semantic Versioning (SemVer).

Version format:

```text
MAJOR.MINOR.PATCH
```

Example:

```text
4.2.1
```

| Part | Meaning |
| --- | --- |
| MAJOR | Incompatible API changes |
| MINOR | Backward-compatible functionality |
| PATCH | Backward-compatible bug fixes |

Examples:

* `4.2.1` → `4.2.2`: Patch update.
* `4.2.1` → `4.3.0`: Minor update.
* `4.2.1` → `5.0.0`: Major update.

npm uses version ranges to determine which package versions can satisfy a dependency declaration.

# 🗁 npm Security Auditing

Dependencies can contain known security vulnerabilities.

npm provides an auditing command:

```bash
npm audit
```

Attempt to fix compatible vulnerabilities automatically:

```bash
npm audit fix
```

Force potentially breaking updates where applicable:

```bash
npm audit fix --force
```

Use the force option carefully because it may introduce incompatible dependency versions.

For security-sensitive applications:

* Review audit reports.
* Update vulnerable dependencies.
* Inspect transitive dependencies.
* Prefer maintained packages.
* Avoid unnecessary dependencies.
* Review package provenance and publisher information.

# 🗁 Useful npm Commands

| Command | Purpose |
| --- | --- |
| `npm init` | Creates a package configuration |
| `npm init -y` | Initializes with default values |
| `npm install` | Installs dependencies |
| `npm install package` | Installs a package |
| `npm install -D package` | Installs a development dependency |
| `npm uninstall package` | Removes a package |
| `npm update` | Updates dependencies within declared ranges |
| `npm outdated` | Lists outdated dependencies |
| `npm list --depth=0` | Lists direct dependencies |
| `npm ci` | Performs a clean lockfile-based installation |
| `npm run script` | Executes a custom script |
| `npm test` | Runs the test script |
| `npm start` | Runs the start script |
| `npm cache verify` | Verifies npm cache integrity |
| `npm config list` | Displays configuration |
| `npm audit` | Checks dependencies for known vulnerabilities |
| `npm publish` | Publishes a package |
| `npm pack` | Creates a package archive |

# 🗁 npm Best Practices

* Use a supported Node.js and npm version.
* Keep `package.json` and `package-lock.json` committed for applications.
* Use `npm ci` in CI environments.
* Install development tools as development dependencies.
* Avoid unnecessary global package installations.
* Review dependency updates before applying them.
* Audit dependencies regularly.
* Avoid manually modifying `node_modules`.
* Use semantic versioning for published packages.
* Never expose authentication tokens in source control.
* Inspect package contents before publishing.
* Prefer maintained and trustworthy dependencies.

# 🗁 Summary

npm is the package management ecosystem commonly used with Node.js.

Key concepts:

* npm CLI manages JavaScript packages.
* The npm registry distributes packages.
* `package.json` stores project metadata and dependency declarations.
* `package-lock.json` records the resolved dependency tree.
* `node_modules` contains installed packages.
* `npm install` installs dependencies.
* `npm ci` provides clean, lockfile-based installations.
* npm scripts automate development tasks.
* npm supports package publishing and semantic versioning.
* `npm audit` helps identify known dependency vulnerabilities.
