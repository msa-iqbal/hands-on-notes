# Console Debugging

Console debugging is a technique for inspecting JavaScript execution, identifying problems, and understanding application behavior using the browser or runtime console.

JavaScript provides the `console` object with several methods for displaying messages, inspecting values, measuring execution time, and tracking program behavior.

Console debugging is useful for:

* Inspecting variable values.
* Tracking program execution.
* Identifying errors and warnings.
* Understanding function behavior.
* Debugging asynchronous operations.
* Measuring execution time.
* Inspecting objects and arrays.

# 🗁 The Console Object

The `console` object provides methods for writing diagnostic information to the console.

```javascript
console.log("Hello, JavaScript!");
console.info("Application started");
console.warn("This is a warning");
console.error("Something went wrong");
```

These methods are available in modern browsers and common JavaScript runtimes such as Node.js.

# 🗁 console.log()

`console.log()` displays general information in the console.

```javascript
const username = "John";
const age = 25;

console.log(username);
console.log(age);
console.log("User:", username);
```

Output:

```text
John
25
User: John
```

It is commonly used to inspect variable values and follow program execution.

You can also display multiple values:

```javascript
const name = "Sarah";
const role = "Developer";

console.log(name, role);
```

# 🗁 console.info()

`console.info()` displays informational messages.

```javascript
console.info("Application initialized");
console.info("User successfully authenticated");
```

It communicates general application information rather than errors.

The visual presentation of `console.info()` may vary between environments.

# 🗁 console.warn()

`console.warn()` displays warning messages.

```javascript
const remainingStorage = 5;

if (remainingStorage < 10) {
  console.warn("Storage is almost full");
}
```

Warnings indicate potentially problematic conditions that do not necessarily prevent execution.

# 🗁 console.error()

`console.error()` displays error messages.

```javascript
try {
  JSON.parse("{ invalid json }");
} catch (error) {
  console.error("JSON parsing failed:", error);
}
```

It is useful for displaying failures and diagnostic information.

Calling `console.error()` does not automatically throw an exception or stop execution.

# 🗁 console.debug()

`console.debug()` displays debugging messages.

```javascript
const user = {
  id: 1,
  name: "John"
};

console.debug("Current user:", user);
```

Some browser consoles hide debug-level messages depending on their selected log levels.

# 🗁 console.table()

`console.table()` displays tabular data in a structured table.

```javascript
const users = [
  {
    id: 1,
    name: "John",
    role: "Developer"
  },
  {
    id: 2,
    name: "Sarah",
    role: "Designer"
  },
  {
    id: 3,
    name: "Alex",
    role: "Manager"
  }
];

console.table(users);
```

You can also specify which properties should appear:

```javascript
console.table(users, ["name", "role"]);
```

This method is particularly useful when inspecting arrays of objects.

# 🗁 console.dir()

`console.dir()` displays an object in an interactive or hierarchical representation, depending on the environment.

```javascript
const user = {
  id: 1,
  name: "John",
  address: {
    city: "Dhaka",
    country: "Bangladesh"
  }
};

console.dir(user);
```

It is useful when inspecting object properties and nested structures.

# 🗁 console.group()

`console.group()` groups related console messages together.

```javascript
console.group("User Information");

console.log("Name: John");
console.log("Age: 25");
console.log("Role: Developer");

console.groupEnd();
```

Groups can be nested:

```javascript
console.group("Application");

console.log("Application started");

console.group("Authentication");

console.log("Checking credentials");
console.log("Authentication successful");

console.groupEnd();

console.groupEnd();
```

# 🗁 console.groupCollapsed()

`console.groupCollapsed()` creates a collapsed message group.

```javascript
console.groupCollapsed("API Response");

console.log("Status: 200");
console.log("Message: Success");

console.groupEnd();
```

Unlike `console.group()`, the group starts collapsed in environments that support this presentation.

# 🗁 console.count()

`console.count()` displays how many times a particular label has been used.

```javascript
function processRequest() {
  console.count("Request processed");
}

processRequest();
processRequest();
processRequest();
```

Output:

```text
Request processed: 1
Request processed: 2
Request processed: 3
```

Each distinct label maintains its own counter.

```javascript
console.count("Login");
console.count("Register");
console.count("Login");
```

# 🗁 console.countReset()

`console.countReset()` resets a counter created with `console.count()`.

```javascript
console.count("Request");

console.count("Request");

console.countReset("Request");

console.count("Request");
```

Output:

```text
Request: 1
Request: 2
Request: 1
```

# 🗁 console.time()

`console.time()` starts a timer with a specified label.

```javascript
console.time("Operation");

for (let i = 0; i < 1000000; i++) {
  // Perform an operation
}

console.timeEnd("Operation");
```

The elapsed time is displayed when the timer ends.

# 🗁 console.timeEnd()

`console.timeEnd()` stops a previously started timer and displays its elapsed duration.

```javascript
console.time("Data Processing");

const numbers = Array.from(
  { length: 100000 },
  (_, index) => index
);

console.timeEnd("Data Processing");
```

Timer labels must match the corresponding `console.time()` label.

# 🗁 console.timeLog()

`console.timeLog()` displays the current elapsed time without stopping the timer.

```javascript
console.time("Task");

for (let i = 0; i < 100000; i++) {
  // First operation
}

console.timeLog("Task", "First operation completed");

for (let i = 0; i < 100000; i++) {
  // Second operation
}

console.timeEnd("Task");
```

This is useful for measuring different stages of a longer operation.

# 🗁 console.assert()

`console.assert()` displays an assertion message when its condition evaluates to false.

```javascript
const age = 15;

console.assert(age >= 18, "User must be at least 18 years old");
```

If the condition is true, nothing is normally displayed.

If the condition is false, an assertion message appears.

Important: `console.assert()` does not throw an exception or stop program execution.

# 🗁 console.trace()

`console.trace()` displays a stack trace showing the current call path.

```javascript
function first() {
  second();
}

function second() {
  third();
}

function third() {
  console.trace("Execution trace");
}

first();
```

It helps identify which function calls led to a particular point in the program.

# 🗁 console.clear()

`console.clear()` clears the console output when supported by the environment.

```javascript
console.log("Old messages");

console.clear();

console.log("New messages");
```

This is useful when you want to remove previous diagnostic output.

# 🗁 console.log() with Variables

You can inspect multiple variables in one statement.

```javascript
const username = "John";
const age = 25;
const isActive = true;

console.log(username, age, isActive);
```

For more readable output, use object shorthand:

```javascript
console.log({
  username,
  age,
  isActive
});
```

Output:

```javascript
{
  username: "John",
  age: 25,
  isActive: true
}
```

This approach makes it easier to identify which value belongs to which variable.

# 🗁 String Substitution

Console methods support format specifiers in many environments.

```javascript
const username = "John";
const age = 25;

console.log("Name: %s", username);
console.log("Age: %d", age);
console.log("User: %s, Age: %d", username, age);
```

Common format specifiers include:

| Specifier | Purpose |
| --- | --- |
| `%s` | String formatting |
| `%d` | Numeric formatting |
| `%i` | Integer formatting |
| `%f` | Floating-point formatting |
| `%o` | Object formatting |
| `%O` | Object formatting in supported browser consoles |

Formatting behavior can vary between browser and runtime implementations.

# 🗁 Inspecting Objects

Console methods make it easier to inspect complex JavaScript objects.

```javascript
const product = {
  id: 101,
  name: "Laptop",
  price: 75000,
  specifications: {
    processor: "Intel Core i7",
    memory: "16GB",
    storage: "512GB SSD"
  }
};

console.log(product);
console.dir(product);
console.table(product.specifications);
```

For arrays of objects, `console.table()` is often more readable than `console.log()`.

# 🗁 Debugging Conditional Logic

Console messages can help determine which branch of a condition executes.

```javascript
const score = 75;

if (score >= 80) {
  console.log("Grade A");
} else if (score >= 60) {
  console.log("Grade B");
} else {
  console.log("Grade C");
}
```

You can add diagnostic messages to inspect the evaluated condition:

```javascript
console.log("Score:", score);
console.log("Passed condition:", score >= 60);
```

# 🗁 Debugging Loops

Console methods help inspect loop execution and identify unexpected iterations.

```javascript
for (let i = 0; i < 5; i++) {
  console.log("Current iteration:", i);
}
```

For more structured output:

```javascript
const users = ["John", "Sarah", "Alex"];

users.forEach((user, index) => {
  console.log({
    index,
    user
  });
});
```

# 🗁 Debugging Asynchronous Code

Console messages can help track the execution order of asynchronous operations.

```javascript
console.log("Start");

setTimeout(() => {
  console.log("Timeout callback");
}, 0);

Promise.resolve().then(() => {
  console.log("Promise callback");
});

console.log("End");
```

Output:

```text
Start
End
Promise callback
Timeout callback
```

This demonstrates how synchronous code, Promise callbacks, and timer callbacks execute in different stages of the event loop.

# 🗁 Debugging Fetch Requests

Console debugging can help inspect HTTP requests and responses.

```javascript
async function getUsers() {
  try {
    console.log("Request started");

    const response = await fetch("/api/users");

    console.log("Response status:", response.status);

    if (!response.ok) {
      throw new Error(`HTTP error: ${response.status}`);
    }

    const users = await response.json();

    console.table(users);
  } catch (error) {
    console.error("Request failed:", error);
  }
}

getUsers();
```

This example distinguishes HTTP response failures from network or parsing failures.

# 🗁 Debugging with console.assert()

Assertions can help verify assumptions during development.

```javascript
function calculateTotal(price, quantity) {
  console.assert(
    typeof price === "number",
    "Price must be a number"
  );

  console.assert(
    typeof quantity === "number",
    "Quantity must be a number"
  );

  return price * quantity;
}

calculateTotal(100, 5);
```

For production-critical validation, use explicit validation logic and appropriate error handling instead of relying on console assertions.

# 🗁 Styling Console Messages

Browser developer consoles support CSS styling through the `%c` format specifier.

```javascript
console.log(
  "%cApplication Started",
  "color: green; font-size: 18px; font-weight: bold;"
);
```

Multiple styles can be applied:

```javascript
console.log(
  "%cSuccess %cUser authenticated",
  "color: green; font-weight: bold;",
  "color: blue;"
);
```

CSS styling support is primarily a browser developer tools feature and is not consistently supported by Node.js consoles.

# 🗁 Console Methods Reference

| Method | Purpose |
| --- | --- |
| `console.log()` | General output |
| `console.info()` | Informational messages |
| `console.warn()` | Warning messages |
| `console.error()` | Error messages |
| `console.debug()` | Debugging messages |
| `console.table()` | Tabular data |
| `console.dir()` | Object inspection |
| `console.group()` | Group messages |
| `console.groupCollapsed()` | Create collapsed groups |
| `console.groupEnd()` | End a message group |
| `console.count()` | Count occurrences |
| `console.countReset()` | Reset a counter |
| `console.time()` | Start a timer |
| `console.timeLog()` | Display elapsed time |
| `console.timeEnd()` | Stop a timer |
| `console.assert()` | Display failed assertions |
| `console.trace()` | Display a stack trace |
| `console.clear()` | Clear console output |

# 🗁 Common Mistakes

**Leaving excessive console logs in production**

Debugging messages can expose internal information and clutter application output.

Remove unnecessary logs or use a structured logging system.

**Using console.log() for every debugging problem**

Some situations require more specialized methods.

For example:

* Use `console.table()` for collections.
* Use `console.trace()` for call paths.
* Use `console.time()` for execution timing.
* Use browser breakpoints for inspecting execution step by step.

**Assuming console.error() throws an exception**

```javascript
console.error("Something failed");

console.log("Execution continues");
```

`console.error()` only displays a message. It does not throw an exception.

**Assuming console.assert() stops execution**

A failed assertion only displays diagnostic information. It does not automatically terminate the program.

**Ignoring console output during asynchronous execution**

Asynchronous operations can produce messages in an order different from the order in which they were initiated.

# 🗁 Best Practices

* Use descriptive messages that explain what is being inspected.
* Prefer `console.table()` for arrays of objects.
* Use `console.group()` to organize related output.
* Use `console.time()` to measure specific operations.
* Use `console.trace()` to investigate unexpected call paths.
* Log errors with their relevant diagnostic information.
* Avoid logging passwords, access tokens, or other sensitive data.
* Remove temporary debugging messages before production deployment.
* Use browser breakpoints for complex execution problems.
* Use structured logging tools for larger production applications.

# 🗁 Key Takeaways

* The `console` object provides built-in debugging methods.
* `console.log()` displays general information.
* `console.warn()` and `console.error()` communicate warnings and failures.
* `console.table()` makes structured data easier to inspect.
* `console.group()` organizes related messages.
* `console.count()` tracks how many times an operation occurs.
* `console.time()` measures execution duration.
* `console.assert()` checks assumptions without stopping execution.
* `console.trace()` displays the current call stack.
* Console debugging is useful during development, but complex applications also benefit from dedicated debugging and logging tools.
