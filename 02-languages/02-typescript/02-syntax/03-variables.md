# TypeScript Variables

Variables are named storage locations used to hold data that a program can access and manipulate.

In TypeScript, variables are declared using `let`, `const`, or `var`.

TypeScript extends JavaScript variable declarations with static type annotations, allowing developers to specify the expected type of a variable.

Example:

```typescript
let username: string = "John";
let age: number = 25;

console.log(username);
console.log(age);
```

Output:

```text
John
25
```

# 🗁 What Is a Variable?

A variable is a named identifier that refers to a value.

Example:

```typescript
let message: string = "Hello, TypeScript!";

console.log(message);
```

In this example:

* `let` is the declaration keyword.
* `message` is the variable name.
* `string` is the declared type.
* `"Hello, TypeScript!"` is the assigned value.

# 🗁 Variable Declaration

TypeScript supports three main variable declaration keywords.

| Keyword | Scope | Reassignment | Redeclaration in Same Scope |
| --- | --- | --- | --- |
| `let` | Block scope | Allowed | Not allowed |
| `const` | Block scope | Not allowed | Not allowed |
| `var` | Function scope | Allowed | Allowed |

**Using let:**

```typescript
let age: number = 25;

age = 30;

console.log(age);
```

Output:

```text
30
```

**Using const:**

```typescript
const country: string = "Bangladesh";

console.log(country);
```

A `const` variable must be initialized when declared and cannot be reassigned.

**Using var:**

```typescript
var score: number = 100;

score = 150;

console.log(score);
```

Although `var` is supported, `let` and `const` are generally preferred in modern TypeScript development.

# 🗁 Variable Type Annotations

Type annotations specify the expected type of a variable.

Syntax:

```typescript
let variableName: Type = value;
```

Examples:

```typescript
let username: string = "John";

let age: number = 25;

let isActive: boolean = true;

let salary: number = 50000;
```

TypeScript checks whether the assigned value is compatible with the declared type.

Incorrect assignment:

```typescript
let age: number = 25;

age = "Twenty-five";
```

The compiler reports an error because a string cannot be assigned to a number variable.

# 🗁 Type Inference

TypeScript can automatically determine a variable's type from its initial value.

This feature is called type inference.

Example:

```typescript
let username = "John";

let age = 25;

let isActive = true;
```

TypeScript infers:

```text
username  → string
age       → number
isActive  → boolean
```

Therefore, explicit annotations are not always necessary.

For example:

```typescript
let age = 25;

age = 30; // Valid

age = "Thirty"; // Type error
```

The variable remains restricted to the inferred number type.

# 🗁 Variables with Different Data Types

TypeScript supports variables containing different kinds of values.

**String:**

```typescript
let username: string = "John";
```

**Number:**

```typescript
let price: number = 1500;
```

**Boolean:**

```typescript
let isLoggedIn: boolean = true;
```

**Array:**

```typescript
let numbers: number[] = [10, 20, 30, 40];
```

**Object:**

```typescript
let user: {
    id: number;
    name: string;
} = {
    id: 1,
    name: "John"
};
```

**Union type:**

```typescript
let userId: number | string;

userId = 101;
userId = "USER-101";
```

Union types allow a variable to hold values belonging to more than one specified type.

# 🗁 Multiple Variable Declarations

You can declare multiple variables in a single statement.

```typescript
let firstName: string = "John",
    lastName: string = "Smith",
    age: number = 25;
```

You can also use type inference:

```typescript
let firstName = "John",
    lastName = "Smith",
    age = 25;
```

However, declaring related variables separately often improves readability.

# 🗁 Variable Initialization

Initialization means assigning an initial value to a variable when it is declared.

Example:

```typescript
let username: string = "John";

const age: number = 25;
```

A `let` variable can also be declared without an initial value if its type is specified.

```typescript
let score: number;

score = 100;

console.log(score);
```

TypeScript recognizes that `score` must be assigned a number before it is used in a context requiring a definite value.

A `const` variable must be initialized immediately:

```typescript
const score: number = 100;
```

This is invalid:

```typescript
const score: number;
```

# 🗁 Variable Reassignment

Variables declared with `let` and `var` can be reassigned.

```typescript
let age: number = 25;

age = 30;

console.log(age);
```

Output:

```text
30
```

A `const` variable cannot be reassigned.

```typescript
const age: number = 25;

age = 30;
```

This produces a compiler error.

# 🗁 Variable Scope

Scope determines where a variable can be accessed in a program.

TypeScript follows JavaScript's lexical scoping rules.

The main variable scopes are:

* Global scope.
* Function scope.
* Block scope.

**Global scope:**

A variable declared outside functions and blocks is accessible throughout its applicable module or script scope.

```typescript
const appName: string = "My Application";

function displayAppName(): void {
    console.log(appName);
}

displayAppName();
```

**Function scope:**

Variables declared inside a function are accessible within that function.

```typescript
function calculate(): void {
    let total: number = 100;

    console.log(total);
}

calculate();
```

The variable `total` cannot be accessed outside the function.

**Block scope:**

Variables declared using `let` and `const` inside a block are accessible only within that block.

```typescript
if (true) {
    let message: string = "Hello";

    console.log(message);
}
```

The variable `message` cannot be accessed outside the `if` block.

# 🗁 Variable Shadowing

Variable shadowing occurs when a variable declared in an inner scope has the same name as a variable in an outer scope.

Example:

```typescript
let username: string = "John";

function displayUser(): void {
    let username: string = "David";

    console.log(username);
}

displayUser();

console.log(username);
```

Output:

```text
David
John
```

The inner variable temporarily shadows the outer variable within its scope.

# 🗁 Destructuring Variable Declarations

Destructuring allows values to be extracted from arrays and objects into variables.

**Array destructuring:**

```typescript
const colors: string[] = ["Red", "Green", "Blue"];

const [first, second, third] = colors;

console.log(first);
console.log(second);
console.log(third);
```

Output:

```text
Red
Green
Blue
```

**Object destructuring:**

```typescript
const user = {
    id: 1,
    name: "John",
    email: "john@example.com"
};

const { id, name, email } = user;

console.log(id);
console.log(name);
console.log(email);
```

Destructuring is particularly useful when working with objects, arrays, function parameters, and API responses.

# 🗁 Variable Naming Rules

TypeScript follows JavaScript identifier naming rules.

Valid variable names:

```typescript
let username: string = "John";

let userAge: number = 25;

let _privateValue: number = 100;

let $price: number = 500;

let user123: string = "David";
```

Invalid variable names:

```typescript
// let 123user = "John";
// let user-name = "John";
// let let = 100;
```

Variable naming rules:

* Names can contain letters, digits, underscores, and dollar signs.
* Names cannot begin with a digit.
* Spaces are not allowed.
* Reserved keywords cannot be used as identifiers.
* Variable names are case-sensitive.

For example:

```typescript
let username: string = "John";

let Username: string = "David";

console.log(username);
console.log(Username);
```

These are two different variables.

# 🗁 Type Safety and Variables

TypeScript checks variable assignments to help prevent incompatible values.

Example:

```typescript
let price: number = 1000;

price = 1500; // Valid

price = "Expensive"; // Type error
```

You can use union types when multiple types are expected.

```typescript
let price: number | string = 1000;

price = "One thousand";

console.log(price);
```

You can also use the `unknown` type when the actual type is not yet known.

```typescript
let value: unknown = "Hello";

value = 100;
value = true;
```

Unlike `any`, an `unknown` value must be narrowed or checked before most operations can be performed on it.

# 🗁 const with Objects and Arrays

The `const` keyword prevents reassignment of a variable, but it does not automatically make objects or arrays immutable.

Example:

```typescript
const user = {
    name: "John",
    age: 25
};

user.age = 30;

console.log(user.age);
```

Output:

```text
30
```

The object can be modified because `const` prevents reassignment of the variable binding, not modification of the object's properties.

However, this is invalid:

```typescript
const user = {
    name: "John"
};

user = {
    name: "David"
};
```

Arrays behave similarly:

```typescript
const numbers: number[] = [10, 20, 30];

numbers.push(40);

console.log(numbers);
```

Output:

```text
[10, 20, 30, 40]
```

To prevent reassignment and make the array type readonly:

```typescript
const numbers: readonly number[] = [10, 20, 30];
```

Now TypeScript prevents operations that modify the array through this readonly reference.

# 🗁 Best Practices

* Prefer `const` for variables that do not need reassignment.
* Use `let` when reassignment is necessary.
* Avoid `var` in modern TypeScript applications.
* Use meaningful and descriptive variable names.
* Follow consistent naming conventions, commonly `camelCase`.
* Let TypeScript infer obvious types.
* Add explicit type annotations when they improve clarity.
* Avoid unnecessary use of `any`.
* Use union types when a variable legitimately supports multiple types.
* Keep variable scope as narrow as practical.
* Use `readonly` when data should not be modified through a particular reference.

# 🗁 Summary

Variables are essential for storing and managing data in TypeScript programs.

You have learned about:

* Variable declaration and initialization.
* `let`, `const`, and `var`.
* Type annotations.
* Type inference.
* Different variable data types.
* Variable reassignment.
* Global, function, and block scope.
* Variable shadowing.
* Array and object destructuring.
* Variable naming rules.
* Type safety.
* `const` with objects and arrays.

Understanding variables is fundamental to writing reliable, maintainable, and type-safe TypeScript applications.
