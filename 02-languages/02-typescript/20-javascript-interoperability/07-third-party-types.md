# Third-Party Types in TypeScript

Third-party types are TypeScript type definitions provided for external JavaScript or TypeScript libraries.

They describe a library's public API, including:

* Functions and their parameters.
* Return types.
* Classes and their methods.
* Interfaces and type aliases.
* Configuration objects.
* Events and callbacks.
* Module exports.

These definitions allow TypeScript to understand external packages and provide type checking, autocomplete, and IntelliSense.

For example, when using a JavaScript library:

```javascript
import axios from "axios";

const response = await axios.get("/users");
```

Without type information, TypeScript may not understand the library's API.

With proper type definitions, TypeScript can provide accurate information about `axios.get()`, its configuration, and its response.

# 🗁 Why Third-Party Types Are Important

Third-party types provide several benefits:

* Enable type checking when using external libraries.
* Improve editor autocomplete and IntelliSense.
* Reduce mistakes when calling library APIs.
* Document library interfaces and configuration options.
* Improve refactoring and maintainability.
* Help integrate JavaScript libraries into TypeScript applications.

# 🗁 Where Third-Party Types Come From

Third-party type definitions generally come from three sources.

**Built-in TypeScript declarations**

Some libraries are written in TypeScript and include their own `.d.ts` files.

**DefinitelyTyped**

The DefinitelyTyped repository provides community-maintained type definitions for many JavaScript libraries.

Its definitions are commonly distributed through npm packages using the `@types/` naming convention.

**Custom declaration files**

When official or community definitions are unavailable, you can create your own `.d.ts` files.

# 🗁 Understanding the @types Package Convention

The `@types` namespace is commonly used for packages containing TypeScript declarations.

For example:

| JavaScript package | Type definitions package |
| --- | --- |
| `express` | `@types/express` |
| `lodash` | `@types/lodash` |
| `node` | `@types/node` |
| `jest` | `@types/jest` |
| `cors` | `@types/cors` |
| `jsonwebtoken` | `@types/jsonwebtoken` |

The type package usually describes the API of its corresponding JavaScript package.

# 🗁 Installing Third-Party Types

Install the JavaScript package first:

```bash
npm install express
```

Then install its type definitions:

```bash
npm install -D @types/express
```

For a Node.js application, you may also need Node.js declarations:

```bash
npm install -D @types/node
```

The `-D` flag installs packages as development dependencies.

Type declarations are generally needed during development and compilation rather than as runtime dependencies.

# 🗁 Using Built-In Type Definitions

Many modern libraries include their own TypeScript declarations.

For example, install Axios:

```bash
npm install axios
```

Import and use it:

```typescript
import axios from "axios";

interface User {
  id: number;
  name: string;
  email: string;
}

async function getUser(id: number): Promise<User> {
  const response = await axios.get<User>(`/users/${id}`);

  return response.data;
}
```

Axios provides its own type definitions, so installing `@types/axios` is generally unnecessary.

# 🗁 Using @types/node

Node.js provides runtime APIs such as:

* `process`
* `Buffer`
* `NodeJS`
* `__dirname`
* `setImmediate`

Install the corresponding type definitions:

```bash
npm install -D @types/node
```

Example:

```typescript
import { readFile } from "node:fs/promises";

const environment = process.env.NODE_ENV;

async function readConfig(): Promise<string> {
  return readFile("./config.json", "utf8");
}

console.log(environment);
```

TypeScript can now understand Node.js APIs through the installed declarations.

# 🗁 Using @types/express

Express is a popular Node.js web framework.

Install Express and its declarations:

```bash
npm install express

npm install -D @types/express @types/node
```

Example:

```typescript
import express, {
  type Request,
  type Response,
} from "express";

const app = express();

app.get(
  "/users",
  (req: Request, res: Response) => {
    res.json({
      message: "Users retrieved successfully",
    });
  }
);

app.listen(3000);
```

The Express type definitions provide types for requests, responses, middleware, application instances, and other framework APIs.

# 🗁 Finding Third-Party Type Definitions

When a library does not include its own types, search for its corresponding `@types` package.

For example:

```bash
npm view @types/lodash version
```

Install it:

```bash
npm install -D @types/lodash
```

You can also search npm:

```bash
npm search @types/library-name
```

Before installing a type package, check whether the library already includes declarations.

# 🗁 How TypeScript Resolves Third-Party Types

TypeScript searches for declaration files when resolving imports.

For example:

```typescript
import express from "express";
```

TypeScript looks for type information associated with the `express` package.

Common locations include:

```text
node_modules/
└── express/
    ├── package.json
    └── ...

node_modules/
└── @types/
    └── express/
        └── index.d.ts
```

A package may also provide declarations through its own package metadata.

For example:

```json
{
  "name": "example-library",
  "types": "./dist/index.d.ts"
}
```

The `types` or `typings` field identifies the package's main declaration file.

Modern packages can also expose declarations through the `exports` field.

# 🗁 Configuring Type Definitions with tsconfig.json

TypeScript provides compiler options for controlling how type definitions are discovered.

Example:

```json
{
  "compilerOptions": {
    "strict": true,
    "module": "NodeNext",
    "moduleResolution": "NodeNext",
    "types": [
      "node"
    ]
  }
}
```

The `types` option explicitly controls which visible `@types` packages are included in the global scope.

Important distinction:

* `types` controls automatic inclusion of visible type packages in the global scope.
* It does not prevent normal module imports from resolving their own declaration files.

For example, importing a typed package can still work even when that package is not listed in `types`.

# 🗁 Understanding typeRoots

The `typeRoots` option specifies directories where TypeScript searches for automatically included type packages.

Example:

```json
{
  "compilerOptions": {
    "typeRoots": [
      "./node_modules/@types",
      "./custom-types"
    ]
  }
}
```

With this configuration, TypeScript searches the specified directories for type packages.

Important: specifying `typeRoots` replaces the default type-root locations. Include the relevant `node_modules/@types` directory if you still need it.

# 🗁 What Happens When Types Are Missing?

Suppose you install a JavaScript library:

```bash
npm install some-library
```

But TypeScript cannot find its declarations.

You may see an error similar to:

```text
Could not find a declaration file for module 'some-library'.
```

There are several ways to solve this.

**Install the official type package**

```bash
npm install -D @types/some-library
```

**Check whether the library already includes declarations**

Inspect its `package.json` for fields such as `types`, `typings`, or type-related entries in `exports`.

**Create a custom declaration file**

```typescript
// types/some-library.d.ts

declare module "some-library" {
  export function initialize(): void;

  export function getVersion(): string;
}
```

**Use a temporary minimal declaration**

If you only need to acknowledge the module temporarily:

```typescript
declare module "some-library";
```

This allows TypeScript to recognize the module, but its exports effectively have no useful type information.

Prefer accurate declarations instead of relying on this approach for long-term development.

# 🗁 Handling Packages with Incomplete Types

Sometimes a library provides declarations, but certain APIs are missing or incorrectly typed.

You can extend its declarations using module augmentation.

For example:

```typescript
import "some-library";

declare module "some-library" {
  interface User {
    role?: string;
  }
}
```

This example assumes the package exports an augmentable `User` interface.

Module augmentation extends existing declarations without replacing the library's implementation.

# 🗁 Third-Party Types and Version Compatibility

Type definitions must be compatible with the JavaScript library version.

For example:

```text
express@5
@types/express@5
```

Using incompatible versions can cause:

* Missing properties.
* Incorrect method signatures.
* Unexpected compiler errors.
* Type mismatches.
* Incorrect assumptions about runtime behavior.

Check the library's supported type-definition versions before installing or upgrading packages.

# 🗁 Managing Type Dependencies

A typical TypeScript project may have dependencies like:

```json
{
  "dependencies": {
    "axios": "^1.0.0",
    "express": "^5.0.0"
  },
  "devDependencies": {
    "@types/express": "^5.0.0",
    "@types/node": "^22.0.0",
    "typescript": "^5.0.0"
  }
}
```

The versions above are illustrative. Use versions compatible with your actual dependencies and TypeScript version.

General guidelines:

* Keep type packages compatible with their corresponding libraries.
* Install development-only type packages in `devDependencies`.
* Avoid installing redundant `@types` packages.
* Commit the lockfile to maintain reproducible installations.
* Review type-definition changes when upgrading dependencies.

# 🗁 Best Practices

* Check whether a library already includes TypeScript declarations.
* Prefer official type definitions over custom declarations.
* Use DefinitelyTyped packages when official declarations are unavailable.
* Keep declaration versions compatible with library versions.
* Avoid unnecessary use of `any`.
* Use custom `.d.ts` files only when needed.
* Prefer module augmentation for extending existing declarations.
* Keep `tsconfig.json` type resolution settings intentional.
* Review declaration changes when updating dependencies.
* Verify external data at runtime instead of trusting static types alone.

# 🗁 Common Mistakes

**Installing redundant @types packages**

Some libraries already include declarations. Installing another type package can be unnecessary or cause conflicts.

**Using incompatible versions**

A type package designed for a different major library version may describe APIs incorrectly.

**Misconfiguring typeRoots**

Adding a custom `typeRoots` directory without including the default `node_modules/@types` directory can make existing type packages undiscoverable.

**Assuming TypeScript types validate runtime data**

Type declarations provide compile-time checks. They do not automatically validate API responses or other external input.

**Using broad declarations unnecessarily**

Declaring an entire module as `any` removes useful type checking.

# 🗁 Key Takeaways

* Third-party types describe external library APIs for TypeScript.
* Libraries can include their own declarations or rely on external `@types` packages.
* DefinitelyTyped is a major community source of TypeScript definitions.
* Install missing type packages through npm.
* Use `types` and `typeRoots` carefully when configuring declaration discovery.
* Custom `.d.ts` files can describe libraries without available definitions.
* Module augmentation can extend existing library types.
* Keep type definitions compatible with the actual library version.
