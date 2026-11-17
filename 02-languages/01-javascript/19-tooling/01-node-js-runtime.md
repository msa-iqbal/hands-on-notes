# Node.js Runtime

**Node.js** is an open-source, cross-platform JavaScript runtime environment that allows developers to execute JavaScript outside web browsers.

It is built on the V8 JavaScript engine and provides additional APIs for interacting with the operating system, file system, network, processes, and other system resources.

Node.js makes it possible to use JavaScript for:

* Backend development.
* REST APIs and web servers.
* Command-line applications.
* Real-time applications.
* Microservices.
* Build tools and automation.
* Development tooling.
* Server-side rendering.

Example:

```javascript
// hello.js

console.log("Hello from Node.js!");
```

Execute the file:

```bash
node hello.js
```

Output:

```text
Hello from Node.js!
```

# 🗁 JavaScript vs Node.js

JavaScript is a programming language, while Node.js is a runtime environment that executes JavaScript outside the browser.

| JavaScript in Browser | JavaScript in Node.js |
| --- | --- |
| Executes inside a browser | Executes outside the browser |
| Uses browser APIs | Uses Node.js APIs |
| Can access the DOM | Does not provide a browser DOM by default |
| Uses `window` as a browser global | Uses `globalThis` as the universal global object |
| Uses browser networking APIs | Provides networking APIs such as HTTP and TCP |
| Uses browser storage APIs | Can access files and operating-system resources |

JavaScript language features such as variables, functions, classes, promises, and modules are available in both environments, subject to runtime and version support.

# 🗁 Node.js Architecture

Node.js uses several important technologies to execute JavaScript and handle asynchronous operations.

<AsyncImage query="Node.js runtime architecture diagram V8 event loop libuv thread pool operating system" aspectRatio="16:9" width="100%" />

The main components include:

**V8 JavaScript Engine**

V8 executes JavaScript code. It compiles JavaScript into machine code using techniques such as Just-In-Time (JIT) compilation.

**Node.js Core APIs**

Node.js provides built-in modules for interacting with system resources.

Examples:

* `fs` — File system operations.
* `http` — HTTP server and client functionality.
* `path` — File path utilities.
* `os` — Operating system information.
* `crypto` — Cryptographic functionality.
* `process` — Process information and control.

**libuv**

libuv is a cross-platform library responsible for important asynchronous infrastructure, including:

* Event loop implementation.
* Asynchronous I/O coordination.
* Thread pool management.
* Operating-system event notification integration.

**Event Loop**

The event loop coordinates asynchronous callbacks and other scheduled work, allowing Node.js to handle many I/O operations without blocking the JavaScript execution thread.

**Operating System**

The operating system provides lower-level capabilities such as file access, networking, processes, and system resources.

# 🗁 How Node.js Executes JavaScript

A simplified execution flow:

<AsyncImage query="Node.js JavaScript execution flow V8 event loop libuv asynchronous operations architecture diagram" aspectRatio="16:9" width="100%"/>

* Node.js loads the JavaScript application.
* V8 parses and executes JavaScript.
* Synchronous operations execute on the JavaScript thread.
* Asynchronous operations are delegated to the appropriate runtime or operating-system facilities.
* libuv coordinates asynchronous operations and its thread pool where applicable.
* Completed operations schedule callbacks or promise continuations.
* The event loop processes scheduled work.

Not every asynchronous operation uses the thread pool. Network I/O generally relies on operating-system mechanisms, while many file-system, DNS, and cryptographic operations use libuv's thread pool.

# 🗁 Installing Node.js

Node.js can be installed on Windows, Linux, and macOS.

The official website provides installers and download options:

<https://nodejs.org/>

There are two main release channels:

* **LTS:** Long-Term Support release, recommended for most production applications.
* **Current:** Latest feature release, useful for testing newer capabilities.

For most development environments, the LTS release is the practical default.

# 🗁 Installing on Windows

Download the Windows installer from:

<https://nodejs.org/en/download>

Run the installer and follow the installation wizard.

Verify the installation:

```bash
node --version
```

Check npm:

```bash
npm --version
```

# 🗁 Installing on Linux

On Linux, a Node.js version manager is often preferable to manually installing a system-wide version.

For example, using NVM:

```bash
nvm install --lts
```

Set the installed LTS release as the default:

```bash
nvm alias default 'lts/*'
```

Verify:

```bash
node --version
npm --version
```

Installation instructions:

<https://github.com/nvm-sh/nvm>

# 🗁 Installing on macOS

A version manager such as NVM can also be used on macOS.

After installing NVM:

```bash
nvm install --lts
nvm use --lts
```

Verify:

```bash
node --version
npm --version
```

# 🗁 Node.js Version Management

Different projects may require different Node.js versions.

A version manager allows developers to switch between versions without repeatedly installing and uninstalling Node.js.

Common tools include:

* NVM — Node Version Manager for Unix-like environments.
* NVM for Windows — Windows-specific version manager.
* Volta — JavaScript toolchain manager.
* fnm — Fast Node Manager.
* asdf — Multi-language version manager.

Example with NVM:

```bash
nvm install 22
nvm use 22
```

Check available installed versions:

```bash
nvm ls
```

Check the currently active version:

```bash
node --version
```

# 🗁 Node.js REPL

REPL stands for Read-Eval-Print Loop.

It is an interactive environment that allows developers to execute JavaScript expressions directly from the terminal.

Start the REPL:

```bash
node
```

Example:

```javascript
> const name = "John";
undefined

> console.log(`Hello, ${name}`);
Hello, John

> 10 + 20
30
```

The REPL performs four basic operations:

* Read: Accepts JavaScript input.
* Evaluate: Executes the input.
* Print: Displays the result.
* Loop: Waits for additional input.

Exit the REPL:

```javascript
.exit
```

Alternatively, press `Ctrl + C` twice or `Ctrl + D` in supported terminal environments.

# 🗁 Executing JavaScript Files

Node.js executes JavaScript files directly.

Create a file:

```javascript
// app.js

const message = "Node.js is running!";

console.log(message);
```

Run it:

```bash
node app.js
```

You can also provide command-line arguments:

```bash
node app.js development
```

Access them through `process.argv`:

```javascript
console.log(process.argv);
```

For application arguments, Node.js exposes the values after the executable and script paths:

```javascript
const environment = process.argv[2];

console.log(`Environment: ${environment}`);
```

# 🗁 Node.js Global Objects

Node.js provides several global objects and functions that can be accessed without importing them.

Common examples:

| Global | Purpose |
| --- | --- |
| `globalThis` | Universal global object |
| `process` | Information and control over the current process |
| `console` | Logging and debugging |
| `Buffer` | Handling binary data |
| `URL` | URL parsing and manipulation |
| `URLSearchParams` | Query-string manipulation |
| `setTimeout()` | Schedules a callback after a delay |
| `setInterval()` | Repeatedly schedules a callback |
| `setImmediate()` | Schedules a callback for a later event-loop phase |
| `queueMicrotask()` | Schedules a microtask |

Example:

```javascript
console.log(process.platform);
console.log(process.version);
console.log(process.cwd());
```

Note that `window` and `document` are browser-specific globals and are not available in a standard Node.js environment.

# 🗁 The process Object

The `process` object provides information about and control over the currently running Node.js process.

Examples:

**Node.js version**

```javascript
console.log(process.version);
```

**Platform**

```javascript
console.log(process.platform);
```

**Current working directory**

```javascript
console.log(process.cwd());
```

**Environment variables**

```javascript
console.log(process.env.NODE_ENV);
```

**Command-line arguments**

```javascript
console.log(process.argv);
```

**Process identifier**

```javascript
console.log(process.pid);
```

**Exit process**

```javascript
process.exit(0);
```

Exit code `0` conventionally indicates successful execution. Nonzero exit codes conventionally indicate failure.

Use `process.exit()` carefully because it can terminate the process before pending asynchronous operations complete.

# 🗁 CommonJS Modules

CommonJS is an older and widely used JavaScript module system in Node.js.

It uses `require()` to import modules and `module.exports` or `exports` to expose functionality.

Example:

```javascript
// math.js

function add(a, b) {
  return a + b;
}

module.exports = {
  add,
};
```

Import:

```javascript
// app.js

const { add } = require("./math.js");

console.log(add(10, 20));
```

CommonJS remains supported in Node.js, although modern JavaScript applications commonly use ECMAScript modules.

# 🗁 ECMAScript Modules in Node.js

ECMAScript Modules (ESM) are the standard JavaScript module system.

They use `import` and `export`.

Example:

```javascript
// math.js

export function add(a, b) {
  return a + b;
}
```

Import:

```javascript
// app.js

import { add } from "./math.js";

console.log(add(10, 20));
```

To explicitly use ESM in a Node.js package, configure `package.json`:

```json
{
  "type": "module"
}
```

Alternatively, use the `.mjs` file extension.

CommonJS files can use `.cjs` or be interpreted according to the package configuration.

# 🗁 Node.js Built-in Modules

Node.js includes many built-in modules that provide functionality without installing additional npm packages.

Common modules:

| Module | Purpose |
| --- | --- |
| `fs` | File system operations |
| `path` | File path manipulation |
| `http` | HTTP server and client |
| `https` | HTTPS server and client |
| `os` | Operating system information |
| `crypto` | Cryptographic operations |
| `events` | Event-driven programming |
| `stream` | Stream processing |
| `url` | URL handling |
| `util` | Utility functions |
| `child_process` | Creating child processes |
| `worker_threads` | Running JavaScript in worker threads |
| `assert` | Assertions |
| `test` | Built-in test runner |

Import a built-in module:

```javascript
import fs from "node:fs";
```

Using the `node:` prefix makes it explicit that the module comes from Node.js rather than an installed package.

# 🗁 Node.js Event-Driven Architecture

Node.js uses an event-driven architecture.

Components can emit events, and other components can listen for and respond to those events.

The built-in `EventEmitter` class provides a common event-handling mechanism.

```javascript
import { EventEmitter } from "node:events";

const emitter = new EventEmitter();

emitter.on("userCreated", (user) => {
  console.log(`User created: ${user.name}`);
});

emitter.emit("userCreated", {
  id: 1,
  name: "John",
});
```

Output:

```text
User created: John
```

This pattern is useful for application events, notifications, and communication between components.

# 🗁 Node.js Non-Blocking I/O

Node.js supports non-blocking I/O operations.

An asynchronous operation can begin without requiring the JavaScript thread to wait until that operation completes.

Example:

```javascript
import { readFile } from "node:fs";

console.log("Starting");

readFile("example.txt", "utf8", (error, data) => {
  if (error) {
    console.error(error);
    return;
  }

  console.log(data);
});

console.log("Finished");
```

The final `console.log()` can execute before the file-reading callback.

This behavior allows Node.js applications to perform other work while waiting for I/O operations.

# 🗁 Node.js Single-Threaded Execution Model

JavaScript application code normally executes on a single main JavaScript thread per Node.js isolate.

However, Node.js is not entirely single-threaded internally.

It uses:

* A main JavaScript execution thread.
* An event loop.
* Operating-system asynchronous I/O mechanisms.
* A libuv thread pool for certain operations.
* Worker threads when parallel JavaScript execution is required.

This architecture allows Node.js to handle many concurrent I/O operations efficiently.

For CPU-intensive work, worker threads or separate processes may be more appropriate than performing long computations on the main JavaScript thread.

# 🗁 Node.js Use Cases

Node.js is particularly suitable for applications involving asynchronous I/O.

Common use cases include:

**REST APIs**

Building backend services that expose HTTP endpoints.

**Real-time applications**

Developing chat applications, notifications, and collaborative systems using technologies such as WebSockets.

**Microservices**

Building independent services that communicate through HTTP, messaging systems, or other protocols.

**Command-line tools**

Creating developer utilities, automation scripts, and build tools.

**Server-side rendering**

Rendering web application content on the server.

**Streaming applications**

Processing large amounts of data without loading everything into memory at once.

# 🗁 When Node.js Is Not the Best Choice

Node.js is not automatically the best runtime for every workload.

It may require additional architecture for:

* CPU-intensive numerical computations.
* Heavy image and video processing.
* Large-scale scientific computation.
* Workloads that require specialized parallel processing.

These applications may benefit from worker threads, native extensions, separate processing services, or another runtime better suited to the workload.

# 🗁 Useful Node.js Commands

| Command | Purpose |
| --- | --- |
| `node --version` | Displays Node.js version |
| `node --help` | Displays command-line help |
| `node app.js` | Executes a JavaScript file |
| `node` | Starts the REPL |
| `node --watch app.js` | Restarts the process when watched files change |
| `node --test` | Runs the built-in test runner |
| `node --inspect app.js` | Enables debugging through the inspector |
| `node --check app.js` | Checks JavaScript syntax without executing the file |
| `node --trace-warnings app.js` | Displays stack traces for process warnings |

# 🗁 Summary

Node.js is a JavaScript runtime environment built on the V8 engine.

Key concepts:

* Node.js executes JavaScript outside browsers.
* V8 executes JavaScript code.
* libuv provides event-loop and asynchronous infrastructure.
* Node.js supports non-blocking I/O.
* CommonJS and ECMAScript Modules are supported.
* Built-in modules provide access to system functionality.
* The `process` object exposes runtime and process information.
* Node.js is well suited to I/O-intensive applications.
* Worker threads can support CPU-intensive JavaScript workloads.

Understanding the Node.js runtime is essential for backend development, server-side JavaScript, and modern JavaScript tooling.
