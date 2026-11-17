# Package Management

Package management is the process of installing, updating, removing, configuring, and maintaining external packages used by a JavaScript project.

In the Node.js ecosystem, npm is one of the most widely used package managers. It provides tools for managing dependencies, resolving version requirements, maintaining lockfiles, and sharing reusable packages.

# 🗁 What Is a Package?

A package is a collection of JavaScript code and related files distributed for reuse.

Packages can provide:

* Utility functions
* UI components
* Frameworks
* HTTP clients
* Database integrations
* Testing tools
* Build tools
* Development utilities

For example, a Node.js application might use:

```json
{
  "dependencies": {
    "express": "^5.1.0",
    "dotenv": "^16.4.5"
  }
}
```

Here, Express provides web application functionality, while dotenv loads environment variables from a file.

# 🗁 Package Managers

A package manager automates dependency management.

Common JavaScript package managers include:

| Package Manager | Description |
| --- | --- |
| npm | Default package manager distributed with Node.js |
| Yarn | Package manager with workspaces and dependency management features |
| pnpm | Package manager that uses a content-addressable store and efficient linking |
| Bun | JavaScript runtime that also includes a package manager |

Each package manager has its own commands, lockfile format, and dependency installation behavior.

# 🗁 Installing Dependencies

Install all dependencies declared in `package.json`:

```bash
npm install
```

npm reads the dependency declarations and installs the required packages into `node_modules`.

Install a specific package:

```bash
npm install express
```

Install a specific version:

```bash
npm install express@5.1.0
```

Install a package as a development dependency:

```bash
npm install --save-dev eslint
```

Install a package globally:

```bash
npm install --global typescript
```

Global packages are generally used for command-line tools that should be available across projects.

For application dependencies, prefer local installation.

# 🗁 Removing Dependencies

Remove a package from the project:

```bash
npm uninstall express
```

npm removes the package from `node_modules` and updates the relevant dependency declaration and lockfile.

Remove a globally installed package:

```bash
npm uninstall --global typescript
```

You can also remove packages manually from `package.json`, but using npm commands keeps the dependency files synchronized.

# 🗁 Updating Dependencies

Update installed packages within the version ranges specified in `package.json`:

```bash
npm update
```

Update a specific package:

```bash
npm update express
```

Check which packages have newer versions available:

```bash
npm outdated
```

Example output:

```text
Package   Current  Wanted  Latest
express   5.1.0    5.1.2   5.2.0
```

The columns mean:

* `Current` — Version currently installed.
* `Wanted` — Highest version satisfying the declared version range.
* `Latest` — Latest version published to the registry.

For example, if `package.json` contains:

```json
{
  "dependencies": {
    "express": "^5.1.0"
  }
}
```

npm can update Express to a compatible version within the allowed range, but it will not automatically cross into an incompatible major version.

To install a newer version and update the declaration:

```bash
npm install express@latest
```

Review major-version updates carefully because they may introduce breaking changes.

# 🗁 Dependency Versioning

JavaScript packages commonly use Semantic Versioning (SemVer).

The format is:

```text
MAJOR.MINOR.PATCH
```

Example:

```text
2.5.3
```

* `2` — Major version
* `5` — Minor version
* `3` — Patch version

Version range operators determine which package versions are acceptable.

| Range | Meaning |
| --- | --- |
| `1.2.3` | Exact version |
| `^1.2.3` | Compatible versions below `2.0.0` |
| `~1.2.3` | Compatible patch updates below `1.3.0` |
| `>=1.2.3` | Versions greater than or equal to `1.2.3` |
| `1.x` | Compatible versions in the `1.x` major line |
| `*` | Any version |

Important considerations:

* `^` generally allows minor and patch updates without changing the major version.
* `~` generally allows patch updates without changing the minor version.
* Exact versions provide tighter control over dependency selection.
* Pre-release versions have additional SemVer rules.

Version ranges express acceptable versions, not necessarily the version currently installed.

# 🗁 Direct and Transitive Dependencies

Dependencies can be classified by how they are introduced into a project.

**Direct dependencies**

Packages explicitly declared by your project.

Example:

```json
{
  "dependencies": {
    "express": "^5.1.0"
  }
}
```

Your project directly depends on Express.

**Transitive dependencies**

Packages required by other dependencies.

For example:

```text
my-project
└── express
    ├── accepts
    ├── body-parser
    └── cookie
```

Your application directly depends on Express, while Express depends on other packages.

Those additional packages are transitive dependencies.

Inspect the dependency tree:

```bash
npm ls
```

Inspect a specific package and its dependency relationships:

```bash
npm ls express
```

Display only top-level dependencies:

```bash
npm ls --depth=0
```

Transitive dependencies are important because they can affect project security, compatibility, and installation size.

# 🗁 node_modules

`node_modules` is the directory where npm installs project dependencies.

Example:

```text
my-project/
├── node_modules/
│   ├── express/
│   ├── dotenv/
│   └── ...
├── src/
├── package.json
└── package-lock.json
```

Important characteristics:

* Contains installed packages.
* Can become large because dependencies may have their own dependencies.
* Is generated and maintained by the package manager.
* Can be recreated from dependency declarations and the lockfile.
* Should generally not be committed to Git.

Add it to `.gitignore`:

```gitignore
node_modules/
```

To recreate it:

```bash
npm install
```

Or use a clean, lockfile-based installation:

```bash
npm ci
```

# 🗁 package-lock.json

`package-lock.json` records the exact dependency resolution produced by npm.

It helps ensure that different environments install consistent dependency versions.

Example:

```json
{
  "name": "my-project",
  "version": "1.0.0",
  "lockfileVersion": 3,
  "packages": {
    "": {
      "name": "my-project",
      "version": "1.0.0",
      "dependencies": {
        "express": "^5.1.0"
      }
    },
    "node_modules/express": {
      "version": "5.1.0"
    }
  }
}
```

The actual lockfile contains additional metadata, including resolved package information and integrity values.

Best practices:

* Commit `package-lock.json` for application projects.
* Do not manually edit it unless you understand its structure.
* Use `npm install` when changing dependencies.
* Use `npm ci` for reproducible clean installations.
* Keep the lockfile synchronized with `package.json`.

# 🗁 npm install vs npm ci

Both commands install project dependencies, but their behavior differs.

| Feature | `npm install` | `npm ci` |
| --- | --- | --- |
| Reads `package.json` | Yes | Yes |
| Uses lockfile | Yes, when available | Required |
| Can update lockfile | Yes | No |
| Removes existing `node_modules` first | No | Yes |
| Requires compatible lockfile | No | Yes |
| Typical use | Local development | CI and clean installations |

**npm install**

```bash
npm install
```

Useful when:

* Setting up a project.
* Adding dependencies.
* Updating dependency declarations.
* Updating the lockfile.

**npm ci**

```bash
npm ci
```

Useful when:

* Running automated builds.
* Installing dependencies in CI pipelines.
* Reproducing a known dependency tree.
* Performing clean installations.

If `package.json` and `package-lock.json` are incompatible, `npm ci` fails instead of modifying the lockfile.

# 🗁 Inspecting Dependencies

npm provides commands for inspecting installed packages.

**List installed dependencies**

```bash
npm ls
```

**List direct dependencies**

```bash
npm ls --depth=0
```

**Inspect a package**

```bash
npm view express
```

**Inspect a specific package version**

```bash
npm view express@5.1.0
```

**Show why a package is installed**

```bash
npm explain accepts
```

**Check dependency updates**

```bash
npm outdated
```

These commands help developers understand the dependency structure and identify outdated packages.

# 🗁 Cleaning Dependencies

Sometimes dependency installations become inconsistent or corrupted.

A clean installation can resolve many dependency-related problems.

Using npm:

```bash
npm ci
```

Alternatively, manually remove the installation directory and reinstall:

```bash
rm -rf node_modules
npm install
```

On Windows PowerShell:

```powershell
Remove-Item -Recurse -Force node_modules
npm install
```

If the lockfile itself needs to be regenerated, remove it only when you intentionally want npm to resolve dependencies again:

```bash
rm package-lock.json
npm install
```

Regenerating a lockfile can change many resolved dependency versions. Review the resulting changes before committing them.

# 🗁 npm Cache

npm maintains a cache to avoid downloading identical package data repeatedly.

Inspect the cache:

```bash
npm cache ls
```

Verify cache integrity:

```bash
npm cache verify
```

npm generally manages its cache automatically. Clearing it should not be a routine maintenance task.

If necessary:

```bash
npm cache clean --force
```

Use this only when there is a specific reason to clear the cache.

# 🗁 Dependency Security

Dependencies are part of the application's attack surface.

An application may inherit vulnerabilities through both direct and transitive dependencies.

**Audit dependencies**

```bash
npm audit
```

This checks installed dependencies against known vulnerability information.

**Attempt automatic fixes**

```bash
npm audit fix
```

npm attempts to update vulnerable dependencies within compatible version constraints.

**Review all available fixes**

```bash
npm audit fix --dry-run
```

This previews the changes npm would attempt.

**Force potentially breaking updates**

```bash
npm audit fix --force
```

This may install versions outside the declared dependency ranges, including major versions. It should not be used without reviewing the proposed changes.

Security best practices:

* Run dependency audits regularly.
* Keep dependencies updated.
* Remove unused packages.
* Review transitive dependencies.
* Evaluate packages before installing them.
* Investigate vulnerability reports rather than blindly applying fixes.
* Use maintained packages with reliable release histories.

# 🗁 Dependency Overrides

Sometimes a transitive dependency contains a bug or vulnerability, and an upstream package has not yet updated its dependency requirement.

npm provides the `overrides` field to control dependency versions.

Example:

```json
{
  "overrides": {
    "some-package": "2.5.0"
  }
}
```

This tells npm to use version `2.5.0` for that dependency where the override applies.

You can also target a dependency nested under another package:

```json
{
  "overrides": {
    "parent-package": {
      "some-package": "2.5.0"
    }
  }
}
```

Overrides should be used carefully because forcing a dependency version may introduce compatibility problems.

# 🗁 Optional Dependencies

Optional dependencies allow packages to be installed without making their installation failure fatal.

Example:

```json
{
  "optionalDependencies": {
    "some-platform-package": "^1.0.0"
  }
}
```

Applications should account for optional packages potentially being unavailable.

You can omit optional dependencies during installation:

```bash
npm install --omit=optional
```

# 🗁 Development vs Production Installation

npm supports installing dependencies according to their dependency classification.

Install all dependencies:

```bash
npm install
```

Install only production dependencies:

```bash
npm install --omit=dev
```

Development dependencies are excluded from the installed dependency tree.

This is useful for deployment environments where testing, linting, and build tools are not needed at runtime.

For example:

```json
{
  "dependencies": {
    "express": "^5.1.0"
  },
  "devDependencies": {
    "eslint": "^9.0.0",
    "prettier": "^3.3.0"
  }
}
```

A production installation includes Express but omits ESLint and Prettier.

# 🗁 Package Manager Version Management

Different package manager versions can produce different installation behavior and lockfile changes.

The `packageManager` field can declare the expected package manager and version.

Example:

```json
{
  "packageManager": "npm@10.8.2"
}
```

This communicates the intended package manager version to compatible tooling.

For teams, standardizing the Node.js and package manager versions helps reduce environment differences.

# 🗁 Practical Dependency Management Workflow

A typical workflow for a Node.js application:

**Initialize the project**

```bash
mkdir my-project
cd my-project
npm init -y
```

**Install runtime dependencies**

```bash
npm install express dotenv
```

**Install development dependencies**

```bash
npm install --save-dev eslint prettier
```

**Inspect dependencies**

```bash
npm ls --depth=0
```

**Check outdated packages**

```bash
npm outdated
```

**Audit security vulnerabilities**

```bash
npm audit
```

**Run project scripts**

```bash
npm run lint
npm test
```

**Perform a clean installation**

```bash
npm ci
```

**Prepare for production**

```bash
npm ci --omit=dev
```

# 🗁 Best Practices

* Use local dependencies for project-specific tools.
* Separate runtime and development dependencies.
* Commit lockfiles for application projects.
* Use `npm ci` in CI environments.
* Avoid committing `node_modules`.
* Review major dependency updates before installing them.
* Remove unused dependencies.
* Monitor security vulnerabilities.
* Avoid unnecessary global package installations.
* Use dependency overrides only when necessary.
* Keep Node.js and package manager versions consistent across environments.
* Avoid manually modifying lockfiles.
* Understand transitive dependencies rather than treating them as invisible implementation details.

# 🗁 Key Takeaways

* Package management handles dependency installation, updates, removal, and maintenance.
* npm is the default package manager distributed with Node.js.
* `package.json` declares project dependencies.
* `package-lock.json` records resolved dependency versions.
* `node_modules` stores installed packages.
* Direct dependencies are declared by the project; transitive dependencies are required by other packages.
* `npm install` is commonly used during development.
* `npm ci` provides clean, lockfile-based installations.
* `npm audit` helps identify known dependency vulnerabilities.
* Good dependency management improves project stability, reproducibility, and security.
