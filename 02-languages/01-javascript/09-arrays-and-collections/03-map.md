# JavaScript Array `map()`

The `map()` method is a built-in JavaScript array method that transforms every element of an array by executing a callback function.

It returns a new array containing the results of the callback function.

The original array remains unchanged.

`map()` is commonly used for:

* Transforming array elements.
* Extracting object properties.
* Converting data between different formats.
* Preparing data for rendering in user interfaces.
* Performing calculations on collections.
* Creating derived datasets.

# 🗁 Syntax

### 🖆 Basic Syntax

```javascript
const newArray = array.map(callback);
```

### 🖆 Complete Syntax

```javascript
const newArray = array.map(
    (element, index, array) => {
        // Return the transformed element.
    }
);
```

The callback receives three arguments:

| Parameter | Description |
| --- | --- |
| `element` | Current element being processed |
| `index` | Index of the current element |
| `array` | Original array on which `map()` was called |

Only the `element` parameter is required in most use cases.

# 🗁 Basic Usage

### 🖆 Transforming Numbers

Multiply every number by two.

```javascript
const numbers = [1, 2, 3, 4, 5];

const doubled = numbers.map(number => number * 2);

console.log(doubled);
// [2, 4, 6, 8, 10]

console.log(numbers);
// [1, 2, 3, 4, 5]
```

The callback executes once for each element and returns a transformed value.

### 🖆 Converting Values

Convert an array of strings into numbers.

```javascript
const values = ["10", "20", "30", "40"];

const numbers = values.map(value => Number(value));

console.log(numbers);
// [10, 20, 30, 40]
```

### 🖆 Converting Numbers into Strings

```javascript
const numbers = [10, 20, 30];

const strings = numbers.map(number => String(number));

console.log(strings);
// ["10", "20", "30"]
```

### 🖆 Performing Calculations

Calculate the square of each number.

```javascript
const numbers = [2, 3, 4, 5];

const squares = numbers.map(number => number ** 2);

console.log(squares);
// [4, 9, 16, 25]
```

# 🗁 Using Callback Parameters

### 🖆 Accessing the Current Index

The second callback parameter provides the current element's index.

```javascript
const fruits = ["Apple", "Banana", "Orange"];

const result = fruits.map((fruit, index) => {
    return `${index}: ${fruit}`;
});

console.log(result);

// [
//   "0: Apple",
//   "1: Banana",
//   "2: Orange"
// ]
```

### 🖆 Using the Original Array

The third callback parameter provides access to the original array.

```javascript
const numbers = [10, 20, 30];

const result = numbers.map((number, index, array) => {
    return {
        value: number,
        index,
        totalElements: array.length
    };
});

console.log(result);
```

Output:

```javascript
[
    { value: 10, index: 0, totalElements: 3 },
    { value: 20, index: 1, totalElements: 3 },
    { value: 30, index: 2, totalElements: 3 }
]
```

# 🗁 Working with Objects

### 🖆 Extracting Object Properties

Suppose an array contains user objects, and you only need their names.

```javascript
const users = [
    { id: 1, name: "Alice", age: 25 },
    { id: 2, name: "Bob", age: 30 },
    { id: 3, name: "Charlie", age: 28 }
];

const names = users.map(user => user.name);

console.log(names);
// ["Alice", "Bob", "Charlie"]
```

### 🖆 Transforming Object Properties

Create a new array containing updated user objects.

```javascript
const users = [
    { id: 1, name: "Alice", age: 25 },
    { id: 2, name: "Bob", age: 30 }
];

const updatedUsers = users.map(user => ({
    ...user,
    active: true
}));

console.log(updatedUsers);

// [
//   { id: 1, name: "Alice", age: 25, active: true },
//   { id: 2, name: "Bob", age: 30, active: true }
// ]
```

The spread operator preserves the existing properties while adding a new property.

### 🖆 Renaming Object Properties

Transform objects into a different data structure.

```javascript
const users = [
    { id: 1, firstName: "Alice", lastName: "Smith" },
    { id: 2, firstName: "Bob", lastName: "Johnson" }
];

const result = users.map(user => ({
    id: user.id,
    fullName: `${user.firstName} ${user.lastName}`
}));

console.log(result);

// [
//   { id: 1, fullName: "Alice Smith" },
//   { id: 2, fullName: "Bob Johnson" }
// ]
```

### 🖆 Extracting Nested Properties

```javascript
const users = [
    {
        id: 1,
        name: "Alice",
        address: {
            city: "Dhaka",
            country: "Bangladesh"
        }
    },
    {
        id: 2,
        name: "Bob",
        address: {
            city: "London",
            country: "UK"
        }
    }
];

const cities = users.map(user => user.address.city);

console.log(cities);
// ["Dhaka", "London"]
```

# 🗁 Using `map()` with Other Array Methods

Array methods can be combined to build readable data-processing pipelines.

### 🖆 `map()` with `filter()`

Filter elements first, then transform the remaining elements.

```javascript
const numbers = [1, 2, 3, 4, 5, 6];

const result = numbers
    .filter(number => number % 2 === 0)
    .map(number => number * 10);

console.log(result);
// [20, 40, 60]
```

### 🖆 `map()` with `sort()`

Sort a copy of the array and then transform its elements.

```javascript
const numbers = [5, 2, 8, 1, 3];

const result = [...numbers]
    .sort((a, b) => a - b)
    .map(number => number * 2);

console.log(result);
// [2, 4, 6, 10, 16]
```

### 🖆 `map()` with `flatMap()`

When each element needs to produce multiple output elements, `flatMap()` may be more appropriate.

```javascript
const numbers = [1, 2, 3];

const result = numbers.flatMap(number => [
    number,
    number * 10
]);

console.log(result);
// [1, 10, 2, 20, 3, 30]
```

# 🗁 Practical Use Cases

### 🖆 Preparing Data for UI Rendering

A frontend application often receives an array of objects from an API.

You can transform the data into the structure required by your components.

```javascript
const products = [
    { id: 1, name: "Keyboard", price: 50 },
    { id: 2, name: "Mouse", price: 25 },
    { id: 3, name: "Monitor", price: 200 }
];

const productCards = products.map(product => ({
    id: product.id,
    title: product.name,
    formattedPrice: `$${product.price.toFixed(2)}`
}));

console.log(productCards);
```

Output:

```javascript
[
    { id: 1, title: "Keyboard", formattedPrice: "$50.00" },
    { id: 2, title: "Mouse", formattedPrice: "$25.00" },
    { id: 3, title: "Monitor", formattedPrice: "$200.00" }
]
```

### 🖆 Calculating Prices with Tax

```javascript
const prices = [100, 200, 300];

const pricesWithTax = prices.map(price => {
    const tax = price * 0.15;

    return price + tax;
});

console.log(pricesWithTax);
// [115, 230, 345]
```

### 🖆 Formatting Dates

```javascript
const dates = [
    new Date("2026-01-15"),
    new Date("2026-02-20"),
    new Date("2026-03-25")
];

const formattedDates = dates.map(date =>
    date.toLocaleDateString("en-US")
);

console.log(formattedDates);
// ["1/15/2026", "2/20/2026", "3/25/2026"]
```

### 🖆 Creating a Lookup List

```javascript
const users = [
    { id: 101, name: "Alice" },
    { id: 102, name: "Bob" },
    { id: 103, name: "Charlie" }
];

const userOptions = users.map(user => ({
    value: user.id,
    label: user.name
}));

console.log(userOptions);

// [
//   { value: 101, label: "Alice" },
//   { value: 102, label: "Bob" },
//   { value: 103, label: "Charlie" }
// ]
```

# 🗁 Important Characteristics

### 🖆 Returns a New Array

`map()` never directly modifies the original array.

```javascript
const numbers = [1, 2, 3];

const result = numbers.map(number => number * 2);

console.log(numbers);
// [1, 2, 3]

console.log(result);
// [2, 4, 6]
```

### 🖆 Preserves Array Length

For ordinary arrays, `map()` produces an array with the same length as the original array.

```javascript
const numbers = [1, 2, 3, 4];

const result = numbers.map(number => number * 2);

console.log(result.length);
// 4
```

### 🖆 Does Not Automatically Flatten Results

If a callback returns an array, `map()` keeps that array as a nested element.

```javascript
const numbers = [1, 2, 3];

const result = numbers.map(number => [number, number * 2]);

console.log(result);

// [
//   [1, 2],
//   [2, 4],
//   [3, 6]
// ]
```

Use `flatMap()` when you want to map and flatten one level.

### 🖆 Skips Empty Slots

`map()` does not execute its callback for empty slots in sparse arrays.

```javascript
const numbers = [1, , 3];

const result = numbers.map(number => number * 2);

console.log(result);
// [2, empty, 6]

console.log(1 in result);
// false
```

An empty slot is different from an explicitly assigned `undefined` value.

```javascript
const numbers = [1, undefined, 3];

const result = numbers.map(number => number ?? 0);

console.log(result);
// [1, 0, 3]
```

### 🖆 Does Not Wait for Promises

When an asynchronous callback is used, `map()` immediately returns an array of promises.

```javascript
const numbers = [1, 2, 3];

const result = numbers.map(async number => number * 2);

console.log(result);
// [Promise, Promise, Promise]
```

Use `Promise.all()` to wait for all asynchronous operations.

```javascript
const numbers = [1, 2, 3];

const result = await Promise.all(
    numbers.map(async number => number * 2)
);

console.log(result);
// [2, 4, 6]
```

This example must run inside an environment that supports `await` in the current context, such as an ES module or an async function.

# 🗁 Common Mistakes

### 🖆 Forgetting to Return a Value

Incorrect:

```javascript
const numbers = [1, 2, 3];

const result = numbers.map(number => {
    number * 2;
});

console.log(result);
// [undefined, undefined, undefined]
```

Correct:

```javascript
const numbers = [1, 2, 3];

const result = numbers.map(number => {
    return number * 2;
});

console.log(result);
// [2, 4, 6]
```

With an arrow function using braces, an explicit `return` is required.

### 🖆 Using `map()` Only for Side Effects

Avoid:

```javascript
const numbers = [1, 2, 3];

numbers.map(number => {
    console.log(number);
});
```

Prefer:

```javascript
numbers.forEach(number => {
    console.log(number);
});
```

Use `map()` when you need a new array of transformed values.

### 🖆 Mutating Objects Inside the Callback

Avoid modifying existing objects when you intend to produce independent updated objects.

```javascript
const users = [
    { id: 1, name: "Alice" },
    { id: 2, name: "Bob" }
];

const updatedUsers = users.map(user => {
    user.active = true;

    return user;
});
```

Although `map()` returns a new array, the original objects are modified.

Prefer:

```javascript
const updatedUsers = users.map(user => ({
    ...user,
    active: true
}));
```

This creates new outer objects while preserving the original objects.

### 🖆 Expecting `map()` to Remove Elements

`map()` preserves the number of positions in an ordinary array. Returning `null` or `undefined` does not remove an element.

```javascript
const numbers = [1, 2, 3, 4];

const result = numbers.map(number => {
    return number % 2 === 0 ? number : null;
});

console.log(result);
// [null, 2, null, 4]
```

Use `filter()` when you need to remove elements that do not satisfy a condition.

# 🗁 `map()` vs Other Array Methods

| Method | Purpose | Return Value |
| --- | --- | --- |
| `map()` | Transform every element | New array |
| `filter()` | Select matching elements | New array |
| `reduce()` | Accumulate values | Accumulated result |
| `forEach()` | Execute side effects | `undefined` |
| `find()` | Find the first matching element | Element or `undefined` |
| `flatMap()` | Transform and flatten one level | New array |

### 🖆 Choosing the Correct Method

Use `map()` when:

* Every element needs transformation.
* You need to preserve the collection's positions.
* You want to create a new array from existing data.

Use `filter()` when:

* You need only elements satisfying a condition.
* The resulting array may contain fewer elements.

Use `reduce()` when:

* You need a single accumulated result.
* You need to group, count, or aggregate data.

Use `forEach()` when:

* You need to perform an operation for every element.
* You do not need a returned array.

# 🗁 Best Practices

* Prefer concise arrow functions for simple transformations.
* Use descriptive callback parameter names.
* Return a value from every callback execution.
* Avoid using `map()` exclusively for side effects.
* Avoid mutating original objects inside the callback.
* Use object spread when creating updated object records.
* Use `flatMap()` when each element produces multiple output values.
* Use `Promise.all()` when waiting for asynchronous operations created through `map()`.
* Use `filter()` instead of returning `null` or `undefined` to remove elements.
* Avoid unnecessary transformations that create intermediate arrays in performance-critical code.

# 🗁 Summary

The `map()` method is one of the most important tools for array transformation in JavaScript.

Key points:

* Executes a callback for each existing array element.
* Returns a new array containing callback results.
* Does not modify the original array by itself.
* Preserves the number of positions, including empty slots.
* Supports element, index, and original-array callback parameters.
* Does not automatically flatten nested arrays.
* Does not automatically await asynchronous callbacks.
* Is best suited for transforming data rather than performing side effects.

**Remember:** `map()` transforms every element and returns the results as a new array.
