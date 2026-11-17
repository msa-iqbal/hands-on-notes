# Modifying DOM Elements

JavaScript provides several DOM properties and methods for modifying existing HTML elements.

After selecting an element, you can change its:

* Text content.
* HTML content.
* CSS styles.
* Attributes.
* Classes.
* Child elements.
* Position within the DOM.
* Visibility and other presentation properties.

DOM modification is fundamental to building dynamic and interactive web applications.

# 🗁 textContent

The `textContent` property gets or sets the text content of an element and its descendants.

Example HTML:

```html
<div id="container">
  <h1>Hello World</h1>
  <p>Welcome to JavaScript.</p>
</div>
```

Reading text:

```javascript
const container = document.getElementById("container");

console.log(container.textContent);
```

Output:

```text
Hello World
Welcome to JavaScript.
```

Changing text:

```javascript
container.textContent = "New content";
```

Result:

```html
<div id="container">New content</div>
```

Important behavior:

* Existing child elements are removed when you assign a new value.
* HTML markup inside the assigned string is treated as plain text.
* It includes text from descendant elements.
* It does not interpret HTML tags.

Example:

```javascript
container.textContent = "<strong>Hello</strong>";
```

Result:

```html
<div id="container">&lt;strong&gt;Hello&lt;/strong&gt;</div>
```

The browser displays the literal text rather than creating a `strong` element.

# 🗁 innerHTML

The `innerHTML` property gets or sets the HTML markup inside an element.

Example HTML:

```html
<div id="container">
  <h1>Hello World</h1>
  <p>Welcome to JavaScript.</p>
</div>
```

Reading HTML:

```javascript
const container = document.getElementById("container");

console.log(container.innerHTML);
```

Changing HTML:

```javascript
container.innerHTML = `
  <h2>New Heading</h2>
  <p>New paragraph.</p>
`;
```

Result:

```html
<div id="container">
  <h2>New Heading</h2>
  <p>New paragraph.</p>
</div>
```

Unlike `textContent`, `innerHTML` parses the assigned string as HTML.

Important behavior:

* Existing child elements are replaced.
* New HTML elements are created from the supplied markup.
* Existing event listeners attached to replaced child elements are lost.
* Assigning untrusted content can introduce cross-site scripting (XSS) vulnerabilities.

Use `textContent` when inserting plain text, especially user-provided content.

# 🗁 outerHTML

The `outerHTML` property gets or replaces an element's complete HTML representation, including the element itself.

Example HTML:

```html
<div id="container">
  <p>Hello World</p>
</div>
```

Reading:

```javascript
const container = document.getElementById("container");

console.log(container.outerHTML);
```

Output:

```html
<div id="container">
  <p>Hello World</p>
</div>
```

Replacing:

```javascript
container.outerHTML = `
  <section id="new-container">
    <h2>New Section</h2>
  </section>
`;
```

The original `div` is replaced with a new `section`.

Important distinction:

* `innerHTML` modifies the content inside an element.
* `outerHTML` replaces the element itself.

The JavaScript variable `container` still references the original detached element after replacement.

# 🗁 innerText

The `innerText` property gets or sets the rendered text of an element.

Example HTML:

```html
<div id="container">
  <h1>Hello World</h1>
  <p>Welcome to JavaScript.</p>
</div>
```

Reading:

```javascript
const container = document.getElementById("container");

console.log(container.innerText);
```

Changing:

```javascript
container.innerText = "Updated visible text";
```

`innerText` behaves differently from `textContent`:

| Feature | textContent | innerText |
| --- | --- | --- |
| Returns text from descendants | Yes | Rendered text |
| Considers CSS visibility | No | Yes |
| Includes hidden text | Generally yes | Generally no |
| Triggers layout calculations | Not normally | May require layout information |
| Interprets HTML when assigning | No | No |

Use `textContent` for general text manipulation. Use `innerText` when rendered text and CSS visibility matter.

# 🗁 Changing CSS Styles with style

The `style` property provides access to an element's inline CSS declarations.

Example HTML:

```html
<p id="message">Hello World</p>
```

JavaScript:

```javascript
const message = document.getElementById("message");

message.style.color = "blue";

message.style.backgroundColor = "lightgray";

message.style.fontSize = "20px";

message.style.padding = "10px";
```

The resulting inline styles are equivalent to:

```html
<p
  id="message"
  style="
    color: blue;
    background-color: lightgray;
    font-size: 20px;
    padding: 10px;
  "
>
  Hello World
</p>
```

CSS property names generally use camelCase in JavaScript.

| CSS property | JavaScript property |
| --- | --- |
| `background-color` | `backgroundColor` |
| `font-size` | `fontSize` |
| `margin-top` | `marginTop` |
| `border-radius` | `borderRadius` |
| `z-index` | `zIndex` |
| `text-align` | `textAlign` |

Example:

```javascript
message.style.backgroundColor = "yellow";
message.style.borderRadius = "8px";
```

You can remove an inline style using `removeProperty()`:

```javascript
message.style.removeProperty("color");
```

You can also assign multiple inline declarations:

```javascript
message.style.cssText = `
  color: white;
  background-color: black;
  padding: 12px;
`;
```

Assigning `cssText` replaces the element's existing inline style declarations. Use it carefully.

# 🗁 getComputedStyle()

The `getComputedStyle()` method returns the computed CSS styles of an element.

Computed styles include values resulting from stylesheets, inheritance, and the browser's CSS calculation process.

Example HTML:

```html
<p id="message" class="message">Hello World</p>
```

CSS:

```css
.message {
  color: blue;
  font-size: 20px;
}
```

JavaScript:

```javascript
const message = document.getElementById("message");

const styles = getComputedStyle(message);

console.log(styles.color);

console.log(styles.fontSize);
```

Example output:

```text
rgb(0, 0, 255)
20px
```

Unlike `element.style`, `getComputedStyle()` can read styles applied through external stylesheets and style rules.

It returns a read-only style declaration.

# 🗁 Modifying Classes with classList

The `classList` property provides methods for managing an element's CSS classes.

Consider:

```html
<div id="container" class="box"></div>
```

```javascript
const container = document.getElementById("container");
```

**add()**

Adds one or more classes.

```javascript
container.classList.add("active");

container.classList.add("visible", "highlighted");
```

**remove()**

Removes one or more classes.

```javascript
container.classList.remove("active");

container.classList.remove("visible", "highlighted");
```

**toggle()**

Adds a class if it does not exist or removes it if it already exists.

```javascript
container.classList.toggle("active");
```

You can provide a second argument to force a specific state:

```javascript
container.classList.toggle("active", true);

container.classList.toggle("active", false);
```

**contains()**

Checks whether an element contains a particular class.

```javascript
console.log(container.classList.contains("box")); // true

console.log(container.classList.contains("active")); // false
```

**replace()**

Replaces an existing class with another class.

```javascript
container.classList.replace("box", "card");
```

These methods are generally preferable to manually modifying the entire `className` string.

# 🗁 className

The `className` property gets or sets the complete class attribute of an element.

Example:

```html
<div id="container" class="box active"></div>
```

```javascript
const container = document.getElementById("container");

console.log(container.className);
```

Output:

```text
box active
```

Replacing classes:

```javascript
container.className = "card highlighted";
```

This replaces all existing classes.

To preserve existing classes while adding another, use `classList.add()`.

```javascript
container.classList.add("responsive");
```

# 🗁 Setting and Removing Attributes

DOM elements expose methods for managing HTML attributes.

Consider:

```html
<input id="username" type="text" placeholder="Enter username">
```

Selecting the element:

```javascript
const input = document.getElementById("username");
```

Set an attribute:

```javascript
input.setAttribute("maxlength", "30");

input.setAttribute("required", "");
```

Get an attribute:

```javascript
console.log(input.getAttribute("placeholder"));
```

Output:

```text
Enter username
```

Check whether an attribute exists:

```javascript
console.log(input.hasAttribute("required")); // true
```

Remove an attribute:

```javascript
input.removeAttribute("placeholder");
```

These methods are useful when working with HTML attributes dynamically.

# 🗁 Working with Data Attributes

HTML allows custom data attributes using the `data-*` naming convention.

Example:

```html
<button id="delete-button" data-user-id="42">
  Delete User
</button>
```

JavaScript provides the `dataset` property to access these attributes.

```javascript
const button = document.getElementById("delete-button");

console.log(button.dataset.userId);
```

Output:

```text
42
```

Updating a data attribute:

```javascript
button.dataset.userId = "100";
```

This updates the HTML attribute to:

```html
data-user-id="100"
```

You can also create new data attributes:

```javascript
button.dataset.action = "delete";
```

Result:

```html
data-action="delete"
```

The `dataset` property exposes data attributes as strings.

# 🗁 Modifying Form Element Properties

Form elements expose properties for reading and updating their current values.

Example HTML:

```html
<input id="username" type="text" value="Alice">

<input id="subscribe" type="checkbox">

<select id="country">
  <option value="bd">Bangladesh</option>
  <option value="us">United States</option>
</select>
```

JavaScript:

```javascript
const username = document.getElementById("username");

const subscribe = document.getElementById("subscribe");

const country = document.getElementById("country");
```

Read and update the input value:

```javascript
console.log(username.value);

username.value = "Bob";
```

Read and update the checkbox state:

```javascript
console.log(subscribe.checked);

subscribe.checked = true;
```

Read and update the selected option:

```javascript
console.log(country.value);

country.value = "us";
```

These properties are especially useful when building forms and handling user input.

# 🗁 Removing DOM Elements

The `remove()` method removes an element from the DOM.

Example:

```html
<div id="container">
  <p id="message">Hello World</p>
</div>
```

```javascript
const message = document.getElementById("message");

message.remove();
```

The paragraph is removed from its parent.

You can also use `removeChild()`:

```javascript
const container = document.getElementById("container");

const message = container.querySelector("#message");

container.removeChild(message);
```

The `removeChild()` method requires the node to be a child of the specified parent.

# 🗁 Replacing DOM Elements

The `replaceWith()` method replaces an element with one or more nodes or strings.

Example:

```html
<div id="container">
  <p id="old-message">Old Message</p>
</div>
```

```javascript
const oldMessage = document.getElementById("old-message");

const newMessage = document.createElement("p");

newMessage.textContent = "New Message";

oldMessage.replaceWith(newMessage);
```

You can also use `replaceChild()`:

```javascript
const container = document.getElementById("container");

const oldMessage = container.querySelector("#old-message");

const newMessage = document.createElement("p");

newMessage.textContent = "Replacement Message";

container.replaceChild(newMessage, oldMessage);
```

The method signatures differ:

```javascript
newElement.replaceWith(oldElement);
```

```javascript
parent.replaceChild(newElement, oldElement);
```

# 🗁 Working with Child Elements

JavaScript provides several properties for accessing an element's children.

**children**

Returns an `HTMLCollection` containing only element children.

```javascript
const container = document.getElementById("container");

console.log(container.children);
```

**childNodes**

Returns a `NodeList` containing all child nodes, including text nodes and comments.

```javascript
console.log(container.childNodes);
```

**firstElementChild**

Returns the first child element.

```javascript
console.log(container.firstElementChild);
```

**lastElementChild**

Returns the last child element.

```javascript
console.log(container.lastElementChild);
```

**parentElement**

Returns the parent element.

```javascript
const child = document.querySelector(".child");

console.log(child.parentElement);
```

**nextElementSibling**

Returns the next sibling element.

```javascript
console.log(child.nextElementSibling);
```

**previousElementSibling**

Returns the previous sibling element.

```javascript
console.log(child.previousElementSibling);
```

These properties are useful when modifying DOM structures without repeatedly searching the entire document.

# 🗁 Practical Example: Dynamic Content Editor

HTML:

```html
<div id="app">
  <h1 id="title">Original Title</h1>

  <p id="description">Original description.</p>

  <button id="update">Update Content</button>

  <button id="reset">Reset Content</button>
</div>
```

JavaScript:

```javascript
const title = document.getElementById("title");

const description = document.getElementById("description");

const updateButton = document.getElementById("update");

const resetButton = document.getElementById("reset");

updateButton.addEventListener("click", () => {
  title.textContent = "Updated Title";

  description.textContent = "The content has been updated.";

  title.classList.add("updated");

  description.style.color = "green";
});

resetButton.addEventListener("click", () => {
  title.textContent = "Original Title";

  description.textContent = "Original description.";

  title.classList.remove("updated");

  description.style.removeProperty("color");
});
```

CSS:

```css
.updated {
  color: blue;
  font-weight: bold;
}
```

This example demonstrates how JavaScript can update text, CSS classes, and inline styles in response to user interactions.

# 🗁 Common Mistakes

**Confusing textContent and innerHTML**

```javascript
element.textContent = "<h1>Hello</h1>";
```

This inserts plain text rather than creating a heading.

Use `innerHTML` only when actual HTML parsing is required and the input is trusted or properly sanitized.

**Replacing all existing classes accidentally**

```javascript
element.className = "active";
```

This removes every other class.

Use:

```javascript
element.classList.add("active");
```

**Trying to modify a missing element**

```javascript
const element = document.querySelector(".missing");

element.textContent = "Hello";
```

If no element matches, accessing `textContent` causes an error.

Check the result:

```javascript
if (element) {
  element.textContent = "Hello";
}
```

**Confusing inline styles with computed styles**

```javascript
console.log(element.style.color);
```

This reads the inline style declaration, not necessarily the final CSS color applied by the browser.

Use:

```javascript
console.log(getComputedStyle(element).color);
```

**Keeping references to replaced elements**

When an element is replaced through `outerHTML`, `replaceWith()`, or similar operations, existing references continue pointing to the old detached element.

Select the replacement when you need to work with it.

# 🗁 Best Practices

* Use `textContent` for plain text updates.
* Use `innerHTML` only when HTML parsing is necessary and the content is trusted or sanitized.
* Prefer CSS classes over extensive inline style manipulation.
* Use `classList` to manage individual classes.
* Use `setAttribute()` and `removeAttribute()` when managing HTML attributes.
* Use `dataset` for custom `data-*` attributes.
* Use form properties such as `value` and `checked` for current form state.
* Check that selected elements exist before modifying them.
* Avoid unnecessarily replacing elements when updating their existing content is sufficient.
* Keep presentation rules in CSS and application behavior in JavaScript.

# 🗁 Key Takeaways

* `textContent` reads or updates text content.
* `innerHTML` reads or replaces the HTML inside an element.
* `outerHTML` reads or replaces the complete element.
* `innerText` works with rendered text.
* `style` modifies inline CSS declarations.
* `getComputedStyle()` reads computed CSS values.
* `classList` manages CSS classes.
* `setAttribute()`, `getAttribute()`, `hasAttribute()`, and `removeAttribute()` manage attributes.
* `dataset` provides access to custom data attributes.
* `remove()` and `removeChild()` remove elements.
* `replaceWith()` and `replaceChild()` replace elements.
* Child and sibling properties help navigate and modify DOM structures.

# 🗁 References

* [MDN — Node.textContent](https://developer.mozilla.org/en-US/docs/Web/API/Node/textContent)
* [MDN — Element.innerHTML](https://developer.mozilla.org/en-US/docs/Web/API/Element/innerHTML)
* [MDN — HTMLElement.innerText](https://developer.mozilla.org/en-US/docs/Web/API/HTMLElement/innerText)
* [MDN — HTMLElement.style](https://developer.mozilla.org/en-US/docs/Web/API/HTMLElement/style)
* [MDN — Window.getComputedStyle()](https://developer.mozilla.org/en-US/docs/Web/API/Window/getComputedStyle)
* [MDN — Element.classList](https://developer.mozilla.org/en-US/docs/Web/API/Element/classList)
* [MDN — Element.setAttribute()](https://developer.mozilla.org/en-US/docs/Web/API/Element/setAttribute)
* [MDN — Element.remove()](https://developer.mozilla.org/en-US/docs/Web/API/Element/remove)
* [MDN — Element.replaceWith()](https://developer.mozilla.org/en-US/docs/Web/API/Element/replaceWith)
