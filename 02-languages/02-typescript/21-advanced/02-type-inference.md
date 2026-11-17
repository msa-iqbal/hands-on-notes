# Type Inference in TypeScript

Type inference is the ability of TypeScript to automatically determine the type of a variable, expression, function, or other construct without requiring an explicit type annotation.

Instead of manually specifying every type, TypeScript analyzes the code and derives types from available information.

Example:

```typescript
let username = "Alice";

let age = 25;

let isActive = true;
```

TypeScript automatically infers:

```typescript
let username: string = "Alice";

let age: number = 25;

let isActive: boolean = true;
```

Type inference reduces unnecessary annotations while maintaining static type checking.

# 🗁 Why Type Inference Matters

Type inference provides several benefits:

* Reduces repetitive type annotations.
* Makes code cleaner and easier to read.
* Preserves type safety.
* Improves editor autocomplete.
* Helps identify type errors.
* Simplifies function and expression declarations.
* Supports more maintainable code.

# 🗁 Variable Type Inference

TypeScript infers a variable's type from its initialization value.

```typescript
let username = "Alice";

let age = 25;

let isActive = true;
```

The inferred types are:

```typescript
// username: string
// age: number
// isActive: boolean
```

TypeScript then checks subsequent assignments.

```typescript
let username = "Alice";

username = "Bob"; // Valid

username = 100; // Type error
```

The variable is inferred as `string`, so assigning a number is invalid.

# 🗁 Type Inference with const

Variables declared with `const` can have more specific inferred types because their bindings cannot be reassigned.

```typescript
const username = "Alice";

const age = 25;

const isActive = true;
```

TypeScript can infer literal types:

```typescript
// username: "Alice"
// age: 25
// isActive: true
```

Compare this with `let`:

```typescript
let username = "Alice";

const fixedUsername = "Alice";
```

The inferred types are generally:

```typescript
// username: string
// fixedUsername: "Alice"
```

A `let` variable can be reassigned, so TypeScript usually infers a wider type.

A `const` variable cannot be reassigned, allowing TypeScript to preserve its literal type when appropriate.

# 🗁 Array Type Inference

TypeScript infers array element types from their initial values.

```typescript
const numbers = [10, 20, 30];

const names = ["Alice", "Bob", "Charlie"];

const flags = [true, false, true];
```

Inferred types:

```typescript
// numbers: number[]
// names: string[]
// flags: boolean[]
```

Mixed arrays can produce union element types:

```typescript
const values = [10, "Hello", true];
```

Inferred type:

```typescript
// (string | number | boolean)[]
```

The array can contain values belonging to any of the inferred element types.

# 🗁 Object Type Inference

TypeScript infers object property types from their initial values.

```typescript
const user = {
  id: 1,
  name: "Alice",
  isActive: true,
};
```

Inferred structure:

```typescript
{
  id: number;
  name: string;
  isActive: boolean;
}
```

Property assignments are checked against their inferred types.

```typescript
user.name = "Bob"; // Valid

user.id = "100"; // Type error
```

TypeScript does not automatically make object properties readonly just because the object variable uses `const`.

# 🗁 Function Parameter Inference

TypeScript can infer function parameter types from contextual information.

For example, when using an array method:

```typescript
const numbers = [10, 20, 30];

numbers.forEach((number) => {
  console.log(number.toFixed(2));
});
```

TypeScript knows that `number` is a `number` because `forEach` provides contextual type information.

The parameter does not need an explicit annotation.

# 🗁 Return Type Inference

TypeScript can infer a function's return type from its return statements.

```typescript
function add(a: number, b: number) {
  return a + b;
}
```

Inferred return type:

```typescript
// number
```

Another example:

```typescript
function getUser() {
  return {
    id: 1,
    name: "Alice",
  };
}
```

Inferred return type:

```typescript
{
  id: number;
  name: string;
}
```

Explicit return annotations are still useful for public APIs and functions where a clear contract improves readability.

# 🗁 Return Type Inference with Multiple Return Statements

When a function returns different types from different branches, TypeScript can infer a union return type.

```typescript
function getValue(condition: boolean) {
  if (condition) {
    return "Success";
  }

  return 404;
}
```

Inferred return type:

```typescript
// string | number
```

The caller must account for both possibilities.

```typescript
const result = getValue(true);

if (typeof result === "string") {
  console.log(result.toUpperCase());
} else {
  console.log(result.toFixed(0));
}
```

# 🗁 Contextual Typing

Contextual typing occurs when TypeScript infers a type from the location where an expression is used.

Example:

```typescript
type ClickHandler = (
  event: MouseEvent
) => void;

const handleClick: ClickHandler = (event) => {
  console.log(event.clientX);
};
```

TypeScript knows that `event` is a `MouseEvent` because the function is assigned to a variable with the `ClickHandler` type.

Contextual typing is commonly used with:

* Callback functions.
* Event handlers.
* Array methods.
* Object properties.
* Function arguments.
* Generic APIs.

# 🗁 Type Inference with Callbacks

Callback parameters often receive their types from the function that accepts the callback.

```typescript
const names = ["Alice", "Bob", "Charlie"];

const results = names.map((name) => {
  return name.toUpperCase();
});
```

TypeScript infers:

```typescript
// name: string
// results: string[]
```

This allows callback functions to remain concise without losing type information.

# 🗁 Type Inference with Destructuring

TypeScript can infer types when extracting values from arrays and objects.

Object destructuring:

```typescript
const user = {
  id: 1,
  name: "Alice",
  active: true,
};

const { id, name, active } = user;
```

Inferred types:

```typescript
// id: number
// name: string
// active: boolean
```

Array destructuring:

```typescript
const coordinates = [10, 20] as const;

const [x, y] = coordinates;
```

Inferred types:

```typescript
// x: 10
// y: 20
```

The `as const` assertion preserves literal types and makes the tuple readonly.

# 🗁 Type Inference with Conditional Expressions

TypeScript infers a common type from the possible results of a conditional expression.

```typescript
const result = Math.random() > 0.5
  ? "Success"
  : 404;
```

Inferred type:

```typescript
// string | number
```

Both possible values are represented in the resulting union type.

# 🗁 Best Common Type

When TypeScript infers a type from multiple expressions, it may look for a common type that represents all possible values.

This is called the best common type algorithm.

Example:

```typescript
const values = [1, 2, 3];
```

All elements are numbers, so TypeScript infers:

```typescript
// number[]
```

Another example:

```typescript
class Animal {}

class Dog extends Animal {}

class Cat extends Animal {}

const animals = [
  new Dog(),
  new Cat(),
];
```

TypeScript can infer a common base type:

```typescript
// Animal[]
```

The exact result depends on the available type relationships and the structure of the expressions.

# 🗁 Type Inference with Generics

TypeScript can infer generic type parameters from function arguments.

```typescript
function identity<T>(value: T): T {
  return value;
}
```

Calling the function:

```typescript
const first = identity("Hello");

const second = identity(100);
```

TypeScript infers:

```typescript
// first: string
// second: number
```

Explicit generic arguments are optional when TypeScript can determine the types.

```typescript
const first = identity<string>("Hello");
```

# 🗁 Type Inference with Generic Constraints

Generic constraints allow TypeScript to infer types while enforcing structural requirements.

```typescript
function getLength<T extends { length: number }>(
  value: T
): number {
  return value.length;
}
```

Usage:

```typescript
const stringLength = getLength("Hello");

const arrayLength = getLength([10, 20, 30]);
```

TypeScript infers the argument types while checking that they satisfy the constraint.

The following is invalid:

```typescript
getLength(100); // Type error
```

A number does not have a `length` property.

# 🗁 Type Inference with Promise

TypeScript can infer the resolved value type of a Promise.

```typescript
async function getUser() {
  return {
    id: 1,
    name: "Alice",
  };
}
```

Inferred return type:

```typescript
// Promise<{
//   id: number;
//   name: string;
// }>
```

Using `await`:

```typescript
const user = await getUser();

console.log(user.name);
```

TypeScript understands the resolved object structure.

# 🗁 Type Inference with the satisfies Operator

The `satisfies` operator checks whether an expression conforms to a type without simply replacing its inferred type with the target type.

```typescript
type RouteConfig = Record<
  string,
  {
    path: string;
    authenticated: boolean;
  }
>;

const routes = {
  home: {
    path: "/",
    authenticated: false,
  },
  dashboard: {
    path: "/dashboard",
    authenticated: true,
  },
} satisfies RouteConfig;
```

TypeScript checks the structure against `RouteConfig` while retaining useful information about the actual expression.

For example:

```typescript
routes.home.path;
```

The property information remains available through inference.

# 🗁 When Explicit Type Annotations Are Necessary

Type inference is powerful, but some situations benefit from explicit annotations.

**Variables without initial values**

```typescript
let username: string;

username = "Alice";
```

Without an annotation, an uninitialized variable may receive a broader inferred type.

**Public function contracts**

```typescript
export function calculateTotal(
  price: number,
  quantity: number
): number {
  return price * quantity;
}
```

Explicit return types make the public contract clear.

**Complex object structures**

```typescript
interface User {
  id: number;
  name: string;
}

const user: User = {
  id: 1,
  name: "Alice",
};
```

An explicit interface can improve consistency across multiple parts of an application.

**Avoiding unintended inference**

```typescript
let status: "pending" | "success" | "failed" = "pending";
```

An explicit union type allows the variable to accept all intended states.

# 🗁 Type Inference vs Type Annotation

| Feature | Type inference | Type annotation |
| --- | --- | --- |
| Type source | Determined by TypeScript | Specified by developer |
| Syntax | No explicit type required | Uses `: Type` |
| Code verbosity | Usually lower | Can be higher |
| Type control | Based on available context | Explicitly defines expected type |
| Best use | Straightforward local values | Public contracts and complex structures |

Example using inference:

```typescript
const age = 25;
```

Example using annotation:

```typescript
const age: number = 25;
```

Both provide the same effective variable type in this example.

# 🗁 Common Type Inference Problems

**Uninitialized variables**

```typescript
let value;

value = 100;

value = "Hello";
```

Without a useful initialization or annotation, TypeScript may infer `any` for the variable.

Prefer:

```typescript
let value: string | number;

value = 100;

value = "Hello";
```

**Unexpected literal widening**

```typescript
let status = "pending";
```

TypeScript generally infers `string`, not the literal type `"pending"`.

If only specific values should be allowed:

```typescript
type Status = "pending" | "success" | "failed";

let status: Status = "pending";
```

**Empty array inference**

```typescript
const values = [];
```

The inferred type of an empty array can depend on compiler options and context.

When the intended element type is known, specify it:

```typescript
const values: number[] = [];
```

**Lost contextual typing**

Extracting a callback from its original context can remove information that TypeScript would otherwise infer.

```typescript
type Handler = (value: string) => void;

const handleValue: Handler = (value) => {
  console.log(value.toUpperCase());
};
```

Here, the annotation provides the context needed to infer `value` as a string.

# 🗁 Best Practices

* Let TypeScript infer straightforward local variable types.
* Use explicit annotations for important public contracts.
* Annotate variables when they are declared without an initial value.
* Use explicit union types when a variable has a known set of valid values.
* Prefer contextual typing for callbacks.
* Use `as const` when literal and readonly information is required.
* Use `satisfies` to validate structures while retaining useful inference.
* Avoid unnecessary type assertions.
* Understand how literal widening affects inferred types.
* Use strict compiler options to improve type checking.

# 🗁 Key Takeaways

* Type inference allows TypeScript to determine types automatically.
* Variables, arrays, objects, functions, and expressions can all benefit from inference.
* `const` often preserves more specific literal types than `let`.
* Callback parameters frequently receive contextual types.
* TypeScript uses best common type analysis when combining expressions.
* Generic type parameters can be inferred from function arguments.
* Promise return types are inferred from resolved values.
* The `satisfies` operator checks compatibility while preserving useful inferred information.
* Explicit annotations remain valuable for public APIs and complex contracts.
* Good TypeScript code balances inference with intentional type annotations.
