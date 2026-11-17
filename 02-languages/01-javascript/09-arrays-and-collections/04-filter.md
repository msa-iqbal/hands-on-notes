# JavaScript Array `filter()`

The `filter()` method is a built-in JavaScript array method that creates a new array containing only the elements that satisfy a specified condition.

It executes a callback function for each existing array element and includes an element in the resulting array when the callback returns a truthy value.

The original array remains unchanged.

`filter()` is commonly used for:

* Selecting records based on conditions.
* Removing unwanted elements.
* Searching collections using custom criteria.
* Filtering API responses.
* Implementing search and category filters in frontend applications.
* Building data-processing pipelines.

# 🗁 Syntax

### 🖆 Basic Syntax

```javascript
const filteredArray = array.filter(callback);
```

### 🖆 Complete Syntax

```javascript
const filteredArray = array.filter(
    (element, index, array) => {
        // Return true to keep the element.
        // Return false to exclude the element.
    }
);
```

The callback receives three arguments:

| Parameter | Description |
| --- | --- |
| `element` | Current element being processed |
| `index` | Index of the current element |
| `array` | Original array on which `filter()` was called |

Only the `element` parameter is required in most use cases.

# 🗁 Basic Usage

### 🖆 Filtering Even Numbers

Select only even numbers from an array.

```javascript
const numbers = [1, 2, 3, 4, 5, 6];

const evenNumbers = numbers.filter(number => {
    return number % 2 === 0;
});

console.log(evenNumbers);
// [2, 4, 6]
```

The callback returns:

* `true` for even numbers.
* `false` for odd numbers.

Only elements that produce a truthy result are included.

### 🖆 Filtering Odd Numbers

```javascript
const numbers = [1, 2, 3, 4, 5, 6];

const oddNumbers = numbers.filter(number => number % 2 !== 0);

console.log(oddNumbers);
// [1, 3, 5]
```

### 🖆 Filtering Numbers Greater Than a Value

```javascript
const numbers = [10, 25, 40, 15, 60, 5];

const result = numbers.filter(number => number > 20);

console.log(result);
// [25, 40, 60]
```

### 🖆 Filtering Strings by Length

```javascript
const names = ["Ali", "Alexander", "Bob", "Christopher"];

const result = names.filter(name => name.length > 3);

console.log(result);
// ["Alexander", "Christopher"]
```

### 🖆 Filtering Empty Values

JavaScript treats values such as `false`, `0`, `""`, `null`, `undefined`, and `NaN` as falsy.

You can use `Boolean` as a callback to remove all falsy values.

```javascript
const values = [
    "JavaScript",
    "",
    null,
    "TypeScript",
    undefined,
    false,
    0,
    100
];

const result = values.filter(Boolean);

console.log(result);
// ["JavaScript", "TypeScript", 100]
```

Be careful: this also removes valid values such as `0` and `false`.

If those values should be preserved, define an explicit condition.

```javascript
const values = [0, 1, null, 2, undefined, 3];

const result = values.filter(value => value != null);

console.log(result);
// [0, 1, 2, 3]
```

# 🗁 Using Callback Parameters

### 🖆 Accessing the Current Index

The second callback parameter provides the current element's index.

```javascript
const fruits = ["Apple", "Banana", "Orange", "Mango"];

const result = fruits.filter((fruit, index) => {
    return index >= 2;
});

console.log(result);
// ["Orange", "Mango"]
```

### 🖆 Using the Original Array

The third callback parameter provides access to the original array.

```javascript
const numbers = [10, 20, 30, 40];

const result = numbers.filter((number, index, array) => {
    return number > array[index - 1];
});

console.log(result);
// [20, 30, 40]
```

The first element is excluded because `array[-1]` is `undefined`, making the comparison false.

# 🗁 Working with Objects

### 🖆 Filtering Objects by Property

Suppose you have an array of users and want to select users older than 25.

```javascript
const users = [
    { id: 1, name: "Alice", age: 22 },
    { id: 2, name: "Bob", age: 30 },
    { id: 3, name: "Charlie", age: 28 },
    { id: 4, name: "David", age: 20 }
];

const result = users.filter(user => user.age > 25);

console.log(result);

// [
//   { id: 2, name: "Bob", age: 30 },
//   { id: 3, name: "Charlie", age: 28 }
// ]
```

### 🖆 Filtering by Multiple Conditions

Select users who are older than 25 and active.

```javascript
const users = [
    { id: 1, name: "Alice", age: 22, active: true },
    { id: 2, name: "Bob", age: 30, active: true },
    { id: 3, name: "Charlie", age: 28, active: false },
    { id: 4, name: "David", age: 35, active: true }
];

const result = users.filter(user => {
    return user.age > 25 && user.active;
});

console.log(result);

// [
//   { id: 2, name: "Bob", age: 30, active: true },
//   { id: 4, name: "David", age: 35, active: true }
// ]
```

### 🖆 Filtering by Status

```javascript
const orders = [
    { id: 101, status: "pending", total: 500 },
    { id: 102, status: "completed", total: 800 },
    { id: 103, status: "cancelled", total: 300 },
    { id: 104, status: "completed", total: 1200 }
];

const completedOrders = orders.filter(
    order => order.status === "completed"
);

console.log(completedOrders);

// [
//   { id: 102, status: "completed", total: 800 },
//   { id: 104, status: "completed", total: 1200 }
// ]
```

### 🖆 Filtering Nested Object Properties

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

const result = users.filter(
    user => user.address.city === "Dhaka"
);

console.log(result);

// Alice and Charlie
```

When nested properties may be missing, optional chaining can prevent errors.

```javascript
const result = users.filter(
    user => user.address?.city === "Dhaka"
);
```

# 🗁 Using `filter()` with Other Array Methods

Array methods can be chained to create readable data-processing pipelines.

### 🖆 `filter()` with `map()`

Filter elements first, then transform the remaining elements.

```javascript
const numbers = [1, 2, 3, 4, 5, 6];

const result = numbers
    .filter(number => number % 2 === 0)
    .map(number => number * 10);

console.log(result);
// [20, 40, 60]
```

### 🖆 `map()` with `filter()`

Transform elements first, then select matching results.

```javascript
const numbers = [1, 2, 3, 4, 5];

const result = numbers
    .map(number => number * 2)
    .filter(number => number > 5);

console.log(result);
// [6, 8, 10]
```

The order matters because transforming values before filtering can change which elements satisfy the condition.

### 🖆 `filter()` with `reduce()`

Filter records and then calculate an accumulated value.

```javascript
const products = [
    { name: "Keyboard", price: 50 },
    { name: "Mouse", price: 25 },
    { name: "Monitor", price: 200 },
    { name: "USB Cable", price: 10 }
];

const total = products
    .filter(product => product.price >= 25)
    .reduce((sum, product) => sum + product.price, 0);

console.log(total);
// 275
```

### 🖆 Multiple Filters

You can chain multiple `filter()` calls when separate conditions improve readability.

```javascript
const products = [
    { name: "Keyboard", category: "electronics", price: 50 },
    { name: "Chair", category: "furniture", price: 150 },
    { name: "Mouse", category: "electronics", price: 25 },
    { name: "Desk", category: "furniture", price: 300 }
];

const result = products
    .filter(product => product.category === "electronics")
    .filter(product => product.price >= 30);

console.log(result);

// [
//   { name: "Keyboard", category: "electronics", price: 50 }
// ]
```

# 🗁 Practical Use Cases

### 🖆 Implementing Search Functionality

Filter users by a search keyword.

```javascript
const users = [
    { id: 1, name: "Alice Johnson" },
    { id: 2, name: "Bob Smith" },
    { id: 3, name: "Charlie Brown" },
    { id: 4, name: "David Johnson" }
];

function searchUsers(keyword) {
    const query = keyword.toLowerCase();

    return users.filter(user =>
        user.name.toLowerCase().includes(query)
    );
}

console.log(searchUsers("john"));

// [
//   { id: 1, name: "Alice Johnson" },
//   { id: 4, name: "David Johnson" }
// ]
```

### 🖆 Product Category Filtering

```javascript
const products = [
    { id: 1, name: "Laptop", category: "electronics" },
    { id: 2, name: "Chair", category: "furniture" },
    { id: 3, name: "Phone", category: "electronics" },
    { id: 4, name: "Desk", category: "furniture" }
];

function getProductsByCategory(category) {
    return products.filter(product =>
        product.category === category
    );
}

console.log(getProductsByCategory("electronics"));

// Laptop and Phone
```

### 🖆 Filtering API Response Data

Imagine an API returns user records, and your application needs only verified users.

```javascript
const apiResponse = [
    { id: 1, name: "Alice", verified: true },
    { id: 2, name: "Bob", verified: false },
    { id: 3, name: "Charlie", verified: true }
];

const verifiedUsers = apiResponse.filter(user => user.verified);

console.log(verifiedUsers);

// Alice and Charlie
```

### 🖆 Filtering by Date

Select upcoming events using a date comparison.

```javascript
const events = [
    { name: "Conference", date: "2026-12-10" },
    { name: "Workshop", date: "2026-11-20" },
    { name: "Meetup", date: "2026-10-15" }
];

const today = new Date("2026-11-14");

const upcomingEvents = events.filter(event => {
    return new Date(event.date) >= today;
});

console.log(upcomingEvents);

// Conference and Workshop
```

For production applications, consider timezone handling when comparing date and time values.

# 🗁 Important Characteristics

### 🖆 Returns a New Array

`filter()` creates a new array and does not directly modify the original array.

```javascript
const numbers = [1, 2, 3, 4];

const result = numbers.filter(number => number > 2);

console.log(numbers);
// [1, 2, 3, 4]

console.log(result);
// [3, 4]
```

### 🖆 Can Return an Empty Array

If no elements satisfy the condition, `filter()` returns an empty array.

```javascript
const numbers = [1, 2, 3];

const result = numbers.filter(number => number > 10);

console.log(result);
// []
```

### 🖆 Preserves Element Order

Matching elements appear in the same relative order as they appeared in the original array.

```javascript
const numbers = [5, 2, 8, 1, 6];

const result = numbers.filter(number => number > 4);

console.log(result);
// [5, 8, 6]
```

### 🖆 Uses Truthy and Falsy Results

The callback does not have to return an explicit boolean.

Any truthy result keeps the element; any falsy result excludes it.

```javascript
const numbers = [0, 1, 2, 3, 4];

const result = numbers.filter(number => number);

console.log(result);
// [1, 2, 3, 4]
```

Explicit boolean conditions are generally easier to understand.

### 🖆 Skips Empty Slots

`filter()` does not execute its callback for empty slots in sparse arrays.

```javascript
const numbers = [1, , 3, 4];

const result = numbers.filter(number => number > 1);

console.log(result);
// [3, 4]
```

The resulting array contains only the elements that passed the condition.

### 🖆 Shallow Copy Behavior

`filter()` creates a new array, but it does not clone the objects inside it.

```javascript
const users = [
    { id: 1, name: "Alice", active: true },
    { id: 2, name: "Bob", active: false }
];

const activeUsers = users.filter(user => user.active);

activeUsers[0].name = "Charlie";

console.log(users[0].name);
// "Charlie"
```

Both arrays reference the same user object.

If independent objects are required, transform the filtered results as well.

```javascript
const activeUsers = users
    .filter(user => user.active)
    .map(user => ({ ...user }));
```

# 🗁 Common Mistakes

### 🖆 Forgetting the Return Statement

Incorrect:

```javascript
const numbers = [1, 2, 3, 4];

const result = numbers.filter(number => {
    number > 2;
});

console.log(result);
// []
```

The callback returns `undefined` because there is no explicit return statement.

Correct:

```javascript
const result = numbers.filter(number => {
    return number > 2;
});

console.log(result);
// [3, 4]
```

### 🖆 Using Assignment Instead of Comparison

Incorrect:

```javascript
const users = [
    { name: "Alice", active: true },
    { name: "Bob", active: false }
];

const result = users.filter(user => user.active = true);
```

This assigns `true` instead of checking the existing value, modifying the objects.

Correct:

```javascript
const result = users.filter(user => user.active === true);
```

### 🖆 Expecting `filter()` to Transform Elements

`filter()` selects elements; it does not transform them.

```javascript
const numbers = [1, 2, 3];

const result = numbers.filter(number => number * 2);

console.log(result);
// [1, 2, 3]
```

Every multiplication result is truthy, so every element is retained.

Use `map()` for transformation.

```javascript
const result = numbers.map(number => number * 2);

console.log(result);
// [2, 4, 6]
```

### 🖆 Confusing `filter()` with `find()`

`filter()` returns an array containing every matching element.

`find()` returns only the first matching element.

```javascript
const numbers = [10, 20, 30, 40];

const filtered = numbers.filter(number => number > 20);
const found = numbers.find(number => number > 20);

console.log(filtered);
// [30, 40]

console.log(found);
// 30
```

# 🗁 Performance Considerations

* `filter()` generally performs a linear scan of the array.
* Its time complexity is typically \(O(n)\), where `n` is the number of elements.
* It creates a new array containing references to matching elements.
* Chaining multiple filters creates intermediate arrays.
* For small and medium collections, readable method chains are usually preferable.
* For performance-critical processing of large arrays, a single loop may reduce intermediate allocations.

Example with chained filters:

```javascript
const result = users
    .filter(user => user.active)
    .filter(user => user.age >= 18);
```

Equivalent single-filter approach:

```javascript
const result = users.filter(user =>
    user.active && user.age >= 18
);
```

Both approaches are valid. Choose based on readability and performance requirements.

# 🗁 `filter()` vs Other Array Methods

| Method | Purpose | Return Value |
| --- | --- | --- |
| `filter()` | Select matching elements | New array |
| `map()` | Transform every element | New array |
| `find()` | Find first matching element | Element or `undefined` |
| `findIndex()` | Find first matching index | Index or `-1` |
| `some()` | Check whether at least one element matches | Boolean |
| `every()` | Check whether all elements match | Boolean |
| `reduce()` | Accumulate values | Accumulated result |
| `forEach()` | Execute a callback for each element | `undefined` |

# 🗁 Best Practices

* Use `filter()` when selecting multiple elements based on a condition.
* Return explicit boolean expressions from callbacks.
* Use descriptive callback parameter names.
* Combine related conditions when doing so improves readability.
* Use optional chaining when accessing potentially missing nested properties.
* Avoid mutating objects inside the callback.
* Use `map()` when transforming elements.
* Use `find()` when only the first matching element is needed.
* Avoid unnecessary intermediate arrays in performance-critical code.
* Remember that the resulting array is a shallow copy.

# 🗁 Summary

The `filter()` method is used to create a new array containing elements that satisfy a condition.

Key points:

* Executes a callback for each existing array element.
* Includes elements when the callback returns a truthy value.
* Excludes elements when the callback returns a falsy value.
* Returns a new array.
* Does not directly modify the original array.
* Preserves the relative order of matching elements.
* Can return an empty array.
* Creates a shallow copy rather than cloning nested objects.

**Remember:** `filter()` selects elements; `map()` transforms elements.
