# JavaScript Comments

Comments are explanatory notes written inside source code to describe its purpose, behavior, or implementation details.

JavaScript engines ignore comments during normal program execution. They do not affect the program's output or behavior.

Comments are useful for documenting code, explaining complex logic, temporarily disabling code, and improving readability and maintainability.

JavaScript supports two main types of comments:

* Single-line comments
* Multi-line comments

# 🗁 Single-Line Comments

A single-line comment begins with two forward slashes (`//`).

Everything after `//` on that line is treated as a comment.

Syntax:

```javascript
// This is a single-line comment
```

Example:

```javascript
// Display a welcome message
console.log("Welcome to JavaScript!");
```

Output:

```text
Welcome to JavaScript!
```

The JavaScript engine ignores the comment and executes the `console.log()` statement.

# 🗁 Comments After Statements

You can write comments after executable code on the same line.

Example:

```javascript
const name = "Muhammad"; // Store the user's name

console.log(name); // Display the name
```

Output:

```text
Muhammad
```

The comment does not interfere with the statement.

# 🗁 Multi-Line Comments

A multi-line comment begins with `/*` and ends with `*/`.

Everything between these markers is treated as a comment, including line breaks.

Syntax:

```javascript
/*
This is a multi-line comment.
It can contain multiple lines.
*/
```

Example:

```javascript
/*
    This program calculates
    the sum of two numbers.
*/

const firstNumber = 10;
const secondNumber = 20;

const result = firstNumber + secondNumber;

console.log(result);
```

Output:

```text
30
```

Multi-line comments are useful when an explanation requires more than one line.

# 🗁 Comments Between Code Statements

Comments can appear before, between, or after JavaScript statements.

Example:

```javascript
// Declare the first number
const a = 10;

// Declare the second number
const b = 20;

// Calculate the sum
const sum = a + b;

// Display the result
console.log(sum);
```

Output:

```text
30
```

This approach makes the purpose of each operation easier to understand.

# 🗁 Temporarily Disabling Code

Comments can be used to prevent JavaScript statements from executing without deleting them.

This technique is commonly called commenting out code.

**Using single-line comments:**

```javascript
console.log("This statement executes.");

// console.log("This statement does not execute.");

console.log("This statement also executes.");
```

Output:

```text
This statement executes.
This statement also executes.
```

**Using multi-line comments:**

```javascript
console.log("Start");

/*
console.log("First disabled statement");
console.log("Second disabled statement");
console.log("Third disabled statement");
*/

console.log("End");
```

Output:

```text
Start
End
```

This technique is useful when temporarily disabling code during debugging or development.

# 🗁 JavaScript Comments Inside HTML

JavaScript comments and HTML comments use different syntax.

JavaScript single-line comment:

```javascript
// JavaScript comment
```

JavaScript multi-line comment:

```javascript
/*
JavaScript multi-line comment
*/
```

HTML comment:

```html
<!-- HTML comment -->
```

Example:

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>JavaScript Comments</title>
</head>
<body>

    <!-- This is an HTML comment -->

    <script>
        // This is a JavaScript comment

        /*
            This is another
            JavaScript comment
        */

        console.log("Hello, JavaScript!");
    </script>

</body>
</html>
```

Use HTML comment syntax for HTML markup and JavaScript comment syntax for JavaScript source code.

# 🗁 Nested Comments

JavaScript does not support directly nested multi-line comments.

Incorrect:

```javascript
/*
    Outer comment

    /*
        Inner comment
    */

    More outer comment
*/
```

The first `*/` closes the multi-line comment. The remaining text may then be interpreted as JavaScript code and cause a syntax error.

Instead, use single-line comments inside a multi-line comment:

```javascript
/*
    Outer comment

    // Inner explanation

    More outer comment
*/
```

# 🗁 Comments and Strings

Comment markers inside JavaScript strings are treated as ordinary characters.

Example:

```javascript
const message = "This is not a // comment";

console.log(message);
```

Output:

```text
This is not a // comment
```

Similarly:

```javascript
const text = "This is not a /* comment */";

console.log(text);
```

Output:

```text
This is not a /* comment */
```

JavaScript recognizes comments according to the language's lexical rules, not simply by searching for comment markers.

# 🗁 Best Practices for Writing Comments

Good comments explain information that may not be immediately obvious from the code.

**Explain why, not just what**

Avoid:

```javascript
// Add 10 to the total
total = total + 10;
```

A more useful comment explains the reason:

```javascript
// Include the fixed service charge in the final amount
total = total + 10;
```

**Use meaningful comments**

Comments should provide useful context rather than repeat obvious code.

Avoid:

```javascript
// Declare a variable
const age = 25;
```

Prefer:

```javascript
// Minimum age required for account registration
const minimumAge = 18;
```

**Keep comments updated**

Incorrect or outdated comments can be more confusing than having no comments.

When code changes, review its related comments as well.

**Avoid excessive comments**

Do not add comments to every statement. Use meaningful variable names and clear code structure to communicate intent.

**Document complex logic**

Explain important assumptions, business rules, algorithms, and non-obvious decisions.

Example:

```javascript
// Apply a 10% discount only when the order total reaches 1000.
function calculateDiscount(total) {
    if (total >= 1000) {
        return total * 0.1;
    }

    return 0;
}
```

# 🗁 Documentation Comments and JSDoc

JSDoc is a documentation syntax commonly used in JavaScript projects.

It uses specially formatted multi-line comments beginning with `/**`.

Example:

```javascript
/**
 * Calculates the sum of two numbers.
 *
 * @param {number} a - The first number.
 * @param {number} b - The second number.
 * @returns {number} The calculated sum.
 */
function add(a, b) {
    return a + b;
}
```

Common JSDoc tags:

| Tag | Purpose |
| --- | --- |
| `@param` | Documents a function parameter |
| `@returns` | Documents a return value |
| `@type` | Documents a variable's type |
| `@example` | Provides usage examples |
| `@throws` | Documents possible exceptions |
| `@deprecated` | Marks an API as deprecated |
| `@description` | Provides a description |

JSDoc comments can improve documentation, editor IntelliSense, and static analysis.

# 🗁 Single-Line vs Multi-Line Comments

| Feature | Single-line | Multi-line |
| --- | --- | --- |
| Syntax | `//` | `/* ... */` |
| Multiple lines | Requires a comment marker on each line | Supports multiple lines inside one comment |
| Common use | Short explanations | Longer explanations |
| Inline usage | Yes | Yes |
| Documentation | Possible | Commonly used with JSDoc |

# 🗁 Common Mistakes

**Forgetting to close a multi-line comment**

Incorrect:

```javascript
/*
This comment is never closed.

console.log("Hello");
```

Correct:

```javascript
/*
This comment is properly closed.
*/

console.log("Hello");
```

**Using HTML comment syntax in JavaScript**

Incorrect:

```javascript
<!-- This is not a JavaScript comment -->
console.log("Hello");
```

Correct:

```javascript
// This is a JavaScript comment
console.log("Hello");
```

**Assuming commented code still executes**

```javascript
// console.log("Hello");
```

This statement does not execute because it is commented out.

# 🗁 Key Takeaways

* Comments explain JavaScript code and improve readability.
* JavaScript supports single-line and multi-line comments.
* `//` creates a single-line comment.
* `/* ... */` creates a multi-line comment.
* JavaScript engines ignore comments during normal execution.
* Comments can temporarily disable code.
* Comment markers inside strings are treated as ordinary characters.
* JavaScript does not support directly nested multi-line comments.
* JSDoc uses `/** ... */` to document functions, parameters, return values, and other code elements.
* Good comments explain important reasoning, assumptions, and non-obvious behavior.
