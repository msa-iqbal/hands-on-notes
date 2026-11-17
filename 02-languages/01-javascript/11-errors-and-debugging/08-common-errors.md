# Common Errors

JavaScript errors occur when code encounters invalid operations, unexpected values, incorrect syntax, or conditions that prevent normal execution.

Understanding common JavaScript errors helps developers identify problems quickly and write more reliable applications.

Common error categories include:

* Syntax errors.
* Reference errors.
* Type errors.
* Range errors.
* URI errors.
* JSON parsing errors.
* Asynchronous errors.
* Logical errors.

This documentation covers frequent JavaScript mistakes, their causes, examples, and solutions.

# 🗁 SyntaxError

A `SyntaxError` occurs when JavaScript encounters invalid syntax while parsing code or data.

**Missing closing bracket**

```javascript
const user = {
  name: "John",
  age: 25
```

Correct:

```javascript
const user = {
  name: "John",
  age: 25
};
```

**Missing quotation mark**

```javascript
const message = "Hello World;
```

Correct:

```javascript
const message = "Hello World";
```

**Invalid JSON**

```javascript
JSON.parse("{ name: 'John' }");
```

Correct JSON requires double-quoted property names and string values:

```javascript
JSON.parse('{"name": "John"}');
```

**Common causes**

* Missing brackets.
* Missing quotation marks.
* Invalid object or array syntax.
* Incorrect function declarations.
* Invalid JSON.
* Invalid dynamically evaluated JavaScript.

# 🗁 ReferenceError

A `ReferenceError` occurs when JavaScript cannot resolve an identifier in the current execution context.

**Using an undeclared variable**

```javascript
console.log(username);
```

If `username` has not been declared, JavaScript throws a `ReferenceError`.

Correct:

```javascript
const username = "John";

console.log(username);
```

**Accessing a variable before initialization**

```javascript
console.log(age);

let age = 25;
```

This produces a `ReferenceError` because `age` is in its temporal dead zone before its declaration is initialized.

Correct:

```javascript
let age = 25;

console.log(age);
```

**Incorrect variable names**

```javascript
const userName = "John";

console.log(username);
```

JavaScript is case-sensitive, so `userName` and `username` are different identifiers.

Correct:

```javascript
const userName = "John";

console.log(userName);
```

# 🗁 TypeError

A `TypeError` occurs when an operation is performed on a value that does not support that operation.

**Accessing a property of null**

```javascript
const user = null;

console.log(user.name);
```

Correct:

```javascript
const user = null;

console.log(user?.name);
```

Optional chaining returns `undefined` when the value before `?.` is `null` or `undefined`.

**Calling a non-function**

```javascript
const username = "John";

username();
```

Correct:

```javascript
function greet() {
  console.log("Hello");
}

greet();
```

**Using an incorrect array method**

```javascript
const numbers = [1, 2, 3];

numbers.map();
```

Calling `map()` without a callback produces a `TypeError`.

Correct:

```javascript
const doubled = numbers.map(number => number * 2);

console.log(doubled);
```

**Assigning to a constant**

```javascript
const age = 25;

age = 30;
```

Correct:

```javascript
let age = 25;

age = 30;
```

Reassigning a `const` binding produces a `TypeError`.

# 🗁 RangeError

A `RangeError` occurs when a value is outside the acceptable range for an operation.

**Invalid array length**

```javascript
const numbers = new Array(-5);
```

Correct:

```javascript
const numbers = new Array(5);
```

**Invalid numeric precision**

```javascript
const number = 10.12345;

console.log(number.toFixed(101));
```

`toFixed()` does not support that many fractional digits.

Correct:

```javascript
const number = 10.12345;

console.log(number.toFixed(2));
```

**Excessive recursion**

```javascript
function repeat() {
  repeat();
}

repeat();
```

Uncontrolled recursion can exceed the available call stack and produce a `RangeError` in common JavaScript engines.

Correct:

```javascript
function repeat(count) {
  if (count <= 0) {
    return;
  }

  repeat(count - 1);
}

repeat(5);
```

# 🗁 URIError

A `URIError` occurs when URI encoding or decoding functions receive malformed encoded input.

Example:

```javascript
decodeURIComponent("%");
```

Correct:

```javascript
decodeURIComponent("Hello%20World");
```

Output:

```text
Hello World
```

Common causes:

* Invalid percent-encoded sequences.
* Malformed encoded URI components.
* Invalid encoded character sequences.

# 🗁 EvalError

`EvalError` is associated with the global `eval()` function but is primarily retained for backward compatibility.

Modern JavaScript engines generally do not throw it for ordinary `eval()` failures.

Example:

```javascript
const error = new EvalError("Invalid evaluation");

console.log(error.name);
console.log(error.message);
```

In modern JavaScript, errors involving evaluated source code are more commonly represented by `SyntaxError`, `ReferenceError`, or other appropriate error types.

# 🗁 JSON Parsing Errors

JSON parsing errors occur when input does not follow valid JSON syntax.

Example:

```javascript
const data = JSON.parse("{ name: John }");
```

Problems include:

* Property names must use double quotes.
* String values must use double quotes.
* Trailing commas are not allowed.
* Comments are not allowed.
* JavaScript expressions are not valid JSON.

Correct:

```javascript
const data = JSON.parse(`{
  "name": "John",
  "age": 25,
  "active": true
}`);

console.log(data);
```

Handle parsing failures:

```javascript
try {
  const data = JSON.parse('{"name": "John"}');

  console.log(data);
} catch (error) {
  console.error("Invalid JSON:", error.message);
}
```

# 🗁 Undefined and Null Errors

JavaScript distinguishes between `undefined` and `null`.

* `undefined` usually indicates that a value has not been assigned or is unavailable.
* `null` represents an intentional absence of a value.

Both can cause errors when accessing properties directly.

Example:

```javascript
let user;

console.log(user.name);
```

This produces a `TypeError` because `user` is `undefined`.

Correct:

```javascript
let user;

console.log(user?.name);
```

Another example:

```javascript
const user = null;

console.log(user?.name);
```

Optional chaining prevents the property-access error.

# 🗁 Array Errors

Array-related errors often occur when developers use incorrect indexes or assume an array method behaves differently than it does.

**Accessing an invalid index**

```javascript
const users = ["John", "Sarah"];

console.log(users[10]);
```

This returns `undefined`, not an exception.

Correct handling:

```javascript
const users = ["John", "Sarah"];

const index = 10;

if (index >= 0 && index < users.length) {
  console.log(users[index]);
} else {
  console.log("User does not exist");
}
```

**Using an incorrect method**

```javascript
const numbers = [1, 2, 3];

numbers.push = 10;
```

This replaces the `push` method with a number, so subsequent calls to `numbers.push()` will fail.

Correct:

```javascript
const numbers = [1, 2, 3];

numbers.push(10);
```

**Confusing map() and forEach()**

```javascript
const numbers = [1, 2, 3];

const result = numbers.forEach(number => number * 2);

console.log(result);
```

`forEach()` returns `undefined`.

If you need a new array, use `map()`:

```javascript
const result = numbers.map(number => number * 2);

console.log(result);
```

# 🗁 Object Errors

Object-related errors commonly involve incorrect property access, mutation, and assumptions about object structure.

**Accessing a missing property**

```javascript
const user = {
  name: "John"
};

console.log(user.address.city);
```

This produces a `TypeError` because `address` is `undefined`.

Correct:

```javascript
console.log(user.address?.city);
```

**Incorrect object destructuring**

```javascript
const user = null;

const { name } = user;
```

Correct:

```javascript
const user = null;

const { name } = user ?? {};
```

**Confusing object references**

```javascript
const first = {
  name: "John"
};

const second = first;

second.name = "Sarah";

console.log(first.name);
```

Output:

```text
Sarah
```

Both variables reference the same object.

If an independent shallow copy is required:

```javascript
const first = {
  name: "John"
};

const second = { ...first };

second.name = "Sarah";

console.log(first.name);
console.log(second.name);
```

# 🗁 Function Errors

Function-related mistakes often involve incorrect arguments, missing return statements, and unexpected execution contexts.

**Calling a function before initialization**

```javascript
const greet = () => {
  console.log("Hello");
};

greet();
```

Function expressions assigned to `const` cannot be called before initialization.

**Missing return statement**

```javascript
function add(a, b) {
  a + b;
}

console.log(add(10, 20));
```

Output:

```text
undefined
```

Correct:

```javascript
function add(a, b) {
  return a + b;
}
```

**Incorrect callback usage**

```javascript
const numbers = [1, 2, 3];

numbers.map(console.log("Hello"));
```

The logging expression executes immediately, and its result is passed to `map()`.

Correct:

```javascript
numbers.map(number => {
  console.log(number);

  return number * 2;
});
```

# 🗁 Promise Errors

Promise-related errors often occur when rejected Promises are not handled correctly.

**Unhandled Promise rejection**

```javascript
Promise.reject(new Error("Request failed"));
```

The rejected Promise has no rejection handler.

Correct:

```javascript
Promise.reject(new Error("Request failed"))
  .catch(error => {
    console.error(error.message);
  });
```

**Using try...catch without await**

```javascript
async function getData() {
  try {
    fetch("/api/data");
  } catch (error) {
    console.error(error);
  }
}
```

The `try...catch` does not handle a later rejection because the Promise is not awaited.

Correct:

```javascript
async function getData() {
  try {
    const response = await fetch("/api/data");

    console.log(response);
  } catch (error) {
    console.error(error);
  }
}
```

**Forgetting that fetch does not reject on HTTP error statuses**

```javascript
async function getData() {
  const response = await fetch("/api/users");

  const users = await response.json();

  return users;
}
```

A response with status `404` or `500` normally still fulfills the Fetch Promise.

Correct:

```javascript
async function getData() {
  const response = await fetch("/api/users");

  if (!response.ok) {
    throw new Error(`HTTP error: ${response.status}`);
  }

  return response.json();
}
```

# 🗁 Async and Timer Errors

Asynchronous callbacks execute after the surrounding synchronous code has finished.

A synchronous `try...catch` cannot catch exceptions thrown later inside a timer callback.

Incorrect:

```javascript
try {
  setTimeout(() => {
    throw new Error("Timer failed");
  }, 1000);
} catch (error) {
  console.error(error);
}
```

The error occurs outside the original synchronous `try...catch` execution.

For callback-based operations, handle errors inside the callback or use an asynchronous API that exposes failures through a Promise.

```javascript
setTimeout(() => {
  try {
    throw new Error("Timer failed");
  } catch (error) {
    console.error(error.message);
  }
}, 1000);
```

# 🗁 Logical Errors

Logical errors occur when code executes without throwing an exception but produces an incorrect result.

Example:

```javascript
function calculateArea(width, height) {
  return width + height;
}

console.log(calculateArea(10, 5));
```

The code executes successfully, but the formula is incorrect.

Correct:

```javascript
function calculateArea(width, height) {
  return width * height;
}

console.log(calculateArea(10, 5));
```

Output:

```text
50
```

Logical errors are often harder to detect because JavaScript does not necessarily report them.

Common causes:

* Incorrect formulas.
* Wrong comparison operators.
* Incorrect conditions.
* Unexpected operator precedence.
* Incorrect assumptions about data.
* Off-by-one errors.

# 🗁 Off-by-One Errors

Off-by-one errors occur when loop boundaries or indexes are incorrect.

Incorrect:

```javascript
const numbers = [10, 20, 30];

for (let i = 0; i <= numbers.length; i++) {
  console.log(numbers[i]);
}
```

The final iteration accesses an index that does not exist.

Correct:

```javascript
const numbers = [10, 20, 30];

for (let i = 0; i < numbers.length; i++) {
  console.log(numbers[i]);
}
```

# 🗁 Equality Errors

JavaScript provides strict equality (`===`) and loose equality (`==`).

Incorrect assumptions about type coercion can produce unexpected results.

```javascript
console.log(5 == "5");
console.log(5 === "5");
```

Output:

```text
true
false
```

Strict equality checks both type and value without performing ordinary implicit type coercion.

Prefer `===` and `!==` for predictable comparisons.

# 🗁 NaN Errors

`NaN` represents an invalid numeric result.

Example:

```javascript
const result = Number("Hello");

console.log(result);
```

Output:

```text
NaN
```

An important characteristic is that `NaN` is not equal to itself.

```javascript
console.log(NaN === NaN);
```

Output:

```text
false
```

Use `Number.isNaN()` to check whether a value is `NaN`.

```javascript
const value = Number("Hello");

if (Number.isNaN(value)) {
  console.log("Invalid number");
}
```

# 🗁 Type Coercion Errors

Implicit type conversion can produce unexpected results.

```javascript
console.log("10" + 5);
console.log("10" - 5);
```

Output:

```text
105
5
```

The `+` operator performs string concatenation when a string operand is involved, while subtraction performs numeric conversion.

Use explicit conversion when appropriate:

```javascript
const value = "10";

const number = Number(value);

console.log(number + 5);
```

# 🗁 DOM Errors

DOM-related errors commonly occur when JavaScript interacts with elements that do not exist or are not yet available.

**Element not found**

```javascript
const button = document.querySelector("#submit");

button.addEventListener("click", () => {
  console.log("Clicked");
});
```

If no matching element exists, `button` is `null`, and calling `addEventListener()` produces a `TypeError`.

Correct:

```javascript
const button = document.querySelector("#submit");

if (button) {
  button.addEventListener("click", () => {
    console.log("Clicked");
  });
}
```

**Script executes before the DOM is ready**

If a script executes before its target elements have been parsed, selectors may return `null`.

One solution is to use `defer`:

```html
<script src="app.js" defer></script>
```

Another approach is to wait for the DOM:

```javascript
document.addEventListener("DOMContentLoaded", () => {
  const button = document.querySelector("#submit");

  button?.addEventListener("click", () => {
    console.log("Clicked");
  });
});
```

# 🗁 Module Errors

JavaScript modules can produce errors related to imports, exports, and module configuration.

**Incorrect import path**

```javascript
import { greet } from "./greet.js";
```

If the referenced module cannot be resolved, the application may fail during module loading.

Verify the file path, filename, extension, and case sensitivity.

**Importing a nonexistent export**

```javascript
import { username } from "./user.js";
```

If `username` is not exported by the module, module linking fails.

**Using import outside a module context**

```javascript
import { greet } from "./greet.js";
```

This syntax requires a module context.

For browser applications:

```html
<script type="module" src="app.js"></script>
```

For Node.js, configure the project appropriately for ECMAScript modules.

# 🗁 Browser Security Errors

Browser security restrictions can prevent certain operations.

Common examples include:

* CORS restrictions.
* Mixed content.
* Blocked popups.
* Insecure resource loading.
* Cross-origin frame restrictions.
* Restricted access to browser APIs.

For example, a frontend application may attempt to request data from a different origin without the server providing appropriate CORS headers.

```javascript
fetch("https://api.example.com/users")
  .then(response => response.json())
  .catch(error => {
    console.error("Request failed:", error);
  });
```

Inspect the browser Console and Network panels to identify the actual failure.

CORS permissions must generally be configured on the server that provides the resource.

# 🗁 Common Error Reference

| Error | Typical Cause |
| --- | --- |
| `SyntaxError` | Invalid syntax or malformed JSON |
| `ReferenceError` | Accessing an unavailable identifier |
| `TypeError` | Performing an unsupported operation |
| `RangeError` | Value outside an acceptable range |
| `URIError` | Invalid URI encoding or decoding |
| `EvalError` | Legacy error type associated with `eval()` |
| `AggregateError` | Multiple errors grouped together |

Some common mistakes, such as incorrect formulas or invalid array indexes, do not necessarily throw exceptions.

# 🗁 Debugging Workflow

When an error occurs, follow a consistent debugging process.

* Reproduce the problem.
* Read the complete error message.
* Identify the error type.
* Locate the source file and line number.
* Inspect the relevant variables.
* Check function arguments and return values.
* Verify assumptions about data types and structures.
* Examine the call stack.
* Inspect network requests if the problem involves APIs.
* Apply a targeted fix.
* Test the affected behavior again.

A useful example:

```javascript
function calculateDiscount(price, discount) {
  if (typeof price !== "number") {
    throw new TypeError("Price must be a number");
  }

  if (typeof discount !== "number") {
    throw new TypeError("Discount must be a number");
  }

  if (discount < 0 || discount > 100) {
    throw new RangeError("Discount must be between 0 and 100");
  }

  return price - (price * discount / 100);
}

try {
  console.log(calculateDiscount(1000, 20));
} catch (error) {
  console.error(error.name, error.message);
}
```

Output:

```text
800
```

# 🗁 Best Practices

* Read the complete error message before changing code.
* Identify whether the problem is a syntax, runtime, or logical error.
* Validate external data before using it.
* Use strict equality for predictable comparisons.
* Handle missing values explicitly.
* Use optional chaining when property access is legitimately optional.
* Handle Promise rejections.
* Check HTTP response status when using Fetch.
* Use browser DevTools to inspect runtime behavior.
* Avoid swallowing exceptions silently.
* Write tests for important business rules.
* Fix the underlying cause instead of hiding the symptom.

# 🗁 Key Takeaways

* JavaScript errors can originate from syntax problems, invalid operations, unexpected values, or incorrect logic.
* `SyntaxError` identifies invalid syntax or parsing input.
* `ReferenceError` identifies unresolved identifiers.
* `TypeError` identifies unsupported operations.
* `RangeError` identifies out-of-range values.
* `URIError` identifies invalid URI sequences.
* Logical errors may not produce any exception.
* Asynchronous failures require appropriate asynchronous error handling.
* DOM and module errors often involve missing elements or incorrect configuration.
* Browser DevTools help identify and investigate errors.
* A consistent debugging workflow makes troubleshooting faster and more reliable.
