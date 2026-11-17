# TypeScript Project References

Project References are a TypeScript feature that allows a large codebase to be divided into smaller, independent TypeScript projects.

They help organize large applications, improve build performance, enforce project boundaries, and support incremental compilation.

Instead of compiling an entire codebase as one project, you can create multiple smaller projects and define dependencies between them.

# 🗁 What Are Project References?

Consider a large application containing several independent parts:

```text
project/
├── packages/
│   ├── shared/
│   │   └── tsconfig.json
│   ├── backend/
│   │   └── tsconfig.json
│   └── frontend/
│       └── tsconfig.json
└── tsconfig.json
```

Each directory represents a separate TypeScript project.

The frontend and backend may both depend on the shared package.

Project References allow you to express these relationships explicitly.

# 🗁 Why Use Project References?

Project References are useful when a TypeScript project becomes large enough that a single configuration is difficult to maintain.

They provide several benefits:

* **Better organization:** Divide an application into smaller TypeScript projects.
* **Faster builds:** Rebuild only projects affected by changes when possible.
* **Incremental compilation:** Reuse previous build information.
* **Dependency management:** Define relationships between projects.
* **Project isolation:** Establish clear boundaries between application modules.
* **Declaration output:** Allow dependent projects to consume generated type declarations.

# 🗁 The `references` Option

Project references are configured using the `references` property in `tsconfig.json`.

Example:

```json
{
  "files": [],
  "references": [
    { "path": "./packages/shared" },
    { "path": "./packages/backend" },
    { "path": "./packages/frontend" }
  ]
}
```

Each reference points to another TypeScript project.

The referenced directory must contain a `tsconfig.json` file.

The root configuration acts as a solution configuration that organizes the individual projects.

# 🗁 Creating a Basic Project Structure

Consider this example:

```text
my-app/
├── packages/
│   ├── shared/
│   │   ├── src/
│   │   │   └── index.ts
│   │   └── tsconfig.json
│   └── backend/
│       ├── src/
│       │   └── index.ts
│       └── tsconfig.json
└── tsconfig.json
```

The shared package contains functionality used by other projects.

**packages/shared/src/index.ts**

```typescript
export function add(a: number, b: number): number {
  return a + b;
}
```

**packages/shared/tsconfig.json**

```json
{
  "compilerOptions": {
    "target": "ES2022",
    "module": "NodeNext",
    "moduleResolution": "NodeNext",
    "composite": true,
    "declaration": true,
    "declarationMap": true,
    "rootDir": "src",
    "outDir": "dist",
    "strict": true
  },
  "include": ["src"]
}
```

The `composite` option enables a project to be referenced by another TypeScript project.

It also imposes additional requirements on the project configuration, including explicit source-file inclusion or exclusion.

# 🗁 Creating a Dependent Project

The backend project can reference the shared package.

**packages/backend/src/index.ts**

```typescript
import { add } from "../../shared/dist/index.js";

console.log(add(10, 20));
```

**packages/backend/tsconfig.json**

```json
{
  "compilerOptions": {
    "target": "ES2022",
    "module": "NodeNext",
    "moduleResolution": "NodeNext",
    "composite": true,
    "declaration": true,
    "rootDir": "src",
    "outDir": "dist",
    "strict": true
  },
  "include": ["src"],
  "references": [
    { "path": "../shared" }
  ]
}
```

The backend project declares that it depends on the shared project.

The import uses the shared project's generated output. In a real package-based monorepo, you can also expose the shared package through package metadata and use its package name.

# 🗁 Understanding `composite`

The `composite` compiler option enables a project to participate in a project reference build.

Example:

```json
{
  "compilerOptions": {
    "composite": true,
    "declaration": true
  }
}
```

When enabled:

* The project can be referenced by other TypeScript projects.
* Declaration files are generated.
* Source files must be explicitly included through `files` or `include`.
* The default `rootDir` becomes the directory containing the `tsconfig.json` file when it is not specified.

A referenced project generally needs `composite: true`.

# 🗁 Understanding Declaration Files

Project References rely on declaration files to expose the public type information of referenced projects.

For example:

**Source file**

```typescript
export function multiply(a: number, b: number): number {
  return a * b;
}
```

**Generated declaration file**

```typescript
export declare function multiply(a: number, b: number): number;
```

The declaration file describes the exported API without containing the implementation.

Dependent projects can use these declarations for type checking.

# 🗁 Understanding `declaration`

The `declaration` option instructs TypeScript to generate `.d.ts` files.

Example:

```json
{
  "compilerOptions": {
    "declaration": true
  }
}
```

For referenced projects, declaration output is important because dependent projects consume the referenced project's type information.

# 🗁 Understanding `declarationMap`

The `declarationMap` option generates source maps for declaration files.

Example:

```json
{
  "compilerOptions": {
    "declaration": true,
    "declarationMap": true
  }
}
```

It helps editors navigate from declarations back to the original TypeScript source.

This improves the development experience when working across multiple referenced projects.

# 🗁 Building Referenced Projects

Use TypeScript's build mode to build projects and their dependencies.

Build the root project:

```bash
tsc -b
```

Build a specific project:

```bash
tsc -b packages/backend
```

Build with detailed output:

```bash
tsc -b --verbose
```

Build without performing compilation:

```bash
tsc -b --dry
```

Build mode understands project references and processes dependencies before dependent projects.

# 🗁 Incremental Builds

Project References work with TypeScript's incremental build capabilities.

The compiler stores build information in `.tsbuildinfo` files.

Example configuration:

```json
{
  "compilerOptions": {
    "composite": true,
    "incremental": true,
    "tsBuildInfoFile": "./.tsbuildinfo"
  }
}
```

When a project is rebuilt, TypeScript can reuse previously generated build information to avoid unnecessary work.

The `tsBuildInfoFile` option controls where the build information is stored.

# 🗁 Solution-Style Configuration

A root `tsconfig.json` can coordinate multiple projects without compiling source files directly.

Example:

```text
my-app/
├── packages/
│   ├── shared/
│   │   └── tsconfig.json
│   ├── backend/
│   │   └── tsconfig.json
│   └── frontend/
│       └── tsconfig.json
└── tsconfig.json
```

**Root tsconfig.json**

```json
{
  "files": [],
  "references": [
    { "path": "./packages/shared" },
    { "path": "./packages/backend" },
    { "path": "./packages/frontend" }
  ]
}
```

Using an empty `files` array prevents the root configuration from including source files directly.

It acts as a solution configuration for the referenced projects.

# 🗁 Project Dependencies

Project References can form a dependency graph.

For example:

```text
          shared
          /    \
         /      \
    backend   frontend
```

Both backend and frontend depend on shared.

A more complex application might have:

```text
             core
              |
       ┌──────┴──────┐
       |             |
    database       shared
       |             |
       └──────┬──────┘
              |
          backend
              |
          frontend
```

TypeScript uses the reference graph to determine build order.

Avoid circular project dependencies because they make project boundaries and build relationships difficult to manage.

# 🗁 Useful Build Commands

| Command | Purpose |
| --- | --- |
| `tsc -b` | Build referenced projects |
| `tsc -b --verbose` | Display detailed build information |
| `tsc -b --dry` | Show what would be built without compiling |
| `tsc -b --clean` | Remove outputs from a build |
| `tsc -b --force` | Force rebuilding projects |
| `tsc -b packages/shared` | Build a specific project and its dependencies |

These commands are particularly useful in monorepos and large TypeScript applications.

# 🗁 Common Mistakes

**Forgetting `composite: true`**

A project generally needs `composite: true` before another project can reference it.

**Not generating declaration files**

Referenced projects need declaration output so dependent projects can consume their types.

**Creating circular dependencies**

Project references should form a clear dependency graph.

**Importing source files instead of project outputs**

In a project-reference architecture, dependent projects should consume the referenced project's public API rather than bypassing its boundary.

**Building projects individually in the wrong order**

Use `tsc -b` to let TypeScript process dependencies automatically.

**Confusing references with imports**

The `references` property establishes build and project relationships. It does not automatically create runtime import paths or package aliases.

# 🗁 Best Practices

* Divide large applications into logically independent projects.
* Use `composite: true` for referenced projects.
* Generate declaration files for project APIs.
* Keep dependency relationships explicit.
* Avoid circular references.
* Use a solution-style root configuration.
* Use `tsc -b` for coordinated builds.
* Keep generated output directories separate from source directories.
* Treat project boundaries as architectural boundaries.

# 🗁 Summary

Project References allow a large TypeScript codebase to be divided into smaller, independently configurable projects.

The most important concepts are:

| Feature | Purpose |
| --- | --- |
| `references` | Defines project dependencies |
| `composite` | Enables project referencing |
| `declaration` | Generates type declaration files |
| `declarationMap` | Maps declarations back to source |
| `tsc -b` | Builds projects and their dependencies |
| `.tsbuildinfo` | Stores incremental build information |

Project References are especially useful for monorepos, shared packages, and large applications where maintainability and build performance matter.
