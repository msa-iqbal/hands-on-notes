# Arrays

An array is a special JavaScript object used to store multiple values in a single variable.

Arrays allow developers to organize, access, modify, and process collections of data.

An array can contain:

* Strings
* Numbers
* Booleans
* Objects
* Other arrays
* Functions
* Different data types in the same collection

Example:

```javascript
const fruits = ["Apple", "Banana", "Orange"];

console.log(fruits);
```

Output:

```text
["Apple", "Banana", "Orange"]
```

Instead of creating separate variables for every fruit, an array stores them together.

# 🗁 Creating Arrays

JavaScript provides several ways to create arrays.

**Array Literal**

The array literal is the most common and recommended way to create an array.

```javascript
const numbers = [10, 20, 30, 40];

console.log(numbers);
```

Output:

```text
[10, 20, 30, 40]
```

**Array Constructor**

You can also create an array using the `Array` constructor.

```javascript
const fruits = new Array("Apple", "Banana", "Orange");

console.log(fruits);
```

Output:

```text
["Apple", "Banana", "Orange"]
```

Be careful when passing a single numeric argument to `Array()`.

```javascript
const numbers = new Array(5);

console.log(numbers);
console.log(numbers.length);
```

Output:

```text
[empty × 5]
5
```

This creates an array with five empty slots, not an array containing the number `5`.

To create an array containing one number, use:

```javascript
const numbers = [5];

console.log(numbers);
```

Output:

```text
[5]
```

**Array.of()**

`Array.of()` creates an array from the supplied arguments.

```javascript
const numbers = Array.of(5);

console.log(numbers);
```

Output:

```text
[5]
```

Unlike `new Array(5)`, it creates an array containing the number `5`.

**Array.from()**

`Array.from()` creates a new array from an iterable or array-like object.

```javascript
const text = "Hello";

const characters = Array.from(text);

console.log(characters);
```

Output:

```text
["H", "e", "l", "l", "o"]
```

It can also generate values using a mapping function.

```javascript
const numbers = Array.from(
  { length: 5 },
  (_, index) => index + 1
);

console.log(numbers);
```

Output:

```text
[1, 2, 3, 4, 5]
```

# 🗁 Array Indexing

JavaScript arrays use zero-based indexing.

This means the first element has index `0`, the second has index `1`, and so on.

Example:

```javascript
const fruits = ["Apple", "Banana", "Orange"];

console.log(fruits[0]);
console.log(fruits[1]);
console.log(fruits[2]);
```

Output:

```text
Apple
Banana
Orange
```

For an array containing `n` elements, the last element normally has index `n - 1`.

# 🗁 Accessing Array Elements

You can access array elements using bracket notation.

```javascript
const languages = ["JavaScript", "Python", "TypeScript"];

const firstLanguage = languages[0];
const secondLanguage = languages[1];

console.log(firstLanguage);
console.log(secondLanguage);
```

Output:

```text
JavaScript
Python
```

Accessing an index that does not contain an element returns `undefined`.

```javascript
const fruits = ["Apple", "Banana"];

console.log(fruits[5]);
```

Output:

```text
undefined
```

# 🗁 Updating Array Elements

You can modify an existing element by assigning a new value to its index.

```javascript
const fruits = ["Apple", "Banana", "Orange"];

fruits[1] = "Mango";

console.log(fruits);
```

Output:

```text
["Apple", "Mango", "Orange"]
```

Although the array is declared with `const`, its elements can still be modified.

`const` prevents reassignment of the array variable, not mutation of the array itself.

# 🗁 Array Length

The `length` property returns the number of array slots from index `0` through the highest assigned array index plus one.

Example:

```javascript
const fruits = ["Apple", "Banana", "Orange"];

console.log(fruits.length);
```

Output:

```text
3
```

You can also use `length` to access the last element.

```javascript
const fruits = ["Apple", "Banana", "Orange"];

console.log(fruits[fruits.length - 1]);
```

Output:

```text
Orange
```

# 🗁 Adding Elements to an Array

JavaScript provides methods for adding elements to arrays.

**push()**

Adds one or more elements to the end of an array.

```javascript
const fruits = ["Apple", "Banana"];

fruits.push("Orange");

console.log(fruits);
```

Output:

```text
["Apple", "Banana", "Orange"]
```

**unshift()**

Adds one or more elements to the beginning of an array.

```javascript
const fruits = ["Banana", "Orange"];

fruits.unshift("Apple");

console.log(fruits);
```

Output:

```text
["Apple", "Banana", "Orange"]
```

Both methods modify the original array and return its new length.

# 🗁 Removing Elements from an Array

JavaScript also provides methods for removing elements.

**pop()**

Removes the last element and returns the removed value.

```javascript
const fruits = ["Apple", "Banana", "Orange"];

const removed = fruits.pop();

console.log(removed);
console.log(fruits);
```

Output:

```text
Orange
["Apple", "Banana"]
```

**shift()**

Removes the first element and returns the removed value.

```javascript
const fruits = ["Apple", "Banana", "Orange"];

const removed = fruits.shift();

console.log(removed);
console.log(fruits);
```

Output:

```text
Apple
["Banana", "Orange"]
```

Both methods modify the original array.

# 🗁 Arrays with Different Data Types

JavaScript arrays can contain values of different data types.

```javascript
const data = [
  "Muhammad",
  25,
  true,
  null,
  undefined,
  { role: "Developer" },
  [10, 20, 30]
];

console.log(data);
```

An array does not require every element to have the same type.

However, using consistent data structures usually makes application code easier to maintain.

# 🗁 Arrays of Objects

Arrays are commonly used to store collections of objects.

```javascript
const users = [
  {
    id: 1,
    name: "Muhammad",
    role: "Developer"
  },
  {
    id: 2,
    name: "Alex",
    role: "Designer"
  },
  {
    id: 3,
    name: "John",
    role: "Manager"
  }
];

console.log(users[0].name);
console.log(users[1].role);
```

Output:

```text
Muhammad
Designer
```

This structure is especially useful when working with API responses, databases, and application state.

# 🗁 Nested Arrays

An array can contain other arrays. These are called nested arrays.

```javascript
const matrix = [
  [1, 2, 3],
  [4, 5, 6],
  [7, 8, 9]
];

console.log(matrix[0]);
console.log(matrix[1][2]);
```

Output:

```text
[1, 2, 3]
6
```

Nested arrays are useful for representing matrices, tables, grids, and hierarchical data.

# 🗁 Accessing Nested Array Elements

Each nested array has its own zero-based indexes.

```javascript
const matrix = [
  ["A", "B"],
  ["C", "D"],
  ["E", "F"]
];

console.log(matrix[0][0]);
console.log(matrix[1][1]);
console.log(matrix[2][0]);
```

Output:

```text
A
D
E
```

The first index selects the inner array, and the second index selects an element inside it.

# 🗁 Checking Whether a Value Is an Array

JavaScript's `typeof` operator returns `"object"` for arrays.

```javascript
const fruits = ["Apple", "Banana"];

console.log(typeof fruits);
```

Output:

```text
object
```

Use `Array.isArray()` to determine whether a value is an array.

```javascript
const fruits = ["Apple", "Banana"];
const user = { name: "Muhammad" };

console.log(Array.isArray(fruits));
console.log(Array.isArray(user));
```

Output:

```text
true
false
```

`Array.isArray()` is the recommended way to perform array checks.

# 🗁 Iterating Over Arrays

You can iterate over array elements using different looping techniques.

**for Loop**

```javascript
const fruits = ["Apple", "Banana", "Orange"];

for (let index = 0; index < fruits.length; index++) {
  console.log(fruits[index]);
}
```

Output:

```text
Apple
Banana
Orange
```

**for...of Loop**

The `for...of` loop directly iterates over array values.

```javascript
const fruits = ["Apple", "Banana", "Orange"];

for (const fruit of fruits) {
  console.log(fruit);
}
```

Output:

```text
Apple
Banana
Orange
```

**forEach()**

The `forEach()` method executes a callback function for each assigned element.

```javascript
const fruits = ["Apple", "Banana", "Orange"];

fruits.forEach((fruit, index) => {
  console.log(index, fruit);
});
```

Output:

```text
0 Apple
1 Banana
2 Orange
```

The `forEach()` method is covered in more detail in the array methods documentation.

# 🗁 Modifying Array Length

The `length` property can be used to change an array's size.

**Reducing Length**

Setting a smaller length removes elements beyond the new length.

```javascript
const numbers = [10, 20, 30, 40, 50];

numbers.length = 3;

console.log(numbers);
```

Output:

```text
[10, 20, 30]
```

**Increasing Length**

Setting a larger length creates additional empty slots.

```javascript
const numbers = [10, 20];

numbers.length = 5;

console.log(numbers);
console.log(numbers.length);
```

Output:

```text
[10, 20, empty × 3]
5
```

Increasing `length` does not populate the new slots with `undefined` values as actual stored elements. It creates holes.

# 🗁 Sparse Arrays

A sparse array contains empty slots, also called holes.

Example:

```javascript
const numbers = [10, , 30];

console.log(numbers);
console.log(numbers.length);
console.log(1 in numbers);
```

Output:

```text
[10, empty, 30]
3
false
```

The middle position exists in the array's index range but has no assigned property.

A hole is different from an explicitly stored `undefined` value.

```javascript
const sparse = [10, , 30];

const withUndefined = [10, undefined, 30];

console.log(1 in sparse);
console.log(1 in withUndefined);
```

Output:

```text
false
true
```

Sparse arrays can produce surprising behavior with certain array methods, so they should generally be avoided unless their behavior is specifically required.

# 🗁 Arrays and Reference Types

Arrays are objects and are assigned by reference.

Assigning an array to another variable does not automatically create a copy.

```javascript
const first = [10, 20, 30];

const second = first;

second[0] = 100;

console.log(first);
console.log(second);
```

Output:

```text
[100, 20, 30]
[100, 20, 30]
```

Both variables refer to the same array object.

# 🗁 Copying Arrays

There are several ways to create a shallow copy of an array.

**Spread Operator**

```javascript
const original = [10, 20, 30];

const copy = [...original];

copy[0] = 100;

console.log(original);
console.log(copy);
```

Output:

```text
[10, 20, 30]
[100, 20, 30]
```

**slice()**

```javascript
const original = [10, 20, 30];

const copy = original.slice();
```

**Array.from()**

```javascript
const original = [10, 20, 30];

const copy = Array.from(original);
```

These approaches create new arrays but do not recursively copy nested objects or arrays.

# 🗁 Shallow Copy Behavior

A shallow copy duplicates the outer array but preserves references to nested objects.

Example:

```javascript
const original = [
  { name: "Muhammad" },
  { name: "Alex" }
];

const copy = [...original];

copy[0].name = "John";

console.log(original[0].name);
console.log(copy[0].name);
```

Output:

```text
John
John
```

Both arrays reference the same first object.

For nested data structures, consider an appropriate deep-copy strategy when independent nested values are required.

# 🗁 Deep Copying Arrays

For structured-clone-compatible values, `structuredClone()` can create a deep copy.

```javascript
const original = [
  {
    name: "Muhammad",
    skills: ["JavaScript", "TypeScript"]
  }
];

const copy = structuredClone(original);

copy[0].name = "Alex";
copy[0].skills.push("Node.js");

console.log(original[0]);
console.log(copy[0]);
```

Output:

```text
{
  name: "Muhammad",
  skills: ["JavaScript", "TypeScript"]
}

{
  name: "Alex",
  skills: ["JavaScript", "TypeScript", "Node.js"]
}
```

`structuredClone()` supports many built-in data types but does not clone every JavaScript value, such as functions.

# 🗁 Array Destructuring

Array destructuring extracts elements into individual variables.

```javascript
const colors = ["Red", "Green", "Blue"];

const [first, second, third] = colors;

console.log(first);
console.log(second);
console.log(third);
```

Output:

```text
Red
Green
Blue
```

You can skip elements:

```javascript
const numbers = [10, 20, 30, 40];

const [first, , third] = numbers;

console.log(first);
console.log(third);
```

Output:

```text
10
30
```

You can also collect remaining elements using the rest syntax.

```javascript
const numbers = [10, 20, 30, 40];

const [first, ...remaining] = numbers;

console.log(first);
console.log(remaining);
```

Output:

```text
10
[20, 30, 40]
```

# 🗁 Checking Array Membership

The `includes()` method checks whether an array contains a specific value.

```javascript
const fruits = ["Apple", "Banana", "Orange"];

console.log(fruits.includes("Banana"));
console.log(fruits.includes("Mango"));
```

Output:

```text
true
false
```

The `indexOf()` method returns the first matching index or `-1` when the value is not found.

```javascript
const numbers = [10, 20, 30, 20];

console.log(numbers.indexOf(20));
console.log(numbers.indexOf(50));
```

Output:

```text
1
-1
```

For checking membership, `includes()` is usually more readable.

# 🗁 Array Methods Overview

JavaScript provides many built-in array methods for working with collections.

| Method | Purpose |
| --- | --- |
| `push()` | Add elements to the end |
| `pop()` | Remove the last element |
| `unshift()` | Add elements to the beginning |
| `shift()` | Remove the first element |
| `slice()` | Extract a portion into a new array |
| `splice()` | Add, remove, or replace elements |
| `concat()` | Combine arrays |
| `includes()` | Check whether a value exists |
| `indexOf()` | Find the first matching index |
| `find()` | Find the first matching element |
| `filter()` | Create an array of matching elements |
| `map()` | Transform array elements |
| `reduce()` | Accumulate array values |
| `some()` | Check whether at least one element matches |
| `every()` | Check whether all elements match |
| `sort()` | Sort elements |
| `reverse()` | Reverse element order |
| `flat()` | Flatten nested arrays |
| `flatMap()` | Map and flatten one level |
| `join()` | Convert elements into a string |
| `at()` | Access an element using an index |
| `forEach()` | Execute a callback for each element |

These methods are explored individually in the following topics.

# 🗁 Arrays and Immutability

JavaScript arrays are mutable, meaning their contents can be changed after creation.

Example:

```javascript
const numbers = [10, 20, 30];

numbers.push(40);

console.log(numbers);
```

Output:

```text
[10, 20, 30, 40]
```

However, many application architectures prefer immutable update patterns, where the original array is preserved and a new array is created.

Example:

```javascript
const original = [10, 20, 30];

const updated = [...original, 40];

console.log(original);
console.log(updated);
```

Output:

```text
[10, 20, 30]
[10, 20, 30, 40]
```

Immutable updates are particularly useful in React, state management, and predictable application architectures.

# 🗁 Practical Example: Managing a User List

The following example demonstrates common array operations in a simple user management scenario.

```javascript
const users = [
  { id: 1, name: "Muhammad", role: "Developer" },
  { id: 2, name: "Alex", role: "Designer" },
  { id: 3, name: "John", role: "Manager" }
];

// Access a user
console.log(users[0].name);

// Add a user
users.push({
  id: 4,
  name: "Sarah",
  role: "Tester"
});

// Update a user
users[1].role = "Frontend Developer";

// Find a user
const user = users.find(user => user.id === 3);

// Remove a user
const index = users.findIndex(user => user.id === 1);

if (index !== -1) {
  users.splice(index, 1);
}

console.log(user);
console.log(users);
```

The example demonstrates how arrays can manage collections of structured application data.

# 🗁 Common Mistakes

* Assuming array indexes start at `1`.
* Confusing `new Array(5)` with `[5]`.
* Using `typeof` to determine whether a value is an array.
* Assuming `const` makes array contents immutable.
* Forgetting that arrays are reference types.
* Assuming assigning an array to another variable creates a copy.
* Confusing shallow copying with deep copying.
* Forgetting that array methods such as `push()`, `pop()`, `shift()`, and `splice()` mutate the original array.
* Accidentally creating sparse arrays by assigning values to distant indexes.
* Assuming every array method handles sparse slots in the same way.

# 🗁 Best Practices

* Prefer array literals (`[]`) for creating arrays.
* Use `Array.isArray()` to check array types.
* Use descriptive variable names for collections.
* Prefer `for...of` when iterating over values.
* Use array methods such as `map()`, `filter()`, and `reduce()` when they improve readability.
* Avoid sparse arrays.
* Use immutable update patterns when managing application state.
* Understand the difference between shallow and deep copies.
* Use consistent data structures in arrays of objects.
* Avoid unnecessary mutations when predictable state management is important.
* Choose the appropriate array method based on whether you need to mutate or preserve the original array.

# 🗁 Key Takeaways

* Arrays store multiple values in a single variable.
* JavaScript arrays are zero-indexed objects.
* Arrays can contain values of different data types.
* Arrays can be created using literals, constructors, `Array.of()`, and `Array.from()`.
* The `length` property represents the array's length.
* Arrays are mutable even when declared with `const`.
* `push()`, `pop()`, `shift()`, and `unshift()` modify arrays.
* Arrays can contain objects and nested arrays.
* `Array.isArray()` checks whether a value is an array.
* Arrays are reference types, so assignments copy references rather than contents.
* Spread syntax, `slice()`, and `Array.from()` create shallow copies.
* `structuredClone()` can deep-copy supported values.
* Array destructuring provides convenient access to individual elements.
* JavaScript provides extensive built-in methods for processing arrays.
