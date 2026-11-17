# const and let

`const` and `let` are modern JavaScript and TypeScript keywords used to declare variables.

Both keywords provide block scope and help prevent several problems associated with the older `var` declaration.

The primary difference is:

* `let` allows a variable to be reassigned.
* `const` prevents a variable from being reassigned.

Example:

```typescript
let age: number = 25;

age = 30;

const country: string = "Bangladesh";

console.log(age);
console.log(country);
```

Output:

```text
30
Bangladesh
```

# 🗁 The let Keyword

The `let` keyword declares a block-scoped variable whose value can be reassigned.

Syntax:

```typescript
let variableName: Type = value;
```

Example:

```typescript
let username: string = "John";

console.log(username);

username = "David";

console.log(username);
```

Output:

```text
John
David
```

The variable `username` can hold a new string value because it was declared using `let`.

# 🗁 The const Keyword

The `const` keyword declares a block-scoped variable whose binding cannot be reassigned.

Syntax:

```typescript
const variableName: Type = value;
```

Example:

```typescript
const username: string = "John";

console.log(username);
```

A `const` variable must be initialized when it is declared.

Invalid example:

```typescript
const age: number;
```

TypeScript reports an error because a constant declaration requires an initializer.

Reassignment is also invalid:

```typescript
const age: number = 25;

age = 30;
```

The compiler reports an error because a `const` variable cannot be reassigned.

# 🗁 Differences Between const and let

| Feature | let | const |
| --- | --- | --- |
| Declaration | Supported | Supported |
| Block scope | Yes | Yes |
| Reassignment | Allowed | Not allowed |
| Redeclaration in the same scope | Not allowed | Not allowed |
| Initialization at declaration | Optional when type is specified | Required |
| Hoisting | Yes, but inaccessible before initialization | Yes, but inaccessible before initialization |
| Temporal Dead Zone | Yes | Yes |
| Object property modification | Allowed | Allowed |
| Array modification | Allowed | Allowed |

# 🗁 Block Scope

Both `let` and `const` are block-scoped.

A block is a section of code enclosed in curly braces `{}`.

Example:

```typescript
{
    let message: string = "Hello";

    console.log(message);
}
```

Output:

```text
Hello
```

The variable `message` cannot be accessed outside the block.

```typescript
{
    let message: string = "Hello";
}

console.log(message);
```

This produces a compiler error because `message` is not accessible outside its block.

The same rule applies to `const`:

```typescript
{
    const country: string = "Bangladesh";
}

console.log(country);
```

The variable `country` is inaccessible outside the block.

# 🗁 Reassignment with let

Variables declared with `let` can be reassigned.

```typescript
let score: number = 100;

score = 150;
score = 200;

console.log(score);
```

Output:

```text
200
```

The variable retains its declared type.

```typescript
let score: number = 100;

score = "Two hundred";
```

This produces a type error because the variable expects a number.

If multiple types are required, use a union type:

```typescript
let score: number | string = 100;

score = "Two hundred";

console.log(score);
```

# 🗁 Reassignment with const

A `const` variable cannot be reassigned after initialization.

```typescript
const pi: number = 3.14159;

console.log(pi);
```

This is invalid:

```typescript
const pi: number = 3.14159;

pi = 3.14;
```

However, `const` prevents reassignment of the variable binding, not every possible modification of the value it references.

# 🗁 const with Objects

Objects declared with `const` can have their properties modified.

Example:

```typescript
const user = {
    name: "John",
    age: 25
};

user.age = 30;

console.log(user);
```

Output:

```text
{
    name: "John",
    age: 30
}
```

The following reassignment is invalid:

```typescript
const user = {
    name: "John"
};

user = {
    name: "David"
};
```

The variable `user` cannot be assigned a completely different object.

# 🗁 const with Arrays

Arrays declared with `const` can still be modified.

Example:

```typescript
const numbers: number[] = [10, 20, 30];

numbers.push(40);

console.log(numbers);
```

Output:

```text
[10, 20, 30, 40]
```

Changing an existing element is also allowed:

```typescript
const numbers: number[] = [10, 20, 30];

numbers[0] = 100;

console.log(numbers);
```

Output:

```text
[100, 20, 30]
```

However, assigning a new array is invalid:

```typescript
const numbers: number[] = [10, 20, 30];

numbers = [40, 50, 60];
```

# 🗁 const Does Not Mean Immutable

A common misconception is that `const` makes a value immutable.

It does not.

`const` prevents reassignment of a variable binding. It does not automatically prevent changes to object properties or array elements.

To restrict modifications through a TypeScript reference, use `readonly`.

**Readonly object properties:**

```typescript
interface User {
    readonly id: number;
    name: string;
}

const user: User = {
    id: 1,
    name: "John"
};

user.name = "David"; // Valid

user.id = 2; // Type error
```

**Readonly arrays:**

```typescript
const numbers: readonly number[] = [10, 20, 30];

// numbers.push(40); // Type error
// numbers[0] = 100; // Type error
```

TypeScript's `readonly` provides compile-time restrictions. It does not automatically freeze the underlying object at runtime.

# 🗁 Temporal Dead Zone (TDZ)

The Temporal Dead Zone is the period between entering a scope and initializing a `let` or `const` variable.

Variables declared with `let` and `const` are hoisted, but they cannot be accessed before their declaration is initialized.

Example:

```typescript
console.log(username);

let username: string = "John";
```

This produces a TypeScript error.

JavaScript also throws a `ReferenceError` when this code executes.

The same applies to `const`:

```typescript
console.log(country);

const country: string = "Bangladesh";
```

The variable cannot be accessed before initialization.

# 🗁 Hoisting with let and const

JavaScript processes declarations before executing code, but `let` and `const` remain inaccessible during their Temporal Dead Zone.

Example:

```typescript
let value: number = 10;

{
    console.log(value);

    let value: number = 20;
}
```

This produces a runtime `ReferenceError` because the inner `value` shadows the outer variable throughout the inner block, including before its initialization.

The outer variable is not used by the `console.log()` statement.

# 🗁 Redeclaration Rules

Variables declared with `let` and `const` cannot be redeclared in the same scope.

Invalid example:

```typescript
let username: string = "John";

let username: string = "David";
```

This also applies when mixing the two keywords:

```typescript
let age: number = 25;

const age: number = 30;
```

Both produce declaration errors.

However, declaring the same variable name in a different nested block is valid:

```typescript
let username: string = "John";

{
    let username: string = "David";

    console.log(username);
}

console.log(username);
```

Output:

```text
David
John
```

Each declaration belongs to a different lexical scope.

# 🗁 Using let and const in Loops

Both keywords can be used in loops.

**Using let:**

```typescript
for (let i: number = 1; i <= 5; i++) {
    console.log(i);
}
```

The loop variable `i` is scoped to the loop.

**Using const:**

```typescript
const numbers: number[] = [10, 20, 30];

for (const number of numbers) {
    console.log(number);
}
```

The loop variable is declared separately for each iteration and cannot be reassigned within that iteration.

# 🗁 const vs let in Closures

The difference between `let` and `const` can also affect how values are captured by functions.

Example using `let`:

```typescript
const callbacks: (() => void)[] = [];

for (let i: number = 0; i < 3; i++) {
    callbacks.push(() => console.log(i));
}

callbacks.forEach(callback => callback());
```

Output:

```text
0
1
2
```

Each iteration of the loop creates a new binding for `i`.

Example using `const`:

```typescript
const callbacks: (() => void)[] = [];

for (const number of [10, 20, 30]) {
    callbacks.push(() => console.log(number));
}

callbacks.forEach(callback => callback());
```

Output:

```text
10
20
30
```

Each iteration receives its own loop binding.

# 🗁 When to Use const

Use `const` when a variable does not need reassignment.

Example:

```typescript
const appName: string = "My Application";

const taxRate: number = 0.15;

const isActive: boolean = true;
```

Typical use cases include:

* Configuration values.
* Fixed references.
* Function declarations assigned to variables.
* Objects whose references should remain unchanged.
* Arrays whose references should remain unchanged.
* Imported bindings.

Using `const` communicates that the variable binding should not be reassigned.

# 🗁 When to Use let

Use `let` when a variable needs reassignment.

Example:

```typescript
let counter: number = 0;

counter++;

let total: number = 100;

total += 50;
```

Typical use cases include:

* Counters.
* Accumulators.
* Loop variables.
* Values that change during calculations.
* Variables updated by conditional logic.

# 🗁 Why Avoid var?

Although TypeScript supports `var`, modern TypeScript development generally favors `let` and `const`.

Consider:

```typescript
if (true) {
    var message: string = "Hello";
}

console.log(message);
```

This is valid because `var` is function-scoped or globally scoped rather than block-scoped.

Compare it with `let`:

```typescript
if (true) {
    let message: string = "Hello";
}

console.log(message);
```

This produces an error because `message` is block-scoped.

Using `let` and `const` makes variable visibility more predictable and reduces accidental redeclarations.

# 🗁 Practical Example

Consider a simple shopping cart calculation:

```typescript
const productName: string = "Laptop";
const unitPrice: number = 1200;
const taxRate: number = 0.10;

let quantity: number = 2;

quantity = 3;

const subtotal: number = unitPrice * quantity;
const tax: number = subtotal * taxRate;
const total: number = subtotal + tax;

console.log(`Product: ${productName}`);
console.log(`Quantity: ${quantity}`);
console.log(`Subtotal: $${subtotal}`);
console.log(`Tax: $${tax}`);
console.log(`Total: $${total}`);
```

Output:

```text
Product: Laptop
Quantity: 3
Subtotal: $3600
Tax: $360
Total: $3960
```

In this example:

* `const` is used for values that are not reassigned.
* `let` is used for `quantity` because its value changes.
* TypeScript checks the compatibility of assigned values.

# 🗁 Best Practices

* Prefer `const` by default.
* Use `let` only when reassignment is necessary.
* Avoid `var` in modern TypeScript applications.
* Remember that `const` prevents reassignment, not object mutation.
* Use `readonly` when properties or arrays should not be modified through a particular reference.
* Keep variables within the smallest practical scope.
* Avoid redeclaring variables in nested scopes when it reduces readability.
* Do not access `let` or `const` variables before initialization.
* Use meaningful variable names.
* Enable strict TypeScript compiler settings.

# 🗁 Summary

`const` and `let` are the preferred variable declaration keywords in modern TypeScript.

| Concept | let | const |
| --- | --- | --- |
| Reassignment | Allowed | Not allowed |
| Block-scoped | Yes | Yes |
| Must initialize immediately | No | Yes |
| Redeclaration in same scope | Not allowed | Not allowed |
| Temporal Dead Zone | Yes | Yes |
| Object mutation | Allowed | Allowed |
| Array mutation | Allowed | Allowed |

**Key takeaway:** Prefer `const` for values that do not need reassignment and use `let` when reassignment is required. This creates predictable variable behavior and improves code maintainability.
