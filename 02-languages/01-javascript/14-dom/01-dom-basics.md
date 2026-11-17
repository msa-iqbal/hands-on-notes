# JavaScript DOM Basics

The Document Object Model (DOM) is a programming interface that represents an HTML or XML document as a structured tree of objects.

JavaScript uses the DOM to access, manipulate, create, and remove HTML elements dynamically.

The DOM allows developers to build interactive web applications by changing page content, structure, attributes, styles, and behavior without manually rewriting the HTML document.

# 🗁 What Is the DOM?

DOM stands for **Document Object Model**.

When a browser loads an HTML document, it parses the HTML and creates an object-based representation of the document.

JavaScript can interact with this representation through the DOM API.

**Example HTML**

```html
<!DOCTYPE html>
<html>
<head>
    <title>DOM Example</title>
</head>
<body>
    <h1>Hello, World!</h1>
    <p>Learning JavaScript DOM.</p>
</body>
</html>
```

The browser creates a document tree representing these elements.

JavaScript can access the document:

```javascript
console.log(document);
```

The `document` object represents the loaded HTML document.

# 🗁 Why Is the DOM Important?

The DOM allows JavaScript to make web pages interactive and dynamic.

Common operations include:

* Selecting HTML elements.
* Reading and modifying text content.
* Changing HTML attributes.
* Modifying CSS styles.
* Creating new elements.
* Removing existing elements.
* Handling user events.
* Managing forms.
* Updating the page without reloading it.

For example:

```html
<h1 id="heading">Original Heading</h1>

<script>
    const heading = document.getElementById("heading");

    heading.textContent = "Updated Heading";
</script>
```

JavaScript changes the heading after accessing its DOM element.

# 🗁 DOM Tree Structure

The DOM represents a document as a hierarchical tree of nodes.

Consider this HTML:

```html
<!DOCTYPE html>
<html>
<head>
    <title>My Website</title>
</head>
<body>
    <h1>Welcome</h1>

    <p>Learning DOM.</p>
</body>
</html>
```

Its simplified DOM structure looks like this:

```text
Document
└── html
    ├── head
    │   └── title
    │       └── "My Website"
    └── body
        ├── h1
        │   └── "Welcome"
        └── p
            └── "Learning DOM."
```

Each element, text value, and other document component is represented by a node.

The tree structure establishes relationships between nodes.

# 🗁 DOM Nodes

A node is a fundamental object in the DOM tree.

The DOM defines several node types.

| Node Type | Constant | Description |
| --- | --- | --- |
| Element | `Node.ELEMENT_NODE` | HTML elements |
| Attribute | `Node.ATTRIBUTE_NODE` | Element attributes |
| Text | `Node.TEXT_NODE` | Text inside elements |
| Comment | `Node.COMMENT_NODE` | HTML comments |
| Document | `Node.DOCUMENT_NODE` | The entire document |
| Document Type | `Node.DOCUMENT_TYPE_NODE` | Document type declaration |
| Document Fragment | `Node.DOCUMENT_FRAGMENT_NODE` | Lightweight document container |

Example:

```html
<body>
    <h1>Hello</h1>
    <!-- This is a comment -->
</body>
```

The `<h1>` is an element node, `Hello` is a text node, and the HTML comment is a comment node.

You can inspect node types using JavaScript:

```javascript
const heading = document.querySelector("h1");

console.log(heading.nodeType);
console.log(heading.firstChild.nodeType);
```

**Output**

```text
1
3
```

`1` represents an element node, while `3` represents a text node.

# 🗁 Elements vs Nodes

Every HTML element is a node, but not every node is an HTML element.

For example:

```html
<div>
    Hello
    <p>Welcome</p>
</div>
```

This structure contains:

* One `div` element node.
* One text node containing `Hello`.
* One `p` element node.
* One text node containing `Welcome`.

This distinction is important when traversing the DOM.

# 🗁 The window Object

The `window` object represents the browser window and serves as the global object in a traditional browser script environment.

It provides access to browser-related functionality.

```javascript
console.log(window);
```

The `window` object provides access to:

* `document`
* `location`
* `history`
* `navigator`
* `localStorage`
* `sessionStorage`
* `alert()`
* `setTimeout()`
* `setInterval()`

Example:

```javascript
console.log(window.document);
console.log(window.location.href);
console.log(window.innerWidth);
```

In browser scripts, many properties and methods of `window` can be accessed without explicitly writing `window.`.

```javascript
console.log(document);
```

This is equivalent to:

```javascript
console.log(window.document);
```

The global environment can differ between classic scripts, ES modules, and other JavaScript execution environments.

# 🗁 The document Object

The `document` object represents the current HTML document.

It provides methods and properties for accessing and manipulating the page.

```javascript
console.log(document);
```

Some commonly used document properties include:

| Property | Description |
| --- | --- |
| `document.title` | Gets or sets the page title |
| `document.URL` | Returns the document URL |
| `document.documentURI` | Returns the document URI |
| `document.body` | Returns the body element |
| `document.head` | Returns the head element |
| `document.documentElement` | Returns the root HTML element |
| `document.forms` | Returns the document's forms |
| `document.images` | Returns the document's images |
| `document.links` | Returns the document's links |
| `document.scripts` | Returns the document's scripts |

Example:

```javascript
console.log(document.title);
console.log(document.body);
console.log(document.head);
console.log(document.documentElement);
```

# 🗁 document.documentElement

The `document.documentElement` property returns the root element of the document.

For an HTML document, this is the `<html>` element.

```javascript
const root = document.documentElement;

console.log(root);
console.log(root.tagName);
```

**Output**

```text
HTML
```

It can be used to access the entire HTML document structure.

# 🗁 document.head and document.body

The `document.head` property returns the document's `<head>` element.

The `document.body` property returns the document's `<body>` element.

```javascript
const head = document.head;
const body = document.body;

console.log(head);
console.log(body);
```

You can also modify their contents:

```javascript
document.title = "JavaScript DOM";

document.body.style.backgroundColor = "lightgray";
```

# 🗁 DOM and HTML

HTML and DOM are related but different concepts.

**HTML** is a markup language used to describe the structure of a web page.

**DOM** is the object-based representation of that document that JavaScript can interact with.

For example:

```html
<h1 id="title">Hello</h1>
```

The browser parses this HTML and creates an element object.

JavaScript can then modify that object:

```javascript
const title = document.getElementById("title");

title.textContent = "Welcome to JavaScript";
```

The DOM changes, and the browser updates the rendered page.

# 🗁 DOM and CSSOM

Browsers use different models to represent document structure and styling.

**DOM (Document Object Model)**

Represents the document's structure and content.

**CSSOM (CSS Object Model)**

Represents CSS styles and rules.

The browser uses these models as part of the rendering process.

Simplified rendering workflow:

```text
HTML
  ↓
DOM Tree

CSS
  ↓
CSSOM Tree

DOM + CSSOM
  ↓
Render Tree
  ↓
Layout
  ↓
Paint
  ↓
Composite
  ↓
Rendered Page
```

The actual browser rendering pipeline can be more complex and may reuse or optimize different stages.

# 🗁 DOM Manipulation

DOM manipulation means changing the document structure, content, or properties through JavaScript.

Example:

```html
<h1 id="title">Old Title</h1>
```

```javascript
const title = document.getElementById("title");

title.textContent = "New Title";
```

The heading changes without modifying the original HTML source file.

Common DOM manipulation operations include:

* Changing text.
* Changing HTML content.
* Updating attributes.
* Changing styles.
* Adding elements.
* Removing elements.
* Replacing elements.

# 🗁 DOM Selection

DOM selection means finding HTML elements through JavaScript.

For example:

```html
<h1 id="title">Hello</h1>
<p class="description">Learning DOM</p>
```

```javascript
const title = document.getElementById("title");

const description = document.querySelector(".description");

console.log(title);
console.log(description);
```

Common selection methods include:

| Method | Description |
| --- | --- |
| `getElementById()` | Selects an element by ID |
| `getElementsByClassName()` | Selects elements by class name |
| `getElementsByTagName()` | Selects elements by tag name |
| `querySelector()` | Selects the first matching CSS selector |
| `querySelectorAll()` | Selects all matching CSS selectors |

These methods are covered in detail in the next DOM documentation topic.

# 🗁 DOM Manipulation vs Direct HTML Editing

Direct HTML editing changes the source markup.

DOM manipulation changes the document representation currently managed by the browser.

Example HTML:

```html
<p id="message">Hello</p>
```

JavaScript:

```javascript
const message = document.querySelector("#message");

message.textContent = "Updated through JavaScript";
```

The browser updates the visible content, but the original HTML file on the server is not automatically modified.

If the page is reloaded, the original server-provided HTML may be loaded again.

To persist application data, use appropriate storage or backend APIs.

# 🗁 DOM and JavaScript Execution

JavaScript must access the relevant DOM elements after they are available.

Consider this HTML:

```html
<head>
    <script>
        const heading = document.querySelector("h1");

        console.log(heading);
    </script>
</head>

<body>
    <h1>Hello World</h1>
</body>
```

The script executes before the browser has parsed the `<h1>` element.

Therefore, the selection returns `null`.

There are several ways to solve this.

**Place the script before the closing body tag**

```html
<body>
    <h1>Hello World</h1>

    <script src="app.js"></script>
</body>
```

**Use the defer attribute**

```html
<head>
    <script src="app.js" defer></script>
</head>
```

A deferred classic external script executes after HTML parsing has completed and before the `DOMContentLoaded` event.

**Use DOMContentLoaded**

```javascript
document.addEventListener("DOMContentLoaded", () => {
    const heading = document.querySelector("h1");

    console.log(heading);
});
```

This event fires when the HTML document has been completely parsed and deferred scripts have executed.

# 🗁 DOMContentLoaded Event

The `DOMContentLoaded` event indicates that the document has been parsed and its deferred scripts have executed.

It does not wait for all images, stylesheets, and other external resources to finish loading.

Example:

```javascript
document.addEventListener("DOMContentLoaded", () => {
    console.log("DOM is ready.");

    const heading = document.querySelector("h1");

    console.log(heading);
});
```

This is useful when JavaScript must initialize after the document structure is available.

# 🗁 load Event

The `load` event fires when the page and its dependent resources have finished loading.

```javascript
window.addEventListener("load", () => {
    console.log("Page and dependent resources loaded.");
});
```

Unlike `DOMContentLoaded`, the `load` event generally waits for resources such as images, stylesheets, and frames.

| Event | When It Fires |
| --- | --- |
| `DOMContentLoaded` | HTML parsing and deferred script execution are complete |
| `load` | The page and its dependent resources have finished loading |

# 🗁 DOM API

The DOM API is a collection of interfaces, properties, and methods that allow JavaScript to interact with documents.

Some important DOM interfaces include:

| Interface | Purpose |
| --- | --- |
| `Document` | Represents the document |
| `Node` | Base interface for DOM nodes |
| `Element` | Represents an element |
| `HTMLElement` | Base interface for HTML elements |
| `Text` | Represents text nodes |
| `DocumentFragment` | Represents a lightweight document container |
| `Event` | Represents an event |
| `EventTarget` | Provides event-listening functionality |
| `DOMTokenList` | Represents a collection of tokens, such as element classes |
| `NodeList` | Represents a collection of nodes |
| `HTMLCollection` | Represents a live collection of HTML elements |

These interfaces form the foundation of browser DOM programming.

# 🗁 DOM in Browser Developer Tools

Browser Developer Tools allow developers to inspect and debug the DOM.

In Chrome or Chromium-based browsers:

* Open Developer Tools with `F12` or `Ctrl + Shift + I`.
* Select the Elements panel.
* Inspect the HTML structure.
* Expand and collapse elements.
* Modify attributes and styles.
* Inspect computed CSS.
* View DOM changes.
* Use the Console to execute JavaScript.

For example, execute this in the browser console:

```javascript
document.body.style.backgroundColor = "lightblue";
```

The page background changes immediately.

Developer Tools are essential for understanding and debugging DOM behavior.

# 🗁 DOM vs Virtual DOM

The DOM and Virtual DOM are different concepts.

**DOM**

The browser's actual document object model.

**Virtual DOM**

A programming concept used by certain UI libraries and frameworks to represent UI structure in JavaScript objects.

For example, React uses a reconciliation process to determine which changes need to be applied to the actual DOM.

| Feature | DOM | Virtual DOM |
| --- | --- | --- |
| Representation | Browser document objects | JavaScript representation of UI |
| Managed by | Browser | Library or framework |
| Purpose | Document access and manipulation | UI representation and reconciliation |
| Direct browser API | Yes | No |
| Example | `document.querySelector()` | React elements and reconciliation |

A Virtual DOM is not a replacement for the browser DOM. It is an abstraction used by certain UI technologies.

# 🗁 Common DOM Properties

The following properties are frequently used when working with DOM nodes.

| Property | Description |
| --- | --- |
| `nodeName` | Returns the node name |
| `nodeType` | Returns the node type |
| `nodeValue` | Gets or sets the node value where applicable |
| `textContent` | Gets or sets text content |
| `parentNode` | Returns the parent node |
| `parentElement` | Returns the parent element |
| `childNodes` | Returns all child nodes |
| `children` | Returns child elements |
| `firstChild` | Returns the first child node |
| `lastChild` | Returns the last child node |
| `firstElementChild` | Returns the first child element |
| `lastElementChild` | Returns the last child element |
| `nextSibling` | Returns the next sibling node |
| `previousSibling` | Returns the previous sibling node |
| `nextElementSibling` | Returns the next sibling element |
| `previousElementSibling` | Returns the previous sibling element |

Example:

```html
<div id="container">
    <h1>Title</h1>
    <p>Description</p>
</div>
```

```javascript
const container = document.querySelector("#container");

console.log(container.children);
console.log(container.firstElementChild);
console.log(container.lastElementChild);
console.log(container.childNodes);
```

The difference between `children` and `childNodes` is important:

* `children` returns element children.
* `childNodes` returns all child nodes, including text and comment nodes.

# 🗁 DOM Performance Considerations

DOM operations can affect browser rendering performance.

Some useful practices include:

* Avoid unnecessary DOM updates.
* Cache frequently accessed element references when appropriate.
* Prefer updating only the required elements.
* Use `DocumentFragment` when building large groups of nodes.
* Avoid repeatedly reading layout-dependent properties while writing styles in a tight loop.
* Use event delegation for large collections of interactive elements.
* Avoid unnecessary layout-triggering operations.

Example using `DocumentFragment`:

```javascript
const list = document.querySelector("#list");

const fragment = document.createDocumentFragment();

for (let i = 1; i <= 5; i++) {
    const item = document.createElement("li");

    item.textContent = `Item ${i}`;

    fragment.appendChild(item);
}

list.appendChild(fragment);
```

The fragment allows multiple nodes to be assembled before being added to the document.

# 🗁 Best Practices

* Understand the DOM tree before manipulating elements.
* Use semantic HTML to create meaningful document structures.
* Keep JavaScript separate from HTML when practical.
* Load scripts at appropriate times using `defer`, modules, or event listeners.
* Use the DOM API instead of manually constructing complex HTML strings.
* Prefer `textContent` when inserting plain text.
* Validate that selected elements exist before using them.
* Avoid unnecessary DOM updates.
* Use browser Developer Tools to inspect and debug DOM behavior.
* Understand the difference between nodes and elements.
* Use `DocumentFragment` for efficiently assembling larger DOM structures.
* Avoid excessive global variables and keep application logic modular.

# 🗁 Quick Reference

| Concept | Description |
| --- | --- |
| DOM | Object-based representation of a document |
| DOM Tree | Hierarchical structure of document nodes |
| Node | Basic unit of the DOM tree |
| Element | HTML element represented as a DOM object |
| `window` | Browser window and global browser environment |
| `document` | Current document object |
| `documentElement` | Root HTML element |
| `document.head` | Head element |
| `document.body` | Body element |
| DOM API | Interfaces and methods for document interaction |
| CSSOM | Object representation of CSS styles |
| DOMContentLoaded | Event fired after HTML parsing and deferred scripts |
| load | Event fired after dependent resources finish loading |
| Virtual DOM | UI representation used by certain libraries |
| DocumentFragment | Lightweight container for assembling nodes |

# 🗁 Practice Exercises

* Create an HTML document and inspect its DOM tree.
* Identify element nodes, text nodes, and comment nodes.
* Print the `document` object in the browser console.
* Access `document.title`, `document.head`, and `document.body`.
* Access the root element using `document.documentElement`.
* Inspect the `nodeType` of different DOM nodes.
* Compare `children` and `childNodes`.
* Change an HTML heading using JavaScript.
* Change the page background through the DOM.
* Execute JavaScript after the `DOMContentLoaded` event.
* Compare `DOMContentLoaded` and `load`.
* Inspect DOM changes using browser Developer Tools.
* Create multiple elements using `DocumentFragment`.
* Build a simple page that dynamically updates its content.

# 🗁 Key Takeaways

* DOM stands for Document Object Model.
* The browser creates a DOM tree when it parses an HTML document.
* JavaScript uses the DOM API to access and manipulate document elements.
* Every HTML element is a node, but not every node is an element.
* The `window` object represents the browser window.
* The `document` object represents the current HTML document.
* DOM manipulation can change content, structure, attributes, and styles.
* `DOMContentLoaded` and `load` represent different loading stages.
* Browser Developer Tools help inspect and debug the DOM.
* Understanding the DOM is essential for frontend JavaScript development.
