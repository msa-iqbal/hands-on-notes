# JavaScript Primitive Types

JavaScript is a dynamically typed programming language. This means variables do not have fixed types; their values determine their types at runtime.

JavaScript has seven primitive data types:

* String
* Number
* BigInt
* Boolean
* Undefined
* Null
* Symbol

Primitive values are immutable, meaning their actual values cannot be modified after creation.

When a primitive value is assigned to another variable, the value is copied.

# 🗁 What Are Primitive Types?

Primitive types represent simple, single values rather than collections of properties and methods.

**Example:**

```javascript
const name = "Alice";
const age = 25;
const isActive = true;

console.log(typeof name);     // string
console.log(typeof age);      // number
console.log(typeof isActive); // boolean
```

Each variable holds a primitive value.

# 🗁 JavaScript Primitive Types Overview

| Primitive Type | Description | Example |
| --- | --- | --- |
| String | Represents textual data | `"Hello"` |
| Number | Represents integers and floating-point numbers | `42`, `3.14` |
| BigInt | Represents large integers | `123n` |
| Boolean | Represents logical values | `true`, `false` |
| Undefined | Represents an uninitialized or missing value | `undefined` |
| Null | Represents an intentional absence of a value | `null` |
| Symbol | Represents a unique identifier | `Symbol("id")` |

# 🗁 String

A string represents textual data.

Strings can be created using single quotes, double quotes, or template literals.

```javascript
const firstName = "Alice";
const lastName = 'Smith';
const message = `Hello, ${firstName}`;

console.log(firstName);
console.log(lastName);
console.log(message);
```

Strings are immutable.

```javascript
let text = "Hello";

text[0] = "Y";

console.log(text); // Hello
```

Changing an individual character does not modify the original string.

More details are covered in `02-string.md`.

# 🗁 Number

The Number type represents both integers and floating-point numbers.

```javascript
const age = 25;
const price = 99.99;
const temperature = -10;

console.log(typeof age);         // number
console.log(typeof price);       // number
console.log(typeof temperature); // number
```

JavaScript also supports special numeric values:

```javascript
console.log(Infinity);
console.log(-Infinity);
console.log(NaN);
```

Most ordinary JavaScript numbers use IEEE 754 double-precision floating-point representation.

More details are covered in `03-number.md`.

# 🗁 BigInt

BigInt represents integers larger than the safe integer range of the Number type.

A BigInt can be created by appending `n` to an integer literal.

```javascript
const bigNumber = 123456789012345678901234567890n;

console.log(bigNumber);
console.log(typeof bigNumber); // bigint
```

BigInt and Number cannot be directly mixed in arithmetic operations.

```javascript
const number = 10;
const bigNumber = 20n;

// Throws TypeError
// console.log(number + bigNumber);
```

Convert values explicitly when necessary:

```javascript
const number = 10;
const bigNumber = 20n;

console.log(BigInt(number) + bigNumber); // 30n
```

More details are covered in `04-bigint.md`.

# 🗁 Boolean

The Boolean type represents logical values.

It has only two possible values:

* `true`
* `false`

```javascript
const isLoggedIn = true;
const hasPermission = false;

console.log(typeof isLoggedIn); // boolean
console.log(typeof hasPermission); // boolean
```

Booleans are commonly used in conditional statements and logical operations.

```javascript
const age = 20;
const canVote = age >= 18;

console.log(canVote); // true
```

More details are covered in `05-boolean.md`.

# 🗁 Undefined

Undefined represents a variable that has been declared but has not been assigned a value.

```javascript
let username;

console.log(username);        // undefined
console.log(typeof username); // undefined
```

JavaScript also returns `undefined` when accessing a missing object property:

```javascript
const user = {
    name: "Alice"
};

console.log(user.email); // undefined
```

A function without an explicit return value also returns `undefined`:

```javascript
function greet() {
    console.log("Hello");
}

const result = greet();

console.log(result); // undefined
```

More details are covered in `06-null-and-undefined.md`.

# 🗁 Null

Null represents an intentional absence of a value.

It is commonly used when a variable should explicitly contain no object or value.

```javascript
const selectedUser = null;

console.log(selectedUser);        // null
console.log(typeof selectedUser); // object
```

The `typeof null` result is `"object"` because of a historical JavaScript behavior. Despite this result, `null` is a primitive value.

More details are covered in `06-null-and-undefined.md`.

# 🗁 Symbol

Symbol represents a unique primitive value.

Symbols are commonly used as unique object property keys.

```javascript
const id = Symbol("id");

console.log(id);
console.log(typeof id); // symbol
```

Even symbols created with the same description are different:

```javascript
const firstId = Symbol("userId");
const secondId = Symbol("userId");

console.log(firstId === secondId); // false
```

More details are covered in `07-symbol.md`.

# 🗁 Primitive Value Assignment

Primitive values are copied when assigned to another variable.

Changing the second variable does not affect the first variable.

```javascript
let firstNumber = 10;

let secondNumber = firstNumber;

secondNumber = 20;

console.log(firstNumber);  // 10
console.log(secondNumber); // 20
```

The variables contain independent primitive values.

# 🗁 Primitive Immutability

Primitive values cannot be modified in place.

Operations on primitive values produce new values rather than changing the original value.

**Example:**

```javascript
let message = "Hello";

message.toUpperCase();

console.log(message); // Hello
```

The `toUpperCase()` method returns a new string. It does not modify the original string.

To store the result:

```javascript
let message = "Hello";

message = message.toUpperCase();

console.log(message); // HELLO
```

The variable now references the newly returned string value.

# 🗁 Primitive Types vs Reference Types

JavaScript values are broadly classified into primitive values and objects.

Objects are reference types, while the seven types discussed here are primitives.

| Feature | Primitive Types | Reference Types |
| --- | --- | --- |
| Examples | String, Number, Boolean | Object, Array, Function |
| Mutability | Immutable values | Objects can be mutable |
| Assignment | Copies the value | Copies the reference |
| Comparison | Usually compares values | Compares object identity |
| Storage semantics | Primitive value semantics | Object reference semantics |

**Primitive Assignment:**

```javascript
let first = 10;
let second = first;

second = 20;

console.log(first);  // 10
console.log(second); // 20
```

**Object Assignment:**

```javascript
const firstUser = {
    name: "Alice"
};

const secondUser = firstUser;

secondUser.name = "Bob";

console.log(firstUser.name);  // Bob
console.log(secondUser.name); // Bob
```

Both variables refer to the same object.

# 🗁 Checking Primitive Types

The `typeof` operator can identify the type of most primitive values.

```javascript
console.log(typeof "Hello");       // string
console.log(typeof 100);           // number
console.log(typeof 100n);          // bigint
console.log(typeof true);          // boolean
console.log(typeof undefined);     // undefined
console.log(typeof Symbol("id"));  // symbol
console.log(typeof null);          // object
```

Remember that `typeof null` is a historical exception.

For an explicit null check, use strict equality:

```javascript
const value = null;

console.log(value === null); // true
```

# 🗁 Best Practices

* Understand the difference between primitive values and objects.
* Use `const` by default when a variable does not need reassignment.
* Use `let` when reassignment is required.
* Use BigInt when working with integers outside the safe integer range of Number.
* Avoid mixing BigInt and Number in arithmetic operations without explicit conversion.
* Use `null` when intentionally representing the absence of a value.
* Understand that `undefined` commonly indicates an unassigned or missing value.
* Remember that primitive values are immutable.
* Use strict equality (`===`) when comparing values.
* Use `typeof` to inspect primitive types, while accounting for the `null` exception.

# 🗁 Summary

* JavaScript has seven primitive data types.
* Primitive types are String, Number, BigInt, Boolean, Undefined, Null, and Symbol.
* Primitive values are immutable.
* Assigning a primitive value copies that value.
* BigInt supports integers beyond Number's safe integer range.
* `null` represents intentional absence, while `undefined` commonly represents a missing or unassigned value.
* Symbols provide unique primitive identifiers.
* Objects, arrays, and functions are reference types.
* The `typeof` operator identifies most primitive types, but `typeof null` returns `"object"`.

# 🗁 Practice

**Exercise: Identify Data Types**

Determine the type of each value:

```javascript
const values = [
    "JavaScript",
    2026,
    99.99,
    12345678901234567890n,
    true,
    false,
    undefined,
    null,
    Symbol("id")
];

values.forEach(value => {
    console.log(typeof value);
});
```

**Exercise: Primitive Assignment**

Predict the output:

```javascript
let first = "Hello";
let second = first;

second = "World";

console.log(first);
console.log(second);
```

Explain why changing `second` does not change `first`.

**Exercise: Primitive Immutability**

Predict the output:

```javascript
let text = "javascript";

text.toUpperCase();

console.log(text);

text = text.toUpperCase();

console.log(text);
```

Explain the difference between the two operations.

**Exercise: BigInt and Number**

Identify the problem with the following code and fix it:

```javascript
const first = 100;
const second = 200n;

console.log(first + second);
```

**Exercise: Null and Undefined**

Explain the difference between these declarations:

```javascript
let first;
let second = undefined;
let third = null;

console.log(first);
console.log(second);
console.log(third);
```

**Exercise: Primitive vs Reference**

Predict the output:

```javascript
let firstNumber = 10;
let secondNumber = firstNumber;

secondNumber = 50;

const firstUser = {
    name: "Alice"
};

const secondUser = firstUser;

secondUser.name = "Bob";

console.log(firstNumber);
console.log(secondNumber);
console.log(firstUser.name);
console.log(secondUser.name);
```

Explain why primitive assignment and object assignment behave differently.
