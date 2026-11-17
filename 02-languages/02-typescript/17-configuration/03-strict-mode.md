# Strict Mode

Strict mode is a TypeScript compiler setting that enables a collection of stricter type-checking rules.

It helps developers identify potential errors during development instead of discovering them at runtime.

Strict mode is enabled through the `strict` option in `tsconfig.json`.

```json
{
  "compilerOptions": {
    "strict": true
  }
}
```

When enabled, TypeScript activates several strict checking options that improve type safety.

# 🗁 Why Strict Mode Matters

Without strict checking, TypeScript allows certain patterns that can introduce runtime bugs.

For example:

```typescript
function greet(name) {
  return `Hello, ${name}`;
}

greet(123);
```

Without `noImplicitAny`, the `name` parameter may implicitly become `any`.

With strict mode enabled, TypeScript reports an error because the parameter has no explicit or inferred type.

A safer implementation:

```typescript
function greet(name: string): string {
  return `Hello, ${name}`;
}

greet("John");
```

Strict mode helps prevent:

* Accidental use of `any`.
* Null and undefined access errors.
* Incorrect function assignments.
* Uninitialized class properties.
* Unsafe use of `this`.
* Invalid function arguments.
* Incorrect handling of caught errors.

# 🗁 Enabling Strict Mode

Enable strict mode in `tsconfig.json`.

```json
{
  "compilerOptions": {
    "strict": true
  }
}
```

You can also enable it through the command line:

```bash
tsc --strict
```

When strict mode is enabled, TypeScript activates the strict checking options as a group.

Individual strict options can still be configured separately.

# 🗁 Strict Mode Options

The `strict` option enables the following compiler checks:

| Option | Purpose |
| --- | --- |
| `noImplicitAny` | Reports declarations that implicitly have type `any` |
| `strictNullChecks` | Treats `null` and `undefined` as distinct types |
| `strictFunctionTypes` | Checks function parameter compatibility more strictly |
| `strictBindCallApply` | Checks arguments passed to `bind`, `call`, and `apply` |
| `strictPropertyInitialization` | Requires class properties to be initialized |
| `noImplicitThis` | Reports expressions with an implicitly typed `this` |
| `useUnknownInCatchVariables` | Gives catch variables the type `unknown` |
| `alwaysStrict` | Parses files in strict mode and emits strict-mode directives where applicable |

These options can also be enabled individually.

# 🗁 noImplicitAny

The `noImplicitAny` option reports declarations where TypeScript would otherwise infer an implicit `any` type.

Example:

```typescript
function calculate(a, b) {
  return a + b;
}
```

TypeScript reports errors because the parameters do not have explicit types and cannot be inferred from their usage.

A better implementation:

```typescript
function calculate(a: number, b: number): number {
  return a + b;
}
```

You can also allow TypeScript to infer types from initial values:

```typescript
const username = "John";
const age = 25;
```

The inferred types are `string` and `number`.

# 🗁 strictNullChecks

The `strictNullChecks` option ensures that `null` and `undefined` are treated as separate types.

Example:

```typescript
let username: string = "John";

// TypeScript error
username = null;
```

To allow null values, explicitly include `null` in the type.

```typescript
let username: string | null = "John";

username = null;
```

It also requires checking nullable values before accessing their properties.

```typescript
function getLength(value: string | null): number {
  if (value === null) {
    return 0;
  }

  return value.length;
}
```

This prevents many common null-related runtime errors.

# 🗁 strictFunctionTypes

The `strictFunctionTypes` option checks function parameter types more carefully when determining whether one function type can be assigned to another.

Example:

```typescript
type Animal = {
  name: string;
};

type Dog = Animal & {
  breed: string;
};

let handleAnimal = (animal: Animal): void => {
  console.log(animal.name);
};

let handleDog = (dog: Dog): void => {
  console.log(dog.breed);
};

// Error under strictFunctionTypes
handleAnimal = handleDog;
```

The assignment is unsafe because `handleAnimal` is expected to accept any `Animal`, but `handleDog` requires a more specific `Dog`.

However, assigning a function that accepts a broader type to a variable that requires a narrower input is safe:

```typescript
handleDog = handleAnimal;
```

This behavior helps prevent incompatible callback assignments.

Note that function parameter checking has special compatibility rules for method declarations.

# 🗁 strictBindCallApply

The `strictBindCallApply` option validates arguments passed to the function methods `bind()`, `call()`, and `apply()`.

Example:

```typescript
function add(a: number, b: number): number {
  return a + b;
}

add.call(null, 10, 20);

// TypeScript error
add.call(null, "10", 20);
```

It also checks argument types for `bind()` and `apply()`.

This helps catch incorrect function invocations before runtime.

# 🗁 strictPropertyInitialization

The `strictPropertyInitialization` option requires class properties to be initialized during declaration or through the constructor.

Example:

```typescript
class User {
  name: string;
}
```

TypeScript reports an error because `name` is not initialized.

A valid implementation:

```typescript
class User {
  name: string;

  constructor(name: string) {
    this.name = name;
  }
}
```

You can also initialize the property directly:

```typescript
class User {
  name: string = "Guest";
}
```

When a property is intentionally initialized later, the definite assignment assertion can be used:

```typescript
class User {
  name!: string;

  initialize(name: string): void {
    this.name = name;
  }
}
```

The `!` tells TypeScript that the property will be assigned through another mechanism.

It does not perform runtime initialization or guarantee that the property is actually assigned.

# 🗁 noImplicitThis

The `noImplicitThis` option reports expressions where `this` has an implicit `any` type.

Example:

```typescript
function getName() {
  return this.name;
}
```

A safer implementation uses an explicit `this` parameter:

```typescript
type User = {
  name: string;
};

function getName(this: User): string {
  return this.name;
}
```

The `this` parameter is a TypeScript-only type annotation and is removed during compilation.

# 🗁 useUnknownInCatchVariables

The `useUnknownInCatchVariables` option makes catch variables default to `unknown` instead of `any`.

Example:

```typescript
try {
  throw new Error("Something went wrong");
} catch (error) {
  if (error instanceof Error) {
    console.log(error.message);
  }
}
```

You must narrow the error before accessing its properties.

This prevents unsafe assumptions about thrown values.

The option is enabled by default when `strict` is enabled.

# 🗁 alwaysStrict

The `alwaysStrict` option parses files in JavaScript strict mode and emits `"use strict"` directives where applicable.

```json
{
  "compilerOptions": {
    "alwaysStrict": true
  }
}
```

JavaScript strict mode changes certain language behaviors and prevents some error-prone operations.

For example, assigning to an undeclared variable throws an error in strict mode.

```javascript
"use strict";

username = "John";
```

The example throws a `ReferenceError` because `username` was never declared.

# 🗁 Overriding Individual Strict Options

Although `strict: true` enables all strict options, individual settings can be overridden.

Example:

```json
{
  "compilerOptions": {
    "strict": true,
    "noImplicitAny": false
  }
}
```

In this configuration:

* Strict mode remains enabled.
* Other strict checks remain enabled.
* Implicit `any` checking is disabled.

You can also enable strict options individually without enabling the entire strict group.

```json
{
  "compilerOptions": {
    "strictNullChecks": true,
    "noImplicitAny": true,
    "strictPropertyInitialization": true
  }
}
```

For most new projects, enabling the complete strict group is preferable.

# 🗁 Strict Mode and Existing Projects

Enabling strict mode in an existing JavaScript or TypeScript project may produce many compiler errors.

This commonly happens when the codebase contains:

* Implicit `any` parameters.
* Nullable values without checks.
* Uninitialized class properties.
* Incompatible callback types.
* Functions with incorrectly typed `this`.

A practical migration strategy is:

* Enable strict mode in a development branch.
* Review the reported diagnostics.
* Add missing type annotations.
* Handle nullable values explicitly.
* Initialize class properties correctly.
* Fix incompatible function assignments.
* Run tests after resolving errors.
* Keep strict mode enabled for future development.

Avoid suppressing errors with `any` or type assertions simply to make the compiler pass.

# 🗁 Checking Strict Mode Behavior

Use the TypeScript compiler to check the project.

```bash
tsc --noEmit
```

This performs type checking without generating JavaScript output.

To inspect the effective configuration:

```bash
tsc --showConfig
```

This helps verify whether strict mode and its individual options are enabled.

# 🗁 Best Practices

* Enable `strict: true` for new TypeScript projects.
* Use explicit types when inference is insufficient.
* Prefer `unknown` over `any` for uncertain values.
* Handle `null` and `undefined` explicitly.
* Initialize class properties properly.
* Use accurate function parameter and return types.
* Avoid unnecessary non-null assertions.
* Fix compiler errors instead of suppressing them.
* Introduce strict mode incrementally in legacy projects.
* Recheck strict settings when upgrading TypeScript.

# 🗁 Summary

Strict mode enables a group of compiler checks that improve TypeScript type safety.

Important concepts:

* `strict: true` enables the strict checking group.
* `noImplicitAny` prevents implicit `any` declarations.
* `strictNullChecks` requires explicit null handling.
* `strictFunctionTypes` improves function parameter compatibility checking.
* `strictBindCallApply` validates function invocation arguments.
* `strictPropertyInitialization` checks class property initialization.
* `noImplicitThis` checks the type of `this`.
* `useUnknownInCatchVariables` makes caught errors safer to inspect.
* `alwaysStrict` enables JavaScript strict-mode parsing and emission behavior.
