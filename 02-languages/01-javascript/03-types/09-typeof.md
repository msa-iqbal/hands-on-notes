# JavaScript typeof Operator

The **`typeof` operator** is a built-in JavaScript operator used to determine the type of a value.

It returns a string representing the type of the operand.

The `typeof` operator is particularly useful for debugging, type checking, input validation, and understanding JavaScript's dynamic type system.

```javascript
console.log(typeof "Hello");  // "string"
console.log(typeof 100);      // "number"
console.log(typeof true);     // "boolean"
console.log(typeof undefined); // "undefined"
```

# 🗁 Syntax

The `typeof` operator supports two syntax forms.

**Operator syntax**

```javascript
typeof operand
```

**Function-like syntax**

```javascript
typeof(operand)
```

Both forms produce the same result.

```javascript
const name = "John";

console.log(typeof name);   // "string"
console.log(typeof(name));  // "string"
```

The parentheses in the second form are optional and do not represent a function call.

# 🗁 typeof with Primitive Types

JavaScript has seven primitive types:

* String
* Number
* BigInt
* Boolean
* Undefined
* Symbol
* Null

The `typeof` operator returns the following values for primitive types.

| JavaScript Value | `typeof` Result |
| --- | --- |
| `"Hello"` | `"string"` |
| `100` | `"number"` |
| `10.5` | `"number"` |
| `true` | `"boolean"` |
| `false` | `"boolean"` |
| `undefined` | `"undefined"` |
| `10n` | `"bigint"` |
| `Symbol("id")` | `"symbol"` |
| `null` | `"object"` |

Example:

```javascript
console.log(typeof "JavaScript"); // "string"
console.log(typeof 42);           // "number"
console.log(typeof 3.14);         // "number"
console.log(typeof true);         // "boolean"
console.log(typeof undefined);    // "undefined"
console.log(typeof 100n);         // "bigint"
console.log(typeof Symbol("id")); // "symbol"
console.log(typeof null);          // "object"
```

# 🗁 typeof with Strings

The `typeof` operator returns `"string"` for string values.

```javascript
const name = "John";
const message = 'Hello World';
const template = `Welcome, ${name}`;

console.log(typeof name);     // "string"
console.log(typeof message);  // "string"
console.log(typeof template); // "string"
```

# 🗁 typeof with Numbers

JavaScript uses the `number` type for both integers and floating-point numbers.

```javascript
const age = 25;
const price = 99.99;
const negative = -10;

console.log(typeof age);      // "number"
console.log(typeof price);    // "number"
console.log(typeof negative); // "number"
```

Special numeric values also have the type `"number"`.

```javascript
console.log(typeof NaN);      // "number"
console.log(typeof Infinity); // "number"
console.log(typeof -Infinity); // "number"
```

Although `NaN` means "Not a Number", JavaScript classifies it as a numeric value.

Use `Number.isNaN()` to check whether a value is `NaN`.

```javascript
console.log(Number.isNaN(NaN)); // true
console.log(Number.isNaN(100)); // false
```

# 🗁 typeof with BigInt

The `typeof` operator returns `"bigint"` for BigInt values.

```javascript
const largeNumber = 12345678901234567890n;

console.log(typeof largeNumber); // "bigint"
```

BigInt represents integers beyond the safe integer range of the Number type.

# 🗁 typeof with Booleans

The `typeof` operator returns `"boolean"` for Boolean values.

```javascript
const isActive = true;
const isDeleted = false;

console.log(typeof isActive);  // "boolean"
console.log(typeof isDeleted); // "boolean"
```

# 🗁 typeof with Undefined

The `typeof` operator returns `"undefined"` when the operand has the value `undefined`.

```javascript
let username;

console.log(typeof username); // "undefined"

username = "John";

console.log(typeof username); // "string"
```

It also returns `"undefined"` for an undeclared identifier.

```javascript
console.log(typeof unknownVariable); // "undefined"
```

This is a special behavior: using `typeof` with an undeclared identifier does not normally throw a `ReferenceError`.

However, accessing that identifier directly does.

```javascript
// console.log(unknownVariable); // ReferenceError
```

There is an exception involving lexical declarations in the temporal dead zone.

```javascript
// console.log(typeof username); // ReferenceError

let username = "John";
```

A `let`, `const`, or `class` binding cannot be accessed before its declaration is initialized.

# 🗁 typeof with Null

The `typeof` operator returns `"object"` when the operand is `null`.

```javascript
const value = null;

console.log(typeof value); // "object"
```

This is a historical JavaScript behavior and is not a reliable way to identify `null`.

Use strict equality instead.

```javascript
if (value === null) {
    console.log("Value is null");
}
```

# 🗁 typeof with Symbols

The `typeof` operator returns `"symbol"` for Symbol values.

```javascript
const id = Symbol("user-id");

console.log(typeof id); // "symbol"
```

Symbols are unique primitive values commonly used as object property keys.

# 🗁 typeof with Objects

The `typeof` operator returns `"object"` for ordinary objects.

```javascript
const user = {
    name: "John",
    age: 25
};

console.log(typeof user); // "object"
```

It also returns `"object"` for arrays, dates, regular expressions, and many other built-in objects.

```javascript
const numbers = [10, 20, 30];
const date = new Date();
const pattern = /javascript/i;

console.log(typeof numbers); // "object"
console.log(typeof date);    // "object"
console.log(typeof pattern); // "object"
```

# 🗁 typeof with Arrays

Arrays are objects in JavaScript, so `typeof` returns `"object"`.

```javascript
const fruits = ["apple", "banana", "orange"];

console.log(typeof fruits); // "object"
```

To determine whether a value is an array, use `Array.isArray()`.

```javascript
console.log(Array.isArray(fruits)); // true

console.log(Array.isArray({})); // false
console.log(Array.isArray("Hello")); // false
```

# 🗁 typeof with Functions

The `typeof` operator returns `"function"` for callable function objects.

```javascript
function greet() {
    return "Hello";
}

console.log(typeof greet); // "function"
```

This also applies to arrow functions and class constructors.

```javascript
const add = (a, b) => a + b;

class User {}

console.log(typeof add);  // "function"
console.log(typeof User); // "function"
```

Although classes are declared using class syntax, their runtime values are callable constructor objects in the sense that `typeof` reports `"function"`. They must still be instantiated using `new`.

```javascript
// User(); // TypeError: Class constructor cannot be invoked without 'new'

const user = new User();
```

# 🗁 typeof with Wrapper Objects

JavaScript provides wrapper objects for some primitive types.

Using constructors with `new` creates objects rather than primitive values.

```javascript
const stringValue = new String("Hello");
const numberValue = new Number(100);
const booleanValue = new Boolean(true);

console.log(typeof stringValue); // "object"
console.log(typeof numberValue); // "object"
console.log(typeof booleanValue); // "object"
```

Prefer primitive values instead.

```javascript
const name = "Hello";
const age = 100;
const isActive = true;

console.log(typeof name);     // "string"
console.log(typeof age);      // "number"
console.log(typeof isActive); // "boolean"
```

# 🗁 typeof and Dynamic Typing

JavaScript is dynamically typed. A variable can hold values of different types during its lifetime.

The `typeof` operator can help identify the current value's type.

```javascript
let value = "Hello";

console.log(typeof value); // "string"

value = 100;

console.log(typeof value); // "number"

value = true;

console.log(typeof value); // "boolean"
```

The variable itself does not have a permanently fixed type. The current value determines the result.

# 🗁 typeof in Conditional Statements

You can use `typeof` to perform type-based conditional logic.

```javascript
const value = 100;

if (typeof value === "number") {
    console.log("Value is a number");
} else if (typeof value === "string") {
    console.log("Value is a string");
} else {
    console.log("Unknown type");
}
```

# 🗁 typeof for Function Parameters

JavaScript does not enforce parameter types automatically. You can use `typeof` to validate arguments.

```javascript
function calculateTotal(price, quantity) {
    if (
        typeof price !== "number" ||
        typeof quantity !== "number"
    ) {
        throw new TypeError("Price and quantity must be numbers");
    }

    return price * quantity;
}

console.log(calculateTotal(100, 3)); // 300
```

For production validation, consider additional checks such as `Number.isFinite()` when values must be finite numbers.

```javascript
function calculateTotal(price, quantity) {
    if (
        !Number.isFinite(price) ||
        !Number.isFinite(quantity)
    ) {
        throw new TypeError("Valid finite numbers are required");
    }

    return price * quantity;
}
```

# 🗁 typeof and Object Type Checking

The `typeof` operator alone cannot distinguish between many object types.

Use specialized checks when necessary.

| Value | Recommended Check |
| --- | --- |
| Array | `Array.isArray(value)` |
| Null | `value === null` |
| Date | `value instanceof Date` |
| Regular expression | `value instanceof RegExp` |
| Map | `value instanceof Map` |
| Set | `value instanceof Set` |
| Error | `value instanceof Error` |
| Plain object | A dedicated plain-object check |

Example:

```javascript
const values = [
    [],
    {},
    null,
    new Date(),
    new Map(),
    new Set()
];

for (const value of values) {
    console.log(typeof value);
}

// object
// object
// object
// object
// object
// object
```

Use the appropriate type-checking mechanism rather than relying only on `typeof`.

# 🗁 Common typeof Results

The following table summarizes common JavaScript values.

| Expression | Result |
| --- | --- |
| `typeof "Hello"` | `"string"` |
| `typeof 100` | `"number"` |
| `typeof 10.5` | `"number"` |
| `typeof NaN` | `"number"` |
| `typeof Infinity` | `"number"` |
| `typeof true` | `"boolean"` |
| `typeof undefined` | `"undefined"` |
| `typeof 10n` | `"bigint"` |
| `typeof Symbol()` | `"symbol"` |
| `typeof null` | `"object"` |
| `typeof {}` | `"object"` |
| `typeof []` | `"object"` |
| `typeof function () {}` | `"function"` |
| `typeof new Date()` | `"object"` |
| `typeof /abc/` | `"object"` |

# 🗁 Common Mistakes

**Assuming typeof null returns "null"**

```javascript
const value = null;

console.log(typeof value); // "object"
```

Use strict equality.

```javascript
console.log(value === null); // true
```

**Assuming typeof can identify arrays**

```javascript
const users = [];

console.log(typeof users); // "object"
```

Use `Array.isArray()`.

```javascript
console.log(Array.isArray(users)); // true
```

**Confusing NaN with a separate typeof result**

```javascript
console.log(typeof NaN); // "number"
```

Use `Number.isNaN()` to identify `NaN`.

```javascript
console.log(Number.isNaN(NaN)); // true
```

**Using typeof for all object types**

```javascript
const date = new Date();
const map = new Map();

console.log(typeof date); // "object"
console.log(typeof map);  // "object"
```

Use `instanceof` or other specialized checks when you need to identify specific built-in object types.

**Accessing a variable before initialization**

```javascript
// console.log(typeof value); // ReferenceError

let value = 100;
```

`typeof` does not bypass the temporal dead zone of lexical declarations.

# 🗁 Best Practices

* Use `typeof` for checking primitive types and functions.
* Use strict comparisons when checking the returned type string.
* Use `value === null` to identify `null`.
* Use `Array.isArray()` to identify arrays.
* Use `instanceof` for suitable built-in object type checks.
* Use `Number.isNaN()` to identify `NaN`.
* Use `Number.isFinite()` when validating finite numeric values.
* Do not assume every value with `typeof value === "object"` is an ordinary object.
* Remember that `typeof` returns `"function"` for callable function objects.
* Account for the temporal dead zone when working with `let`, `const`, and `class`.

# 🗁 Key Takeaways

* `typeof` is a JavaScript operator that returns a string representing a value's type.
* It is useful for checking primitive values and callable functions.
* `typeof null` returns `"object"` because of historical JavaScript behavior.
* Arrays, dates, regular expressions, maps, and sets return `"object"`.
* Functions return `"function"`.
* JavaScript is dynamically typed, so the result depends on the current value.
* `Array.isArray()` is the recommended way to identify arrays.
* Strict equality should be used to check for `null`.
* Specialized validation methods provide more reliable checks for specific built-in types.
