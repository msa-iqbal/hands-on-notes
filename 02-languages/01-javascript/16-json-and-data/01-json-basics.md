# JSON Basics

JSON (JavaScript Object Notation) is a lightweight, text-based data format used to store, exchange, and transfer structured data between applications.

JSON is widely used in:

* REST APIs.
* Web applications.
* Configuration files.
* Database communication.
* Application settings.
* Data storage.
* Microservices communication.

Example:

```json
{
  "name": "John",
  "age": 25,
  "isActive": true,
  "skills": ["JavaScript", "TypeScript", "Node.js"]
}
```

JSON is language-independent, although its syntax is based on JavaScript object literal notation.

# 🗁 Why JSON Is Used

JSON provides a standardized way for different systems to exchange structured information.

For example, a frontend application might request user information from a backend API.

Backend response:

```json
{
  "id": 101,
  "name": "John Doe",
  "email": "john@example.com",
  "role": "developer"
}
```

JavaScript can convert this JSON text into an object and use its properties.

```javascript
const user = {
  id: 101,
  name: "John Doe",
  email: "john@example.com",
  role: "developer"
};

console.log(user.name);
```

JSON is especially useful because most programming languages provide built-in or standard-library support for parsing and generating it.

# 🗁 JSON Syntax

JSON has a strict syntax based on key-value pairs and ordered arrays.

A JSON document can contain:

* Objects.
* Arrays.
* Strings.
* Numbers.
* Booleans.
* `null`.

Example:

```json
{
  "name": "Alice",
  "age": 28,
  "isActive": true,
  "address": null,
  "skills": ["HTML", "CSS", "JavaScript"],
  "profile": {
    "role": "Frontend Developer",
    "experience": 4
  }
}
```

# 🗁 JSON Objects

A JSON object is a collection of key-value pairs enclosed in curly braces `{}`.

Syntax:

```json
{
  "key": "value"
}
```

Example:

```json
{
  "id": 101,
  "name": "Alice",
  "email": "alice@example.com"
}
```

Important rules:

* Object keys must be strings.
* Keys must use double quotes.
* Key-value pairs are separated by commas.
* A colon separates each key from its value.
* Trailing commas are not allowed.

Valid JSON:

```json
{
  "name": "Alice",
  "age": 28
}
```

Invalid JSON:

```javascript
{
  name: "Alice",
  age: 28,
}
```

The second example uses JavaScript object literal syntax, not valid JSON.

# 🗁 JSON Arrays

A JSON array is an ordered collection of values enclosed in square brackets `[]`.

Example:

```json
[
  "JavaScript",
  "TypeScript",
  "Python"
]
```

Arrays can contain different JSON value types.

```json
[
  "Alice",
  25,
  true,
  null,
  {
    "role": "Developer"
  }
]
```

Arrays can also contain nested arrays.

```json
[
  [1, 2, 3],
  [4, 5, 6]
]
```

# 🗁 JSON Strings

JSON strings represent text and must use double quotes.

Example:

```json
{
  "name": "John",
  "message": "Welcome to JavaScript"
}
```

Valid:

```json
{
  "language": "JavaScript"
}
```

Invalid:

```javascript
{
  "language": 'JavaScript'
}
```

JSON does not allow single-quoted strings.

# 🗁 JSON Numbers

JSON numbers can represent integers and decimal values.

Example:

```json
{
  "age": 25,
  "price": 99.99,
  "temperature": -5,
  "population": 1000000
}
```

JSON does not support:

* `NaN`
* `Infinity`
* `-Infinity`
* `undefined`
* BigInt literals

For example, this is invalid JSON:

```javascript
{
  "value": NaN
}
```

Use a suitable string or `null` representation when an application needs to communicate a special numeric state.

# 🗁 JSON Booleans

JSON supports two boolean values:

* `true`
* `false`

Example:

```json
{
  "isActive": true,
  "isAdmin": false
}
```

JSON boolean values must be lowercase.

Invalid:

```javascript
{
  "isActive": True
}
```

# 🗁 JSON Null

JSON uses `null` to represent an explicitly empty or absent value.

Example:

```json
{
  "middleName": null,
  "deletedAt": null
}
```

JSON does not support `undefined`.

If a JavaScript object contains a property with the value `undefined`, that property is omitted during JSON serialization.

# 🗁 Nested JSON Objects

JSON supports objects inside other objects.

Example:

```json
{
  "id": 101,
  "name": "Alice",
  "address": {
    "city": "Dhaka",
    "country": "Bangladesh",
    "postalCode": 1207
  }
}
```

Nested properties can represent hierarchical relationships.

In JavaScript, access them using property notation:

```javascript
const user = {
  name: "Alice",
  address: {
    city: "Dhaka",
    country: "Bangladesh"
  }
};

console.log(user.address.city);
```

# 🗁 Nested JSON Arrays

JSON arrays can contain objects and other arrays.

Example:

```json
{
  "name": "Development Team",
  "members": [
    {
      "id": 1,
      "name": "Alice",
      "role": "Frontend Developer"
    },
    {
      "id": 2,
      "name": "Bob",
      "role": "Backend Developer"
    }
  ]
}
```

JavaScript can access nested array elements:

```javascript
const team = {
  members: [
    {
      id: 1,
      name: "Alice"
    },
    {
      id: 2,
      name: "Bob"
    }
  ]
};

console.log(team.members[0].name);
```

# 🗁 JSON vs JavaScript Objects

JSON and JavaScript objects look similar, but they are different.

| Feature | JSON | JavaScript Object |
| --- | --- | --- |
| Purpose | Data exchange and storage | Representing application data and behavior |
| Keys | Double-quoted strings | Usually unquoted identifiers or computed keys |
| Strings | Double quotes only | Single, double, or template literals |
| Functions | Not supported | Supported as property values |
| `undefined` | Not supported | Supported |
| Symbols | Not supported | Supported as property keys |
| Comments | Not supported | Supported in JavaScript source |
| Trailing commas | Not allowed | Allowed |
| Methods | Not supported | Supported |
| Data format | Text | JavaScript runtime value |

JavaScript object:

```javascript
const user = {
  name: "Alice",
  greet() {
    return `Hello, ${this.name}`;
  }
};
```

JSON cannot represent the `greet()` function.

# 🗁 JSON Data Types vs JavaScript Data Types

JSON supports a smaller set of data types than JavaScript.

| JSON Type | JavaScript Equivalent |
| --- | --- |
| Object | Object |
| Array | Array |
| String | String |
| Number | Number |
| Boolean | Boolean |
| `null` | `null` |

JavaScript types without direct JSON representations include:

* `undefined`
* `BigInt`
* `Symbol`
* Function
* Date
* Map
* Set
* WeakMap
* WeakSet

Some of these values can be converted into JSON-compatible representations, but their original types are not automatically preserved.

# 🗁 JSON File Format

JSON documents commonly use the `.json` file extension.

Example:

`user.json`

```json
{
  "id": 101,
  "name": "Alice",
  "email": "alice@example.com",
  "isActive": true
}
```

JSON files are commonly used for:

* Application configuration.
* Package metadata.
* API mock data.
* Localization data.
* Test fixtures.
* Static application data.

# 🗁 JSON MIME Type

The standard media type for JSON is:

```http
Content-Type: application/json
```

An API response might look like this:

```http
HTTP/1.1 200 OK
Content-Type: application/json

{
  "id": 101,
  "name": "Alice"
}
```

The `Content-Type` header tells the client that the response body contains JSON data.

# 🗁 JSON Comments

Standard JSON does not support comments.

Invalid JSON:

```javascript
{
  // User information
  "name": "Alice"
}
```

Also invalid:

```javascript
{
  "name": "Alice" /* User name */
}
```

If comments are required, use a configuration format that supports them, such as JSONC, or keep documentation outside the JSON document.

JSONC is an extension used by some tools and editors. It is not standard JSON.

# 🗁 JSON Formatting

JSON can be written in compact or formatted form.

Compact JSON:

```json
{"name":"Alice","age":28,"isActive":true}
```

Formatted JSON:

```json
{
  "name": "Alice",
  "age": 28,
  "isActive": true
}
```

Both represent the same JSON data.

Formatted JSON is easier to read and maintain.

# 🗁 Common JSON Syntax Errors

**Using single quotes**

Invalid:

```javascript
{
  'name': 'Alice'
}
```

Correct:

```json
{
  "name": "Alice"
}
```

**Using unquoted keys**

Invalid:

```javascript
{
  name: "Alice"
}
```

Correct:

```json
{
  "name": "Alice"
}
```

**Using trailing commas**

Invalid:

```javascript
{
  "name": "Alice",
  "age": 28,
}
```

Correct:

```json
{
  "name": "Alice",
  "age": 28
}
```

**Using comments**

Invalid:

```javascript
{
  // User details
  "name": "Alice"
}
```

Correct:

```json
{
  "name": "Alice"
}
```

**Using unsupported values**

Invalid:

```javascript
{
  "value": undefined,
  "amount": NaN
}
```

Correct:

```json
{
  "value": null,
  "amount": null
}
```

The correct replacement depends on the application's data model and the meaning of each value.

# 🗁 Validating JSON

A JSON document must follow the JSON grammar to be valid.

JavaScript provides `JSON.parse()` to validate and parse JSON text.

Example:

```javascript
const jsonText = '{"name":"Alice","age":28}';

try {
  const data = JSON.parse(jsonText);

  console.log("Valid JSON:", data);
} catch (error) {
  console.error("Invalid JSON:", error.message);
}
```

Other tools include:

* Code editor JSON validation.
* JSON command-line tools.
* API testing tools.
* JSON schema validators.

Validation helps identify malformed JSON before applications process it.

# 🗁 Best Practices

* Use double quotes for all JSON keys and strings.
* Avoid trailing commas.
* Use consistent indentation.
* Keep property names meaningful.
* Use appropriate JSON data types.
* Avoid unnecessary nesting.
* Do not include comments in standard JSON.
* Validate JSON before processing it.
* Use `application/json` for JSON HTTP content.
* Define clear structures for API request and response data.
* Avoid placing secrets in publicly accessible JSON files.

# 🗁 Summary

JSON is a lightweight, text-based format for representing and exchanging structured data.

Important concepts include:

* JSON objects and arrays.
* Strings, numbers, booleans, and `null`.
* Nested objects and arrays.
* JSON syntax rules.
* JSON versus JavaScript objects.
* JSON file format and MIME type.
* JSON comments and formatting.
* Common syntax errors.
* JSON validation.

Understanding JSON is essential for working with APIs, application configuration, frontend-backend communication, and data serialization.
