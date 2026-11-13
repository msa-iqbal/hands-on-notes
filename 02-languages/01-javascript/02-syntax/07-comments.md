# JavaScript Comments

Comments are non-executable text written inside source code to explain its purpose, describe complex logic, provide useful information, or temporarily disable code during development.

JavaScript engines ignore comments during normal program execution.

Comments help developers understand code, maintain projects, collaborate with other developers, and document important implementation details.

JavaScript supports two primary types of comments:

* Single-line comments
* Multi-line comments

# 🗁 Single-Line Comments

Single-line comments begin with two forward slashes (`//`).

Everything after `//` on that line is treated as a comment.

**Syntax:**

```javascript
// This is a single-line comment
```

**Example:**

```javascript
// Store the user's name
const name = "Alice";

// Store the user's age
const age = 25;

// Display user information
console.log(name, age);
```

Single-line comments can also appear after executable code:

```javascript
const price = 100; // Product price
const quantity = 3; // Number of products

console.log(price * quantity);
```

# 🗁 Multi-Line Comments

Multi-line comments begin with `/*` and end with `*/`.

Everything between these markers is treated as a comment, including line breaks.

**Syntax:**

```javascript
/*
    This is a multi-line comment.
    It can span multiple lines.
*/
```

**Example:**

```javascript
/*
    Calculate the total price.

    Parameters:
    - price: Price of one product
    - quantity: Number of products

    Returns:
    - Total price
*/

function calculateTotal(price, quantity) {
    return price * quantity;
}

console.log(calculateTotal(100, 3));
```

Multi-line comments can also appear on a single line:

```javascript
const discount = 10; /* Discount percentage */
```

# 🗁 Commenting Out Code

Comments can temporarily disable JavaScript code without deleting it.

**Using Single-Line Comments:**

```javascript
const name = "Alice";

// console.log(name);
console.log("Program started");
```

**Using Multi-Line Comments:**

```javascript
/*
const firstName = "Alice";
const lastName = "Smith";

console.log(firstName, lastName);
*/

console.log("Program started");
```

Commenting out code is useful when debugging or testing alternative implementations.

However, large blocks of obsolete code should generally be removed rather than permanently kept inside comments. Version control systems such as Git preserve previous versions.

# 🗁 Comments Inside Expressions

Comments can appear between JavaScript tokens without changing the expression's meaning.

**Example:**

```javascript
const total = 10 /* first value */ + 20 /* second value */;

console.log(total);
```

Comments can also appear between operators and operands:

```javascript
const result = 10 +
    /* Add the second value */
    20;

console.log(result);
```

Comments should not split tokens in ways that change how the source code is parsed.

# 🗁 Nested Comments

JavaScript multi-line comments do not support nesting.

The first `*/` encountered terminates the comment.

**Incorrect:**

```javascript
/*
    Outer comment

    /*
        Inner comment
    */

    More outer comment
*/
```

The inner closing marker ends the outer comment unexpectedly. The remaining text may cause a syntax error.

**Correct Approach:**

Use single-line comments inside a multi-line comment:

```javascript
/*
    Outer documentation.

    // Additional explanation
    // More information
*/
```

Alternatively, restructure the comments so that multi-line comment markers are not nested.

# 🗁 Documentation Comments (JSDoc)

JSDoc is a documentation convention that uses specially formatted comments to describe JavaScript functions, classes, parameters, return values, and other program elements.

JSDoc comments begin with `/**` and end with `*/`.

**Example:**

```javascript
/**
 * Calculates the total price.
 *
 * @param {number} price - Price of one product.
 * @param {number} quantity - Number of products.
 * @returns {number} The calculated total.
 */
function calculateTotal(price, quantity) {
    return price * quantity;
}
```

Common JSDoc tags include:

| Tag | Purpose |
| --- | --- |
| `@param` | Documents a function parameter |
| `@returns` | Documents the return value |
| `@type` | Specifies a variable's type |
| `@typedef` | Defines a custom documentation type |
| `@property` | Documents an object property |
| `@example` | Provides usage examples |
| `@deprecated` | Marks an API as deprecated |
| `@throws` | Documents possible exceptions |
| `@async` | Identifies an asynchronous function |
| `@see` | References related documentation |

JSDoc is useful for generating API documentation and improving editor assistance.

# 🗁 JSDoc Function Documentation

**Documenting Parameters and Return Values:**

```javascript
/**
 * Adds two numbers.
 *
 * @param {number} a - First number.
 * @param {number} b - Second number.
 * @returns {number} Sum of both numbers.
 */
function add(a, b) {
    return a + b;
}
```

**Documenting Optional Parameters:**

```javascript
/**
 * Greets a user.
 *
 * @param {string} name - User's name.
 * @param {string} [greeting="Hello"] - Optional greeting.
 * @returns {string} Greeting message.
 */
function greet(name, greeting = "Hello") {
    return `${greeting}, ${name}!`;
}
```

**Documenting Object Parameters:**

```javascript
/**
 * Creates a user profile.
 *
 * @param {Object} user - User information.
 * @param {string} user.name - User's name.
 * @param {number} user.age - User's age.
 * @returns {Object} User profile.
 */
function createUser(user) {
    return {
        name: user.name,
        age: user.age
    };
}
```

# 🗁 TODO, FIXME, and NOTE Comments

Developers often use comment prefixes to identify work that needs attention.

These prefixes are conventions, not special JavaScript keywords.

**TODO**

Indicates work that should be completed later.

```javascript
// TODO: Add input validation.
function createUser(name) {
    return {
        name
    };
}
```

**FIXME**

Identifies code that needs correction.

```javascript
// FIXME: Handle negative quantities correctly.
function calculateTotal(price, quantity) {
    return price * quantity;
}
```

**NOTE**

Provides additional information about implementation details.

```javascript
// NOTE: Results are cached to avoid repeated database queries.
function getUser(id) {
    return cache.get(id);
}
```

Other commonly used prefixes include:

* `BUG` — Identifies a known defect.
* `HACK` — Identifies a temporary workaround.
* `OPTIMIZE` — Indicates a potential performance improvement.
* `REVIEW` — Requests additional code review.

Many IDEs and code editors can highlight these comments automatically.

# 🗁 Comments and Source Maps

Comments can contain special directives used by development tools.

One common example is the source map directive:

```javascript
//# sourceMappingURL=app.js.map
```

Source maps connect generated JavaScript files with their original source files.

They are useful when debugging bundled or transpiled applications.

Another example is a bundler-specific annotation:

```javascript
/* webpackChunkName: "user-module" */
```

Some bundlers recognize comments like this as configuration hints.

These comments are not standard JavaScript language features; their meaning depends on the tools processing the source code.

# 🗁 HTML Comments in JavaScript

JavaScript supports certain legacy HTML-like comment forms in script contexts for historical compatibility.

For example:

```javascript
<!-- This is a legacy comment form
console.log("Hello");
```

However, these forms are outdated and should not be used in modern JavaScript applications.

Use `//` or `/* ... */` instead.

# 🗁 Best Practices

* Write comments that explain why something is done rather than simply repeating what the code already says.
* Keep comments accurate and update them whenever the implementation changes.
* Use single-line comments for short explanations.
* Use multi-line comments for longer descriptions.
* Use JSDoc for public functions, classes, and reusable APIs.
* Use consistent prefixes such as `TODO`, `FIXME`, and `NOTE`.
* Avoid excessive comments that make code harder to read.
* Remove obsolete commented-out code.
* Avoid using comments as a replacement for clear variable and function names.
* Keep comments professional, concise, and meaningful.
* Never put passwords, API keys, tokens, or other secrets in source-code comments.

# 🗁 Common Mistakes

**Forgetting to Close a Multi-Line Comment**

Incorrect:

```javascript
/*
    This comment is not closed.

const value = 100;
```

The parser treats the remaining source as part of the comment.

Correct:

```javascript
/*
    This comment is properly closed.
*/

const value = 100;
```

**Incorrectly Nesting Multi-Line Comments**

Incorrect:

```javascript
/*
    Outer comment
    /* Inner comment */
*/
```

The first closing marker terminates the comment.

**Writing Comments That Repeat the Code**

Unhelpful:

```javascript
// Increment count by one
count++;
```

Better:

```javascript
// Track the number of successful login attempts.
count++;
```

The second comment provides context rather than repeating the operation.

**Leaving Outdated Comments**

Incorrect:

```javascript
// Uses the old authentication service.
const user = authenticateWithNewService(credentials);
```

The comment no longer describes the implementation.

Update or remove comments when code changes.

# 🗁 Summary

* Comments are ignored during normal JavaScript execution.
* `//` creates a single-line comment.
* `/* ... */` creates a multi-line comment.
* Comments can temporarily disable code.
* Multi-line comments do not support nesting.
* JSDoc uses `/** ... */` to document functions, classes, parameters, and return values.
* `TODO`, `FIXME`, and `NOTE` are common comment conventions.
* Source map directives and bundler annotations provide instructions to development tools.
* Good comments explain intent, decisions, and important implementation details.

# 🗁 Practice

**Exercise: Write Single-Line Comments**

Create a JavaScript program that:

* Declares a user's name.
* Declares the user's age.
* Calculates the user's birth year approximately.
* Displays the result.

Add meaningful single-line comments.

**Exercise: Write Multi-Line Comments**

Create a function named `calculateArea` that calculates the area of a rectangle.

Add a multi-line comment explaining its purpose, parameters, and return value.

**Exercise: Use JSDoc**

Document the following function using JSDoc:

```javascript
function calculateDiscount(price, percentage) {
    return price - (price * percentage / 100);
}
```

Include parameter types, parameter descriptions, and the return type.

**Exercise: Identify the Problem**

Explain why the following code does not behave as expected:

```javascript
/*
    This is the main comment.

    /*
        This is another comment.
    */

    const value = 100;
*/

console.log(value);
```

Rewrite it using valid JavaScript comments.

**Exercise: Improve the Comments**

Improve the following code by replacing unnecessary comments with meaningful explanations:

```javascript
// Create a variable
const users = [];

// Create a function
function addUser(user) {
    // Push user into array
    users.push(user);
}
```
