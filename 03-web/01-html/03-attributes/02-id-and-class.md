# HTML ID and Class Attributes

The `id` and `class` attributes are global HTML attributes used to identify and group HTML elements.

They are commonly used for:

* Selecting elements with CSS.
* Finding and manipulating elements with JavaScript.
* Creating links to specific sections of a page.
* Connecting form labels and accessibility descriptions to elements.
* Organizing reusable UI components and styles.

Although both attributes help identify elements, they serve different purposes:

| Attribute | Purpose | Reusability |
| --- | --- | --- |
| `id` | Uniquely identifies an element within a document | Should be unique |
| `class` | Groups elements that share common characteristics | Can be reused |

# 🗁 The ID Attribute

The `id` attribute assigns a unique identifier to an HTML element.

### 🖆 Syntax

```html
<element id="unique-name">Content</element>
```

### 🖆 Example

```html
<header id="site-header">
  <h1>My Website</h1>
</header>

<main id="main-content">
  <h2>Welcome</h2>
  <p>This is the main content.</p>
</main>

<footer id="site-footer">
  <p>Copyright 2026</p>
</footer>
```

Each element has a different ID.

### 🖆 Important Rules

* An ID should be unique within its document.
* An ID must not contain ASCII whitespace characters.
* Use meaningful and descriptive identifiers.
* Avoid changing the capitalization of IDs inconsistently.
* Do not use an ID simply because you want to apply reusable styling.
* Avoid relying on an element's ID as an automatically created JavaScript global variable.

Duplicate IDs can cause unexpected behavior when using CSS selectors, JavaScript selection methods, fragment navigation, and accessibility relationships.

# 🗁 Using ID with CSS

CSS ID selectors target elements using the `#` symbol followed by the ID value.

### 🖆 Example

```html
<h1 id="page-title">Welcome to My Website</h1>
<p id="description">Learn HTML and CSS.</p>
```

```css
#page-title {
  color: blue;
  font-size: 32px;
}

#description {
  color: gray;
}
```

The `#page-title` selector targets the element whose ID is `page-title`.

### 🖆 ID Selector Specificity

ID selectors have higher specificity than class and type selectors.

```css
h1 {
  color: black;
}

.title {
  color: green;
}

#main-title {
  color: blue;
}
```

```html
<h1 id="main-title" class="title">
  Hello World
</h1>
```

The text appears blue because the ID selector has greater specificity than the class and element selectors, assuming no other cascade factors override these declarations.

# 🗁 Using ID with Fragment Links

IDs allow users to navigate directly to a specific section of a page.

### 🖆 Example

```html
<nav>
  <a href="#about">About</a>
  <a href="#services">Services</a>
  <a href="#contact">Contact</a>
</nav>

<section id="about">
  <h2>About Us</h2>
  <p>Learn more about our company.</p>
</section>

<section id="services">
  <h2>Our Services</h2>
  <p>Explore our services.</p>
</section>

<section id="contact">
  <h2>Contact Us</h2>
  <p>Get in touch with us.</p>
</section>
```

When a user clicks a link such as:

```html
<a href="#contact">Contact</a>
```

The browser navigates to the element with `id="contact"`.

You can also link to an element on another page:

```html
<a href="about.html#team">Our Team</a>
```

The browser opens `about.html` and navigates to the element with `id="team"`.

# 🗁 Using ID with JavaScript

JavaScript provides several methods for selecting elements by their IDs.

### 🖆 getElementById()

Returns the first element with the specified ID.

```html
<h1 id="heading">Hello World</h1>
```

```javascript
const heading = document.getElementById("heading");

console.log(heading);
console.log(heading.textContent);
```

### 🖆 querySelector()

Selects an element using a CSS selector.

```javascript
const heading = document.querySelector("#heading");

console.log(heading.textContent);
```

Both methods can select an element by ID. `getElementById()` accepts an ID value directly, while `querySelector()` accepts a CSS selector.

# 🗁 The Class Attribute

The `class` attribute assigns one or more class names to an HTML element.

Unlike IDs, class names can be reused across multiple elements.

### 🖆 Syntax

```html
<element class="class-name">Content</element>
```

### 🖆 Example

```html
<div class="card">
  <h2>HTML</h2>
  <p>Learn HTML fundamentals.</p>
</div>

<div class="card">
  <h2>CSS</h2>
  <p>Learn CSS styling.</p>
</div>

<div class="card">
  <h2>JavaScript</h2>
  <p>Learn JavaScript programming.</p>
</div>
```

All three elements share the same `card` class.

This makes it possible to apply the same styles to multiple elements.

# 🗁 Using Class with CSS

CSS class selectors target elements using the `.` symbol followed by the class name.

### 🖆 Example

```html
<p class="text">First paragraph.</p>
<p class="text">Second paragraph.</p>
<p class="text">Third paragraph.</p>
```

```css
.text {
  color: blue;
  font-size: 18px;
}
```

All three paragraphs receive the same styles.

### 🖆 Multiple Classes on an Element

An HTML element can have multiple classes.

Separate class names with whitespace.

```html
<div class="card featured rounded">
  <h2>Featured Product</h2>
  <p>This is a featured product.</p>
</div>
```

```css
.card {
  padding: 20px;
  border: 1px solid #ddd;
}

.featured {
  background-color: #f0f0f0;
}

.rounded {
  border-radius: 12px;
}
```

The element receives styles from all three classes.

### 🖆 Combining Class Selectors

You can combine class selectors to target elements with multiple specific classes.

```css
.card.featured {
  border: 2px solid blue;
}
```

This selector targets an element that has both `card` and `featured` classes.

```html
<div class="card featured">
  Featured Card
</div>
```

### 🖆 Descendant Class Selectors

A space between selectors targets matching descendants.

```css
.card .title {
  color: darkblue;
}
```

This targets elements with the `title` class inside an element with the `card` class.

```html
<div class="card">
  <h2 class="title">Product Name</h2>
  <p>Product description.</p>
</div>
```

# 🗁 Using Class with JavaScript

JavaScript provides different methods to select and manipulate elements by class.

### 🖆 getElementsByClassName()

Returns a live HTMLCollection of elements matching the specified class name.

```html
<p class="message">First message</p>
<p class="message">Second message</p>
```

```javascript
const messages = document.getElementsByClassName("message");

console.log(messages);
console.log(messages.length);
```

### 🖆 querySelectorAll()

Returns a static NodeList of elements matching a CSS selector.

```javascript
const messages = document.querySelectorAll(".message");

messages.forEach((message) => {
  console.log(message.textContent);
});
```

Unlike `getElementsByClassName()`, the NodeList returned by `querySelectorAll()` does not automatically update when the document changes.

# 🗁 Manipulating Classes with classList

The `classList` property provides convenient methods for working with an element's classes.

### 🖆 add()

Adds one or more classes.

```javascript
const card = document.querySelector(".card");

card.classList.add("active");
card.classList.add("featured", "highlighted");
```

### 🖆 remove()

Removes one or more classes.

```javascript
card.classList.remove("active");
```

### 🖆 toggle()

Adds a class if it does not exist or removes it if it already exists.

```javascript
card.classList.toggle("active");
```

You can also force a class to be added or removed:

```javascript
card.classList.toggle("active", true);
card.classList.toggle("active", false);
```

### 🖆 contains()

Checks whether an element contains a specific class.

```javascript
if (card.classList.contains("active")) {
  console.log("The card is active.");
}
```

### 🖆 replace()

Replaces an existing class with another class.

```javascript
card.classList.replace("active", "inactive");
```

# 🗁 className vs classList

Both `className` and `classList` are used to access an element's classes, but they behave differently.

| Property | Description |
| --- | --- |
| `className` | Gets or sets the complete class attribute as a string |
| `classList` | Provides methods to manage individual class tokens |

### 🖆 Using className

```javascript
const element = document.querySelector(".card");

console.log(element.className);

// Replace all existing classes
element.className = "card active";
```

Setting `className` replaces the entire class attribute value.

### 🖆 Using classList

```javascript
const element = document.querySelector(".card");

element.classList.add("active");
element.classList.remove("featured");
```

`classList` allows you to modify individual classes without replacing the others.

**Recommendation:** Prefer `classList` when adding, removing, toggling, or checking individual classes.

# 🗁 CSS Selector Comparison

| Selector | Meaning |
| --- | --- |
| `#header` | Element with ID `header` |
| `.card` | Element with class `card` |
| `div` | All `div` elements |
| `.card.featured` | Element with both classes |
| `.card .title` | Element with class `title` inside `.card` |
| `#header .nav` | Element with class `nav` inside `#header` |
| `.card, .panel` | Elements matching either selector |

# 🗁 ID and Class Best Practices

### 🖆 Use IDs for Unique Elements

```html
<header id="main-header">
  Website Header
</header>
```

Do not reuse the same ID for multiple elements in one document.

### 🖆 Use Classes for Reusable Components

```html
<button class="button">Save</button>
<button class="button">Cancel</button>
<button class="button">Submit</button>
```

Classes are ideal for reusable styles and component states.

### 🖆 Use Meaningful Names

Good examples:

```html
<div id="main-content"></div>

<div class="product-card"></div>

<button class="button-primary"></button>
```

Avoid unclear names:

```html
<div id="x"></div>

<div class="box1"></div>

<button class="red"></button>
```

Prefer names that describe the element's purpose rather than only its visual appearance.

### 🖆 Follow a Consistent Naming Convention

Kebab-case is a common naming convention for HTML IDs and classes.

```html
<section id="user-profile">
  <div class="profile-card">
    <h2 class="profile-title">User Profile</h2>
  </div>
</section>
```

You may also use established conventions such as BEM (Block, Element, Modifier).

```html
<div class="card card--featured">
  <h2 class="card__title">Featured Card</h2>
</div>
```

### 🖆 Use IDs for Accessibility Relationships

IDs are important when connecting HTML elements through accessibility-related attributes.

```html
<label for="email">Email Address</label>

<input
  type="email"
  id="email"
  name="email"
  aria-describedby="email-help"
>

<p id="email-help">
  Enter a valid email address.
</p>
```

Here:

* `for="email"` connects the label to the input.
* `aria-describedby="email-help"` associates the help text with the input.
* Both referenced IDs identify specific elements.

### 🖆 Escape Unusual IDs in CSS Selectors

HTML IDs can contain characters that require escaping when used in CSS selectors.

For example:

```html
<div id="user:profile">
  User Profile
</div>
```

A selector such as `#user:profile` would interpret the colon as CSS syntax rather than part of the ID.

Use `CSS.escape()` when constructing selectors dynamically:

```javascript
const id = "user:profile";

const element = document.querySelector(
  `#${CSS.escape(id)}`
);

console.log(element);
```

Using simple identifiers containing letters, digits, hyphens, and underscores makes selectors easier to read and maintain.

# 🗁 Common Mistakes

### 🖆 Reusing the Same ID

Incorrect:

```html
<div id="card">First Card</div>
<div id="card">Second Card</div>
```

Correct:

```html
<div id="first-card">First Card</div>
<div id="second-card">Second Card</div>
```

If both elements need the same styling, use a class instead.

```html
<div class="card">First Card</div>
<div class="card">Second Card</div>
```

### 🖆 Using Commas Between Class Names

Incorrect:

```html
<div class="card, featured"></div>
```

Correct:

```html
<div class="card featured"></div>
```

Class names are separated by whitespace, not commas.

### 🖆 Forgetting the CSS Selector Prefix

Incorrect:

```css
card {
  color: blue;
}
```

Correct:

```css
.card {
  color: blue;
}
```

The `.` identifies a class selector.

For an ID selector, use `#`.

```css
#main-header {
  background-color: black;
}
```

### 🖆 Replacing All Classes Accidentally

Potentially problematic:

```javascript
element.className = "active";
```

This removes every existing class.

A safer approach when adding a class is:

```javascript
element.classList.add("active");
```

### 🖆 Using IDs for Every Styling Requirement

Avoid assigning unique IDs to every element just to style them.

Prefer reusable classes:

```html
<div class="card"></div>
<div class="card"></div>
<div class="card"></div>
```

```css
.card {
  padding: 20px;
  border-radius: 8px;
}
```

# 🗁 Practical Example

The following example demonstrates how IDs and classes work together with HTML, CSS, and JavaScript.

```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>ID and Class Example</title>

  <style>
    #page-title {
      color: darkblue;
      text-align: center;
    }

    .card {
      padding: 20px;
      margin: 10px 0;
      border: 1px solid #ddd;
      border-radius: 8px;
    }

    .active {
      background-color: lightgreen;
    }

    .button {
      padding: 10px 16px;
      cursor: pointer;
    }
  </style>
</head>
<body>

  <h1 id="page-title">Product List</h1>

  <div class="card">
    <h2>Laptop</h2>
    <p>High-performance laptop.</p>
    <button class="button">Activate</button>
  </div>

  <div class="card">
    <h2>Smartphone</h2>
    <p>Modern smartphone.</p>
    <button class="button">Activate</button>
  </div>

  <script>
    const buttons = document.querySelectorAll(".button");

    buttons.forEach((button) => {
      button.addEventListener("click", () => {
        const card = button.closest(".card");

        card.classList.toggle("active");
      });
    });
  </script>

</body>
</html>
```

**How it works:**

* `page-title` uniquely identifies the page heading.
* `card` is reused across multiple product containers.
* `button` is reused across multiple buttons.
* `active` represents a reusable visual state.
* JavaScript selects all buttons and toggles the `active` class on their respective cards.

# 🗁 Practice Exercises

* Create a webpage with a unique ID for the main heading.
* Create three product cards using the same `card` class.
* Add multiple classes to one element.
* Create navigation links that jump to different sections using IDs.
* Use JavaScript `getElementById()` to change a heading.
* Use `querySelectorAll()` to select multiple elements by class.
* Use `classList.toggle()` to implement a dark-mode toggle.
* Connect a form label to an input using `for` and `id`.
* Create a component with BEM-style class names.

# 🗁 Interview Questions

**What is the difference between ID and class in HTML?**

An ID uniquely identifies an element within a document, while a class groups elements and can be reused across multiple elements.

**Can multiple HTML elements have the same class?**

Yes. Multiple elements can share the same class.

**Can multiple elements have the same ID?**

An ID should be unique within its document. Duplicate IDs can cause unexpected behavior.

**Can an HTML element have multiple classes?**

Yes. Multiple class names can be assigned using whitespace-separated values.

**What is the difference between `getElementById()` and `querySelector()`?**

`getElementById()` accepts an ID value directly, while `querySelector()` accepts a CSS selector.

**What is the difference between `getElementsByClassName()` and `querySelectorAll()`?**

`getElementsByClassName()` returns a live HTMLCollection. `querySelectorAll()` returns a static NodeList.

**What is the difference between `className` and `classList`?**

`className` accesses the class attribute as a string, while `classList` provides methods for manipulating individual class tokens.

**Which is better for reusable CSS styling: ID or class?**

Classes are generally preferred because they can be reused across multiple elements and have lower specificity than ID selectors.

**What does `classList.toggle()` do?**

It adds a class if the class is absent or removes it if the class is already present.

**Why are IDs important in forms?**

IDs connect labels to form controls through the `for` attribute and can also be referenced by accessibility attributes.

# 🗁 Official References

* [MDN — id global attribute](https://developer.mozilla.org/en-US/docs/Web/HTML/Reference/Global_attributes/id)
* [MDN — class global attribute](https://developer.mozilla.org/en-US/docs/Web/HTML/Reference/Global_attributes/class)
* [MDN — CSS ID selectors](https://developer.mozilla.org/en-US/docs/Web/CSS/ID_selectors)
* [MDN — CSS class selectors](https://developer.mozilla.org/en-US/docs/Web/CSS/Class_selectors)
* [MDN — Element.classList](https://developer.mozilla.org/en-US/docs/Web/API/Element/classList)
* [MDN — Document.getElementById()](https://developer.mozilla.org/en-US/docs/Web/API/Document/getElementById)
* [WHATWG — HTML Living Standard](https://html.spec.whatwg.org/)
