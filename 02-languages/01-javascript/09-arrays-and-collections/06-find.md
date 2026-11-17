# JavaScript Array `find()`

The `find()` method is a built-in JavaScript array method that searches an array and returns the first element that satisfies a specified condition.

It executes a callback function for each existing array element until it finds an element for which the callback returns a truthy value.

Once a matching element is found, iteration stops.

If no matching element exists, `find()` returns `undefined`.

`find()` is commonly used for:

* Finding a user by ID.
* Searching for a product by name.
* Retrieving a specific record from API response data.
* Locating an object that satisfies multiple conditions.
* Searching collections using custom logic.

# 🗁 Syntax

### 🖆 Basic Syntax

```javascript
const result = array.find(callback);
```

### 🖆 Complete Syntax

```javascript
const result = array.find(
    (element, index, array) => {
        // Return true when the desired element is found.
    }
);
```

The callback receives three arguments:

| Parameter | Description |
| --- | --- |
| `element` | Current element being processed |
| `index` | Index of the current element |
| `array` | Original array on which `find()` was called |

Only the `element` parameter is required in most use cases.

# 🗁 Basic Usage

### 🖆 Finding a Number

Find the first number greater than 20.

```javascript
const numbers = [10, 15, 25, 30, 40];

const result = numbers.find(number => number > 20);

console.log(result);
// 25
```

The search stops when `25` is found. The remaining elements are not processed.

### 🖆 Finding an Exact Value

```javascript
const numbers = [10, 20, 30, 40];

const result = numbers.find(number => number === 30);

console.log(result);
// 30
```

### 🖆 Finding an Even Number

```javascript
const numbers = [1, 3, 5, 8, 10, 12];

const result = numbers.find(number => number % 2 === 0);

console.log(result);
// 8
```

Only the first matching element is returned.

### 🖆 Finding a String

```javascript
const fruits = ["Apple", "Banana", "Orange", "Mango"];

const result = fruits.find(fruit => fruit.startsWith("O"));

console.log(result);
// "Orange"
```

### 🖆 Case-Insensitive Search

```javascript
const languages = ["JavaScript", "TypeScript", "Python"];

const result = languages.find(language =>
    language.toLowerCase() === "typescript"
);

console.log(result);
// "TypeScript"
```

# 🗁 Working with Objects

### 🖆 Finding an Object by ID

This is one of the most common use cases.

```javascript
const users = [
    { id: 101, name: "Alice", age: 25 },
    { id: 102, name: "Bob", age: 30 },
    { id: 103, name: "Charlie", age: 28 }
];

const user = users.find(user => user.id === 102);

console.log(user);

// { id: 102, name: "Bob", age: 30 }
```

### 🖆 Finding an Object by Multiple Conditions

Find an active user older than 25.

```javascript
const users = [
    { id: 1, name: "Alice", age: 22, active: true },
    { id: 2, name: "Bob", age: 30, active: true },
    { id: 3, name: "Charlie", age: 28, active: false },
    { id: 4, name: "David", age: 35, active: true }
];

const user = users.find(user =>
    user.age > 25 && user.active
);

console.log(user);

// { id: 2, name: "Bob", age: 30, active: true }
```

### 🖆 Finding an Object by Status

```javascript
const orders = [
    { id: 101, status: "pending", total: 500 },
    { id: 102, status: "completed", total: 800 },
    { id: 103, status: "cancelled", total: 300 }
];

const order = orders.find(order => order.status === "completed");

console.log(order);

// { id: 102, status: "completed", total: 800 }
```

### 🖆 Finding an Object Using Nested Properties

```javascript
const users = [
    {
        id: 1,
        name: "Alice",
        address: { city: "Dhaka" }
    },
    {
        id: 2,
        name: "Bob",
        address: { city: "London" }
    },
    {
        id: 3,
        name: "Charlie",
        address: { city: "Dhaka" }
    }
];

const user = users.find(user =>
    user.address.city === "Dhaka"
);

console.log(user.name);
// "Alice"
```

When nested properties may be missing, use optional chaining.

```javascript
const user = users.find(user =>
    user.address?.city === "Dhaka"
);
```

# 🗁 Using Callback Parameters

### 🖆 Accessing the Current Index

The second callback parameter provides the index of the current element.

```javascript
const fruits = ["Apple", "Banana", "Orange", "Mango"];

const result = fruits.find((fruit, index) => {
    return index >= 2 && fruit.startsWith("O");
});

console.log(result);
// "Orange"
```

### 🖆 Accessing the Original Array

The third callback parameter provides access to the original array.

```javascript
const numbers = [10, 20, 30, 40];

const result = numbers.find((number, index, array) => {
    return number > array[index - 1];
});

console.log(result);
// 20
```

The first element does not match because its previous position resolves to `undefined`.

# 🗁 Related Search Methods

JavaScript provides several array methods for searching and checking elements.

### 🖆 `find()`

Returns the first matching element.

```javascript
const numbers = [10, 20, 30, 40];

console.log(numbers.find(number => number > 20));
// 30
```

### 🖆 `findIndex()`

Returns the index of the first matching element.

Returns `-1` if no element matches.

```javascript
const numbers = [10, 20, 30, 40];

console.log(numbers.findIndex(number => number > 20));
// 2

console.log(numbers.findIndex(number => number > 100));
// -1
```

### 🖆 `findLast()`

Returns the last element that satisfies a condition.

Searches from right to left.

```javascript
const numbers = [10, 20, 30, 40, 50];

const result = numbers.findLast(number => number > 20);

console.log(result);
// 50
```

### 🖆 `findLastIndex()`

Returns the index of the last element that satisfies a condition.

```javascript
const numbers = [10, 20, 30, 40, 50];

const result = numbers.findLastIndex(number => number > 20);

console.log(result);
// 4
```

### 🖆 `includes()`

Checks whether an array contains a specific value.

```javascript
const fruits = ["Apple", "Banana", "Orange"];

console.log(fruits.includes("Banana"));
// true
```

### 🖆 `indexOf()`

Returns the first index of a specified value.

```javascript
const fruits = ["Apple", "Banana", "Orange"];

console.log(fruits.indexOf("Banana"));
// 1
```

# 🗁 Understanding `find()` vs `filter()`

These methods are similar because both accept a callback condition, but their return values are different.

### 🖆 Using `find()`

Returns only the first matching element.

```javascript
const users = [
    { id: 1, name: "Alice", active: true },
    { id: 2, name: "Bob", active: false },
    { id: 3, name: "Charlie", active: true }
];

const result = users.find(user => user.active);

console.log(result);

// { id: 1, name: "Alice", active: true }
```

### 🖆 Using `filter()`

Returns all matching elements in a new array.

```javascript
const result = users.filter(user => user.active);

console.log(result);

// [
//   { id: 1, name: "Alice", active: true },
//   { id: 3, name: "Charlie", active: true }
// ]
```

### 🖆 Comparison

| Feature | `find()` | `filter()` |
| --- | --- | --- |
| Purpose | Find first matching element | Find all matching elements |
| Return type | Element or `undefined` | New array |
| Stops after first match | Yes | No |
| No match | `undefined` | `[]` |
| Original array modified | No | No |
| Typical use | Find one record | Select multiple records |

# 🗁 Practical Use Cases

### 🖆 Finding a Product

```javascript
const products = [
    { id: 1, name: "Keyboard", price: 50 },
    { id: 2, name: "Mouse", price: 25 },
    { id: 3, name: "Monitor", price: 200 }
];

function getProductById(id) {
    return products.find(product => product.id === id);
}

console.log(getProductById(2));

// { id: 2, name: "Mouse", price: 25 }
```

### 🖆 Finding a User from API Data

```javascript
const apiResponse = {
    users: [
        { id: 101, name: "Alice", role: "admin" },
        { id: 102, name: "Bob", role: "user" },
        { id: 103, name: "Charlie", role: "user" }
    ]
};

const admin = apiResponse.users.find(user =>
    user.role === "admin"
);

console.log(admin.name);
// "Alice"
```

### 🖆 Finding a Cart Item

```javascript
const cart = [
    { productId: 101, name: "Keyboard", quantity: 1 },
    { productId: 102, name: "Mouse", quantity: 2 },
    { productId: 103, name: "Monitor", quantity: 1 }
];

const item = cart.find(item => item.productId === 102);

if (item) {
    console.log(item.quantity);
    // 2
}
```

### 🖆 Finding the First Available Item

```javascript
const seats = [
    { number: 1, available: false },
    { number: 2, available: false },
    { number: 3, available: true },
    { number: 4, available: true }
];

const availableSeat = seats.find(seat => seat.available);

console.log(availableSeat);
// { number: 3, available: true }
```

### 🖆 Finding an Item with a Case-Insensitive Keyword

```javascript
const products = [
    { id: 1, name: "Mechanical Keyboard" },
    { id: 2, name: "Wireless Mouse" },
    { id: 3, name: "Gaming Monitor" }
];

function findProduct(keyword) {
    const query = keyword.toLowerCase();

    return products.find(product =>
        product.name.toLowerCase().includes(query)
    );
}

console.log(findProduct("mouse"));

// { id: 2, name: "Wireless Mouse" }
```

# 🗁 Important Characteristics

### 🖆 Returns the First Matching Element

When multiple elements satisfy the condition, `find()` returns only the first one.

```javascript
const numbers = [10, 20, 30, 40];

const result = numbers.find(number => number >= 20);

console.log(result);
// 20
```

### 🖆 Stops Iteration After a Match

The callback is not executed for subsequent elements after a match is found.

```javascript
const numbers = [10, 20, 30, 40];

const result = numbers.find(number => {
    console.log("Checking:", number);

    return number > 20;
});

// Checking: 10
// Checking: 20
// Checking: 30

console.log(result);
// 30
```

### 🖆 Returns `undefined` When No Match Exists

```javascript
const numbers = [10, 20, 30];

const result = numbers.find(number => number > 100);

console.log(result);
// undefined
```

Always account for the possibility that no matching element exists.

### 🖆 Works with Sparse Arrays

Unlike methods such as `map()`, `filter()`, and `reduce()`, `find()` invokes its callback for every index, including empty slots.

An empty slot is treated as `undefined` when passed to the callback.

```javascript
const numbers = [10, , 30];

const result = numbers.find(number => number === undefined);

console.log(result);
// undefined
```

The returned value is `undefined`, so it is indistinguishable from the result of an unsuccessful search by return value alone.

### 🖆 Does Not Modify the Original Array by Itself

```javascript
const numbers = [10, 20, 30];

const result = numbers.find(number => number > 15);

console.log(numbers);
// [10, 20, 30]

console.log(result);
// 20
```

However, a callback can still modify the array or objects referenced by its elements if explicitly written to do so.

### 🖆 Returns Object References

When searching an array of objects, `find()` returns a reference to the matching object rather than a copy.

```javascript
const users = [
    { id: 1, name: "Alice" },
    { id: 2, name: "Bob" }
];

const user = users.find(user => user.id === 1);

user.name = "Charlie";

console.log(users[0].name);
// "Charlie"
```

The original object is modified because both variables reference the same object.

# 🗁 Common Mistakes

### 🖆 Expecting an Array Result

Incorrect assumption:

```javascript
const numbers = [10, 20, 30, 40];

const result = numbers.find(number => number > 15);

console.log(result);
// 20
```

`find()` returns one element, not an array.

Use `filter()` when you need multiple matching elements.

### 🖆 Accessing Properties Without Checking the Result

Potentially problematic:

```javascript
const users = [
    { id: 1, name: "Alice" }
];

const user = users.find(user => user.id === 2);

console.log(user.name);
// TypeError
```

The result is `undefined`, so accessing `.name` throws an error.

A safer approach:

```javascript
const user = users.find(user => user.id === 2);

if (user) {
    console.log(user.name);
} else {
    console.log("User not found");
}
```

Optional chaining is another option:

```javascript
console.log(user?.name);
// undefined
```

### 🖆 Using Assignment Instead of Comparison

Incorrect:

```javascript
const users = [
    { id: 1, name: "Alice" },
    { id: 2, name: "Bob" }
];

const user = users.find(user => user.id = 2);
```

This assigns `2` to each examined object's `id` property rather than checking its value.

Correct:

```javascript
const user = users.find(user => user.id === 2);
```

### 🖆 Confusing `find()` with `findIndex()`

```javascript
const numbers = [10, 20, 30];

const value = numbers.find(number => number > 15);
const index = numbers.findIndex(number => number > 15);

console.log(value);
// 20

console.log(index);
// 1
```

Use `find()` for the element and `findIndex()` for its position.

# 🗁 Performance Considerations

* `find()` has a worst-case time complexity of \(O(n)\).
* It stops searching as soon as a matching element is found.
* Searching for an element near the beginning is generally faster than searching near the end.
* It does not create a new array.
* Use `find()` instead of `filter()` when only the first matching element is needed.
* For repeated lookups by a unique key, consider using a `Map` for faster average lookup performance.

For example, repeatedly searching a large user array:

```javascript
const user = users.find(user => user.id === 102);
```

If the application performs many lookups by ID, build a lookup map once:

```javascript
const usersById = new Map(
    users.map(user => [user.id, user])
);

const user = usersById.get(102);
```

This avoids repeatedly scanning the entire array.

# 🗁 Best Practices

* Use `find()` when you need the first matching element.
* Use `filter()` when you need every matching element.
* Use `findIndex()` when you need the matching position.
* Always handle the possibility of an `undefined` result.
* Use strict equality (`===`) for predictable comparisons.
* Use optional chaining when accessing potentially missing nested properties.
* Avoid modifying objects inside the callback unless intentional.
* Use `Map` for repeated lookups by unique keys in large collections.
* Keep search conditions readable and focused.

# 🗁 Summary

The `find()` method searches an array and returns the first element that satisfies a condition.

Key points:

* Executes a callback for each array index until a match is found.
* Returns the first matching element.
* Stops iteration immediately after finding a match.
* Returns `undefined` when no match exists.
* Does not create a new array.
* Does not directly modify the original array.
* Works with primitive values and objects.
* `findIndex()` returns the matching index instead of the element.
* `findLast()` searches from right to left.

**Remember:** `find()` returns the first matching element, while `filter()` returns all matching elements.
