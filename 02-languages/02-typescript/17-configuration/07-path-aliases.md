# TypeScript Path Aliases

Path aliases allow developers to define custom import paths for modules instead of repeatedly writing long relative paths.

They make imports cleaner, improve code organization, and simplify navigation in large TypeScript projects.

TypeScript supports path aliases through the `paths` and `baseUrl` compiler options.

# 🗁 What Are Path Aliases?

Consider a project with deeply nested directories:

```text
src/
├── app/
│   └── components/
│       └── Button.ts
├── services/
│   └── userService.ts
├── utils/
│   └── formatDate.ts
└── index.ts
```

Without aliases, importing a utility from a deeply nested file may require several `../` segments.

```typescript
import { formatDate } from "../../../utils/formatDate";
```

With a path alias, the same import can become:

```typescript
import { formatDate } from "@/utils/formatDate";
```

The alias `@/` represents a configured project directory.

# 🗁 Configuring Path Aliases

Path aliases are configured inside `compilerOptions` in `tsconfig.json`.

Example:

```json
{
  "compilerOptions": {
    "baseUrl": ".",
    "paths": {
      "@/*": ["src/*"]
    }
  }
}
```

This configuration defines:

* `@/` as a custom import prefix.
* `src/` as the corresponding directory.
* `@/utils/formatDate` as a reference to `src/utils/formatDate`.

The `paths` option tells TypeScript how to resolve matching import specifiers.

# 🗁 Understanding `baseUrl`

The `baseUrl` option establishes the base directory used to resolve non-relative module imports.

Example:

```json
{
  "compilerOptions": {
    "baseUrl": "./src"
  }
}
```

Now an import can reference a module relative to the configured base directory:

```typescript
import { formatDate } from "utils/formatDate";
```

TypeScript resolves this relative to `src/`.

In modern TypeScript versions, `baseUrl` is not required when using `paths`. Without it, path mapping targets are resolved relative to the directory containing the `tsconfig.json` file.

# 🗁 Understanding `paths`

The `paths` option defines custom mappings between module specifiers and file locations.

Example:

```json
{
  "compilerOptions": {
    "paths": {
      "@/*": ["src/*"],
      "@components/*": ["src/components/*"],
      "@services/*": ["src/services/*"],
      "@utils/*": ["src/utils/*"]
    }
  }
}
```

Each mapping contains:

* A pattern used in import statements.
* One or more target paths used for resolution.

For example:

```typescript
import { Button } from "@components/Button";
import { getUser } from "@services/userService";
import { formatDate } from "@utils/formatDate";
```

The compiler uses these mappings to locate the corresponding modules.

# 🗁 Wildcard Patterns

The `*` character represents a wildcard that can match part of a module path.

Example:

```json
{
  "compilerOptions": {
    "paths": {
      "@/*": ["src/*"]
    }
  }
}
```

This mapping supports imports such as:

```typescript
import { Button } from "@/components/Button";
import { getUser } from "@/services/userService";
import { formatDate } from "@/utils/formatDate";
```

The wildcard portion is substituted into the target path.

You can also define multiple target locations:

```json
{
  "compilerOptions": {
    "paths": {
      "@/*": [
        "src/*",
        "generated/*"
      ]
    }
  }
}
```

TypeScript checks the configured substitutions according to module resolution rules.

# 🗁 Organizing a Real Project

Consider this structure:

```text
project/
├── src/
│   ├── components/
│   │   └── Button.ts
│   ├── services/
│   │   └── authService.ts
│   ├── hooks/
│   │   └── useAuth.ts
│   ├── utils/
│   │   └── formatDate.ts
│   └── index.ts
└── tsconfig.json
```

A practical configuration:

```json
{
  "compilerOptions": {
    "target": "ES2022",
    "module": "ESNext",
    "moduleResolution": "Bundler",
    "strict": true,
    "baseUrl": ".",
    "paths": {
      "@/*": ["src/*"],
      "@components/*": ["src/components/*"],
      "@services/*": ["src/services/*"],
      "@hooks/*": ["src/hooks/*"],
      "@utils/*": ["src/utils/*"]
    }
  },
  "include": ["src"]
}
```

You can now use readable imports throughout the project:

```typescript
import { Button } from "@components/Button";
import { authenticate } from "@services/authService";
import { useAuth } from "@hooks/useAuth";
import { formatDate } from "@utils/formatDate";
```

# 🗁 Important: TypeScript Paths Do Not Rewrite Imports

One of the most important details about path aliases is that TypeScript's `paths` option does not rewrite import paths in emitted JavaScript.

For example:

```typescript
import { formatDate } from "@/utils/formatDate";
```

The generated JavaScript generally preserves the same import:

```javascript
import { formatDate } from "@/utils/formatDate";
```

TypeScript understands the alias during module resolution, but the JavaScript runtime may not.

If the runtime or build tool does not understand the alias, the import can fail.

You must configure the runtime or build tool to recognize the same alias.

# 🗁 Configuring Aliases in Vite

For a Vite project, configure the alias in `vite.config.ts`.

```typescript
import { defineConfig } from "vite";
import path from "node:path";

export default defineConfig({
  resolve: {
    alias: {
      "@": path.resolve(__dirname, "src")
    }
  }
});
```

Then configure TypeScript:

```json
{
  "compilerOptions": {
    "baseUrl": ".",
    "paths": {
      "@/*": ["src/*"]
    }
  }
}
```

Both configurations must describe the same mapping.

# 🗁 Configuring Aliases in Webpack

Webpack supports aliases through its `resolve.alias` configuration.

Example:

```javascript
const path = require("node:path");

module.exports = {
  resolve: {
    alias: {
      "@": path.resolve(__dirname, "src")
    },
    extensions: [".ts", ".tsx", ".js"]
  }
};
```

TypeScript should use a matching `paths` configuration.

# 🗁 Path Aliases in Node.js

Node.js does not automatically understand arbitrary TypeScript `paths` mappings.

For a Node.js application, you must ensure that the runtime or build process supports the configured aliases.

Possible approaches include:

* Using a bundler that resolves aliases.
* Using a runtime with explicit alias support.
* Using Node.js package `imports` mappings with `#`-prefixed specifiers.
* Using relative imports where appropriate.

For example, a Node.js package can define internal import mappings in `package.json`:

```json
{
  "type": "module",
  "imports": {
    "#utils/*": "./dist/utils/*.js"
  }
}
```

Then use:

```typescript
import { formatDate } from "#utils/formatDate";
```

With an appropriate Node.js-aware TypeScript configuration, these package mappings can be resolved according to Node.js rules.

# 🗁 Common Mistakes

**Configuring aliases only in `tsconfig.json`**

TypeScript may recognize the import, but the runtime or bundler may fail to resolve it.

**Using incorrect target paths**

```json
{
  "compilerOptions": {
    "paths": {
      "@/*": ["src/*"]
    }
  }
}
```

If the actual source directory is named `app`, this mapping will not resolve the intended files.

**Using inconsistent configurations**

The TypeScript compiler and bundler must agree on the alias location.

**Assuming `paths` changes generated JavaScript**

It does not rewrite import specifiers.

**Creating too many aliases**

Excessive aliases can make project navigation and refactoring harder. Prefer a small, consistent set of meaningful aliases.

# 🗁 Best Practices

* Use a consistent alias convention across the project.
* Keep aliases short and meaningful.
* Configure aliases in both TypeScript and the relevant runtime or bundler.
* Prefer project-root or source-root mappings that are easy to understand.
* Avoid aliases for modules that are already easy to import relatively.
* Verify that production builds resolve aliases correctly.
* Keep package-level imports separate from application-specific aliases.

# 🗁 Summary

Path aliases provide shorter and more maintainable import paths.

The main configuration options are:

| Option | Purpose |
| --- | --- |
| `baseUrl` | Establishes a base directory for non-relative module resolution |
| `paths` | Defines custom module path mappings |

Remember that TypeScript uses these mappings for module resolution but does not automatically rewrite emitted imports. Your bundler or runtime must also support the aliases.
