# npm Scripts

npm scripts are custom commands defined in the `scripts` field of `package.json`. They automate common development tasks such as starting an application, running tests, building production files, formatting code, and executing linters.

Instead of remembering long commands, developers can define reusable scripts and execute them through npm.

# 🗁 What Are npm Scripts?

npm scripts are commands configured inside `package.json`.

Example:

```json
{
  "scripts": {
    "start": "node src/index.js",
    "dev": "node --watch src/index.js",
    "test": "node --test",
    "build": "node scripts/build.js"
  }
}
```

Run a script using:

```bash
npm run dev
```

npm executes the command associated with the `dev` script.

# 🗁 Defining Scripts

Scripts are defined inside the `scripts` object in `package.json`.

```json
{
  "name": "my-project",
  "version": "1.0.0",
  "scripts": {
    "start": "node src/index.js",
    "dev": "node --watch src/index.js",
    "test": "node --test"
  }
}
```

Each property represents a script name, and its value is the command npm should execute.

The general structure is:

```json
{
  "scripts": {
    "script-name": "command-to-execute"
  }
}
```

You can define as many scripts as your project requires.

# 🗁 Running Scripts

npm provides different commands for executing scripts.

**npm run**

Executes a custom script.

```bash
npm run dev
npm run build
npm run lint
```

**npm start**

Runs the `start` script.

```bash
npm start
```

**npm test**

Runs the `test` script.

```bash
npm test
```

**npm stop**

Runs the `stop` script when defined.

```bash
npm stop
```

**npm restart**

Runs the `restart` script when defined. If no restart script exists, npm uses its restart lifecycle behavior, which can involve running `stop` and `start`.

```bash
npm restart
```

Some lifecycle script names have dedicated npm commands, while custom script names require `npm run`.

# 🗁 Listing Available Scripts

Use:

```bash
npm run
```

npm displays the scripts defined in the current package.

Example output:

```text
Scripts available in my-project:
  start
    node src/index.js

  dev
    node --watch src/index.js

  test
    node --test

  build
    node scripts/build.js
```

This is useful when working with an unfamiliar project.

# 🗁 Predefined Lifecycle Scripts

npm recognizes certain script names and associates them with specific lifecycle events.

Common lifecycle scripts include:

| Script | Purpose |
| --- | --- |
| `preinstall` | Runs before dependencies are installed |
| `install` | Runs during package installation |
| `postinstall` | Runs after installation |
| `prepare` | Runs during package preparation and publishing workflows |
| `prepublishOnly` | Runs before publishing |
| `prepack` | Runs before creating a package archive |
| `postpack` | Runs after creating a package archive |
| `prestart` | Runs before `start` |
| `poststart` | Runs after `start` |
| `pretest` | Runs before `test` |
| `posttest` | Runs after `test` |

For supported lifecycle scripts, npm automatically executes the corresponding `pre` and `post` scripts around the main script.

Example:

```json
{
  "scripts": {
    "pretest": "npm run lint",
    "test": "node --test",
    "posttest": "echo Tests completed"
  }
}
```

Running:

```bash
npm test
```

Executes:

```text
pretest
test
posttest
```

The sequence stops if a preceding lifecycle script fails.

Note that not every script name supports automatic `pre` and `post` execution. For example, npm does not automatically run `prebuild` and `postbuild` around `npm run build`.

# 🗁 Script Execution Environment

npm provides a special execution environment when running scripts.

**PATH modification**

npm automatically adds local executable directories to the `PATH` environment variable.

For example, after installing ESLint:

```bash
npm install --save-dev eslint
```

You can execute it through a script without specifying its full path:

```json
{
  "scripts": {
    "lint": "eslint ."
  }
}
```

Run:

```bash
npm run lint
```

npm resolves the local executable from:

```text
node_modules/.bin/
```

This helps ensure the project uses its installed tool version rather than depending on a globally installed version.

**Working directory**

npm runs scripts from the package root directory by default, regardless of the directory from which the npm command was invoked within the project.

For example:

```text
my-project/
├── scripts/
│   └── build.js
├── src/
│   └── index.js
└── package.json
```

A script such as:

```json
{
  "scripts": {
    "build": "node scripts/build.js"
  }
}
```

uses the project root as its working directory.

**Environment variables**

npm exposes several environment variables to scripts.

Examples:

```javascript
console.log(process.env.npm_package_name);
console.log(process.env.npm_package_version);
console.log(process.env.npm_lifecycle_event);
```

These provide information about the package and the currently running npm script.

# 🗁 Passing Arguments to Scripts

You can pass additional arguments to a script using `--`.

Example:

```json
{
  "scripts": {
    "test": "node --test"
  }
}
```

Run:

```bash
npm run test -- --watch
```

The additional `--watch` argument is passed to the underlying command.

Another example:

```json
{
  "scripts": {
    "build": "node scripts/build.js"
  }
}
```

Run:

```bash
npm run build -- --production
```

Inside the Node.js script, the argument can be accessed through:

```javascript
console.log(process.argv);
```

The `--` separator tells npm that the following arguments should be forwarded to the script command.

# 🗁 Running Multiple Commands

You can combine multiple commands in a single npm script.

**Sequential execution**

Use `&&` to execute commands in sequence.

```json
{
  "scripts": {
    "build": "npm run clean && npm run compile"
  }
}
```

The second command runs only if the first command succeeds.

**Independent sequential execution**

Use `;` on POSIX shells to execute commands regardless of the previous command's exit status.

```json
{
  "scripts": {
    "check": "npm run lint; npm run test"
  }
}
```

Shell behavior can differ across operating systems, so this approach is less portable than using dedicated tooling.

**Parallel execution**

You can use `&` in many POSIX shells to run commands in the background.

```bash
npm run lint & npm run test & wait
```

For cross-platform parallel execution, tools such as `concurrently` are often more reliable.

Install it:

```bash
npm install --save-dev concurrently
```

Define:

```json
{
  "scripts": {
    "dev": "concurrently \"npm run server\" \"npm run client\""
  }
}
```

# 🗁 Script Composition

An npm script can invoke another npm script.

```json
{
  "scripts": {
    "lint": "eslint .",
    "test": "node --test",
    "check": "npm run lint && npm run test"
  }
}
```

Run:

```bash
npm run check
```

This executes the lint script first and runs the tests only if linting succeeds.

Script composition helps keep individual tasks small and reusable.

# 🗁 Cross-Platform Scripts

Shell commands do not always behave identically across Windows, Linux, and macOS.

For example:

```json
{
  "scripts": {
    "clean": "rm -rf dist"
  }
}
```

This command works in common POSIX shells but is not directly compatible with the standard Windows Command Prompt.

For cross-platform projects, consider using Node.js APIs or cross-platform packages.

Example using Node.js:

```javascript
// scripts/clean.js

import { rm } from "node:fs/promises";

await rm("dist", {
  recursive: true,
  force: true
});

console.log("Build directory cleaned.");
```

Define:

```json
{
  "scripts": {
    "clean": "node scripts/clean.js"
  }
}
```

This approach avoids shell-specific deletion commands.

Another option is a cross-platform utility such as `rimraf`:

```bash
npm install --save-dev rimraf
```

```json
{
  "scripts": {
    "clean": "rimraf dist"
  }
}
```

# 🗁 Environment-Specific Scripts

Applications often need different configurations for development, testing, and production.

Example:

```json
{
  "scripts": {
    "dev": "node src/index.js",
    "test": "node --test",
    "start": "node src/index.js"
  }
}
```

A script can also set environment variables, but inline syntax differs between operating systems.

For cross-platform environment variable management, use a package such as `cross-env`.

Install:

```bash
npm install --save-dev cross-env
```

Configure:

```json
{
  "scripts": {
    "dev": "cross-env NODE_ENV=development node src/index.js",
    "start": "cross-env NODE_ENV=production node src/index.js"
  }
}
```

Inside JavaScript:

```javascript
console.log(process.env.NODE_ENV);
```

# 🗁 npm Script Configuration Options

npm provides options that change how scripts execute.

**--if-present**

Prevents an error when a requested script is not defined.

```bash
npm run build --if-present
```

This is useful in monorepos where not every package defines the same scripts.

**--silent**

Suppresses most npm output.

```bash
npm run build --silent
```

**--workspace**

Runs a script in a specific workspace.

```bash
npm run build --workspace=frontend
```

**--workspaces**

Runs a script across configured workspaces.

```bash
npm run test --workspaces
```

**--foreground-scripts**

Runs lifecycle scripts in the foreground, which can make their output easier to inspect.

```bash
npm install --foreground-scripts
```

# 🗁 Practical Project Example

Consider a Node.js application with the following structure:

```text
my-api/
├── src/
│   └── index.js
├── scripts/
│   └── clean.js
├── test/
│   └── index.test.js
├── package.json
└── package-lock.json
```

A practical `package.json`:

```json
{
  "name": "my-api",
  "version": "1.0.0",
  "type": "module",
  "scripts": {
    "dev": "node --watch src/index.js",
    "start": "node src/index.js",
    "clean": "node scripts/clean.js",
    "test": "node --test",
    "lint": "eslint .",
    "format": "prettier --write .",
    "check": "npm run lint && npm run test",
    "prebuild": "npm run clean",
    "build": "node scripts/build.js"
  },
  "devDependencies": {
    "eslint": "^9.0.0",
    "prettier": "^3.3.0"
  }
}
```

Common commands:

```bash
# Start development server
npm run dev

# Start application
npm start

# Run tests
npm test

# Run linting
npm run lint

# Format source files
npm run format

# Run linting and tests
npm run check

# Run the build lifecycle
npm run build
```

The `prebuild` script automatically runs before `build` because npm supports the `pre` lifecycle convention for the `build` lifecycle event.

# 🗁 Best Practices

* Use descriptive and consistent script names.
* Keep scripts focused on one responsibility.
* Compose smaller scripts instead of duplicating commands.
* Use local project dependencies instead of relying on globally installed tools.
* Prefer Node.js scripts or cross-platform utilities for filesystem operations.
* Use `&&` when later commands should run only after earlier commands succeed.
* Use `npm run` to inspect available scripts in unfamiliar projects.
* Keep environment-specific configuration outside hardcoded production scripts when practical.
* Use `--` when forwarding arguments to the underlying command.
* Avoid unnecessarily complicated shell commands.
* Keep lifecycle scripts predictable, especially installation and publishing hooks.
* Review scripts before installing dependencies because lifecycle scripts can execute code during installation.

# 🗁 Key Takeaways

* npm scripts are defined in the `scripts` field of `package.json`.
* `npm run script-name` executes custom scripts.
* `npm start` and `npm test` are shortcuts for standard script names.
* npm automatically adds local executable directories to the script `PATH`.
* Lifecycle scripts can run automatically before or after supported npm operations.
* Scripts can invoke other scripts to create reusable workflows.
* Use `&&` for sequential commands that depend on successful execution.
* Cross-platform tools help avoid operating-system-specific shell problems.
* npm scripts are a fundamental part of JavaScript project automation.
