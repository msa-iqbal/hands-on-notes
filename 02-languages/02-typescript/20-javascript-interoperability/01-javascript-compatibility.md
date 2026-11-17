# JavaScript Compatibility in TypeScript

TypeScript is designed to work with JavaScript rather than replace its runtime.

Every valid JavaScript program is also valid TypeScript, provided the file does not contain syntax or constructs that conflict with TypeScript's type system.

TypeScript extends JavaScript with features such as:

* Static type annotations.
* Interfaces and type aliases.
* Generics.
* Enums.
* Access modifiers.
* Compile-time type checking.

These additional features are used during development and are generally removed when TypeScript is compiled into JavaScript.

This allows TypeScript projects to use existing JavaScript libraries, frameworks, tools, and packages.

# 🗁 TypeScript as a JavaScript Superset

TypeScript is commonly described as a syntactic superset of JavaScript.

This means TypeScript supports JavaScript syntax while adding additional language features.

JavaScript:

```javascript
function greet(name) {
  return `Hello, ${name}`;
}

console.log(greet("Alice"));
```

Equivalent TypeScript:

```typescript
function greet(name: string): string {
  return `Hello, ${name}`;
}

console.log(greet("Alice"));
```

The TypeScript version adds compile-time type information without fundamentally changing the function's runtime behavior.

# 🗁 JavaScript Runs at Runtime

TypeScript does not introduce a separate runtime environment.

TypeScript source code is transformed into JavaScript, which is then executed by a JavaScript runtime such as:

* Web browsers.
* Node.js.
* Deno.
* Bun.

Example:

```typescript
const username: string = "Alice";

console.log(username);
```

Compiled JavaScript:

```javascript
const username = "Alice";

console.log(username);
```

The type annotation is removed during compilation.

# 🗁 Using JavaScript Libraries in TypeScript

TypeScript can use existing JavaScript packages from npm.

For example, install Lodash:

```bash
npm install lodash
```

Install its community-maintained type definitions:

```bash
npm install -D @types/lodash
```

Use it in TypeScript:

```typescript
import { chunk } from "lodash";

const numbers = [1, 2, 3, 4, 5, 6];

const result = chunk(numbers, 2);

console.log(result);
```

The `@types/lodash` package provides TypeScript declarations describing the JavaScript library's API.

Many modern JavaScript packages include their own TypeScript declarations, so a separate `@types` package is not always necessary.

# 🗁 JavaScript and TypeScript Files in One Project

TypeScript projects can contain both JavaScript and TypeScript files.

Example structure:

```text
my-project/
├── src/
│   ├── app.ts
│   ├── user.js
│   └── utils.ts
├── package.json
└── tsconfig.json
```

JavaScript file:

```javascript
// user.js

export function getUser() {
  return {
    id: 1,
    name: "Alice",
  };
}
```

TypeScript file:

```typescript
// app.ts

import { getUser } from "./user.js";

const user = getUser();

console.log(user.name);
```

TypeScript can import JavaScript modules when the project configuration permits it.

The `allowJs` compiler option controls whether JavaScript files are included in the TypeScript project.

# 🗁 Configuring JavaScript Compatibility

The following `tsconfig.json` configuration allows JavaScript and TypeScript files to coexist:

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

Important options:

| Option | Description |
| --- | --- |
| `allowJs` | Allows JavaScript files in the TypeScript project |
| `checkJs` | Reports type errors in included JavaScript files |
| `strict` | Enables strict type-checking options |
| `noEmit` | Prevents JavaScript output generation |
| `target` | Controls the JavaScript language version used for emitted output |
| `module` | Controls the module format of emitted JavaScript |
| `moduleResolution` | Controls how imported modules are resolved |

`checkJs` is useful when gradually introducing TypeScript into an existing JavaScript project.

# 🗁 TypeScript Can Infer JavaScript Types

TypeScript can infer types from JavaScript code.

Consider:

```javascript
// calculator.js

export function add(a, b) {
  return a + b;
}
```

With JavaScript checking enabled, TypeScript analyzes the function and infers types from available information.

However, JavaScript does not provide explicit TypeScript parameter annotations.

You can improve type information using JSDoc.

```javascript
/**
 * Adds two numbers.
 *
 * @param {number} a
 * @param {number} b
 * @returns {number}
 */
export function add(a, b) {
  return a + b;
}
```

TypeScript can use these annotations for type checking and editor support.

# 🗁 JavaScript Type Checking with JSDoc

JSDoc provides a way to describe types in ordinary JavaScript files.

Example:

```javascript
/**
 * @typedef {Object} User
 * @property {number} id
 * @property {string} name
 */

/**
 * @param {User} user
 * @returns {string}
 */
export function getUsername(user) {
  return user.name;
}
```

This provides type information without converting the file to TypeScript.

It is useful for projects that want stronger type checking while retaining JavaScript source files.

# 🗁 Gradual Migration from JavaScript to TypeScript

TypeScript supports gradual adoption.

You do not need to rewrite an entire JavaScript application at once.

A typical migration strategy:

* Enable `allowJs`.
* Enable `checkJs` when ready to analyze existing JavaScript.
* Add JSDoc annotations to important JavaScript modules.
* Introduce TypeScript files alongside JavaScript files.
* Convert modules incrementally.
* Add explicit types to public APIs.
* Remove JavaScript files when migration is complete, if desired.

Example migration structure:

```text
src/
├── legacy/
│   ├── auth.js
│   └── logger.js
├── services/
│   └── user-service.ts
└── app.ts
```

This approach reduces migration risk and allows teams to improve type safety incrementally.

# 🗁 Important Compatibility Considerations

Although TypeScript supports JavaScript, some differences require attention.

**Runtime type checking**

TypeScript types are removed during compilation.

```typescript
function greet(name: string) {
  return `Hello, ${name}`;
}
```

This does not prevent JavaScript callers from passing unexpected values at runtime.

**JavaScript dependencies**

A JavaScript library may not include TypeScript declarations.

You may need to install a corresponding `@types` package or write your own declarations.

**Module compatibility**

JavaScript and TypeScript files must use compatible module systems and module resolution settings.

**Third-party API behavior**

Type declarations describe expected APIs, but they do not guarantee that a JavaScript library follows those declarations at runtime.

# 🗁 Best Practices

* Reuse existing JavaScript libraries whenever appropriate.
* Check whether dependencies already provide TypeScript declarations.
* Use `allowJs` when integrating existing JavaScript files.
* Enable `checkJs` to catch errors in JavaScript gradually.
* Use JSDoc when adding type information to JavaScript.
* Migrate large projects incrementally.
* Keep module configuration consistent across JavaScript and TypeScript files.
* Validate external data at runtime instead of relying only on static types.

# 🗁 Key Takeaways

* TypeScript is a superset of JavaScript.
* TypeScript compiles to JavaScript and uses existing JavaScript runtimes.
* Existing JavaScript libraries can be imported into TypeScript projects.
* JavaScript and TypeScript files can coexist in the same project.
* `allowJs` enables JavaScript files in a TypeScript project.
* `checkJs` enables type checking for JavaScript files.
* JSDoc adds type information without changing JavaScript into TypeScript.
* Gradual migration makes TypeScript adoption practical for existing applications.
* TypeScript's static types do not replace runtime validation.
