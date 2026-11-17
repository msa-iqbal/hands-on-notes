# package.json

The `package.json` file is the central configuration file of a Node.js project. It stores project metadata, dependencies, scripts, package requirements, and other settings used by npm and the JavaScript tooling ecosystem.

It helps developers manage a project consistently across development, testing, and production environments.

# 🗁 What Is package.json?

`package.json` is a JSON file located in the root directory of a Node.js project.

A typical project structure looks like this:

```text
my-project/
├── node_modules/
├── src/
│   └── index.js
├── package.json
└── package-lock.json
```

A basic `package.json`:

```json
{
  "name": "my-project",
  "version": "1.0.0",
  "description": "A sample Node.js project",
  "main": "src/index.js",
  "scripts": {
    "start": "node src/index.js"
  }
}
```

# 🗁 Creating package.json

You can create a `package.json` file using npm.

```bash
npm init
```

npm starts an interactive process that asks for project information, including:

* Package name
* Version
* Description
* Entry point
* Test command
* Git repository
* Keywords
* Author
* License

To generate a file with default values without answering each question:

```bash
npm init -y
```

You can also create the file manually using a text editor.

# 🗁 Package Metadata

Metadata describes the package and identifies it within the npm ecosystem.

Common metadata fields include:

**name**

Defines the package name.

```json
{
  "name": "my-project"
}
```

Package names must follow npm's naming rules. They are commonly lowercase and may contain hyphens.

**version**

Defines the package version.

```json
{
  "version": "1.0.0"
}
```

npm packages commonly follow Semantic Versioning:

```text
MAJOR.MINOR.PATCH
```

For example:

* `1.0.0` — Initial stable release
* `1.1.0` — Backward-compatible feature addition
* `1.1.1` — Backward-compatible bug fix
* `2.0.0` — Potentially breaking changes

**description**

Provides a short explanation of the package.

```json
{
  "description": "A REST API built with Node.js"
}
```

**keywords**

Defines searchable keywords for npm.

```json
{
  "keywords": [
    "nodejs",
    "api",
    "backend"
  ]
}
```

**author**

Identifies the package author.

```json
{
  "author": "Developer Name"
}
```

**license**

Specifies the package license.

```json
{
  "license": "MIT"
}
```

**homepage**

Defines the package's homepage URL.

```json
{
  "homepage": "https://example.com"
}
```

**repository**

Identifies the source code repository.

```json
{
  "repository": {
    "type": "git",
    "url": "https://github.com/example/my-project.git"
  }
}
```

# 🗁 Entry Point Fields

Entry point fields describe how a package exposes its code.

**main**

Specifies the traditional package entry point.

```json
{
  "main": "./index.js"
}
```

It is commonly used by CommonJS consumers and tools that rely on the legacy package entry point.

**module**

A convention used by some bundlers to identify an ES module entry point.

```json
{
  "module": "./dist/index.esm.js"
}
```

It is not a universally standardized Node.js package field.

**browser**

A convention used by certain bundlers to provide a browser-specific entry point or replacements.

```json
{
  "browser": "./dist/index.browser.js"
}
```

**type**

Controls how Node.js interprets `.js` files in the package scope.

```json
{
  "type": "module"
}
```

Common values:

* `"module"` — Treat `.js` files as ES modules.
* `"commonjs"` — Treat `.js` files as CommonJS modules.

Without an explicit `"type"` field, Node.js generally treats `.js` files as CommonJS, subject to its module detection rules and configuration.

File extensions can also determine module format:

* `.mjs` — Always interpreted as an ES module.
* `.cjs` — Always interpreted as a CommonJS module.

Example:

```json
{
  "name": "my-project",
  "version": "1.0.0",
  "type": "module"
}
```

Now `.js` files in this package scope use ES module syntax:

```javascript
import fs from "node:fs";

console.log("ES modules are enabled.");
```

# 🗁 Dependencies

Dependencies are external packages required by an application.

npm records installed dependencies in `package.json`.

**dependencies**

Packages required by the application at runtime.

```json
{
  "dependencies": {
    "express": "^5.1.0",
    "dotenv": "^16.4.5"
  }
}
```

Install a runtime dependency:

```bash
npm install express
```

npm adds it to the `dependencies` section.

**devDependencies**

Packages required during development, testing, or building but generally not required at runtime.

```json
{
  "devDependencies": {
    "eslint": "^9.0.0",
    "prettier": "^3.3.0"
  }
}
```

Install a development dependency:

```bash
npm install --save-dev eslint
```

Or use the shorthand:

```bash
npm i -D eslint
```

**optionalDependencies**

Packages that are optional for the application.

```json
{
  "optionalDependencies": {
    "some-optional-package": "^1.0.0"
  }
}
```

If an optional dependency fails to install, npm can continue installing the remaining packages.

Applications should handle the possibility that an optional package is unavailable.

**peerDependencies**

Packages that declare compatibility requirements with a host package or environment.

They are commonly used by libraries and plugins that expect the consuming project to provide a compatible dependency.

```json
{
  "peerDependencies": {
    "react": "^18.0.0 || ^19.0.0"
  }
}
```

A React component library might use this field because the consuming application should provide React.

**peerDependenciesMeta**

Provides additional metadata for peer dependencies, including whether a peer dependency is optional.

```json
{
  "peerDependencies": {
    "some-plugin": "^2.0.0"
  },
  "peerDependenciesMeta": {
    "some-plugin": {
      "optional": true
    }
  }
}
```

**bundledDependencies**

Specifies dependencies that should be included inside the package when it is packed.

```json
{
  "bundledDependencies": [
    "some-package"
  ]
}
```

This is useful when distributing a package with selected dependencies bundled into its archive.

# 🗁 Dependency Version Ranges

Dependency versions can use different range operators.

```json
{
  "dependencies": {
    "package-a": "1.2.3",
    "package-b": "^1.2.3",
    "package-c": "~1.2.3",
    "package-d": ">=1.2.0 <2.0.0"
  }
}
```

Common version range operators:

| Operator | Meaning |
| --- | --- |
| `1.2.3` | Exact version |
| `^1.2.3` | Compatible version range that does not change the major version |
| `~1.2.3` | Compatible patch-level updates |
| `>=1.2.3` | Version greater than or equal to the specified version |
| `<2.0.0` | Version lower than the specified version |
| `*` | Any version |

For example:

```json
{
  "dependencies": {
    "express": "^5.1.0"
  }
}
```

The caret allows compatible updates within the same major version, according to Semantic Versioning.

The actual installed version is recorded in `package-lock.json` when a lockfile is used.

# 🗁 Scripts

The `scripts` field defines reusable commands for project tasks.

```json
{
  "scripts": {
    "start": "node src/index.js",
    "dev": "node --watch src/index.js",
    "test": "node --test"
  }
}
```

Run a script using npm:

```bash
npm run dev
```

Some standard script names have dedicated npm commands:

```bash
npm start
npm test
npm stop
npm restart
```

For other scripts, use:

```bash
npm run script-name
```

Scripts can execute local binaries installed inside `node_modules/.bin` without requiring their full paths.

For example:

```json
{
  "scripts": {
    "lint": "eslint .",
    "format": "prettier --write ."
  }
}
```

# 🗁 Configuration Fields

`package.json` can also contain configuration fields used by npm and other development tools.

**engines**

Specifies compatible Node.js and npm versions.

```json
{
  "engines": {
    "node": ">=20.0.0",
    "npm": ">=10.0.0"
  }
}
```

This communicates the expected runtime environment. Enforcement depends on the package manager and its configuration.

**private**

Prevents accidental publication to the npm registry.

```json
{
  "private": true
}
```

This is especially useful for application projects that should not be published as npm packages.

**files**

Controls which files are included when the package is packed or published.

```json
{
  "files": [
    "dist/",
    "README.md",
    "LICENSE"
  ]
}
```

It helps avoid publishing unnecessary files.

**publishConfig**

Defines configuration specifically for package publishing.

```json
{
  "publishConfig": {
    "access": "public",
    "registry": "https://registry.npmjs.org/"
  }
}
```

For scoped packages, `"access": "public"` makes the package publicly accessible when published.

**workspaces**

Defines multiple packages managed within a single repository.

```json
{
  "name": "my-monorepo",
  "private": true,
  "workspaces": [
    "packages/*",
    "apps/*"
  ]
}
```

Example structure:

```text
my-monorepo/
├── apps/
│   ├── frontend/
│   │   └── package.json
│   └── backend/
│       └── package.json
├── packages/
│   └── shared/
│       └── package.json
└── package.json
```

npm workspaces allow dependencies and scripts to be managed across multiple packages.

# 🗁 package.json vs package-lock.json

Both files are important, but they serve different purposes.

| package.json | package-lock.json |
| --- | --- |
| Defines project metadata | Records dependency resolution details |
| Declares dependency version ranges | Records resolved package versions |
| Defines scripts and configuration | Records dependency tree information |
| Can be manually edited | Usually generated and maintained by npm |
| Describes project requirements | Helps reproduce installations consistently |

Example:

`package.json`:

```json
{
  "dependencies": {
    "express": "^5.1.0"
  }
}
```

`package-lock.json` records the exact resolved version and other installation metadata.

Install dependencies using the lockfile:

```bash
npm ci
```

Unlike `npm install`, `npm ci` requires a compatible lockfile, removes the existing `node_modules` directory, and installs the locked dependency tree.

For application repositories, commit both `package.json` and `package-lock.json` to version control.

# 🗁 Complete Example

A practical `package.json` for a Node.js application:

```json
{
  "name": "node-api",
  "version": "1.0.0",
  "description": "A REST API built with Node.js",
  "type": "module",
  "main": "src/index.js",
  "scripts": {
    "start": "node src/index.js",
    "dev": "node --watch src/index.js",
    "test": "node --test",
    "lint": "eslint .",
    "format": "prettier --write ."
  },
  "dependencies": {
    "express": "^5.1.0",
    "dotenv": "^16.4.5"
  },
  "devDependencies": {
    "eslint": "^9.0.0",
    "prettier": "^3.3.0"
  },
  "engines": {
    "node": ">=20.0.0"
  },
  "private": true,
  "license": "MIT"
}
```

# 🗁 Useful npm Commands

| Command | Purpose |
| --- | --- |
| `npm init` | Create package.json interactively |
| `npm init -y` | Create package.json with defaults |
| `npm install` | Install dependencies |
| `npm install package-name` | Install a runtime dependency |
| `npm install -D package-name` | Install a development dependency |
| `npm uninstall package-name` | Remove a dependency |
| `npm update` | Update dependencies within allowed ranges |
| `npm ci` | Perform a clean lockfile-based installation |
| `npm run` | List available scripts |
| `npm pkg get name` | Read a package.json property |
| `npm pkg set name="my-project"` | Set a package.json property |
| `npm pkg delete description` | Delete a package.json property |
| `npm pack` | Create a package archive |
| `npm publish` | Publish a package to a registry |

# 🗁 Best Practices

* Keep `package.json` valid JSON.
* Use meaningful package names and descriptions.
* Follow Semantic Versioning for package releases.
* Separate runtime dependencies from development dependencies.
* Specify compatible Node.js versions using `engines`.
* Use `"private": true` for applications that should not be published.
* Commit `package-lock.json` for reproducible application installations.
* Use `npm ci` in automated CI environments when a lockfile is available.
* Avoid unnecessarily broad dependency version ranges.
* Review dependencies before adding them to a project.
* Define reusable project commands in the `scripts` field.
* Configure `files` or `.npmignore` to control published package contents.
* Keep secrets and credentials out of `package.json` and source control.

# 🗁 Key Takeaways

* `package.json` is the central configuration file of a Node.js project.
* It contains project metadata, dependencies, scripts, and configuration.
* `dependencies` define runtime packages.
* `devDependencies` define development and build-time packages.
* `peerDependencies` declare compatibility requirements with host packages.
* `type` controls the interpretation of `.js` files as CommonJS or ES modules.
* `scripts` provide reusable commands for project workflows.
* `package-lock.json` records the resolved dependency tree.
* npm uses `package.json` to manage project dependencies and operations.
