# JSON Data Types

JSON supports six fundamental data types: strings, numbers, booleans, null, objects, and arrays.

Understanding these data types is essential when designing API responses, processing application data, and exchanging information between different programming languages.

# 🗁 Overview of JSON Data Types

| Data Type | Example | Description |
| --- | --- | --- |
| String | `"Hello"` | Text enclosed in double quotes |
| Number | `100`, `25.5` | Integer or decimal numeric value |
| Boolean | `true`, `false` | Logical true or false value |
| Null | `null` | Represents an explicitly empty value |
| Object | `{"name": "John"}` | Collection of key-value pairs |
| Array | `[10, 20, 30]` | Ordered collection of values |

Example containing all six types:

```json
{
  "name": "John Doe",
  "age": 28,
  "isDeveloper": true,
  "middleName": null,
  "address": {
    "city": "Dhaka",
    "country": "Bangladesh"
  },
  "skills": [
    "JavaScript",
    "Python",
    "PHP"
  ]
}
```

# 🗁 String

A string represents textual data and must be enclosed in double quotation marks.

Example:

```json
{
  "name": "John",
  "email": "john@example.com",
  "country": "Bangladesh",
  "message": "Hello World"
}
```

Strings can contain:

* Letters and numbers.
* Spaces.
* Special characters.
* Unicode characters.
* Escape sequences.

**Escape sequences**

JSON uses backslashes to represent certain special characters inside strings.

| Escape Sequence | Meaning |
| --- | --- |
| `\"` | Double quotation mark |
| `\\` | Backslash |
| `\/` | Forward slash |
| `\b` | Backspace |
| `\f` | Form feed |
| `\n` | Newline |
| `\r` | Carriage return |
| `\t` | Horizontal tab |
| `\uXXXX` | Unicode code point escape using four hexadecimal digits |

Example:

```json
{
  "message": "Hello\nWorld",
  "quote": "He said, \"Welcome!\"",
  "path": "C:\\Users\\John"
}
```

The parsed message contains a newline between `Hello` and `World`.

**Invalid string examples**

Invalid:

```text
{
  "name": 'John'
}
```

Invalid:

```text
{
  "message": "Hello
  World"
}
```

JSON strings require double quotes, and literal control characters such as an unescaped newline cannot appear inside a string.

# 🗁 Number

JSON supports numeric values, including integers, decimal numbers, and exponent notation.

Examples:

```json
{
  "age": 28,
  "price": 1250.50,
  "temperature": -5.5,
  "quantity": 100,
  "largeNumber": 1.5e6,
  "smallNumber": 2.5e-4
}
```

**Number formats**

| Format | Example | Meaning |
| --- | --- | --- |
| Integer | `100` | Whole number |
| Decimal | `25.75` | Fractional number |
| Negative | `-50` | Negative number |
| Exponent | `1.5e6` | 1.5 × 10⁶ |
| Negative exponent | `2.5e-4` | 2.5 × 10⁻⁴ |

**Invalid JSON numbers**

JSON does not permit:

```text
{
  "a": 001,
  "b": +100,
  "c": .5,
  "d": 10.,
  "e": NaN,
  "f": Infinity
}
```

Valid alternatives:

```json
{
  "a": 1,
  "b": 100,
  "c": 0.5,
  "d": 10,
  "e": null,
  "f": null
}
```

Use `null` only when it accurately represents the intended data. It is not automatically equivalent to `NaN` or infinity.

**Important: Numeric precision**

JSON syntax allows large numeric values, but programming languages may represent them differently.

For example, JavaScript uses IEEE 754 double-precision numbers for ordinary JSON numbers. Integers larger than `Number.MAX_SAFE_INTEGER` cannot all be represented exactly.

JavaScript example:

```javascript
const value = 9007199254740993;

console.log(value);
console.log(Number.MAX_SAFE_INTEGER);
```

For identifiers, account numbers, or very large integer values, consider representing them as strings:

```json
{
  "accountNumber": "9007199254740993"
}
```

This avoids accidental numeric precision loss when systems parse the value.

# 🗁 Boolean

A boolean represents one of two logical values:

* `true`
* `false`

JSON booleans are written in lowercase.

Example:

```json
{
  "isActive": true,
  "isVerified": false,
  "hasPermission": true,
  "isDeleted": false
}
```

Booleans are commonly used for:

* Feature flags.
* Account status.
* Permission checks.
* Configuration options.
* API success indicators.

**Invalid boolean values**

```text
{
  "active": True,
  "verified": False,
  "enabled": "true"
}
```

`True` and `False` are not valid JSON boolean literals. `"true"` is a string, not a boolean.

# 🗁 Null

`null` represents an explicitly empty or absent value.

Example:

```json
{
  "name": "John",
  "middleName": null,
  "phone": null,
  "email": "john@example.com"
}
```

This indicates that `middleName` and `phone` currently have no value.

**Null vs missing property**

These two JSON objects are different:

```json
{
  "name": "John",
  "phone": null
}
```

```json
{
  "name": "John"
}
```

In the first object, `phone` exists and has the value `null`.

In the second object, `phone` is not present.

This distinction matters when designing APIs and updating database records.

**Null vs empty string**

```json
{
  "name": "",
  "middleName": null
}
```

* `""` is an existing string containing zero characters.
* `null` represents the absence of a value.

# 🗁 Object

An object stores data as key-value pairs.

Objects use curly braces `{}`.

Example:

```json
{
  "user": {
    "id": 101,
    "name": "John",
    "email": "john@example.com"
  }
}
```

Each key must be a double-quoted string.

Object values can be any JSON data type, including another object or an array.

Objects are useful for representing:

* Users.
* Products.
* Orders.
* API responses.
* Application configurations.

Example:

```json
{
  "product": {
    "id": 501,
    "name": "Laptop",
    "price": 75000,
    "available": true
  }
}
```

# 🗁 Array

An array represents an ordered collection of values.

Arrays use square brackets `[]`.

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

Arrays can contain values of different JSON data types.

Example:

```json
{
  "mixedValues": [
    "Hello",
    100,
    true,
    null,
    {
      "name": "John"
    },
    [10, 20, 30]
  ]
}
```

Array order is preserved. Array indexes are zero-based when accessed through common programming languages such as JavaScript and Python.

# 🗁 Nested Data Types

JSON allows objects and arrays to contain other objects and arrays.

Example:

```json
{
  "company": "Example Technologies",
  "employees": [
    {
      "id": 101,
      "name": "John",
      "active": true,
      "skills": [
        "JavaScript",
        "Node.js"
      ]
    },
    {
      "id": 102,
      "name": "Alice",
      "active": false,
      "skills": [
        "Python",
        "Django"
      ]
    }
  ]
}
```

This structure contains:

* A string representing the company name.
* An array of employees.
* Objects representing individual employees.
* Boolean employee status.
* Nested arrays containing skills.

Nested structures are common in real-world API responses.

# 🗁 JSON Data Types vs JavaScript Types

JSON and JavaScript have related but different type systems.

| JSON | JavaScript After Parsing |
| --- | --- |
| String | `string` |
| Number | `number` |
| Boolean | `boolean` |
| Null | `null` |
| Object | `object` |
| Array | `object` |

JavaScript example:

```javascript
const json = `{
  "name": "John",
  "age": 28,
  "active": true,
  "middleName": null,
  "skills": ["JavaScript", "Python"],
  "address": {
    "city": "Dhaka"
  }
}`;

const data = JSON.parse(json);

console.log(typeof data.name);
console.log(typeof data.age);
console.log(typeof data.active);
console.log(typeof data.middleName);
console.log(typeof data.skills);
console.log(Array.isArray(data.skills));
```

Output:

```text
string
number
boolean
object
object
true
```

In JavaScript, `typeof null` returns `"object"` because of a historical language behavior. Use `value === null` to check for null explicitly.

# 🗁 JSON Data Types in Python

Python converts JSON values into corresponding Python types when using `json.loads()`.

| JSON Type | Python Type |
| --- | --- |
| String | `str` |
| Number without decimal or exponent | `int` |
| Number with decimal or exponent | `float` |
| Boolean | `bool` |
| Null | `None` |
| Object | `dict` |
| Array | `list` |

Example:

```python
import json

json_text = """
{
    "name": "John",
    "age": 28,
    "price": 1250.50,
    "active": true,
    "middleName": null,
    "skills": ["JavaScript", "Python"],
    "address": {
        "city": "Dhaka"
    }
}
"""

data = json.loads(json_text)

for key, value in data.items():
    print(key, type(value).__name__)
```

Output:

```text
name str
age int
price float
active bool
middleName NoneType
skills list
address dict
```

# 🗁 JSON Does Not Support Other JavaScript Values

JSON has a deliberately limited set of data types.

The following JavaScript values cannot be represented directly as JSON values:

| JavaScript Value | JSON Support |
| --- | --- |
| `undefined` | Not supported |
| Function | Not supported |
| Symbol | Not supported |
| `BigInt` | Not supported directly |
| `NaN` | Not supported |
| `Infinity` | Not supported |
| `-Infinity` | Not supported |
| `Date` | No dedicated date type |

When JavaScript serializes values, special conversion rules apply.

Example:

```javascript
const data = {
  name: "John",
  age: undefined,
  active: true,
  score: NaN,
  createdAt: new Date("2026-11-12T00:00:00Z")
};

console.log(JSON.stringify(data));
```

Output:

```text
{"name":"John","active":true,"score":null,"createdAt":"2026-11-12T00:00:00.000Z"}
```

Notice:

* The `age` property is omitted because its value is `undefined`.
* `NaN` becomes `null`.
* A `Date` is converted to an ISO-formatted string through its serialization behavior.

Do not assume every JavaScript value will survive JSON serialization unchanged.

# 🗁 Best Practices

* Use strings for identifiers that are not intended for arithmetic.
* Use booleans for true/false states rather than strings.
* Distinguish between `null`, empty strings, and missing properties.
* Avoid unnecessary mixing of different types in the same array.
* Keep object structures consistent across API responses.
* Consider numeric precision when transferring large integers.
* Use ISO 8601 strings for date and time values when no separate schema defines another representation.
* Validate data types at application boundaries.
* Document the expected types in API contracts.

# 🗁 Key Takeaways

* JSON supports six fundamental value types: string, number, boolean, null, object, and array.
* Strings and object keys require double quotation marks.
* JSON numbers support integer, decimal, and exponent notation.
* Boolean literals are lowercase.
* `null`, an empty string, and a missing property represent different states.
* Objects contain key-value pairs, while arrays contain ordered values.
* Objects and arrays can be nested to represent complex data.
* JSON does not support JavaScript-specific values such as functions, `undefined`, `NaN`, and `Infinity`.
* Programming languages map JSON values to their own native types.
