# DOM Events

DOM events are actions or occurrences that happen in the browser and can be detected and handled by JavaScript.

Events allow web applications to respond to user interactions and browser activities, such as:

* Clicking a button
* Typing into an input field
* Submitting a form
* Moving the mouse
* Pressing a keyboard key
* Loading a page
* Resizing a window

JavaScript uses event listeners to execute code when specific events occur.

# 🗁 Understanding Event Handling

Event handling involves three main concepts:

* **Event:** An action or occurrence, such as a click.
* **Event target:** The element on which the event occurred.
* **Event listener:** A function that executes when the event occurs.

Example:

```html
<button id="clickButton">Click Me</button>

<script>
  const button = document.querySelector("#clickButton");

  button.addEventListener("click", () => {
    console.log("Button clicked!");
  });
</script>
```

When the button is clicked, the registered callback function executes.

# 🗁 addEventListener()

The `addEventListener()` method registers an event listener on an element.

Syntax:

```javascript
element.addEventListener(event, callback, options);
```

Parameters:

* `event`: The event type, such as `"click"` or `"submit"`.
* `callback`: The function executed when the event occurs.
* `options`: Optional configuration for the listener.

Example:

```javascript
const button = document.querySelector("button");

button.addEventListener("click", function () {
  console.log("Clicked!");
});
```

Arrow functions can also be used:

```javascript
button.addEventListener("click", () => {
  console.log("Clicked!");
});
```

# 🗁 Event Object

When an event occurs, the browser passes an event object to the listener callback.

It contains information about the event, including its target, type, and other event-specific properties.

```javascript
const button = document.querySelector("button");

button.addEventListener("click", (event) => {
  console.log(event);
  console.log(event.type);
  console.log(event.target);
});
```

Common event properties:

| Property | Description |
| --- | --- |
| `type` | Event type |
| `target` | Original element that triggered the event |
| `currentTarget` | Element whose listener is currently executing |
| `eventPhase` | Current event propagation phase |
| `bubbles` | Whether the event bubbles |
| `cancelable` | Whether its default action can be canceled |
| `defaultPrevented` | Whether the default action has been prevented |
| `timeStamp` | Time when the event was created |

# 🗁 target vs currentTarget

These properties have different meanings.

* `target` identifies the original element where the event originated.
* `currentTarget` identifies the element to which the currently executing listener is attached.

Example:

```html
<div id="container">
  <button id="button">Click Me</button>
</div>

<script>
  const container = document.querySelector("#container");

  container.addEventListener("click", (event) => {
    console.log("Target:", event.target);
    console.log("Current Target:", event.currentTarget);
  });
</script>
```

Clicking the button produces:

* `event.target`: The button.
* `event.currentTarget`: The container.

# 🗁 Common Mouse Events

Mouse events respond to mouse interactions.

| Event | Description |
| --- | --- |
| `click` | Mouse click |
| `dblclick` | Double-click |
| `mousedown` | Mouse button pressed |
| `mouseup` | Mouse button released |
| `mousemove` | Mouse pointer moves |
| `mouseover` | Pointer enters an element or one of its descendants |
| `mouseout` | Pointer leaves an element or one of its descendants |
| `mouseenter` | Pointer enters an element |
| `mouseleave` | Pointer leaves an element |
| `contextmenu` | Context menu is requested |

Example:

```javascript
const button = document.querySelector("button");

button.addEventListener("mouseenter", () => {
  console.log("Mouse entered");
});

button.addEventListener("mouseleave", () => {
  console.log("Mouse left");
});
```

# 🗁 Common Keyboard Events

Keyboard events respond to keyboard interactions.

| Event | Description |
| --- | --- |
| `keydown` | A key is pressed |
| `keyup` | A key is released |
| `keypress` | Deprecated; avoid using it |

Example:

```javascript
document.addEventListener("keydown", (event) => {
  console.log("Key:", event.key);
  console.log("Code:", event.code);
});
```

Important keyboard properties:

* `event.key`: Represents the interpreted key value, such as `"Enter"` or `"a"`.
* `event.code`: Represents the physical key position, such as `"KeyA"` or `"Enter"`.
* `event.repeat`: Indicates whether the key is being held down and repeatedly triggering the event.
* `event.ctrlKey`: Indicates whether Ctrl was pressed.
* `event.shiftKey`: Indicates whether Shift was pressed.
* `event.altKey`: Indicates whether Alt was pressed.
* `event.metaKey`: Indicates whether Meta was pressed.

Example:

```javascript
document.addEventListener("keydown", (event) => {
  if (event.key === "Enter") {
    console.log("Enter pressed");
  }

  if (event.ctrlKey && event.key.toLowerCase() === "s") {
    event.preventDefault();
    console.log("Custom save action");
  }
});
```

# 🗁 Common Form Events

Form events respond to interactions with form controls.

| Event | Description |
| --- | --- |
| `submit` | A form is submitted |
| `input` | An input value changes |
| `change` | A control's value is committed |
| `focus` | An element receives focus |
| `blur` | An element loses focus |
| `reset` | A form is reset |

Example:

```html
<input id="username" type="text">

<script>
  const input = document.querySelector("#username");

  input.addEventListener("input", (event) => {
    console.log(event.target.value);
  });
</script>
```

The `input` event fires as the user changes the value.

The `change` event generally fires when a value has been committed. Its exact timing depends on the form control.

# 🗁 Common Document and Window Events

These events relate to document loading and browser window activity.

| Event | Description |
| --- | --- |
| `DOMContentLoaded` | HTML parsing is complete |
| `load` | A resource or the entire page has finished loading |
| `resize` | Window dimensions change |
| `scroll` | Document or element scrolling occurs |
| `beforeunload` | Document is about to be unloaded |
| `visibilitychange` | Document visibility state changes |

Example:

```javascript
document.addEventListener("DOMContentLoaded", () => {
  console.log("HTML document parsed");
});

window.addEventListener("resize", () => {
  console.log("Window resized");
});

document.addEventListener("visibilitychange", () => {
  console.log(document.visibilityState);
});
```

# 🗁 Removing Event Listeners

The `removeEventListener()` method removes a previously registered listener.

The callback reference and event type must match the registered listener.

```javascript
const button = document.querySelector("button");

function handleClick() {
  console.log("Button clicked");
}

button.addEventListener("click", handleClick);

button.removeEventListener("click", handleClick);
```

Using a new anonymous function does not remove the original listener:

```javascript
button.addEventListener("click", () => {
  console.log("Clicked");
});

// This does not remove the previous listener.
button.removeEventListener("click", () => {
  console.log("Clicked");
});
```

Each arrow function expression creates a different function object.

# 🗁 Event Listener Options

The third argument of `addEventListener()` accepts a boolean or an options object.

Common options:

| Option | Description |
| --- | --- |
| `capture` | Registers the listener for the capturing phase |
| `once` | Automatically removes the listener after its first invocation |
| `passive` | Indicates that the listener will not call `preventDefault()` |
| `signal` | Uses an AbortSignal to control listener removal |

Example using `once`:

```javascript
button.addEventListener(
  "click",
  () => {
    console.log("This runs only once");
  },
  { once: true }
);
```

Example using `capture`:

```javascript
container.addEventListener(
  "click",
  () => {
    console.log("Capturing listener");
  },
  { capture: true }
);
```

Example using `AbortController`:

```javascript
const controller = new AbortController();

button.addEventListener(
  "click",
  () => {
    console.log("Button clicked");
  },
  { signal: controller.signal }
);

// Removes listeners registered with this signal.
controller.abort();
```

# 🗁 Inline Event Handlers vs addEventListener()

There are multiple ways to register event handlers.

Inline HTML handler:

```html
<button onclick="handleClick()">Click</button>
```

JavaScript event property:

```javascript
button.onclick = () => {
  console.log("Clicked");
};
```

Using `addEventListener()`:

```javascript
button.addEventListener("click", () => {
  console.log("Clicked");
});
```

Comparison:

| Approach | Characteristics |
| --- | --- |
| Inline handler | Mixes HTML and JavaScript |
| Event property | Only one handler per event property |
| `addEventListener()` | Supports multiple listeners and listener options |

`addEventListener()` is generally preferred because it separates behavior from markup and supports multiple independent listeners.

# 🗁 Event Propagation

Event propagation describes how an event travels through the DOM tree.

For events that participate in propagation, the browser processes three phases:

* Capturing phase: The event travels from the document toward the target.
* Target phase: The event reaches its target.
* Bubbling phase: The event travels from the target back toward the document.

Example:

```html
<div id="parent">
  <button id="child">Click Me</button>
</div>

<script>
  const parent = document.querySelector("#parent");
  const child = document.querySelector("#child");

  parent.addEventListener("click", () => {
    console.log("Parent");
  });

  child.addEventListener("click", () => {
    console.log("Child");
  });
</script>
```

Clicking the button produces:

```text
Child
Parent
```

The event reaches the button first and then bubbles to the parent.

# 🗁 stopPropagation()

The `stopPropagation()` method prevents an event from continuing through the propagation path.

```javascript
parent.addEventListener("click", () => {
  console.log("Parent clicked");
});

child.addEventListener("click", (event) => {
  event.stopPropagation();
  console.log("Child clicked");
});
```

Clicking the child button produces:

```text
Child clicked
```

The parent listener does not execute for that event.

# 🗁 stopImmediatePropagation()

The `stopImmediatePropagation()` method prevents the event from reaching other listeners on the same element and stops further propagation.

```javascript
button.addEventListener("click", (event) => {
  console.log("First listener");

  event.stopImmediatePropagation();
});

button.addEventListener("click", () => {
  console.log("Second listener");
});
```

Output:

```text
First listener
```

The second listener does not execute.

# 🗁 preventDefault()

The `preventDefault()` method prevents the browser's default action for a cancelable event.

Example: Preventing form submission.

```html
<form id="loginForm">
  <input type="text" required>
  <button type="submit">Login</button>
</form>

<script>
  const form = document.querySelector("#loginForm");

  form.addEventListener("submit", (event) => {
    event.preventDefault();

    console.log("Default form submission prevented");
  });
</script>
```

Other examples include:

* Preventing a link from navigating.
* Preventing a form from submitting normally.
* Preventing the browser's context menu from opening.

Example:

```javascript
document.addEventListener("contextmenu", (event) => {
  event.preventDefault();

  console.log("Custom context menu behavior");
});
```

`preventDefault()` and `stopPropagation()` solve different problems:

| Method | Purpose |
| --- | --- |
| `preventDefault()` | Prevents the browser's default action |
| `stopPropagation()` | Stops further event propagation |
| `stopImmediatePropagation()` | Stops remaining listeners and further propagation |

# 🗁 Custom Events

JavaScript allows developers to create and dispatch custom events.

The `CustomEvent` constructor creates an event with optional custom data.

```javascript
const event = new CustomEvent("userRegistered", {
  detail: {
    id: 101,
    username: "john"
  }
});
```

Register a listener:

```javascript
document.addEventListener("userRegistered", (event) => {
  console.log(event.detail);
});
```

Dispatch the event:

```javascript
document.dispatchEvent(event);
```

Output:

```javascript
{
  id: 101,
  username: "john"
}
```

Custom events are useful for communication between independent components.

# 🗁 Dispatching Events Programmatically

The `dispatchEvent()` method triggers an event on a target element.

Example:

```javascript
const button = document.querySelector("button");

button.addEventListener("click", () => {
  console.log("Click event handled");
});

button.dispatchEvent(new Event("click"));
```

The registered listener executes even though the user did not physically click the button.

Programmatically dispatched events are not trusted user-generated events. Their `isTrusted` property is `false`.

# 🗁 Event Delegation Preview

Event delegation is a technique where a parent element handles events originating from its descendants.

It relies on event bubbling and reduces the need to attach separate listeners to many child elements.

Example:

```html
<ul id="list">
  <li>JavaScript</li>
  <li>TypeScript</li>
  <li>Python</li>
</ul>

<script>
  const list = document.querySelector("#list");

  list.addEventListener("click", (event) => {
    if (event.target.matches("li")) {
      console.log(event.target.textContent);
    }
  });
</script>
```

A single listener handles clicks on all list items.

Event delegation is covered in detail in the next document.

# 🗁 Best Practices

* Prefer `addEventListener()` over inline event handlers.
* Use named functions when listeners need to be removed.
* Use `event.target` and `event.currentTarget` appropriately.
* Avoid unnecessary event listeners on individual elements.
* Use event delegation for dynamic collections of elements.
* Use `preventDefault()` only when overriding the browser's default behavior.
* Avoid stopping propagation unless necessary.
* Use `{ once: true }` for one-time event handlers.
* Use `AbortController` when managing groups of event listeners.
* Keep event handlers small and delegate complex operations to separate functions.
* Consider keyboard accessibility when implementing mouse-based interactions.
* Use passive listeners where appropriate for scroll and touch performance.

# 🗁 Practical Example

Create an interactive counter using DOM events.

```html
<div>
  <h2>Counter: <span id="count">0</span></h2>

  <button id="increment">Increment</button>
  <button id="decrement">Decrement</button>
  <button id="reset">Reset</button>
</div>

<script>
  const countElement = document.querySelector("#count");

  const incrementButton = document.querySelector("#increment");
  const decrementButton = document.querySelector("#decrement");
  const resetButton = document.querySelector("#reset");

  let count = 0;

  function updateCounter() {
    countElement.textContent = count;
  }

  incrementButton.addEventListener("click", () => {
    count++;
    updateCounter();
  });

  decrementButton.addEventListener("click", () => {
    count--;
    updateCounter();
  });

  resetButton.addEventListener("click", () => {
    count = 0;
    updateCounter();
  });
</script>
```

This example demonstrates:

* Selecting DOM elements.
* Registering event listeners.
* Responding to click events.
* Updating DOM content.
* Separating state management from UI updates.

# 🗁 Summary

DOM events provide the foundation for interactive browser applications.

Important concepts include:

* Event listeners and event objects.
* Mouse, keyboard, form, document, and window events.
* Adding and removing event listeners.
* Event listener options.
* Event propagation and its phases.
* Preventing default browser behavior.
* Creating and dispatching custom events.
* Event delegation.

Understanding DOM events is essential for building interactive interfaces, handling user input, and implementing client-side application behavior.
