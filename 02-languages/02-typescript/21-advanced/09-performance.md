# TypeScript Performance

TypeScript performance refers to how efficiently the TypeScript compiler analyzes, checks, and processes a project's source code.

As a project grows, TypeScript may need to analyze thousands of files, resolve complex dependencies, instantiate generic types, and perform extensive type compatibility checks.

Poorly designed type definitions and project configurations can increase:

* Compilation time.
* Type-checking time.
* Memory consumption.
* Editor responsiveness issues.
* Build pipeline duration.

Understanding TypeScript performance helps maintain fast development workflows, especially in large applications and monorepositories.

# 🗁 How TypeScript Compilation Works

The TypeScript compiler performs several major operations:

* **Parsing:** Reads source files and creates syntax trees.
* **Binding:** Connects declarations and establishes symbols.
* **Type checking:** Analyzes types and verifies compatibility.
* **Transformation:** Converts TypeScript syntax into the configured JavaScript output.
* **Emission:** Generates JavaScript, declaration files, and source maps when configured.

Type checking can become expensive when the compiler encounters deeply nested types, complex conditional types, large unions, or extensive generic relationships.

Understanding these stages helps identify where performance problems originate.

# 🗁 Measuring Compiler Performance

Before optimizing a TypeScript project, measure its current performance.

TypeScript provides several useful compiler options.

**Generate compiler performance diagnostics:**

```bash
tsc --extendedDiagnostics
```

This reports information such as:

* Files processed.
* Lines of code.
* Number of types.
* Number of type instantiations.
* Memory usage.
* Parse time.
* Bind time.
* Check time.
* Emit time.

Example:

```text
Files:                         250
Lines of TypeScript:         18000
Types:                        45000
Instantiations:               12000
Memory used:                150 MB
Parse time:                  0.80s
Bind time:                   0.30s
Check time:                  2.40s
Emit time:                   0.90s
Total time:                  4.40s
```

The values above are illustrative, not expected performance targets.

**Generate a compiler trace:**

```bash
tsc --generateTrace trace
```

This creates tracing information that can be analyzed using TypeScript's performance tooling.

For example:

```bash
npm install --global @typescript/analyze-trace

analyze-trace trace
```

A trace helps identify expensive compiler operations and types.

# 🗁 Reduce Unnecessary Type Complexity

Complex types can require significant work from the compiler.

For example, deeply nested conditional and mapped types can become expensive.

```typescript
type DeepTransform<T> =
  T extends object
    ? {
        [K in keyof T]: DeepTransform<T[K]>;
      }
    : T;
```

Recursive types can be useful, but unnecessary recursion should be avoided.

Prefer simpler types when they provide the same functionality.

```typescript
interface User {
  id: number;
  name: string;
  email: string;
}
```

Keep type definitions readable and avoid introducing complex abstractions without a practical reason.

# 🗁 Avoid Unnecessary Large Union Types

Large unions can increase the number of comparisons required during type checking.

```typescript
type Status =
  | "pending"
  | "approved"
  | "rejected"
  | "cancelled"
  | "processing"
  | "completed"
  | "failed";
```

This union is reasonable because it represents a small, meaningful set of values.

However, generating extremely large unions through template literal types or combinations of other unions can become expensive.

```typescript
type Method = "GET" | "POST" | "PUT" | "DELETE";

type Version = "v1" | "v2" | "v3";

type Endpoint = `/${Version}/${Method}`;
```

The resulting type contains every possible combination.

When unions grow significantly, consider using simpler string types, runtime validation, or more focused type definitions.

# 🗁 Prefer Interfaces for Object Type Relationships

Interfaces can be easier for TypeScript to compare in certain object-oriented type relationships because they support declaration merging and have identifiable declarations.

For example:

```typescript
interface User {
  id: number;
  name: string;
}

interface Admin extends User {
  permissions: string[];
}
```

This expresses the relationship directly.

An equivalent intersection can also work:

```typescript
type Admin = User & {
  permissions: string[];
};
```

There is no universal performance rule that interfaces are always faster than type aliases.

Choose the clearest representation and measure actual performance before changing type definitions.

# 🗁 Avoid Repeated Complex Type Computations

Repeatedly expressing the same complex type can make code harder to maintain and may increase compiler work.

Instead, extract reusable type definitions.

Less reusable:

```typescript
type UserResponse = {
  data: {
    id: number;
    name: string;
    roles: string[];
  };
};

type AdminResponse = {
  data: {
    id: number;
    name: string;
    roles: string[];
    permissions: string[];
  };
};
```

More reusable:

```typescript
interface User {
  id: number;
  name: string;
  roles: string[];
}

type UserResponse = {
  data: User;
};

type AdminResponse = {
  data: User & {
    permissions: string[];
  };
};
```

This improves consistency and makes shared structures easier to maintain.

# 🗁 Use Type Annotations Strategically

TypeScript can infer many types automatically.

```typescript
const user = {
  id: 1,
  name: "Alice",
};
```

Adding unnecessary annotations does not automatically improve compiler performance.

However, explicit annotations at important boundaries can reduce inference complexity and make APIs easier to understand.

```typescript
interface User {
  id: number;
  name: string;
}

function getUser(): User {
  return {
    id: 1,
    name: "Alice",
  };
}
```

Useful places for explicit types include:

* Public function signatures.
* Exported library APIs.
* Complex callback boundaries.
* Shared application contracts.
* Recursive or highly generic code.

Avoid annotating every local variable without a clear reason.

# 🗁 Use Project References

Project references help organize large TypeScript applications into smaller, independently configured projects.

Instead of compiling an entire monorepository as one large project, separate related applications and libraries.

Example structure:

```text
my-project/
├── packages/
│   ├── shared/
│   │   ├── src/
│   │   └── tsconfig.json
│   ├── api/
│   │   ├── src/
│   │   └── tsconfig.json
│   └── web/
│       ├── src/
│       └── tsconfig.json
├── tsconfig.json
└── package.json
```

Root configuration:

```json
{
  "files": [],
  "references": [
    {
      "path": "./packages/shared"
    },
    {
      "path": "./packages/api"
    },
    {
      "path": "./packages/web"
    }
  ]
}
```

A referenced project can use composite compilation:

```json
{
  "compilerOptions": {
    "composite": true,
    "declaration": true,
    "declarationMap": true,
    "outDir": "dist"
  },
  "include": ["src"]
}
```

Build the project reference graph:

```bash
tsc --build
```

Project references provide a structured way to reuse declaration outputs and avoid repeatedly treating every package as one compilation unit.

They are especially useful in large monorepositories.

# 🗁 Use Incremental Compilation

Incremental compilation allows TypeScript to reuse information from previous compilations.

Enable it through `tsconfig.json`:

```json
{
  "compilerOptions": {
    "incremental": true
  }
}
```

TypeScript stores build information in a `.tsbuildinfo` file.

Subsequent builds can reuse cached information when the project has not changed significantly.

You can specify a custom location:

```json
{
  "compilerOptions": {
    "incremental": true,
    "tsBuildInfoFile": "./.cache/typescript.tsbuildinfo"
  }
}
```

Incremental compilation is useful during development and repeated CI builds when the cache can be preserved.

# 🗁 Use skipLibCheck Carefully

The `skipLibCheck` option skips type checking of declaration files (`.d.ts`).

```json
{
  "compilerOptions": {
    "skipLibCheck": true
  }
}
```

This can improve compilation speed in projects with many dependencies.

However, it does not mean TypeScript ignores library types completely.

TypeScript still uses declaration information when checking application code. It simply skips checking the declaration files themselves for errors.

Considerations:

* It can reduce checking time.
* It may hide inconsistencies inside dependency declaration files.
* It does not replace proper dependency version management.
* It does not disable type checking of your application source files.

# 🗁 Control Included Files

TypeScript can become slower when it unnecessarily includes large numbers of files.

Use `include`, `exclude`, and `files` carefully.

Example:

```json
{
  "compilerOptions": {
    "strict": true
  },
  "include": [
    "src/**/*.ts",
    "src/**/*.tsx"
  ],
  "exclude": [
    "node_modules",
    "dist",
    "coverage",
    ".next"
  ]
}
```

Avoid including generated files, build outputs, test artifacts, or unrelated directories unless they are intentionally part of the project.

The `exclude` option affects file discovery through `include`; it does not prevent an excluded file from being included when another file imports it or when it is explicitly listed.

# 🗁 Optimize Module Resolution

Module resolution determines how TypeScript finds imported modules and their declarations.

Poorly organized imports and unnecessarily complex resolution configurations can make large projects harder to analyze.

Prefer consistent import paths:

```typescript
import { User } from "./types/user";
import { createUser } from "./services/user-service";
```

Use path aliases when they improve project organization:

```json
{
  "compilerOptions": {
    "baseUrl": ".",
    "paths": {
      "@/*": ["src/*"],
      "@services/*": ["src/services/*"]
    }
  }
}
```

Avoid excessively complex alias mappings and circular dependency structures.

Remember that TypeScript path aliases primarily affect type checking and module resolution. Your runtime or bundler must also understand the corresponding paths.

# 🗁 Avoid Unnecessary Circular Dependencies

Circular dependencies occur when modules depend on one another in a cycle.

Example:

```text
user.ts
  └── imports profile.ts

profile.ts
  └── imports user.ts
```

Circular dependencies can make project architecture harder to understand and may complicate module resolution and initialization.

Prefer clear dependency directions:

```text
types/
  └── shared-types.ts

user/
  └── user-service.ts

profile/
  └── profile-service.ts
```

Move shared types into independent modules when necessary.

Not every circular dependency causes a TypeScript performance problem, but reducing unnecessary cycles improves maintainability and module organization.

# 🗁 Improve Editor Performance

TypeScript performance also affects the development experience inside editors.

Common symptoms include:

* Slow autocomplete.
* Delayed error reporting.
* High memory consumption.
* Slow navigation to definitions.
* Unresponsive language services.

Useful practices:

* Keep project boundaries clear.
* Avoid loading unnecessary generated files.
* Reduce excessively complex generic types.
* Use project references for large codebases.
* Keep dependencies and TypeScript versions consistent.
* Avoid opening an unnecessarily large directory as one editor workspace.
* Use the TypeScript language service diagnostics when investigating editor-specific issues.

For VS Code, you can inspect TypeScript server logs through the command palette:

```text
TypeScript: Open TS Server Log
```

This can help investigate slow language-service operations.

# 🗁 Practical Example: Optimizing a Large Project

Consider a project with multiple packages:

```text
application/
├── packages/
│   ├── types/
│   ├── services/
│   ├── api/
│   └── frontend/
├── tsconfig.json
└── package.json
```

A practical optimization strategy:

**Establish project boundaries**

Separate packages according to their responsibilities.

**Create shared type contracts**

Avoid duplicating complex interfaces across packages.

**Enable incremental builds**

Reuse previous compilation information.

**Use project references**

Build related packages through a dependency graph.

**Measure compiler performance**

Run:

```bash
tsc --extendedDiagnostics
```

**Investigate expensive types**

Generate a trace:

```bash
tsc --generateTrace trace
```

**Remove unnecessary source files**

Review `include`, `exclude`, and dependency relationships.

**Measure again**

Compare the results to determine whether the changes actually improved performance.

# 🗁 Best Practices

* Measure performance before attempting optimization.
* Use `--extendedDiagnostics` to understand compiler workload.
* Use compiler traces to investigate expensive operations.
* Avoid unnecessary recursive and deeply nested types.
* Keep large union types manageable.
* Prefer clear and reusable type definitions.
* Use project references for large multi-package applications.
* Enable incremental compilation where repeated builds benefit from caching.
* Use `skipLibCheck` when appropriate, understanding its trade-offs.
* Keep included files and module dependencies under control.
* Investigate editor performance separately from build performance.
* Re-measure after every significant optimization.

# 🗁 Key Takeaways

* TypeScript performance depends on parsing, binding, type checking, and emission.
* Complex generic types and large unions can increase type-checking costs.
* Compiler diagnostics help identify performance bottlenecks.
* Project references help organize large applications into smaller compilation units.
* Incremental compilation reuses previous build information.
* `skipLibCheck` skips checking declaration files, not application source files.
* Proper file inclusion and module organization improve maintainability and can reduce unnecessary compiler work.
* Performance optimization should be driven by measurements rather than assumptions.
