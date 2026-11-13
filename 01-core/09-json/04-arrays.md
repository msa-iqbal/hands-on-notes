# JSON Arrays

JSON arrays are ordered collections of values enclosed in square brackets (`[]`). They allow you to store multiple values in a single JSON structure.

Arrays are commonly used in API responses, configuration files, application data, and database communication.

# 🗁 What Is a JSON Array?

A JSON array contains zero or more values separated by commas.

```json
{
  "name": "John",
  "skills": ["JavaScript", "Python", "PHP"]
}
```

In this example, `skills` is an array containing three string values.

# 🗁 Array Syntax

The basic syntax of a JSON array is:

```json
[
  "value1",
  "value2",
  "value3"
]
```

An array can also be assigned to an object property:

```json
{
  "colors": ["red", "green", "blue"]
}
```

# 🗁 JSON Array Value Types

JSON arrays can contain any valid JSON value.

**Strings**

```json
["JavaScript", "Python", "PHP"]
```

**Numbers**

```json
[10, 20, 30, 40]
```

**Booleans**

```json
[true, false, true]
```

**Null values**

```json
["John", null, "David"]
```

**Objects**

```json
[
  {
    "id": 1,
    "name": "John"
  },
  {
    "id": 2,
    "name": "David"
  }
]
```

**Nested arrays**

```json
[
  ["red", "green"],
  ["blue", "yellow"]
]
```

**Mixed values**

```json
[
  "John",
  25,
  true,
  null,
  {
    "city": "Dhaka"
  },
  [10, 20, 30]
]
```

Although mixed types are valid, keeping array elements consistent usually makes application code easier to understand and maintain.

# 🗁 Empty Arrays

An empty array contains no elements.

```json
{
  "users": [],
  "products": []
}
```

Empty arrays are useful when a collection exists but currently has no data.

# 🗁 Array Indexing

Array elements are accessed using their index positions.

Most programming languages, including JavaScript and Python, use zero-based indexing.

| Element | Index |
| --- | ---: |
| JavaScript | 0 |
| Python | 1 |
| PHP | 2 |

For example:

```json
{
  "languages": ["JavaScript", "Python", "PHP"]
}
```

The values are positioned as follows:

* Index `0`: `JavaScript`
* Index `1`: `Python`
* Index `2`: `PHP`

The indexes start at `0`, not `1`.

# 🗁 Accessing Array Elements in JavaScript

After parsing JSON, JavaScript provides access to array elements using square brackets.

```javascript
const jsonData = `{
  "languages": ["JavaScript", "Python", "PHP"]
}`;

const data = JSON.parse(jsonData);

console.log(data.languages[0]); // JavaScript
console.log(data.languages[1]); // Python
console.log(data.languages[2]); // PHP
```

You can also access the array length:

```javascript
console.log(data.languages.length); // 3
```

Accessing an index outside the array's range returns `undefined`.

```javascript
console.log(data.languages[5]); // undefined
```

# 🗁 Accessing Array Elements in Python

Python converts JSON arrays into native Python lists.

```python
import json

json_data = """
{
    "languages": ["JavaScript", "Python", "PHP"]
}
"""

data = json.loads(json_data)

print(data["languages"][0])  # JavaScript
print(data["languages"][1])  # Python
print(data["languages"][2])  # PHP
```

You can get the number of elements using `len()`:

```python
print(len(data["languages"]))  # 3
```

Accessing an invalid index raises an `IndexError`.

# 🗁 Iterating Over JSON Arrays in JavaScript

Use loops to process every element in an array.

**Using `for...of`**

```javascript
const languages = ["JavaScript", "Python", "PHP"];

for (const language of languages) {
  console.log(language);
}
```

**Using `forEach()`**

```javascript
languages.forEach((language) => {
  console.log(language);
});
```

**Using a traditional `for` loop**

```javascript
for (let i = 0; i < languages.length; i++) {
  console.log(languages[i]);
}
```

# 🗁 Iterating Over JSON Arrays in Python

Python provides several ways to iterate over lists.

**Using a `for` loop**

```python
languages = ["JavaScript", "Python", "PHP"]

for language in languages:
    print(language)
```

**Using `enumerate()`**

```python
for index, language in enumerate(languages):
    print(index, language)
```

**Using a list comprehension**

```python
uppercase_languages = [
    language.upper()
    for language in languages
]

print(uppercase_languages)
```

# 🗁 Modifying Arrays After Parsing

JSON itself is a data format, not a programming language. You cannot modify a JSON document directly through array methods.

Instead, parse the JSON into a native data structure, modify it, and serialize it again.

**JavaScript**

```javascript
const data = JSON.parse(`{
  "languages": ["JavaScript", "Python"]
}`);

// Add an element
data.languages.push("PHP");

// Update an element
data.languages[0] = "TypeScript";

// Remove the last element
data.languages.pop();

// Serialize back into JSON
const updatedJson = JSON.stringify(data, null, 2);

console.log(updatedJson);
```

**Python**

```python
import json

data = json.loads("""
{
    "languages": ["JavaScript", "Python"]
}
""")

# Add an element
data["languages"].append("PHP")

# Update an element
data["languages"][0] = "TypeScript"

# Remove the last element
data["languages"].pop()

# Serialize back into JSON
updated_json = json.dumps(data, indent=2)

print(updated_json)
```

# 🗁 Common Array Operations

| Operation | JavaScript | Python |
| --- | --- | --- |
| Add element | `push()` | `append()` |
| Remove last element | `pop()` | `pop()` |
| Access element | `array[index]` | `array[index]` |
| Get length | `array.length` | `len(array)` |
| Iterate | `for...of` | `for` |
| Check element | `includes()` | `in` |
| Sort | `sort()` | `sort()` |

These operations work on native arrays and lists after JSON parsing. They are not JSON syntax.

# 🗁 Arrays of Objects

One of the most common JSON patterns is an array containing multiple objects.

```json
{
  "users": [
    {
      "id": 1,
      "name": "John",
      "email": "john@example.com"
    },
    {
      "id": 2,
      "name": "David",
      "email": "david@example.com"
    },
    {
      "id": 3,
      "name": "Sarah",
      "email": "sarah@example.com"
    }
  ]
}
```

Access an individual object's properties using its array index.

**JavaScript**

```javascript
const data = JSON.parse(jsonString);

console.log(data.users[0].name);
console.log(data.users[1].email);
```

**Python**

```python
data = json.loads(json_string)

print(data["users"][0]["name"])
print(data["users"][1]["email"])
```

# 🗁 Array Order

JSON arrays are ordered collections.

Their element positions are significant and must be preserved.

For example:

```json
["JavaScript", "Python", "PHP"]
```

is different in order from:

```json
["PHP", "JavaScript", "Python"]
```

This is particularly important when working with:

* Product lists.
* Search results.
* Navigation menus.
* Transaction histories.
* Ordered steps in a workflow.
* API pagination results.

Unlike arrays, JSON object member ordering should not be relied upon for application logic.

# 🗁 JSON Arrays in API Responses

APIs frequently return arrays to represent collections of resources.

**Example: Users API Response**

```json
{
  "success": true,
  "total": 3,
  "users": [
    {
      "id": 101,
      "name": "John",
      "role": "admin"
    },
    {
      "id": 102,
      "name": "David",
      "role": "editor"
    },
    {
      "id": 103,
      "name": "Sarah",
      "role": "user"
    }
  ]
}
```

**Example: Products API Response**

```json
{
  "success": true,
  "products": [
    {
      "id": 1,
      "name": "Laptop",
      "price": 75000,
      "in_stock": true
    },
    {
      "id": 2,
      "name": "Keyboard",
      "price": 2500,
      "in_stock": true
    },
    {
      "id": 3,
      "name": "Mouse",
      "price": 1200,
      "in_stock": false
    }
  ]
}
```

**Example: Orders API Response**

```json
{
  "order_id": "ORD-1001",
  "customer": "John",
  "items": [
    {
      "product": "Laptop",
      "quantity": 1,
      "price": 75000
    },
    {
      "product": "Mouse",
      "quantity": 2,
      "price": 1200
    }
  ],
  "total": 77400
}
```

# 🗁 Serialization and Deserialization

When working with JSON arrays, serialization and deserialization are important operations.

**Deserialization:** Converts JSON text into a native programming-language data structure.

**Serialization:** Converts a native data structure into JSON text.

JavaScript:

```javascript
const jsonString = '["JavaScript", "Python", "PHP"]';

// Deserialization
const languages = JSON.parse(jsonString);

// Serialization
const updatedJson = JSON.stringify(languages);

console.log(updatedJson);
```

Python:

```python
import json

json_string = '["JavaScript", "Python", "PHP"]'

# Deserialization
languages = json.loads(json_string)

# Serialization
updated_json = json.dumps(languages)

print(updated_json)
```

# 🗁 JSON Arrays vs Programming-Language Arrays

JSON arrays and programming-language arrays are related, but they are not identical.

| Feature | JSON Array | JavaScript Array | Python List |
| --- | --- | --- | --- |
| Syntax | `[]` | `[]` | `[]` |
| Ordered | Yes | Yes | Yes |
| Mixed values | Yes | Yes | Yes |
| Mutable | Not applicable | Yes | Yes |
| Supports methods | No | Yes | Yes |
| Data representation | Text format | Runtime object | Runtime object |

JSON arrays are part of a data-interchange format. JavaScript arrays and Python lists are native programming-language structures.

# 🗁 Common Mistakes

**Using parentheses instead of square brackets**

Invalid:

```json
{
  "languages": ("JavaScript", "Python")
}
```

Correct:

```json
{
  "languages": ["JavaScript", "Python"]
}
```

**Adding a trailing comma**

Invalid:

```json
{
  "languages": [
    "JavaScript",
    "Python",
  ]
}
```

Correct:

```json
{
  "languages": [
    "JavaScript",
    "Python"
  ]
}
```

**Using an incorrect array index**

JavaScript:

```javascript
const languages = ["JavaScript", "Python"];

console.log(languages[2]); // undefined
```

Python:

```python
languages = ["JavaScript", "Python"]

print(languages[2])  # IndexError
```

**Forgetting that JSON arrays become native data structures**

JavaScript:

```javascript
const jsonString = '["JavaScript", "Python"]';

const languages = JSON.parse(jsonString);

languages.push("PHP");
```

Python:

```python
import json

json_string = '["JavaScript", "Python"]'

languages = json.loads(json_string)

languages.append("PHP")
```

# 🗁 Best Practices

* Use arrays when representing collections of values.
* Keep array elements consistent in structure and type whenever practical.
* Use arrays of objects for collections of structured records.
* Remember that array indexing starts at zero in JavaScript and Python.
* Preserve element order when processing or transforming arrays.
* Handle empty arrays in application logic.
* Validate array contents when receiving external API data.
* Avoid assuming that an array always contains at least one element.
* Use meaningful property names when storing arrays inside objects.

# 🗁 Summary

* JSON arrays are ordered collections enclosed in square brackets (`[]`).
* Arrays can contain strings, numbers, booleans, null, objects, and other arrays.
* Empty arrays are represented by `[]`.
* Array indexes start at zero in JavaScript and Python.
* JSON arrays commonly represent collections in API responses.
* JavaScript converts JSON arrays into native `Array` objects.
* Python converts JSON arrays into native `list` objects.
* Modify native data structures after parsing, then serialize them back into JSON when needed.
