# JavaScript Introduction

JavaScript is a high-level, dynamically typed programming language used to develop interactive and dynamic applications. It is one of the fundamental technologies of web development and can execute in browsers, servers, and other JavaScript runtimes.

JavaScript enables developers to implement application logic, manipulate web pages, process data, communicate with APIs, and respond to user interactions.

# 🗁 What Is JavaScript?

JavaScript is a programming language originally developed to make web pages interactive.

Today, it is a general-purpose language used across multiple development environments.

JavaScript supports several programming paradigms:

* **Procedural programming:** Organizing code into procedures and sequential instructions.
* **Object-oriented programming:** Organizing code around objects and their behavior.
* **Functional programming:** Using functions as reusable building blocks and composing operations.
* **Event-driven programming:** Executing code in response to events.
* **Asynchronous programming:** Managing operations that do not complete immediately.

Example:

```javascript
const user = {
    name: "John",
    role: "Developer"
};

function greet(person) {
    return `Hello, ${person.name}!`;
}

console.log(greet(user));
```

Output:

```text
Hello, John!
```

This example demonstrates variables, objects, functions, template literals, and console output.

# 🗁 History of JavaScript

JavaScript was created in 1995 by Brendan Eich while working at Netscape Communications.

Its development and standardization evolved through several important milestones.

| Year        | Milestone                                                          |
| ----------- | ------------------------------------------------------------------ |
| 1995        | JavaScript was created by Brendan Eich.                            |
| 1995        | Initially introduced under the name Mocha.                         |
| 1995        | Renamed LiveScript and subsequently JavaScript.                    |
| 1996        | Microsoft introduced JScript for Internet Explorer.                |
| 1997        | ECMAScript was standardized as ECMA-262.                           |
| 1999        | ECMAScript 3 was released.                                         |
| 2009        | ECMAScript 5 introduced significant language improvements.         |
| 2015        | ECMAScript 2015 (ES6) introduced major modern JavaScript features. |
| 2016 onward | ECMAScript releases continued with regular annual updates.         |

# 🗁 JavaScript vs Java

Despite their similar names, JavaScript and Java are different programming languages.

They have different language designs, execution environments, type systems, and development ecosystems.

| Feature | JavaScript | Java |
| --- | --- | --- |
| Language type | Dynamically typed | Statically typed |
| Primary execution | JavaScript engine | Java Virtual Machine (JVM) |
| Compilation | Commonly executed through an engine with JIT compilation | Compiled to bytecode and executed by the JVM |
| Object model | Prototype-based | Class-based |
| Variables | `let`, `const`, `var` | Explicit type declarations |
| Common use | Web development, backend, scripting | Enterprise, backend, Android, desktop |
| File extension | `.js` | `.java` |

Example JavaScript:

```javascript
let message = "Hello";
message = 100;
```

Example Java:

```java
String message = "Hello";
message = 100; // Compilation error
```

JavaScript allows a variable to hold values of different types during execution. Java generally requires a variable to retain its declared type.

# 🗁 Why JavaScript Is Important

JavaScript is important because it provides a common programming language across many application environments.

**Frontend development**

JavaScript powers interactive browser applications.

Examples:

* Dynamic user interfaces
* Form validation
* Interactive navigation
* Data visualization
* API communication
* Single-page applications

**Backend development**

JavaScript can execute on servers using runtimes such as Node.js.

Examples:

* REST APIs
* Authentication systems
* Database operations
* Real-time applications
* Background jobs
* Microservices

**Full-stack development**

JavaScript can be used on both the frontend and backend.

This allows developers to share language knowledge, validation logic, data structures, and sometimes code across application layers.

**Other applications**

JavaScript is also used for:

* Desktop applications
* Mobile applications
* Browser extensions
* Automation scripts
* Testing frameworks
* Command-line tools
* Development tooling

# 🗁 How JavaScript Works

JavaScript source code is executed by a JavaScript engine.

A simplified execution process looks like this:

```text
JavaScript Source Code
         |
         v
    JavaScript Engine
         |
         v
      Parsing
         |
         v
   Intermediate Representation
         |
         v
   Interpretation / JIT Compilation
         |
         v
    Machine Instructions
         |
         v
       Execution
```

Modern JavaScript engines optimize code using techniques such as Just-In-Time (JIT) compilation.

Examples of JavaScript engines:

| Engine | Used by |
| --- | --- |
| V8 | Google Chrome, Node.js, and other environments |
| SpiderMonkey | Mozilla Firefox |
| JavaScriptCore | Apple Safari |

The exact implementation and optimization process differ between engines.

# 🗁 JavaScript in the Browser

Browsers provide JavaScript engines and browser APIs.

The engine executes JavaScript, while browser APIs provide functionality for interacting with the browser environment.

Examples of browser APIs:

* DOM API
* Fetch API
* Storage API
* History API
* Location API
* Clipboard API
* Notification API

Example:

```javascript
document.querySelector("h1").textContent = "Hello, JavaScript!";
```

This code selects the first `h1` element and changes its text content.

The `document` object is provided by the browser's DOM API, not by the core ECMAScript language.

# 🗁 JavaScript Outside the Browser

JavaScript is not limited to web browsers.

Node.js provides a runtime environment that allows JavaScript to execute on operating systems and servers.

Example:

```javascript
console.log("Running JavaScript with Node.js");
```

Run the program using:

```bash
node app.js
```

Node.js provides additional APIs for:

* File system operations
* Network communication
* HTTP servers
* Process management
* Operating system interaction
* Streams and buffers

Other runtimes, including Deno and Bun, also execute JavaScript outside browsers.

# 🗁 JavaScript Language Characteristics

JavaScript has several important characteristics.

**High-level language**

Developers work with abstractions such as objects, arrays, functions, and strings instead of directly managing machine instructions.

**Dynamically typed**

Variable types are determined by the values assigned at runtime.

```javascript
let value = 100;

value = "Hello";

value = true;
```

**Multi-paradigm**

JavaScript supports procedural, object-oriented, and functional programming styles.

**Prototype-based inheritance**

Objects can inherit properties and methods from other objects through the prototype chain.

**First-class functions**

Functions can be assigned to variables, passed as arguments, and returned from other functions.

```javascript
const greet = function () {
    console.log("Hello!");
};

function execute(callback) {
    callback();
}

execute(greet);
```

**Event-driven**

JavaScript applications frequently respond to events such as clicks, keyboard input, network responses, and timers.

**Asynchronous capabilities**

JavaScript supports asynchronous operations through callbacks, promises, and async/await.

# 🗁 JavaScript Standards

JavaScript follows the ECMAScript language specification.

Important organizations and concepts include:

* **ECMA International:** The standards organization responsible for publishing ECMAScript.
* **TC39:** The technical committee responsible for developing ECMAScript.
* **ECMA-262:** The specification defining the ECMAScript language.
* **ECMAScript:** The standardized language specification implemented by JavaScript engines.

JavaScript environments may support different language features depending on their engine versions.

# 🗁 JavaScript Example

The following example combines variables, functions, conditions, and template literals.

```javascript
const username = "Muhammad";
const age = 25;

function checkEligibility(name, userAge) {
    if (userAge >= 18) {
        return `${name} is eligible.`;
    }

    return `${name} is not eligible.`;
}

const result = checkEligibility(username, age);

console.log(result);
```

Output:

```text
Muhammad is eligible.
```

# 🗁 Advantages of JavaScript

* Runs in all major modern web browsers.
* Supports frontend and backend development.
* Has a large ecosystem of libraries and frameworks.
* Supports multiple programming paradigms.
* Integrates naturally with HTML and CSS.
* Provides asynchronous programming capabilities.
* Has extensive community support.
* Supports rapid application development.

# 🗁 Limitations of JavaScript

* Dynamic typing can introduce runtime type-related errors.
* Browser environments impose security and execution restrictions.
* Different runtimes may provide different APIs.
* Large applications require disciplined architecture and maintainable code.
* Poorly optimized JavaScript can negatively affect application performance.
* JavaScript's implicit type coercion can sometimes produce unexpected results.

# 🗁 Important Concepts to Understand

Before moving into more advanced topics, understand these fundamental terms:

| Concept | Description |
| --- | --- |
| JavaScript engine | Software that executes JavaScript code |
| Runtime environment | Environment providing an engine and additional APIs |
| ECMAScript | Standard specification for the language |
| Variable | Named storage for a value |
| Data type | Classification of a value |
| Function | Reusable block of executable code |
| Object | Collection of properties and values |
| Event | An occurrence that application code can respond to |
| DOM | Programming interface for HTML and XML documents |
| API | Interface through which software components communicate |
| JIT compilation | Runtime compilation used to optimize execution |

# 🗁 Official References

* [MDN JavaScript Guide](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide)
* [MDN JavaScript Reference](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference)
* [ECMAScript Specification](https://tc39.es/ecma262/)
* [TC39 Official Website](https://tc39.es/)
* [Node.js Documentation](https://nodejs.org/docs/latest/api/)
