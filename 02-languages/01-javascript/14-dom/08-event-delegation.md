# Event Delegation

Event delegation is a JavaScript technique where a single event listener is attached to a parent element to handle events triggered by its child elements.

It works by taking advantage of **event bubbling**, where an event travels from its target element through its ancestor elements.

Instead of attaching separate event listeners to every child, we attach one listener to a common parent.

This approach is particularly useful when working with:

* Dynamic lists
* Tables
* Menus
* Navigation items
* Repeated UI components
* Elements created dynamically with JavaScript

# 🗁 Understanding the Problem

Consider a list containing multiple items.

Without event delegation, we might attach an event listener to every list item.

```html
<ul id="list">
  <li>JavaScript</li>
  <li>TypeScript</li>
  <li>Python</li>
</ul>

<script>
  const items = document.querySelectorAll("#list li");

  items.forEach((item) => {
    item.addEventListener("click", () => {
      console.log(item.textContent);
    });
  });
</script>
```

This works, but every item requires its own event listener.

There are some limitations:

* More listeners must be registered.
* Dynamically added elements do not automatically receive listeners.
* Managing listeners across many elements becomes more complicated.

Event delegation provides a simpler approach.

# 🗁 How Event Delegation Works

Event delegation follows this process:

* Attach an event listener to a common parent.
* A child element triggers an event.
* The event bubbles up through its ancestors.
* The parent listener receives the event.
* Use `event.target` to identify the original event target.
* Determine whether the target matches the intended child element.
* Execute the appropriate action.

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

The parent handles clicks from all its matching child elements.

# 🗁 event.target

The `event.target` property identifies the original element where the event occurred.

In event delegation, it helps determine which descendant triggered the event.

```javascript
list.addEventListener("click", (event) => {
  console.log(event.target);
});
```

If the user clicks a list item, `event.target` identifies that list item.

However, the target may be a nested element rather than the expected child.

Example:

```html
<ul id="list">
  <li>
    <span>JavaScript</span>
  </li>
</ul>
```

If the user clicks the `span`, the target is the `span`, not the `li`.

This is why `matches()` alone may not be sufficient.

# 🗁 closest()

The `closest()` method searches the element itself and its ancestors until it finds an element matching a CSS selector.

It returns the matching element or `null`.

Syntax:

```javascript
element.closest(selector);
```

Example:

```html
<ul id="list">
  <li>
    <span>JavaScript</span>
  </li>
</ul>

<script>
  const list = document.querySelector("#list");

  list.addEventListener("click", (event) => {
    const item = event.target.closest("li");

    if (item && list.contains(item)) {
      console.log(item.textContent.trim());
    }
  });
</script>
```

This works even when the user clicks a nested element inside the list item.

The `list.contains(item)` check ensures that the matched element belongs to the intended parent.

# 🗁 Event Delegation with Dynamic Elements

One of the biggest advantages of event delegation is that it works with elements added after the listener has been registered.

Example:

```html
<ul id="list">
  <li>JavaScript</li>
  <li>TypeScript</li>
</ul>

<button id="addItem">Add Item</button>

<script>
  const list = document.querySelector("#list");
  const addButton = document.querySelector("#addItem");

  list.addEventListener("click", (event) => {
    const item = event.target.closest("li");

    if (item && list.contains(item)) {
      console.log("Clicked:", item.textContent);
    }
  });

  addButton.addEventListener("click", () => {
    const item = document.createElement("li");

    item.textContent = "Python";

    list.appendChild(item);
  });
</script>
```

The newly created list item automatically works with the existing delegated listener.

No additional event listener needs to be registered on the new item.

# 🗁 Event Delegation with Multiple Actions

A parent can handle different actions based on attributes or selectors.

Example:

```html
<div id="actions">
  <button data-action="save">Save</button>
  <button data-action="edit">Edit</button>
  <button data-action="delete">Delete</button>
</div>

<script>
  const actions = document.querySelector("#actions");

  actions.addEventListener("click", (event) => {
    const button = event.target.closest("button[data-action]");

    if (!button || !actions.contains(button)) {
      return;
    }

    const action = button.dataset.action;

    switch (action) {
      case "save":
        console.log("Saving...");
        break;

      case "edit":
        console.log("Editing...");
        break;

      case "delete":
        console.log("Deleting...");
        break;
    }
  });
</script>
```

The `data-action` attribute identifies the intended operation.

This pattern is useful for toolbars, action menus, and reusable components.

# 🗁 Event Delegation with Tables

Event delegation is useful for tables containing many rows.

Example:

```html
<table id="users">
  <tbody>
    <tr>
      <td>John</td>
      <td>
        <button data-action="edit">Edit</button>
        <button data-action="delete">Delete</button>
      </td>
    </tr>

    <tr>
      <td>Sarah</td>
      <td>
        <button data-action="edit">Edit</button>
        <button data-action="delete">Delete</button>
      </td>
    </tr>
  </tbody>
</table>

<script>
  const table = document.querySelector("#users");

  table.addEventListener("click", (event) => {
    const button = event.target.closest("button[data-action]");

    if (!button || !table.contains(button)) {
      return;
    }

    const row = button.closest("tr");
    const name = row.querySelector("td").textContent;

    const action = button.dataset.action;

    console.log(`${action}: ${name}`);
  });
</script>
```

A single listener handles actions across every table row.

# 🗁 Event Delegation and currentTarget

The `event.currentTarget` property identifies the element whose listener is executing.

In event delegation, it usually identifies the parent element.

```javascript
list.addEventListener("click", (event) => {
  console.log("Target:", event.target);
  console.log("Current Target:", event.currentTarget);
});
```

For a click on a nested list item:

* `event.target` identifies the original clicked element.
* `event.currentTarget` identifies the parent with the registered listener.

Use `event.target` to determine what was clicked and `event.currentTarget` to identify the element handling the event.

# 🗁 Event Delegation and stopPropagation()

Event delegation depends on events reaching the parent through propagation.

Calling `stopPropagation()` on a descendant can prevent the parent listener from receiving the event.

Example:

```javascript
const child = document.querySelector("#child");
const parent = document.querySelector("#parent");

child.addEventListener("click", (event) => {
  event.stopPropagation();
});

parent.addEventListener("click", () => {
  console.log("Parent listener");
});
```

Clicking the child prevents the parent listener from executing.

Be careful when using propagation control inside components that rely on delegated events.

# 🗁 Capturing vs Bubbling

Most common event delegation implementations use event bubbling.

However, event listeners can also be registered during the capturing phase.

```javascript
parent.addEventListener(
  "click",
  (event) => {
    console.log("Capturing phase");
  },
  { capture: true }
);
```

The listener executes during the capturing phase instead of the bubbling phase.

Capturing can be useful when handling events that need to be observed before they reach their target.

# 🗁 Events That Do Not Bubble

Not every DOM event bubbles.

Examples of events that do not normally bubble include:

* `focus`
* `blur`
* `mouseenter`
* `mouseleave`

For some of these events, bubbling alternatives exist:

| Non-bubbling event | Bubbling alternative |
| --- | --- |
| `focus` | `focusin` |
| `blur` | `focusout` |
| `mouseenter` | `mouseover` |
| `mouseleave` | `mouseout` |

For example, focus delegation can use `focusin`:

```javascript
const form = document.querySelector("form");

form.addEventListener("focusin", (event) => {
  console.log("Focused element:", event.target);
});
```

Some non-bubbling events can also be observed through capturing, depending on the event.

# 🗁 Event Delegation with Forms

A parent form can handle input events from multiple form controls.

Example:

```html
<form id="profileForm">
  <input name="username" placeholder="Username">
  <input name="email" placeholder="Email">

  <select name="country">
    <option value="bd">Bangladesh</option>
    <option value="us">United States</option>
  </select>
</form>

<script>
  const form = document.querySelector("#profileForm");

  form.addEventListener("input", (event) => {
    const field = event.target;

    if (!field.matches("input, select, textarea")) {
      return;
    }

    console.log(field.name, field.value);
  });

  form.addEventListener("change", (event) => {
    console.log("Changed:", event.target.name);
  });
</script>
```

This approach can simplify handling multiple form controls.

# 🗁 Event Delegation vs Individual Event Listeners

| Feature | Individual Listeners | Event Delegation |
| --- | --- | --- |
| Listener placement | On each child | On a common parent |
| Number of listeners | Potentially many | Usually fewer |
| Dynamic elements | Require additional handling | Automatically supported within the parent |
| Event propagation | Not required for direct handling | Usually relies on bubbling |
| Implementation | Straightforward for a few elements | Useful for collections |
| Performance | Can create many listener registrations | Can reduce listener registrations |

Event delegation does not automatically guarantee better performance in every situation. For a small number of elements, individual listeners may be simpler and entirely sufficient.

# 🗁 Advantages

* Reduces the number of event listeners.
* Handles dynamically created elements.
* Simplifies event management for repeated elements.
* Reduces the need to register and remove listeners individually.
* Supports centralized handling of related UI actions.
* Makes large lists and tables easier to manage.

# 🗁 Limitations

* Requires events that reach the delegated ancestor.
* Event target identification requires careful handling.
* Nested elements can complicate selector matching.
* Excessive delegation can make event handlers difficult to maintain.
* Propagation control can interfere with delegated listeners.
* A single parent listener may need additional logic to distinguish many actions.

# 🗁 Best Practices

* Attach listeners to the nearest suitable common ancestor.
* Use `closest()` when child elements may contain nested elements.
* Verify that the matched element belongs to the intended container.
* Use `data-*` attributes to identify actions.
* Keep delegated event handlers small and readable.
* Prefer bubbling events when using delegation.
* Avoid unnecessary calls to `stopPropagation()`.
* Use individual listeners when there are only a few independent elements.
* Do not assume every DOM event bubbles.
* Separate event handling from business logic.

# 🗁 Practical Example: Dynamic Todo List

Build a todo list where items can be completed or deleted using event delegation.

```html
<div>
  <input id="todoInput" type="text" placeholder="Enter a task">
  <button id="addTodo">Add Todo</button>

  <ul id="todoList"></ul>
</div>

<script>
  const input = document.querySelector("#todoInput");
  const addButton = document.querySelector("#addTodo");
  const todoList = document.querySelector("#todoList");

  addButton.addEventListener("click", () => {
    const value = input.value.trim();

    if (!value) {
      return;
    }

    const item = document.createElement("li");

    item.innerHTML = `
      <span>${value}</span>
      <button data-action="complete">Complete</button>
      <button data-action="delete">Delete</button>
    `;

    todoList.appendChild(item);

    input.value = "";
  });

  todoList.addEventListener("click", (event) => {
    const button = event.target.closest("button[data-action]");

    if (!button || !todoList.contains(button)) {
      return;
    }

    const item = button.closest("li");

    switch (button.dataset.action) {
      case "complete":
        item.querySelector("span").classList.toggle("completed");
        break;

      case "delete":
        item.remove();
        break;
    }
  });
</script>
```

In production code, avoid inserting untrusted user input through `innerHTML`. Create the elements with DOM methods and assign user-provided text through `textContent`.

A safer item creation implementation:

```javascript
function createTodoItem(value) {
  const item = document.createElement("li");
  const text = document.createElement("span");

  const completeButton = document.createElement("button");
  completeButton.textContent = "Complete";
  completeButton.dataset.action = "complete";

  const deleteButton = document.createElement("button");
  deleteButton.textContent = "Delete";
  deleteButton.dataset.action = "delete";

  text.textContent = value;

  item.append(text, completeButton, deleteButton);

  return item;
}
```

Use it inside the add button listener:

```javascript
addButton.addEventListener("click", () => {
  const value = input.value.trim();

  if (!value) {
    return;
  }

  todoList.appendChild(createTodoItem(value));

  input.value = "";
});
```

# 🗁 Summary

Event delegation is a technique for handling events from multiple child elements through a shared parent listener.

Important concepts include:

* Event bubbling
* `event.target`
* `event.currentTarget`
* `closest()`
* Dynamic element handling
* `data-*` attributes
* Event propagation
* Bubbling and non-bubbling events
* Centralized event handling

Event delegation is especially useful when building dynamic interfaces with lists, tables, menus, and repeated components.
