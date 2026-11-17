# JavaScript `Map`

A JavaScript `Map` is a built-in collection that stores data as key-value pairs.

Unlike ordinary JavaScript objects, a Map allows keys of any data type, including:

* Strings
* Numbers
* Booleans
* Objects
* Arrays
* Functions
* Symbols

A Map maintains the insertion order of its entries and provides methods for adding, retrieving, checking, and deleting key-value pairs.

Maps are useful for:

* Managing key-value data.
* Storing objects using object references as keys.
* Caching computed results.
* Managing user sessions.
* Counting occurrences of values.
* Implementing lookup tables.
* Tracking application state.
* Building data structures such as graphs.

# 🗁 Creating a Map

### 🖆 Map Constructor

Create an empty Map using the `Map` constructor.

```javascript
const users = new Map();

console.log(users);
// Map(0) {}
```

### 🖆 Creating a Map with Initial Values

A Map can be initialized with an iterable containing key-value pairs.

```javascript
const user = new Map([
    ["name", "Alice"],
    ["age", 25],
    ["email", "alice@example.com"]
]);

console.log(user);
```

Each entry must contain a key and a value.

### 🖆 Creating a Map from an Array

```javascript
const entries = [
    ["id", 101],
    ["name", "Alice"],
    ["role", "admin"]
];

const user = new Map(entries);

console.log(user.get("name"));
// Alice
```

### 🖆 Creating a Map from Another Map

```javascript
const original = new Map([
    ["name", "Alice"],
    ["age", 25]
]);

const copied = new Map(original);

console.log(copied);
// Map(2) { 'name' => 'Alice', 'age' => 25 }
```

The new Map has separate collection structure, but object values and keys are still shared references.

# 🗁 Map Properties and Methods

JavaScript provides several properties and methods for working with Maps.

| Property or method | Description |
| --- | --- |
| `size` | Returns the number of entries |
| `set()` | Adds or updates a key-value pair |
| `get()` | Retrieves a value by key |
| `has()` | Checks whether a key exists |
| `delete()` | Removes an entry by key |
| `clear()` | Removes all entries |
| `keys()` | Returns an iterator of keys |
| `values()` | Returns an iterator of values |
| `entries()` | Returns an iterator of key-value pairs |
| `forEach()` | Executes a callback for each entry |
| `Symbol.iterator` | Provides the default entry iterator |

# 🗁 Adding and Updating Entries with `set()`

The `set()` method adds a new key-value pair or updates an existing entry.

### 🖆 Basic Usage

```javascript
const user = new Map();

user.set("name", "Alice");
user.set("age", 25);
user.set("role", "admin");

console.log(user);
```

### 🖆 Updating an Existing Entry

If a key already exists, `set()` replaces its associated value.

```javascript
const user = new Map();

user.set("name", "Alice");

user.set("name", "Bob");

console.log(user.get("name"));
// Bob
```

Updating an existing key does not increase the Map's size.

```javascript
console.log(user.size);
// 1
```

### 🖆 Chaining `set()`

The `set()` method returns the Map itself, so multiple calls can be chained.

```javascript
const user = new Map();

user
    .set("name", "Alice")
    .set("age", 25)
    .set("role", "admin");

console.log(user.size);
// 3
```

# 🗁 Retrieving Values with `get()`

The `get()` method retrieves the value associated with a key.

If the key does not exist, it returns `undefined`.

### 🖆 Basic Usage

```javascript
const user = new Map([
    ["name", "Alice"],
    ["age", 25]
]);

console.log(user.get("name"));
// Alice

console.log(user.get("age"));
// 25
```

### 🖆 Accessing a Missing Key

```javascript
console.log(user.get("email"));
// undefined
```

### 🖆 Distinguishing Missing Keys from Undefined Values

A Map can explicitly store `undefined` as a value.

Therefore, `get()` alone cannot distinguish between a missing key and a key whose value is `undefined`.

Use `has()` when this distinction matters.

```javascript
const settings = new Map();

settings.set("theme", undefined);

console.log(settings.get("theme"));
// undefined

console.log(settings.has("theme"));
// true

console.log(settings.has("language"));
// false
```

# 🗁 Checking Keys with `has()`

The `has()` method checks whether a key exists in a Map.

It returns `true` if the key exists; otherwise, it returns `false`.

### 🖆 Basic Usage

```javascript
const user = new Map([
    ["name", "Alice"],
    ["age", 25]
]);

console.log(user.has("name"));
// true

console.log(user.has("email"));
// false
```

### 🖆 Checking User Permissions

```javascript
const permissions = new Map([
    ["read", true],
    ["write", true],
    ["delete", false]
]);

console.log(permissions.has("read"));
// true

console.log(permissions.has("publish"));
// false
```

Remember that `has()` checks whether the key exists, not whether its associated value is truthy.

```javascript
console.log(permissions.has("delete"));
// true

console.log(permissions.get("delete"));
// false
```

# 🗁 Removing Entries

### 🖆 Using `delete()`

The `delete()` method removes an entry using its key.

It returns `true` if the entry was found and removed, or `false` if the key did not exist.

```javascript
const user = new Map([
    ["name", "Alice"],
    ["age", 25],
    ["role", "admin"]
]);

const deleted = user.delete("age");

console.log(deleted);
// true

console.log(user.has("age"));
// false
```

### 🖆 Using `clear()`

The `clear()` method removes all entries from a Map.

```javascript
const user = new Map([
    ["name", "Alice"],
    ["age", 25]
]);

user.clear();

console.log(user);
// Map(0) {}
```

# 🗁 Getting the Size of a Map

The `size` property returns the number of key-value pairs.

```javascript
const user = new Map([
    ["name", "Alice"],
    ["age", 25],
    ["role", "admin"]
]);

console.log(user.size);
// 3
```

Unlike arrays, Maps use `size` instead of `length`.

# 🗁 Using Different Data Types as Keys

One of the most important advantages of Map is that keys can have any data type.

### 🖆 String Keys

```javascript
const data = new Map();

data.set("name", "Alice");

console.log(data.get("name"));
// Alice
```

### 🖆 Number Keys

```javascript
const data = new Map();

data.set(101, "Alice");
data.set(102, "Bob");

console.log(data.get(101));
// Alice
```

### 🖆 Boolean Keys

```javascript
const data = new Map();

data.set(true, "Enabled");
data.set(false, "Disabled");

console.log(data.get(true));
// Enabled
```

### 🖆 Object Keys

Objects can be used directly as Map keys.

```javascript
const user = {
    id: 101,
    name: "Alice"
};

const userMetadata = new Map();

userMetadata.set(user, {
    lastLogin: "2026-11-14",
    role: "admin"
});

console.log(userMetadata.get(user));
```

The object itself is the key, not its string representation.

### 🖆 Array Keys

Arrays can also be used as keys.

```javascript
const coordinates = new Map();

const position = [10, 20];

coordinates.set(position, "Location A");

console.log(coordinates.get(position));
// Location A
```

### 🖆 Function Keys

Functions are objects and can be used as Map keys.

```javascript
function greet() {
    console.log("Hello");
}

const functionMetadata = new Map();

functionMetadata.set(greet, "Greeting function");

console.log(functionMetadata.get(greet));
// Greeting function
```

# 🗁 Understanding Map Key Equality

Maps determine key equality using the SameValueZero algorithm.

Important behaviors:

* Two equal primitive values are treated as the same key.
* `NaN` is considered equal to another `NaN`.
* Positive zero and negative zero are considered equal.
* Objects are compared by reference.

### 🖆 Primitive Key Equality

```javascript
const data = new Map();

data.set("name", "Alice");
data.set("name", "Bob");

console.log(data.size);
// 1

console.log(data.get("name"));
// Bob
```

The second `set()` updates the existing entry.

### 🖆 `NaN` as a Key

```javascript
const data = new Map();

data.set(NaN, "Invalid number");

console.log(data.get(NaN));
// Invalid number
```

### 🖆 Object Reference Equality

```javascript
const map = new Map();

const firstUser = { id: 1 };
const secondUser = { id: 1 };

map.set(firstUser, "Alice");
map.set(secondUser, "Bob");

console.log(map.size);
// 2
```

Although both objects contain identical properties, they are different references.

### 🖆 Reusing the Same Object Key

```javascript
const user = { id: 1 };

const map = new Map();

map.set(user, "Alice");
map.set(user, "Updated Alice");

console.log(map.size);
// 1

console.log(map.get(user));
// Updated Alice
```

Both operations use the same object reference.

# 🗁 Iterating Over a Map

Maps preserve insertion order. Iteration follows the order in which entries were first inserted.

### 🖆 Using `for...of`

```javascript
const user = new Map([
    ["name", "Alice"],
    ["age", 25],
    ["role", "admin"]
]);

for (const [key, value] of user) {
    console.log(key, value);
}
```

Output:

```text
name Alice
age 25
role admin
```

The default Map iterator returns key-value pairs.

### 🖆 Using `forEach()`

```javascript
const user = new Map([
    ["name", "Alice"],
    ["age", 25]
]);

user.forEach((value, key, map) => {
    console.log(key, value);
});
```

Unlike `Set.forEach()`, Map's callback receives the value first and the key second.

### 🖆 Using `keys()`

The `keys()` method returns an iterator containing all keys.

```javascript
const user = new Map([
    ["name", "Alice"],
    ["age", 25],
    ["role", "admin"]
]);

console.log([...user.keys()]);
// ["name", "age", "role"]
```

### 🖆 Using `values()`

The `values()` method returns an iterator containing all values.

```javascript
console.log([...user.values()]);
// ["Alice", 25, "admin"]
```

### 🖆 Using `entries()`

The `entries()` method returns an iterator containing key-value pairs.

```javascript
console.log([...user.entries()]);
// [["name", "Alice"], ["age", 25], ["role", "admin"]]
```

### 🖆 Using the Spread Operator

```javascript
const entries = [...user];

console.log(entries);
// [["name", "Alice"], ["age", 25], ["role", "admin"]]
```

The default iterator of a Map is equivalent to its `entries()` iterator.

# 🗁 Converting Between Map and Other Collections

### 🖆 Converting an Object into a Map

Use `Object.entries()` to convert an object's enumerable own string-keyed properties into an array of key-value pairs.

```javascript
const user = {
    name: "Alice",
    age: 25,
    role: "admin"
};

const userMap = new Map(Object.entries(user));

console.log(userMap.get("name"));
// Alice
```

### 🖆 Converting a Map into an Object

Use `Object.fromEntries()` to create an object from a Map's entries.

```javascript
const userMap = new Map([
    ["name", "Alice"],
    ["age", 25],
    ["role", "admin"]
]);

const user = Object.fromEntries(userMap);

console.log(user);
// { name: "Alice", age: 25, role: "admin" }
```

This conversion is suitable when Map keys are property keys that can be represented as object keys. Non-string primitive keys are converted to property keys, and object keys are converted to strings or symbols according to property-key conversion rules. Different Map keys can therefore become the same object property.

### 🖆 Converting an Array into a Map

```javascript
const users = [
    ["101", "Alice"],
    ["102", "Bob"],
    ["103", "Charlie"]
];

const userMap = new Map(users);

console.log(userMap.get("102"));
// Bob
```

### 🖆 Converting a Map into an Array

```javascript
const userMap = new Map([
    ["101", "Alice"],
    ["102", "Bob"]
]);

const users = [...userMap];

console.log(users);
// [["101", "Alice"], ["102", "Bob"]]
```

# 🗁 Map vs Object

Both Map and Object can store key-value data, but they have different characteristics.

| Feature | Map | Object |
| --- | --- | --- |
| Key types | Any value | Strings and symbols |
| Key order | Insertion order | Defined property enumeration rules |
| Number of entries | `size` | Manual calculation |
| Adding entries | `set()` | Assignment |
| Retrieving values | `get()` | Property access |
| Checking keys | `has()` | `Object.hasOwn()` |
| Deleting entries | `delete()` | `delete` operator |
| Iteration | Directly iterable | Not directly iterable |
| Built-in iteration methods | `keys()`, `values()`, `entries()` | `Object.keys()`, `Object.values()`, `Object.entries()` |
| Prototype inheritance | No ordinary object property namespace for entries | Inherits from a prototype by default |
| JSON serialization | Requires conversion | Supported for compatible values |

### 🖆 When to Use a Map

Use a Map when:

* Keys are not limited to strings and symbols.
* You frequently add and remove entries.
* You need direct iteration over entries.
* You need to preserve insertion order.
* You need to use objects as keys.
* You need a dedicated key-value collection.

### 🖆 When to Use an Object

Use an Object when:

* Representing a structured entity.
* Defining records with known property names.
* Working with JSON data.
* Using property access such as `user.name`.
* Building configuration objects.
* Defining methods and behavior on an entity.

# 🗁 Practical Map Examples

### 🖆 Counting Element Frequencies

A Map can count how many times each value occurs.

```javascript
const fruits = [
    "Apple",
    "Banana",
    "Apple",
    "Mango",
    "Banana",
    "Apple"
];

const frequency = new Map();

for (const fruit of fruits) {
    frequency.set(
        fruit,
        (frequency.get(fruit) ?? 0) + 1
    );
}

console.log(frequency);
```

Result:

```text
Map(3) {
    'Apple' => 3,
    'Banana' => 2,
    'Mango' => 1
}
```

### 🖆 Caching Computed Results

Maps can store previously calculated results to avoid repeating expensive operations.

```javascript
const cache = new Map();

function calculateSquare(number) {
    if (cache.has(number)) {
        return cache.get(number);
    }

    const result = number * number;

    cache.set(number, result);

    return result;
}

console.log(calculateSquare(5));
// 25

console.log(calculateSquare(5));
// 25
```

The second call retrieves the cached result.

### 🖆 Managing User Sessions

```javascript
const sessions = new Map();

function createSession(userId, sessionData) {
    sessions.set(userId, sessionData);
}

function getSession(userId) {
    return sessions.get(userId);
}

function removeSession(userId) {
    sessions.delete(userId);
}

createSession(101, {
    username: "Alice",
    authenticated: true
});

console.log(getSession(101));
```

### 🖆 Grouping Data

Maps can group records based on a selected property.

```javascript
const users = [
    { name: "Alice", department: "Engineering" },
    { name: "Bob", department: "Marketing" },
    { name: "Charlie", department: "Engineering" },
    { name: "David", department: "Marketing" }
];

const groupedUsers = new Map();

for (const user of users) {
    const department = user.department;

    if (!groupedUsers.has(department)) {
        groupedUsers.set(department, []);
    }

    groupedUsers.get(department).push(user);
}

console.log(groupedUsers);
```

Result:

```text
Map(2) {
    'Engineering' => [
        { name: 'Alice', department: 'Engineering' },
        { name: 'Charlie', department: 'Engineering' }
    ],
    'Marketing' => [
        { name: 'Bob', department: 'Marketing' },
        { name: 'David', department: 'Marketing' }
    ]
}
```

### 🖆 Building a Lookup Table

```javascript
const statusCodes = new Map([
    [200, "OK"],
    [201, "Created"],
    [400, "Bad Request"],
    [401, "Unauthorized"],
    [404, "Not Found"],
    [500, "Internal Server Error"]
]);

function getStatusMessage(code) {
    return statusCodes.get(code) ?? "Unknown Status";
}

console.log(getStatusMessage(404));
// Not Found

console.log(getStatusMessage(503));
// Unknown Status
```

# 🗁 Understanding Map Insertion Order

A Map maintains the order in which entries were first inserted.

Updating an existing key does not move that entry to the end.

```javascript
const map = new Map();

map.set("a", 1);
map.set("b", 2);
map.set("c", 3);

map.set("a", 100);

console.log([...map.keys()]);
// ["a", "b", "c"]
```

Deleting an entry and inserting it again places it at the end.

```javascript
map.delete("a");
map.set("a", 100);

console.log([...map.keys()]);
// ["b", "c", "a"]
```

# 🗁 Map Performance Considerations

* Map implementations must provide average access times that are sublinear in the number of entries.
* `get()`, `set()`, `has()`, and `delete()` are designed for efficient key-based operations.
* Maps are generally suitable for frequent insertion and deletion.
* Iteration takes time proportional to the number of entries.
* Map keys can be objects without converting them into strings.
* Avoid creating unnecessary Maps for simple fixed object structures.
* Use `WeakMap` when object-keyed data should not keep its keys alive solely because they are stored in the collection.

# 🗁 Common Mistakes

### 🖆 Using Property Access Instead of `get()`

Incorrect:

```javascript
const user = new Map();

user.set("name", "Alice");

console.log(user.name);
// undefined
```

Correct:

```javascript
console.log(user.get("name"));
// Alice
```

Map entries are not ordinary object properties.

### 🖆 Using `length` Instead of `size`

Incorrect:

```javascript
const map = new Map([
    ["name", "Alice"]
]);

console.log(map.length);
// undefined
```

Correct:

```javascript
console.log(map.size);
// 1
```

### 🖆 Expecting Objects with Identical Contents to Be Equal Keys

```javascript
const map = new Map();

map.set({ id: 1 }, "Alice");

console.log(map.get({ id: 1 }));
// undefined
```

The second object has a different reference.

Store the object reference if you need to retrieve its associated value.

```javascript
const user = { id: 1 };

map.set(user, "Alice");

console.log(map.get(user));
// Alice
```

### 🖆 Confusing `has()` with Value Checking

```javascript
const map = new Map([
    ["active", false]
]);

console.log(map.has("active"));
// true

console.log(map.get("active"));
// false
```

`has()` checks the key, not the truthiness of the value.

### 🖆 Expecting JSON Serialization to Work Directly

```javascript
const map = new Map([
    ["name", "Alice"],
    ["age", 25]
]);

console.log(JSON.stringify(map));
// "{}"
```

Convert the Map into an array or object before serializing it.

```javascript
console.log(JSON.stringify([...map]));
// [["name","Alice"],["age",25]]
```

Or:

```javascript
console.log(JSON.stringify(Object.fromEntries(map)));
// {"name":"Alice","age":25}
```

Choose the representation based on the key types and data structure you need to preserve.

# 🗁 Best Practices

* Use Map for dedicated key-value collections.
* Use `set()` to add or update entries.
* Use `get()` to retrieve values.
* Use `has()` to check key existence.
* Use `delete()` to remove individual entries.
* Use `size` to count entries.
* Use `for...of` when iterating over key-value pairs.
* Use object keys when reference-based association is useful.
* Prefer Objects for structured records and JSON-compatible data.
* Convert Maps explicitly when serializing to JSON.
* Consider `WeakMap` when storing metadata associated with object lifetimes.

# 🗁 Summary

A JavaScript `Map` is a built-in collection that stores unique keys and their associated values.

Key points:

* Map stores data as key-value pairs.
* Keys can be values of any type.
* Duplicate keys update existing entries.
* Map preserves insertion order.
* `set()` adds or updates entries.
* `get()` retrieves values.
* `has()` checks whether a key exists.
* `delete()` removes an entry.
* `clear()` removes all entries.
* `size` returns the number of entries.
* `keys()`, `values()`, and `entries()` provide iterators.
* Maps can use objects and arrays as keys.
* Maps require explicit conversion for ordinary JSON serialization.

**Remember:** Use an Object for structured records and a Map for dynamic key-value collections that require flexible key types and direct iteration.
