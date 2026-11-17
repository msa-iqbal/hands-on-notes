# JSON.parse()

`JSON.parse()` is a built-in JavaScript method used to convert a JSON-formatted string into a JavaScript value.

It is commonly used when working with:

* REST API responses.
* JSON configuration files.
* Local storage data.
* Session storage data.
* Data received from external services.
* Serialized application data.

Example:

```javascript
const jsonString = '{"name":"Alice","age":25}';

const user = JSON.parse(jsonString);

console.log(user.name); // Alice
console.log(user.age);  // 25
```

`JSON.parse()` converts JSON text into a JavaScript object that can be accessed and manipulated like other JavaScript values.

# 🗁 Syntax

```javascript
JSON.parse(text, reviver);
```

Parameters:

| Parameter | Description |
| --- | --- |
| `text` | A valid JSON-formatted string |
| `reviver` | Optional function that transforms parsed values |

Return value:

* A JavaScript value represented by the JSON string.

If the input is not valid JSON, `JSON.parse()` throws a `SyntaxError`.

# 🗁 Parsing a JSON Object

A JSON object string becomes a JavaScript object.

```javascript
const jsonString = `{
  "id": 101,
  "name": "Alice",
  "email": "alice@example.com",
  "isActive": true
}`;

const user = JSON.parse(jsonString);

console.log(user);
console.log(user.name);
console.log(user.email);
```

Output:

```text
{
  id: 101,
  name: 'Alice',
  email: 'alice@example.com',
  isActive: true
}

Alice
alice@example.com
```

The resulting object supports normal JavaScript operations.

```javascript
user.name = "Bob";

console.log(user.name); // Bob
```

# 🗁 Parsing a JSON Array

`JSON.parse()` can also convert JSON arrays into JavaScript arrays.

```javascript
const jsonString = `[
  "JavaScript",
  "TypeScript",
  "Node.js"
]`;

const languages = JSON.parse(jsonString);

console.log(languages);
console.log(languages[0]);
```

Output:

```text
[ 'JavaScript', 'TypeScript', 'Node.js' ]

JavaScript
```

The parsed array supports normal array methods.

```javascript
languages.push("Python");

console.log(languages);
```

# 🗁 Parsing Primitive Values

JSON can represent primitive values as well as objects and arrays.

String:

```javascript
const value = JSON.parse('"Hello World"');

console.log(value); // Hello World
```

Number:

```javascript
const value = JSON.parse("100");

console.log(value); // 100
```

Boolean:

```javascript
const value = JSON.parse("true");

console.log(value); // true
```

Null:

```javascript
const value = JSON.parse("null");

console.log(value); // null
```

The result is not always an object. It can be any valid JSON value.

# 🗁 Parsing Nested JSON Data

JSON supports nested objects and arrays.

Example:

```javascript
const jsonString = `{
  "id": 101,
  "name": "Alice",
  "address": {
    "city": "Dhaka",
    "country": "Bangladesh"
  },
  "skills": [
    "JavaScript",
    "TypeScript"
  ]
}`;

const user = JSON.parse(jsonString);

console.log(user.address.city);
console.log(user.skills[0]);
```

Output:

```text
Dhaka
JavaScript
```

Nested structures become corresponding JavaScript objects and arrays.

# 🗁 Parsing JSON from an API

The Fetch API provides a convenient way to retrieve JSON responses.

Example:

```javascript
async function getUsers() {
  try {
    const response = await fetch("/api/users");

    if (!response.ok) {
      throw new Error(`HTTP error: ${response.status}`);
    }

    const users = await response.json();

    console.log(users);
  } catch (error) {
    console.error("Unable to fetch users:", error);
  }
}

getUsers();
```

The `response.json()` method reads the response body and parses it as JSON.

It is important to understand that `response.json()` is not the same as manually calling `JSON.parse()`.

The Fetch API handles reading the response stream and parsing the JSON content.

If JSON text is already available as a string, use `JSON.parse()` directly.

# 🗁 Parsing JSON from Local Storage

Local Storage stores values as strings.

When an application stores an object, it commonly serializes it using `JSON.stringify()`.

Example:

```javascript
const user = {
  id: 101,
  name: "Alice",
  role: "Developer"
};

localStorage.setItem("user", JSON.stringify(user));
```

To retrieve the original object:

```javascript
const storedUser = localStorage.getItem("user");

if (storedUser !== null) {
  const user = JSON.parse(storedUser);

  console.log(user.name);
}
```

`JSON.parse()` converts the stored JSON string back into a JavaScript value.

# 🗁 Handling Invalid JSON

Invalid JSON causes `JSON.parse()` to throw a `SyntaxError`.

Example:

```javascript
const jsonString = "{ name: 'Alice' }";

const result = JSON.parse(jsonString);
```

This throws an error because:

* The property name is not enclosed in double quotes.
* The string uses single quotes.

Use `try...catch` to handle invalid JSON safely.

```javascript
const jsonString = "{ name: 'Alice' }";

try {
  const result = JSON.parse(jsonString);

  console.log(result);
} catch (error) {
  console.error("Invalid JSON:", error.message);
}
```

This prevents malformed JSON from unexpectedly terminating the current execution flow.

# 🗁 The Reviver Function

The second parameter of `JSON.parse()` is an optional `reviver` function.

A reviver allows developers to transform parsed values during the parsing process.

Syntax:

```javascript
JSON.parse(text, (key, value) => {
  // Transform value
});
```

The function receives:

* `key`: The current property name.
* `value`: The current property value.

The reviver is called for nested values before their containing objects.

# 🗁 Transforming Values with a Reviver

Example:

```javascript
const jsonString = `{
  "name": "Alice",
  "age": 25,
  "isActive": true
}`;

const user = JSON.parse(jsonString, (key, value) => {
  if (typeof value === "string") {
    return value.toUpperCase();
  }

  return value;
});

console.log(user);
```

Output:

```javascript
{
  name: "ALICE",
  age: 25,
  isActive: true
}
```

The reviver transforms every string value into uppercase.

# 🗁 Converting Date Strings into Date Objects

JSON does not have a native Date data type.

Dates are commonly represented as strings.

Example JSON:

```javascript
const jsonString = `{
  "name": "Alice",
  "createdAt": "2026-11-14T10:30:00.000Z"
}`;
```

A reviver can convert date strings into JavaScript `Date` objects.

```javascript
const user = JSON.parse(jsonString, (key, value) => {
  if (key === "createdAt") {
    return new Date(value);
  }

  return value;
});

console.log(user.createdAt);
console.log(user.createdAt instanceof Date);
```

Output:

```text
2026-11-14T10:30:00.000Z
true
```

In production applications, date conversion should be based on an explicit data contract or a reliable date-field convention.

# 🗁 Removing Properties with a Reviver

A reviver can return `undefined` to remove a property from the resulting object.

Example:

```javascript
const jsonString = `{
  "name": "Alice",
  "password": "secret",
  "role": "admin"
}`;

const user = JSON.parse(jsonString, (key, value) => {
  if (key === "password") {
    return undefined;
  }

  return value;
});

console.log(user);
```

Output:

```javascript
{
  name: "Alice",
  role: "admin"
}
```

Returning `undefined` removes the corresponding property.

For array elements, returning `undefined` creates an empty slot rather than shortening the array.

# 🗁 The Reviver's Root Value

After processing nested values, the reviver is called once more for the root value.

The root property uses an empty string as its key.

Example:

```javascript
const result = JSON.parse('{"name":"Alice"}', (key, value) => {
  console.log(key, value);

  return value;
});
```

The reviver receives the nested `name` property first and then the root object with an empty key.

This behavior is useful when transforming an entire parsed document.

# 🗁 Parsing Large Integers

JavaScript's `Number` type uses IEEE 754 double-precision floating-point representation.

Integers outside the safe integer range may lose precision.

Example:

```javascript
const jsonString = '{"id":9007199254740993}';

const data = JSON.parse(jsonString);

console.log(data.id);
```

The original integer cannot be represented exactly as a JavaScript `Number`.

The safe integer range is:

```javascript
Number.MIN_SAFE_INTEGER;
Number.MAX_SAFE_INTEGER;
```

For identifiers or values that may exceed this range, prefer representing them as JSON strings.

```javascript
const jsonString = '{"id":"9007199254740993"}';

const data = JSON.parse(jsonString);

const id = BigInt(data.id);

console.log(id);
```

Modern JavaScript implementations also support the optional `context` argument for reviver callbacks on primitive values. Its `source` property can expose the original JSON text for that value, allowing precision-sensitive conversions.

Example:

```javascript
const jsonString = '{"id":9007199254740993}';

const data = JSON.parse(jsonString, (key, value, context) => {
  if (key === "id" && context?.source) {
    return BigInt(context.source);
  }

  return value;
});

console.log(data.id);
```

The `context` argument is available for primitive values, not objects and arrays. Check runtime support before relying on it.

# 🗁 JSON.parse() vs response.json()

Both methods produce JavaScript values from JSON data, but they operate at different levels.

| Feature | `JSON.parse()` | `response.json()` |
| --- | --- | --- |
| Input | JSON string | Fetch `Response` object |
| Purpose | Parse existing JSON text | Read and parse an HTTP response body |
| Asynchronous | No | Yes |
| Returns | JavaScript value | Promise resolving to a JavaScript value |
| Reads response stream | No | Yes |

Example using `JSON.parse()`:

```javascript
const jsonText = '{"name":"Alice"}';

const user = JSON.parse(jsonText);
```

Example using `response.json()`:

```javascript
const response = await fetch("/api/user");

const user = await response.json();
```

Use the method that matches the source of the data.

# 🗁 Security Considerations

Parsing JSON is generally safer than executing JavaScript source code because JSON is a data format rather than executable code.

However, parsed data should still be treated as untrusted input.

Best practices:

* Validate data received from external sources.
* Do not assume parsed values have the expected structure.
* Avoid inserting untrusted strings into HTML.
* Use `textContent` when displaying plain text.
* Validate identifiers and numeric values.
* Avoid trusting client-side data for authorization decisions.
* Handle malformed JSON with `try...catch`.

Example of validating the expected structure:

```javascript
function parseUser(jsonText) {
  const user = JSON.parse(jsonText);

  if (
    typeof user !== "object" ||
    user === null ||
    typeof user.name !== "string" ||
    typeof user.email !== "string"
  ) {
    throw new Error("Invalid user data");
  }

  return user;
}
```

For complex data structures, use a dedicated schema validation library.

# 🗁 Common Mistakes

**Passing a JavaScript object instead of a JSON string**

Incorrect:

```javascript
const user = {
  name: "Alice"
};

JSON.parse(user);
```

Correct:

```javascript
const user = {
  name: "Alice"
};

const parsed = JSON.parse(JSON.stringify(user));
```

Normally, there is no reason to parse an object that is already available as a JavaScript value.

**Using single quotes inside JSON**

Incorrect:

```javascript
JSON.parse("{'name':'Alice'}");
```

Correct:

```javascript
JSON.parse('{"name":"Alice"}');
```

**Forgetting error handling**

Incorrect:

```javascript
const data = JSON.parse(jsonText);
```

Better:

```javascript
try {
  const data = JSON.parse(jsonText);
} catch (error) {
  console.error("Invalid JSON:", error);
}
```

**Assuming parsed data has the expected structure**

Incorrect:

```javascript
const user = JSON.parse(jsonText);

console.log(user.profile.name);
```

If `profile` is missing or `null`, this operation can throw an error.

Validate the structure before accessing nested properties.

# 🗁 Best Practices

* Use `JSON.parse()` only with JSON-formatted text.
* Handle parsing errors with `try...catch`.
* Use revivers for controlled data transformations.
* Convert date strings explicitly when required.
* Validate external data after parsing.
* Use strings for precision-sensitive large integers.
* Prefer `response.json()` when parsing Fetch API responses.
* Avoid parsing data that is already a JavaScript object.
* Never use `eval()` as a replacement for JSON parsing.
* Define consistent data contracts between frontend and backend.

# 🗁 Summary

`JSON.parse()` converts JSON-formatted text into JavaScript values.

Important concepts include:

* Parsing JSON objects and arrays.
* Parsing primitive JSON values.
* Parsing nested structures.
* Handling invalid JSON.
* Using the reviver function.
* Transforming date strings.
* Removing properties during parsing.
* Handling large integers and precision.
* Differences between `JSON.parse()` and `response.json()`.
* Validating parsed data.

`JSON.parse()` is essential for applications that consume JSON APIs, read stored data, and process structured information.
