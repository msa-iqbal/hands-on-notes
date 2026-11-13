# JavaScript Boolean

A **Boolean** is a primitive data type in JavaScript that represents one of two logical values:

* `true`
* `false`

Booleans are fundamental to decision-making and logical operations in JavaScript. They are commonly used in conditional statements, loops, comparisons, and application state management.

```javascript
const isLoggedIn = true;
const isAdmin = false;

console.log(isLoggedIn); // true
console.log(isAdmin);    // false
```

# 🗁 Boolean Values

JavaScript has exactly two Boolean primitive values.

**`true`**

Represents a logical condition that is satisfied or an affirmative state.

```javascript
const isActive = true;

if (isActive) {
    console.log("Account is active");
}
```

**`false`**

Represents a logical condition that is not satisfied or a negative state.

```javascript
const hasPermission = false;

if (!hasPermission) {
    console.log("Access denied");
}
```

Boolean values are case-sensitive. JavaScript recognizes `true` and `false`, not `True`, `False`, or `TRUE`.

```javascript
const valid = true;
const invalid = false;

// const value = True; // ReferenceError
```

# 🗁 Boolean Type

Use the `typeof` operator to determine whether a value is Boolean.

```javascript
const isAvailable = true;
const isDeleted = false;

console.log(typeof isAvailable); // "boolean"
console.log(typeof isDeleted);   // "boolean"
```

The primitive Boolean type is written as `boolean` in documentation and type annotations.

# 🗁 Boolean from Comparisons

Comparison operators evaluate expressions and return Boolean values.

```javascript
console.log(10 > 5);   // true
console.log(10 < 5);   // false
console.log(10 === 10); // true
console.log(10 !== 10); // false
```

Common comparison operators include:

| Operator | Meaning |
| --- | --- |
| `===` | Strictly equal |
| `!==` | Strictly not equal |
| `>` | Greater than |
| `<` | Less than |
| `>=` | Greater than or equal |
| `<=` | Less than or equal |

# 🗁 Boolean from Logical Operations

Logical operators combine or invert Boolean expressions.

**Logical AND (`&&`)**

Returns `true` when both operands are truthy. When both operands are Boolean values, the result follows this truth table:

| A | B | A && B |
| --- | --- | --- |
| true | true | true |
| true | false | false |
| false | true | false |
| false | false | false |

```javascript
const hasAccount = true;
const hasPassword = true;

const canLogin = hasAccount && hasPassword;

console.log(canLogin); // true
```

**Logical OR (`||`)**

Returns `true` when at least one operand is truthy.

| A | B | A \|\| B |
| --- | --- | --- |
| true | true | true |
| true | false | true |
| false | true | true |
| false | false | false |

```javascript
const hasEmail = false;
const hasPhone = true;

const canContact = hasEmail || hasPhone;

console.log(canContact); // true
```

**Logical NOT (`!`)**

Inverts the truthiness of a value and returns a Boolean.

```javascript
const isOnline = true;

console.log(!isOnline); // false
console.log(!!isOnline); // true
```

The double NOT operator (`!!`) is a common way to convert a value into its Boolean equivalent.

# 🗁 Boolean Constructor

JavaScript provides the `Boolean()` function to convert a value into a Boolean.

```javascript
console.log(Boolean(true));    // true
console.log(Boolean(false));   // false

console.log(Boolean(1));       // true
console.log(Boolean(0));       // false

console.log(Boolean("hello")); // true
console.log(Boolean(""));      // false

console.log(Boolean(null));      // false
console.log(Boolean(undefined)); // false
```

`Boolean()` performs Boolean conversion. It does not create a Boolean object when called as a function.

```javascript
const value = Boolean(1);

console.log(value);        // true
console.log(typeof value); // "boolean"
```

# 🗁 Truthy and Falsy Values

JavaScript evaluates values in Boolean contexts, such as `if` conditions and logical expressions.

Values that convert to `true` are called **truthy** values.

Values that convert to `false` are called **falsy** values.

**Falsy values**

The following values are falsy:

| Value | Description |
| --- | --- |
| `false` | Boolean false |
| `0` | Positive zero |
| `-0` | Negative zero |
| `0n` | BigInt zero |
| `""` | Empty string |
| `null` | Intentional absence of a value |
| `undefined` | Uninitialized or missing value |
| `NaN` | Not a Number |

```javascript
console.log(Boolean(false));     // false
console.log(Boolean(0));         // false
console.log(Boolean(-0));        // false
console.log(Boolean(0n));        // false
console.log(Boolean(""));        // false
console.log(Boolean(null));      // false
console.log(Boolean(undefined)); // false
console.log(Boolean(NaN));       // false
```

**Truthy values**

Almost every other JavaScript value is truthy, including:

```javascript
console.log(Boolean(true));      // true
console.log(Boolean(1));         // true
console.log(Boolean(-1));        // true
console.log(Boolean("hello"));   // true
console.log(Boolean("0"));       // true
console.log(Boolean("false"));   // true
console.log(Boolean([]));        // true
console.log(Boolean({}));        // true
console.log(Boolean(function () {})); // true
```

Empty arrays and empty objects are truthy, not falsy.

```javascript
const items = [];
const user = {};

if (items) {
    console.log("Array exists");
}

if (user) {
    console.log("Object exists");
}
```

Both conditions execute because arrays and objects are truthy, even when empty.

# 🗁 Boolean in Conditional Statements

Booleans are commonly used to control program execution.

```javascript
const isAuthenticated = true;

if (isAuthenticated) {
    console.log("Welcome!");
} else {
    console.log("Please log in.");
}
```

You can also use comparison expressions directly.

```javascript
const age = 20;

if (age >= 18) {
    console.log("Eligible");
} else {
    console.log("Not eligible");
}
```

# 🗁 Boolean in Loops

Boolean expressions determine whether a loop continues executing.

```javascript
let isRunning = true;
let count = 0;

while (isRunning) {
    count++;

    console.log(count);

    if (count === 3) {
        isRunning = false;
    }
}
```

The loop stops when `isRunning` becomes `false`.

# 🗁 Boolean Object

JavaScript also provides a `Boolean` constructor that can create Boolean wrapper objects when called with `new`.

```javascript
const primitiveBoolean = true;
const booleanObject = new Boolean(false);

console.log(typeof primitiveBoolean); // "boolean"
console.log(typeof booleanObject);    // "object"
```

A Boolean object is an object, not a Boolean primitive.

**Important:** Avoid creating Boolean objects with `new Boolean()`.

Even when the wrapped value is `false`, the object itself is truthy.

```javascript
const value = new Boolean(false);

if (value) {
    console.log("This executes");
}

console.log(Boolean(value)); // true
console.log(value.valueOf()); // false
```

Prefer primitive Boolean values instead:

```javascript
const isEnabled = false;

console.log(typeof isEnabled); // "boolean"
```

# 🗁 Boolean Best Practices

* Use primitive values (`true` and `false`) instead of Boolean wrapper objects.
* Use descriptive Boolean variable names such as `isActive`, `hasPermission`, and `canEdit`.
* Use strict equality (`===`) when comparing values.
* Avoid unnecessary Boolean conversions when the expression already returns a Boolean.
* Remember that empty arrays and objects are truthy.
* Use `Boolean()` or `!!` when explicit Boolean conversion improves readability.
* Avoid confusing strings such as `"false"` with the Boolean value `false`.

```javascript
const isAuthenticated = true;
const hasPermission = false;

const canAccessDashboard = isAuthenticated && hasPermission;

console.log(canAccessDashboard); // false
```

# 🗁 Common Mistakes

**Using a string instead of a Boolean**

```javascript
const isActive = "false";

if (isActive) {
    console.log("This executes");
}
```

The string `"false"` is truthy because it is a non-empty string.

Use an actual Boolean:

```javascript
const isActive = false;

if (isActive) {
    console.log("This does not execute");
}
```

**Assuming an empty array is falsy**

```javascript
const users = [];

if (users) {
    console.log("Users exist");
}
```

This condition executes because an empty array is truthy.

To check whether the array contains elements:

```javascript
if (users.length > 0) {
    console.log("Users exist");
} else {
    console.log("No users found");
}
```

**Using `new Boolean()`**

```javascript
const isEnabled = new Boolean(false);

console.log(Boolean(isEnabled)); // true
```

Use a primitive Boolean instead:

```javascript
const isEnabled = false;
```

# 🗁 Practical Examples

**User authentication**

```javascript
const isLoggedIn = true;

if (isLoggedIn) {
    console.log("Access granted");
} else {
    console.log("Please sign in");
}
```

**Age verification**

```javascript
const age = 21;
const isAdult = age >= 18;

console.log(isAdult); // true
```

**Permission checking**

```javascript
const isAuthenticated = true;
const hasAdminPermission = false;

const canManageUsers = isAuthenticated && hasAdminPermission;

console.log(canManageUsers); // false
```

**Checking whether a value exists**

```javascript
const username = "";

if (username) {
    console.log("Username provided");
} else {
    console.log("Username is required");
}
```

This checks truthiness, not merely whether a value is `null` or `undefined`. An empty string also fails the condition.

# 🗁 Key Takeaways

* Boolean is a JavaScript primitive data type.
* It has exactly two values: `true` and `false`.
* Comparison and logical expressions can produce Boolean results.
* `Boolean()` converts values into their Boolean equivalents.
* Truthy values convert to `true`; falsy values convert to `false`.
* Empty strings, zero, `null`, `undefined`, and `NaN` are falsy.
* Empty arrays and objects are truthy.
* Avoid `new Boolean()` because Boolean wrapper objects can produce unexpected conditional behavior.
* Booleans are essential for conditional logic, validation, state management, and application control flow.
