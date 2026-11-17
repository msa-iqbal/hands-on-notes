# Selecting DOM Elements

JavaScript provides several methods for selecting HTML elements from the DOM (Document Object Model).

Selecting elements allows you to access and manipulate page content, attributes, styles, and behavior.

For example, you can:

* Change an element's text or HTML.
* Modify CSS classes and styles.
* Read or update form values.
* Attach event listeners.
* Create interactive user interfaces.

# 🗁 Understanding the DOM

Consider the following HTML:

```html
<div id="container">
  <h1 class="title">Hello World</h1>
  <p class="description">Welcome to JavaScript.</p>
  <button class="action">Click Me</button>
</div>
```

The browser converts this HTML into a tree of objects called the DOM.

JavaScript can access these objects using the `document` object.

```javascript
const heading = document.querySelector(".title");

console.log(heading.textContent);
```

Output:

```text
Hello World
```

# 🗁 getElementById()

The `getElementById()` method selects an element using its unique `id` attribute.

Syntax:

```javascript
document.getElementById(id);
```

Example:

```html
<h1 id="heading">Hello World</h1>
```

```javascript
const heading = document.getElementById("heading");

console.log(heading);
```

You do not need to include the `#` symbol when passing an ID.

```javascript
document.getElementById("heading");
```

Not:

```javascript
document.getElementById("#heading");
```

If no matching element exists, the method returns `null`.

```javascript
const element = document.getElementById("missing");

console.log(element); // null
```

# 🗁 getElementsByClassName()

The `getElementsByClassName()` method selects all elements containing a specified class name.

It returns a live `HTMLCollection`.

Syntax:

```javascript
document.getElementsByClassName(className);
```

Example:

```html
<p class="message">First message</p>
<p class="message">Second message</p>
<p class="message">Third message</p>
```

```javascript
const messages = document.getElementsByClassName("message");

console.log(messages.length); // 3

console.log(messages[0].textContent);
console.log(messages[1].textContent);
```

You can access individual elements using indexes.

```javascript
const firstMessage = messages[0];
const secondMessage = messages[1];
```

You can iterate over the collection using a traditional loop:

```javascript
for (let i = 0; i < messages.length; i++) {
  console.log(messages[i].textContent);
}
```

`HTMLCollection` is live, meaning it automatically reflects matching DOM changes.

# 🗁 getElementsByTagName()

The `getElementsByTagName()` method selects all elements with a specified HTML tag name.

Syntax:

```javascript
document.getElementsByTagName(tagName);
```

Example:

```html
<p>First paragraph</p>
<p>Second paragraph</p>
<p>Third paragraph</p>
```

```javascript
const paragraphs = document.getElementsByTagName("p");

console.log(paragraphs.length); // 3

console.log(paragraphs[0].textContent);
```

You can select all elements in the document using the wildcard `*`.

```javascript
const allElements = document.getElementsByTagName("*");

console.log(allElements.length);
```

This method also returns a live `HTMLCollection`.

# 🗁 querySelector()

The `querySelector()` method selects the first element that matches a CSS selector.

Syntax:

```javascript
document.querySelector(selector);
```

Example using an ID selector:

```html
<h1 id="heading">Hello World</h1>
```

```javascript
const heading = document.querySelector("#heading");
```

Example using a class selector:

```html
<p class="message">Hello JavaScript</p>
```

```javascript
const message = document.querySelector(".message");
```

Example using a tag selector:

```javascript
const paragraph = document.querySelector("p");
```

Example using an attribute selector:

```html
<input type="email" name="email">
```

```javascript
const emailInput = document.querySelector('input[type="email"]');
```

Example using a descendant selector:

```html
<div class="container">
  <p class="message">Hello</p>
</div>
```

```javascript
const message = document.querySelector(".container .message");
```

If multiple elements match, `querySelector()` returns only the first matching element.

If no element matches, it returns `null`.

# 🗁 querySelectorAll()

The `querySelectorAll()` method selects all elements matching a CSS selector.

Syntax:

```javascript
document.querySelectorAll(selector);
```

Example:

```html
<p class="message">First message</p>
<p class="message">Second message</p>
<p class="message">Third message</p>
```

```javascript
const messages = document.querySelectorAll(".message");

console.log(messages.length); // 3
```

It returns a static `NodeList`.

You can access elements using indexes:

```javascript
console.log(messages[0].textContent);
console.log(messages[1].textContent);
console.log(messages[2].textContent);
```

You can iterate using `forEach()`:

```javascript
messages.forEach((message) => {
  console.log(message.textContent);
});
```

A static `NodeList` does not automatically update when the DOM changes.

```javascript
const messages = document.querySelectorAll(".message");

const newMessage = document.createElement("p");

newMessage.className = "message";
newMessage.textContent = "New message";

document.body.append(newMessage);

console.log(messages.length);
```

The collection length remains unchanged because the `NodeList` represents the elements matched when the query was executed.

Run `querySelectorAll()` again to get an updated collection.

# 🗁 Selecting Elements Inside Another Element

You do not always need to select elements from the entire document.

You can call selection methods on an existing element to search only inside that element.

Example:

```html
<div id="container">
  <h2>Title</h2>
  <p class="message">First message</p>
  <p class="message">Second message</p>
</div>

<p class="message">Outside message</p>
```

```javascript
const container = document.getElementById("container");

const messages = container.querySelectorAll(".message");

messages.forEach((message) => {
  console.log(message.textContent);
});
```

Only the two paragraphs inside the container are selected.

You can also use:

```javascript
container.querySelector(".message");
container.getElementsByClassName("message");
container.getElementsByTagName("p");
```

Note that `getElementById()` is a method of `document`, not a general element-level selection method.

# 🗁 CSS Selectors with querySelector()

The `querySelector()` and `querySelectorAll()` methods support standard CSS selectors.

Common selectors include:

| Selector | Description |
| --- | --- |
| `p` | Selects all paragraph elements |
| `#heading` | Selects an element by ID |
| `.message` | Selects elements by class |
| `*` | Selects all elements |
| `div p` | Selects paragraphs inside div elements |
| `div > p` | Selects direct child paragraphs |
| `h1, h2` | Selects both h1 and h2 elements |
| `input[type="text"]` | Selects text inputs |
| `li:first-child` | Selects the first child list item |
| `li:last-child` | Selects the last child list item |
| `li:nth-child(2)` | Selects the second child list item |
| `button:hover` | Matches buttons in the hover state |

Example:

```html
<div class="container">
  <p class="message">First</p>
  <p class="message">Second</p>
  <span>Other content</span>
</div>
```

```javascript
const firstParagraph = document.querySelector(
  ".container > p:first-child"
);

console.log(firstParagraph.textContent);
```

Output:

```text
First
```

# 🗁 HTMLCollection vs NodeList

Both `HTMLCollection` and `NodeList` represent collections of DOM nodes, but they have important differences.

| Feature | HTMLCollection | NodeList |
| --- | --- | --- |
| Contains | HTML elements | DOM nodes |
| Live or static | Usually live | Depends on the API |
| Index access | Yes | Yes |
| `length` property | Yes | Yes |
| `forEach()` | No | Available on modern NodeList |
| Common source | `getElementsByClassName()` | `querySelectorAll()` |

Important distinction:

* `getElementsByClassName()` returns a live `HTMLCollection`.
* `getElementsByTagName()` returns a live `HTMLCollection`.
* `querySelectorAll()` returns a static `NodeList`.
* `childNodes` returns a live `NodeList`.

You can convert an `HTMLCollection` into an array when you need array methods:

```javascript
const elements = document.getElementsByClassName("message");

const elementsArray = Array.from(elements);

elementsArray.forEach((element) => {
  console.log(element.textContent);
});
```

You can also use the spread operator:

```javascript
const elementsArray = [...elements];
```

# 🗁 Handling Missing Elements

Selection methods do not always find a matching element.

For example:

```javascript
const button = document.querySelector("#submit");

button.addEventListener("click", () => {
  console.log("Button clicked");
});
```

If the button does not exist, `button` will be `null`, and calling `addEventListener()` will cause a `TypeError`.

Check the result before using it:

```javascript
const button = document.querySelector("#submit");

if (button) {
  button.addEventListener("click", () => {
    console.log("Button clicked");
  });
}
```

You can also use optional chaining when an operation can safely be skipped:

```javascript
button?.addEventListener("click", () => {
  console.log("Button clicked");
});
```

For more complex initialization, explicit checks can make missing elements easier to diagnose.

```javascript
const button = document.querySelector("#submit");

if (!button) {
  throw new Error("Submit button was not found.");
}

button.addEventListener("click", () => {
  console.log("Submitted");
});
```

# 🗁 Choosing the Right Selection Method

| Requirement | Recommended method |
| --- | --- |
| Select one element by ID | `getElementById()` |
| Select all elements by class | `getElementsByClassName()` |
| Select all elements by tag | `getElementsByTagName()` |
| Select the first CSS selector match | `querySelector()` |
| Select all CSS selector matches | `querySelectorAll()` |

For most modern DOM development, `querySelector()` and `querySelectorAll()` are convenient because they support flexible CSS selectors.

However, the specialized methods remain useful, especially when selecting elements by ID or working with live collections.

# 🗁 Practical Example

HTML:

```html
<div id="app">
  <h1 class="title">User List</h1>

  <ul class="users">
    <li class="user">Alice</li>
    <li class="user">Bob</li>
    <li class="user">Charlie</li>
  </ul>

  <button id="show-users">Show Users</button>
</div>
```

JavaScript:

```javascript
const app = document.getElementById("app");

const title = app.querySelector(".title");

const users = app.querySelectorAll(".user");

const button = app.querySelector("#show-users");

console.log(title.textContent);

users.forEach((user) => {
  console.log(user.textContent);
});

button.addEventListener("click", () => {
  alert(`Total users: ${users.length}`);
});
```

This example demonstrates how to select elements at different levels of the DOM and use them in JavaScript.

# 🗁 Best Practices

* Prefer `getElementById()` when selecting a single element by its unique ID.
* Use `querySelector()` when you need the first CSS selector match.
* Use `querySelectorAll()` when you need multiple matching elements.
* Scope selections to a parent element when possible.
* Check whether an element exists before accessing its properties or methods.
* Avoid unnecessarily selecting the same elements repeatedly; store frequently used references.
* Remember that `querySelectorAll()` returns a static collection.
* Use meaningful IDs and classes to make DOM selection predictable.
* Ensure the DOM elements exist before running code that accesses them. Use appropriate script placement, such as loading a script with `defer`.

# 🗁 Key Takeaways

* The `document` object provides methods for accessing DOM elements.
* `getElementById()` selects an element by its ID.
* `getElementsByClassName()` selects elements by class and returns a live `HTMLCollection`.
* `getElementsByTagName()` selects elements by tag name and returns a live `HTMLCollection`.
* `querySelector()` returns the first element matching a CSS selector.
* `querySelectorAll()` returns a static `NodeList` containing all matching elements.
* Selection methods can also be called on parent elements to search within a specific DOM subtree.
* A missing element generally results in `null` for single-element selection methods.

# 🗁 References

* [MDN — Document](https://developer.mozilla.org/en-US/docs/Web/API/Document)
* [MDN — Document.getElementById()](https://developer.mozilla.org/en-US/docs/Web/API/Document/getElementById)
* [MDN — Document.querySelector()](https://developer.mozilla.org/en-US/docs/Web/API/Document/querySelector)
* [MDN — Document.querySelectorAll()](https://developer.mozilla.org/en-US/docs/Web/API/Document/querySelectorAll)
* [MDN — HTMLCollection](https://developer.mozilla.org/en-US/docs/Web/API/HTMLCollection)
* [MDN — NodeList](https://developer.mozilla.org/en-US/docs/Web/API/NodeList)
