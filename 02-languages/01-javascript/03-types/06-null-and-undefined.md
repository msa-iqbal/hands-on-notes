# JavaScript Null and Undefined

JavaScript has two primitive values that represent the absence of a meaningful value:

* `null`
* `undefined`

Although both represent an absence of value, they have different meanings and are used in different situations.

* **`undefined`** generally indicates that a value has not been assigned or is unavailable.
* **`null`** generally represents an intentional absence of a value.

Understanding the distinction is important for writing predictable JavaScript applications, handling API responses, validating data, and preventing runtime errors.

```javascript
let username;

console.log(username); // undefined

const selectedUser = null;

console.log(selectedUser); // null
```

# 🗁 Undefined

`undefined` is a primitive value automatically assigned to variables that have been declared but not initialized.

```javascript
let name;

console.log(name); // undefined
console.log(typeof name); // "undefined"
```

You can also explicitly assign `undefined` to a variable.

```javascript
let age = 25;

age = undefined;

console.log(age); // undefined
```

However, explicitly assigning `undefined` is generally discouraged when `null` would better communicate an intentional absence of value.

# 🗁 Common Sources of Undefined

**Uninitialized variables**

Variables declared with `let` or `var` receive `undefined` when no initial value is provided.

```javascript
let username;
var email;

console.log(username); // undefined
console.log(email);    // undefined
```

Variables declared with `const` must be initialized.

```javascript
// const username; // SyntaxError
```

**Missing object properties**

Accessing a property that does not exist returns `undefined`.

```javascript
const user = {
    name: "John",
    age: 25
};

console.log(user.name);     // "John"
console.log(user.email);    // undefined
console.log(user.address);  // undefined
```

**Missing function arguments**

If a function parameter receives no argument, its value is `undefined`, unless a default parameter is provided.

```javascript
function greet(name) {
    console.log(name);
}

greet("John"); // "John"
greet();       // undefined
```

**Functions without a return statement**

A function that does not explicitly return a value produces `undefined`.

```javascript
function calculate() {
    const result = 10 + 20;
}

const value = calculate();

console.log(value); // undefined
```

**Explicit return without a value**

```javascript
function getValue() {
    return;
}

console.log(getValue()); // undefined
```

**Array elements outside the array's existing indexes**

Accessing an array index that does not contain an element returns `undefined`.

```javascript
const colors = ["red", "green", "blue"];

console.log(colors[0]); // "red"
console.log(colors[5]); // undefined
```

An array can also contain an explicit `undefined` value.

```javascript
const values = [10, undefined, 30];

console.log(values[1]); // undefined
```

# 🗁 Null

`null` is a primitive value that represents the intentional absence of an object or value.

Unlike `undefined`, which often appears automatically, `null` is usually assigned explicitly by a developer.

```javascript
const selectedUser = null;

console.log(selectedUser); // null
console.log(typeof selectedUser); // "object"
```

The `typeof null` result is a historical JavaScript behavior. It does not mean that `null` is an object.

# 🗁 Common Uses of Null

**Resetting a variable**

```javascript
let selectedProduct = {
    id: 101,
    name: "Laptop"
};

selectedProduct = null;

console.log(selectedProduct); // null
```

**Representing missing data**

```javascript
const user = {
    id: 1,
    name: "John",
    profileImage: null
};

console.log(user.profileImage); // null
```

Here, `null` communicates that the user has no profile image assigned.

**Clearing application state**

```javascript
let currentUser = {
    id: 1,
    name: "John"
};

function logout() {
    currentUser = null;
}

logout();

console.log(currentUser); // null
```

**Representing an optional result**

```javascript
function findUser(id) {
    const users = [
        { id: 1, name: "John" },
        { id: 2, name: "Sarah" }
    ];

    return users.find(user => user.id === id) ?? null;
}

console.log(findUser(1));   // { id: 1, name: "John" }
console.log(findUser(100)); // null
```

# 🗁 Differences Between Null and Undefined

| Feature | `null` | `undefined` |
| --- | --- | --- |
| Meaning | Intentional absence of value | Missing or uninitialized value |
| Type | Primitive | Primitive |
| `typeof` result | `"object"` | `"undefined"` |
| Default variable value | No | Yes, for uninitialized `let` and `var` |
| Explicit assignment | Common | Possible, but usually avoided |
| Boolean conversion | `false` | `false` |
| Loose equality | Equal to `undefined` | Equal to `null` |
| Strict equality | Not equal to `undefined` | Not equal to `null` |

```javascript
console.log(typeof null);      // "object"
console.log(typeof undefined); // "undefined"

console.log(Boolean(null));      // false
console.log(Boolean(undefined)); // false

console.log(null == undefined);  // true
console.log(null === undefined); // false
```

# 🗁 Equality Comparison

JavaScript provides two commonly used equality operators.

**Loose equality (`==`)**

Loose equality performs type conversion when comparing values.

```javascript
console.log(null == undefined); // true
console.log(null == 0);         // false
console.log(undefined == 0);    // false
```

One important special case is that `null` and `undefined` are loosely equal to each other.

**Strict equality (`===`)**

Strict equality compares values without performing type conversion.

```javascript
console.log(null === undefined); // false
console.log(null === null);      // true
console.log(undefined === undefined); // true
```

Prefer strict equality for predictable comparisons.

# 🗁 Checking for Null and Undefined

**Strict equality checks**

Use strict equality when you need to distinguish between the two values.

```javascript
const value = null;

console.log(value === null);      // true
console.log(value === undefined); // false
```

**Checking for undefined**

```javascript
let username;

if (username === undefined) {
    console.log("Username is undefined");
}
```

**Checking for null**

```javascript
let selectedUser = null;

if (selectedUser === null) {
    console.log("No user selected");
}
```

**Checking for both null and undefined**

Loose equality can be useful when both values should be treated as equivalent.

```javascript
let value;

if (value == null) {
    console.log("Value is null or undefined");
}
```

This is one of the few cases where using `==` can be intentional and useful.

**Using `typeof`**

```javascript
let value;

if (typeof value === "undefined") {
    console.log("Value is undefined");
}
```

The `typeof` operator is especially useful when checking an identifier that may not have been declared.

```javascript
if (typeof optionalVariable === "undefined") {
    console.log("Variable is unavailable");
}
```

Directly accessing an undeclared identifier would throw a `ReferenceError`.

# 🗁 Nullish Coalescing Operator

The nullish coalescing operator (`??`) returns the right-hand value when the left-hand value is either `null` or `undefined`.

```javascript
const username = null;

const displayName = username ?? "Guest";

console.log(displayName); // "Guest"
```

It preserves other falsy values such as `0`, `false`, and `""`.

```javascript
console.log(0 ?? 100);       // 0
console.log(false ?? true);  // false
console.log("" ?? "Default"); // ""
console.log(null ?? "Default"); // "Default"
console.log(undefined ?? "Default"); // "Default"
```

This makes `??` particularly useful when providing fallback values without accidentally replacing valid falsy values.

# 🗁 Logical OR vs Nullish Coalescing

The logical OR operator (`||`) returns the right-hand operand when the left-hand operand is falsy.

The nullish coalescing operator (`??`) returns the right-hand operand only when the left-hand operand is `null` or `undefined`.

```javascript
const count = 0;

console.log(count || 10); // 10
console.log(count ?? 10); // 0
```

Comparison:

| Value | `value || "Default"` | `value ?? "Default"` |
|---|---|---|
| `null` | `"Default"` | `"Default"` |
| `undefined` | `"Default"` | `"Default"` |
| `0` | `"Default"` | `0` |
| `false` | `"Default"` | `false` |
| `""` | `"Default"` | `""` |
| `"Hello"` | `"Hello"` | `"Hello"` |

Use `||` when every falsy value should trigger a fallback.

Use `??` when only missing values (`null` and `undefined`) should trigger a fallback.

# 🗁 Optional Chaining

Optional chaining (`?.`) safely accesses properties or methods when the value before the operator is `null` or `undefined`.

Without optional chaining, accessing a property on `null` or `undefined` causes a `TypeError`.

```javascript
const user = null;

// console.log(user.name); // TypeError

console.log(user?.name); // undefined
```

It is particularly useful when working with nested objects.

```javascript
const user = {
    profile: {
        address: {
            city: "Dhaka"
        }
    }
};

console.log(user.profile?.address?.city); // "Dhaka"
console.log(user.profile?.contact?.phone); // undefined
```

Optional chaining can also be used with method calls and array access.

```javascript
const user = {
    greet() {
        return "Hello!";
    }
};

console.log(user.greet?.()); // "Hello!"

const users = null;

console.log(users?.[0]); // undefined
```

Optional chaining only protects against `null` and `undefined`. It does not automatically validate other incorrect values or guarantee that a property is callable.

# 🗁 Nullish Assignment Operator

The nullish assignment operator (`??=`) assigns a value only when the existing variable or property is `null` or `undefined`.

```javascript
let username = null;

username ??= "Guest";

console.log(username); // "Guest"
```

It does not overwrite valid falsy values.

```javascript
let count = 0;
let isActive = false;
let title = "";

count ??= 10;
isActive ??= true;
title ??= "Default";

console.log(count);    // 0
console.log(isActive); // false
console.log(title);    // ""
```

This operator is useful for initializing optional values without overwriting existing data.

# 🗁 JSON Behavior

JavaScript treats `undefined` and `null` differently during JSON serialization.

**Object properties**

Properties containing `undefined` are omitted by `JSON.stringify()`.

Properties containing `null` are preserved.

```javascript
const user = {
    name: "John",
    age: undefined,
    email: null
};

console.log(JSON.stringify(user));

// {"name":"John","email":null}
```

**Array elements**

An `undefined` array element is serialized as `null`.

```javascript
const values = [10, undefined, 30, null];

console.log(JSON.stringify(values));

// [10,null,30,null]
```

**Top-level undefined**

Serializing `undefined` directly returns `undefined`, rather than a JSON string.

```javascript
console.log(JSON.stringify(undefined)); // undefined
console.log(JSON.stringify(null));      // "null"
```

These differences are important when sending data to APIs or storing application state.

# 🗁 Common Mistakes

**Confusing null with undefined**

```javascript
const user = null;

console.log(user === undefined); // false
```

They represent different values, even though `user == null` evaluates to `true`.

**Assuming typeof null returns null**

```javascript
console.log(typeof null); // "object"
```

Use strict equality to identify `null`.

```javascript
if (value === null) {
    console.log("Value is null");
}
```

**Accessing properties of null or undefined**

```javascript
const user = null;

// console.log(user.name); // TypeError
```

Use optional chaining when missing values are expected.

```javascript
console.log(user?.name); // undefined
```

**Using logical OR when zero is valid**

```javascript
const quantity = 0;

const result = quantity || 1;

console.log(result); // 1
```

Use nullish coalescing when zero should be preserved.

```javascript
const result = quantity ?? 1;

console.log(result); // 0
```

**Assuming JSON preserves undefined**

```javascript
const data = {
    value: undefined
};

console.log(JSON.stringify(data)); // "{}"
```

Use `null` when an explicit missing value must be preserved in JSON.

# 🗁 Practical Examples

**Handling optional API data**

```javascript
const response = {
    id: 101,
    name: "John",
    profile: null
};

const city = response.profile?.address?.city ?? "Unknown";

console.log(city); // "Unknown"
```

**Setting default configuration values**

```javascript
const config = {
    timeout: 0,
    retries: null
};

config.timeout ??= 5000;
config.retries ??= 3;

console.log(config);

// {
//     timeout: 0,
//     retries: 3
// }
```

**Checking function arguments**

```javascript
function createUser(name, email) {
    const user = {
        name: name ?? "Anonymous",
        email: email ?? null
    };

    return user;
}

console.log(createUser("John"));

// {
//     name: "John",
//     email: null
// }
```

**Finding an item with a fallback**

```javascript
const products = [
    { id: 1, name: "Laptop" },
    { id: 2, name: "Keyboard" }
];

function getProduct(id) {
    return products.find(product => product.id === id) ?? null;
}

console.log(getProduct(1));
console.log(getProduct(100)); // null
```

# 🗁 Best Practices

* Use `undefined` to represent values that are missing or not initialized.
* Use `null` when you intentionally want to represent the absence of a value.
* Prefer strict equality (`===`) for explicit comparisons.
* Use `value == null` intentionally when checking for both `null` and `undefined`.
* Use optional chaining (`?.`) when accessing potentially missing nested properties.
* Use nullish coalescing (`??`) when providing defaults only for `null` and `undefined`.
* Use nullish assignment (`??=`) when initializing missing values.
* Be aware of how `JSON.stringify()` handles `undefined`.
* Establish consistent conventions for representing missing values in application data and API contracts.

# 🗁 Key Takeaways

* `null` and `undefined` are primitive JavaScript values.
* `undefined` commonly represents an uninitialized or missing value.
* `null` represents an intentional absence of value.
* `typeof null` returns `"object"` because of a historical JavaScript behavior.
* `typeof undefined` returns `"undefined"`.
* Both values are falsy.
* `null == undefined` returns `true`, but `null === undefined` returns `false`.
* The nullish coalescing operator (`??`) handles both `null` and `undefined`.
* Optional chaining (`?.`) safely accesses potentially missing properties.
* JSON serialization omits `undefined` object properties but preserves `null` values.
