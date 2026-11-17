# JSON.stringify()

`JSON.stringify()` is a built-in JavaScript method used to convert a JavaScript value into a JSON-formatted string.

It is commonly used for:

* Sending data to REST APIs.
* Storing objects in Local Storage.
* Saving application settings.
* Serializing data for transmission.
* Creating JSON configuration files.
* Converting JavaScript objects into text.

Example:

```javascript
const user = {
  name: "Alice",
  age: 25,
  isActive: true
};

const jsonString = JSON.stringify(user);

console.log(jsonString);
```

Output:

```json
{"name":"Alice","age":25,"isActive":true}
```

The resulting value is a string, not a JavaScript object.

# 🗁 Syntax

```javascript
JSON.stringify(value, replacer, space);
```

Parameters:

| Parameter | Description |
| --- | --- |
| `value` | The JavaScript value to serialize |
| `replacer` | Optional function or array used to transform or select properties |
| `space` | Optional indentation configuration for formatting |

Return value:

* A JSON-formatted string when serialization succeeds.
* `undefined` for certain unsupported top-level values, such as functions, symbols, and `undefined`.

Some values, such as circular references and BigInt values, cause serialization to throw an error unless handled appropriately.

# 🗁 Converting JavaScript Objects into JSON

The most common use of `JSON.stringify()` is converting an object into a JSON string.

```javascript
const user = {
  id: 101,
  name: "Alice",
  email: "alice@example.com"
};

const jsonString = JSON.stringify(user);

console.log(typeof jsonString); // string
console.log(jsonString);
```

Output:

```text
string
{"id":101,"name":"Alice","email":"alice@example.com"}
```

The resulting string can be transmitted, stored, or written to a file.

# 🗁 Converting Arrays into JSON

`JSON.stringify()` also supports JavaScript arrays.

```javascript
const languages = [
  "JavaScript",
  "TypeScript",
  "Python"
];

const jsonString = JSON.stringify(languages);

console.log(jsonString);
```

Output:

```json
["JavaScript","TypeScript","Python"]
```

Nested arrays and objects are also serialized.

```javascript
const users = [
  {
    id: 1,
    name: "Alice"
  },
  {
    id: 2,
    name: "Bob"
  }
];

console.log(JSON.stringify(users));
```

# 🗁 Converting Primitive Values

Primitive JavaScript values can also be serialized.

String:

```javascript
JSON.stringify("Hello World");
```

Output:

```json
"Hello World"
```

Number:

```javascript
JSON.stringify(100);
```

Output:

```text
100
```

Boolean:

```javascript
JSON.stringify(true);
```

Output:

```text
true
```

Null:

```javascript
JSON.stringify(null);
```

Output:

```text
null
```

# 🗁 Formatting JSON with the Space Parameter

The third parameter controls the formatting of the resulting JSON string.

Without formatting:

```javascript
const user = {
  name: "Alice",
  age: 25,
  address: {
    city: "Dhaka",
    country: "Bangladesh"
  }
};

console.log(JSON.stringify(user));
```

Formatted using two spaces:

```javascript
console.log(JSON.stringify(user, null, 2));
```

Output:

```json
{
  "name": "Alice",
  "age": 25,
  "address": {
    "city": "Dhaka",
    "country": "Bangladesh"
  }
}
```

The `space` parameter accepts:

* A number from `0` to `10`, representing indentation spaces.
* A string, used as the indentation characters.

Example using four spaces:

```javascript
JSON.stringify(user, null, 4);
```

Example using a tab:

```javascript
JSON.stringify(user, null, "\t");
```

The `space` parameter improves readability but does not change the underlying data.

# 🗁 Using the Replacer Function

The second parameter can be a function that transforms or filters values during serialization.

Syntax:

```javascript
JSON.stringify(value, (key, value) => {
  // Transform or filter value
});
```

The replacer function receives:

* `key`: The current property name.
* `value`: The current property value.

It is called for nested properties and the root value.

Example:

```javascript
const user = {
  name: "Alice",
  age: 25,
  password: "secret"
};

const jsonString = JSON.stringify(user, (key, value) => {
  if (key === "password") {
    return undefined;
  }

  return value;
});

console.log(jsonString);
```

Output:

```json
{"name":"Alice","age":25}
```

Returning `undefined` excludes an object property from the resulting JSON.

# 🗁 Transforming Values with a Replacer

A replacer can also modify values before serialization.

Example:

```javascript
const product = {
  name: "Laptop",
  price: 1200,
  discount: 10
};

const jsonString = JSON.stringify(product, (key, value) => {
  if (key === "price") {
    return value * 0.9;
  }

  return value;
});

console.log(jsonString);
```

Output:

```json
{"name":"Laptop","price":1080,"discount":10}
```

This transforms the price during serialization without modifying the original object.

# 🗁 Using the Replacer Array

The second parameter can also be an array containing property names to include.

Example:

```javascript
const user = {
  id: 101,
  name: "Alice",
  email: "alice@example.com",
  role: "developer"
};

const jsonString = JSON.stringify(
  user,
  ["name", "email"]
);

console.log(jsonString);
```

Output:

```json
{"name":"Alice","email":"alice@example.com"}
```

Only the selected properties are serialized.

The property list also applies to matching properties in nested objects.

# 🗁 Handling Undefined Values

JavaScript supports `undefined`, but JSON does not.

Its behavior depends on where the value appears.

**Object properties**

Properties with `undefined` values are omitted.

```javascript
const user = {
  name: "Alice",
  age: undefined
};

console.log(JSON.stringify(user));
```

Output:

```json
{"name":"Alice"}
```

**Array elements**

Undefined array elements become `null`.

```javascript
const values = [
  "Alice",
  undefined,
  "Bob"
];

console.log(JSON.stringify(values));
```

Output:

```json
["Alice",null,"Bob"]
```

**Top-level undefined**

```javascript
console.log(JSON.stringify(undefined));
```

Output:

```text
undefined
```

The method returns JavaScript `undefined`, not the JSON string `"undefined"`.

# 🗁 Handling Functions

Functions are not valid JSON values.

For object properties, functions are omitted.

```javascript
const user = {
  name: "Alice",
  greet() {
    return "Hello";
  }
};

console.log(JSON.stringify(user));
```

Output:

```json
{"name":"Alice"}
```

For array elements, functions become `null`.

```javascript
const values = [
  10,
  function () {},
  20
];

console.log(JSON.stringify(values));
```

Output:

```json
[10,null,20]
```

# 🗁 Handling Symbol Values

Symbols cannot be represented directly in JSON.

Symbol-valued object properties are omitted.

```javascript
const user = {
  name: "Alice",
  id: Symbol("user-id")
};

console.log(JSON.stringify(user));
```

Output:

```json
{"name":"Alice"}
```

Symbol-valued array elements become `null`.

```javascript
const values = [
  "Alice",
  Symbol("id")
];

console.log(JSON.stringify(values));
```

Output:

```json
["Alice",null]
```

# 🗁 Handling NaN and Infinity

JSON does not support `NaN`, `Infinity`, or `-Infinity`.

These values are serialized as `null`.

```javascript
const numbers = {
  normal: 100,
  notANumber: NaN,
  positiveInfinity: Infinity,
  negativeInfinity: -Infinity
};

console.log(JSON.stringify(numbers));
```

Output:

```json
{
  "normal": 100,
  "notANumber": null,
  "positiveInfinity": null,
  "negativeInfinity": null
}
```

If the application needs to preserve these special values, define an explicit representation, such as strings or tagged objects.

# 🗁 Handling BigInt

BigInt values cannot be serialized directly using `JSON.stringify()`.

Example:

```javascript
const user = {
  id: 12345678901234567890n
};

JSON.stringify(user);
```

This throws a `TypeError`.

One common solution is converting BigInt values into strings.

```javascript
const jsonString = JSON.stringify(user, (key, value) => {
  if (typeof value === "bigint") {
    return value.toString();
  }

  return value;
});

console.log(jsonString);
```

Output:

```json
{"id":"12345678901234567890"}
```

The receiving application must convert the string back into a BigInt when necessary.

# 🗁 Serializing Date Objects

Date objects are automatically converted into ISO 8601 strings through their `toJSON()` method.

Example:

```javascript
const user = {
  name: "Alice",
  createdAt: new Date("2026-11-14T10:30:00.000Z")
};

console.log(JSON.stringify(user));
```

Output:

```json
{"name":"Alice","createdAt":"2026-11-14T10:30:00.000Z"}
```

The original Date object becomes a string.

Parsing this JSON later does not automatically restore the Date object. Explicit conversion is required.

# 🗁 The toJSON() Method

Objects can define a `toJSON()` method to customize how they are serialized.

Example:

```javascript
const user = {
  name: "Alice",
  password: "secret",

  toJSON() {
    return {
      name: this.name
    };
  }
};

console.log(JSON.stringify(user));
```

Output:

```json
{"name":"Alice"}
```

When an object provides a `toJSON()` method, the method's return value is used during serialization.

This is useful for controlling how custom objects are represented in JSON.

# 🗁 Serializing Map and Set

`Map` and `Set` are not directly represented by standard JSON structures.

Example:

```javascript
const userRoles = new Map([
  ["Alice", "admin"],
  ["Bob", "editor"]
]);

console.log(JSON.stringify(userRoles));
```

Output:

```json
{}
```

A `Map` can be converted into an array of entries.

```javascript
const jsonString = JSON.stringify(
  [...userRoles]
);

console.log(jsonString);
```

Output:

```json
[["Alice","admin"],["Bob","editor"]]
```

Similarly, a `Set` can be converted into an array.

```javascript
const skills = new Set([
  "JavaScript",
  "TypeScript"
]);

console.log(JSON.stringify([...skills]));
```

Output:

```json
["JavaScript","TypeScript"]
```

The receiving application must reconstruct the original collection types explicitly.

# 🗁 Circular References

A circular reference occurs when an object references itself directly or indirectly.

Example:

```javascript
const user = {
  name: "Alice"
};

user.self = user;

JSON.stringify(user);
```

This throws a `TypeError` because JSON cannot represent circular references.

A replacer can omit known circular properties.

```javascript
const user = {
  name: "Alice"
};

user.self = user;

const jsonString = JSON.stringify(user, (key, value) => {
  if (key === "self") {
    return undefined;
  }

  return value;
});

console.log(jsonString);
```

Output:

```json
{"name":"Alice"}
```

For complex object graphs, use a serialization strategy specifically designed to handle circular references.

# 🗁 Serializing Objects with Shared References

Shared references are different from circular references.

Example:

```javascript
const address = {
  city: "Dhaka"
};

const user = {
  homeAddress: address,
  workAddress: address
};

console.log(JSON.stringify(user));
```

This succeeds because neither property creates a cycle.

JSON duplicates the address data in the output.

The resulting JSON does not preserve the original shared object identity.

# 🗁 Using JSON.stringify() with Local Storage

Local Storage stores strings rather than JavaScript objects.

`JSON.stringify()` allows structured values to be stored.

```javascript
const user = {
  id: 101,
  name: "Alice",
  role: "developer"
};

localStorage.setItem(
  "user",
  JSON.stringify(user)
);
```

Retrieve and parse the value:

```javascript
const storedUser = localStorage.getItem("user");

if (storedUser !== null) {
  const user = JSON.parse(storedUser);

  console.log(user.name);
}
```

This is a common pattern for storing non-sensitive application preferences and state.

# 🗁 Using JSON.stringify() with Fetch API

`JSON.stringify()` is commonly used to send JSON data to backend APIs.

Example:

```javascript
async function createUser() {
  const user = {
    name: "Alice",
    email: "alice@example.com"
  };

  const response = await fetch("/api/users", {
    method: "POST",
    headers: {
      "Content-Type": "application/json"
    },
    body: JSON.stringify(user)
  });

  if (!response.ok) {
    throw new Error(`HTTP error: ${response.status}`);
  }

  return response.json();
}
```

The object is converted into a JSON string before being sent.

The `Content-Type` header identifies the request body format.

# 🗁 JSON.stringify() vs JSON.parse()

These methods perform opposite operations.

| Feature | `JSON.stringify()` | `JSON.parse()` |
| --- | --- | --- |
| Purpose | Serialize JavaScript values | Parse JSON text |
| Input | JavaScript value | JSON string |
| Output | JSON string or `undefined` | JavaScript value |
| Main use | Storage and data transmission | Reading and processing JSON |
| Error handling | Can throw serialization errors | Can throw syntax errors |

Example:

```javascript
const user = {
  name: "Alice",
  age: 25
};

// JavaScript object -> JSON string
const jsonString = JSON.stringify(user);

// JSON string -> JavaScript object
const parsedUser = JSON.parse(jsonString);

console.log(parsedUser.name);
```

# 🗁 Common Mistakes

**Assuming the result is still an object**

Incorrect:

```javascript
const user = {
  name: "Alice"
};

const result = JSON.stringify(user);

console.log(result.name); // undefined
```

Correct:

```javascript
const result = JSON.stringify(user);

const parsed = JSON.parse(result);

console.log(parsed.name); // Alice
```

**Trying to serialize BigInt directly**

Incorrect:

```javascript
JSON.stringify(100n);
```

Convert BigInt into a string or use a custom serialization strategy.

**Ignoring circular references**

Circular structures cause serialization errors. Handle them explicitly.

**Assuming undefined is preserved**

Undefined object properties disappear, while undefined array elements become `null`.

**Assuming Date objects remain Date objects**

Date instances become strings. Restore them explicitly when parsing.

**Using JSON serialization as a universal deep-cloning method**

A common shortcut is:

```javascript
const clone = JSON.parse(
  JSON.stringify(original)
);
```

However, this approach loses or changes information that JSON cannot represent, including:

* Functions.
* `undefined`.
* Symbols.
* BigInt values.
* Map and Set instances.
* Date object identity.
* Circular references.
* Shared object references.

For general deep cloning of supported JavaScript values, consider `structuredClone()` instead.

# 🗁 Best Practices

* Use `JSON.stringify()` when converting data into JSON text.
* Use the `space` parameter for readable JSON output.
* Use replacers to control serialized properties.
* Handle BigInt values explicitly.
* Avoid circular references in JSON data structures.
* Remember that Date objects become strings.
* Validate data before sending it to external services.
* Do not use JSON serialization as a universal cloning mechanism.
* Avoid exposing passwords, tokens, or other secrets in serialized data.
* Use consistent API request and response schemas.

# 🗁 Summary

`JSON.stringify()` converts JavaScript values into JSON-formatted strings.

Important concepts include:

* Object and array serialization.
* Primitive value serialization.
* Formatting with the `space` parameter.
* Replacer functions and property arrays.
* Handling `undefined`, functions, and symbols.
* Handling BigInt and special numeric values.
* Date serialization and `toJSON()`.
* Map and Set conversion.
* Circular and shared references.
* Local Storage and Fetch API integration.
* Differences between serialization and parsing.

`JSON.stringify()` is essential for storing and transmitting structured JavaScript data in a standardized format.
