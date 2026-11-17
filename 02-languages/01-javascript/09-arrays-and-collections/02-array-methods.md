# JavaScript Array Methods

Array methods are built-in functions that allow you to inspect, modify, transform, search, and manipulate arrays.

JavaScript provides a rich collection of array methods, including methods for adding and removing elements, copying arrays, extracting portions, and iterating over values.

Understanding these methods is essential for writing clean, readable, and efficient JavaScript.

# 🗁 Adding and Removing Elements

### 🖆 `push()`

Adds one or more elements to the end of an array.

Returns the new array length.

```javascript
const fruits = ["Apple", "Banana"];

const length = fruits.push("Orange", "Mango");

console.log(fruits);
// ["Apple", "Banana", "Orange", "Mango"]

console.log(length);
// 4
```

### 🖆 `pop()`

Removes and returns the last element of an array.

```javascript
const fruits = ["Apple", "Banana", "Orange"];

const removed = fruits.pop();

console.log(removed);
// "Orange"

console.log(fruits);
// ["Apple", "Banana"]
```

Returns `undefined` when the array is empty.

### 🖆 `unshift()`

Adds one or more elements to the beginning of an array.

Returns the new array length.

```javascript
const fruits = ["Banana", "Orange"];

const length = fruits.unshift("Apple");

console.log(fruits);
// ["Apple", "Banana", "Orange"]

console.log(length);
// 3
```

### 🖆 `shift()`

Removes and returns the first element of an array.

```javascript
const fruits = ["Apple", "Banana", "Orange"];

const removed = fruits.shift();

console.log(removed);
// "Apple"

console.log(fruits);
// ["Banana", "Orange"]
```

Returns `undefined` when the array is empty.

# 🗁 Extracting and Modifying Array Contents

### 🖆 `slice()`

Returns a shallow copy of a portion of an array without modifying the original array.

Syntax:

```javascript
array.slice(start, end);
```

* `start`: Index where extraction begins.
* `end`: Index where extraction stops, excluding that index.
* Negative indexes count backward from the end.
* Both arguments are optional.

```javascript
const numbers = [10, 20, 30, 40, 50];

console.log(numbers.slice(1, 4));
// [20, 30, 40]

console.log(numbers.slice(2));
// [30, 40, 50]

console.log(numbers.slice(-2));
// [40, 50]

console.log(numbers.slice());
// [10, 20, 30, 40, 50]

console.log(numbers);
// Original array remains unchanged
```

### 🖆 `splice()`

Adds, removes, or replaces elements in an array by modifying the original array.

Syntax:

```javascript
array.splice(start, deleteCount, item1, item2, ...);
```

* `start`: Index where modification begins.
* `deleteCount`: Number of elements to remove.
* Additional arguments specify elements to insert.

**Remove elements:**

```javascript
const fruits = ["Apple", "Banana", "Orange", "Mango"];

const removed = fruits.splice(1, 2);

console.log(removed);
// ["Banana", "Orange"]

console.log(fruits);
// ["Apple", "Mango"]
```

**Insert elements:**

```javascript
const fruits = ["Apple", "Mango"];

fruits.splice(1, 0, "Banana", "Orange");

console.log(fruits);
// ["Apple", "Banana", "Orange", "Mango"]
```

**Replace elements:**

```javascript
const fruits = ["Apple", "Banana", "Orange"];

fruits.splice(1, 1, "Mango");

console.log(fruits);
// ["Apple", "Mango", "Orange"]
```

### 🖆 `toSpliced()`

Returns a new array with elements added, removed, or replaced without modifying the original array.

It is the copying counterpart of `splice()`.

```javascript
const numbers = [10, 20, 30, 40];

const result = numbers.toSpliced(1, 2, 99);

console.log(result);
// [10, 99, 40]

console.log(numbers);
// [10, 20, 30, 40]
```

`toSpliced()` is available in modern JavaScript environments.

### 🖆 `with()`

Returns a new array with the element at a specified index replaced.

It does not modify the original array.

```javascript
const numbers = [10, 20, 30, 40];

const updated = numbers.with(1, 99);

console.log(updated);
// [10, 99, 30, 40]

console.log(numbers);
// [10, 20, 30, 40]
```

Negative indexes count backward from the end.

```javascript
const numbers = [10, 20, 30];

console.log(numbers.with(-1, 99));
// [10, 20, 99]
```

# 🗁 Combining and Flattening Arrays

### 🖆 `concat()`

Combines two or more arrays and returns a new array.

```javascript
const first = [1, 2];
const second = [3, 4];
const third = [5, 6];

const result = first.concat(second, third);

console.log(result);
// [1, 2, 3, 4, 5, 6]
```

It can also combine arrays with individual values.

```javascript
const numbers = [1, 2];

console.log(numbers.concat(3, [4, 5]));
// [1, 2, 3, 4, 5]
```

`concat()` does not modify the original arrays.

### 🖆 `flat()`

Creates a new array by flattening nested arrays up to a specified depth.

```javascript
const numbers = [1, [2, 3], [4, [5, 6]]];

console.log(numbers.flat());
// [1, 2, 3, 4, [5, 6]]

console.log(numbers.flat(2));
// [1, 2, 3, 4, 5, 6]
```

The default flattening depth is `1`.

Use `Infinity` to flatten all nested levels.

```javascript
const numbers = [1, [2, [3, [4]]]];

console.log(numbers.flat(Infinity));
// [1, 2, 3, 4]
```

### 🖆 `flatMap()`

Maps each array element using a callback function and then flattens the result by one level.

It combines `map()` and `flat(1)`.

```javascript
const numbers = [1, 2, 3];

const result = numbers.flatMap(number => [number, number * 2]);

console.log(result);
// [1, 2, 2, 4, 3, 6]
```

It is useful when each input element can produce zero, one, or multiple output elements.

```javascript
const words = ["hello world", "javascript basics"];

const result = words.flatMap(sentence => sentence.split(" "));

console.log(result);
// ["hello", "world", "javascript", "basics"]
```

# 🗁 Reversing and Copying Arrays

### 🖆 `reverse()`

Reverses the order of elements in an array by modifying the original array.

```javascript
const numbers = [1, 2, 3, 4];

numbers.reverse();

console.log(numbers);
// [4, 3, 2, 1]
```

### 🖆 `toReversed()`

Returns a new array with elements in reverse order without modifying the original array.

```javascript
const numbers = [1, 2, 3, 4];

const reversed = numbers.toReversed();

console.log(reversed);
// [4, 3, 2, 1]

console.log(numbers);
// [1, 2, 3, 4]
```

### 🖆 `copyWithin()`

Copies a portion of an array to another position within the same array.

It modifies the original array and does not change its length.

Syntax:

```javascript
array.copyWithin(target, start, end);
```

* `target`: Index where copied elements should be placed.
* `start`: Index where copying begins.
* `end`: Index where copying stops, excluding that index.

```javascript
const numbers = [1, 2, 3, 4, 5];

numbers.copyWithin(0, 3);

console.log(numbers);
// [4, 5, 3, 4, 5]
```

Another example:

```javascript
const numbers = [1, 2, 3, 4, 5];

numbers.copyWithin(1, 2, 4);

console.log(numbers);
// [1, 3, 4, 4, 5]
```

# 🗁 Searching and Checking Elements

### 🖆 `indexOf()`

Returns the first index of a specified value.

Returns `-1` if the value is not found.

```javascript
const fruits = ["Apple", "Banana", "Orange", "Banana"];

console.log(fruits.indexOf("Banana"));
// 1

console.log(fruits.indexOf("Mango"));
// -1
```

It uses strict equality (`===`) for comparison.

### 🖆 `lastIndexOf()`

Returns the last index of a specified value.

```javascript
const fruits = ["Apple", "Banana", "Orange", "Banana"];

console.log(fruits.lastIndexOf("Banana"));
// 3
```

### 🖆 `includes()`

Checks whether an array contains a specified value.

Returns `true` or `false`.

```javascript
const fruits = ["Apple", "Banana", "Orange"];

console.log(fruits.includes("Banana"));
// true

console.log(fruits.includes("Mango"));
// false
```

Unlike `indexOf()`, `includes()` can detect `NaN`.

```javascript
const values = [10, 20, NaN];

console.log(values.includes(NaN));
// true
```

### 🖆 `at()`

Returns the element at a specified index.

It supports both positive and negative indexes.

```javascript
const fruits = ["Apple", "Banana", "Orange"];

console.log(fruits.at(0));
// "Apple"

console.log(fruits.at(-1));
// "Orange"

console.log(fruits.at(-2));
// "Banana"
```

Negative indexes count backward from the end.

# 🗁 Iterating Over Arrays

### 🖆 `forEach()`

Executes a callback function once for each array element.

```javascript
const numbers = [10, 20, 30];

numbers.forEach(number => {
    console.log(number);
});
```

It can also provide the current index and original array.

```javascript
const fruits = ["Apple", "Banana", "Orange"];

fruits.forEach((fruit, index, array) => {
    console.log(index, fruit);
});
```

Important characteristics:

* Returns `undefined`.
* Does not create a new array.
* Cannot be stopped using `break` or `continue`.
* Does not wait for asynchronous callbacks to finish.

### 🖆 `entries()`

Returns an iterator containing key-value pairs for each array element.

Each pair contains the element's index and value.

```javascript
const fruits = ["Apple", "Banana", "Orange"];

const iterator = fruits.entries();

for (const [index, fruit] of iterator) {
    console.log(index, fruit);
}
```

### 🖆 `keys()`

Returns an iterator containing the array's indexes.

```javascript
const fruits = ["Apple", "Banana", "Orange"];

for (const index of fruits.keys()) {
    console.log(index);
}

// 0
// 1
// 2
```

### 🖆 `values()`

Returns an iterator containing the array's values.

```javascript
const fruits = ["Apple", "Banana", "Orange"];

for (const fruit of fruits.values()) {
    console.log(fruit);
}
```

Using `for...of` directly on an array also iterates over its values.

# 🗁 Converting Arrays to Strings

### 🖆 `join()`

Combines all array elements into a string using a specified separator.

```javascript
const fruits = ["Apple", "Banana", "Orange"];

console.log(fruits.join());
// "Apple,Banana,Orange"

console.log(fruits.join(" - "));
// "Apple - Banana - Orange"

console.log(fruits.join(""));
// "AppleBananaOrange"
```

The default separator is a comma.

`null` and `undefined` elements are represented as empty strings.

```javascript
const values = ["A", null, "B", undefined, "C"];

console.log(values.join("-"));
// "A--B--C"
```

### 🖆 `toString()`

Converts an array into a comma-separated string.

```javascript
const numbers = [10, 20, 30];

console.log(numbers.toString());
// "10,20,30"
```

For nested arrays, conversion can produce a flattened-looking string because array elements are converted to strings recursively.

```javascript
const values = [1, [2, 3], 4];

console.log(values.toString());
// "1,2,3,4"
```

# 🗁 Filling and Creating Arrays

### 🖆 `fill()`

Fills all or part of an array with a specified value.

It modifies the original array.

Syntax:

```javascript
array.fill(value, start, end);
```

```javascript
const numbers = [1, 2, 3, 4, 5];

numbers.fill(0);

console.log(numbers);
// [0, 0, 0, 0, 0]
```

Filling a specific range:

```javascript
const numbers = [1, 2, 3, 4, 5];

numbers.fill(99, 1, 4);

console.log(numbers);
// [1, 99, 99, 99, 5]
```

### 🖆 `Array.from()`

Creates a new array from an iterable or array-like object.

```javascript
const text = "hello";

const characters = Array.from(text);

console.log(characters);
// ["h", "e", "l", "l", "o"]
```

It also accepts a mapping function.

```javascript
const numbers = Array.from([1, 2, 3], number => number * 2);

console.log(numbers);
// [2, 4, 6]
```

### 🖆 `Array.of()`

Creates a new array from the provided arguments.

```javascript
const numbers = Array.of(5);

console.log(numbers);
// [5]

const values = Array.of(1, 2, 3);

console.log(values);
// [1, 2, 3]
```

Unlike the `Array()` constructor, `Array.of()` treats a single numeric argument as an array element.

```javascript
console.log(Array(5));
// [empty × 5]

console.log(Array.of(5));
// [5]
```

# 🗁 Mutating vs Non-Mutating Methods

Array methods can be classified by whether they modify the original array.

### 🖆 Mutating Methods

These methods modify the original array:

* `push()`
* `pop()`
* `shift()`
* `unshift()`
* `splice()`
* `sort()`
* `reverse()`
* `copyWithin()`
* `fill()`

Example:

```javascript
const numbers = [3, 1, 2];

numbers.sort();

console.log(numbers);
// [1, 2, 3]
```

### 🖆 Non-Mutating Methods

These methods return a result without modifying the original array:

* `slice()`
* `concat()`
* `flat()`
* `flatMap()`
* `map()`
* `filter()`
* `toSorted()`
* `toReversed()`
* `toSpliced()`
* `with()`
* `includes()`
* `indexOf()`
* `join()`

Example:

```javascript
const numbers = [3, 1, 2];

const sorted = numbers.toSorted();

console.log(sorted);
// [1, 2, 3]

console.log(numbers);
// [3, 1, 2]
```

Note that `map()`, `filter()`, and several other methods return new arrays, but the copies are shallow when they contain objects or nested arrays.

# 🗁 Shallow Copy Behavior

Many array methods create shallow copies rather than deep copies.

A shallow copy creates a new array, but nested objects are still shared by reference.

```javascript
const users = [
    { name: "Alice" },
    { name: "Bob" }
];

const copiedUsers = users.slice();

copiedUsers[0].name = "Charlie";

console.log(users[0].name);
// "Charlie"
```

The outer arrays are different objects, but both contain a reference to the same first user object.

To create a deep copy of structured-cloneable data, use `structuredClone()`:

```javascript
const users = [
    { name: "Alice" },
    { name: "Bob" }
];

const copiedUsers = structuredClone(users);

copiedUsers[0].name = "Charlie";

console.log(users[0].name);
// "Alice"
```

# 🗁 Important Performance Considerations

* `push()` and `pop()` are generally efficient operations.
* `shift()` and `unshift()` may require moving many existing elements.
* `splice()` can require shifting elements depending on the modification position.
* `includes()`, `indexOf()`, and `find()` generally perform linear searches.
* `slice()` and `concat()` create new arrays and copy element references.
* `sort()` typically has a time complexity around \(O(n \log n)\), although the exact implementation is engine-dependent.
* Avoid creating unnecessary intermediate arrays when processing large datasets.

Choose array methods based on readability, mutation requirements, and performance characteristics rather than assuming one method is always faster.

# 🗁 Best Practices

* Prefer non-mutating methods when working with immutable state.
* Use `slice()` when you need a shallow copy of part or all of an array.
* Use `splice()` when modifying the original array is intentional.
* Use `toSpliced()`, `toSorted()`, and `toReversed()` when you need immutable alternatives.
* Use `includes()` for simple value-existence checks.
* Use `at(-1)` when accessing the last element.
* Use `forEach()` for side effects rather than creating transformed arrays.
* Use `map()` when transforming every element.
* Use `filter()` when selecting matching elements.
* Use `reduce()` when accumulating values.
* Remember that copying an array does not automatically clone nested objects.
* Avoid mutating shared arrays unexpectedly.

# 🗁 Summary

| Method | Purpose | Mutates Original |
| --- | --- | --- |
| `push()` | Add elements to the end | Yes |
| `pop()` | Remove the last element | Yes |
| `shift()` | Remove the first element | Yes |
| `unshift()` | Add elements to the beginning | Yes |
| `slice()` | Extract a portion | No |
| `splice()` | Insert, remove, or replace elements | Yes |
| `toSpliced()` | Immutable splice operation | No |
| `with()` | Replace an element by index | No |
| `concat()` | Combine arrays | No |
| `flat()` | Flatten nested arrays | No |
| `flatMap()` | Map and flatten one level | No |
| `reverse()` | Reverse element order | Yes |
| `toReversed()` | Return a reversed copy | No |
| `copyWithin()` | Copy elements within an array | Yes |
| `indexOf()` | Find the first matching index | No |
| `lastIndexOf()` | Find the last matching index | No |
| `includes()` | Check whether a value exists | No |
| `at()` | Access an element by index | No |
| `forEach()` | Execute a callback for each element | No |
| `join()` | Convert elements into a string | No |
| `fill()` | Fill elements with a value | Yes |
| `Array.from()` | Create an array from iterable or array-like data | No |
| `Array.of()` | Create an array from arguments | No |

The next topics will explore the most important array-processing methods individually: `map()`, `filter()`, `reduce()`, and other higher-level operations.
