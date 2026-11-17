# JavaScript Template Literals

Template literals are a modern JavaScript feature that provides a convenient way to create strings.

Unlike traditional strings enclosed in single or double quotes, template literals use backticks (`` ` ``).

They support several powerful features:

* String interpolation.
* Multiline strings.
* Embedded expressions.
* Dynamic string construction.
* Tagged templates.
* Nested template literals.

Template literals were introduced in ECMAScript 2015 (ES6).

# 🗁 Template Literal Syntax

Template literals use backticks instead of single or double quotation marks.

```javascript
const message = `Hello, JavaScript!`;

console.log(message);
// Hello, JavaScript!
```

Compare traditional strings:

```javascript
const first = "Hello, JavaScript!";
const second = 'Hello, JavaScript!';
const third = `Hello, JavaScript!`;
```

All three create string values, but template literals provide additional capabilities.

# 🗁 String Interpolation

String interpolation allows expressions and variables to be embedded directly inside a string.

It uses the `${expression}` syntax.

### 🖆 Embedding Variables

```javascript
const name = "Muhammad";
const message = `Hello, ${name}!`;

console.log(message);
// Hello, Muhammad!
```

Traditional concatenation:

```javascript
const name = "Muhammad";

const message = "Hello, " + name + "!";

console.log(message);
// Hello, Muhammad!
```

Template literals make dynamic strings easier to read and maintain.

### 🖆 Embedding Multiple Variables

```javascript
const firstName = "Muhammad";
const lastName = "Iqbal";

const fullName = `${firstName} ${lastName}`;

console.log(fullName);
// Muhammad Iqbal
```

### 🖆 Embedding Expressions

Template literals can evaluate JavaScript expressions inside `${}`.

```javascript
const a = 10;
const b = 20;

const result = `The sum is ${a + b}`;

console.log(result);
// The sum is 30
```

Other expression examples:

```javascript
const price = 100;
const quantity = 3;

console.log(`Total: ${price * quantity}`);
// Total: 300

console.log(`Is expensive: ${price > 50}`);
// Is expensive: true
```

### 🖆 Calling Functions Inside Template Literals

Functions can be called inside interpolation expressions.

```javascript
function greet(name) {
    return `Hello, ${name}`;
}

const message = `${greet("Alice")}!`;

console.log(message);
// Hello, Alice!
```

### 🖆 Using Conditional Expressions

```javascript
const age = 20;

const message = `You are ${age >= 18 ? "an adult" : "a minor"}.`;

console.log(message);
// You are an adult.
```

### 🖆 Embedding Object Properties

```javascript
const user = {
    name: "Alice",
    role: "Developer"
};

const message = `${user.name} is a ${user.role}.`;

console.log(message);
// Alice is a Developer.
```

### 🖆 Embedding Array Values

```javascript
const technologies = ["HTML", "CSS", "JavaScript"];

const message = `Learning ${technologies.join(", ")}`;

console.log(message);
// Learning HTML, CSS, JavaScript
```

# 🗁 Multiline Strings

Template literals support multiline strings without requiring newline escape sequences.

### 🖆 Traditional String Concatenation

```javascript
const message = "Hello\n" +
    "Welcome to JavaScript\n" +
    "Happy coding!";

console.log(message);
```

### 🖆 Using Template Literals

```javascript
const message = `Hello
Welcome to JavaScript
Happy coding!`;

console.log(message);
```

The line breaks are preserved in the resulting string.

### 🖆 Multiline HTML Templates

Template literals are useful for constructing HTML strings.

```javascript
const title = "JavaScript";
const description = "A programming language";

const html = `
    <article>
        <h2>${title}</h2>
        <p>${description}</p>
    </article>
`;

console.log(html);
```

The result is a multiline string containing the generated HTML.

When inserting untrusted user input into HTML, escape it or use safe DOM APIs rather than inserting it directly into an HTML string.

# 🗁 Nested Template Literals

A template literal can contain another template literal inside an interpolation expression.

### 🖆 Basic Nested Template

```javascript
const user = {
    name: "Alice",
    age: 25
};

const message = `User information: ${`Name: ${user.name}, Age: ${user.age}`}`;

console.log(message);
// User information: Name: Alice, Age: 25
```

### 🖆 Nested Template with Conditional Logic

```javascript
const users = [
    { name: "Alice", active: true },
    { name: "Bob", active: false }
];

const result = users.map(user => `
    <div>
        <strong>${user.name}</strong>
        <span>${user.active ? "Active" : "Inactive"}</span>
    </div>
`);

console.log(result);
```

Nested templates are useful for dynamic content generation, but excessive nesting can make code difficult to read.

# 🗁 Escaping Characters

Template literals support escape sequences.

### 🖆 Escaping Backticks

Use a backslash to include a literal backtick inside a template literal.

```javascript
const message = `JavaScript uses \`backticks\` for template literals.`;

console.log(message);
// JavaScript uses `backticks` for template literals.
```

### 🖆 Escaping Dollar Signs

To display `${` as ordinary text instead of starting interpolation, escape the dollar sign.

```javascript
const message = `Use \${variable} to interpolate a value.`;

console.log(message);
// Use ${variable} to interpolate a value.
```

### 🖆 Newline Escape Sequence

Although template literals support actual line breaks, you can also use escape sequences.

```javascript
const message = `Hello\nWorld`;

console.log(message);
// Hello
// World
```

### 🖆 Common Escape Sequences

| Escape Sequence | Meaning |
| --- | --- |
| `\n` | New line |
| `\t` | Horizontal tab |
| `\r` | Carriage return |
| `\b` | Backspace |
| `\f` | Form feed |
| `\v` | Vertical tab |
| `\\` | Backslash |
| `\`` | Backtick |
| `\$` | Literal dollar sign when followed by `{` to avoid interpolation |

# 🗁 Tagged Template Literals

A tagged template literal allows a function to process a template literal's contents.

Instead of evaluating the template literal into one ordinary string before calling the function, JavaScript passes the function:

* An array of literal string segments.
* The evaluated values of the interpolation expressions.

### 🖆 Basic Tagged Template

```javascript
function highlight(strings, ...values) {
    console.log(strings);
    console.log(values);
}

const name = "Alice";
const role = "Developer";

highlight`Name: ${name}, Role: ${role}`;
```

The `strings` array contains the literal portions of the template.

The `values` array contains the evaluated interpolation values.

Conceptually, the arguments are:

```javascript
strings = [
    "Name: ",
    ", Role: ",
    ""
];

values = [
    "Alice",
    "Developer"
];
```

### 🖆 Building a Tagged Template Function

```javascript
function format(strings, ...values) {
    let result = "";

    for (let index = 0; index < strings.length; index++) {
        result += strings[index];

        if (index < values.length) {
            result += String(values[index]).toUpperCase();
        }
    }

    return result;
}

const name = "Alice";
const role = "developer";

const message = format`Name: ${name}, Role: ${role}`;

console.log(message);
// Name: ALICE, Role: DEVELOPER
```

The tag function controls how interpolation values are processed.

### 🖆 Using `raw` with Tagged Templates

Tagged template functions receive a `raw` property on the strings array.

It contains the original literal segments before escape sequences are interpreted.

```javascript
function showStrings(strings) {
    console.log(strings[0]);
    console.log(strings.raw[0]);
}

showStrings`Hello\nWorld`;
```

The ordinary segment contains an actual newline, while the raw segment contains the characters `\` and `n`.

### 🖆 The `String.raw()` Tag

JavaScript provides the built-in `String.raw()` tag for accessing template literal content without interpreting escape sequences.

```javascript
const message = String.raw`Hello\nWorld`;

console.log(message);
// Hello\nWorld
```

This is useful when constructing strings where backslashes should be preserved.

For example:

```javascript
const pattern = String.raw`\d+\.\d+`;

console.log(pattern);
// \d+\.\d+
```

This can be convenient when writing regular expression patterns as strings.

# 🗁 Dynamic HTML Generation

Template literals can construct dynamic HTML strings.

### 🖆 Generating HTML from Objects

```javascript
const user = {
    name: "Alice",
    email: "alice@example.com",
    role: "Developer"
};

const html = `
    <article class="user">
        <h2>${user.name}</h2>
        <p>${user.email}</p>
        <span>${user.role}</span>
    </article>
`;

console.log(html);
```

### 🖆 Generating HTML from Arrays

```javascript
const users = [
    { name: "Alice", role: "Developer" },
    { name: "Bob", role: "Designer" },
    { name: "Charlie", role: "Manager" }
];

const html = `
    <ul>
        ${users.map(user => `
            <li>
                <strong>${user.name}</strong>
                <span>${user.role}</span>
            </li>
        `).join("")}
    </ul>
`;

console.log(html);
```

The `map()` method creates an array of HTML strings, while `join("")` combines them into one string.

### 🖆 Rendering HTML in the DOM

```javascript
const users = [
    { name: "Alice", role: "Developer" },
    { name: "Bob", role: "Designer" }
];

const container = document.querySelector("#users");

container.innerHTML = `
    <ul>
        ${users.map(user => `
            <li>${user.name} - ${user.role}</li>
        `).join("")}
    </ul>
`;
```

For untrusted data, do not interpolate user-controlled values directly into `innerHTML`. Prefer DOM creation methods such as `document.createElement()` and assign user-provided text through `textContent`.

# 🗁 Template Literals and Functions

Template literals work naturally with functions that return formatted strings.

### 🖆 Creating a Reusable HTML Template

```javascript
function createUserCard(user) {
    return `
        <article class="user-card">
            <h2>${user.name}</h2>
            <p>${user.email}</p>
        </article>
    `;
}

const user = {
    name: "Alice",
    email: "alice@example.com"
};

console.log(createUserCard(user));
```

### 🖆 Creating a Message Formatter

```javascript
function createMessage(name, notificationCount) {
    return `
        Hello, ${name}!
        You have ${notificationCount} new notifications.
    `;
}

console.log(createMessage("Alice", 5));
```

# 🗁 Template Literals vs String Concatenation

Both approaches can create dynamic strings.

| Feature | String Concatenation | Template Literals |
| --- | --- | --- |
| Syntax | `+` operator | Backticks |
| Variable interpolation | Manual | `${}` |
| Multiline strings | Requires escape sequences or concatenation | Native support |
| Embedded expressions | Possible through concatenation | Directly supported |
| Nested templates | Less convenient | Supported |
| Tagged templates | Not supported | Supported |
| Readability for complex strings | Often lower | Often higher |

### 🖆 String Concatenation

```javascript
const name = "Alice";
const age = 25;

const message = "Name: " + name + ", Age: " + age;
```

### 🖆 Template Literal

```javascript
const name = "Alice";
const age = 25;

const message = `Name: ${name}, Age: ${age}`;
```

Template literals are generally easier to read when constructing strings containing multiple dynamic values.

# 🗁 Common Mistakes

### 🖆 Using Quotes Instead of Backticks

Incorrect:

```javascript
const name = "Alice";

const message = "Hello, ${name}";

console.log(message);
// Hello, ${name}
```

Correct:

```javascript
const name = "Alice";

const message = `Hello, ${name}`;

console.log(message);
// Hello, Alice
```

Interpolation requires backticks.

### 🖆 Forgetting the Dollar Sign

Incorrect:

```javascript
const name = "Alice";

const message = `{name}`;

console.log(message);
// {name}
```

Correct:

```javascript
const message = `Hello, ${name}`;
```

### 🖆 Forgetting Curly Braces

Incorrect:

```javascript
const name = "Alice";

const message = `$name`;
```

Correct:

```javascript
const message = `${name}`;
```

### 🖆 Confusing Literal Text with Expressions

```javascript
const price = 100;

const first = `Price: ${price}`;

// Literal expression syntax as text
const second = `Price: \${price}`;

console.log(first);
// Price: 100

console.log(second);
// Price: ${price}
```

### 🖆 Assuming Template Literals Automatically Escape HTML

Incorrect:

```javascript
const username = "<img src=x onerror=alert(1)>";

const html = `<div>${username}</div>`;
```

Template literals perform interpolation, not HTML sanitization.

For untrusted content, use safe DOM APIs or an appropriate HTML escaping and sanitization strategy.

### 🖆 Confusing Tagged Templates with Ordinary Function Calls

Ordinary function call:

```javascript
function format(value) {
    return value;
}

console.log(format(`Hello ${"Alice"}`));
// Hello Alice
```

Tagged template:

```javascript
function format(strings, ...values) {
    return { strings, values };
}

console.log(format`Hello ${"Alice"}`);
```

A tagged template passes literal segments and interpolation values separately to the tag function.

# 🗁 Best Practices

* Use backticks when constructing strings with dynamic values.
* Prefer template literals over complicated string concatenation.
* Use `${}` for variables and expressions.
* Use multiline template literals for readable text templates.
* Keep nested template literals simple.
* Extract complex formatting logic into reusable functions.
* Use tagged templates when custom interpolation processing is needed.
* Use `String.raw()` when escape sequences should remain uninterpreted.
* Never assume template literals sanitize HTML.
* Prefer safe DOM APIs when rendering untrusted content.
* Remember that template literals produce strings unless used as tagged templates.

# 🗁 Summary

Template literals are an essential JavaScript feature for creating readable and dynamic strings.

Key points:

* Template literals use backticks.
* `${}` enables variable and expression interpolation.
* Multiline strings are supported natively.
* Nested template literals are possible.
* Escape sequences can be used.
* Tagged templates allow custom processing of literal segments and interpolation values.
* `String.raw()` preserves raw template content.
* Template literals are useful for generating messages, HTML strings, and dynamic content.
* Interpolation does not automatically sanitize HTML.

**Remember:** Template literals make dynamic string construction cleaner and more expressive. Use them whenever they improve readability, especially when combining text, variables, and expressions.
