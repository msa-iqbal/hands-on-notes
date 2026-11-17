# package.json

`package.json` is the primary project manifest used by npm and other JavaScript package managers.

It defines project metadata, dependencies, scripts, package entry points, module behavior, and other configuration.

Every Node.js application or reusable npm package typically has a `package.json` file at its root.

# 🗁 What Is package.json?

`package.json` describes how a JavaScript or TypeScript project is organized and managed.

It helps package managers and development tools understand:

* Project name and version.
* Runtime dependencies.
* Development dependencies.
* Available npm scripts.
* Module system configuration.
* Package entry points.
* Supported Node.js versions.
* Package publishing information.

Example:

```json
{
  "name": "my-application",
  "version": "1.0.0",
  "description": "A TypeScript application",
  "type": "module",
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

# 🗁 Creating package.json

Create a new project:

```bash
mkdir my-project
cd my-project
```

Initialize the project:

```bash
npm init
```

npm asks questions about the project and generates `package.json`.

To generate it with default values:

```bash
npm init -y
```

You can also create the file manually.

# 🗁 Project Metadata

Metadata describes the project or package.

Example:

```json
{
  "name": "my-library",
  "version": "1.0.0",
  "description": "A reusable JavaScript library",
  "author": "Developer Name",
  "license": "MIT",
  "keywords": [
    "javascript",
    "typescript",
    "library"
  ]
}
```

Important metadata properties:

| Property | Description |
| --- | --- |
| `name` | Package name |
| `version` | Package version |
| `description` | Short package description |
| `author` | Package author |
| `license` | Package license |
| `keywords` | Searchable package keywords |
| `homepage` | Project homepage |
| `repository` | Source repository information |
| `bugs` | Issue tracker information |
| `private` | Prevents accidental npm publishing when set to `true` |

Example with repository information:

```json
{
  "name": "my-library",
  "version": "1.0.0",
  "repository": {
    "type": "git",
    "url": "https://github.com/example/my-library.git"
  },
  "bugs": {
    "url": "https://github.com/example/my-library/issues"
  },
  "homepage": "https://github.com/example/my-library#readme"
}
```

# 🗁 Package Name and Version

The `name` property identifies the package.

```json
{
  "name": "my-application"
}
```

Package names generally use lowercase letters and may contain hyphens, underscores, and periods, subject to npm naming rules.

The `version` property identifies the current package release.

```json
{
  "version": "1.2.3"
}
```

Semantic Versioning uses this format:

```text
MAJOR.MINOR.PATCH
```

* MAJOR: Breaking changes.
* MINOR: Backward-compatible functionality.
* PATCH: Backward-compatible bug fixes.

# 🗁 Dependencies

The `dependencies` property defines packages required by an application at runtime.

Example:

```json
{
  "dependencies": {
    "express": "^5.0.0",
    "dotenv": "^16.0.0"
  }
}
```

Install a runtime dependency:

```bash
npm install express
```

npm automatically updates `package.json`.

# 🗁 Development Dependencies

The `devDependencies` property defines packages needed during development, testing, and building.

Example:

```json
{
  "devDependencies": {
    "typescript": "^5.0.0",
    "eslint": "^9.0.0",
    "prettier": "^3.0.0",
    "vitest": "^3.0.0"
  }
}
```

Install a development dependency:

```bash
npm install --save-dev typescript
```

Short form:

```bash
npm install -D typescript
```

# 🗁 Peer Dependencies

`peerDependencies` declares packages that a package expects its consuming project to provide.

This is especially useful when building reusable libraries and plugins.

Example:

```json
{
  "name": "my-react-library",
  "version": "1.0.0",
  "peerDependencies": {
    "react": "^18.0.0 || ^19.0.0"
  }
}
```

This indicates that the library expects a compatible React installation in the consuming project.

Peer dependencies help prevent unnecessary duplicate framework installations and communicate compatibility requirements.

# 🗁 Optional Dependencies

`optionalDependencies` defines packages that npm may install but that are not essential for the package to function.

Example:

```json
{
  "optionalDependencies": {
    "fsevents": "^2.3.0"
  }
}
```

If an optional dependency cannot be installed, npm can continue installing the remaining packages.

Applications should handle the absence of optional dependencies gracefully.

# 🗁 Bundled Dependencies

`bundleDependencies` specifies dependencies that should be included in the package archive when publishing.

Example:

```json
{
  "name": "my-package",
  "version": "1.0.0",
  "dependencies": {
    "some-library": "^2.0.0"
  },
  "bundleDependencies": [
    "some-library"
  ]
}
```

The package manager bundles the specified dependencies into the published package archive.

This is useful in certain distribution scenarios where dependencies need to be included with the package.

# 🗁 npm Scripts

The `scripts` property defines reusable commands.

Example:

```json
{
  "scripts": {
    "dev": "tsx watch src/index.ts",
    "build": "tsc",
    "start": "node dist/index.js",
    "lint": "eslint .",
    "format": "prettier --write .",
    "typecheck": "tsc --noEmit",
    "test": "vitest"
  }
}
```

Execute scripts:

```bash
npm run dev
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

List available scripts:

```bash
npm run
```

# 🗁 The type Property

The `type` property controls how Node.js interprets `.js` files within the package scope.

There are two commonly used values:

**CommonJS**

```json
{
  "type": "commonjs"
}
```

**ECMAScript Modules**

```json
{
  "type": "module"
}
```

With `"type": "module"`, `.js` files are interpreted as ECMAScript modules.

With `"type": "commonjs"`, `.js` files are interpreted as CommonJS modules.

If the `type` property is omitted, `.js` files generally use CommonJS semantics, subject to Node.js module-detection behavior and configuration.

The `.mjs` extension explicitly indicates ESM, while `.cjs` explicitly indicates CommonJS.

# 🗁 The main Property

The `main` property specifies the traditional entry point of a package.

Example:

```json
{
  "name": "my-library",
  "version": "1.0.0",
  "main": "./dist/index.js"
}
```

When another application imports the package using CommonJS:

```javascript
const library = require("my-library");
```

Node.js uses the package's entry-point resolution rules to locate the appropriate file.

`main` remains widely used for compatibility with tools and environments that rely on traditional package entry points.

# 🗁 The module Property

The `module` property is a bundler convention commonly used to identify an ESM entry point.

Example:

```json
{
  "main": "./dist/index.cjs",
  "module": "./dist/index.js"
}
```

Some bundlers use `module` to select an ESM build.

However, `module` is not a standard Node.js package entry-point field. Node.js does not generally use it for ordinary package resolution.

# 🗁 The exports Property

The `exports` property defines the public entry points of a package.

It provides more control over package imports than `main`.

Example:

```json
{
  "name": "my-library",
  "version": "1.0.0",
  "type": "module",
  "exports": {
    ".": "./dist/index.js",
    "./utils": "./dist/utils.js"
  }
}
```

Consumers can import:

```javascript
import { greet } from "my-library";
import { formatDate } from "my-library/utils";
```

The `exports` field also encapsulates package internals. Files that are not exposed through its export map generally cannot be imported through package-name subpaths.

Conditional exports support different entry points for different environments:

```json
{
  "exports": {
    ".": {
      "import": "./dist/index.js",
      "require": "./dist/index.cjs"
    }
  }
}
```

This allows ESM and CommonJS consumers to receive different builds.

# 🗁 The types Property

The `types` property identifies the TypeScript declaration entry point for a package.

Example:

```json
{
  "name": "my-library",
  "version": "1.0.0",
  "main": "./dist/index.js",
  "types": "./dist/index.d.ts"
}
```

TypeScript uses the declaration file to understand the package's exported types.

The `typings` property is an older alias for `types`.

For modern packages using `exports`, declaration files can also be exposed through conditional export entries:

```json
{
  "type": "module",
  "exports": {
    ".": {
      "types": "./dist/index.d.ts",
      "import": "./dist/index.js",
      "require": "./dist/index.cjs"
    }
  }
}
```

# 🗁 The files Property

The `files` property controls which files and directories are included when a package is packed or published.

Example:

```json
{
  "files": [
    "dist/",
    "README.md",
    "LICENSE"
  ]
}
```

This helps prevent unnecessary source files, development configuration, and other unwanted files from being included in the published package.

You can inspect the package contents before publishing:

```bash
npm pack --dry-run
```

# 🗁 The engines Property

The `engines` property declares compatible runtime versions.

Example:

```json
{
  "engines": {
    "node": ">=20.0.0",
    "npm": ">=10.0.0"
  }
}
```

This communicates which Node.js and npm versions the project supports.

The `engines` property does not automatically guarantee that an incompatible runtime cannot execute the application. Enforcement depends on the package manager and its configuration.

# 🗁 The private Property

The `private` property prevents accidental publication to the npm Registry.

Example:

```json
{
  "name": "my-application",
  "version": "1.0.0",
  "private": true
}
```

This is recommended for private applications and most internal projects that are not intended to be published as npm packages.

# 🗁 The workspaces Property

The `workspaces` property defines packages managed inside a monorepo.

Example:

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

Possible directory structure:

```text
my-monorepo/
├── package.json
├── package-lock.json
├── apps/
│   ├── web/
│   │   └── package.json
│   └── api/
│       └── package.json
└── packages/
    └── shared/
        └── package.json
```

Install dependencies:

```bash
npm install
```

Run a script in a workspace:

```bash
npm run build --workspace=apps/web
```

Workspaces make it easier to manage multiple related packages from one repository.

# 🗁 The config Property

The `config` property provides configuration values that npm exposes to package scripts through environment variables.

Example:

```json
{
  "config": {
    "port": "3000"
  },
  "scripts": {
    "start": "node server.js"
  }
}
```

Inside the npm script environment, the value can be accessed through:

```javascript
console.log(process.env.npm_package_config_port);
```

This is useful for simple package-specific script configuration.

# 🗁 The bin Property

The `bin` property defines executable commands provided by an npm package.

Example:

```json
{
  "name": "my-cli",
  "version": "1.0.0",
  "bin": {
    "my-cli": "./bin/cli.js"
  }
}
```

The executable file should begin with a Node.js shebang:

```javascript
#!/usr/bin/env node

console.log("Hello from my CLI!");
```

After installation, npm creates the appropriate executable link so users can run:

```bash
my-cli
```

This is commonly used when developing command-line tools.

# 🗁 The os and cpu Properties

These properties can restrict package compatibility to particular operating systems or CPU architectures.

Example:

```json
{
  "os": [
    "linux",
    "darwin"
  ],
  "cpu": [
    "x64",
    "arm64"
  ]
}
```

This package declares compatibility with Linux and macOS on the listed CPU architectures.

These fields are useful for platform-specific packages and optional native binaries.

# 🗁 package.json vs package-lock.json

These files have different responsibilities.

| Feature | package.json | package-lock.json |
| --- | --- | --- |
| Project metadata | Yes | Limited |
| Dependency declarations | Yes | Records resolved dependencies |
| Exact installed versions | Not necessarily | Yes |
| Dependency ranges | Yes | Records the resolved tree |
| npm scripts | Yes | No |
| Module configuration | Yes | No |
| Reproducible installation | Together with lockfile | Yes |

`package.json` describes the project and its dependency requirements.

`package-lock.json` records the resolved dependency tree so installations can be reproduced.

Both files should generally be committed to version control.

# 🗁 Practical TypeScript Project Example

A typical TypeScript backend project might use the following structure:

```text
my-api/
├── node_modules/
├── src/
│   └── index.ts
├── dist/
├── package.json
├── package-lock.json
├── tsconfig.json
├── eslint.config.js
└── .prettierrc.json
```

Example `package.json`:

```json
{
  "name": "my-api",
  "version": "1.0.0",
  "private": true,
  "type": "module",
  "description": "A TypeScript backend application",
  "scripts": {
    "dev": "tsx watch src/index.ts",
    "build": "tsc",
    "start": "node dist/index.js",
    "lint": "eslint .",
    "format": "prettier --write .",
    "format:check": "prettier --check .",
    "typecheck": "tsc --noEmit"
  },
  "dependencies": {
    "express": "^5.0.0"
  },
  "devDependencies": {
    "@types/express": "^5.0.0",
    "@types/node": "^22.0.0",
    "eslint": "^9.0.0",
    "prettier": "^3.0.0",
    "tsx": "^4.0.0",
    "typescript": "^5.0.0"
  },
  "engines": {
    "node": ">=20.0.0"
  }
}
```

The versions are illustrative. Choose compatible package versions for the actual project.

# 🗁 Best Practices

* Keep `package.json` organized and readable.
* Use meaningful package names and descriptions.
* Follow Semantic Versioning for published packages.
* Separate runtime and development dependencies.
* Commit both `package.json` and `package-lock.json`.
* Use `"private": true` for applications that should not be published.
* Declare supported Node.js versions using `engines`.
* Use `exports` to define public package entry points.
* Use `types` to expose TypeScript declaration files.
* Use `files` to control published package contents.
* Keep scripts consistent across development environments.
* Avoid placing secrets or credentials in package metadata.

# 🗁 Summary

`package.json` is the central manifest of a JavaScript or TypeScript project.

Its main responsibilities include:

* Defining project metadata.
* Managing runtime and development dependencies.
* Defining npm scripts.
* Configuring module behavior.
* Exposing package entry points.
* Declaring TypeScript declarations.
* Controlling package publishing.
* Managing monorepo workspaces.

Understanding `package.json` is essential for managing dependencies, configuring Node.js applications, and developing reusable npm packages.
