# Recursion

**Recursion** is a programming technique in which a function calls itself to solve a problem.

A recursive function breaks a complex problem into smaller versions of the same problem until it reaches a condition where no further recursive calls are necessary.

Recursion is commonly used for:

* Tree and graph traversal.
* Searching and sorting algorithms.
* Mathematical calculations.
* Nested data processing.
* Divide-and-conquer algorithms.
* Backtracking problems.

# 🗁 Understanding Recursive Functions

A recursive function calls itself from within its own body.

```javascript
function countDown(number) {
  if (number <= 0) {
    console.log("Done!");
    return;
  }

  console.log(number);

  countDown(number - 1);
}

countDown(5);
```

Output:

```text
5
4
3
2
1
Done!
```

Execution flow:

* `countDown(5)` prints `5`.
* It calls `countDown(4)`.
* `countDown(4)` prints `4` and calls `countDown(3)`.
* The process continues until `number` reaches `0`.
* The base condition stops further recursive calls.

# 🗁 Essential Components of Recursion

A correct recursive function generally contains two essential components.

**Base Case**

The base case defines the condition under which recursion stops.

```javascript
if (number <= 0) {
  return;
}
```

Without a reachable base case, recursive calls may continue until the JavaScript engine encounters a call stack limit.

**Recursive Case**

The recursive case defines how the function reduces the problem and calls itself again.

```javascript
return calculate(number - 1);
```

A recursive case should make progress toward the base case.

# 🗁 Factorial Using Recursion

The factorial of a non-negative integer is the product of all positive integers up to that number.

Mathematical definition:

\[
n! = n \times (n-1)!
\]

Base case:

\[
0! = 1
\]

JavaScript implementation:

```javascript
function factorial(number) {
  if (number === 0 || number === 1) {
    return 1;
  }

  return number * factorial(number - 1);
}

console.log(factorial(5)); // 120
```

Execution:

```text
factorial(5)
5 * factorial(4)
5 * 4 * factorial(3)
5 * 4 * 3 * factorial(2)
5 * 4 * 3 * 2 * factorial(1)
5 * 4 * 3 * 2 * 1
120
```

The recursive calls must return before the multiplication operations can finish.

# 🗁 Fibonacci Sequence Using Recursion

The Fibonacci sequence is defined by adding the previous two numbers.

\[
F(n) = F(n-1) + F(n-2)
\]

Base cases:

\[
F(0) = 0,\quad F(1) = 1
\]

Implementation:

```javascript
function fibonacci(number) {
  if (number <= 1) {
    return number;
  }

  return fibonacci(number - 1) + fibonacci(number - 2);
}

console.log(fibonacci(6)); // 8
```

The sequence begins:

```text
0, 1, 1, 2, 3, 5, 8, 13, 21, 34
```

This implementation is easy to understand but inefficient for larger inputs because it repeatedly calculates the same values.

# 🗁 Understanding the Call Stack

JavaScript uses a call stack to manage function execution.

Every function call creates an execution context that is placed on the stack. When a function returns, its execution context is removed.

Consider:

```javascript
function recursive(number) {
  if (number === 0) {
    return;
  }

  console.log("Entering:", number);

  recursive(number - 1);

  console.log("Leaving:", number);
}

recursive(3);
```

Output:

```text
Entering: 3
Entering: 2
Entering: 1
Leaving: 1
Leaving: 2
Leaving: 3
```

The execution has two phases:

* **Stack growth:** Recursive calls are added to the call stack.
* **Stack unwinding:** Functions return in reverse order.

This behavior follows the Last In, First Out (LIFO) principle.

# 🗁 Recursion with Arrays

Recursion can process arrays by handling one element at a time.

Example: calculating the sum of an array.

```javascript
function sumArray(numbers, index = 0) {
  if (index >= numbers.length) {
    return 0;
  }

  return numbers[index] + sumArray(numbers, index + 1);
}

console.log(sumArray([10, 20, 30, 40]));
// 100
```

The function processes the current element and recursively processes the remaining elements.

# 🗁 Recursion with Nested Objects

Recursive functions are particularly useful when data contains an unknown number of nested levels.

Example: traversing a nested object.

```javascript
const user = {
  name: "Alice",
  address: {
    city: "Dhaka",
    location: {
      area: "Gazipur",
    },
  },
};

function printObject(object) {
  for (const key in object) {
    const value = object[key];

    if (value !== null && typeof value === "object") {
      printObject(value);
    } else {
      console.log(`${key}: ${value}`);
    }
  }
}

printObject(user);
```

Output:

```text
name: Alice
city: Dhaka
area: Gazipur
```

The function continues traversing nested objects until it reaches primitive values.

# 🗁 Direct and Indirect Recursion

**Direct Recursion**

A function directly calls itself.

```javascript
function directRecursion(number) {
  if (number <= 0) {
    return;
  }

  console.log(number);

  directRecursion(number - 1);
}
```

**Indirect Recursion**

Two or more functions call each other in a cycle.

```javascript
function isEven(number) {
  if (number === 0) {
    return true;
  }

  return isOdd(number - 1);
}

function isOdd(number) {
  if (number === 0) {
    return false;
  }

  return isEven(number - 1);
}

console.log(isEven(4)); // true
console.log(isOdd(5));  // true
```

Both functions participate in the recursive process.

# 🗁 Recursion vs Iteration

Recursion and iteration are two different approaches to repetition.

| Feature | Recursion | Iteration |
| --- | --- | --- |
| Mechanism | Function calls itself | Loops repeat instructions |
| Termination | Base case | Loop condition |
| Memory | Uses call stack | Usually uses less call-stack memory |
| Readability | Often natural for hierarchical problems | Often simpler for linear repetition |
| Performance | Can introduce function-call overhead | Usually has lower overhead |
| Stack overflow | Possible with deep recursion | Not caused by ordinary loop repetition |

Iterative factorial:

```javascript
function factorial(number) {
  let result = 1;

  for (let i = 2; i <= number; i++) {
    result *= i;
  }

  return result;
}

console.log(factorial(5)); // 120
```

The iterative approach avoids creating a recursive call stack.

# 🗁 Tail Recursion

Tail recursion occurs when the recursive function call is the final operation performed by the function.

Example:

```javascript
function factorial(number, accumulator = 1) {
  if (number <= 1) {
    return accumulator;
  }

  return factorial(number - 1, number * accumulator);
}

console.log(factorial(5)); // 120
```

The recursive call is in tail position because the function returns its result directly.

Some programming languages optimize tail-recursive calls through **Tail Call Optimization (TCO)**.

JavaScript specifies proper tail calls in certain strict-mode conditions, but widespread engine support is limited. Therefore, do not assume that tail recursion prevents stack overflow in JavaScript.

# 🗁 Memoization in Recursive Functions

Memoization stores previously calculated results so they can be reused.

It is especially useful when recursive calculations repeatedly solve the same subproblems.

Example: optimized Fibonacci calculation.

```javascript
function fibonacci(number, cache = {}) {
  if (number <= 1) {
    return number;
  }

  if (cache[number] !== undefined) {
    return cache[number];
  }

  cache[number] =
    fibonacci(number - 1, cache) +
    fibonacci(number - 2, cache);

  return cache[number];
}

console.log(fibonacci(40)); // 102334155
```

Memoization avoids redundant calculations and reduces the time complexity of this Fibonacci implementation from exponential to linear.

# 🗁 Common Recursion Errors

**Missing Base Case**

A recursive function without a stopping condition can continue indefinitely.

```javascript
function repeat() {
  repeat();
}

// repeat(); // Eventually causes a stack overflow
```

**Incorrect Progression**

The recursive argument must move toward the base case.

```javascript
function countDown(number) {
  if (number === 0) {
    return;
  }

  countDown(number + 1); // Incorrect progression
}
```

**Incorrect Return Handling**

Recursive results must be returned when the calculation depends on them.

```javascript
function factorial(number) {
  if (number <= 1) {
    return 1;
  }

  return number * factorial(number - 1);
}
```

**Excessive Recursion Depth**

Deep recursive calls can exceed the JavaScript engine's call stack limit.

For large inputs, consider iteration, explicit stacks, or other algorithmic approaches.

# 🗁 Best Practices

* Always define a clear base case.
* Ensure every recursive call progresses toward the base case.
* Return recursive results when they are needed by the calculation.
* Avoid unnecessary recursive calls.
* Use memoization when subproblems repeat.
* Prefer iteration when recursion would create excessive call-stack depth.
* Use recursion naturally for trees, nested structures, and divide-and-conquer algorithms.
* Validate input when the function expects a restricted range or type.

# 🗁 Summary

* Recursion is a technique where a function calls itself.
* Every recursive function needs a base case and a recursive case.
* JavaScript manages recursive calls through the call stack.
* Recursive calls execute until the base case is reached, then unwind in reverse order.
* Recursion is useful for nested structures, trees, graphs, and mathematical problems.
* Iteration can be more memory-efficient for simple repetitive operations.
* Memoization improves performance by caching previously calculated results.
* JavaScript recursion can encounter stack overflow when call depth becomes excessive.

# 🗁 Practice Exercises

* Write a recursive function that counts down from a given number.
* Calculate the factorial of a number using recursion.
* Generate the Fibonacci sequence using recursion.
* Calculate the sum of all numbers in an array recursively.
* Find the maximum value in an array using recursion.
* Reverse a string using recursion.
* Check whether a string is a palindrome using recursion.
* Calculate the power of a number recursively.
* Flatten a nested array using recursion.
* Traverse a nested object and print all primitive values.
* Implement Fibonacci with memoization.
* Compare recursive and iterative implementations of the same problem.
