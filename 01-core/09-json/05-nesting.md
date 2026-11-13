# Nested JSON Structures

Nested JSON refers to JSON objects or arrays placed inside other JSON objects or arrays.

Nesting allows you to represent complex and hierarchical data structures while keeping related information organized.

It is commonly used in REST APIs, configuration files, application state, database records, and data exchange between applications.

# 🗁 What Is Nested JSON?

A JSON object can contain another object as a property value.

```json
{
  "name": "John",
  "address": {
    "city": "Dhaka",
    "country": "Bangladesh"
  }
}
```

In this example:

* The root object contains `name` and `address`.
* The `address` property contains another JSON object.
* The nested object contains `city` and `country`.

The `address` object is nested inside the root object.

# 🗁 Nested Objects

A nested object is an object placed inside another object.

```json
{
  "user": {
    "id": 101,
    "name": "John",
    "contact": {
      "email": "john@example.com",
      "phone": "+880123456789"
    }
  }
}
```

The structure contains:

* Root object
  * `user` object
    * `id`
    * `name`
    * `contact` object
      * `email`
      * `phone`

Nested objects are useful when multiple properties belong to a specific entity.

# 🗁 Nested Arrays

An array can contain other arrays.

```json
{
  "matrix": [
    [10, 20, 30],
    [40, 50, 60],
    [70, 80, 90]
  ]
}
```

This structure represents a two-dimensional collection.

Another example:

```json
{
  "departments": [
    ["Development", "Engineering"],
    ["Sales", "Marketing"],
    ["Finance", "Accounting"]
  ]
}
```

# 🗁 Objects Inside Arrays

An array can contain multiple objects.

This is one of the most common patterns in API responses.

```json
{
  "users": [
    {
      "id": 1,
      "name": "John",
      "role": "admin"
    },
    {
      "id": 2,
      "name": "David",
      "role": "editor"
    },
    {
      "id": 3,
      "name": "Sarah",
      "role": "user"
    }
  ]
}
```

Here, `users` is an array containing three objects.

Each object represents an individual user.

# 🗁 Arrays Inside Objects

An object can contain an array as one of its properties.

```json
{
  "name": "John",
  "skills": [
    "JavaScript",
    "Python",
    "PHP"
  ]
}
```

This structure represents one person with multiple skills.

# 🗁 Objects and Arrays Combined

JSON supports combining objects and arrays at different nesting levels.

```json
{
  "company": {
    "name": "Example Technologies",
    "location": {
      "city": "Dhaka",
      "country": "Bangladesh"
    },
    "departments": [
      {
        "name": "Development",
        "employees": [
          {
            "id": 101,
            "name": "John",
            "skills": ["JavaScript", "React"]
          },
          {
            "id": 102,
            "name": "David",
            "skills": ["Python", "Django"]
          }
        ]
      },
      {
        "name": "Marketing",
        "employees": [
          {
            "id": 103,
            "name": "Sarah",
            "skills": ["SEO", "Content Marketing"]
          }
        ]
      }
    ]
  }
}
```

This example contains:

* Nested objects.
* Arrays of objects.
* Objects containing arrays.
* Multiple levels of nesting.

# 🗁 Accessing Nested JSON in JavaScript

After parsing JSON text with `JSON.parse()`, you can access nested properties using dot notation or bracket notation.

```javascript
const jsonString = `{
  "user": {
    "id": 101,
    "name": "John",
    "address": {
      "city": "Dhaka",
      "country": "Bangladesh"
    }
  }
}`;

const data = JSON.parse(jsonString);

// Access root-level properties
console.log(data.user.id);

// Access nested properties
console.log(data.user.name);
console.log(data.user.address.city);
console.log(data.user.address.country);
```

You can also use bracket notation:

```javascript
console.log(data["user"]["name"]);
console.log(data["user"]["address"]["city"]);
```

Both approaches access the same properties.

# 🗁 Accessing Nested JSON in Python

Python converts JSON objects into dictionaries and JSON arrays into lists.

Use dictionary keys and list indexes to access nested values.

```python
import json

json_string = """
{
    "user": {
        "id": 101,
        "name": "John",
        "address": {
            "city": "Dhaka",
            "country": "Bangladesh"
        }
    }
}
"""

data = json.loads(json_string)

# Access root-level properties
print(data["user"]["id"])

# Access nested properties
print(data["user"]["name"])
print(data["user"]["address"]["city"])
print(data["user"]["address"]["country"])
```

# 🗁 Accessing Objects Inside Arrays

When an array contains objects, combine property access with array indexing.

**JSON**

```json
{
  "users": [
    {
      "id": 1,
      "name": "John"
    },
    {
      "id": 2,
      "name": "David"
    }
  ]
}
```

**JavaScript**

```javascript
const data = JSON.parse(jsonString);

console.log(data.users[0].name); // John
console.log(data.users[1].name); // David
```

**Python**

```python
data = json.loads(json_string)

print(data["users"][0]["name"])  # John
print(data["users"][1]["name"])  # David
```

# 🗁 Accessing Arrays Inside Nested Objects

Consider a JSON object containing departments and their employees.

```json
{
  "company": {
    "name": "Example Technologies",
    "departments": [
      {
        "name": "Development",
        "employees": ["John", "David"]
      },
      {
        "name": "Marketing",
        "employees": ["Sarah", "Michael"]
      }
    ]
  }
}
```

**JavaScript**

```javascript
const data = JSON.parse(jsonString);

console.log(data.company.departments[0].name);
console.log(data.company.departments[0].employees[1]);

// Development
// David
```

**Python**

```python
data = json.loads(json_string)

print(data["company"]["departments"][0]["name"])
print(data["company"]["departments"][0]["employees"][1])

# Development
# David
```

# 🗁 Iterating Over Nested JSON in JavaScript

You can use nested loops to process arrays containing objects and other arrays.

```javascript
const data = {
  "departments": [
    {
      "name": "Development",
      "employees": ["John", "David"]
    },
    {
      "name": "Marketing",
      "employees": ["Sarah", "Michael"]
    }
  ]
};

for (const department of data.departments) {
  console.log(`Department: ${department.name}`);

  for (const employee of department.employees) {
    console.log(`Employee: ${employee}`);
  }
}
```

You can also use array methods:

```javascript
const employees = data.departments.flatMap(
  department => department.employees
);

console.log(employees);
```

The `flatMap()` method transforms each department into its employee array and combines the results into one array.

# 🗁 Iterating Over Nested JSON in Python

Python also supports nested loops for processing nested structures.

```python
data = {
    "departments": [
        {
            "name": "Development",
            "employees": ["John", "David"]
        },
        {
            "name": "Marketing",
            "employees": ["Sarah", "Michael"]
        }
    ]
}

for department in data["departments"]:
    print(f"Department: {department['name']}")

    for employee in department["employees"]:
        print(f"Employee: {employee}")
```

You can flatten the employee collections using a list comprehension:

```python
employees = [
    employee
    for department in data["departments"]
    for employee in department["employees"]
]

print(employees)
```

# 🗁 Handling Missing Nested Properties in JavaScript

Not every API response contains all expected properties.

Attempting to access a property that does not exist can cause errors when an intermediate value is `undefined` or `null`.

Consider:

```javascript
const user = {
  "name": "John"
};

console.log(user.address.city);
```

This causes a `TypeError` because `address` does not exist.

Use optional chaining (`?.`) to safely access nested properties.

```javascript
console.log(user.address?.city); // undefined
```

Optional chaining prevents an error when an intermediate property is `null` or `undefined`.

You can provide a default value using the nullish coalescing operator (`??`):

```javascript
const city = user.address?.city ?? "Unknown";

console.log(city); // Unknown
```

You can also use destructuring with default values:

```javascript
const { address = {} } = user;

console.log(address.city ?? "Unknown");
```

# 🗁 Handling Missing Nested Properties in Python

Python raises a `KeyError` when you access a dictionary key that does not exist.

```python
user = {
    "name": "John"
}

print(user["address"]["city"])
```

Use the dictionary `get()` method to provide safe access.

```python
address = user.get("address", {})

city = address.get("city", "Unknown")

print(city)  # Unknown
```

For deeply nested structures, you can define a reusable helper:

```python
def get_nested_value(data, keys, default=None):
    current = data

    for key in keys:
        if isinstance(current, dict):
            current = current.get(key)
        elif isinstance(current, list) and isinstance(key, int):
            if 0 <= key < len(current):
                current = current[key]
            else:
                return default
        else:
            return default

        if current is None:
            return default

    return current


user = {
    "name": "John",
    "address": {
        "city": "Dhaka"
    }
}

city = get_nested_value(
    user,
    ["address", "city"],
    "Unknown"
)

print(city)  # Dhaka
```

This helper supports dictionary keys and list indexes. It returns the supplied default when a path cannot be traversed.

# 🗁 Working with API Responses

Real-world APIs frequently return nested JSON structures.

Consider an e-commerce API response:

```json
{
  "success": true,
  "data": {
    "order": {
      "id": "ORD-1001",
      "status": "processing",
      "customer": {
        "id": 501,
        "name": "John",
        "email": "john@example.com"
      },
      "shipping": {
        "address": {
          "city": "Dhaka",
          "country": "Bangladesh"
        }
      },
      "items": [
        {
          "product_id": 101,
          "name": "Laptop",
          "quantity": 1,
          "price": 75000
        },
        {
          "product_id": 102,
          "name": "Mouse",
          "quantity": 2,
          "price": 1200
        }
      ]
    }
  }
}
```

**Accessing the order information in JavaScript**

```javascript
const response = JSON.parse(jsonString);

const order = response.data.order;

console.log(order.id);
console.log(order.customer.name);
console.log(order.shipping.address.city);

for (const item of order.items) {
  console.log(item.name, item.quantity, item.price);
}
```

**Accessing the order information in Python**

```python
response = json.loads(json_string)

order = response["data"]["order"]

print(order["id"])
print(order["customer"]["name"])
print(order["shipping"]["address"]["city"])

for item in order["items"]:
    print(item["name"], item["quantity"], item["price"])
```

# 🗁 Traversing Nested JSON Dynamically

Sometimes you do not know the exact structure of a JSON object in advance.

In JavaScript, `Object.entries()` can help you inspect object properties dynamically.

```javascript
function inspect(value, depth = 0) {
  const indentation = "  ".repeat(depth);

  if (Array.isArray(value)) {
    value.forEach((item, index) => {
      console.log(`${indentation}[${index}]`);
      inspect(item, depth + 1);
    });

    return;
  }

  if (value !== null && typeof value === "object") {
    for (const [key, item] of Object.entries(value)) {
      console.log(`${indentation}${key}:`);

      inspect(item, depth + 1);
    }

    return;
  }

  console.log(`${indentation}${value}`);
}
```

In Python, you can use recursion to inspect nested dictionaries and lists.

```python
def inspect(value, depth=0):
    indentation = "  " * depth

    if isinstance(value, dict):
        for key, item in value.items():
            print(f"{indentation}{key}:")
            inspect(item, depth + 1)

    elif isinstance(value, list):
        for index, item in enumerate(value):
            print(f"{indentation}[{index}]")
            inspect(item, depth + 1)

    else:
        print(f"{indentation}{value}")
```

These approaches are useful for debugging unknown API responses and exploring complex data structures.

# 🗁 Common Nested JSON Mistakes

**Incorrect nesting structure**

Invalid:

```json
{
  "user": {
    "name": "John",
    "address": "city": "Dhaka"
  }
}
```

Correct:

```json
{
  "user": {
    "name": "John",
    "address": {
      "city": "Dhaka"
    }
  }
}
```

**Missing closing braces**

Invalid:

```json
{
  "user": {
    "name": "John"
}
```

Correct:

```json
{
  "user": {
    "name": "John"
  }
}
```

**Accessing an object as an array**

```javascript
const data = {
  user: {
    name: "John"
  }
};

console.log(data.user[0]); // undefined
```

The `user` property is an object, not an array.

Correct:

```javascript
console.log(data.user.name);
```

**Accessing an array as an object**

```javascript
const data = {
  users: [
    {
      name: "John"
    }
  ]
};

console.log(data.users.name); // undefined
```

Correct:

```javascript
console.log(data.users[0].name);
```

**Assuming every nested property exists**

Always consider missing properties, `null` values, empty arrays, and unexpected data types when processing external JSON.

# 🗁 Best Practices

* Group related properties into nested objects.
* Use arrays when representing multiple records or values.
* Keep nesting depth reasonable to improve readability.
* Use consistent structures for collections of similar objects.
* Handle missing properties and `null` values safely.
* Validate external JSON before using deeply nested data.
* Use meaningful property names.
* Avoid unnecessary nesting when a simpler structure communicates the same information.
* Use reusable helper functions when accessing deeply nested data repeatedly.
* Document complex API response structures with schemas or examples.

# 🗁 Summary

* Nested JSON contains objects or arrays inside other objects or arrays.
* Objects can contain objects, arrays, or primitive values.
* Arrays can contain objects, arrays, or primitive values.
* Access nested values using property names and array indexes.
* JavaScript uses objects and arrays after parsing JSON.
* Python uses dictionaries and lists after parsing JSON.
* Nested loops and recursion help process complex structures.
* Optional chaining in JavaScript and `dict.get()` in Python help handle missing properties.
* Nested JSON is widely used in REST APIs and application data exchange.
