# DOM Classes and Styles

JavaScript provides several ways to manipulate CSS classes and styles through the DOM API.

These capabilities allow you to dynamically control the appearance and presentation of HTML elements based on application state, user interactions, and runtime conditions.

Common use cases include:

* Showing and hiding elements.
* Switching between light and dark themes.
* Highlighting selected elements.
* Applying validation styles to forms.
* Creating dropdown menus and modals.
* Implementing responsive interactive components.
* Dynamically changing colors, dimensions, and layouts.

There are two primary approaches to managing element appearance:

* CSS class manipulation using `classList` and `className`.
* Inline style manipulation using the `style` property.

# 🗁 Understanding CSS Classes in JavaScript

HTML elements can have one or multiple CSS classes.

Example:

```html
<div id="container" class="box active highlighted">
  Hello World
</div>
```

Each class can define a different set of CSS rules.

```css
.box {
  padding: 20px;
  border: 1px solid gray;
}

.active {
  background-color: lightgreen;
}

.highlighted {
  color: blue;
}
```

JavaScript can dynamically add, remove, replace, and toggle these classes.

# 🗁 className

The `className` property gets or sets the complete value of an element's `class` attribute.

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

Setting classes:

```javascript
container.className = "card highlighted";
```

This replaces all existing classes.

You can also assign an empty string to remove every class:

```javascript
container.className = "";
```

**Appending a class using className**

You can append a class manually:

```javascript
container.className += " active";
```

However, this approach requires careful handling of spaces and duplicate class names.

For individual class operations, `classList` is generally the better choice.

# 🗁 classList

The `classList` property returns a live `DOMTokenList` representing an element's CSS classes.

Example:

```html
<div id="container" class="box active"></div>
```

```javascript
const container = document.getElementById("container");

console.log(container.classList);
```

You can use several methods to manage classes.

# 🗁 classList.add()

The `add()` method adds one or more classes to an element.

Syntax:

```javascript
element.classList.add(className);
```

Example:

```javascript
container.classList.add("highlighted");
```

Adding multiple classes:

```javascript
container.classList.add("visible", "responsive", "selected");
```

Existing classes are preserved.

Adding a class that already exists does not create a duplicate.

# 🗁 classList.remove()

The `remove()` method removes one or more classes.

Syntax:

```javascript
element.classList.remove(className);
```

Example:

```javascript
container.classList.remove("active");
```

Removing multiple classes:

```javascript
container.classList.remove("selected", "highlighted");
```

Removing a class that does not exist does not throw an error.

# 🗁 classList.toggle()

The `toggle()` method adds a class if it does not exist or removes it if it already exists.

Syntax:

```javascript
element.classList.toggle(className);
```

Example:

```javascript
container.classList.toggle("active");
```

Calling it repeatedly switches the class between present and absent states.

**Using the force parameter**

The optional second argument forces a particular state.

```javascript
container.classList.toggle("active", true);
```

Ensures that the class exists.

```javascript
container.classList.toggle("active", false);
```

Ensures that the class does not exist.

This is useful when synchronizing CSS classes with application state.

Example:

```javascript
const isActive = true;

container.classList.toggle("active", isActive);
```

The method returns a Boolean indicating whether the class is present after the operation.

# 🗁 classList.contains()

The `contains()` method checks whether an element has a particular class.

Syntax:

```javascript
element.classList.contains(className);
```

Example:

```javascript
console.log(container.classList.contains("box")); // true

console.log(container.classList.contains("hidden")); // false
```

This is useful for conditional behavior.

```javascript
if (container.classList.contains("active")) {
  console.log("Container is active");
}
```

# 🗁 classList.replace()

The `replace()` method replaces an existing class with another class.

Syntax:

```javascript
element.classList.replace(oldClass, newClass);
```

Example:

```javascript
container.classList.replace("box", "card");
```

If the original class does not exist, the method returns `false`.

If the replacement succeeds, it returns `true`.

```javascript
const replaced = container.classList.replace("card", "panel");

console.log(replaced);
```

# 🗁 Iterating Over Classes

The `classList` property is iterable.

You can use `for...of`:

```javascript
for (const className of container.classList) {
  console.log(className);
}
```

You can also use `forEach()`:

```javascript
container.classList.forEach((className) => {
  console.log(className);
});
```

Converting classes into an array:

```javascript
const classes = Array.from(container.classList);

console.log(classes);
```

Or:

```javascript
const classes = [...container.classList];
```

# 🗁 The style Property

The `style` property provides access to an element's inline CSS declarations.

Example:

```html
<p id="message">Hello World</p>
```

```javascript
const message = document.getElementById("message");

message.style.color = "blue";

message.style.backgroundColor = "lightgray";

message.style.fontSize = "20px";
```

The resulting inline styles are equivalent to:

```html
<p
  id="message"
  style="
    color: blue;
    background-color: lightgray;
    font-size: 20px;
  "
>
  Hello World
</p>
```

The `style` property modifies inline styles, not the original external stylesheet rules.

# 🗁 CSS Property Naming in JavaScript

CSS property names usually use kebab-case, while JavaScript style properties generally use camelCase.

| CSS property | JavaScript property |
| --- | --- |
| `background-color` | `backgroundColor` |
| `font-size` | `fontSize` |
| `font-weight` | `fontWeight` |
| `margin-top` | `marginTop` |
| `padding-left` | `paddingLeft` |
| `border-radius` | `borderRadius` |
| `text-align` | `textAlign` |
| `z-index` | `zIndex` |
| `max-width` | `maxWidth` |
| `min-height` | `minHeight` |

Example:

```javascript
message.style.backgroundColor = "yellow";

message.style.borderRadius = "8px";

message.style.textAlign = "center";
```

# 🗁 Setting Multiple Inline Styles

You can assign multiple inline CSS declarations using `cssText`.

Example:

```javascript
message.style.cssText = `
  color: white;
  background-color: black;
  padding: 15px;
  border-radius: 8px;
`;
```

You can also use `setProperty()`:

```javascript
message.style.setProperty("color", "white");

message.style.setProperty("background-color", "black");

message.style.setProperty("font-size", "20px");
```

The `setProperty()` method uses standard CSS property names.

**Important:** Assigning `cssText` replaces the existing inline style declarations. If you only need to change individual properties, use direct property assignment or `setProperty()`.

# 🗁 Removing Inline Styles

There are several ways to remove inline styles.

**Assign an empty string**

```javascript
message.style.color = "";
```

**Use removeProperty()**

```javascript
message.style.removeProperty("background-color");
```

**Remove all inline declarations**

```javascript
message.removeAttribute("style");
```

These approaches affect inline declarations. They do not remove styles applied through external stylesheets or CSS classes.

# 🗁 Reading Inline Styles

You can read inline CSS declarations using the `style` property.

Example:

```html
<p id="message" style="color: red; font-size: 20px;">
  Hello World
</p>
```

```javascript
const message = document.getElementById("message");

console.log(message.style.color);

console.log(message.style.fontSize);
```

Output:

```text
red
20px
```

However, if a style comes from an external stylesheet rather than an inline declaration, `element.style` generally returns an empty string for that property.

# 🗁 getComputedStyle()

The `getComputedStyle()` method retrieves the computed CSS styles applied to an element.

Syntax:

```javascript
getComputedStyle(element);
```

Example:

HTML:

```html
<p id="message" class="message">
  Hello World
</p>
```

CSS:

```css
.message {
  color: blue;
  font-size: 20px;
  padding: 10px;
}
```

JavaScript:

```javascript
const message = document.getElementById("message");

const styles = getComputedStyle(message);

console.log(styles.color);

console.log(styles.fontSize);

console.log(styles.padding);
```

Example output:

```text
rgb(0, 0, 255)
20px
10px
```

Unlike `element.style`, `getComputedStyle()` can read CSS rules applied through:

* External stylesheets.
* Internal stylesheets.
* Inline styles.
* Inherited styles.
* Browser default styles.

The returned style declaration is read-only.

# 🗁 getPropertyValue()

The `getPropertyValue()` method retrieves a CSS property's value from a style declaration.

Example:

```javascript
const styles = getComputedStyle(message);

console.log(styles.getPropertyValue("color"));

console.log(styles.getPropertyValue("font-size"));
```

Output:

```text
rgb(0, 0, 255)
20px
```

You can also use it with inline styles:

```javascript
console.log(message.style.getPropertyValue("color"));
```

# 🗁 CSS Custom Properties

CSS custom properties, commonly called CSS variables, can be accessed through `style` methods.

Example CSS:

```css
:root {
  --primary-color: #2563eb;
  --text-color: #ffffff;
}
```

Read a custom property:

```javascript
const root = document.documentElement;

const primaryColor = getComputedStyle(root)
  .getPropertyValue("--primary-color");

console.log(primaryColor);
```

Update a custom property:

```javascript
root.style.setProperty("--primary-color", "#16a34a");
```

Remove a custom property override:

```javascript
root.style.removeProperty("--primary-color");
```

CSS custom properties are useful for implementing themes and centralized design tokens.

# 🗁 Working with CSS Priority

CSS declarations can have priority modifiers such as `!important`.

You can inspect the priority of an inline CSS declaration using `getPropertyPriority()`.

Example:

```javascript
message.style.setProperty("color", "red", "important");

console.log(
  message.style.getPropertyPriority("color")
);
```

Output:

```text
important
```

The third argument of `setProperty()` accepts a priority value, commonly `"important"` or an empty string.

Use `!important` sparingly because it can make CSS rules harder to override and maintain.

# 🗁 Working with Visibility

JavaScript can control element visibility by modifying CSS properties or classes.

**Using display**

```javascript
const element = document.getElementById("container");

element.style.display = "none";
```

The element is removed from the layout.

Show it again:

```javascript
element.style.display = "block";
```

Be careful: setting `display` to `"block"` may override the element's intended display type.

**Using visibility**

```javascript
element.style.visibility = "hidden";
```

The element becomes invisible but continues occupying its layout space.

Show it again:

```javascript
element.style.visibility = "visible";
```

**Using the hidden property**

```javascript
element.hidden = true;
```

Show the element:

```javascript
element.hidden = false;
```

The `hidden` property reflects the HTML `hidden` attribute.

# 🗁 display vs visibility vs opacity

These CSS properties have different effects.

| Property | Visible | Occupies layout space | Interaction |
| --- | --- | --- | --- |
| `display: none` | No | No | Not normally interactive |
| `visibility: hidden` | No | Yes | Not normally interactive |
| `opacity: 0` | No | Yes | Can remain interactive |
| `opacity: 0; pointer-events: none` | No | Yes | Pointer interaction disabled |

Example:

```javascript
element.style.opacity = "0";
```

This makes the element transparent but does not automatically remove it from the layout or prevent keyboard interaction.

Choose the appropriate technique based on the required behavior.

# 🗁 Managing Themes with CSS Classes

CSS classes are particularly useful for implementing themes.

HTML:

```html
<div id="app">
  <h1>Theme Example</h1>

  <p>Switch between light and dark themes.</p>

  <button id="theme-toggle">Toggle Theme</button>
</div>
```

CSS:

```css
body {
  background-color: white;
  color: black;
  transition: background-color 0.3s, color 0.3s;
}

body.dark-theme {
  background-color: #111827;
  color: white;
}

button {
  padding: 10px 16px;
  cursor: pointer;
}
```

JavaScript:

```javascript
const themeToggle = document.getElementById("theme-toggle");

themeToggle.addEventListener("click", () => {
  document.body.classList.toggle("dark-theme");
});
```

The button switches the theme by adding or removing a CSS class.

# 🗁 Practical Example: Interactive Card

HTML:

```html
<div id="card" class="card">
  <h2>JavaScript</h2>

  <p>Learn DOM manipulation.</p>

  <button id="highlight">Highlight</button>
</div>
```

CSS:

```css
.card {
  padding: 20px;
  border: 1px solid #ccc;
  border-radius: 8px;
  transition: all 0.3s ease;
}

.card.highlighted {
  background-color: #dbeafe;
  border-color: #2563eb;
  transform: translateY(-3px);
}
```

JavaScript:

```javascript
const card = document.getElementById("card");

const highlightButton = document.getElementById("highlight");

highlightButton.addEventListener("click", () => {
  card.classList.toggle("highlighted");
});
```

The card's appearance changes when the button is clicked.

# 🗁 Practical Example: Dynamic Progress Bar

HTML:

```html
<div class="progress-container">
  <div id="progress-bar"></div>
</div>

<button id="increase">Increase Progress</button>
```

CSS:

```css
.progress-container {
  width: 300px;
  height: 20px;
  background-color: #e5e7eb;
  border-radius: 10px;
  overflow: hidden;
}

#progress-bar {
  width: 0%;
  height: 100%;
  background-color: #2563eb;
  transition: width 0.3s ease;
}
```

JavaScript:

```javascript
const progressBar = document.getElementById("progress-bar");

const increaseButton = document.getElementById("increase");

let progress = 0;

increaseButton.addEventListener("click", () => {
  progress = Math.min(progress + 10, 100);

  progressBar.style.width = `${progress}%`;
});
```

Each click increases the progress by 10%, up to a maximum of 100%.

# 🗁 Common Mistakes

**Using kebab-case with direct style properties**

```javascript
element.style.background-color = "red";
```

This is invalid JavaScript syntax.

Use camelCase:

```javascript
element.style.backgroundColor = "red";
```

Or use `setProperty()`:

```javascript
element.style.setProperty("background-color", "red");
```

**Confusing className and classList**

```javascript
element.className = "active";
```

This replaces all existing classes.

Use:

```javascript
element.classList.add("active");
```

when you only want to add a class.

**Reading external CSS through element.style**

```javascript
console.log(element.style.color);
```

This may return an empty string when the color is defined in an external stylesheet.

Use:

```javascript
console.log(getComputedStyle(element).color);
```

**Setting a CSS value without its required unit**

```javascript
element.style.width = 200;
```

Most dimensional CSS properties require units.

Use:

```javascript
element.style.width = "200px";
```

Some properties, such as `opacity` and `zIndex`, accept unitless values.

**Using display to hide an element without considering its original layout**

```javascript
element.style.display = "none";

element.style.display = "block";
```

Restoring it to `block` may change its original layout behavior.

Consider using a CSS class or the `hidden` property instead.

# 🗁 Best Practices

* Prefer CSS classes for reusable presentation changes.
* Use `classList` instead of manually manipulating class strings.
* Use inline styles for genuinely dynamic values, such as calculated dimensions or progress.
* Use `getComputedStyle()` when you need to inspect final computed styles.
* Use CSS custom properties for theme colors and reusable design values.
* Avoid excessive use of `!important`.
* Keep presentation rules in CSS rather than embedding large style declarations in JavaScript.
* Use the `hidden` property or an appropriate CSS class for visibility management.
* Consider accessibility when hiding elements, especially keyboard navigation and screen-reader behavior.
* Avoid repeatedly reading layout-dependent computed styles while performing many DOM writes, as this can cause layout thrashing.

# 🗁 Key Takeaways

* `className` reads or replaces the complete class attribute.
* `classList` provides methods for managing individual CSS classes.
* `add()` adds classes.
* `remove()` removes classes.
* `toggle()` switches classes.
* `contains()` checks whether a class exists.
* `replace()` replaces one class with another.
* `style` manages inline CSS declarations.
* `getComputedStyle()` reads computed CSS values.
* `setProperty()` and `removeProperty()` manage CSS declarations.
* CSS custom properties can be managed through `setProperty()` and `getPropertyValue()`.
* CSS classes are generally preferable for reusable presentation changes.

# 🗁 References

* [MDN — Element.classList](https://developer.mozilla.org/en-US/docs/Web/API/Element/classList)
* [MDN — Element.className](https://developer.mozilla.org/en-US/docs/Web/API/Element/className)
* [MDN — HTMLElement.style](https://developer.mozilla.org/en-US/docs/Web/API/HTMLElement/style)
* [MDN — CSSStyleDeclaration](https://developer.mozilla.org/en-US/docs/Web/API/CSSStyleDeclaration)
* [MDN — Window.getComputedStyle()](https://developer.mozilla.org/en-US/docs/Web/API/Window/getComputedStyle)
* [MDN — CSS Custom Properties](https://developer.mozilla.org/en-US/docs/Web/CSS/Guides/Cascading_variables/Using_custom_properties)
* [MDN — HTMLElement.hidden](https://developer.mozilla.org/en-US/docs/Web/API/HTMLElement/hidden)
