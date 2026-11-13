# JavaScript var, let, and const

JavaScript provides three keywords for declaring variables: `var`, `let`, and `const`.

Although all three keywords create variable bindings, they differ in scope, hoisting behavior, initialization requirements, redeclaration rules, and reassignment capabilities.

Understanding these differences is essential for writing predictable JavaScript code and avoiding common errors.

# 🗁 Overview of var, let, and const

| Feature | `var` | `let` | `const` |
| --- | --- | --- | --- |
| Scope | Function or global | Block | Block |
| Reassignment | Allowed | Allowed | Not allowed |
| Redeclaration in same scope | Allowed | Not allowed | Not allowed |
| Declaration without initialization | Allowed | Allowed | Not allowed |
| Initialization required at declaration | No | No | Yes |
| Hoisted | Yes | Yes | Yes |
| Initial value before declaration | `undefined` | Uninitialized | Uninitialized |
| Temporal Dead Zone | No | Yes | Yes |
| Global object property in classic browser scripts | Yes, for top-level declarations | No | No |

# 🗁 var Keyword

The `var` keyword is the traditional way to declare variables in JavaScript.

It was the primary variable declaration mechanism before ES2015 (ES6) introduced `let` and `const`.

Syntax:

```javascript
var variableName = value;
```

Example:

```javascript
var username = "Muhammad";

console.log(username);
```

Output:

```text
Muhammad
```

**Reassignment**

Variables declared with `var` can be reassigned.

```javascript
var age = 25;

age = 30;

console.log(age);
```

Output:

```text
30
```

**Redeclaration**

Variables declared with `var` can be redeclared within the same scope.

```javascript
var username = "John";

var username = "Jane";

console.log(username);
```

Output:

```text
Jane
```

**Declaration without initialization**

A `var` variable can be declared without assigning an initial value.

```javascript
var score;

console.log(score);
```

Output:

```text
undefined
```

# 🗁 var Function Scope

Variables declared with `var` are function-scoped.

This means they are accessible throughout the function in which they are declared.

Example:

```javascript
function greet() {
    if (true) {
        var message = "Hello JavaScript";
    }

    console.log(message);
}

greet();
```

Output:

```text
Hello JavaScript
```

Although `message` was declared inside the `if` block, it is accessible outside that block because `var` does not create block scope.

However, it cannot be accessed outside the function.

```javascript
function greet() {
    var message = "Hello";
}

console.log(message);
```

This causes a `ReferenceError`.

**var inside loops**

The same behavior applies to loops.

```javascript
for (var i = 0; i < 3; i++) {
    console.log(i);
}

console.log(i);
```

Output:

```text
0
1
2
3
```

The variable `i` remains accessible after the loop because it is not block-scoped.

# 🗁 var Hoisting

Declarations made with `var` are hoisted to the top of their containing function or global scope.

They are initialized with `undefined` before the code executes.

Example:

```javascript
console.log(username);

var username = "John";
```

Output:

```text
undefined
```

Conceptually, JavaScript handles this similarly to:

```javascript
var username;

console.log(username);

username = "John";
```

Only the declaration is hoisted, not the assignment.

# 🗁 let Keyword

The `let` keyword was introduced in ES2015 (ES6) to provide block-scoped variable declarations.

Syntax:

```javascript
let variableName = value;
```

Example:

```javascript
let username = "Muhammad";

console.log(username);
```

Output:

```text
Muhammad
```

**Reassignment**

Variables declared with `let` can be reassigned.

```javascript
let age = 25;

age = 30;

console.log(age);
```

Output:

```text
30
```

**Redeclaration**

A variable declared with `let` cannot be redeclared within the same scope.

```javascript
let username = "John";

let username = "Jane";
```

This causes a `SyntaxError`.

However, a `let` variable can be updated without redeclaring it.

```javascript
let username = "John";

username = "Jane";

console.log(username);
```

Output:

```text
Jane
```

**Declaration without initialization**

A `let` variable can be declared without an initial value.

```javascript
let score;

console.log(score);
```

Output:

```text
undefined
```

# 🗁 let Block Scope

Variables declared with `let` are limited to their enclosing block.

A block is a section of code enclosed by curly braces (`{}`).

Example:

```javascript
if (true) {
    let message = "Hello JavaScript";

    console.log(message);
}
```

Output:

```text
Hello JavaScript
```

The variable cannot be accessed outside the block.

```javascript
if (true) {
    let message = "Hello JavaScript";
}

console.log(message);
```

This causes a `ReferenceError`.

**let inside loops**

```javascript
for (let i = 0; i < 3; i++) {
    console.log(i);
}

console.log(i);
```

Output:

```text
0
1
2
ReferenceError
```

The loop variable is not accessible outside the loop.

**let inside nested blocks**

```javascript
let username = "John";

{
    let username = "Jane";

    console.log(username);
}

console.log(username);
```

Output:

```text
Jane
John
```

Each block has its own lexical binding.

# 🗁 let Hoisting

Variables declared with `let` are hoisted, but they are not initialized until execution reaches their declaration.

Accessing them before initialization causes a `ReferenceError`.

Example:

```javascript
console.log(age);

let age = 25;
```

This causes a `ReferenceError`.

The period between entering the scope and evaluating the declaration is called the Temporal Dead Zone (TDZ).

# 🗁 const Keyword

The `const` keyword was introduced in ES2015 (ES6) to declare block-scoped bindings that cannot be reassigned.

Syntax:

```javascript
const variableName = value;
```

Example:

```javascript
const country = "Bangladesh";

console.log(country);
```

Output:

```text
Bangladesh
```

**Reassignment**

A `const` binding cannot be reassigned.

```javascript
const age = 25;

age = 30;
```

This causes a `TypeError`.

**Redeclaration**

A `const` variable cannot be redeclared within the same scope.

```javascript
const username = "John";

const username = "Jane";
```

This causes a `SyntaxError`.

**Initialization requirement**

A `const` declaration must be initialized when declared.

```javascript
const age = 25;
```

The following is invalid:

```javascript
const age;
```

This causes a `SyntaxError`.

# 🗁 const Block Scope

Variables declared with `const` are block-scoped, just like `let`.

Example:

```javascript
if (true) {
    const message = "Hello JavaScript";

    console.log(message);
}
```

Output:

```text
Hello JavaScript
```

The variable cannot be accessed outside the block.

```javascript
if (true) {
    const message = "Hello JavaScript";
}

console.log(message);
```

This causes a `ReferenceError`.

# 🗁 const Hoisting

Like `let`, `const` declarations are hoisted but remain uninitialized until their declaration is evaluated.

Accessing a `const` variable before initialization causes a `ReferenceError`.

Example:

```javascript
console.log(country);

const country = "Bangladesh";
```

This causes a `ReferenceError`.

A `const` declaration also has a Temporal Dead Zone.

# 🗁 const with Primitive Values

When a `const` variable stores a primitive value, the binding cannot be reassigned.

Example:

```javascript
const username = "John";

console.log(username);
```

The following assignment is invalid:

```javascript
const username = "John";

username = "Jane";
```

Primitive values include:

* String
* Number
* BigInt
* Boolean
* Undefined
* Null
* Symbol

# 🗁 const with Objects

When a `const` variable references an object, the reference cannot be reassigned, but the object's properties can be modified.

Example:

```javascript
const user = {
    name: "John",
    age: 25
};

user.age = 30;

user.email = "john@example.com";

console.log(user);
```

Output:

```text
{
    name: 'John',
    age: 30,
    email: 'john@example.com'
}
```

However, assigning another object to the same binding is not allowed.

```javascript
const user = {
    name: "John"
};

user = {
    name: "Jane"
};
```

This causes a `TypeError`.

# 🗁 const with Arrays

The same behavior applies to arrays.

A `const` array can be modified without reassigning the variable.

Example:

```javascript
const numbers = [10, 20, 30];

numbers.push(40);

numbers[0] = 100;

console.log(numbers);
```

Output:

```text
[100, 20, 30, 40]
```

However, reassigning the array is not allowed.

```javascript
const numbers = [10, 20, 30];

numbers = [40, 50, 60];
```

This causes a `TypeError`.

# 🗁 Object.freeze() with const

The `const` keyword prevents reassignment of a binding, not mutation of the referenced object.

If an object should not be modified, `Object.freeze()` can prevent changes to its own properties.

Example:

```javascript
const user = Object.freeze({
    name: "John",
    age: 25
});

user.age = 30;

console.log(user.age);
```

Output:

```text
25
```

`Object.freeze()` is shallow. Nested objects are not automatically frozen.

Example:

```javascript
const user = Object.freeze({
    name: "John",
    address: {
        city: "Dhaka"
    }
});

user.address.city = "Gazipur";

console.log(user.address.city);
```

Output:

```text
Gazipur
```

The nested `address` object remains mutable because it was not separately frozen.

# 🗁 Global Scope Differences

In classic browser scripts, top-level `var` declarations create properties on the global object.

Top-level `let` and `const` declarations do not.

Example:

```javascript
var firstName = "John";

let lastName = "Doe";

const age = 25;

console.log(globalThis.firstName);

console.log(globalThis.lastName);

console.log(globalThis.age);
```

In a classic browser script, the output is:

```text
John
undefined
undefined
```

This distinction does not mean `let` and `const` are inaccessible globally. They are accessible within their applicable script scope.

JavaScript modules have their own module scope, so top-level declarations in modules do not become properties of `globalThis`.

# 🗁 var vs let vs const Comparison

Consider the following examples.

**Reassignment comparison**

```javascript
var first = 10;

first = 20;

let second = 10;

second = 20;

const third = 10;

third = 20;
```

The `var` and `let` assignments are valid. The `const` reassignment throws a `TypeError`.

**Redeclaration comparison**

```javascript
var first = 10;
var first = 20;

let second = 10;
let second = 20;

const third = 10;
const third = 20;
```

The `var` redeclaration is valid. The `let` and `const` redeclarations in the same scope cause `SyntaxError` and prevent the script from executing.

**Scope comparison**

```javascript
function example() {
    if (true) {
        var first = 10;
        let second = 20;
        const third = 30;
    }

    console.log(first);

    // second and third are inaccessible here.
}

example();
```

Output:

```text
10
```

# 🗁 When to Use var, let, and const

Modern JavaScript development generally favors `const` and `let`.

**Use const when:**

* A variable does not need reassignment.
* Storing configuration values.
* Referencing objects or arrays whose binding should remain stable.
* Defining functions that are assigned to variables.
* Declaring values that should not point to another value.

Example:

```javascript
const API_URL = "https://api.example.com";

const user = {
    name: "John"
};

const getUser = () => {
    return user;
};
```

**Use let when:**

* A variable needs reassignment.
* A loop counter changes during iteration.
* A value is calculated or updated over time.
* A variable is conditionally assigned.

Example:

```javascript
let total = 0;

for (let i = 1; i <= 5; i++) {
    total += i;
}

console.log(total);
```

Output:

```text
15
```

**Use var when:**

* Maintaining older JavaScript code.
* Working with legacy environments or code that specifically depends on `var` behavior.
* Supporting a particular existing code convention.

For new JavaScript code, `var` is generally unnecessary.

# 🗁 Common Mistakes

**Using const without initialization**

Incorrect:

```javascript
const username;
```

Correct:

```javascript
const username = "John";
```

**Redeclaring let in the same scope**

Incorrect:

```javascript
let age = 25;

let age = 30;
```

Correct:

```javascript
let age = 25;

age = 30;
```

**Reassigning const**

Incorrect:

```javascript
const country = "Bangladesh";

country = "India";
```

Correct when reassignment is required:

```javascript
let country = "Bangladesh";

country = "India";
```

**Assuming const makes objects immutable**

Incorrect assumption:

```javascript
const user = {
    name: "John"
};

// This mutation is allowed.
user.name = "Jane";
```

To prevent ordinary property modifications, use `Object.freeze()` where appropriate.

**Assuming let is function-scoped**

Incorrect assumption:

```javascript
if (true) {
    let message = "Hello";
}

console.log(message);
```

The variable is block-scoped and cannot be accessed outside the block.

# 🗁 Best Practices

* Prefer `const` by default.
* Use `let` when reassignment is necessary.
* Avoid `var` in modern JavaScript applications.
* Keep variables within the smallest practical scope.
* Initialize variables close to where they are needed.
* Use descriptive variable names.
* Avoid unnecessary redeclarations.
* Remember that `const` prevents reassignment, not object mutation.
* Use `Object.freeze()` when shallow object immutability is required.
* Avoid accessing `let` and `const` variables before initialization.
* Use block-scoped declarations to reduce accidental variable leakage.

# 🗁 Practical Example

The following example demonstrates how `const` and `let` work together in a typical application.

```javascript
const products = [
    {
        name: "Keyboard",
        price: 50
    },
    {
        name: "Mouse",
        price: 25
    },
    {
        name: "Monitor",
        price: 200
    }
];

let total = 0;

for (const product of products) {
    total += product.price;
}

const discount = 25;

total -= discount;

console.log("Total:", total);
```

Output:

```text
Total: 250
```

Explanation:

* `products` uses `const` because the array binding is not reassigned.
* `product` uses `const` because each iteration receives a new binding that is not reassigned.
* `total` uses `let` because its value changes during calculation.
* `discount` uses `const` because its binding remains unchanged.

# 🗁 Key Takeaways

* JavaScript provides `var`, `let`, and `const` for declaring variables.
* `var` is function-scoped or globally scoped.
* `let` and `const` are block-scoped.
* `var` allows redeclaration and reassignment.
* `let` allows reassignment but not redeclaration in the same scope.
* `const` requires initialization and prevents reassignment.
* All three declarations are hoisted, but their initialization behavior differs.
* `let` and `const` have a Temporal Dead Zone.
* A `const` object or array can still be mutated.
* Top-level `var` behaves differently from top-level `let` and `const` in classic browser scripts.
* Prefer `const` by default and use `let` when reassignment is required.
