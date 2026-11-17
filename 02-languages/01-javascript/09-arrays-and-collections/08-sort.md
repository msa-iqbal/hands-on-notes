# JavaScript Array `sort()`

The JavaScript `sort()` method sorts the elements of an array in place and returns the same array reference.

By default, `sort()` converts array elements into strings and compares their UTF-16 code unit sequences.

This means the default sorting behavior is suitable for many strings but can produce unexpected results when sorting numbers.

The `sort()` method is commonly used for:

* Sorting numbers in ascending or descending order.
* Sorting strings alphabetically.
* Sorting objects by their properties.
* Sorting products by price.
* Sorting users by name or age.
* Implementing ranking and leaderboard systems.
* Sorting dates and timestamps.

# 🗁 Basic Syntax

### 🖆 Syntax

```javascript
array.sort(compareFunction);
```

The `compareFunction` is optional.

* If omitted, elements are sorted according to their string representations.
* If provided, it determines the sorting order.

### 🖆 Basic Example

```javascript
const fruits = ["Banana", "Apple", "Mango", "Orange"];

fruits.sort();

console.log(fruits);
// ["Apple", "Banana", "Mango", "Orange"]
```

The original array is modified.

# 🗁 Default Sorting Behavior

### 🖆 Sorting Strings

By default, strings are sorted according to their UTF-16 code units.

```javascript
const names = ["Charlie", "Alice", "Bob", "David"];

names.sort();

console.log(names);
// ["Alice", "Bob", "Charlie", "David"]
```

### 🖆 Sorting Numbers Without a Compare Function

Default sorting converts numbers into strings before comparison.

```javascript
const numbers = [100, 25, 5, 40, 9];

numbers.sort();

console.log(numbers);
// [100, 25, 40, 5, 9]
```

This result is not numerically ascending.

The values are compared as strings:

```text
"100"
"25"
"40"
"5"
"9"
```

For numeric sorting, always provide a comparison function.

# 🗁 Understanding the Compare Function

A comparison function receives two elements:

```javascript
array.sort((a, b) => {
    // Return a negative number, zero, or a positive number.
});
```

The return value determines their relative order.

| Return value | Sorting behavior |
| --- | --- |
| Negative number | Place `a` before `b` |
| Positive number | Place `a` after `b` |
| Zero | Treat `a` and `b` as equal for sorting |

The comparison function does not need to return exactly `-1`, `0`, or `1`. Any negative or positive number works.

# 🗁 Sorting Numbers

### 🖆 Ascending Order

Use subtraction to sort numbers from smallest to largest.

```javascript
const numbers = [100, 25, 5, 40, 9];

numbers.sort((a, b) => a - b);

console.log(numbers);
// [5, 9, 25, 40, 100]
```

How it works:

* If `a - b` is negative, `a` comes first.
* If `a - b` is positive, `b` comes first.
* If the result is zero, their relative order is treated as equal.

### 🖆 Descending Order

Reverse the subtraction order.

```javascript
const numbers = [100, 25, 5, 40, 9];

numbers.sort((a, b) => b - a);

console.log(numbers);
// [100, 40, 25, 9, 5]
```

### 🖆 Sorting Negative Numbers

```javascript
const numbers = [10, -5, 20, -15, 0];

numbers.sort((a, b) => a - b);

console.log(numbers);
// [-15, -5, 0, 10, 20]
```

### 🖆 Sorting Decimal Numbers

```javascript
const prices = [19.99, 5.5, 12.75, 100.25];

prices.sort((a, b) => a - b);

console.log(prices);
// [5.5, 12.75, 19.99, 100.25]
```

# 🗁 Sorting Strings

### 🖆 Alphabetical Sorting

```javascript
const names = ["David", "Alice", "Charlie", "Bob"];

names.sort((a, b) => a.localeCompare(b));

console.log(names);
// ["Alice", "Bob", "Charlie", "David"]
```

`localeCompare()` compares strings according to language-sensitive collation rules.

It is generally preferable to default string sorting when working with human-readable text.

### 🖆 Descending Alphabetical Order

```javascript
const names = ["David", "Alice", "Charlie", "Bob"];

names.sort((a, b) => b.localeCompare(a));

console.log(names);
// ["David", "Charlie", "Bob", "Alice"]
```

### 🖆 Case-Insensitive Sorting

```javascript
const names = ["banana", "Apple", "cherry", "Date"];

names.sort((a, b) => {
    return a.localeCompare(b, undefined, {
        sensitivity: "base"
    });
});

console.log(names);
// ["Apple", "banana", "cherry", "Date"]
```

The `sensitivity: "base"` option makes comparisons insensitive to differences such as case and accents.

### 🖆 Numeric String Sorting

When sorting strings that represent numbers, use the `numeric` option.

```javascript
const versions = ["version10", "version2", "version1"];

versions.sort((a, b) => {
    return a.localeCompare(b, undefined, {
        numeric: true
    });
});

console.log(versions);
// ["version1", "version2", "version10"]
```

Without numeric comparison, `"version10"` may appear before `"version2"`.

# 🗁 Sorting Objects

The `sort()` method is especially useful when working with arrays of objects.

A comparison function can access object properties and determine the sorting order.

### 🖆 Sorting Objects by Age

```javascript
const users = [
    { name: "Alice", age: 30 },
    { name: "Bob", age: 20 },
    { name: "Charlie", age: 25 }
];

users.sort((a, b) => a.age - b.age);

console.log(users);
```

Result:

```javascript
[
    { name: "Bob", age: 20 },
    { name: "Charlie", age: 25 },
    { name: "Alice", age: 30 }
]
```

### 🖆 Sorting Objects by Name

```javascript
const users = [
    { name: "Charlie", age: 25 },
    { name: "Alice", age: 30 },
    { name: "Bob", age: 20 }
];

users.sort((a, b) => {
    return a.name.localeCompare(b.name);
});

console.log(users);
```

Result:

```javascript
[
    { name: "Alice", age: 30 },
    { name: "Bob", age: 20 },
    { name: "Charlie", age: 25 }
]
```

### 🖆 Sorting Products by Price

```javascript
const products = [
    { name: "Keyboard", price: 80 },
    { name: "Mouse", price: 25 },
    { name: "Monitor", price: 200 }
];

products.sort((a, b) => a.price - b.price);

console.log(products);
```

Result:

```javascript
[
    { name: "Mouse", price: 25 },
    { name: "Keyboard", price: 80 },
    { name: "Monitor", price: 200 }
]
```

### 🖆 Sorting by Multiple Properties

Sometimes one property is not enough to determine the sorting order.

For example, sort users by department first and name second.

```javascript
const users = [
    { name: "Charlie", department: "Engineering" },
    { name: "Alice", department: "Engineering" },
    { name: "Bob", department: "Marketing" },
    { name: "David", department: "Marketing" }
];

users.sort((a, b) => {
    const departmentComparison = a.department.localeCompare(
        b.department
    );

    if (departmentComparison !== 0) {
        return departmentComparison;
    }

    return a.name.localeCompare(b.name);
});

console.log(users);
```

Result:

```javascript
[
    { name: "Alice", department: "Engineering" },
    { name: "Charlie", department: "Engineering" },
    { name: "Bob", department: "Marketing" },
    { name: "David", department: "Marketing" }
]
```

# 🗁 Sorting Dates

JavaScript dates can be sorted by comparing their timestamps.

### 🖆 Ascending Date Order

```javascript
const dates = [
    new Date("2025-06-15"),
    new Date("2023-01-10"),
    new Date("2024-09-20")
];

dates.sort((a, b) => a - b);

console.log(dates);
```

The result is ordered from the oldest date to the newest date.

### 🖆 Descending Date Order

```javascript
dates.sort((a, b) => b - a);
```

### 🖆 Sorting Objects by Date

```javascript
const events = [
    { name: "Conference", date: "2025-08-20" },
    { name: "Workshop", date: "2025-03-10" },
    { name: "Seminar", date: "2025-06-15" }
];

events.sort((a, b) => {
    return new Date(a.date) - new Date(b.date);
});

console.log(events);
```

For large collections, consider converting date strings into timestamps once instead of repeatedly parsing them inside the comparator.

# 🗁 Sort Stability

Modern JavaScript requires `sort()` to be stable.

A stable sort preserves the relative order of elements that compare equal.

### 🖆 Example

```javascript
const users = [
    { name: "Alice", department: "Engineering" },
    { name: "Bob", department: "Marketing" },
    { name: "Charlie", department: "Engineering" },
    { name: "David", department: "Marketing" }
];

users.sort((a, b) => {
    return a.department.localeCompare(b.department);
});

console.log(users);
```

Result:

```javascript
[
    { name: "Alice", department: "Engineering" },
    { name: "Charlie", department: "Engineering" },
    { name: "Bob", department: "Marketing" },
    { name: "David", department: "Marketing" }
]
```

Alice remains before Charlie, and Bob remains before David, because their department comparisons are equal within each group.

# 🗁 `sort()` Mutates the Original Array

One important characteristic of `sort()` is that it changes the original array.

### 🖆 Example

```javascript
const numbers = [5, 2, 8, 1];

const sortedNumbers = numbers.sort((a, b) => a - b);

console.log(numbers);
// [1, 2, 5, 8]

console.log(sortedNumbers);
// [1, 2, 5, 8]

console.log(numbers === sortedNumbers);
// true
```

Both variables reference the same array.

This can be problematic when working with immutable state, React state, or shared data.

# 🗁 Sorting Without Mutating the Original Array

There are several ways to create a sorted copy.

### 🖆 Using the Spread Operator

```javascript
const numbers = [5, 2, 8, 1];

const sortedNumbers = [...numbers].sort((a, b) => a - b);

console.log(numbers);
// [5, 2, 8, 1]

console.log(sortedNumbers);
// [1, 2, 5, 8]
```

### 🖆 Using `slice()`

```javascript
const numbers = [5, 2, 8, 1];

const sortedNumbers = numbers.slice().sort((a, b) => a - b);

console.log(numbers);
// [5, 2, 8, 1]

console.log(sortedNumbers);
// [1, 2, 5, 8]
```

### 🖆 Using `toSorted()`

JavaScript also provides `toSorted()`, which returns a new sorted array without modifying the original.

```javascript
const numbers = [5, 2, 8, 1];

const sortedNumbers = numbers.toSorted((a, b) => a - b);

console.log(numbers);
// [5, 2, 8, 1]

console.log(sortedNumbers);
// [1, 2, 5, 8]
```

`toSorted()` is the copying counterpart of `sort()`.

It is part of the modern JavaScript array copying methods.

# 🗁 Understanding `sort()` and `toSorted()`

| Feature | `sort()` | `toSorted()` |
| --- | --- | --- |
| Sorts elements | Yes | Yes |
| Modifies original array | Yes | No |
| Returns an array | Same array reference | New array |
| Supports compare function | Yes | Yes |
| Useful for immutable workflows | Requires copying first | Yes |

# 🗁 Sorting Special Values

Special values require additional consideration when sorting.

### 🖆 Sorting `undefined`

When using the built-in `sort()` behavior, `undefined` values are placed at the end of the array.

The comparator is not called for `undefined` elements.

```javascript
const values = [3, undefined, 1, 2];

values.sort((a, b) => a - b);

console.log(values);
// [1, 2, 3, undefined]
```

### 🖆 Sorting `null`

`null` is not treated the same as `undefined`.

In numeric subtraction, `null` is converted to `0`.

```javascript
const values = [3, null, 1, 2];

values.sort((a, b) => a - b);

console.log(values);
// [null, 1, 2, 3]
```

### 🖆 Sorting Mixed Types

Avoid relying on default sorting when an array contains different data types.

```javascript
const values = [10, "2", 5, "20"];

values.sort((a, b) => Number(a) - Number(b));

console.log(values);
// ["2", 5, 10, "20"]
```

The elements retain their original types, even though the comparator converts them to numbers for comparison.

# 🗁 Sorting with `Intl.Collator`

For applications that sort large collections of human-readable strings, `Intl.Collator` can provide reusable locale-aware comparison.

### 🖆 Basic Usage

```javascript
const collator = new Intl.Collator("en", {
    sensitivity: "base",
    numeric: true
});

const names = ["item10", "item2", "item1"];

names.sort(collator.compare);

console.log(names);
// ["item1", "item2", "item10"]
```

The collator can be reused across multiple sorting operations.

```javascript
const collator = new Intl.Collator("en");

const first = ["Banana", "Apple", "Mango"];
const second = ["Zebra", "Cat", "Dog"];

first.sort(collator.compare);
second.sort(collator.compare);
```

This is useful when an application performs repeated locale-aware sorting.

# 🗁 Common Mistakes

### 🖆 Forgetting the Numeric Compare Function

Incorrect:

```javascript
const numbers = [10, 2, 30, 4];

numbers.sort();

console.log(numbers);
// [10, 2, 30, 4]
```

Correct:

```javascript
numbers.sort((a, b) => a - b);

console.log(numbers);
// [2, 4, 10, 30]
```

### 🖆 Reversing the Comparison Logic

Ascending:

```javascript
numbers.sort((a, b) => a - b);
```

Descending:

```javascript
numbers.sort((a, b) => b - a);
```

Changing the subtraction order reverses the numeric sorting direction.

### 🖆 Returning Boolean Values from the Comparator

Avoid:

```javascript
const numbers = [5, 2, 8, 1];

numbers.sort((a, b) => a > b);

console.log(numbers);
```

A comparator should return a negative number, zero, or a positive number. Returning booleans does not express the intended ordering reliably.

Correct:

```javascript
numbers.sort((a, b) => a - b);
```

### 🖆 Mutating Shared Data Accidentally

Avoid directly sorting shared state when the original order must remain unchanged.

```javascript
const sortedUsers = [...users].sort((a, b) => {
    return a.name.localeCompare(b.name);
});
```

Or use `toSorted()`:

```javascript
const sortedUsers = users.toSorted((a, b) => {
    return a.name.localeCompare(b.name);
});
```

### 🖆 Writing an Inconsistent Comparator

A comparator should provide a consistent ordering.

For example, avoid comparators that produce contradictory results depending on unrelated mutable state or random values.

A reliable comparator should consistently determine the relative order of the same pair of values.

# 🗁 Performance Considerations

* `sort()` generally has \(O(n \log n)\) time complexity, although ECMAScript does not mandate a specific sorting algorithm or complexity.
* Sorting happens in place.
* `toSorted()` creates a new array before returning the sorted result.
* Expensive comparison functions can significantly affect performance.
* Cache calculated values when repeatedly comparing expensive properties.
* Use `Intl.Collator` for efficient repeated locale-aware string comparisons.
* Avoid sorting when a collection does not need an ordered representation.
* For large datasets, consider whether sorting should happen in the database or backend rather than in the browser.

# 🗁 Best Practices

* Always provide a compare function when sorting numbers.
* Use `localeCompare()` for human-readable string sorting.
* Use `Intl.Collator` when performing repeated locale-aware comparisons.
* Use `a - b` for ascending numeric order.
* Use `b - a` for descending numeric order.
* Use `toSorted()` when the original array must remain unchanged.
* Use `[...array].sort()` when a copying alternative is needed.
* Use stable, deterministic comparison logic.
* Handle missing or invalid values explicitly when sorting application data.
* Avoid mutating shared application state unexpectedly.

# 🗁 Summary

The JavaScript `sort()` method sorts array elements and modifies the original array.

Key points:

* `sort()` returns the same array reference.
* Default sorting compares string representations.
* Numeric sorting requires a compare function.
* `a - b` sorts numbers in ascending order.
* `b - a` sorts numbers in descending order.
* `localeCompare()` supports language-sensitive string comparison.
* `sort()` is stable in modern JavaScript.
* `toSorted()` returns a new sorted array without mutating the original.
* Sorting object arrays requires comparing their properties.
* A consistent comparator is essential for predictable results.

**Remember:**

* `sort()` modifies the original array.
* `toSorted()` creates a new sorted array.
* Always define the comparison logic when the default ordering is not appropriate.
