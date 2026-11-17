# Browser DevTools

Browser Developer Tools (DevTools) are built-in browser utilities that help developers inspect, debug, test, and optimize web applications.

Modern browsers provide powerful development tools for working with:

* HTML and DOM structures.
* CSS styles and layouts.
* JavaScript execution.
* Network requests and responses.
* Browser storage.
* Performance and memory usage.
* Accessibility.
* Security and application behavior.

DevTools are essential for frontend development because they allow developers to understand what happens inside a browser while an application is running.

# 🗁 Opening Browser DevTools

Most modern browsers provide several ways to open Developer Tools.

**Keyboard shortcuts**

| Browser | Windows / Linux | macOS |
| --- | --- | --- |
| Chrome | `F12` or `Ctrl + Shift + I` | `Cmd + Option + I` |
| Firefox | `F12` or `Ctrl + Shift + I` | `Cmd + Option + I` |
| Edge | `F12` or `Ctrl + Shift + I` | `Cmd + Option + I` |

You can also:

* Right-click an element and select **Inspect**.
* Open the browser menu and navigate to Developer Tools.
* Use the browser's command menu when available.

# 🗁 DevTools Interface

Developer Tools usually contain several panels.

Common panels include:

| Panel | Purpose |
| --- | --- |
| Elements | Inspect and modify HTML and CSS |
| Console | Execute JavaScript and inspect messages |
| Sources | Debug JavaScript source code |
| Network | Inspect HTTP requests and responses |
| Performance | Analyze runtime performance |
| Memory | Investigate memory usage |
| Application | Inspect browser storage and application resources |
| Security | Inspect connection security information |
| Lighthouse | Audit performance, accessibility, SEO, and best practices |

The available panels and their names can vary between browsers.

# 🗁 Elements Panel

The Elements panel allows you to inspect and modify the Document Object Model (DOM).

It displays the HTML structure currently rendered by the browser.

Example HTML:

```html
<div class="container">
  <h1>Hello World</h1>
  <p>Learning browser DevTools</p>
</div>
```

In the Elements panel, you can:

* Inspect HTML elements.
* Expand and collapse DOM nodes.
* Edit HTML attributes.
* Change element content.
* Add or remove elements.
* Inspect CSS rules.
* Examine computed styles.
* Identify layout dimensions.

Changes made directly in DevTools are generally temporary and do not modify the original source files.

# 🗁 Inspecting HTML Elements

To inspect an element:

* Open the Elements panel.
* Select the element in the DOM tree.
* Examine its attributes and properties.
* Review its associated styles.

You can also use the element picker to select an element directly from the rendered webpage.

The selected element becomes available through the special `$0` reference in the Console panel.

```javascript
console.log($0);
```

`$0` refers to the currently selected element. Previous selections can be accessed through `$1`, `$2`, and other numbered references.

# 🗁 Editing HTML in DevTools

You can temporarily modify HTML directly in the Elements panel.

For example, change:

```html
<h1>Hello World</h1>
```

To:

```html
<h1>Welcome to JavaScript</h1>
```

The browser immediately updates the rendered page.

This is useful for testing markup changes without editing the source file first.

# 🗁 Styles Panel

The Styles panel displays CSS rules applied to the selected element.

Example:

```css
.container {
  width: 100%;
  max-width: 1200px;
  margin: 0 auto;
  padding: 20px;
}

h1 {
  color: blue;
  font-size: 32px;
}
```

You can:

* Enable or disable individual declarations.
* Modify CSS property values.
* Add new declarations.
* Add new CSS rules.
* Inspect inherited styles.
* Identify overridden declarations.
* Navigate to the source stylesheet.

This is particularly useful when debugging unexpected styling behavior.

# 🗁 Computed Styles

The Computed panel displays the final computed CSS values for an element.

A CSS property can be affected by:

* Browser default styles.
* Inheritance.
* CSS specificity.
* The cascade.
* Media queries.
* CSS custom properties.
* Layout calculations.

For example:

```css
h1 {
  color: red;
}

.page h1 {
  color: blue;
}
```

The computed color will be blue when the element matches both selectors because `.page h1` has higher specificity.

The Computed panel helps identify the final value and trace its source.

# 🗁 Box Model Inspection

The box model describes how an element occupies space.

It consists of:

* Content.
* Padding.
* Border.
* Margin.

Example:

```css
.card {
  width: 300px;
  padding: 20px;
  border: 2px solid black;
  margin: 10px;
}
```

In the Elements panel, the box model visualization helps inspect these dimensions.

This is useful when troubleshooting:

* Unexpected element sizes.
* Overflow issues.
* Incorrect spacing.
* Alignment problems.
* Layout inconsistencies.

# 🗁 Responsive Design Mode

Responsive design tools allow you to test how a webpage behaves at different viewport sizes.

You can simulate:

* Mobile screens.
* Tablets.
* Desktop displays.
* Custom viewport dimensions.
* Device pixel ratios.
* Different screen orientations.

For example, test a layout at:

| Device category | Example viewport |
| --- | --- |
| Small mobile | 375 × 667 |
| Large mobile | 430 × 932 |
| Tablet | 768 × 1024 |
| Laptop | 1366 × 768 |
| Desktop | 1920 × 1080 |

These are example viewport dimensions, not fixed standards for every device.

Use responsive design mode to identify horizontal overflow, broken navigation, and layout issues.

# 🗁 Console Panel

The Console panel provides a JavaScript execution environment and displays application messages.

You can execute JavaScript directly:

```javascript
const message = "Hello from DevTools";

console.log(message);
```

You can also inspect the DOM:

```javascript
document.querySelector("h1");
```

Modify an element:

```javascript
document.querySelector("h1").textContent = "Updated Heading";
```

Inspect browser information:

```javascript
console.log(window.location.href);
console.log(window.innerWidth);
console.log(window.innerHeight);
```

The Console panel is useful for testing small expressions, inspecting variables, and identifying runtime errors.

# 🗁 Console Error Inspection

When JavaScript execution fails, the browser often displays an error message with a source location.

Example:

```javascript
function greetUser() {
  console.log(username);
}

greetUser();
```

If `username` is not declared, the console displays a `ReferenceError`.

You can click the source location associated with the error to navigate to the relevant code in the Sources panel.

# 🗁 Sources Panel

The Sources panel allows you to inspect and debug JavaScript source code loaded by the browser.

It commonly includes:

* JavaScript files.
* CSS files.
* HTML documents.
* Source maps.
* Loaded scripts.
* Breakpoint controls.
* Call stack information.

It is one of the most important panels for debugging complex JavaScript applications.

# 🗁 Setting Breakpoints

A breakpoint pauses JavaScript execution at a specific line.

To set a breakpoint:

* Open the Sources panel.
* Locate the JavaScript file.
* Find the line you want to inspect.
* Click the line number.
* Execute the code path that reaches the breakpoint.

Example:

```javascript
function calculateTotal(price, quantity) {
  const subtotal = price * quantity;

  const tax = subtotal * 0.1;

  const total = subtotal + tax;

  return total;
}

console.log(calculateTotal(100, 2));
```

Set a breakpoint at:

```javascript
const tax = subtotal * 0.1;
```

When execution pauses, you can inspect the values of `price`, `quantity`, and `subtotal`.

# 🗁 Debugger Controls

When execution pauses at a breakpoint, DevTools provides controls for navigating through the program.

| Control | Purpose |
| --- | --- |
| Resume | Continue execution |
| Step over | Execute the current line without entering called functions |
| Step into | Enter a function call |
| Step out | Finish the current function and return to its caller |
| Restart frame | Restart the current function when supported |
| Deactivate breakpoints | Temporarily disable breakpoints |

These controls help you understand the exact execution flow of JavaScript.

# 🗁 Watch Expressions

Watch expressions allow you to monitor specific values while execution is paused.

For example:

```javascript
const price = 100;
const quantity = 3;
const taxRate = 0.1;

const subtotal = price * quantity;
const total = subtotal * (1 + taxRate);
```

You can add expressions such as:

```javascript
price
quantity
subtotal
total
```

DevTools evaluates these expressions in the current debugging context.

This is useful when investigating how values change during execution.

# 🗁 Call Stack Inspection

The Call Stack panel displays the sequence of function calls that led to the current execution point.

Example:

```javascript
function first() {
  second();
}

function second() {
  third();
}

function third() {
  debugger;
}

first();
```

When execution pauses at `debugger`, the call stack contains:

```text
third()
second()
first()
```

This helps identify where a function was called and how execution reached the current location.

# 🗁 The debugger Statement

JavaScript provides the `debugger` statement to programmatically trigger a debugging pause when developer tools are available and debugging is active.

```javascript
function calculate(a, b) {
  const result = a + b;

  debugger;

  return result;
}

calculate(10, 20);
```

When execution reaches `debugger`, the browser pauses if an appropriate debugger is attached.

Remove unnecessary `debugger` statements before production deployment.

# 🗁 Conditional Breakpoints

A conditional breakpoint pauses execution only when a specified expression evaluates to true.

Example:

```javascript
for (let i = 0; i < 100; i++) {
  console.log(i);
}
```

You can set a conditional breakpoint with:

```javascript
i === 50
```

Execution pauses only when `i` reaches `50`.

This is useful when debugging loops, repeated function calls, and large datasets.

# 🗁 Network Panel

The Network panel records network activity generated by a webpage.

It allows you to inspect:

* HTTP requests.
* HTTP responses.
* Request methods.
* Status codes.
* Request headers.
* Response headers.
* Payloads.
* Timing information.
* Transferred resources.

It is especially important when debugging frontend-to-backend communication.

# 🗁 Inspecting HTTP Requests

Suppose your application executes:

```javascript
fetch("/api/users")
  .then(response => response.json())
  .then(users => console.log(users));
```

Open the Network panel and reload the page or trigger the request.

Select the corresponding request to inspect its details.

Common request information includes:

| Field | Description |
| --- | --- |
| Request URL | Requested resource address |
| Request Method | HTTP method such as GET or POST |
| Status Code | Server response status |
| Request Headers | Headers sent by the client |
| Response Headers | Headers returned by the server |
| Payload | Request body when applicable |
| Response | Returned response content |
| Timing | Time spent in different request phases |

# 🗁 Understanding HTTP Status Codes

The Network panel helps identify HTTP response failures.

Common status codes:

| Status | Meaning |
| --- | --- |
| 200 | OK |
| 201 | Created |
| 204 | No Content |
| 301 | Moved Permanently |
| 302 | Found |
| 304 | Not Modified |
| 400 | Bad Request |
| 401 | Unauthorized |
| 403 | Forbidden |
| 404 | Not Found |
| 405 | Method Not Allowed |
| 409 | Conflict |
| 422 | Unprocessable Content |
| 429 | Too Many Requests |
| 500 | Internal Server Error |
| 502 | Bad Gateway |
| 503 | Service Unavailable |
| 504 | Gateway Timeout |

An HTTP error response does not necessarily cause `fetch()` to reject. Inspect `response.ok` or `response.status` to handle unsuccessful HTTP responses.

# 🗁 Network Throttling

Network throttling simulates slower network conditions.

You can test application behavior under conditions such as:

* Fast network connections.
* Slow mobile connections.
* High latency.
* Limited bandwidth.
* Offline mode.

This helps identify applications that depend too heavily on fast network access.

# 🗁 Preserving Network Logs

The Preserve log option keeps network entries when navigating between pages.

This is useful when debugging:

* Redirects.
* Authentication flows.
* Page navigation.
* API requests triggered during navigation.
* Failed requests that disappear after a page transition.

The Disable cache option can also help test requests without relying on the browser's normal HTTP cache while DevTools is open.

# 🗁 Application Panel

The Application panel helps inspect browser-managed application resources and storage.

Common sections include:

* Local Storage.
* Session Storage.
* IndexedDB.
* Cookies.
* Cache Storage.
* Service Workers.
* Manifest.
* Web App resources.

This panel is particularly useful for debugging Progressive Web Applications and client-side persistence.

# 🗁 Local Storage Inspection

Local Storage stores key-value pairs in the browser.

Example:

```javascript
localStorage.setItem("username", "John");

localStorage.setItem("theme", "dark");
```

In the Application panel, open Local Storage and select the website's origin.

You can inspect, modify, or remove stored entries.

# 🗁 Session Storage Inspection

Session Storage stores key-value pairs associated with a browser page session.

Example:

```javascript
sessionStorage.setItem("sessionId", "abc123");
```

Open Session Storage in the Application panel to inspect its entries.

# 🗁 Cookie Inspection

Cookies are used for browser-managed data and server communication.

Example:

```javascript
document.cookie;
```

The Application panel allows you to inspect cookie properties such as:

* Name.
* Value.
* Domain.
* Path.
* Expiration.
* Secure.
* HttpOnly.
* SameSite.

HttpOnly cookies cannot be accessed through `document.cookie`, but they can still be inspected through browser developer tools when permitted.

# 🗁 IndexedDB Inspection

IndexedDB is a browser database for storing structured client-side data.

The Application panel allows you to inspect:

* Database names.
* Object stores.
* Records.
* Indexes.
* Database versions.

It is useful when debugging applications that store larger amounts of structured data locally.

# 🗁 Performance Panel

The Performance panel records browser activity and helps identify performance bottlenecks.

It can provide information about:

* JavaScript execution.
* Rendering.
* Layout calculations.
* Painting.
* Event handling.
* Network activity.
* Long tasks.

For example, a function that performs expensive synchronous work may block the main thread and delay user interactions.

Use a performance recording to locate the expensive operation and investigate its execution time.

# 🗁 Memory Panel

The Memory panel helps investigate memory usage and potential memory leaks.

Common profiling options include:

* Heap snapshots.
* Allocation instrumentation.
* Allocation sampling.

A memory leak occurs when an application unintentionally retains references to objects that are no longer needed.

Example:

```javascript
const storedElements = [];

function createElement() {
  const element = document.createElement("div");

  storedElements.push(element);
}
```

If elements are continually added to `storedElements` and never released, memory consumption can grow unnecessarily.

Heap snapshots help identify retained objects and their reference paths.

# 🗁 Lighthouse Panel

Lighthouse audits web applications across several quality dimensions.

Common audit categories include:

* Performance.
* Accessibility.
* Best Practices.
* SEO.

Lighthouse can identify issues such as:

* Large image resources.
* Render-blocking resources.
* Accessibility problems.
* Missing metadata.
* Inefficient loading strategies.
* Poor performance metrics.

The exact audit results depend on the page, browser environment, and Lighthouse version.

# 🗁 Security Panel

The Security panel helps inspect the security characteristics of a webpage's connections.

It can provide information about:

* HTTPS usage.
* TLS certificate details.
* Certificate validity.
* Connection security.
* Mixed content issues.

Mixed content occurs when an HTTPS page loads certain resources over insecure HTTP.

For example:

```html
<img src="http://example.com/image.jpg" alt="Example">
```

An HTTPS page should generally load resources through secure HTTPS connections.

# 🗁 Source Maps

Source maps connect generated or transformed JavaScript code to its original source files.

They are particularly useful when working with:

* TypeScript.
* Bundlers.
* Minified JavaScript.
* Transpiled code.
* Framework-based applications.

For example, a TypeScript source file:

```typescript
function greet(name: string): string {
  return `Hello, ${name}`;
}
```

May be compiled into JavaScript:

```javascript
function greet(name) {
  return `Hello, ${name}`;
}
```

With a properly configured source map, DevTools can display the original TypeScript source while debugging the generated JavaScript.

# 🗁 Debugging JavaScript with DevTools

Consider the following example:

```javascript
const users = [
  { id: 1, name: "John" },
  { id: 2, name: "Sarah" },
  { id: 3, name: "Alex" }
];

function findUser(id) {
  const user = users.find(user => user.id === id);

  return user.name;
}

console.log(findUser(10));
```

This code produces a `TypeError` because no matching user exists and `user` is therefore `undefined`.

A debugging workflow:

* Open DevTools.
* Navigate to the Sources panel.
* Locate the JavaScript file.
* Set a breakpoint inside `findUser()`.
* Execute the function.
* Inspect the `id` parameter.
* Inspect the `user` variable.
* Identify why the lookup returned `undefined`.
* Correct the implementation.

A safer implementation:

```javascript
function findUser(id) {
  const user = users.find(user => user.id === id);

  if (!user) {
    return null;
  }

  return user.name;
}

console.log(findUser(10));
```

Output:

```text
null
```

# 🗁 Common DevTools Workflows

**Debugging a JavaScript error**

* Open Console.
* Read the error message.
* Follow the source location.
* Open the relevant source file.
* Set breakpoints.
* Inspect variables and the call stack.
* Correct the underlying problem.

**Debugging a CSS layout issue**

* Open Elements.
* Select the affected element.
* Inspect the Styles panel.
* Review computed styles.
* Inspect the box model.
* Modify CSS declarations temporarily.
* Apply the verified changes to the source stylesheet.

**Debugging a failed API request**

* Open Network.
* Trigger the request.
* Inspect the request method and URL.
* Check the status code.
* Review request headers and payload.
* Inspect response headers and body.
* Identify whether the failure originates in the client, network, or server.

**Debugging browser storage**

* Open Application.
* Select the relevant storage type.
* Inspect stored values.
* Check expiration and scope.
* Verify that the application reads and writes the expected data.

# 🗁 Common Mistakes

**Editing code only in DevTools**

Changes made directly in DevTools are generally temporary. Update the actual source files after testing a solution.

**Ignoring the Network panel**

A frontend application can appear broken because of an API failure rather than a JavaScript error.

**Debugging without breakpoints**

Console messages are useful, but complex execution problems are often easier to investigate with breakpoints and variable inspection.

**Confusing HTTP errors with network errors**

A `404` or `500` response is still an HTTP response. A network failure may prevent a usable response from being received.

**Assuming source maps always exist**

If source maps are missing or incorrectly configured, DevTools may display bundled or generated code instead of the original source.

**Ignoring browser caching**

Cached resources can make local changes appear ineffective. Use the Network panel and appropriate cache controls when investigating loading behavior.

# 🗁 Best Practices

* Become comfortable with the Elements, Console, Sources, and Network panels.
* Use breakpoints for complex JavaScript debugging.
* Inspect computed CSS rather than guessing which declaration is applied.
* Check Network requests when debugging API integration.
* Use responsive mode to test different viewport sizes.
* Inspect browser storage when debugging persistence issues.
* Use Performance recordings to investigate slow interactions.
* Use Memory profiling when investigating memory leaks.
* Keep source maps available in development builds.
* Reproduce problems consistently before investigating them.
* Test fixes in the actual application source code.
* Avoid exposing sensitive information while inspecting or sharing diagnostic data.

# 🗁 Key Takeaways

* Browser DevTools provide an integrated environment for inspecting and debugging web applications.
* Elements helps inspect and modify the DOM and CSS.
* Console provides a JavaScript execution and diagnostic environment.
* Sources supports breakpoints, step debugging, and call stack inspection.
* Network helps diagnose HTTP requests and API failures.
* Application provides access to browser storage and service workers.
* Performance helps identify rendering and execution bottlenecks.
* Memory helps investigate memory usage and leaks.
* Lighthouse audits application quality.
* Security helps inspect HTTPS and connection-related issues.
* Source maps make debugging transformed and bundled code easier.
* Effective DevTools usage improves development speed and application reliability.
