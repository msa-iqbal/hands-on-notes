# JavaScript while Loop

A **`while` loop** is a control-flow statement that repeatedly executes a block of code as long as a specified condition evaluates to `true`.

Unlike a traditional `for` loop, a `while` loop does not have dedicated initialization, condition, and increment sections.

It is particularly useful when the number of iterations is unknown in advance and depends on a changing condition.

**Example**

```javascript
let count = 1;

while (count <= 5) {
    console.log(count);
    count++;
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

Common use cases include:

* Repeating operations until a condition changes.
* Processing data while items remain available.
* Waiting for a particular state.
* Implementing search algorithms.
* Reading input until a termination condition is reached.
* Performing calculations until a threshold is reached.

# 🗁 while Loop Syntax

The basic syntax is:

```javascript
while (condition) {
    // Code to execute
}
```

| Component | Description |
| --- | --- |
| `while` | Declares a while loop |
| `condition` | Determines whether another iteration should execute |
| Loop body | Contains statements executed while the condition is truthy |

The condition is evaluated before every iteration.

If the condition is initially `false`, the loop body never executes.

**Example**

```javascript
let age = 18;

while (age < 21) {
    console.log(`Age: ${age}`);
    age++;
}
```

**Output**

```text
Age: 18
Age: 19
Age: 20
```

# 🗁 How a while Loop Works

A `while` loop follows this execution sequence:

* Evaluate the condition.
* If the condition is `true`, execute the loop body.
* Update the relevant variables or state.
* Evaluate the condition again.
* Repeat until the condition becomes `false`.
* Exit the loop.

<CodeBlock language="mermaid" editable>
flowchart TD
    A([Start]) --> B{Condition true?}
    B -->|Yes| C[Execute loop body]
    C --> D[Update state]
    D --> B
    B -->|No| E([End])
</CodeBlock>

**Example**

```javascript
let number = 1;

while (number <= 3) {
    console.log(number);
    number++;
}
```

Execution trace:

| Iteration | Condition | Output | Updated value |
| --- | --- | --- | --- |
| 1 | `1 <= 3` | `1` | `2` |
| 2 | `2 <= 3` | `2` | `3` |
| 3 | `3 <= 3` | `3` | `4` |
| 4 | `4 <= 3` | Loop terminates | `4` |

# 🗁 Initialization

Initialization establishes the initial state before the loop begins.

Unlike a `for` loop, initialization is written separately.

```javascript
let i = 0;

while (i < 5) {
    console.log(i);
    i++;
}
```

Here, `let i = 0` initializes the loop counter.

The initialization can involve multiple variables or more complex state.

```javascript
let current = 1;
let total = 0;

while (current <= 5) {
    total += current;
    current++;
}

console.log(total); // 15
```

# 🗁 Condition

The condition determines whether the loop body executes.

JavaScript evaluates it before every iteration.

```javascript
let temperature = 30;

while (temperature > 25) {
    console.log(`Temperature: ${temperature}`);
    temperature--;
}
```

**Output**

```text
Temperature: 30
Temperature: 29
Temperature: 28
Temperature: 27
Temperature: 26
```

When `temperature` reaches `25`, the condition becomes false and the loop terminates.

The condition can use:

* Comparison operators.
* Logical operators.
* Boolean variables.
* Function calls.
* Expressions that produce truthy or falsy values.

**Using a Boolean condition**

```javascript
let isRunning = true;
let count = 0;

while (isRunning) {
    console.log(count);

    count++;

    if (count === 3) {
        isRunning = false;
    }
}
```

**Output**

```text
0
1
2
```

# 🗁 Updating Loop State

A while loop usually requires an update operation inside its body.

The update changes the state used by the condition.

**Increment**

```javascript
let count = 1;

while (count <= 5) {
    console.log(count);
    count++;
}
```

**Decrement**

```javascript
let count = 5;

while (count >= 1) {
    console.log(count);
    count--;
}
```

**Custom increment**

```javascript
let number = 0;

while (number <= 20) {
    console.log(number);
    number += 5;
}
```

**Updating multiple variables**

```javascript
let a = 0;
let b = 10;

while (a < 5) {
    console.log(`a = ${a}, b = ${b}`);

    a++;
    b--;
}
```

The update logic must eventually make the condition false when the loop is intended to terminate.

# 🗁 while Loop with Arrays

A while loop can iterate over arrays using indexes.

```javascript
const fruits = ["Apple", "Banana", "Orange"];

let index = 0;

while (index < fruits.length) {
    console.log(fruits[index]);
    index++;
}
```

**Output**

```text
Apple
Banana
Orange
```

**Iterating in reverse**

```javascript
const numbers = [10, 20, 30, 40, 50];

let index = numbers.length - 1;

while (index >= 0) {
    console.log(numbers[index]);
    index--;
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

**Processing an array conditionally**

```javascript
const numbers = [2, 4, 6, 8, 10];

let index = 0;
let sum = 0;

while (index < numbers.length) {
    if (numbers[index] > 5) {
        sum += numbers[index];
    }

    index++;
}

console.log(sum); // 24
```

# 🗁 Infinite while Loops

An infinite loop occurs when the loop condition never becomes false.

```javascript
while (true) {
    console.log("Running...");
}
```

This loop continues indefinitely unless its execution is interrupted or an exit mechanism is reached.

Infinite loops can be intentional in:

* Event-processing systems.
* Game loops.
* Server processes.
* Background workers.
* Interactive command-line applications.

**Infinite loop with break**

```javascript
let count = 1;

while (true) {
    console.log(count);

    if (count === 5) {
        break;
    }

    count++;
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

# 🗁 The break Statement

The `break` statement immediately terminates the nearest enclosing loop.

```javascript
let number = 1;

while (number <= 10) {
    if (number === 6) {
        break;
    }

    console.log(number);
    number++;
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

The loop terminates when `number` reaches `6`.

**Finding an element**

```javascript
const users = ["John", "Alice", "David", "Sarah"];

const target = "David";

let index = 0;
let found = false;

while (index < users.length) {
    if (users[index] === target) {
        found = true;
        break;
    }

    index++;
}

console.log(found); // true
```

Once the target is found, continuing the search is unnecessary.

# 🗁 The continue Statement

The `continue` statement skips the remaining statements in the current iteration and starts the next condition check.

```javascript
let number = 0;

while (number < 5) {
    number++;

    if (number === 3) {
        continue;
    }

    console.log(number);
}
```

**Output**

```text
1
2
4
5
```

Unlike a traditional `for` loop, a `while` loop does not have an automatic increment expression.

Therefore, update the loop state before executing `continue` when that update is necessary for termination.

**Incorrect implementation**

```javascript
let number = 0;

while (number < 5) {
    if (number === 3) {
        continue;
    }

    number++;
}
```

When `number` becomes `3`, the loop repeatedly executes `continue` without updating the variable.

This creates an infinite loop.

**Correct implementation**

```javascript
let number = 0;

while (number < 5) {
    number++;

    if (number === 3) {
        continue;
    }

    console.log(number);
}
```

# 🗁 Nested while Loops

A nested while loop is a while loop inside another while loop.

The inner loop executes completely for each iteration of the outer loop, unless its execution is interrupted.

```javascript
let i = 1;

while (i <= 3) {
    let j = 1;

    while (j <= 3) {
        console.log(`i = ${i}, j = ${j}`);
        j++;
    }

    i++;
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

Each loop needs its own correctly managed state.

**Generating a multiplication grid**

```javascript
let row = 1;

while (row <= 5) {
    let column = 1;

    while (column <= 5) {
        console.log(`${row} x ${column} = ${row * column}`);
        column++;
    }

    row++;
}
```

# 🗁 Labeled while Loops

JavaScript supports labeled statements for controlling nested loops.

A labeled `break` terminates the specified enclosing loop.

```javascript
outer:
while (true) {
    let number = 1;

    while (number <= 5) {
        console.log(number);

        if (number === 3) {
            break outer;
        }

        number++;
    }
}
```

**Output**

```text
1
2
3
```

A labeled `continue` can also skip the current iteration of a specified outer loop.

```javascript
let row = 1;

outer:
while (row <= 3) {
    let column = 1;

    while (column <= 3) {
        if (column === 2) {
            row++;
            continue outer;
        }

        console.log(`row = ${row}, column = ${column}`);
        column++;
    }

    row++;
}
```

Use labels selectively because complex labeled control flow can reduce readability.

# 🗁 while Loop with Function Calls

A function call can be used as the loop condition.

```javascript
let attempts = 0;

function canContinue() {
    return attempts < 3;
}

while (canContinue()) {
    attempts++;

    console.log(`Attempt: ${attempts}`);
}
```

**Output**

```text
Attempt: 1
Attempt: 2
Attempt: 3
```

The function is called before every iteration.

Be careful with functions that have side effects because their execution can change the loop's behavior.

# 🗁 while Loop vs for Loop

Both loops support repeated execution, but they are suited to different situations.

| Feature | while | for |
| --- | --- | --- |
| Initialization | Usually outside the loop | In the initialization section |
| Condition | Required syntactically | Optional |
| Update | Usually inside the body | In the update section or body |
| Iteration count | Often condition-dependent | Often predetermined |
| Readability | Useful for state-driven repetition | Useful for counter-based repetition |

**Using while**

```javascript
let attempts = 0;

while (attempts < 3) {
    console.log(`Attempt: ${attempts + 1}`);
    attempts++;
}
```

**Using for**

```javascript
for (let attempts = 0; attempts < 3; attempts++) {
    console.log(`Attempt: ${attempts + 1}`);
}
```

Use `while` when the repetition depends primarily on a condition or changing state.

Use `for` when initialization, termination, and iteration updates naturally belong together.

# 🗁 Performance and Complexity

The time complexity of a while loop depends on the number of iterations and the operations performed during each iteration.

**Linear iteration**

```javascript
let i = 0;

while (i < n) {
    i++;
}
```

Time complexity: `O(n)`.

**Logarithmic iteration**

```javascript
let number = n;

while (number > 1) {
    number = Math.floor(number / 2);
}
```

Time complexity: `O(log n)` for positive integer `n`.

**Nested iteration**

```javascript
let i = 0;

while (i < n) {
    let j = 0;

    while (j < n) {
        j++;
    }

    i++;
}
```

Time complexity: `O(n²)`.

The loop construct itself does not determine complexity. The number of iterations and the work performed inside each iteration do.

# 🗁 Practical Examples

**Calculate the sum from 1 to N**

```javascript
const n = 10;

let number = 1;
let sum = 0;

while (number <= n) {
    sum += number;
    number++;
}

console.log(sum); // 55
```

**Calculate a factorial**

```javascript
const n = 5;

let factorial = 1;
let number = 1;

while (number <= n) {
    factorial *= number;
    number++;
}

console.log(factorial); // 120
```

**Count digits in a number**

```javascript
let number = 12345;
let count = 0;

number = Math.abs(Math.trunc(number));

while (number > 0) {
    number = Math.floor(number / 10);
    count++;
}

console.log(count); // 5
```

For zero, the number of digits is one.

```javascript
function countDigits(value) {
    let number = Math.abs(Math.trunc(value));

    if (number === 0) {
        return 1;
    }

    let count = 0;

    while (number > 0) {
        number = Math.floor(number / 10);
        count++;
    }

    return count;
}

console.log(countDigits(12345)); // 5
console.log(countDigits(0));     // 1
```

**Reverse a number**

```javascript
let number = 12345;
let reversed = 0;

while (number > 0) {
    const digit = number % 10;

    reversed = reversed * 10 + digit;

    number = Math.floor(number / 10);
}

console.log(reversed); // 54321
```

**Calculate powers of two**

```javascript
let power = 1;
let exponent = 0;

while (exponent <= 5) {
    console.log(`2^${exponent} = ${power}`);

    power *= 2;
    exponent++;
}
```

**Find the first number divisible by both 7 and 11**

```javascript
let number = 1;

while (number <= 1000) {
    if (number % 7 === 0 && number % 11 === 0) {
        break;
    }

    number++;
}

console.log(number); // 77
```

**Generate the Fibonacci sequence**

```javascript
const limit = 10;

let first = 0;
let second = 1;
let count = 0;

while (count < limit) {
    console.log(first);

    const next = first + second;

    first = second;
    second = next;

    count++;
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

# 🗁 Common Mistakes

**Forgetting to update the loop variable**

```javascript
let i = 0;

while (i < 5) {
    console.log(i);
}
```

The condition remains true because `i` never changes.

**Incorrect update direction**

```javascript
let i = 5;

while (i > 0) {
    console.log(i);
    i++;
}
```

The variable increases instead of decreasing, so the condition remains true.

**Using an incorrect boundary**

```javascript
const numbers = [10, 20, 30];

let index = 0;

while (index <= numbers.length) {
    console.log(numbers[index]);
    index++;
}
```

The final iteration accesses an invalid array index and produces `undefined`.

Use `index < numbers.length`.

**Incorrect continue placement**

```javascript
let i = 0;

while (i < 5) {
    if (i === 2) {
        continue;
    }

    i++;
}
```

The counter stops updating when `i` reaches `2`.

Move the update before `continue` or restructure the loop.

**Modifying the condition variable unexpectedly**

```javascript
let count = 10;

while (count > 0) {
    console.log(count);

    count -= 3;
}
```

This loop produces `10`, `7`, `4`, and `1`. It does not visit every integer.

Understand how the update affects the condition and expected sequence.

# 🗁 Best Practices

* Initialize loop variables before the loop.
* Use clear and predictable termination conditions.
* Update the state that controls the condition.
* Ensure the loop can terminate when termination is expected.
* Use `break` when the required result has been found.
* Use `continue` carefully, especially when updates are performed inside the loop body.
* Avoid unnecessary nested loops.
* Prefer `for` when counter initialization and updates are naturally grouped.
* Prefer `for...of` when processing iterable values without requiring manual index management.
* Keep loop conditions simple and readable.
* Test boundary cases, including empty collections and zero values.

# 🗁 Key Takeaways

* A `while` loop executes repeatedly while its condition is truthy.
* The condition is evaluated before every iteration.
* The loop body may execute zero or more times.
* Initialization and state updates are generally handled outside or inside the loop body.
* A missing or incorrect update can create an infinite loop.
* `break` terminates the nearest enclosing loop.
* `continue` skips the remaining statements in the current iteration.
* Nested while loops are useful for multidimensional processing.
* Labeled statements can control outer loops.
* Time complexity depends on iteration count and operations performed.
* Use `while` when repetition is driven by a condition rather than a predetermined iteration count.
