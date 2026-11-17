# Module Resolution

Module resolution is the process TypeScript uses to locate and load modules referenced through `import` and `export` statements.

When a TypeScript file imports another module, the compiler must determine which file or package the import refers to.

For example:

```typescript
import { User } from "./models/user";
```

TypeScript resolves the module specifier `"./models/user"` to the appropriate source file.

Module resolution is important because modern applications use many files, directories, packages, and external dependencies.

# 🗁 What Is Module Resolution?

Module resolution determines how TypeScript finds modules referenced in source code.

Consider this project:

```text
src/
├── app.ts
└── models/
    └── user.ts
```

`models/user.ts`

```typescript
export interface User {
  id: number;
  name: string;
}
```

`app.ts`

```typescript
import type { User } from "./models/user";

const user: User = {
  id: 101,
  name: "Alex",
};
```

TypeScript resolves the relative module path to `src/models/user.ts`.

# 🗁 Module Specifiers

A module specifier is the string used in an import or export statement to identify another module.

There are several common forms.

**Relative module specifier**

```typescript
import { User } from "./models/user";
```

Relative paths begin with:

* `./` for the current directory.
* `../` for the parent directory.

**Package module specifier**

```typescript
import express from "express";
```

Package imports usually refer to dependencies installed in `node_modules` or packages resolved through the configured module system.

**Absolute or aliased module specifier**

```typescript
import { User } from "@/models/user";
```

This can refer to a project-defined path alias configured through TypeScript and supported by the runtime or bundler.

# 🗁 Relative Module Resolution

Relative imports locate modules based on the directory of the importing file.

Example structure:

```text
src/
├── app.ts
├── utils/
│   └── format.ts
└── models/
    └── user.ts
```

Importing a module from the same directory:

```typescript
import { formatDate } from "./utils/format";
```

Importing from a parent directory:

```typescript
import { User } from "../models/user";
```

Relative paths are useful for keeping dependencies explicit and understandable.

# 🗁 Extension Resolution

TypeScript can resolve certain file extensions when an import does not specify the extension.

For example:

```typescript
import { User } from "./user";
```

Depending on the module resolution strategy and project configuration, TypeScript may search for compatible files such as:

* `user.ts`
* `user.tsx`
* `user.d.ts`
* `user.js`
* `user.jsx`

The exact behavior depends on the selected module resolution mode and options such as `allowImportingTsExtensions`.

For example, an import can explicitly include a TypeScript extension in supported configurations:

```typescript
import { User } from "./user.ts";
```

This is commonly used with configurations that preserve TypeScript extensions for a runtime or tool that understands them.

# 🗁 Directory Module Resolution

A module specifier can refer to a directory.

Example structure:

```text
src/
└── users/
    ├── index.ts
    ├── user.model.ts
    └── user.service.ts
```

Importing:

```typescript
import { getUsers } from "./users";
```

Under compatible resolution strategies, TypeScript can resolve the directory through its `index.ts` file.

The `index.ts` file might contain:

```typescript
export { getUsers } from "./user.service";

export type { User } from "./user.model";
```

Directory-based imports are commonly used with barrel files.

Modern Node.js ESM resolution has stricter rules for directory imports, so do not assume that every runtime or bundler resolves them identically.

# 🗁 Package Module Resolution

When importing an external package, TypeScript searches for the package's type declarations and runtime entry points.

Example:

```typescript
import express from "express";
```

A package may provide type information through:

* A `types` or `typings` field in `package.json`.
* A `types` condition in its `exports` field.
* Declaration files such as `index.d.ts`.
* Compatible type packages such as `@types/express`.

Example package metadata:

```json
{
  "name": "my-library",
  "version": "1.0.0",
  "main": "./dist/index.js",
  "types": "./dist/index.d.ts"
}
```

Here:

* `main` identifies a traditional runtime entry point.
* `types` identifies the package's TypeScript declarations.

Modern packages may use the `exports` field instead of relying exclusively on `main`.

# 🗁 The `moduleResolution` Option

The `moduleResolution` compiler option controls how TypeScript resolves module specifiers.

Example:

```json
{
  "compilerOptions": {
    "moduleResolution": "bundler"
  }
}
```

Common module resolution strategies include:

| Strategy | Description |
| --- | --- |
| `classic` | Older TypeScript resolution strategy |
| `node10` | Legacy Node.js-style resolution |
| `node16` | Node.js ESM and CommonJS-aware resolution |
| `nodenext` | Tracks modern Node.js module resolution behavior |
| `bundler` | Resolution behavior designed for modern bundlers |

The appropriate strategy depends on the runtime and build system.

# 🗁 Classic Resolution

`classic` is the original TypeScript module resolution strategy.

It is primarily relevant to older TypeScript projects.

It does not model modern Node.js package resolution behavior.

Example:

```json
{
  "compilerOptions": {
    "moduleResolution": "classic"
  }
}
```

For modern Node.js and frontend applications, other strategies are generally more appropriate.

# 🗁 Node10 Resolution

`node10` represents the legacy Node.js resolution behavior associated with older CommonJS-based environments.

It supports traditional package resolution patterns, including:

* Searching `node_modules`.
* Resolving package entry points.
* Looking for declaration files.
* Resolving supported extensions.

Example:

```json
{
  "compilerOptions": {
    "module": "commonjs",
    "moduleResolution": "node10"
  }
}
```

This mode is useful when maintaining older projects, but it does not model modern Node.js ESM resolution rules.

# 🗁 Node16 Resolution

`node16` models Node.js module resolution with support for both ECMAScript modules and CommonJS modules.

It considers the module format and package metadata when resolving imports.

Example:

```json
{
  "compilerOptions": {
    "module": "Node16",
    "moduleResolution": "node16"
  }
}
```

Node.js uses information such as:

* The file extension.
* The nearest `package.json`.
* The package's `type` field.
* The import or require context.
* The package's `exports` field.

For example, a package can declare:

```json
{
  "type": "module"
}
```

This makes `.js` files in that package scope behave as ECMAScript modules in Node.js.

# 🗁 NodeNext Resolution

`nodenext` follows modern Node.js module resolution behavior.

It is designed for projects targeting Node.js and supports the evolving Node.js module system.

Example:

```json
{
  "compilerOptions": {
    "module": "NodeNext",
    "moduleResolution": "NodeNext"
  }
}
```

This mode is useful when developing:

* Node.js applications.
* Node.js libraries.
* Packages supporting ESM and CommonJS.
* Projects using modern package exports.

Unlike a fixed historical model, `nodenext` tracks modern Node.js behavior as TypeScript evolves.

# 🗁 Bundler Resolution

`bundler` is designed for applications that use modern bundlers.

Examples include:

* Vite.
* webpack.
* esbuild.
* Rollup.
* Parcel.

Configuration:

```json
{
  "compilerOptions": {
    "module": "ESNext",
    "moduleResolution": "bundler"
  }
}
```

Bundler resolution supports package features such as conditional exports while allowing extensionless relative imports commonly supported by bundlers.

Example:

```typescript
import { User } from "./models/user";
```

A bundler may resolve this without requiring the `.ts` or `.js` extension.

This mode is not intended to model native Node.js runtime resolution exactly.

# 🗁 Comparing Module Resolution Strategies

| Strategy | Typical Environment | Main Purpose |
| --- | --- | --- |
| `classic` | Legacy TypeScript projects | Historical resolution |
| `node10` | Legacy Node.js projects | Traditional CommonJS resolution |
| `node16` | Node.js | ESM and CommonJS-aware resolution |
| `nodenext` | Modern Node.js | Current Node.js behavior |
| `bundler` | Frontend and bundled applications | Bundler-compatible resolution |

Choose a strategy based on how the application actually loads modules at runtime.

# 🗁 The `module` Option and Module Resolution

The `module` and `moduleResolution` options are related but control different aspects of the module system.

**`module`**

Controls the module format TypeScript emits or models.

Examples:

```json
{
  "compilerOptions": {
    "module": "ESNext"
  }
}
```

```json
{
  "compilerOptions": {
    "module": "CommonJS"
  }
}
```

**`moduleResolution`**

Controls how TypeScript locates imported modules.

Example:

```json
{
  "compilerOptions": {
    "module": "ESNext",
    "moduleResolution": "bundler"
  }
}
```

The two settings should be selected together according to the intended runtime environment.

# 🗁 Package Exports

The `exports` field in `package.json` defines which package entry points are publicly accessible.

Example:

```json
{
  "name": "my-library",
  "type": "module",
  "exports": {
    ".": "./dist/index.js",
    "./utils": "./dist/utils.js"
  }
}
```

Consumers can import the root package:

```typescript
import { createUser } from "my-library";
```

They can also import the exposed utility entry point:

```typescript
import { formatDate } from "my-library/utils";
```

Unlisted internal package paths are generally not accessible through package-name imports when an `exports` map is defined.

# 🗁 Conditional Exports

A package can provide different entry points depending on the environment or resolution condition.

Example:

```json
{
  "name": "my-library",
  "exports": {
    ".": {
      "import": "./dist/index.js",
      "require": "./dist/index.cjs"
    }
  }
}
```

This allows:

* ESM consumers to use the `import` entry point.
* CommonJS consumers to use the `require` entry point.

TypeScript can also resolve type declarations through a `types` condition.

Example:

```json
{
  "name": "my-library",
  "exports": {
    ".": {
      "types": "./dist/index.d.ts",
      "import": "./dist/index.js",
      "require": "./dist/index.cjs"
    }
  }
}
```

For packages with separate ESM and CommonJS declarations, declaration files should accurately reflect the corresponding module formats.

# 🗁 Package `types` and `typings`

A package can use the `types` field to identify its declaration entry point.

Example:

```json
{
  "name": "my-library",
  "version": "1.0.0",
  "main": "./dist/index.js",
  "types": "./dist/index.d.ts"
}
```

The `typings` field is an alternative name for `types`.

The `types` field is commonly preferred.

# 🗁 The `typesVersions` Field

The `typesVersions` field allows a package to provide different type declaration files for different TypeScript compiler version ranges.

Example:

```json
{
  "types": "./types/index.d.ts",
  "typesVersions": {
    ">=5.0": {
      "*": [
        "types/modern/*"
      ]
    },
    ">=4.0": {
      "*": [
        "types/legacy/*"
      ]
    }
  }
}
```

This is useful when maintaining libraries that support multiple TypeScript versions and need different declaration structures.

When a package uses conditional exports, TypeScript's handling of declaration entry points may depend on the package's `exports` configuration and selected resolution mode.

# 🗁 The `resolvePackageJsonExports` Option

This option controls whether TypeScript consults package `exports` fields during package resolution.

```json
{
  "compilerOptions": {
    "moduleResolution": "bundler",
    "resolvePackageJsonExports": true
  }
}
```

It is enabled by default for `node16`, `nodenext`, and `bundler` resolution modes.

It can be useful when diagnosing package-resolution behavior.

# 🗁 The `resolvePackageJsonImports` Option

The `imports` field in `package.json` defines private package import aliases.

Example:

```json
{
  "type": "module",
  "imports": {
    "#utils/*": "./src/utils/*.js"
  }
}
```

A module inside the package can use:

```typescript
import { formatDate } from "#utils/date.js";
```

TypeScript supports resolving package import mappings in compatible module resolution modes.

The `resolvePackageJsonImports` option controls this behavior and is enabled by default in `node16`, `nodenext`, and `bundler` modes.

# 🗁 The `baseUrl` Option

The `baseUrl` option establishes a base directory for non-relative module imports.

Example project:

```text
project/
├── tsconfig.json
└── src/
    ├── app.ts
    └── models/
        └── user.ts
```

Configuration:

```json
{
  "compilerOptions": {
    "baseUrl": "./src"
  }
}
```

Now the application can use:

```typescript
import { User } from "models/user";
```

The import is resolved relative to the configured base directory, subject to module resolution rules.

Since TypeScript 6.0, `baseUrl` is deprecated. Modern projects should generally prefer `paths` aliases without `baseUrl` when they need custom import mappings.

# 🗁 The `paths` Option

The `paths` option allows you to define custom module path mappings.

Example structure:

```text
src/
├── app.ts
├── components/
│   └── button.ts
└── utils/
    └── format.ts
```

Configuration:

```json
{
  "compilerOptions": {
    "paths": {
      "@components/*": [
        "./src/components/*"
      ],
      "@utils/*": [
        "./src/utils/*"
      ]
    }
  }
}
```

Imports:

```typescript
import { Button } from "@components/button";

import { formatDate } from "@utils/format";
```

Important: TypeScript's `paths` option does not rewrite import paths in emitted JavaScript.

The runtime, bundler, or another tool must understand the same aliases.

# 🗁 The `rootDirs` Option

The `rootDirs` option allows TypeScript to treat multiple directories as a single virtual root during module resolution.

Example structure:

```text
project/
├── src/
│   └── index.ts
└── generated/
    └── templates/
        └── email.ts
```

Configuration:

```json
{
  "compilerOptions": {
    "rootDirs": [
      "src",
      "generated"
    ]
  }
}
```

This is useful when source files and generated files are organized in separate physical directories but need to resolve relative imports through a shared virtual structure.

It affects TypeScript's resolution behavior and does not merge the directories on disk.

# 🗁 The `moduleSuffixes` Option

The `moduleSuffixes` option allows TypeScript to search for files with additional suffixes during module resolution.

Example:

```json
{
  "compilerOptions": {
    "moduleSuffixes": [
      ".ios",
      ".native",
      ""
    ]
  }
}
```

Given:

```typescript
import { PlatformService } from "./platform-service";
```

TypeScript can search for candidates such as:

```text
platform-service.ios.ts
platform-service.native.ts
platform-service.ts
```

This is useful in projects targeting multiple platforms.

The empty string at the end allows TypeScript to fall back to the unsuffixed filename.

# 🗁 The `customConditions` Option

The `customConditions` option adds custom conditions to package resolution.

Example package metadata:

```json
{
  "exports": {
    ".": {
      "development": "./dist/development.js",
      "default": "./dist/index.js"
    }
  }
}
```

Configuration:

```json
{
  "compilerOptions": {
    "module": "ESNext",
    "moduleResolution": "bundler",
    "customConditions": [
      "development"
    ]
  }
}
```

TypeScript can consider the `development` condition while resolving package exports.

The runtime or bundler must use compatible conditions to ensure the application loads the expected implementation.

# 🗁 The `allowImportingTsExtensions` Option

This option allows TypeScript source imports to include TypeScript-specific extensions.

Example:

```typescript
import { User } from "./user.ts";
```

Configuration:

```json
{
  "compilerOptions": {
    "allowImportingTsExtensions": true,
    "noEmit": true
  }
}
```

It is useful with tools that execute or bundle TypeScript files directly.

The option is restricted to configurations where TypeScript does not emit JavaScript with unresolved TypeScript extension paths, or where the appropriate rewriting behavior is enabled.

# 🗁 The `rewriteRelativeImportExtensions` Option

This option allows TypeScript to rewrite relative TypeScript extension imports into JavaScript extensions during emission.

Example:

```typescript
import { User } from "./user.ts";
```

With an appropriate emitting configuration, TypeScript can rewrite the emitted import to:

```javascript
import { User } from "./user.js";
```

Configuration:

```json
{
  "compilerOptions": {
    "target": "ES2022",
    "module": "NodeNext",
    "rewriteRelativeImportExtensions": true
  }
}
```

This is useful for projects that write explicit TypeScript extensions in source files but execute the emitted JavaScript.

# 🗁 Debugging Module Resolution

Sometimes TypeScript cannot find an imported module even though the file exists.

The `traceResolution` option provides detailed information about the compiler's resolution process.

Example:

```bash
tsc --traceResolution
```

You can also enable it in `tsconfig.json`:

```json
{
  "compilerOptions": {
    "traceResolution": true
  }
}
```

The output shows:

* Which module specifier is being resolved.
* Which directories TypeScript searches.
* Which file extensions are considered.
* Which package metadata is examined.
* Why a particular file was selected or rejected.

This is one of the most useful tools for diagnosing module resolution problems.

# 🗁 Common Module Resolution Errors

**Cannot find module**

```typescript
import { User } from "./models/user";
```

Possible causes:

* Incorrect relative path.
* Missing source file.
* Incorrect filename capitalization.
* Missing package dependency.
* Incompatible module resolution configuration.

**Package subpath is not exported**

```typescript
import { InternalHelper } from "my-library/internal/helper";
```

The package may define an `exports` map that does not expose this subpath.

Use an officially exported entry point.

**Runtime cannot resolve an alias**

```typescript
import { User } from "@models/user";
```

TypeScript may understand the alias through `paths`, but Node.js may not.

Configure the runtime or bundler to support the same mapping.

**Missing declaration files**

A JavaScript package may not provide TypeScript declarations.

Possible solutions include:

* Installing the package's corresponding `@types` package.
* Creating an appropriate declaration file.
* Using a package that provides built-in types.

**Extension mismatch**

Node.js ESM may require explicit JavaScript extensions in relative imports.

For example:

```typescript
import { User } from "./user.js";
```

TypeScript can resolve this to the corresponding TypeScript source file in compatible Node.js module resolution modes.

# 🗁 Choosing the Right Module Resolution Strategy

For a modern frontend application using Vite:

```json
{
  "compilerOptions": {
    "target": "ES2022",
    "module": "ESNext",
    "moduleResolution": "bundler",
    "strict": true,
    "noEmit": true
  }
}
```

For a modern Node.js application:

```json
{
  "compilerOptions": {
    "target": "ES2022",
    "module": "NodeNext",
    "moduleResolution": "NodeNext",
    "strict": true
  }
}
```

For an older CommonJS project:

```json
{
  "compilerOptions": {
    "target": "ES2020",
    "module": "CommonJS",
    "moduleResolution": "node10",
    "strict": true
  }
}
```

These are starting points, not universal configurations. Always align TypeScript's settings with the actual runtime and build pipeline.

# 🗁 Best Practices

* Choose module resolution based on the runtime environment.
* Prefer `bundler` for applications managed by modern bundlers.
* Prefer `NodeNext` or `Node16` for Node.js projects.
* Avoid `classic` for new applications.
* Keep TypeScript path aliases synchronized with bundler or runtime configuration.
* Understand package `exports` before importing package internals.
* Use explicit file extensions where required by the runtime.
* Use `traceResolution` when diagnosing unresolved modules.
* Avoid relying on undocumented package entry points.
* Keep `module` and `moduleResolution` configurations compatible.

# 🗁 Summary

Module resolution determines how TypeScript locates imported modules.

Key concepts:

* Module specifiers identify modules.
* Relative imports resolve from the importing file.
* Package imports resolve through package metadata and dependency locations.
* `moduleResolution` controls the resolution strategy.
* `node16` and `nodenext` model modern Node.js resolution.
* `bundler` supports modern bundler-based applications.
* `package.json` fields such as `exports`, `imports`, and `types` influence resolution.
* `paths` defines custom module mappings.
* `moduleSuffixes` supports platform-specific module variants.
* `traceResolution` helps debug resolution problems.

Understanding module resolution is essential for managing dependencies, configuring TypeScript projects, and building applications that work consistently across development and production environments.
