# JavaScript Array `reduce()`

The `reduce()` method is a built-in JavaScript array method that executes a callback function for each array element and accumulates the results into a single value.

Unlike `map()` and `filter()`, which return arrays, `reduce()` can return any data type, including:

* Numbers
* Strings
* Objects
* Arrays
* Booleans
* Maps and other collections

`reduce()` is commonly used for:

* Calculating totals and averages.
* Counting occurrences.
* Grouping objects.
* Transforming arrays into objects.
* Flattening nested data.
* Building lookup structures.
* Implementing data aggregation pipelines.

# 🗁 Syntax

### 🖆 Basic Syntax

```javascript
const result = array.reduce(callback, initialValue);
```

### 🖆 Complete Syntax

```javascript
const result = array.reduce(
    (accumulator, currentValue, currentIndex, array) => {
        // Update and return the accumulator.
    },
    initialValue
);
```

The callback receives four arguments:

| Parameter | Description |
| --- | --- |
| `accumulator` | Value accumulated from previous callback executions |
| `currentValue` | Current element being processed |
| `currentIndex` | Index of the current element |
| `array` | Original array being processed |

The `initialValue` is the optional starting value of the accumulator.

For predictable behavior, explicitly provide an initial value in most practical use cases.

# 🗁 Understanding the Accumulator

The accumulator stores the result of each callback execution.

Consider the following example:

```javascript
const numbers = [1, 2, 3, 4];

const total = numbers.reduce((accumulator, currentValue) => {
    return accumulator + currentValue;
}, 0);

console.log(total);
// 10
```

The execution process:

| Iteration | Accumulator | Current Value | Returned Value |
| --- | ---: | ---: | ---: |
| Initial | 0 | - | 0 |
| First | 0 | 1 | 1 |
| Second | 1 | 2 | 3 |
| Third | 3 | 3 | 6 |
| Fourth | 6 | 4 | 10 |

The final accumulated value becomes the return value of `reduce()`.

### 🖆 Visual Execution Flow

```text
Initial Value: 0
       |
       v
  Accumulator = 0
       |
       v
  Current Value = 1
       |
       v
  0 + 1 = 1
       |
       v
  Accumulator = 1
       |
       v
  Current Value = 2
       |
       v
  1 + 2 = 3
       |
       v
  Accumulator = 3
       |
       v
  Current Value = 3
       |
       v
  3 + 3 = 6
       |
       v
  Accumulator = 6
       |
       v
  Current Value = 4
       |
       v
  6 + 4 = 10
       |
       v
  Final Result = 10
```

# 🗁 Basic Usage

### 🖆 Calculating the Sum

Add all numbers in an array.

```javascript
const numbers = [10, 20, 30, 40];

const total = numbers.reduce((sum, number) => {
    return sum + number;
}, 0);

console.log(total);
// 100
```

### 🖆 Calculating the Product

Multiply all numbers together.

```javascript
const numbers = [2, 3, 4, 5];

const product = numbers.reduce((result, number) => {
    return result * number;
}, 1);

console.log(product);
// 120
```

### 🖆 Finding the Maximum Number

```javascript
const numbers = [10, 45, 20, 80, 35];

const maximum = numbers.reduce((max, number) => {
    return number > max ? number : max;
}, -Infinity);

console.log(maximum);
// 80
```

### 🖆 Finding the Minimum Number

```javascript
const numbers = [10, 45, 20, 80, 35];

const minimum = numbers.reduce((min, number) => {
    return number < min ? number : min;
}, Infinity);

console.log(minimum);
// 10
```

### 🖆 Calculating the Average

```javascript
const numbers = [10, 20, 30, 40, 50];

const total = numbers.reduce((sum, number) => {
    return sum + number;
}, 0);

const average = numbers.length > 0
    ? total / numbers.length
    : 0;

console.log(average);
// 30
```

# 🗁 Working with Strings

### 🖆 Joining Strings

Combine array elements into a single string.

```javascript
const words = ["JavaScript", "is", "powerful"];

const sentence = words.reduce((result, word) => {
    return `${result} ${word}`;
});

console.log(sentence);
// "JavaScript is powerful"
```

For simple string joining, `join()` is usually more appropriate.

```javascript
const sentence = words.join(" ");

console.log(sentence);
// "JavaScript is powerful"
```

### 🖆 Counting Total Characters

```javascript
const words = ["JavaScript", "TypeScript", "Python"];

const totalCharacters = words.reduce((total, word) => {
    return total + word.length;
}, 0);

console.log(totalCharacters);
// 27
```

# 🗁 Working with Objects

### 🖆 Calculating Total Product Prices

```javascript
const products = [
    { name: "Keyboard", price: 50 },
    { name: "Mouse", price: 25 },
    { name: "Monitor", price: 200 }
];

const total = products.reduce((sum, product) => {
    return sum + product.price;
}, 0);

console.log(total);
// 275
```

### 🖆 Counting Object Records

Count the total number of users.

```javascript
const users = [
    { id: 1, name: "Alice" },
    { id: 2, name: "Bob" },
    { id: 3, name: "Charlie" }
];

const totalUsers = users.reduce((count) => {
    return count + 1;
}, 0);

console.log(totalUsers);
// 3
```

For simply counting array elements, `length` is preferable.

### 🖆 Extracting Object Properties

Create an array containing only user names.

```javascript
const users = [
    { id: 1, name: "Alice" },
    { id: 2, name: "Bob" },
    { id: 3, name: "Charlie" }
];

const names = users.reduce((result, user) => {
    result.push(user.name);

    return result;
}, []);

console.log(names);
// ["Alice", "Bob", "Charlie"]
```

For this particular task, `map()` is more readable.

```javascript
const names = users.map(user => user.name);
```

# 🗁 Advanced Usage

### 🖆 Counting Element Occurrences

Count how many times each value appears in an array.

```javascript
const fruits = [
    "Apple",
    "Banana",
    "Apple",
    "Orange",
    "Banana",
    "Apple"
];

const counts = fruits.reduce((result, fruit) => {
    result[fruit] = (result[fruit] || 0) + 1;

    return result;
}, {});

console.log(counts);

// {
//     Apple: 3,
//     Banana: 2,
//     Orange: 1
// }
```

### 🖆 Grouping Objects by Property

Group users according to their role.

```javascript
const users = [
    { id: 1, name: "Alice", role: "admin" },
    { id: 2, name: "Bob", role: "user" },
    { id: 3, name: "Charlie", role: "admin" },
    { id: 4, name: "David", role: "user" }
];

const groupedUsers = users.reduce((groups, user) => {
    const role = user.role;

    if (!groups[role]) {
        groups[role] = [];
    }

    groups[role].push(user);

    return groups;
}, {});

console.log(groupedUsers);
```

Result:

```javascript
{
    admin: [
        { id: 1, name: "Alice", role: "admin" },
        { id: 3, name: "Charlie", role: "admin" }
    ],
    user: [
        { id: 2, name: "Bob", role: "user" },
        { id: 4, name: "David", role: "user" }
    ]
}
```

### 🖆 Converting an Array into an Object

Create an object where each user's ID becomes a key.

```javascript
const users = [
    { id: 101, name: "Alice" },
    { id: 102, name: "Bob" },
    { id: 103, name: "Charlie" }
];

const userMap = users.reduce((result, user) => {
    result[user.id] = user;

    return result;
}, {});

console.log(userMap);
```

Result:

```javascript
{
    101: { id: 101, name: "Alice" },
    102: { id: 102, name: "Bob" },
    103: { id: 103, name: "Charlie" }
}
```

For lookup structures with arbitrary keys, `Map` can be a better choice.

### 🖆 Flattening Nested Arrays

Flatten a nested array into a single-level array.

```javascript
const nested = [
    [1, 2],
    [3, 4],
    [5, 6]
];

const flattened = nested.reduce((result, currentArray) => {
    return result.concat(currentArray);
}, []);

console.log(flattened);
// [1, 2, 3, 4, 5, 6]
```

For ordinary flattening, `flat()` is simpler.

```javascript
const flattened = nested.flat();
```

### 🖆 Removing Duplicate Values

Create an array containing unique values.

```javascript
const numbers = [1, 2, 2, 3, 3, 4, 5, 5];

const unique = numbers.reduce((result, number) => {
    if (!result.includes(number)) {
        result.push(number);
    }

    return result;
}, []);

console.log(unique);
// [1, 2, 3, 4, 5]
```

For this task, `Set` is generally more efficient and readable.

```javascript
const unique = [...new Set(numbers)];

console.log(unique);
// [1, 2, 3, 4, 5]
```

### 🖆 Building a Nested Object

Transform a list of records into a nested structure.

```javascript
const employees = [
    { department: "IT", name: "Alice" },
    { department: "HR", name: "Bob" },
    { department: "IT", name: "Charlie" }
];

const result = employees.reduce((groups, employee) => {
    const department = employee.department;

    if (!groups[department]) {
        groups[department] = {
            employees: []
        };
    }

    groups[department].employees.push(employee.name);

    return groups;
}, {});

console.log(result);
```

Result:

```javascript
{
    IT: {
        employees: ["Alice", "Charlie"]
    },
    HR: {
        employees: ["Bob"]
    }
}
```

# 🗁 Understanding the Initial Value

The second argument of `reduce()` specifies the initial accumulator value.

### 🖆 Providing an Initial Value

```javascript
const numbers = [10, 20, 30];

const result = numbers.reduce((sum, number) => {
    return sum + number;
}, 0);

console.log(result);
// 60
```

The accumulator starts at `0`, and every array element is processed.

### 🖆 Omitting the Initial Value

```javascript
const numbers = [10, 20, 30];

const result = numbers.reduce((sum, number) => {
    return sum + number;
});

console.log(result);
// 60
```

Without an initial value:

* The first array element becomes the initial accumulator.
* The callback starts processing from the second element.
* The first element is not passed as `currentValue` during the first callback execution.

Execution:

| Iteration | Accumulator | Current Value | Result |
| --- | ---: | ---: | ---: |
| Initial | 10 | - | 10 |
| First | 10 | 20 | 30 |
| Second | 30 | 30 | 60 |

### 🖆 Empty Arrays

An empty array without an initial value causes a `TypeError`.

```javascript
const numbers = [];

const result = numbers.reduce((sum, number) => {
    return sum + number;
});

// TypeError
```

Providing an initial value allows the operation to complete.

```javascript
const numbers = [];

const result = numbers.reduce((sum, number) => {
    return sum + number;
}, 0);

console.log(result);
// 0
```

### 🖆 Single-Element Arrays

When an array contains one element and an initial value is omitted, `reduce()` returns that element without executing the callback.

```javascript
const numbers = [10];

const result = numbers.reduce((sum, number) => {
    console.log("Callback executed");

    return sum + number;
});

console.log(result);
// 10
```

The callback is not executed.

# 🗁 `reduceRight()`

The `reduceRight()` method works like `reduce()`, but processes array elements from right to left.

```javascript
const numbers = [1, 2, 3, 4];

const result = numbers.reduceRight((accumulator, number) => {
    return accumulator - number;
});

console.log(result);
// -2
```

Execution:

```text
4 - 3 = 1
1 - 2 = -1
-1 - 1 = -2
```

The direction matters when the operation is not associative, such as subtraction or string construction.

Another example:

```javascript
const words = ["JavaScript", "is", "awesome"];

const result = words.reduceRight((sentence, word) => {
    return `${sentence} ${word}`;
});

console.log(result);
// "awesome is JavaScript"
```

# 🗁 Important Characteristics

### 🖆 Returns an Accumulated Result

`reduce()` returns the final accumulator value.

The result does not have to be an array.

```javascript
const numbers = [1, 2, 3];

const total = numbers.reduce((sum, number) => {
    return sum + number;
}, 0);

console.log(total);
// 6
```

### 🖆 Does Not Modify the Original Array by Itself

```javascript
const numbers = [1, 2, 3];

const result = numbers.reduce((sum, number) => {
    return sum + number;
}, 0);

console.log(numbers);
// [1, 2, 3]
```

However, a callback can still mutate the accumulator or objects referenced by the original array.

### 🖆 Skips Empty Slots

Like other iterative array methods, `reduce()` does not execute its callback for empty slots in sparse arrays.

```javascript
const numbers = [1, , 3];

const result = numbers.reduce((sum, number) => {
    return sum + number;
}, 0);

console.log(result);
// 4
```

An explicitly assigned `undefined` value is processed normally.

### 🖆 Executes Synchronously

`reduce()` does not automatically wait for promises returned by its callback.

```javascript
const numbers = [1, 2, 3];

const result = numbers.reduce(async (sum, number) => {
    return (await sum) + number;
}, 0);

console.log(await result);
// 6
```

This works because each callback awaits the previous accumulator promise.

For many asynchronous collection-processing tasks, a simple loop can be easier to understand and maintain.

# 🗁 Common Mistakes

### 🖆 Forgetting to Return the Accumulator

Incorrect:

```javascript
const numbers = [1, 2, 3];

const result = numbers.reduce((sum, number) => {
    sum + number;
}, 0);

console.log(result);
// undefined
```

Correct:

```javascript
const result = numbers.reduce((sum, number) => {
    return sum + number;
}, 0);

console.log(result);
// 6
```

### 🖆 Using an Incorrect Initial Value

Incorrect:

```javascript
const numbers = [10, 20, 30];

const result = numbers.reduce((sum, number) => {
    return sum + number;
}, 1);

console.log(result);
// 61
```

The initial value is part of the calculation.

Correct:

```javascript
const result = numbers.reduce((sum, number) => {
    return sum + number;
}, 0);

console.log(result);
// 60
```

### 🖆 Forgetting to Initialize an Array Accumulator

Incorrect:

```javascript
const numbers = [1, 2, 3];

const result = numbers.reduce((accumulator, number) => {
    accumulator.push(number * 2);

    return accumulator;
});

console.log(result);
// TypeError
```

Without an initial value, the first number becomes the accumulator. Numbers do not have an array `push()` method.

Correct:

```javascript
const result = numbers.reduce((accumulator, number) => {
    accumulator.push(number * 2);

    return accumulator;
}, []);

console.log(result);
// [2, 4, 6]
```

### 🖆 Creating Unnecessary Copies

Repeatedly spreading the accumulator can create unnecessary allocations.

```javascript
const numbers = [1, 2, 3, 4];

const result = numbers.reduce((accumulator, number) => {
    return [...accumulator, number * 2];
}, []);
```

For large arrays, this pattern can become inefficient because each iteration copies the accumulated elements.

A more efficient alternative is to mutate a newly created local accumulator:

```javascript
const result = numbers.reduce((accumulator, number) => {
    accumulator.push(number * 2);

    return accumulator;
}, []);
```

This does not mutate the original array.

For a simple transformation, however, `map()` is still the better choice.

# 🗁 `reduce()` vs Other Array Methods

| Method | Purpose | Return Value |
| --- | --- | --- |
| `reduce()` | Accumulate values from left to right | Accumulated result |
| `reduceRight()` | Accumulate values from right to left | Accumulated result |
| `map()` | Transform every element | New array |
| `filter()` | Select matching elements | New array |
| `forEach()` | Execute a callback for each element | `undefined` |
| `find()` | Find the first matching element | Element or `undefined` |
| `flat()` | Flatten nested arrays | New array |

### 🖆 Choosing the Correct Method

Use `reduce()` when:

* You need to calculate a total or aggregate.
* You need to group records.
* You need to transform an array into an object.
* You need to accumulate multiple values into one result.

Use `map()` when every element needs a transformation.

Use `filter()` when selecting matching elements.

Use `find()` when searching for the first matching element.

Use `forEach()` when performing side effects without needing a returned collection.

Do not use `reduce()` simply because it can implement almost every array-processing operation. Prefer the most expressive method for the task.

# 🗁 Performance Considerations

* `reduce()` generally processes an array in \(O(n)\) time when each callback operation is constant-time.
* It can efficiently combine multiple calculations into one traversal.
* Repeatedly copying large accumulators can introduce unnecessary allocation costs.
* Mutating a local accumulator can be efficient when the accumulator is newly created and not shared.
* Avoid unnecessarily complex reduction callbacks.
* For complicated asynchronous workflows, consider `for...of` or other control-flow structures.

# 🗁 Best Practices

* Provide an explicit initial value in most practical cases.
* Use meaningful accumulator names such as `sum`, `total`, `groups`, or `result`.
* Always return the accumulator from the callback.
* Choose an initial value that matches the intended result type.
* Avoid using `reduce()` when `map()`, `filter()`, or `find()` expresses the operation more clearly.
* Avoid unnecessary accumulator copying.
* Keep complex grouping and aggregation logic readable.
* Remember that `reduce()` skips empty slots.
* Use `reduceRight()` when right-to-left processing is required.

# 🗁 Summary

The `reduce()` method processes array elements and combines their results into a single accumulated value.

Key points:

* Executes a callback for each existing array element.
* Maintains an accumulator between callback executions.
* Supports an optional initial value.
* Returns the final accumulated result.
* Can return numbers, strings, objects, arrays, and other values.
* Does not directly modify the original array.
* Throws a `TypeError` for an empty array without an initial value.
* `reduceRight()` performs accumulation from right to left.

**Remember:** `reduce()` takes multiple elements and accumulates them into one result.
