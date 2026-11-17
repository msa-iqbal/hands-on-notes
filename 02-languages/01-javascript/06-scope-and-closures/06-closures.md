# Closures

A closure is a JavaScript feature that allows a function to **retain access to variables from its surrounding lexical scope**, even after the outer function has finished executing.

A closure is created when a function is defined inside another function and maintains access to the outer function's variables.

Closures are commonly used for:

* Data privacy and encapsulation.
* Maintaining state between function calls.
* Function factories.
* Callbacks and event handlers.
* Memoization.
* Module patterns.
* Maintaining state in asynchronous operations.

# 🗁 Basic Closure Example

```javascript
function outer() {
  const message = "Hello from outer function";

  function inner() {
    console.log(message);
  }

  return inner;
}

const result = outer();

result();
```

Output:

```text
Hello from outer function
```

**How it works:**

* `outer()` creates a local variable named `message`.
* `outer()` defines the `inner()` function.
* `outer()` returns the `inner` function.
* The outer function finishes execution.
* `result()` calls the returned function.
* `inner()` still has access to `message`.

The `inner()` function retains access to its lexical environment through a closure.

# 🗁 Closures Maintain State

Closures allow functions to preserve state between multiple function calls.

Example:

```javascript
function createCounter() {
  let count = 0;

  return function () {
    count++;

    return count;
  };
}

const counter = createCounter();

console.log(counter());
console.log(counter());
console.log(counter());
```

Output:

```text
1
2
3
```

The `count` variable is not recreated every time `counter()` executes.

Instead, the returned function continues accessing the same `count` variable.

# 🗁 Independent Closure Instances

Each invocation of an outer function creates a separate lexical environment.

```javascript
function createCounter() {
  let count = 0;

  return function () {
    count++;

    return count;
  };
}

const counterA = createCounter();
const counterB = createCounter();

console.log(counterA());
console.log(counterA());

console.log(counterB());
console.log(counterB());
```

Output:

```text
1
2
1
2
```

`counterA` and `counterB` maintain independent `count` variables.

Each call to `createCounter()` creates a new closure environment.

# 🗁 Closures with Parameters

Closures can retain access to parameters of their outer functions.

```javascript
function greet(name) {
  return function () {
    console.log(`Hello, ${name}!`);
  };
}

const greetMuhammad = greet("Muhammad");
const greetDeveloper = greet("Developer");

greetMuhammad();
greetDeveloper();
```

Output:

```text
Hello, Muhammad!
Hello, Developer!
```

Each returned function retains the `name` value from its own invocation.

# 🗁 Closures for Data Privacy

JavaScript closures can provide private state by keeping variables inside a function's lexical environment.

```javascript
function createAccount(initialBalance) {
  let balance = initialBalance;

  return {
    deposit(amount) {
      if (amount > 0) {
        balance += amount;
      }

      return balance;
    },

    withdraw(amount) {
      if (amount > 0 && amount <= balance) {
        balance -= amount;
      }

      return balance;
    },

    getBalance() {
      return balance;
    }
  };
}

const account = createAccount(1000);

console.log(account.getBalance());

account.deposit(500);
console.log(account.getBalance());

account.withdraw(200);
console.log(account.getBalance());
```

Output:

```text
1000
1500
1300
```

The `balance` variable cannot be accessed directly from outside the `createAccount()` function.

```javascript
console.log(account.balance);
```

Output:

```text
undefined
```

The returned methods can access and modify `balance` because they share the same lexical environment.

# 🗁 Closures with Multiple Methods

Multiple functions returned from the same outer function can share the same variables.

```javascript
function createUser() {
  let username = "Muhammad";

  return {
    getUsername() {
      return username;
    },

    setUsername(newUsername) {
      username = newUsername;
    }
  };
}

const user = createUser();

console.log(user.getUsername());

user.setUsername("Iqbal");

console.log(user.getUsername());
```

Output:

```text
Muhammad
Iqbal
```

Both methods share access to the same `username` variable.

# 🗁 Closures in Callbacks

Closures are particularly useful when functions are passed as callbacks.

```javascript
function createGreeting(name) {
  return function () {
    console.log(`Welcome, ${name}!`);
  };
}

function executeCallback(callback) {
  callback();
}

const greeting = createGreeting("Muhammad");

executeCallback(greeting);
```

Output:

```text
Welcome, Muhammad!
```

The callback retains access to the `name` variable even though it executes in another function.

# 🗁 Closures with setTimeout

Closures allow asynchronous callbacks to access variables from their original lexical environment.

```javascript
function delayedMessage(message) {
  setTimeout(() => {
    console.log(message);
  }, 1000);
}

delayedMessage("Hello after one second");
```

Output after approximately one second:

```text
Hello after one second
```

The callback retains access to `message` after `delayedMessage()` has returned.

# 🗁 Closures and Loops

Closures can produce unexpected results when combined with `var` in loops.

**Using var**

```javascript
for (var i = 0; i < 3; i++) {
  setTimeout(() => {
    console.log(i);
  }, 1000);
}
```

Output after approximately one second:

```text
3
3
3
```

The `var` declaration creates one function-scoped variable shared by all three callbacks.

By the time the callbacks execute, the loop has completed and `i` is `3`.

**Using let**

```javascript
for (let i = 0; i < 3; i++) {
  setTimeout(() => {
    console.log(i);
  }, 1000);
}
```

Output after approximately one second:

```text
0
1
2
```

The `let` declaration creates a separate binding for each loop iteration, allowing each callback to retain its corresponding value.

# 🗁 Closures and Function Factories

A function factory is a function that returns another function.

Closures allow the returned functions to retain configuration values.

```javascript
function multiplyBy(multiplier) {
  return function (number) {
    return number * multiplier;
  };
}

const double = multiplyBy(2);
const triple = multiplyBy(3);

console.log(double(10));
console.log(triple(10));
```

Output:

```text
20
30
```

The `double` function retains `multiplier = 2`, while `triple` retains `multiplier = 3`.

# 🗁 Closures and Memoization

Memoization is an optimization technique that stores previously calculated results so they can be reused.

Closures can maintain a cache between function calls.

```javascript
function memoize(fn) {
  const cache = new Map();

  return function (number) {
    if (cache.has(number)) {
      console.log("Returning cached result");

      return cache.get(number);
    }

    const result = fn(number);

    cache.set(number, result);

    return result;
  };
}

function square(number) {
  console.log("Calculating...");

  return number * number;
}

const memoizedSquare = memoize(square);

console.log(memoizedSquare(5));
console.log(memoizedSquare(5));
console.log(memoizedSquare(6));
```

Output:

```text
Calculating...
25
Returning cached result
25
Calculating...
36
```

The `cache` variable remains accessible through the returned function.

# 🗁 Closures and Garbage Collection

Closures retain references to variables they need from their lexical environments.

As long as a closure remains reachable, the variables it references must remain available.

```javascript
function createLogger() {
  const message = "Application started";

  return function () {
    console.log(message);
  };
}

let logger = createLogger();

logger();

// Release the reference to the closure.
logger = null;
```

After the closure becomes unreachable, its associated environment and other unreachable objects can become eligible for garbage collection.

JavaScript engines determine when memory can actually be reclaimed.

Closures do not automatically cause memory leaks. However, unnecessarily retaining closures can keep large objects reachable longer than intended.

# 🗁 Common Mistakes

**Assuming outer variables are copied into closures**

Closures retain access to variable bindings, not simply snapshots of their values.

```javascript
function createCounter() {
  let count = 0;

  return {
    increment() {
      count++;
    },

    getCount() {
      return count;
    }
  };
}

const counter = createCounter();

counter.increment();
counter.increment();

console.log(counter.getCount());
```

Output:

```text
2
```

Both methods access the same variable binding.

**Assuming closures only work after outer functions return**

Closures are a consequence of lexical scope. A function can access its surrounding lexical environment even while the outer function is still executing.

**Using closures unnecessarily**

Closures are useful for state and encapsulation, but excessive nesting can make code harder to understand and maintain.

**Retaining unnecessary references**

Long-lived callbacks, event listeners, or cached functions can retain objects that are no longer needed.

# 🗁 Best Practices

* Use closures to encapsulate private state.
* Keep closure environments as small as practical.
* Use closures for callbacks and function factories when they improve code organization.
* Prefer `let` and `const` over `var` in loops involving callbacks.
* Avoid unnecessary nested functions.
* Release references to long-lived callbacks when they are no longer needed.
* Consider classes or modules when they provide a clearer abstraction.
* Understand which variables a closure retains when investigating memory usage.

# 🗁 Key Takeaways

* A closure is a function combined with access to its surrounding lexical environment.
* Closures retain access to outer variables even after the outer function returns.
* Each invocation of an outer function can create an independent closure environment.
* Multiple closures can share the same lexical environment.
* Closures are useful for private state, callbacks, function factories, and memoization.
* JavaScript closures retain variable bindings rather than merely copying values.
* Retained references remain available while reachable and can affect memory usage.
