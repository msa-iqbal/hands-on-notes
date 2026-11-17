# TypeScript Configuration Basics

TypeScript configuration defines how the TypeScript compiler processes, checks, and generates JavaScript from a project.

A TypeScript project can be configured through a file named `tsconfig.json`.

This file controls compiler behavior, including:

* Which files belong to the project.
* Which JavaScript version to generate.
* Which module system to use.
* How strictly types are checked.
* How modules are resolved.
* Where compiled files are generated.
* Which compiler features are enabled.

A well-structured configuration makes a TypeScript project consistent, maintainable, and predictable.

# 🗁 What Is tsconfig.json?

`tsconfig.json` is the primary configuration file used by the TypeScript compiler.

Example:

```json
{
  "compilerOptions": {
    "target": "ES2022",
    "module": "NodeNext",
    "strict": true,
    "outDir": "./dist"
  },
  "include": ["src/**/*"],
  "exclude": ["node_modules", "dist"]
}
```

This configuration tells TypeScript how to compile and validate the project.

# 🗁 Creating a tsconfig.json File

You can generate a configuration file using the TypeScript compiler.

```bash
tsc --init
```

This command creates a `tsconfig.json` file in the current directory.

You can also create it manually.

A minimal configuration:

```json
{
  "compilerOptions": {
    "target": "ES2022",
    "strict": true
  }
}
```

# 🗁 Understanding the Configuration Structure

A typical `tsconfig.json` contains several important sections.

```json
{
  "compilerOptions": {},
  "include": [],
  "exclude": [],
  "files": [],
  "extends": ""
}
```

These properties serve different purposes.

| Property | Purpose |
| --- | --- |
| `compilerOptions` | Configures TypeScript compiler behavior |
| `include` | Specifies files to include |
| `exclude` | Excludes files from automatic inclusion |
| `files` | Explicitly lists source files |
| `extends` | Inherits configuration from another configuration file |

The `compilerOptions` object contains most of the settings used in everyday TypeScript development.

# 🗁 Understanding compilerOptions

`compilerOptions` defines how TypeScript checks and compiles source code.

Example:

```json
{
  "compilerOptions": {
    "target": "ES2022",
    "module": "NodeNext",
    "strict": true,
    "rootDir": "./src",
    "outDir": "./dist",
    "sourceMap": true
  }
}
```

Important options include:

| Option | Description |
| --- | --- |
| `target` | JavaScript language version for emitted code |
| `module` | Module system used for emitted JavaScript |
| `strict` | Enables strict type-checking options |
| `rootDir` | Defines the expected source directory structure |
| `outDir` | Specifies the output directory |
| `sourceMap` | Generates source map files |
| `declaration` | Generates TypeScript declaration files |
| `noEmit` | Prevents JavaScript and other output files from being generated |

These options are covered individually in the configuration section.

# 🗁 Understanding include

The `include` property specifies file patterns that TypeScript should include in the project.

Example:

```json
{
  "include": ["src/**/*"]
}
```

This includes files under the `src` directory and its subdirectories.

Multiple patterns can be specified:

```json
{
  "include": [
    "src/**/*",
    "tests/**/*",
    "scripts/**/*.ts"
  ]
}
```

Common patterns:

| Pattern | Meaning |
| --- | --- |
| `src/**/*` | All files and subdirectories under `src` |
| `src/**/*.ts` | TypeScript files under `src` |
| `tests/**/*.ts` | TypeScript files under `tests` |
| `**/*.ts` | TypeScript files throughout the project |

The `include` property uses glob patterns to identify files.

# 🗁 Understanding exclude

The `exclude` property specifies files and directories excluded from automatic inclusion through `include`.

Example:

```json
{
  "include": ["src/**/*"],
  "exclude": [
    "node_modules",
    "dist",
    "coverage"
  ]
}
```

Common exclusions include:

* `node_modules`
* `dist`
* `build`
* `coverage`
* Generated files.

Important: `exclude` does not guarantee that a file can never become part of the TypeScript program.

An excluded file can still be included through an import, a `files` entry, a type reference, or another inclusion mechanism.

# 🗁 Understanding files

The `files` property explicitly specifies individual files that belong to the TypeScript project.

Example:

```json
{
  "files": [
    "src/index.ts",
    "src/config.ts",
    "src/types.ts"
  ]
}
```

Only the listed root files are explicitly included through this property.

Their imported dependencies can also become part of the program.

Use `files` when a project requires precise control over its root source files.

For larger applications, `include` is generally more convenient.

# 🗁 Understanding extends

The `extends` property allows one configuration file to inherit settings from another.

This helps maintain consistent compiler settings across multiple projects.

Example:

```json
{
  "extends": "./tsconfig.base.json",
  "compilerOptions": {
    "outDir": "./dist"
  },
  "include": ["src/**/*"]
}
```

Base configuration:

```json
{
  "compilerOptions": {
    "target": "ES2022",
    "strict": true,
    "noImplicitOverride": true
  }
}
```

The child configuration inherits the base settings and can override individual options.

This is especially useful in monorepos and projects containing multiple TypeScript applications.

# 🗁 Understanding tsconfig File Discovery

When you run the TypeScript compiler without specifying input files, it searches for a `tsconfig.json` file in the current directory.

```bash
tsc
```

If a configuration file is found, TypeScript compiles the project according to its settings.

You can explicitly specify a configuration file:

```bash
tsc -p tsconfig.json
```

You can also specify a different configuration:

```bash
tsc -p tsconfig.build.json
```

The `-p` option is an alias for `--project`.

When you pass individual source files directly:

```bash
tsc src/index.ts
```

TypeScript compiles those files using command-line defaults rather than loading the project's `tsconfig.json`.

This distinction is important when debugging unexpected compiler behavior.

# 🗁 Understanding the Project Structure

A common TypeScript project structure looks like this:

```text
my-project/
├── src/
│   ├── index.ts
│   ├── app.ts
│   ├── services/
│   │   └── user.service.ts
│   └── types/
│       └── user.ts
├── tests/
│   └── app.test.ts
├── dist/
├── package.json
├── tsconfig.json
└── tsconfig.build.json
```

The main configuration might look like this:

```json
{
  "compilerOptions": {
    "target": "ES2022",
    "module": "NodeNext",
    "strict": true,
    "rootDir": "./src",
    "outDir": "./dist",
    "sourceMap": true
  },
  "include": ["src/**/*"],
  "exclude": ["node_modules", "dist"]
}
```

This configuration establishes a clear separation between source code and generated output.

# 🗁 Inspecting the Active Configuration

TypeScript provides commands for inspecting project configuration.

Show the resolved configuration:

```bash
tsc --showConfig
```

This displays the effective configuration after inherited settings and defaults have been applied.

Check which files belong to the project:

```bash
tsc --listFiles
```

Show only files that are part of the project:

```bash
tsc --listFilesOnly
```

These commands are useful when troubleshooting unexpected file inclusion or configuration inheritance.

# 🗁 Common Configuration Mistakes

**Running TypeScript with individual files**

```bash
tsc src/index.ts
```

This bypasses `tsconfig.json`.

Use the project configuration instead:

```bash
tsc -p tsconfig.json
```

**Incorrect include patterns**

```json
{
  "include": ["source/**/*"]
}
```

If the actual directory is named `src`, this pattern will not match the intended files.

**Incorrect rootDir**

```json
{
  "compilerOptions": {
    "rootDir": "./src"
  }
}
```

If included source files are located outside `src`, TypeScript may report that they are outside the configured root directory.

**Forgetting to exclude generated output**

Generated JavaScript or declaration files can accidentally become part of the input project when directory patterns are too broad.

Use appropriate inclusion patterns and exclusions.

**Assuming exclude blocks imported files**

An excluded file can still enter the project through imports or other inclusion mechanisms.

# 🗁 Best Practices

* Keep `tsconfig.json` at the project root.
* Use `strict: true` for stronger type safety.
* Separate source files from generated output.
* Use meaningful `include` and `exclude` patterns.
* Use `extends` to share configuration across projects.
* Create separate configurations for different build targets when necessary.
* Use `tsc --showConfig` to inspect effective settings.
* Use `tsc -p` to compile through the intended project configuration.
* Avoid unnecessary compiler options that make the project difficult to maintain.
* Document unusual configuration decisions.

# 🗁 Summary

`tsconfig.json` defines how TypeScript analyzes and compiles a project.

Key concepts:

* `compilerOptions` controls compiler behavior.
* `include` selects files through glob patterns.
* `exclude` removes files from automatic inclusion.
* `files` explicitly lists root source files.
* `extends` inherits settings from another configuration.
* `tsc -p` compiles using a specified project configuration.
* `tsc --showConfig` displays the resolved configuration.
* A well-organized configuration improves consistency and maintainability.
