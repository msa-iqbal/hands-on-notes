# Window Object

The `window` object represents the browser window or browsing context in which a web page is running.

In a browser, it acts as the global object for JavaScript code executing in that window.

It provides access to important browser functionality, including:

* Document manipulation through the DOM.
* Browser location and navigation.
* Browser history.
* Timers.
* Dialog boxes.
* Storage APIs.
* Browser dimensions.
* Event handling.
* Other browser APIs.

Example:

```javascript
console.log(window);
console.log(window.document);
console.log(window.location);
console.log(window.history);
```

# 🗁 Understanding the Global Window Object

In browser environments, global variables and functions declared using certain global declaration mechanisms become properties of `window`.

For example:

```javascript
var username = "John";

function greet() {
  console.log("Hello!");
}

console.log(window.username);
window.greet();
```

Output:

```text
John
Hello!
```

However, top-level `let`, `const`, and class declarations in classic scripts do not become properties of `window`.

```javascript
let age = 25;
const country = "Bangladesh";

console.log(window.age);
console.log(window.country);
```

Both values are `undefined`.

In JavaScript modules, top-level declarations also do not become properties of `window`.

# 🗁 window vs globalThis

`globalThis` provides a standardized way to access the global object across different JavaScript environments.

In browsers, `globalThis` normally refers to the same global object as `window`.

```javascript
console.log(window === globalThis);
```

Output:

```text
true
```

Important differences:

| Feature | `window` | `globalThis` |
| --- | --- | --- |
| Browser | Yes | Yes |
| Node.js | No | Yes |
| Web Workers | No | Yes |
| Standardized global access | Browser-specific | Cross-environment |

Use `globalThis` when writing JavaScript that should work across multiple environments.

# 🗁 window.document

The `document` property provides access to the current HTML document.

```javascript
const title = window.document.title;

console.log(title);
```

You can also access it without the `window` prefix:

```javascript
console.log(document.title);
```

Common document operations:

```javascript
document.title = "My Application";

const heading = document.querySelector("h1");

heading.textContent = "Welcome";
```

The `document` object is covered in the DOM section.

# 🗁 window.location

The `location` property provides information about the current URL and allows navigation.

```javascript
console.log(window.location.href);
console.log(window.location.hostname);
console.log(window.location.pathname);
```

Example URL:

```text
https://example.com/products?id=10#details
```

Important properties:

| Property | Description |
| --- | --- |
| `href` | Complete URL |
| `protocol` | URL protocol |
| `hostname` | Hostname |
| `host` | Hostname and port |
| `port` | Port number |
| `pathname` | URL path |
| `search` | Query string |
| `hash` | Fragment identifier |
| `origin` | Origin consisting of scheme, hostname, and port |

Navigation example:

```javascript
window.location.href = "https://example.com";
```

Other navigation methods:

```javascript
window.location.assign("https://example.com");

window.location.replace("https://example.com");
```

The `assign()` method navigates to a new URL while retaining the previous history entry.

The `replace()` method navigates without retaining the current page as a separate history entry.

Reload the current page:

```javascript
window.location.reload();
```

# 🗁 window.history

The `history` property provides access to the browser session history for the current tab.

```javascript
console.log(window.history);
console.log(window.history.length);
```

Common methods:

```javascript
window.history.back();

window.history.forward();

window.history.go(-2);
```

Modern web applications can also manipulate the current history entry.

```javascript
history.pushState(
  { page: "profile" },
  "",
  "/profile"
);
```

Replace the current history entry:

```javascript
history.replaceState(
  { page: "dashboard" },
  "",
  "/dashboard"
);
```

These methods are commonly used by client-side routers.

# 🗁 window.innerWidth and innerHeight

These properties return the width and height of the layout viewport in CSS pixels.

```javascript
console.log(window.innerWidth);
console.log(window.innerHeight);
```

Example:

```javascript
window.addEventListener("resize", () => {
  console.log("Width:", window.innerWidth);
  console.log("Height:", window.innerHeight);
});
```

They are useful for responsive behavior and adapting application interfaces to different viewport sizes.

# 🗁 window.outerWidth and outerHeight

These properties return the width and height of the browser window, including browser interface areas where applicable.

```javascript
console.log(window.outerWidth);
console.log(window.outerHeight);
```

Comparison:

| Property | Description |
| --- | --- |
| `innerWidth` | Layout viewport width |
| `innerHeight` | Layout viewport height |
| `outerWidth` | Browser window width |
| `outerHeight` | Browser window height |

These dimensions may differ depending on the browser and operating system.

# 🗁 window.screen

The `screen` property provides information about the user's display.

```javascript
console.log(window.screen.width);
console.log(window.screen.height);
console.log(window.screen.availWidth);
console.log(window.screen.availHeight);
```

Common properties:

| Property | Description |
| --- | --- |
| `width` | Screen width |
| `height` | Screen height |
| `availWidth` | Available screen width |
| `availHeight` | Available screen height |
| `colorDepth` | Screen color depth |
| `pixelDepth` | Screen pixel depth |

Example:

```javascript
console.log(
  `Screen: ${screen.width} × ${screen.height}`
);
```

# 🗁 window.navigator

The `navigator` property provides information about the browser and its environment.

```javascript
console.log(window.navigator);
```

Common properties:

| Property | Description |
| --- | --- |
| `userAgent` | Browser user-agent string |
| `language` | Preferred browser language |
| `languages` | Preferred languages |
| `platform` | Platform information |
| `onLine` | Current online status indication |
| `cookieEnabled` | Whether cookies are enabled |

Example:

```javascript
console.log(navigator.language);
console.log(navigator.languages);
console.log(navigator.onLine);
```

The `navigator.onLine` property is only an indication of network connectivity, not proof that the application can reach a particular server.

# 🗁 window.console

The `console` property provides methods for debugging and logging.

```javascript
console.log("Normal message");

console.info("Information");

console.warn("Warning");

console.error("Error");

console.table([
  { name: "John", age: 25 },
  { name: "Sarah", age: 28 }
]);
```

Common methods:

| Method | Description |
| --- | --- |
| `log()` | General logging |
| `info()` | Informational message |
| `warn()` | Warning message |
| `error()` | Error message |
| `table()` | Displays tabular data |
| `clear()` | Clears the console |
| `group()` | Starts a console group |
| `groupEnd()` | Ends a console group |
| `time()` | Starts a timer |
| `timeEnd()` | Ends a timer |

# 🗁 window.alert()

The `alert()` method displays a simple message dialog.

```javascript
window.alert("Welcome to our website!");
```

The dialog blocks normal JavaScript execution until the user dismisses it.

It is generally unsuitable for complex application interfaces.

# 🗁 window.confirm()

The `confirm()` method displays a dialog with confirmation options.

It returns a boolean value.

```javascript
const result = window.confirm("Do you want to continue?");

if (result) {
  console.log("User confirmed");
} else {
  console.log("User canceled");
}
```

Return values:

* `true`: User confirms.
* `false`: User cancels or dismisses the dialog.

# 🗁 window.prompt()

The `prompt()` method displays a dialog requesting text input.

```javascript
const username = window.prompt("Enter your name:");

if (username !== null) {
  console.log(`Hello, ${username}`);
}
```

Return values:

* A string containing the entered text.
* `null` if the user cancels the dialog.

These dialog methods are simple browser features. Modern applications generally prefer custom UI components for better control and accessibility.

# 🗁 window.setTimeout()

The `setTimeout()` method schedules a callback to execute after a specified delay.

```javascript
window.setTimeout(() => {
  console.log("Executed after approximately 2 seconds");
}, 2000);
```

The delay is specified in milliseconds.

Cancel a scheduled timeout:

```javascript
const timeoutId = window.setTimeout(() => {
  console.log("This will not execute");
}, 3000);

window.clearTimeout(timeoutId);
```

The delay represents a minimum scheduling threshold, not an exact execution time.

# 🗁 window.setInterval()

The `setInterval()` method repeatedly executes a callback after a specified interval.

```javascript
const intervalId = window.setInterval(() => {
  console.log("Running every second");
}, 1000);
```

Stop the interval:

```javascript
window.clearInterval(intervalId);
```

Example countdown:

```javascript
let seconds = 5;

const intervalId = setInterval(() => {
  console.log(seconds);

  seconds--;

  if (seconds < 0) {
    clearInterval(intervalId);
    console.log("Finished");
  }
}, 1000);
```

# 🗁 window.matchMedia()

The `matchMedia()` method checks whether a CSS media query matches the current environment.

```javascript
const mediaQuery = window.matchMedia("(max-width: 768px)");

console.log(mediaQuery.matches);
```

The `matches` property returns a boolean.

You can also listen for changes:

```javascript
const mediaQuery = window.matchMedia("(max-width: 768px)");

mediaQuery.addEventListener("change", (event) => {
  if (event.matches) {
    console.log("Mobile layout");
  } else {
    console.log("Desktop layout");
  }
});
```

This is useful when JavaScript behavior needs to respond to responsive breakpoints.

# 🗁 window.getComputedStyle()

The `getComputedStyle()` method returns an object containing the resolved CSS styles of an element.

```html
<div id="box" class="box">Hello</div>
```

```javascript
const box = document.querySelector("#box");

const styles = window.getComputedStyle(box);

console.log(styles.color);
console.log(styles.width);
console.log(styles.fontSize);
```

It can also inspect pseudo-elements:

```javascript
const styles = getComputedStyle(box, "::before");

console.log(styles.content);
```

Unlike inline style access through `element.style`, computed styles include the final resolved styles from applicable stylesheets and the cascade.

# 🗁 window.open()

The `open()` method opens a URL in a new or existing browsing context.

```javascript
window.open("https://example.com", "_blank");
```

Common target values:

| Target | Description |
| --- | --- |
| `_blank` | New browsing context |
| `_self` | Current browsing context |
| `_parent` | Parent browsing context |
| `_top` | Top-level browsing context |

You can capture the returned window reference:

```javascript
const newWindow = window.open("https://example.com", "_blank");

if (newWindow) {
  console.log("New window opened");
}
```

Browsers may block popups that are not triggered by an appropriate user interaction.

# 🗁 window.close()

The `close()` method attempts to close the current browsing context.

```javascript
window.close();
```

Browsers generally restrict scripts from closing windows that were not opened by script.

# 🗁 window.addEventListener()

The `window` object supports event listeners for browser-level events.

Example:

```javascript
window.addEventListener("resize", () => {
  console.log("Window resized");
});

window.addEventListener("scroll", () => {
  console.log("Page scrolled");
});

window.addEventListener("online", () => {
  console.log("Browser reports online");
});

window.addEventListener("offline", () => {
  console.log("Browser reports offline");
});
```

Common window events:

| Event | Description |
| --- | --- |
| `load` | Window and its dependent resources have loaded |
| `resize` | Viewport size changes |
| `scroll` | Scrolling occurs |
| `online` | Browser reports network connectivity restored |
| `offline` | Browser reports network connectivity lost |
| `focus` | Window receives focus |
| `blur` | Window loses focus |
| `beforeunload` | Document is about to be unloaded |
| `pagehide` | Document is being hidden during navigation or lifecycle changes |
| `pageshow` | Document becomes visible, including restoration from the back-forward cache |

# 🗁 DOMContentLoaded vs load

These events represent different stages of page loading.

**DOMContentLoaded**

Fires when the HTML document has been completely parsed and deferred scripts have executed.

```javascript
document.addEventListener("DOMContentLoaded", () => {
  console.log("DOM is ready");
});
```

**load**

Fires when the window and its dependent resources have finished loading.

```javascript
window.addEventListener("load", () => {
  console.log("Page resources loaded");
});
```

Comparison:

| Feature | DOMContentLoaded | load |
| --- | --- | --- |
| HTML parsing | Complete | Complete |
| Deferred scripts | Executed | Executed |
| Images and dependent resources | Not necessarily loaded | Loaded |
| Typical use | DOM initialization | Work requiring loaded resources |

# 🗁 window.requestAnimationFrame()

The `requestAnimationFrame()` method schedules a callback before the browser's next repaint.

It is commonly used for animations.

```javascript
function animate() {
  console.log("Animation frame");

  requestAnimationFrame(animate);
}

requestAnimationFrame(animate);
```

The callback receives a timestamp representing the current animation time.

Example:

```javascript
let startTime;

function animate(timestamp) {
  if (startTime === undefined) {
    startTime = timestamp;
  }

  const elapsed = timestamp - startTime;

  console.log(elapsed);

  if (elapsed < 3000) {
    requestAnimationFrame(animate);
  }
}

requestAnimationFrame(animate);
```

Unlike `setInterval()`, `requestAnimationFrame()` is synchronized with the browser's rendering cycle and is generally preferred for visual animations.

# 🗁 window.devicePixelRatio

The `devicePixelRatio` property represents the ratio between physical display pixels and CSS pixels.

```javascript
console.log(window.devicePixelRatio);
```

For example:

* `1`: One device pixel per CSS pixel.
* `2`: Two device pixels per CSS pixel.
* `3`: Three device pixels per CSS pixel.

It is useful when rendering high-resolution graphics, especially with the Canvas API.

# 🗁 window.isSecureContext

The `isSecureContext` property indicates whether the current environment is considered a secure context.

```javascript
console.log(window.isSecureContext);
```

Secure contexts are required for many powerful browser APIs.

HTTPS is the standard way to serve secure web applications. Certain local development origins, such as `localhost`, are also treated as potentially trustworthy.

# 🗁 Best Practices

* Prefer standard browser APIs instead of unnecessary global variables.
* Use `globalThis` when cross-environment global access is required.
* Avoid attaching unnecessary properties to `window`.
* Use event listeners instead of inline event handlers.
* Clean up timers and event listeners when they are no longer needed.
* Use `requestAnimationFrame()` for rendering-related animations.
* Prefer `matchMedia()` for JavaScript media-query checks.
* Avoid relying on user-agent strings for browser feature detection when capability detection is possible.
* Do not assume `navigator.onLine` guarantees internet access.
* Use custom UI components instead of blocking dialogs for complex interactions.
* Be careful when opening new windows because browsers may block unsolicited popups.

# 🗁 Practical Example: Responsive Window Information

This example displays the current viewport dimensions and updates them whenever the browser window is resized.

```html
<div>
  <h2>Window Information</h2>

  <p>Width: <span id="width"></span></p>
  <p>Height: <span id="height"></span></p>
  <p>Device Pixel Ratio: <span id="pixelRatio"></span></p>
  <p>Secure Context: <span id="secure"></span></p>
</div>

<script>
  const width = document.querySelector("#width");
  const height = document.querySelector("#height");
  const pixelRatio = document.querySelector("#pixelRatio");
  const secure = document.querySelector("#secure");

  function updateWindowInfo() {
    width.textContent = window.innerWidth;
    height.textContent = window.innerHeight;

    pixelRatio.textContent = window.devicePixelRatio;

    secure.textContent = window.isSecureContext;
  }

  window.addEventListener("resize", updateWindowInfo);

  updateWindowInfo();
</script>
```

This example demonstrates:

* Accessing browser window properties.
* Reading viewport dimensions.
* Detecting display pixel density.
* Checking secure context status.
* Handling window resize events.
* Updating DOM content dynamically.

# 🗁 Summary

The `window` object is the primary interface between browser JavaScript and the browsing context.

Important concepts include:

* Global object and `globalThis`.
* Document access.
* Location and history.
* Browser and screen information.
* Dialog methods.
* Timers.
* Media queries.
* Computed styles.
* Window navigation.
* Browser-level events.
* Animation scheduling.
* Device pixel ratio.
* Secure contexts.

Understanding the `window` object provides a foundation for working with browser APIs and building modern web applications.
