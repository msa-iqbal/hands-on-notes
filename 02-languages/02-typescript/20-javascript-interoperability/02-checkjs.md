# checkJs in TypeScript

`checkJs` is a TypeScript compiler option that enables type checking in JavaScript files.

Normally, TypeScript can include JavaScript files in a project using the `allowJs` option, but it does not report all type errors in those files by default.

Enabling `checkJs` allows TypeScript to analyze JavaScript code and report type-related problems.

This makes it possible to improve JavaScript code quality without converting every `.js` file into a `.ts` file.

# 🗁 Why Use checkJs?

`checkJs` is useful when:

* Maintaining an existing JavaScript application.
* Gradually migrating JavaScript to TypeScript.
* Improving type safety without changing file extensions.
* Detecting incorrect function arguments.
* Identifying incompatible assignments.
* Improving editor diagnostics and IntelliSense.

It provides a practical migration path for projects that are not ready for a complete TypeScript conversion.

# 🗁 Enabling checkJs

The `checkJs` option is configured in `tsconfig.json`.

It requires `allowJs` to be enabled.

```json
{
  "compilerOptions": {
    "allowJs": true,
    "checkJs": true,
    "noEmit": true
  },
  "include": [
    "src/**/*.js"
  ]
}
```

Important options:

| Option | Description |
| --- | --- |
| `allowJs` | Includes JavaScript files in the TypeScript project |
| `checkJs` | Reports type errors in JavaScript files |
| `noEmit` | Prevents JavaScript output generation |

Run the TypeScript compiler:

```bash
npx tsc --noEmit
```

TypeScript analyzes the included JavaScript files and reports detected errors.

# 🗁 Understanding JavaScript Type Inference

TypeScript can infer types from JavaScript expressions.

Consider:

```javascript
// app.js

let username = "Alice";

username = "Bob";

// TypeScript reports an error.
username = 123;
```

TypeScript initially infers `username` as a string.

Assigning a number violates the inferred type.

This helps detect mistakes without adding explicit TypeScript annotations.

# 🗁 Checking Function Parameters

JavaScript functions do not normally declare parameter types.

TypeScript can infer parameter types from usage, default values, and JSDoc annotations.

Consider:

```javascript
// calculator.js

/**
 * @param {number} a
 * @param {number} b
 */
export function add(a, b) {
  return a + b;
}

add(10, 20);

// TypeScript reports an error.
add("10", 20);
```

The JSDoc annotations tell TypeScript that both parameters must be numbers.

Without sufficient information, TypeScript may treat JavaScript parameters as `any`.

# 🗁 Using JSDoc for Type Safety

JSDoc provides type information inside JavaScript files.

You can use it to describe:

* Function parameters.
* Return values.
* Variables.
* Object properties.
* Custom types.
* Generic functions.

Example:

```javascript
/**
 * @param {string} name
 * @returns {string}
 */
function greet(name) {
  return `Hello, ${name}`;
}

greet("Alice");

// TypeScript reports an error.
greet(123);
```

The file remains JavaScript, but TypeScript understands its declared types.

# 🗁 Checking Object Properties

TypeScript can detect invalid object property access when sufficient type information exists.

```javascript
/**
 * @typedef {Object} User
 * @property {number} id
 * @property {string} name
 */

/**
 * @param {User} user
 */
function displayUser(user) {
  console.log(user.name);

  // TypeScript reports an error.
  console.log(user.email);
}

displayUser({
  id: 1,
  name: "Alice",
});
```

The `User` type describes the expected object structure.

Accessing an undeclared property produces a diagnostic.

# 🗁 Using Type Imports in JavaScript

JavaScript files can use TypeScript types through JSDoc import expressions.

Consider a TypeScript definition:

```typescript
// types.ts

export interface User {
  id: number;
  name: string;
}
```

Use it in a JavaScript file:

```javascript
// user.js

/**
 * @param {import("./types.js").User} user
 */
export function displayUser(user) {
  console.log(user.name);
}
```

This allows JavaScript modules to reuse TypeScript type definitions without converting the JavaScript file.

The import path should follow the project's module resolution and runtime conventions.

# 🗁 checkJs with allowJs

These two compiler options serve different purposes.

| Configuration | Behavior |
| --- | --- |
| `allowJs: false` | JavaScript files are not included as project source files |
| `allowJs: true` | JavaScript files can participate in the project |
| `allowJs: true`, `checkJs: false` | JavaScript files are included without full JavaScript error checking |
| `allowJs: true`, `checkJs: true` | JavaScript files are included and checked for type errors |

`checkJs` depends on `allowJs`.

# 🗁 Using checkJs with TypeScript Files

A project can contain both JavaScript and TypeScript files.

Example structure:

```text
src/
├── app.ts
├── calculator.js
├── user.js
└── types.ts
```

Example configuration:

```json
{
  "compilerOptions": {
    "target": "ES2022",
    "module": "NodeNext",
    "moduleResolution": "NodeNext",
    "allowJs": true,
    "checkJs": true,
    "strict": true,
    "noEmit": true
  },
  "include": [
    "src/**/*"
  ]
}
```

TypeScript checks both JavaScript and TypeScript source files.

This is especially useful during gradual migration.

# 🗁 Suppressing Individual Errors

Sometimes JavaScript code contains an intentional pattern that TypeScript cannot infer correctly.

You can use TypeScript suppression directives in JavaScript files.

**@ts-expect-error**

```javascript
// @ts-expect-error - intentional invalid assignment
const value = 123;
```

This directive expects the following line to produce a TypeScript diagnostic. If no diagnostic occurs, TypeScript reports an unused directive.

**@ts-ignore**

```javascript
// @ts-ignore
const value = 123;
```

This suppresses the diagnostic on the following line without checking whether an error actually exists.

Prefer `@ts-expect-error` when suppressing a known and intentional type error.

# 🗁 checkJs in VS Code

Visual Studio Code uses TypeScript language services to provide JavaScript diagnostics.

With `checkJs` enabled, you can benefit from:

* Inline type errors.
* IntelliSense.
* Parameter information.
* Type information on hover.
* Improved code navigation.
* Refactoring assistance.

You can also enable checking for an individual JavaScript file using:

```javascript
// @ts-check
```

This is useful when you want to check selected files without enabling project-wide `checkJs`.

To disable checking for an individual file, use:

```javascript
// @ts-nocheck
```

These directives affect TypeScript checking for the file in which they appear.

# 🗁 Gradual Migration Strategy

A practical migration strategy for an existing JavaScript project:

* Enable `allowJs`.
* Start with `checkJs` disabled if the project contains many existing errors.
* Add JSDoc annotations to important functions and objects.
* Enable `checkJs` for selected files using `// @ts-check`.
* Fix errors incrementally.
* Enable project-wide `checkJs` when the codebase is ready.
* Convert JavaScript files to TypeScript gradually.
* Remove unnecessary JSDoc annotations when replacing them with native TypeScript types.

This approach improves type safety without requiring a large migration in a single step.

# 🗁 Best Practices

* Use `checkJs` to improve existing JavaScript projects incrementally.
* Add JSDoc annotations to important public functions.
* Prefer explicit JSDoc types over unnecessary `any`.
* Keep JavaScript and TypeScript module conventions consistent.
* Fix type errors rather than suppressing them unnecessarily.
* Use `// @ts-check` when enabling project-wide checking is not practical.
* Use `// @ts-expect-error` for intentional and documented exceptions.
* Include JavaScript checking in continuous integration.

# 🗁 Common Mistakes

**Enabling checkJs without allowJs**

`checkJs` requires `allowJs` to be enabled.

**Assuming every JavaScript parameter has an inferred type**

Parameters without sufficient information may be treated as `any`.

**Using JSDoc incorrectly**

Incorrect annotations can create misleading type information or compiler errors.

**Ignoring existing diagnostics**

Enabling checking on a large legacy project may reveal many pre-existing problems.

**Converting everything immediately**

Gradual migration is often safer and easier to maintain.

# 🗁 Key Takeaways

* `checkJs` enables TypeScript type checking in JavaScript files.
* It requires `allowJs: true`.
* JSDoc provides explicit type information in JavaScript.
* `// @ts-check` enables checking for an individual file.
* `// @ts-nocheck` disables checking for an individual file.
* JavaScript and TypeScript files can coexist in the same project.
* `checkJs` is particularly useful for gradual TypeScript migration.
* Type checking improves development feedback without changing JavaScript runtime behavior.
