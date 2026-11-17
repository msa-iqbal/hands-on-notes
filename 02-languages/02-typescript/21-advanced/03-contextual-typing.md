# Contextual Typing in TypeScript

Contextual typing is a TypeScript feature that allows the compiler to infer the type of an expression from the location or context in which that expression is used.

Instead of explicitly annotating every variable or function parameter, TypeScript examines the expected type provided by the surrounding code.

Example:

```typescript
type ClickHandler = (event: MouseEvent) => void;

const handleClick: ClickHandler = (event) => {
  console.log(event.clientX);
};
```

TypeScript automatically understands that `event` is a `MouseEvent` because `handleClick` is expected to satisfy the `ClickHandler` type.

Without contextual typing, you would need to annotate the parameter explicitly.

```typescript
const handleClick = (event: MouseEvent): void => {
  console.log(event.clientX);
};
```

Both examples provide the same parameter type.

# 🗁 Why Contextual Typing Matters

Contextual typing helps developers:

* Reduce unnecessary type annotations.
* Write cleaner callback functions.
* Improve editor autocomplete.
* Maintain type safety.
* Work efficiently with event handlers.
* Use strongly typed library APIs.
* Understand expected types from surrounding code.

It is especially useful when working with callbacks, object literals, event listeners, and generic functions.

# 🗁 Contextual Typing vs Type Inference

These concepts are related, but they work in different directions.

**Type inference**

TypeScript determines a type from the available value or expression.

```typescript
const username = "Alice";
```

TypeScript infers `username` as a string.

**Contextual typing**

TypeScript determines an expression's type from the expected type provided by its surrounding context.

```typescript
type Formatter = (value: number) => string;

const format: Formatter = (value) => {
  return value.toFixed(2);
};
```

TypeScript infers `value` as a number from the `Formatter` type.

| Feature | Type inference | Contextual typing |
| --- | --- | --- |
| Direction | Expression to type | Context to expression |
| Main source | Initial values and expressions | Expected surrounding type |
| Common usage | Variables and return types | Callbacks and object literals |
| Example | `const age = 25` | `(event) => event.clientX` |

# 🗁 Contextual Typing with Function Expressions

When a function expression is assigned to a variable with a function type, TypeScript can infer its parameter types.

```typescript
type Calculate = (
  a: number,
  b: number
) => number;

const add: Calculate = (a, b) => {
  return a + b;
};
```

Inferred parameter types:

```typescript
// a: number
// b: number
```

The return type is also checked against the expected function return type.

```typescript
const multiply: Calculate = (a, b) => {
  return a * b;
};
```

An incompatible return value produces a type error.

```typescript
const invalid: Calculate = (a, b) => {
  return "Invalid"; // Type error
};
```

# 🗁 Contextual Typing with Arrow Functions

Arrow functions commonly benefit from contextual typing.

```typescript
type UserFormatter = (name: string) => string;

const formatName: UserFormatter = (name) => {
  return name.trim().toUpperCase();
};
```

TypeScript knows that `name` is a string.

This also works with inline callbacks:

```typescript
const names = ["Alice", "Bob", "Charlie"];

const uppercaseNames = names.map((name) => {
  return name.toUpperCase();
});
```

The `map()` method provides contextual information about its callback parameter.

TypeScript infers `name` as a string and `uppercaseNames` as `string[]`.

# 🗁 Contextual Typing with Event Handlers

Browser event handlers are a common example of contextual typing.

```typescript
const button = document.querySelector("button");

button?.addEventListener("click", (event) => {
  console.log(event.type);
  console.log(event.clientX);
});
```

TypeScript understands that the callback receives a mouse event.

For a keyboard event:

```typescript
document.addEventListener("keydown", (event) => {
  console.log(event.key);
  console.log(event.code);
});
```

TypeScript infers the callback parameter as a `KeyboardEvent`.

The event type is determined by the selected event name and the event listener overload.

# 🗁 Contextual Typing with Object Literals

TypeScript can infer property types from an expected object structure.

```typescript
interface User {
  name: string;
  age: number;
}

const user: User = {
  name: "Alice",
  age: 25,
};
```

The expected `User` type provides context for the object literal.

It also checks for invalid properties and incompatible values.

```typescript
const user: User = {
  name: "Alice",
  age: "25", // Type error
};
```

# 🗁 Contextual Typing with Object Methods

Contextual typing also applies to methods inside object literals.

```typescript
interface UserService {
  getUser(id: number): string;

  deleteUser(id: number): void;
}

const userService: UserService = {
  getUser(id) {
    return `User ${id}`;
  },

  deleteUser(id) {
    console.log(`Deleting user ${id}`);
  },
};
```

TypeScript infers both method parameters as numbers.

The method implementations must satisfy the interface contracts.

# 🗁 Contextual Typing with Array Callbacks

Array methods provide contextual types for their callback parameters.

```typescript
const products = [
  { id: 1, name: "Laptop", price: 1200 },
  { id: 2, name: "Mouse", price: 30 },
  { id: 3, name: "Keyboard", price: 80 },
];

const expensiveProducts = products.filter((product) => {
  return product.price > 100;
});
```

TypeScript infers the callback parameter as:

```typescript
{
  id: number;
  name: string;
  price: number;
}
```

Other methods that commonly use contextual typing include:

* `map()`
* `filter()`
* `forEach()`
* `find()`
* `some()`
* `every()`
* `reduce()`

Example with `reduce()`:

```typescript
const numbers = [10, 20, 30];

const total = numbers.reduce((sum, number) => {
  return sum + number;
}, 0);
```

The initial value and array element types help TypeScript determine the accumulator and callback parameter types.

# 🗁 Contextual Typing with Function Arguments

A function can provide an expected type for a callback argument.

```typescript
function execute(
  callback: (message: string) => void
): void {
  callback("Hello TypeScript");
}

execute((message) => {
  console.log(message.toUpperCase());
});
```

TypeScript infers `message` as a string from the function parameter's callback signature.

# 🗁 Contextual Typing with Generic Functions

Generic functions can provide contextual types through their parameter and callback signatures.

```typescript
function process<T>(
  value: T,
  callback: (item: T) => void
): void {
  callback(value);
}

process("Hello", (item) => {
  console.log(item.toUpperCase());
});
```

TypeScript infers `T` as `string` from the first argument.

That inferred type is then used to contextually type the callback parameter `item`.

Another example:

```typescript
process(100, (item) => {
  console.log(item.toFixed(2));
});
```

Here, `T` is inferred as `number`.

# 🗁 Contextual Typing with Conditional Expressions

The expected type can influence the types of expressions inside conditional expressions.

```typescript
type Status = "success" | "failed";

const status: Status = Math.random() > 0.5
  ? "success"
  : "failed";
```

The expected `Status` type provides context for the conditional expression.

Both branches must produce compatible values.

# 🗁 Contextual Typing with Template Literals

Contextual typing can also apply to template literal expressions when they are assigned to a compatible template literal type.

```typescript
type Route = `/${string}`;

const homeRoute: Route = "/";
const userRoute: Route = "/users";
```

The expected type checks that the resulting strings follow the required pattern.

# 🗁 Contextual Typing and the satisfies Operator

The `satisfies` operator checks whether an expression conforms to a target type while preserving its inferred type.

```typescript
type ButtonConfig = {
  text: string;
  disabled: boolean;
  onClick: () => void;
};

const buttonConfig = {
  text: "Save",
  disabled: false,
  onClick() {
    console.log("Saved");
  },
} satisfies ButtonConfig;
```

The object is checked against `ButtonConfig`.

Its method and properties receive contextual typing from the expected structure, while the expression retains its own inferred type.

# 🗁 Contextual Typing with Type Assertions

Type assertions can explicitly provide a type for an expression.

```typescript
const input = document.querySelector("#username") as HTMLInputElement;

input.addEventListener("input", (event) => {
  console.log(input.value);
  console.log(event.type);
});
```

The assertion tells TypeScript to treat `input` as an `HTMLInputElement`.

The event listener then provides contextual typing for its callback parameter.

A type assertion does not validate the actual runtime value.

# 🗁 When Contextual Typing Does Not Work as Expected

Contextual typing depends on the availability of an expected type.

Consider:

```typescript
const handleEvent = (event) => {
  console.log(event);
};
```

Without a contextual type or explicit annotation, TypeScript cannot determine the intended parameter type.

With `noImplicitAny` enabled, this produces an implicit `any` error.

Provide an explicit type:

```typescript
const handleEvent = (event: MouseEvent) => {
  console.log(event.clientX);
};
```

Or provide a contextual type:

```typescript
type EventHandler = (event: MouseEvent) => void;

const handleEvent: EventHandler = (event) => {
  console.log(event.clientX);
};
```

# 🗁 Contextual Typing and Strict Compiler Options

The `strict` compiler option enables stronger type checking.

Example:

```json
{
  "compilerOptions": {
    "strict": true
  }
}
```

One important strict option is `noImplicitAny`.

It reports errors when TypeScript cannot infer a parameter type and would otherwise assign an implicit `any`.

Contextual typing helps avoid many such errors by providing expected types from surrounding code.

# 🗁 Best Practices

* Use contextual typing for callback parameters when the surrounding API provides reliable types.
* Avoid repeating annotations that TypeScript can infer clearly.
* Explicitly annotate standalone function parameters when no context exists.
* Define reusable function signatures with type aliases or interfaces.
* Use strict compiler options to detect missing type information.
* Prefer strongly typed library APIs.
* Use `satisfies` when validating object structures while preserving useful inference.
* Do not confuse contextual typing with runtime type validation.
* Avoid unnecessary type assertions to silence inference errors.

# 🗁 Common Mistakes

**Assuming every function parameter is automatically inferred**

```typescript
const greet = (name) => {
  return `Hello ${name}`;
};
```

Without contextual information, `name` may trigger an implicit `any` error under strict settings.

**Confusing contextual typing with explicit annotation**

```typescript
const greet: (name: string) => string = (name) => {
  return `Hello ${name}`;
};
```

The variable has an explicit function type, but the parameter receives its type through contextual typing.

**Assuming contextual typing validates runtime data**

Contextual typing checks source code against expected types. It does not validate values received from external APIs.

**Ignoring callback signatures**

A callback must satisfy the parameter and return type requirements of the API that receives it.

# 🗁 Key Takeaways

* Contextual typing derives expression types from their surrounding expected types.
* It is especially useful for function expressions, arrow functions, and callbacks.
* Event handlers receive contextual types from event listener APIs.
* Object literals can receive contextual property and method types.
* Generic functions can propagate inferred types into callback parameters.
* The `satisfies` operator combines compatibility checking with useful type preservation.
* Explicit annotations are still necessary when no reliable context exists.
* Contextual typing reduces boilerplate while preserving TypeScript's static type checking.
