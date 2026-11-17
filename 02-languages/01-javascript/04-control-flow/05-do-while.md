# JavaScript do...while Loop

A **`do...while` loop** is a control-flow statement that executes a block of code at least once and then repeats it as long as a specified condition evaluates to `true`.

Unlike `for` and `while` loops, a `do...while` loop evaluates its condition after executing the loop body.

This means the loop body always executes at least once, even when the condition is initially `false`.

**Example**

```javascript
let count = 1;

do {
    console.log(count);
    count++;
} while (count <= 5);
```

**Output**

```text
1
2
3
4
5
```

Common use cases include:

* Menu-driven applications.
* Input validation.
* Interactive command-line programs.
* Repeating operations until a user chooses to exit.
* Executing an operation before checking its continuation condition.

# 🗁 do...while Syntax

The basic syntax is:

```javascript
do {
    // Code to execute
} while (condition);
```

| Component | Description |
| --- | --- |
| `do` | Starts the loop |
| Loop body | Contains statements executed at least once |
| `while` | Introduces the continuation condition |
| `condition` | Determines whether another iteration should execute |

**Example**

```javascript
let number = 1;

do {
    console.log(number);
    number++;
} while (number <= 3);
```

**Output**

```text
1
2
3
```

Notice that a semicolon is required after the `while (condition)` expression.

# 🗁 How a do...while Loop Works

A `do...while` loop follows this execution sequence:

* Enter the loop body.
* Execute the statements.
* Update the relevant variables or state.
* Evaluate the condition.
* If the condition is `true`, execute the loop body again.
* If the condition is `false`, terminate the loop.

<CodeBlock language="mermaid" editable>
flowchart TD
    A([Start]) --> B[Execute loop body]
    B --> C[Update state]
    C --> D{Condition true?}
    D -->|Yes| B
    D -->|No| E([End])
</CodeBlock>

**Execution example**

```javascript
let count = 1;

do {
    console.log(count);
    count++;
} while (count <= 3);
```

Execution trace:

| Iteration | Value before execution | Output | Condition after update |
| --- | ---: | ---: | --- |
| 1 | 1 | 1 | `2 <= 3` → true |
| 2 | 2 | 2 | `3 <= 3` → true |
| 3 | 3 | 3 | `4 <= 3` → false |

The loop terminates after the third iteration.

# 🗁 Guaranteed First Execution

The most important characteristic of a `do...while` loop is that its body executes before the condition is evaluated.

**Example with a true condition**

```javascript
let number = 1;

do {
    console.log(number);
    number++;
} while (number <= 3);
```

**Output**

```text
1
2
3
```

**Example with a false condition**

```javascript
let number = 10;

do {
    console.log(number);
} while (number < 5);
```

**Output**

```text
10
```

Even though `number < 5` is initially false, the loop body executes once.

**Comparison with while**

```javascript
let number = 10;

while (number < 5) {
    console.log(number);
}
```

**Output:** Nothing.

The `while` loop checks its condition before executing its body.

# 🗁 Initialization

Initialization establishes the initial state before the loop begins.

It is generally performed outside the `do...while` statement.

```javascript
let count = 1;

do {
    console.log(count);
    count++;
} while (count <= 5);
```

Multiple variables can also be initialized.

```javascript
let current = 1;
let total = 0;

do {
    total += current;
    current++;
} while (current <= 5);

console.log(total); // 15
```

# 🗁 Condition

The condition determines whether another iteration should execute.

It is evaluated after the loop body finishes.

```javascript
let temperature = 30;

do {
    console.log(`Temperature: ${temperature}`);
    temperature--;
} while (temperature > 25);
```

**Output**

```text
Temperature: 30
Temperature: 29
Temperature: 28
Temperature: 27
Temperature: 26
```

The condition can contain:

* Comparison operators.
* Logical operators.
* Boolean variables.
* Function calls.
* Expressions that produce truthy or falsy values.

**Using a Boolean condition**

```javascript
let isRunning = true;
let count = 0;

do {
    console.log(count);

    count++;

    if (count === 3) {
        isRunning = false;
    }
} while (isRunning);
```

**Output**

```text
0
1
2
```

# 🗁 Updating Loop State

A `do...while` loop usually requires an update operation inside its body.

The update changes the state used by the continuation condition.

**Increment**

```javascript
let count = 1;

do {
    console.log(count);
    count++;
} while (count <= 5);
```

**Decrement**

```javascript
let count = 5;

do {
    console.log(count);
    count--;
} while (count >= 1);
```

**Custom increment**

```javascript
let number = 0;

do {
    console.log(number);
    number += 5;
} while (number <= 20);
```

**Output**

```text
0
5
10
15
20
```

# 🗁 The break Statement

The `break` statement immediately terminates the nearest enclosing loop.

```javascript
let number = 1;

do {
    if (number === 4) {
        break;
    }

    console.log(number);
    number++;
} while (number <= 10);
```

**Output**

```text
1
2
3
```

The loop terminates when `number` reaches `4`.

**Finding an element**

```javascript
const users = ["John", "Alice", "David", "Sarah"];

const target = "David";

let index = 0;
let found = false;

do {
    if (users[index] === target) {
        found = true;
        break;
    }

    index++;
} while (index < users.length);

console.log(found); // true
```

The loop searches until the target is found or all array elements have been checked.

# 🗁 The continue Statement

The `continue` statement skips the remaining statements in the current iteration and proceeds to the condition check.

```javascript
let number = 0;

do {
    number++;

    if (number === 3) {
        continue;
    }

    console.log(number);
} while (number < 5);
```

**Output**

```text
1
2
4
5
```

An important difference exists between `continue` in a `do...while` loop and a traditional `for` loop:

* In a `for` loop, `continue` proceeds to the update expression.
* In a `do...while` loop, `continue` proceeds directly to the condition evaluation.

Therefore, ensure that necessary state updates occur before `continue` when they are required for termination.

**Incorrect implementation**

```javascript
let number = 0;

do {
    if (number === 3) {
        continue;
    }

    number++;
} while (number < 5);
```

When `number` reaches `3`, the loop repeatedly evaluates the condition without updating `number`.

**Correct implementation**

```javascript
let number = 0;

do {
    number++;

    if (number === 3) {
        continue;
    }

    console.log(number);
} while (number < 5);
```

# 🗁 Nested do...while Loops

A nested `do...while` loop is a `do...while` loop inside another loop.

The inner loop executes completely for each iteration of the outer loop unless its execution is interrupted.

```javascript
let i = 1;

do {
    let j = 1;

    do {
        console.log(`i = ${i}, j = ${j}`);
        j++;
    } while (j <= 3);

    i++;
} while (i <= 3);
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

Each loop needs its own correctly managed state.

# 🗁 Labeled do...while Loops

JavaScript supports labels that identify statements.

A labeled `break` terminates the specified enclosing loop.

```javascript
let row = 1;

outer:
do {
    let column = 1;

    do {
        console.log(`row = ${row}, column = ${column}`);

        if (row === 2 && column === 2) {
            break outer;
        }

        column++;
    } while (column <= 3);

    row++;
} while (row <= 3);
```

**Output**

```text
row = 1, column = 1
row = 1, column = 2
row = 1, column = 3
row = 2, column = 1
row = 2, column = 2
```

A labeled `continue` can also skip the current iteration of an outer loop.

```javascript
let row = 0;

outer:
do {
    row++;

    let column = 0;

    do {
        column++;

        if (column === 2) {
            continue outer;
        }

        console.log(`row = ${row}, column = ${column}`);
    } while (column < 3);
} while (row < 3);
```

Use labels sparingly to keep control flow readable.

# 🗁 do...while with User Input

A `do...while` loop is useful when an operation must execute at least once before checking whether it should repeat.

For example, a command-line menu can display its options before asking whether the application should continue.

**Menu-driven application**

```javascript
let choice;

do {
    console.log("1. View Profile");
    console.log("2. Update Profile");
    console.log("3. Exit");

    choice = 3;

    console.log(`Selected: ${choice}`);
} while (choice !== 3);
```

This example uses a fixed choice to demonstrate the loop structure.

In a real command-line application, `choice` would be obtained from an input mechanism such as Node.js `readline`.

**Input validation**

```javascript
let age;

do {
    age = Number(prompt("Enter your age:"));
} while (!Number.isFinite(age) || age < 0);

console.log(`Your age is ${age}`);
```

This browser example keeps requesting input until the user provides a finite, non-negative number.

# 🗁 do...while with Arrays

A `do...while` loop can iterate over an array using indexes.

```javascript
const fruits = ["Apple", "Banana", "Orange"];

let index = 0;

do {
    console.log(fruits[index]);
    index++;
} while (index < fruits.length);
```

**Output**

```text
Apple
Banana
Orange
```

Remember that this loop executes at least once.

For an empty array, the example above attempts to access index `0` and produces `undefined`.

A guard can prevent that behavior.

```javascript
const fruits = [];

let index = 0;

if (fruits.length > 0) {
    do {
        console.log(fruits[index]);
        index++;
    } while (index < fruits.length);
}
```

# 🗁 while vs do...while vs for

JavaScript provides different looping constructs for different control-flow requirements.

| Feature | for | while | do...while |
| --- | --- | --- | --- |
| Condition checked | Before iteration | Before iteration | After iteration |
| Guaranteed execution | No | No | Yes |
| Initialization | Usually in loop header | Usually outside | Usually outside |
| Update | Usually in loop header | Usually in body | Usually in body |
| Typical use | Counter-based iteration | Condition-based iteration | At-least-once execution |

**for loop**

```javascript
for (let i = 1; i <= 3; i++) {
    console.log(i);
}
```

**while loop**

```javascript
let i = 1;

while (i <= 3) {
    console.log(i);
    i++;
}
```

**do...while loop**

```javascript
let i = 1;

do {
    console.log(i);
    i++;
} while (i <= 3);
```

All three examples produce the same output, but their execution order differs.

# 🗁 Performance and Complexity

The time complexity of a `do...while` loop depends on the number of iterations and the operations performed during each iteration.

**Linear iteration**

```javascript
let i = 0;

do {
    i++;
} while (i < n);
```

Time complexity: `O(n)` for positive `n`.

**Logarithmic iteration**

```javascript
let number = n;

if (number > 0) {
    do {
        number = Math.floor(number / 2);
    } while (number > 1);
}
```

Time complexity: `O(log n)` for positive integer `n`.

**Nested iteration**

```javascript
let i = 0;

do {
    let j = 0;

    do {
        j++;
    } while (j < n);

    i++;
} while (i < n);
```

Time complexity: `O(n²)` for positive `n`.

The loop type does not determine algorithmic complexity. Iteration count and per-iteration work determine it.

# 🗁 Practical Examples

**Calculate the sum from 1 to N**

```javascript
const n = 10;

let number = 1;
let sum = 0;

do {
    sum += number;
    number++;
} while (number <= n);

console.log(sum); // 55
```

**Calculate a factorial**

```javascript
const n = 5;

let factorial = 1;
let number = 1;

do {
    factorial *= number;
    number++;
} while (number <= n);

console.log(factorial); // 120
```

For this implementation, use a separate validation step if `n` can be negative or non-integer.

**Count digits in a number**

```javascript
function countDigits(value) {
    let number = Math.abs(Math.trunc(value));
    let count = 0;

    do {
        number = Math.floor(number / 10);
        count++;
    } while (number > 0);

    return count;
}

console.log(countDigits(12345)); // 5
console.log(countDigits(0));     // 1
console.log(countDigits(-123));  // 3
```

**Reverse a positive integer**

```javascript
let number = 12345;
let reversed = 0;

do {
    const digit = number % 10;

    reversed = reversed * 10 + digit;

    number = Math.floor(number / 10);
} while (number > 0);

console.log(reversed); // 54321
```

This implementation assumes a positive integer.

**Generate a Fibonacci sequence**

```javascript
const limit = 10;

let first = 0;
let second = 1;
let count = 0;

if (limit > 0) {
    do {
        console.log(first);

        const next = first + second;

        first = second;
        second = next;

        count++;
    } while (count < limit);
}
```

**Output**

```text
0
1
1
2
3
5
8
13
21
34
```

**Repeat an operation until a target is reached**

```javascript
let balance = 0;
const target = 100;

do {
    balance += 25;

    console.log(`Current balance: ${balance}`);
} while (balance < target);
```

**Output**

```text
Current balance: 25
Current balance: 50
Current balance: 75
Current balance: 100
```

# 🗁 Common Mistakes

**Forgetting the trailing semicolon**

```javascript
let count = 1;

do {
    console.log(count);
    count++;
} while (count <= 5);
```

The semicolon after the `while` condition is part of the required syntax.

**Assuming the condition is checked first**

```javascript
let number = 10;

do {
    console.log(number);
} while (number < 5);
```

The body executes once despite the false condition.

**Forgetting to update loop state**

```javascript
let count = 1;

do {
    console.log(count);
} while (count <= 5);
```

The condition never changes, so the loop continues indefinitely.

**Incorrect continue placement**

```javascript
let count = 0;

do {
    if (count === 3) {
        continue;
    }

    count++;
} while (count < 5);
```

When `count` reaches `3`, the update is skipped and the loop cannot terminate.

**Accessing an empty array**

```javascript
const items = [];

let index = 0;

do {
    console.log(items[index]);
    index++;
} while (index < items.length);
```

The body executes once and logs `undefined`.

Check the array length before entering the loop when an empty collection is possible.

# 🗁 Best Practices

* Use `do...while` when the loop body must execute at least once.
* Keep the continuation condition clear.
* Update the relevant state inside the loop body.
* Place necessary updates before `continue`.
* Include the required trailing semicolon.
* Validate user input before using it in calculations.
* Check collection boundaries when processing arrays.
* Use `break` when a result or termination condition is reached.
* Avoid unnecessary nested loops.
* Prefer `while` when the body should not execute if the initial condition is false.
* Prefer `for` for predictable counter-based iteration.

# 🗁 Key Takeaways

* A `do...while` loop executes its body before evaluating the condition.
* Its body always executes at least once.
* The condition determines whether another iteration occurs.
* Initialization and updates are generally handled inside or outside the loop body.
* The trailing semicolon after `while (condition)` is required.
* `break` terminates the nearest enclosing loop.
* `continue` proceeds directly to the condition check.
* Nested and labeled loops support more complex control flow.
* `do...while` is particularly useful for menus and input validation.
* Its time complexity depends on the number of iterations and the work performed inside the loop.
