# JSON Basics

JSON (JavaScript Object Notation) is a lightweight, text-based format for representing structured data. It is commonly used to exchange information between frontend applications, backend services, APIs, and databases.

JSON is easy for humans to read and write, and straightforward for machines to parse and generate.

Despite its name, JSON is not limited to JavaScript. It is supported by almost every modern programming language.

# 🗁 What Is JSON?

JSON represents data using a structured syntax consisting of:

* Objects containing key-value pairs.
* Arrays containing ordered values.
* Strings.
* Numbers.
* Boolean values.
* Null values.

Example:

```json
{
  "name": "John Doe",
  "age": 28,
  "email": "john@example.com",
  "isDeveloper": true,
  "skills": [
    "JavaScript",
    "Python",
    "PHP"
  ]
}
```

This JSON document describes a person with a name, age, email address, developer status, and programming skills.

# 🗁 Why JSON Is Used

JSON is popular because it provides a simple and consistent way to exchange structured data.

Common use cases include:

**API communication**

Frontend applications frequently receive JSON responses from backend APIs.

Example:

```json
{
  "success": true,
  "message": "Login successful",
  "data": {
    "id": 101,
    "username": "john",
    "role": "admin"
  }
}
```

**Application configuration**

Applications can use JSON files to store configuration settings.

Example:

```json
{
  "appName": "My Application",
  "environment": "production",
  "port": 3000,
  "debug": false
}
```

**Data storage and exchange**

JSON can represent structured records that are transferred between different systems.

**Logging**

Applications can generate structured JSON logs that are easier for monitoring systems to process.

# 🗁 JSON Syntax Rules

JSON follows strict syntax rules.

**Object keys must use double quotes.**

Valid:

```json
{
  "name": "John",
  "age": 28
}
```

Invalid:

```text
{
  name: "John",
  age: 28
}
```

**String values must use double quotes.**

Valid:

```json
{
  "message": "Hello World"
}
```

Invalid:

```text
{
  "message": 'Hello World'
}
```

**Key-value pairs are separated by commas.**

Valid:

```json
{
  "name": "John",
  "age": 28,
  "active": true
}
```

**A colon separates each key from its value.**

```json
{
  "name": "John"
}
```

**Trailing commas are not allowed.**

Invalid:

```text
{
  "name": "John",
  "age": 28,
}
```

Valid:

```json
{
  "name": "John",
  "age": 28
}
```

**JSON does not support comments.**

Invalid:

```text
{
  // User information
  "name": "John"
}
```

Standard JSON documents cannot contain comments.

# 🗁 JSON Objects

An object is a collection of key-value pairs enclosed in curly braces `{}`.

Example:

```json
{
  "name": "John",
  "age": 28,
  "isActive": true
}
```

Structure:

```text
{
  "key": "value"
}
```

An object can contain different JSON value types, including other objects and arrays.

# 🗁 JSON Arrays

An array is an ordered collection of values enclosed in square brackets `[]`.

Example:

```json
{
  "languages": [
    "JavaScript",
    "Python",
    "PHP"
  ]
}
```

Arrays can contain strings, numbers, booleans, null values, objects, and other arrays.

Example:

```json
{
  "scores": [85, 90, 95],
  "active": [true, false, true],
  "users": [
    {
      "id": 1,
      "name": "John"
    },
    {
      "id": 2,
      "name": "Alice"
    }
  ]
}
```

# 🗁 JSON Data Format

JSON is a text format, not a programming-language object.

Consider:

```json
{
  "name": "John",
  "age": 28
}
```

When stored in a file, transferred through HTTP, or received from an API, it is JSON text.

A programming language must parse that text before accessing its structured values.

For example, JavaScript converts JSON text into a JavaScript object using `JSON.parse()`.

# 🗁 JSON Serialization and Deserialization

Serialization and deserialization are fundamental operations when working with JSON.

**Serialization**

Serialization converts an in-memory data structure into a JSON string.

```javascript
const user = {
  name: "John",
  age: 28
};

const jsonText = JSON.stringify(user);

console.log(jsonText);
```

Output:

```text
{"name":"John","age":28}
```

**Deserialization**

Deserialization converts JSON text into an in-memory data structure.

```javascript
const jsonText = '{"name":"John","age":28}';

const user = JSON.parse(jsonText);

console.log(user.name);
console.log(user.age);
```

Output:

```text
John
28
```

The same concepts apply to other languages, although method names differ.

# 🗁 JSON with JavaScript

JavaScript provides a global `JSON` object with two commonly used methods.

**JSON.stringify()**

Converts JavaScript values into JSON strings.

```javascript
const product = {
  id: 101,
  name: "Laptop",
  price: 75000
};

const json = JSON.stringify(product);

console.log(json);
```

Output:

```text
{"id":101,"name":"Laptop","price":75000}
```

You can format the output using indentation:

```javascript
const formattedJson = JSON.stringify(product, null, 2);

console.log(formattedJson);
```

Output:

```json
{
  "id": 101,
  "name": "Laptop",
  "price": 75000
}
```

**JSON.parse()**

Converts JSON strings into JavaScript values.

```javascript
const json = '{"id":101,"name":"Laptop","price":75000}';

const product = JSON.parse(json);

console.log(product.name);
console.log(product.price);
```

Output:

```text
Laptop
75000
```

Invalid JSON causes `JSON.parse()` to throw a `SyntaxError`.

# 🗁 JSON with Python

Python provides the built-in `json` module.

**json.dumps()**

Converts Python data structures into JSON strings.

```python
import json

user = {
    "name": "John",
    "age": 28,
    "is_active": True
}

json_text = json.dumps(user, indent=2)

print(json_text)
```

Output:

```json
{
  "name": "John",
  "age": 28,
  "is_active": true
}
```

**json.loads()**

Converts JSON strings into Python data structures.

```python
import json

json_text = '{"name": "John", "age": 28, "is_active": true}'

user = json.loads(json_text)

print(user["name"])
print(user["age"])
print(user["is_active"])
```

Output:

```text
John
28
True
```

Notice that JSON `true` becomes Python `True`.

# 🗁 JSON vs JavaScript Object Literal

JSON syntax is based on JavaScript object notation, but JSON is a stricter data format.

JavaScript object:

```javascript
const user = {
  name: "John",
  age: 28,
  greet() {
    return "Hello";
  }
};
```

This is valid JavaScript but invalid JSON because:

* The property names are not double-quoted.
* The object contains a function.

Valid JSON representation:

```json
{
  "name": "John",
  "age": 28
}
```

Important differences:

| Feature | JSON | JavaScript Object |
| --- | --- | --- |
| Double-quoted keys | Required | Not always required |
| Single-quoted strings | Not allowed | Allowed |
| Functions | Not supported | Supported |
| Comments | Not supported | Supported in source code |
| `undefined` | Not supported | Supported |
| `NaN` | Not supported | Supported |
| `Infinity` | Not supported | Supported |
| Trailing commas | Not allowed | Allowed in modern JavaScript |

# 🗁 JSON Files

JSON data is commonly stored in files with the `.json` extension.

Example:

`user.json`

```json
{
  "id": 101,
  "name": "John Doe",
  "email": "john@example.com",
  "role": "developer"
}
```

JSON files are commonly used for:

* Application configuration.
* Package metadata.
* API mock data.
* Localization resources.
* Test fixtures.
* Data exports.

Example: Node.js reading a JSON file.

```javascript
const fs = require("node:fs");

const jsonText = fs.readFileSync("user.json", "utf8");

const user = JSON.parse(jsonText);

console.log(user.name);
```

# 🗁 JSON MIME Type

When JSON is transmitted over HTTP, the standard media type is:

```text
application/json
```

Example HTTP response header:

```http
Content-Type: application/json
```

Example using JavaScript `fetch()`:

```javascript
async function getUser() {
  const response = await fetch("/api/users/101");

  if (!response.ok) {
    throw new Error(`HTTP error: ${response.status}`);
  }

  const user = await response.json();

  console.log(user);
}

getUser();
```

The `response.json()` method reads the response body and parses it into a JavaScript value.

# 🗁 JSON Validation

A JSON document must follow the JSON syntax rules to be valid.

Example valid JSON:

```json
{
  "name": "John",
  "age": 28,
  "active": true
}
```

Example invalid JSON:

```text
{
  "name": "John",
  "age": 28,
}
```

The trailing comma makes the document invalid.

You can validate JSON using:

* JSONLint.
* JSON Formatter.
* Your programming language's JSON parser.
* IDE extensions and editor integrations.

Validation checks syntax. It does not necessarily verify that the data follows your application's business rules. For that, use a schema or application-level validation.

# 🗁 Best Practices

* Use meaningful and consistent property names.
* Keep JSON structures predictable.
* Use double quotes for all strings and object keys.
* Avoid unnecessary nesting.
* Validate JSON before processing untrusted input.
* Use the correct JSON MIME type for HTTP responses.
* Handle parsing errors appropriately.
* Use schema validation when an API requires a specific structure.
* Avoid storing secrets in publicly accessible JSON files.
* Use a JSON parser instead of regex for processing JSON.

# 🗁 Key Takeaways

* JSON is a lightweight, language-independent data interchange format.
* JSON represents structured data using objects, arrays, and primitive values.
* JSON syntax requires double-quoted strings and keys.
* Standard JSON does not support comments or trailing commas.
* Serialization converts data structures into JSON text.
* Deserialization converts JSON text into data structures.
* JavaScript uses `JSON.stringify()` and `JSON.parse()`.
* Python uses `json.dumps()` and `json.loads()`.
* JSON is widely used in APIs, configuration files, and application communication.
