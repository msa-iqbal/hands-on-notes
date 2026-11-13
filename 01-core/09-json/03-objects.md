# JSON Objects

A JSON object is a collection of key-value pairs used to represent structured data.

Objects are one of the most important parts of JSON because they allow applications to organize related information into meaningful structures.

JSON objects are commonly used to represent users, products, orders, configurations, API responses, and database records.

# 🗁 What Is a JSON Object?

A JSON object is enclosed in curly braces `{}` and contains zero or more key-value pairs.

Basic structure:

```json
{
  "key": "value"
}
```

Example:

```json
{
  "id": 101,
  "name": "John Doe",
  "email": "john@example.com",
  "isActive": true
}
```

This object represents a user with an ID, name, email address, and account status.

# 🗁 Key-Value Pairs

Each property inside a JSON object consists of a key and a value.

Example:

```json
{
  "name": "John",
  "age": 28,
  "active": true
}
```

The structure is:

```text
"key": value
```

| Key | Value | Data Type |
| --- | --- | --- |
| `name` | `"John"` | String |
| `age` | `28` | Number |
| `active` | `true` | Boolean |

Important syntax rules:

* Keys must be strings enclosed in double quotes.
* A colon separates each key from its value.
* Commas separate multiple properties.
* The final property must not have a trailing comma.
* An object can contain any valid JSON value as a property value.

# 🗁 Object Keys

JSON object keys must be strings.

Valid:

```json
{
  "name": "John",
  "age": 28,
  "user_id": 101,
  "is-active": true
}
```

Invalid:

```text
{
  name: "John",
  age: 28
}
```

Unlike JavaScript object literals, JSON does not allow unquoted property names.

**Keys can contain spaces and special characters.**

```json
{
  "first name": "John",
  "last name": "Doe",
  "user-id": 101,
  "account.status": "active"
}
```

These are valid JSON keys, although applications may prefer consistent naming conventions such as `camelCase` or `snake_case`.

# 🗁 Object Values

An object property can contain any valid JSON value.

**String:**

```json
{
  "name": "John"
}
```

**Number:**

```json
{
  "age": 28,
  "price": 1250.50
}
```

**Boolean:**

```json
{
  "isActive": true,
  "isVerified": false
}
```

**Null:**

```json
{
  "middleName": null
}
```

**Array:**

```json
{
  "skills": [
    "JavaScript",
    "Python",
    "PHP"
  ]
}
```

**Nested object:**

```json
{
  "address": {
    "city": "Dhaka",
    "country": "Bangladesh"
  }
}
```

# 🗁 Multiple Properties

An object can contain multiple properties separated by commas.

Example:

```json
{
  "id": 101,
  "name": "John Doe",
  "email": "john@example.com",
  "age": 28,
  "isActive": true,
  "role": "developer"
}
```

Each property describes a different attribute of the same entity.

# 🗁 Nested Objects

A JSON object can contain another JSON object as its value.

Example:

```json
{
  "user": {
    "id": 101,
    "name": "John Doe",
    "address": {
      "city": "Dhaka",
      "country": "Bangladesh"
    }
  }
}
```

Structure:

```text
user
 ├── id
 ├── name
 └── address
      ├── city
      └── country
```

Nested objects are useful for organizing related information.

For example, an address belongs to a user, so it can be represented as a nested object rather than placing every address field at the top level.

# 🗁 Accessing JSON Objects in JavaScript

After parsing JSON text, JavaScript provides several ways to access object properties.

**Dot notation:**

```javascript
const user = {
  id: 101,
  name: "John",
  email: "john@example.com"
};

console.log(user.name);
console.log(user.email);
```

Output:

```text
John
john@example.com
```

**Bracket notation:**

```javascript
const user = {
  name: "John",
  age: 28
};

console.log(user["name"]);
console.log(user["age"]);
```

Output:

```text
John
28
```

Bracket notation is especially useful when property names contain spaces, special characters, or are stored in variables.

```javascript
const user = {
  "first name": "John",
  "user-id": 101
};

console.log(user["first name"]);
console.log(user["user-id"]);
```

# 🗁 Accessing Nested Objects

Use chained property access to retrieve values from nested objects.

Example:

```javascript
const user = {
  id: 101,
  name: "John",
  address: {
    city: "Dhaka",
    country: "Bangladesh"
  }
};

console.log(user.address.city);
console.log(user.address.country);
```

Output:

```text
Dhaka
Bangladesh
```

**Optional chaining**

If a nested property may not exist, optional chaining can prevent an error.

```javascript
const user = {
  name: "John"
};

console.log(user.address?.city);
```

Output:

```text
undefined
```

Without optional chaining, accessing `user.address.city` would throw an error because `address` is undefined.

# 🗁 Modifying JavaScript Objects

Once JSON text has been parsed into a JavaScript object, its properties can be modified.

**Adding a property:**

```javascript
const user = {
  name: "John",
  age: 28
};

user.email = "john@example.com";

console.log(user);
```

**Updating a property:**

```javascript
user.age = 29;

console.log(user.age);
```

**Deleting a property:**

```javascript
delete user.email;

console.log(user);
```

These operations modify the JavaScript object in memory. They do not automatically modify the original JSON text or a JSON file.

# 🗁 Working with JSON Objects in Python

Python converts JSON objects into dictionaries when using `json.loads()`.

Example:

```python
import json

json_text = """
{
    "id": 101,
    "name": "John",
    "email": "john@example.com"
}
"""

user = json.loads(json_text)

print(user["name"])
print(user["email"])
```

Output:

```text
John
john@example.com
```

**Accessing nested objects:**

```python
import json

json_text = """
{
    "name": "John",
    "address": {
        "city": "Dhaka",
        "country": "Bangladesh"
    }
}
"""

user = json.loads(json_text)

print(user["address"]["city"])
print(user["address"]["country"])
```

Output:

```text
Dhaka
Bangladesh
```

**Adding a property:**

```python
user["phone"] = "+8801712345678"

print(user["phone"])
```

**Updating a property:**

```python
user["address"]["city"] = "Gazipur"

print(user["address"]["city"])
```

**Deleting a property:**

```python
del user["phone"]

print(user)
```

# 🗁 Iterating Through Object Properties

Sometimes you need to process every property inside an object.

JavaScript:

```javascript
const user = {
  name: "John",
  age: 28,
  role: "developer"
};

for (const [key, value] of Object.entries(user)) {
  console.log(`${key}: ${value}`);
}
```

Output:

```text
name: John
age: 28
role: developer
```

Python:

```python
user = {
    "name": "John",
    "age": 28,
    "role": "developer"
}

for key, value in user.items():
    print(f"{key}: {value}")
```

Output:

```text
name: John
age: 28
role: developer
```

# 🗁 Converting Objects to JSON Text

After creating or modifying an object, you may need to serialize it into JSON text.

JavaScript:

```javascript
const user = {
  id: 101,
  name: "John",
  age: 28
};

const jsonText = JSON.stringify(user, null, 2);

console.log(jsonText);
```

Output:

```json
{
  "id": 101,
  "name": "John",
  "age": 28
}
```

Python:

```python
import json

user = {
    "id": 101,
    "name": "John",
    "age": 28
}

json_text = json.dumps(user, indent=2)

print(json_text)
```

Output:

```json
{
  "id": 101,
  "name": "John",
  "age": 28
}
```

# 🗁 Duplicate Object Keys

JSON syntax does not explicitly prohibit duplicate object names, but their behavior is not reliably interoperable.

Example:

```json
{
  "name": "John",
  "age": 28,
  "name": "Alice"
}
```

Different parsers or applications may handle duplicate keys differently. Many common parsers retain only the last occurrence.

For example, JavaScript:

```javascript
const data = JSON.parse(
  '{"name":"John","age":28,"name":"Alice"}'
);

console.log(data.name);
```

Output:

```text
Alice
```

Avoid duplicate keys. Every object should use unique property names to ensure predictable behavior across systems.

# 🗁 Object Property Order

JSON objects are conceptually unordered collections of name-value pairs.

Although many programming languages preserve or expose a predictable iteration order, applications should not depend on JSON object property order for meaning.

For example, these objects represent equivalent data:

```json
{
  "name": "John",
  "age": 28
}
```

```json
{
  "age": 28,
  "name": "John"
}
```

If order matters, use a JSON array instead.

# 🗁 Real-World API Object Example

A typical API response may contain several nested objects.

```json
{
  "success": true,
  "message": "User retrieved successfully",
  "data": {
    "id": 101,
    "name": "John Doe",
    "email": "john@example.com",
    "profile": {
      "jobTitle": "Software Engineer",
      "experience": 5
    },
    "address": {
      "city": "Dhaka",
      "country": "Bangladesh"
    }
  }
}
```

JavaScript access:

```javascript
const response = {
  success: true,
  message: "User retrieved successfully",
  data: {
    id: 101,
    name: "John Doe",
    profile: {
      jobTitle: "Software Engineer",
      experience: 5
    }
  }
};

console.log(response.data.name);
console.log(response.data.profile.jobTitle);
```

Output:

```text
John Doe
Software Engineer
```

# 🗁 Best Practices

* Use meaningful and consistent property names.
* Always use double quotes around JSON keys.
* Avoid duplicate object keys.
* Keep related information grouped logically.
* Avoid excessive nesting.
* Use consistent naming conventions across API responses.
* Check whether a property exists before accessing optional values.
* Use optional chaining in JavaScript when appropriate.
* Do not assume object property order has semantic meaning.
* Use schema validation when object structures must follow a defined contract.

# 🗁 Key Takeaways

* A JSON object is a collection of key-value pairs enclosed in `{}`.
* Object keys must be strings enclosed in double quotes.
* Values can be strings, numbers, booleans, null, objects, or arrays.
* Objects can contain other objects, creating nested structures.
* JavaScript uses dot notation and bracket notation to access properties.
* Python represents parsed JSON objects as dictionaries.
* Objects can be added, updated, deleted, and iterated over after parsing.
* `JSON.stringify()` and `json.dumps()` convert objects into JSON text.
* Avoid duplicate keys and do not rely on object property order.
