# JavaScript for Loop

A **`for` loop** is a control-flow statement that repeatedly executes a block of code while a specified condition evaluates to `true`.

Loops help developers execute repetitive operations without writing the same code multiple times.

For example, instead of writing five separate statements to display numbers, a `for` loop can handle the task.

```javascript
for (let i = 1; i <= 5; i++) {
    console.log(i);
}
```

**Output**

```text
1
2
3
4
5
```

The `for` loop is commonly used for:

* Iterating over arrays.
* Processing collections of data.
* Repeating calculations.
* Generating sequences.
* Traversing indexes.
* Implementing algorithms.

# 🗁 for Loop Syntax

The `for` loop consists of three expressions:

```javascript
for (initialization; condition; increment) {
    // Code to execute
}
```

| Component | Description |
| --- | --- |
| Initialization | Executes once before the loop starts |
| Condition | Evaluated before every iteration |
| Increment | Executes after each iteration |
| Loop body | Contains statements executed during each iteration |

**Example**

```javascript
for (let i = 1; i <= 5; i++) {
    console.log(i);
}
```

Execution process:

* Initialization: `let i = 1`
* Condition: `i <= 5`
* Loop body: `console.log(i)`
* Increment: `i++`
* Repeat until the condition becomes `false`.

# 🗁 Initialization

Initialization creates or assigns the initial value of the loop variable.

It executes only once before the first iteration.

```javascript
for (let i = 0; i < 3; i++) {
    console.log(i);
}
```

Here, `let i = 0` initializes the loop variable.

The loop variable can also be initialized outside the loop.

```javascript
let i = 0;

for (; i < 3; i++) {
    console.log(i);
}
```

Initialization is optional, but a loop still needs a condition or another mechanism that eventually terminates it.

# 🗁 Condition

The condition determines whether another iteration should execute.

JavaScript evaluates the condition before executing the loop body.

```javascript
for (let i = 1; i <= 3; i++) {
    console.log(i);
}
```

The loop continues while `i <= 3` evaluates to `true`.

When `i` becomes `4`, the condition evaluates to `false`, and the loop terminates.

**Omitting the condition**

When the condition is omitted, JavaScript treats it as `true`.

```javascript
for (let i = 1; ; i++) {
    console.log(i);

    if (i === 3) {
        break;
    }
}
```

Without a terminating condition or an explicit exit mechanism, the loop can run indefinitely.

# 🗁 Increment and Decrement

The increment expression executes after each iteration.

It commonly updates the loop variable.

**Increment operator (`++`)**

```javascript
for (let i = 1; i <= 5; i++) {
    console.log(i);
}
```

**Decrement operator (`--`)**

```javascript
for (let i = 5; i >= 1; i--) {
    console.log(i);
}
```

**Custom increment**

```javascript
for (let i = 0; i <= 20; i += 5) {
    console.log(i);
}
```

**Output**

```text
0
5
10
15
20
```

The increment expression can use any valid expression that updates the loop's state.

# 🗁 Execution Flow

A `for` loop follows a predictable execution order.

Consider this example:

```javascript
for (let i = 1; i <= 3; i++) {
    console.log(`Iteration: ${i}`);
}
```

Execution sequence:

* Initialization executes once: `i = 1`.
* Condition is checked: `1 <= 3`.
* Loop body executes.
* Increment executes: `i = 2`.
* Condition is checked again.
* The process repeats until the condition becomes `false`.

<CodeBlock language="mermaid" editable>
flowchart TD
    A([Start]) --> B[Initialize loop variable]
    B --> C{Condition true?}
    C -->|Yes| D[Execute loop body]
    D --> E[Execute increment]
    E --> C
    C -->|No| F([End])
</CodeBlock>

# 🗁 Iterating Over Arrays

A traditional `for` loop is commonly used to iterate over arrays through their indexes.

```javascript
const fruits = ["Apple", "Banana", "Orange"];

for (let i = 0; i < fruits.length; i++) {
    console.log(fruits[i]);
}
```

**Output**

```text
Apple
Banana
Orange
```

The loop starts at index `0` and continues until the index reaches the array's length.

**Accessing array indexes**

```javascript
const users = ["John", "Alice", "David"];

for (let i = 0; i < users.length; i++) {
    console.log(`Index: ${i}, User: ${users[i]}`);
}
```

**Output**

```text
Index: 0, User: John
Index: 1, User: Alice
Index: 2, User: David
```

**Iterating in reverse**

```javascript
const numbers = [10, 20, 30, 40, 50];

for (let i = numbers.length - 1; i >= 0; i--) {
    console.log(numbers[i]);
}
```

**Output**

```text
50
40
30
20
10
```

# 🗁 Nested for Loops

A nested `for` loop is a `for` loop placed inside another `for` loop.

The inner loop executes completely for every iteration of the outer loop.

```javascript
for (let i = 1; i <= 3; i++) {
    for (let j = 1; j <= 3; j++) {
        console.log(`i = ${i}, j = ${j}`);
    }
}
```

**Output**

```text
i = 1, j = 1
i = 1, j = 2
i = 1, j = 3
i = 2, j = 1
i = 2, j = 2
i = 2, j = 3
i = 3, j = 1
i = 3, j = 2
i = 3, j = 3
```

**Generating a multiplication table**

```javascript
const number = 5;

for (let i = 1; i <= 10; i++) {
    console.log(`${number} x ${i} = ${number * i}`);
}
```

**Generating a multiplication grid**

```javascript
for (let i = 1; i <= 5; i++) {
    for (let j = 1; j <= 5; j++) {
        console.log(`${i} x ${j} = ${i * j}`);
    }
}
```

Nested loops are useful for multidimensional arrays, matrices, tables, and grid-based algorithms.

# 🗁 Multiple Expressions in a for Loop

The initialization and increment sections can contain multiple comma-separated expressions.

```javascript
for (
    let i = 0, j = 10;
    i <= 5;
    i++, j--
) {
    console.log(`i = ${i}, j = ${j}`);
}
```

**Output**

```text
i = 0, j = 10
i = 1, j = 9
i = 2, j = 8
i = 3, j = 7
i = 4, j = 6
i = 5, j = 5
```

The comma operator evaluates expressions from left to right and returns the value of the final expression.

# 🗁 Omitting for Loop Expressions

The three expressions in a `for` loop are optional.

However, semicolons separating the three sections must remain.

**Omitting initialization**

```javascript
let i = 1;

for (; i <= 5; i++) {
    console.log(i);
}
```

**Omitting increment**

```javascript
for (let i = 1; i <= 5;) {
    console.log(i);
    i++;
}
```

**Omitting condition**

```javascript
for (let i = 1; ; i++) {
    console.log(i);

    if (i === 5) {
        break;
    }
}
```

**Omitting all three expressions**

```javascript
let i = 1;

for (;;) {
    console.log(i);

    if (i === 5) {
        break;
    }

    i++;
}
```

`for (;;)` creates an infinite loop unless its body contains an exit mechanism.

# 🗁 The break Statement in for Loops

The `break` statement immediately terminates the nearest enclosing loop.

```javascript
for (let i = 1; i <= 10; i++) {
    if (i === 5) {
        break;
    }

    console.log(i);
}
```

**Output**

```text
1
2
3
4
```

The loop terminates when `i` becomes `5`.

**Using break in nested loops**

An ordinary `break` only terminates the nearest enclosing loop.

```javascript
for (let i = 1; i <= 3; i++) {
    for (let j = 1; j <= 3; j++) {
        if (j === 2) {
            break;
        }

        console.log(`i = ${i}, j = ${j}`);
    }
}
```

To exit multiple nested loops, use a labeled break when appropriate.

```javascript
outerLoop:
for (let i = 1; i <= 3; i++) {
    for (let j = 1; j <= 3; j++) {
        if (i === 2 && j === 2) {
            break outerLoop;
        }

        console.log(`i = ${i}, j = ${j}`);
    }
}
```

# 🗁 The continue Statement in for Loops

The `continue` statement skips the remaining statements in the current iteration and proceeds to the next iteration.

In a traditional `for` loop, the increment expression still executes after `continue`.

```javascript
for (let i = 1; i <= 5; i++) {
    if (i === 3) {
        continue;
    }

    console.log(i);
}
```

**Output**

```text
1
2
4
5
```

**Skipping even numbers**

```javascript
for (let i = 1; i <= 10; i++) {
    if (i % 2 === 0) {
        continue;
    }

    console.log(i);
}
```

**Output**

```text
1
3
5
7
9
```

# 🗁 Labeled for Loops

JavaScript supports labels that identify statements.

A labeled loop allows `break` or `continue` to target a specific enclosing loop.

```javascript
outer:
for (let i = 1; i <= 3; i++) {
    for (let j = 1; j <= 3; j++) {
        if (i === 2 && j === 2) {
            continue outer;
        }

        console.log(`i = ${i}, j = ${j}`);
    }
}
```

When `continue outer` executes, the current inner loop is abandoned and the next iteration of the outer loop begins.

Labels should be used sparingly because excessive use can make control flow harder to follow.

# 🗁 for Loop and Block Scope

A loop variable declared with `let` or `const` has block scope.

```javascript
for (let i = 0; i < 3; i++) {
    console.log(i);
}

// console.log(i); // ReferenceError
```

The variable `i` is unavailable outside the loop.

**Using var**

Variables declared with `var` are not block-scoped.

```javascript
for (var i = 0; i < 3; i++) {
    console.log(i);
}

console.log(i); // 3
```

Prefer `let` for traditional loop counters.

**Closures and loop variables**

`let` creates a new lexical binding for each iteration in a traditional `for` loop.

```javascript
const callbacks = [];

for (let i = 0; i < 3; i++) {
    callbacks.push(() => console.log(i));
}

callbacks.forEach(callback => callback());
```

**Output**

```text
0
1
2
```

With `var`, all callbacks share the same function-scoped binding.

```javascript
const callbacks = [];

for (var i = 0; i < 3; i++) {
    callbacks.push(() => console.log(i));
}

callbacks.forEach(callback => callback());
```

**Output**

```text
3
3
3
```

# 🗁 for Loop vs Other Loops

JavaScript provides several looping constructs.

| Loop | Typical Use |
| --- | --- |
| `for` | Iteration with explicit initialization, condition, and update |
| `while` | Repetition while a condition remains true |
| `do...while` | Repetition with at least one guaranteed execution |
| `for...of` | Iterating over iterable values |
| `for...in` | Iterating over enumerable property keys |

**Traditional for loop**

```javascript
const numbers = [10, 20, 30];

for (let i = 0; i < numbers.length; i++) {
    console.log(numbers[i]);
}
```

**for...of loop**

```javascript
const numbers = [10, 20, 30];

for (const number of numbers) {
    console.log(number);
}
```

Use a traditional `for` loop when you need direct control over the index, iteration direction, or update logic.

Use `for...of` when you only need the values of an iterable.

# 🗁 Performance and Complexity

Understanding loop complexity helps developers write efficient algorithms.

**Single loop**

```javascript
for (let i = 0; i < n; i++) {
    console.log(i);
}
```

Time complexity: `O(n)`.

The loop executes approximately `n` times.

**Nested loops**

```javascript
for (let i = 0; i < n; i++) {
    for (let j = 0; j < n; j++) {
        console.log(i, j);
    }
}
```

Time complexity: `O(n²)`.

The inner loop executes `n` times for each of the `n` outer iterations.

**Independent sequential loops**

```javascript
for (let i = 0; i < n; i++) {
    console.log(i);
}

for (let j = 0; j < n; j++) {
    console.log(j);
}
```

Time complexity: `O(n)`.

The total number of iterations is approximately `2n`, and constant factors are ignored in Big O notation.

**Avoid recalculating expensive expressions unnecessarily**

```javascript
for (let i = 0; i < users.length; i++) {
    console.log(users[i]);
}
```

For ordinary arrays, reading `.length` is inexpensive. However, caching an expensive or changing calculation can be useful when appropriate.

```javascript
const length = users.length;

for (let i = 0; i < length; i++) {
    console.log(users[i]);
}
```

Choose optimizations based on actual performance requirements rather than assuming every small difference matters.

# 🗁 Practical Examples

**Calculate the sum of numbers**

```javascript
const numbers = [10, 20, 30, 40, 50];

let sum = 0;

for (let i = 0; i < numbers.length; i++) {
    sum += numbers[i];
}

console.log(sum); // 150
```

**Calculate the average**

```javascript
const numbers = [10, 20, 30, 40, 50];

let sum = 0;

for (let i = 0; i < numbers.length; i++) {
    sum += numbers[i];
}

const average = numbers.length > 0
    ? sum / numbers.length
    : 0;

console.log(average); // 30
```

**Find the largest number**

```javascript
const numbers = [12, 45, 7, 89, 23];

let largest = numbers[0];

for (let i = 1; i < numbers.length; i++) {
    if (numbers[i] > largest) {
        largest = numbers[i];
    }
}

console.log(largest); // 89
```

**Find the smallest number**

```javascript
const numbers = [12, 45, 7, 89, 23];

let smallest = numbers[0];

for (let i = 1; i < numbers.length; i++) {
    if (numbers[i] < smallest) {
        smallest = numbers[i];
    }
}

console.log(smallest); // 7
```

**Count even and odd numbers**

```javascript
const numbers = [1, 2, 3, 4, 5, 6, 7, 8];

let evenCount = 0;
let oddCount = 0;

for (let i = 0; i < numbers.length; i++) {
    if (numbers[i] % 2 === 0) {
        evenCount++;
    } else {
        oddCount++;
    }
}

console.log("Even:", evenCount); // 4
console.log("Odd:", oddCount);   // 4
```

**Reverse an array without using reverse()**

```javascript
const numbers = [1, 2, 3, 4, 5];

const reversed = [];

for (let i = numbers.length - 1; i >= 0; i--) {
    reversed.push(numbers[i]);
}

console.log(reversed); // [5, 4, 3, 2, 1]
```

**Create a multiplication table**

```javascript
const number = 7;

for (let i = 1; i <= 10; i++) {
    console.log(`${number} x ${i} = ${number * i}`);
}
```

**Generate a number sequence**

```javascript
const sequence = [];

for (let i = 1; i <= 10; i++) {
    sequence.push(i * 2);
}

console.log(sequence);
// [2, 4, 6, 8, 10, 12, 14, 16, 18, 20]
```

**Iterate over a two-dimensional array**

```javascript
const matrix = [
    [1, 2, 3],
    [4, 5, 6],
    [7, 8, 9]
];

for (let i = 0; i < matrix.length; i++) {
    for (let j = 0; j < matrix[i].length; j++) {
        console.log(matrix[i][j]);
    }
}
```

**Find a value in an array**

```javascript
const users = ["John", "Alice", "David"];

const target = "Alice";

let foundIndex = -1;

for (let i = 0; i < users.length; i++) {
    if (users[i] === target) {
        foundIndex = i;
        break;
    }
}

console.log(foundIndex); // 1
```

# 🗁 Common Mistakes

**Using an incorrect loop condition**

```javascript
for (let i = 0; i <= 5; i++) {
    console.log(i);
}
```

This executes six times because the condition includes `5`.

Use `<` when you want exactly five iterations starting from zero.

```javascript
for (let i = 0; i < 5; i++) {
    console.log(i);
}
```

**Creating an infinite loop**

```javascript
for (let i = 0; i < 5; ) {
    console.log(i);

    // Missing increment
}
```

The condition never becomes false because `i` is never updated.

**Updating the counter in the wrong direction**

```javascript
for (let i = 5; i >= 1; i++) {
    console.log(i);
}
```

The counter increases instead of decreasing, so the condition remains true.

Use `i--` for a descending loop.

**Using the wrong array boundary**

```javascript
const numbers = [10, 20, 30];

for (let i = 0; i <= numbers.length; i++) {
    console.log(numbers[i]);
}
```

The final iteration accesses an invalid index and produces `undefined`.

Use `i < numbers.length`.

**Accidentally skipping iterations**

```javascript
for (let i = 0; i < 5; i++) {
    if (i === 2) {
        continue;
    }

    i++;
    console.log(i);
}
```

Modifying the loop counter inside the body in addition to the increment expression can cause unexpected iteration behavior.

Keep counter updates in one predictable location whenever possible.

# 🗁 Best Practices

* Use `let` for traditional loop counters.
* Use `const` for values that are not reassigned inside the loop.
* Use `< array.length` when iterating through array indexes.
* Use `break` to terminate a loop when a result has been found.
* Use `continue` to skip iterations when appropriate.
* Avoid unnecessary nested loops.
* Avoid modifying the loop counter in multiple places.
* Make loop termination conditions clear.
* Use `for...of` when indexes are not required.
* Use labeled statements sparingly.
* Avoid infinite loops unless they are intentional and have a clear exit mechanism.
* Consider time complexity when processing large datasets.

# 🗁 Key Takeaways

* A `for` loop repeatedly executes code while its condition is truthy.
* Initialization executes once before the first iteration.
* The condition is checked before each iteration.
* The increment expression executes after each iteration.
* Traditional `for` loops provide direct control over iteration.
* Nested loops are useful for multidimensional data.
* `break` terminates the nearest enclosing loop.
* `continue` skips the remaining statements in the current iteration.
* Labels allow control flow to target an outer loop.
* `let` provides block-scoped loop variables.
* A single loop commonly has `O(n)` time complexity, while a nested loop may have `O(n²)` complexity.
* Traditional `for` loops are especially useful when indexes and iteration direction matter.
