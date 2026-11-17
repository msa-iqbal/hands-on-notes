# JavaScript continue Statement

The **`continue` statement** is a control-flow statement that skips the remaining statements in the current loop iteration and proceeds to the next iteration.

Unlike `break`, which terminates an entire loop, `continue` allows the loop to continue executing subsequent iterations.

It is commonly used when:

* Certain values should be skipped.
* Only specific records should be processed.
* Invalid data should be ignored.
* A condition determines whether the current iteration should continue.
* Unwanted elements should be excluded from a calculation.

**Example**

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

When `i` becomes `3`, JavaScript skips `console.log(i)` and proceeds to the next iteration.

# 🗁 continue Syntax

The basic syntax is:

```javascript
continue;
```

The `continue` statement can be used inside:

* `for` loops.
* `while` loops.
* `do...while` loops.
* `for...of` loops.
* `for...in` loops.

It cannot be used arbitrarily outside a loop.

**Example**

```javascript
let count = 0;

while (count < 5) {
    count++;

    if (count === 3) {
        continue;
    }

    console.log(count);
}
```

**Output**

```text
1
2
4
5
```

# 🗁 How continue Works

When JavaScript encounters `continue`:

* The remaining statements in the current iteration are skipped.
* Control moves to the loop's continuation point.
* The next condition check or iteration begins.
* The loop terminates normally when its condition becomes false.

<CodeBlock language="mermaid" editable>
flowchart TD
    A([Start]) --> B[Execute loop body]
    B --> C{Continue condition?}
    C -->|Yes| D[Skip remaining statements]
    D --> E[Proceed to next iteration]
    E --> B
    C -->|No| F[Execute remaining statements]
    F --> G[Proceed to next iteration]
    G --> B
</CodeBlock>

The exact continuation point depends on the loop type.

# 🗁 continue in for Loops

In a traditional `for` loop, `continue` skips the remaining loop body and proceeds to the update expression.

After the update expression executes, JavaScript evaluates the condition again.

**Example**

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

Execution flow:

| Iteration | Condition | continue? | Output | Update |
| --- | --- | --- | --- | --- |
| 1 | `1 <= 5` | No | 1 | `i++` |
| 2 | `2 <= 5` | No | 2 | `i++` |
| 3 | `3 <= 5` | Yes | Skipped | `i++` |
| 4 | `4 <= 5` | No | 4 | `i++` |
| 5 | `5 <= 5` | No | 5 | `i++` |

The increment expression still executes when `continue` is encountered.

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

**Skipping a range**

```javascript
for (let i = 1; i <= 10; i++) {
    if (i >= 4 && i <= 7) {
        continue;
    }

    console.log(i);
}
```

**Output**

```text
1
2
3
8
9
10
```

# 🗁 continue in while Loops

In a `while` loop, `continue` skips the remaining body and proceeds directly to the condition check.

Unlike a traditional `for` loop, a `while` loop has no automatic update expression.

Therefore, update the relevant state before executing `continue` when that update is necessary for termination.

**Example**

```javascript
let i = 0;

while (i < 5) {
    i++;

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

**Skipping odd numbers**

```javascript
let number = 0;

while (number < 10) {
    number++;

    if (number % 2 !== 0) {
        continue;
    }

    console.log(number);
}
```

**Output**

```text
2
4
6
8
10
```

**Important:** If the counter update occurs after `continue`, it may never execute for the skipped iteration.

Incorrect:

```javascript
let i = 0;

while (i < 5) {
    if (i === 3) {
        continue;
    }

    i++;
    console.log(i);
}
```

When `i` reaches `3`, the loop repeatedly executes `continue` without changing `i`.

Correct:

```javascript
let i = 0;

while (i < 5) {
    i++;

    if (i === 3) {
        continue;
    }

    console.log(i);
}
```

# 🗁 continue in do...while Loops

In a `do...while` loop, `continue` skips the remaining loop body and proceeds directly to the condition evaluation.

**Example**

```javascript
let i = 0;

do {
    i++;

    if (i === 3) {
        continue;
    }

    console.log(i);
} while (i < 5);
```

**Output**

```text
1
2
4
5
```

The condition is evaluated after `continue`.

Make sure required state updates happen before the statement when necessary.

# 🗁 continue in for...of Loops

A `for...of` loop iterates over the values of an iterable.

The `continue` statement skips processing the current value and moves to the next value.

**Example**

```javascript
const fruits = ["Apple", "Banana", "Orange", "Mango"];

for (const fruit of fruits) {
    if (fruit === "Orange") {
        continue;
    }

    console.log(fruit);
}
```

**Output**

```text
Apple
Banana
Mango
```

**Skipping empty values**

```javascript
const values = [10, null, 20, undefined, 30];

for (const value of values) {
    if (value == null) {
        continue;
    }

    console.log(value);
}
```

**Output**

```text
10
20
30
```

The loose equality comparison `value == null` matches both `null` and `undefined`.

# 🗁 continue in for...in Loops

A `for...in` loop iterates over enumerable property keys.

The `continue` statement skips processing the current property.

**Example**

```javascript
const user = {
    name: "John",
    age: 25,
    password: "secret",
    city: "London"
};

for (const key in user) {
    if (key === "password") {
        continue;
    }

    console.log(key, user[key]);
}
```

**Output**

```text
name John
age 25
city London
```

The `password` property is skipped.

For objects, use `Object.hasOwn()` when you specifically want to process only the object's own enumerable properties.

```javascript
for (const key in user) {
    if (!Object.hasOwn(user, key)) {
        continue;
    }

    if (key === "password") {
        continue;
    }

    console.log(key, user[key]);
}
```

# 🗁 continue in Nested Loops

In nested loops, an ordinary `continue` affects only the nearest enclosing loop.

The outer loop continues normally.

**Example**

```javascript
for (let i = 1; i <= 3; i++) {
    for (let j = 1; j <= 3; j++) {
        if (j === 2) {
            continue;
        }

        console.log(`i = ${i}, j = ${j}`);
    }
}
```

**Output**

```text
i = 1, j = 1
i = 1, j = 3
i = 2, j = 1
i = 2, j = 3
i = 3, j = 1
i = 3, j = 3
```

The inner loop skips its second iteration for every outer iteration.

# 🗁 Labeled continue

JavaScript supports labeled `continue` statements that allow a loop to skip the current iteration of a specified enclosing labeled loop.

This is useful when nested loops need to continue an outer loop directly.

**Syntax**

```javascript
labelName:
    loopStatement;
```

To continue the labeled loop:

```javascript
continue labelName;
```

**Example**

```javascript
outerLoop:
for (let i = 1; i <= 3; i++) {
    for (let j = 1; j <= 3; j++) {
        if (j === 2) {
            continue outerLoop;
        }

        console.log(`i = ${i}, j = ${j}`);
    }
}
```

**Output**

```text
i = 1, j = 1
i = 2, j = 1
i = 3, j = 1
```

When `j` becomes `2`, the current inner loop is abandoned and the next iteration of the outer loop begins.

A labeled `continue` must target an enclosing iteration statement, such as a `for`, `while`, or `do...while` loop. It cannot target an ordinary labeled block.

# 🗁 Practical Examples

**Print only even numbers**

```javascript
const numbers = [1, 2, 3, 4, 5, 6, 7, 8];

for (const number of numbers) {
    if (number % 2 !== 0) {
        continue;
    }

    console.log(number);
}
```

**Output**

```text
2
4
6
8
```

**Calculate the sum of positive numbers**

```javascript
const numbers = [10, -5, 20, -3, 30, 0];

let sum = 0;

for (const number of numbers) {
    if (number <= 0) {
        continue;
    }

    sum += number;
}

console.log(sum); // 60
```

**Filter valid user records**

```javascript
const users = [
    { name: "John", active: true },
    { name: "Alice", active: false },
    { name: "David", active: true },
    { name: "Sarah", active: false }
];

for (const user of users) {
    if (!user.active) {
        continue;
    }

    console.log(user.name);
}
```

**Output**

```text
John
David
```

**Skip invalid transactions**

```javascript
const transactions = [
    { id: 1, amount: 100 },
    { id: 2, amount: -50 },
    { id: 3, amount: 200 },
    { id: 4, amount: 0 },
    { id: 5, amount: 300 }
];

let total = 0;

for (const transaction of transactions) {
    if (transaction.amount <= 0) {
        continue;
    }

    total += transaction.amount;
}

console.log(total); // 600
```

**Generate multiplication results while skipping multiples of three**

```javascript
for (let i = 1; i <= 10; i++) {
    if (i % 3 === 0) {
        continue;
    }

    console.log(`5 x ${i} = ${5 * i}`);
}
```

**Output**

```text
5 x 1 = 5
5 x 2 = 10
5 x 4 = 20
5 x 5 = 25
5 x 7 = 35
5 x 8 = 40
5 x 10 = 50
```

**Process a two-dimensional array while skipping a row**

```javascript
const matrix = [
    [1, 2, 3],
    [4, 5, 6],
    [7, 8, 9]
];

for (let i = 0; i < matrix.length; i++) {
    if (i === 1) {
        continue;
    }

    for (let j = 0; j < matrix[i].length; j++) {
        console.log(matrix[i][j]);
    }
}
```

**Output**

```text
1
2
3
7
8
9
```

The second row is skipped entirely.

# 🗁 continue vs break

Both statements change normal loop execution, but they have different purposes.

| Feature | continue | break |
| --- | --- | --- |
| Behavior | Skips the current iteration | Terminates the loop |
| Remaining loop iterations | Execute normally | Do not execute |
| Current iteration | Abandoned | Terminated with the loop |
| Common use | Skip unwanted values | Stop after finding a result |

**Using continue**

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

**Using break**

```javascript
for (let i = 1; i <= 5; i++) {
    if (i === 3) {
        break;
    }

    console.log(i);
}
```

**Output**

```text
1
2
```

Use `continue` when the loop should keep running but the current iteration should be skipped.

Use `break` when the loop should stop entirely.

# 🗁 continue vs Conditional Statements

Sometimes a conditional statement can replace `continue`.

**Using continue**

```javascript
for (let i = 1; i <= 5; i++) {
    if (i === 3) {
        continue;
    }

    console.log(i);
}
```

**Using an if statement**

```javascript
for (let i = 1; i <= 5; i++) {
    if (i !== 3) {
        console.log(i);
    }
}
```

Both produce the same output.

The choice depends on readability and the complexity of the remaining loop body.

When multiple statements need to be skipped, `continue` can make the intended control flow clearer.

# 🗁 Common Mistakes

**Using continue outside a loop**

```javascript
if (true) {
    continue;
}
```

This produces a `SyntaxError` because `continue` must be inside an iteration statement.

**Forgetting the update in a while loop**

```javascript
let i = 0;

while (i < 5) {
    if (i === 2) {
        continue;
    }

    i++;
}
```

When `i` reaches `2`, the update is skipped and the loop becomes infinite.

**Assuming continue terminates the loop**

```javascript
for (let i = 1; i <= 5; i++) {
    if (i === 3) {
        continue;
    }

    console.log(i);
}
```

The loop does not stop at `3`. It skips that iteration and continues.

**Using continue when break is required**

```javascript
for (let i = 1; i <= 10; i++) {
    if (i === 5) {
        continue;
    }

    console.log(i);
}
```

This skips `5` but continues through `10`.

If the intention is to stop at `5`, use `break`.

**Using labeled continue unnecessarily**

```javascript
outer:
for (let i = 0; i < 5; i++) {
    if (i === 3) {
        continue outer;
    }
}
```

The label is unnecessary because an ordinary `continue` already targets the nearest loop.

# 🗁 Best Practices

* Use `continue` to skip unwanted iterations.
* Keep skip conditions simple and readable.
* Understand how each loop type handles `continue`.
* Update loop variables before `continue` in `while` and `do...while` loops when necessary.
* Avoid deeply nested labeled control flow.
* Prefer `for...of` when processing iterable values without needing indexes.
* Use conditional statements when they make the code easier to understand.
* Do not use `continue` when the intended behavior is to terminate the entire loop.
* Test boundary conditions to ensure skipped iterations do not create infinite loops.

# 🗁 Key Takeaways

* `continue` skips the remaining statements in the current loop iteration.
* It does not terminate the entire loop.
* It works with `for`, `while`, `do...while`, `for...of`, and `for...in`.
* In a traditional `for` loop, the update expression executes after `continue`.
* In `while` and `do...while` loops, control proceeds directly to the condition check.
* In nested loops, ordinary `continue` affects only the nearest enclosing loop.
* Labeled `continue` can target an enclosing loop.
* It is useful for filtering, skipping invalid data, and controlling iteration.
* Use `break` when the entire loop needs to terminate.
