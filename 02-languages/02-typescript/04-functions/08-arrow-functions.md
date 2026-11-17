# Arrow Functions

Arrow functions are a concise way to write function expressions in JavaScript and TypeScript. They use the `=>` syntax and are commonly used for callbacks, array operations, event handlers, and short functional expressions.

TypeScript supports arrow functions with parameter types, return type annotations, contextual typing, and generic type patterns.

# 🗁 Basic Syntax

```typescript
const functionName = (parameters) => {
  // Function body
};
```

Example:

```typescript
const greet = (name: string): string => {
  return `Hello, ${name}!`;
};

console.log(greet("Alice"));
```

Output:

```text
Hello, Alice!
```

# 🗁 Single-Parameter Arrow Functions

Parentheses around a single parameter are optional when the parameter has no type annotation.

```typescript
const double = (value: number): number => {
  return value * 2;
};
```

You can also omit parentheses in an unannotated parameter:

```typescript
const double = value => value * 2;
```

However, when explicitly annotating the parameter type, parentheses are required:

```typescript
const double = (value: number): number => value * 2;
```

# 🗁 Multiple Parameters

Arrow functions with multiple parameters require parentheses.

```typescript
const add = (a: number, b: number): number => {
  return a + b;
};

console.log(add(10, 20));
```

Output:

```text
30
```

# 🗁 Return Type Annotations

You can explicitly declare an arrow function's return type.

```typescript
const calculateArea = (width: number, height: number): number => {
  return width * height;
};
```

TypeScript also infers the return type when it is not explicitly declared.

```typescript
const multiply = (a: number, b: number) => {
  return a * b;
};

// Inferred return type: number
```

Explicit return types are useful when you want to make a function's contract clear or enforce a particular return type.

# 🗁 Implicit Returns

When an arrow function contains a single expression, you can omit the curly braces and `return` keyword.

```typescript
const square = (value: number): number => value * value;

const greet = (name: string): string => `Hello, ${name}!`;
```

This is called an implicit return.

# 🗁 Returning Objects

When returning an object literal directly, wrap the object in parentheses.

```typescript
const createUser = (name: string, age: number) => ({
  name,
  age,
});

const user = createUser("Alice", 25);

console.log(user);
```

Output:

```text
{ name: "Alice", age: 25 }
```

Without parentheses, JavaScript interprets the curly braces as the function body.

# 🗁 Void Return Type

Use `void` when an arrow function does not return a meaningful value.

```typescript
const logMessage = (message: string): void => {
  console.log(message);
};

logMessage("TypeScript is useful");
```

# 🗁 Arrow Functions as Callbacks

Arrow functions are frequently used as callback functions.

```typescript
const numbers: number[] = [1, 2, 3, 4, 5];

const doubled = numbers.map((number): number => number * 2);

console.log(doubled);
```

Output:

```text
[2, 4, 6, 8, 10]
```

Other common examples:

```typescript
const numbers = [10, 20, 30, 40];

const filtered = numbers.filter(number => number > 20);

const total = numbers.reduce(
  (sum, number) => sum + number,
  0
);

numbers.forEach(number => {
  console.log(number);
});
```

TypeScript can infer callback parameter types from the method being called.

# 🗁 Arrow Function Type Annotations

You can define a function type and assign an arrow function to it.

```typescript
type MathOperation = (a: number, b: number) => number;

const add: MathOperation = (a, b) => a + b;

const subtract: MathOperation = (a, b) => a - b;
```

The parameter and return types are inferred from `MathOperation`.

You can also use a function type directly:

```typescript
const greet: (name: string) => string = name => {
  return `Hello, ${name}!`;
};
```

# 🗁 Optional and Default Parameters

Arrow functions support optional and default parameters just like regular functions.

Optional parameter:

```typescript
const greet = (name?: string): string => {
  return `Hello, ${name ?? "Guest"}!`;
};

console.log(greet());
console.log(greet("Alice"));
```

Default parameter:

```typescript
const calculatePrice = (
  price: number,
  taxRate: number = 0.1
): number => {
  return price + price * taxRate;
};

console.log(calculatePrice(100));
console.log(calculatePrice(100, 0.2));
```

# 🗁 Rest Parameters

Arrow functions also support rest parameters.

```typescript
const sum = (...numbers: number[]): number => {
  return numbers.reduce((total, number) => total + number, 0);
};

console.log(sum(10, 20, 30));
```

Output:

```text
60
```

# 🗁 Generic Arrow Functions

Arrow functions can be generic.

```typescript
const identity = <T>(value: T): T => {
  return value;
};

const numberValue = identity<number>(100);
const stringValue = identity<string>("TypeScript");
```

In TSX files, the syntax `<T>` can be interpreted as a JSX element. Use a trailing comma to disambiguate a generic arrow function:

```tsx
const identity = <T,>(value: T): T => {
  return value;
};
```

You can also let TypeScript infer the generic type:

```typescript
const result = identity("Hello");

// Inferred as string
```

# 🗁 Lexical `this`

One of the most important differences between arrow functions and regular functions is how they handle `this`.

Arrow functions do not create their own `this`. Instead, they capture `this` from the surrounding lexical scope.

```typescript
class Counter {
  count = 0;

  increment = (): void => {
    this.count++;
  };
}

const counter = new Counter();

counter.increment();

console.log(counter.count);
```

Output:

```text
1
```

Because the arrow function captures the instance's `this`, it can be useful as a class field when passing methods as callbacks.

Compare this with a regular method:

```typescript
class Counter {
  count = 0;

  increment(): void {
    this.count++;
  }
}

const counter = new Counter();

const increment = counter.increment;

// Calling increment() without the instance as its receiver
// may result in an undefined this value.
```

A regular method's `this` depends on how the method is called. An arrow function's `this` is determined by where the function is created.

# 🗁 Arrow Functions vs Regular Functions

| Feature | Arrow Function | Regular Function |
| --- | --- | --- |
| Syntax | `=>` | `function` |
| Own `this` | No | Yes, depending on invocation |
| Own `arguments` | No | Yes |
| Constructor | Cannot be used with `new` | Can be constructable when defined appropriately |
| Generator syntax | Cannot directly use `function*` syntax | Supports generator functions |
| Implicit return | Supported | Not supported |
| Suitable for callbacks | Yes | Yes |
| Method definitions | Can be used, but `this` is lexical | Often suitable for dynamic `this` |

# 🗁 When to Use Arrow Functions

Use arrow functions when:

* Writing short callback functions.
* Transforming or filtering arrays.
* Defining functions that should inherit lexical `this`.
* Creating concise function expressions.
* Defining class fields that need to retain the instance context.

Prefer regular methods or function declarations when:

* You need a method with dynamically determined `this`.
* You need a constructor function.
* You need a generator function.
* You want a named function declaration for organization or hoisting behavior.

# 🗁 Common Mistakes

**Forgetting parentheses when returning an object**

Incorrect:

```typescript
const createUser = (name: string) => {
  name: name;
};
```

Correct:

```typescript
const createUser = (name: string) => ({
  name,
});
```

**Using arrow functions as constructors**

Incorrect:

```typescript
const User = (name: string) => {
  this.name = name;
};

// new User("Alice"); // TypeError
```

Arrow functions cannot be used with `new`.

Use a class or a regular constructable function instead.

```typescript
class User {
  constructor(public name: string) {}
}

const user = new User("Alice");
```

**Expecting an arrow function to have its own `this`**

Arrow functions capture `this` from their surrounding scope. They do not bind a new `this` when called.

**Using `arguments` inside an arrow function**

Arrow functions do not have their own `arguments` object.

Use rest parameters instead:

```typescript
const logValues = (...values: unknown[]): void => {
  console.log(values);
};

logValues("Hello", 100, true);
```

# 🗁 Best Practices

* Use arrow functions for concise callbacks and expressions.
* Use explicit parameter types when contextual typing cannot infer them.
* Add explicit return types when they improve readability or enforce an API contract.
* Use parentheses when returning object literals.
* Prefer rest parameters over relying on `arguments`.
* Understand lexical `this` before using arrow functions in classes.
* Avoid unnecessarily long nested arrow functions.
* Use regular methods when dynamic `this` behavior is required.

# 🗁 Summary

* Arrow functions use the `=>` syntax.
* They support parameter and return type annotations.
* Single-expression arrow functions can use implicit returns.
* Object literals returned directly must be wrapped in parentheses.
* Arrow functions support generics, optional parameters, default parameters, and rest parameters.
* They capture `this` lexically instead of creating their own `this`.
* They cannot be used as constructors.
* They are especially useful for callbacks and functional programming patterns.

# 🗁 Related Topics

* Function Types
* Function Parameters
* Return Types
* Optional Parameters
* Default Parameters
* Rest Parameters
* Function Overloads
* Callback Functions
* `this` Parameters
