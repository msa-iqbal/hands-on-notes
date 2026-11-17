# TypeScript Configuration Patterns

TypeScript configuration patterns are reusable approaches for organizing and managing `tsconfig.json` files across different project types.

A good configuration helps maintain consistent compiler behavior, improve type safety, simplify builds, and support different development environments.

This becomes especially important when working with frontend applications, Node.js applications, libraries, and monorepos.

# 🗁 Understanding `tsconfig.json`

The `tsconfig.json` file defines the TypeScript compiler configuration for a project.

Example:

```json
{
  "compilerOptions": {
    "target": "ES2022",
    "module": "ESNext",
    "moduleResolution": "Bundler",
    "strict": true,
    "declaration": true,
    "sourceMap": true,
    "outDir": "./dist",
    "rootDir": "./src"
  },
  "include": ["src"],
  "exclude": ["node_modules", "dist"]
}
```

The configuration determines:

* Which files TypeScript processes.
* Which JavaScript features the compiler targets.
* How modules are resolved and emitted.
* Which type-checking rules are enabled.
* Where compiled files are generated.
* Whether declaration files and source maps are produced.

# 🗁 Understanding Configuration Inheritance

TypeScript supports configuration inheritance through the `extends` option.

This allows multiple projects to share common compiler settings.

Consider this structure:

```text
project/
├── tsconfig.base.json
├── frontend/
│   └── tsconfig.json
└── backend/
    └── tsconfig.json
```

**tsconfig.base.json**

```json
{
  "compilerOptions": {
    "strict": true,
    "forceConsistentCasingInFileNames": true,
    "skipLibCheck": true,
    "noEmitOnError": true
  }
}
```

**frontend/tsconfig.json**

```json
{
  "extends": "../tsconfig.base.json",
  "compilerOptions": {
    "target": "ES2022",
    "module": "ESNext",
    "moduleResolution": "Bundler",
    "lib": ["ES2022", "DOM", "DOM.Iterable"]
  },
  "include": ["src"]
}
```

**backend/tsconfig.json**

```json
{
  "extends": "../tsconfig.base.json",
  "compilerOptions": {
    "target": "ES2022",
    "module": "NodeNext",
    "moduleResolution": "NodeNext",
    "lib": ["ES2022"]
  },
  "include": ["src"]
}
```

Both projects inherit the common settings while maintaining environment-specific configurations.

# 🗁 Important Inheritance Rules

When using `extends`, remember:

* Child compiler options override inherited options with the same property.
* `files`, `include`, and `exclude` in the child configuration replace their inherited counterparts when specified.
* Relative paths are resolved relative to the configuration file where they were originally declared.
* The `references` property is not inherited.

Understanding these rules helps prevent unexpected configuration behavior.

# 🗁 Shared Base Configuration

A shared base configuration is useful when several TypeScript projects belong to the same codebase.

Example:

```json
{
  "compilerOptions": {
    "strict": true,
    "noImplicitOverride": true,
    "noFallthroughCasesInSwitch": true,
    "forceConsistentCasingInFileNames": true,
    "skipLibCheck": true
  }
}
```

You can name the file:

```text
tsconfig.base.json
```

or:

```text
tsconfig.json
```

The filename is a project convention. TypeScript does not require a particular name for an extended configuration.

# 🗁 Frontend Application Configuration

Frontend applications commonly use TypeScript with bundlers such as Vite, Webpack, or Rollup.

A typical configuration:

```json
{
  "compilerOptions": {
    "target": "ES2022",
    "module": "ESNext",
    "moduleResolution": "Bundler",
    "lib": ["ES2022", "DOM", "DOM.Iterable"],
    "jsx": "react-jsx",
    "strict": true,
    "noEmit": true,
    "skipLibCheck": true,
    "forceConsistentCasingInFileNames": true
  },
  "include": ["src"]
}
```

Important options:

| Option | Purpose |
| --- | --- |
| `target` | Determines the JavaScript syntax target |
| `module` | Preserves modern ES module syntax |
| `moduleResolution` | Uses bundler-compatible module resolution |
| `lib` | Includes browser and JavaScript API definitions |
| `jsx` | Configures JSX transformation behavior |
| `strict` | Enables strict type checking |
| `noEmit` | Prevents TypeScript from generating JavaScript |
| `skipLibCheck` | Skips checking declaration files |

When a bundler handles compilation, `noEmit` allows TypeScript to perform type checking without generating JavaScript output.

# 🗁 Node.js Application Configuration

Node.js applications that use native ECMAScript modules can use Node.js-aware module settings.

Example:

```json
{
  "compilerOptions": {
    "target": "ES2022",
    "module": "NodeNext",
    "moduleResolution": "NodeNext",
    "lib": ["ES2022"],
    "strict": true,
    "rootDir": "./src",
    "outDir": "./dist",
    "declaration": true,
    "sourceMap": true,
    "noEmitOnError": true,
    "forceConsistentCasingInFileNames": true,
    "skipLibCheck": true
  },
  "include": ["src"],
  "exclude": ["node_modules", "dist"]
}
```

This configuration:

* Uses Node.js module resolution.
* Generates JavaScript output.
* Generates declaration files.
* Generates source maps.
* Separates source and compiled files.
* Prevents emitting output when compilation reports errors.

The project's `package.json` should also declare the intended module format when necessary.

For example:

```json
{
  "type": "module"
}
```

# 🗁 Library Configuration

A TypeScript library may need to generate JavaScript and declaration files for consumers.

Example:

```json
{
  "compilerOptions": {
    "target": "ES2020",
    "module": "ESNext",
    "moduleResolution": "Bundler",
    "strict": true,
    "declaration": true,
    "declarationMap": true,
    "sourceMap": true,
    "rootDir": "./src",
    "outDir": "./dist",
    "noEmitOnError": true,
    "skipLibCheck": true
  },
  "include": ["src"]
}
```

Important options:

* `declaration` generates `.d.ts` files.
* `declarationMap` generates maps for declaration files.
* `sourceMap` generates source maps for JavaScript output.
* `rootDir` defines the source directory.
* `outDir` defines the output directory.

A library's package configuration must also expose its generated entry points correctly.

For example:

```json
{
  "name": "my-library",
  "type": "module",
  "main": "./dist/index.cjs",
  "module": "./dist/index.js",
  "types": "./dist/index.d.ts",
  "exports": {
    ".": {
      "types": "./dist/index.d.ts",
      "import": "./dist/index.js",
      "require": "./dist/index.cjs"
    }
  }
}
```

This package metadata assumes that the build process actually generates the corresponding files. A TypeScript configuration alone does not generate both CommonJS and ESM outputs automatically.

# 🗁 Monorepo Configuration

A monorepo contains multiple related projects in one repository.

For example:

```text
monorepo/
├── apps/
│   ├── frontend/
│   │   └── tsconfig.json
│   └── backend/
│       └── tsconfig.json
├── packages/
│   ├── shared/
│   │   └── tsconfig.json
│   └── ui/
│       └── tsconfig.json
├── tsconfig.base.json
└── tsconfig.json
```

The root configuration can coordinate individual projects.

**tsconfig.base.json**

```json
{
  "compilerOptions": {
    "strict": true,
    "noImplicitOverride": true,
    "noFallthroughCasesInSwitch": true,
    "forceConsistentCasingInFileNames": true,
    "skipLibCheck": true
  }
}
```

**Root tsconfig.json**

```json
{
  "files": [],
  "references": [
    { "path": "./apps/frontend" },
    { "path": "./apps/backend" },
    { "path": "./packages/shared" },
    { "path": "./packages/ui" }
  ]
}
```

Each referenced project can extend the shared base configuration and define its own environment-specific settings.

For projects using TypeScript Project References, configure `composite: true` and the appropriate declaration output settings in the referenced projects.

# 🗁 Development and Production Configurations

Some applications use separate configurations for development and production workflows.

For example:

```text
project/
├── src/
├── tsconfig.json
├── tsconfig.build.json
└── tsconfig.test.json
```

**tsconfig.json**

```json
{
  "compilerOptions": {
    "target": "ES2022",
    "module": "NodeNext",
    "moduleResolution": "NodeNext",
    "strict": true,
    "sourceMap": true
  },
  "include": ["src"]
}
```

**tsconfig.build.json**

```json
{
  "extends": "./tsconfig.json",
  "compilerOptions": {
    "noEmit": false,
    "declaration": true,
    "outDir": "./dist",
    "rootDir": "./src"
  },
  "exclude": ["node_modules", "dist", "**/*.test.ts"]
}
```

**tsconfig.test.json**

```json
{
  "extends": "./tsconfig.json",
  "compilerOptions": {
    "noEmit": true,
    "types": ["node", "vitest/globals"]
  },
  "include": ["src", "tests"]
}
```

The test configuration assumes that the corresponding type packages are installed.

You can explicitly select a configuration:

```bash
tsc -p tsconfig.build.json
```

Or:

```bash
tsc -p tsconfig.test.json
```

# 🗁 Useful Compiler Options for Production

Several compiler options are particularly useful when preparing applications for production.

| Option | Purpose |
| --- | --- |
| `strict` | Enables strict type checking |
| `noEmitOnError` | Prevents output generation when compilation reports errors |
| `declaration` | Generates type declaration files |
| `sourceMap` | Generates source maps |
| `removeComments` | Removes comments from emitted JavaScript |
| `noUnusedLocals` | Reports unused local variables |
| `noUnusedParameters` | Reports unused function parameters |
| `forceConsistentCasingInFileNames` | Enforces consistent file import casing |

Example:

```json
{
  "compilerOptions": {
    "strict": true,
    "noEmitOnError": true,
    "noUnusedLocals": true,
    "noUnusedParameters": true,
    "forceConsistentCasingInFileNames": true
  }
}
```

These options help maintain code quality, but production optimization such as minification and tree shaking is generally handled by the build tool.

# 🗁 Inspecting the Effective Configuration

When configurations extend other files, it can be difficult to determine the final compiler settings.

Use:

```bash
tsc --showConfig
```

This displays the resolved configuration after TypeScript processes inheritance and defaults.

To check which files belong to a project:

```bash
tsc --listFiles
```

To investigate configuration and file inclusion:

```bash
tsc --explainFiles
```

These commands are useful when debugging unexpected compiler behavior.

# 🗁 Best Practices

* Use a shared base configuration when multiple projects need common settings.
* Enable `strict` for stronger type safety.
* Keep frontend, backend, and library configurations separate when their requirements differ.
* Use `extends` to reduce configuration duplication.
* Keep `include` and `exclude` patterns explicit.
* Configure module resolution according to the actual runtime or bundler.
* Use project references for large projects with independent compilation boundaries.
* Avoid copying compiler options without understanding their behavior.
* Verify the effective configuration using `tsc --showConfig`.
* Keep generated output outside source directories.

# 🗁 Summary

TypeScript configuration patterns help organize compiler settings across different application types and project structures.

The most important practices include:

* Sharing common settings through `extends`.
* Using environment-specific configurations.
* Separating development, testing, and production settings.
* Configuring module behavior according to the runtime.
* Using Project References for large codebases.
* Inspecting resolved settings when debugging.

A well-structured TypeScript configuration makes projects easier to maintain, scale, and build consistently.
