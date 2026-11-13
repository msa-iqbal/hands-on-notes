# HTML Global Attributes

HTML global attributes are attributes that can be used on all HTML elements, including elements that do not normally define their own specific attributes.

They provide common functionality for identifying elements, applying styles, managing accessibility, handling user interactions, and storing custom data.

Global attributes are essential for modern web development because they allow developers to control HTML elements consistently across an application.

Common examples include:

* `id`
* `class`
* `style`
* `title`
* `hidden`
* `lang`
* `dir`
* `tabindex`
* `contenteditable`
* `draggable`
* `spellcheck`
* `translate`
* `data-*`

# 🗁 What Are Global Attributes?

Global attributes are HTML attributes that can be applied to any HTML element.

They provide functionality that is not limited to a particular element type.

### 🖆 Basic Example

```html
<div
  id="main-content"
  class="container"
  title="Main content area"
  lang="en"
>
  <h1>Welcome to HTML</h1>
  <p>Learning global attributes.</p>
</div>
```

In this example:

* `id` uniquely identifies the element.
* `class` associates the element with CSS classes.
* `title` provides advisory information.
* `lang` identifies the language of the content.

### 🖆 Global vs Element-Specific Attributes

HTML attributes can be broadly understood as global attributes and element-specific attributes.

| Attribute | Type | Example |
| --- | --- | --- |
| `id` | Global | `<div id="content">` |
| `class` | Global | `<p class="text">` |
| `title` | Global | `<span title="Information">` |
| `hidden` | Global | `<div hidden>` |
| `href` | Element-specific | `<a href="/about">` |
| `src` | Element-specific | `<img src="photo.jpg">` |
| `alt` | Element-specific | `<img alt="Profile photo">` |
| `type` | Element-specific in common usage | `<input type="email">` |

Global attributes can be used across HTML elements, while element-specific attributes are defined for particular elements.

# 🗁 Common Global Attributes

The following table introduces commonly used global attributes.

| Attribute | Purpose |
| --- | --- |
| `id` | Identifies an element uniquely |
| `class` | Assigns one or more CSS classes |
| `style` | Applies inline CSS declarations |
| `title` | Provides advisory information |
| `hidden` | Indicates that an element is not currently relevant and should be hidden |
| `lang` | Specifies the language of an element's content |
| `dir` | Specifies text direction |
| `tabindex` | Controls sequential keyboard focus behavior and focusability |
| `contenteditable` | Makes an element's content editable |
| `draggable` | Specifies whether an element can be dragged |
| `spellcheck` | Controls whether spelling and grammar checking is enabled |
| `translate` | Indicates whether an element's text should be translated |
| `accesskey` | Defines a keyboard shortcut hint for an element |
| `autocapitalize` | Provides a hint for automatic capitalization during text input |
| `enterkeyhint` | Suggests the label or action for an input method editor's Enter key |
| `inputmode` | Hints which virtual keyboard should be displayed |
| `data-*` | Stores custom application data |
| `inert` | Makes an element and its descendants non-interactive |
| `dir` | Controls text direction |
| `nonce` | Provides a nonce for security policies such as Content Security Policy |
| `part` | Exposes an element's shadow-tree part for external styling |
| `popover` | Declares an element as a popover |
| `slot` | Assigns an element to a named slot in a Web Component |
| `exportparts` | Exposes nested shadow-tree parts |
| `itemid` | Provides a unique identifier for a microdata item |
| `itemprop` | Defines a property of a microdata item |
| `itemref` | References additional elements containing microdata properties |
| `itemscope` | Creates a microdata item |
| `itemtype` | Specifies the vocabulary URL for a microdata item |

Some global attributes have specialized use cases and are not needed in every application.

# 🗁 Identification Attributes

Identification attributes help developers select and organize HTML elements.

### 🖆 The `id` Attribute

The `id` attribute assigns a unique identifier to an HTML element within a document.

```html
<div id="main-content">
  <h1>Welcome</h1>
</div>
```

An ID can be used with CSS:

```css
#main-content {
  padding: 20px;
  background-color: lightgray;
}
```

It can also be used with JavaScript:

```javascript
const element = document.getElementById("main-content");

console.log(element);
```

An ID can also be used as a fragment identifier in a URL:

```html
<a href="#contact">Go to Contact</a>

<section id="contact">
  <h2>Contact Information</h2>
</section>
```

**Important:** IDs should be unique within an HTML document.

### 🖆 The `class` Attribute

The `class` attribute assigns one or more class names to an element.

```html
<div class="container">
  <p class="text highlight">Hello World</p>
</div>
```

CSS can target these classes:

```css
.container {
  padding: 20px;
}

.text {
  font-size: 18px;
}

.highlight {
  color: blue;
}
```

JavaScript can also select elements by class:

```javascript
const elements = document.querySelectorAll(".highlight");

elements.forEach((element) => {
  console.log(element.textContent);
});
```

Unlike IDs, the same class can be assigned to multiple elements.

# 🗁 Presentation Attributes

Presentation-related global attributes influence how content is displayed or interpreted.

### 🖆 The `style` Attribute

The `style` attribute applies CSS declarations directly to an element.

```html
<p style="color: blue; font-size: 20px;">
  This is styled text.
</p>
```

It is useful for small, dynamic styling requirements.

However, external stylesheets are generally preferable for maintainability.

```css
.message {
  color: blue;
  font-size: 20px;
}
```

```html
<p class="message">This is styled text.</p>
```

### 🖆 The `title` Attribute

The `title` attribute provides advisory information about an element.

Browsers commonly display its value as a tooltip when a user hovers over the element.

```html
<button type="button" title="Save your changes">
  Save
</button>
```

The `title` attribute should not be the only way to communicate important information because it may not be accessible to every user or input method.

### 🖆 The `hidden` Attribute

The `hidden` attribute indicates that an element is not currently relevant and should not be rendered.

```html
<p hidden>
  This content is hidden.
</p>
```

JavaScript can control its visibility:

```html
<p id="message" hidden>
  Welcome back!
</p>

<button id="showMessage" type="button">
  Show Message
</button>

<script>
  const message = document.querySelector("#message");

  document
    .querySelector("#showMessage")
    .addEventListener("click", () => {
      message.hidden = false;
    });
</script>
```

The `hidden` attribute is useful for controlling content visibility without removing the element from the DOM.

A CSS rule that explicitly changes an element's display may override the normal rendering behavior of the hidden state. Avoid styling hidden elements in ways that unintentionally make them visible.

### 🖆 The `translate` Attribute

The `translate` attribute indicates whether the text content of an element should be translated when a page is localized.

It supports:

* `yes`: The content may be translated.
* `no`: The content should not be translated.

```html
<p translate="yes">
  Welcome to our website.
</p>

<p translate="no">
  JavaScript
</p>
```

It is useful for controlling translation behavior for product names, code identifiers, and other content that should remain unchanged.

# 🗁 Language and Direction Attributes

Language and direction attributes help browsers and assistive technologies interpret text correctly.

### 🖆 The `lang` Attribute

The `lang` attribute specifies the language of an element's content.

```html
<html lang="en">
  <head>
    <title>My Website</title>
  </head>

  <body>
    <h1>Welcome</h1>
    <p>This page is written in English.</p>
  </body>
</html>
```

A different language can be specified for individual elements:

```html
<p lang="bn">
  আমি HTML শিখছি।
</p>

<p lang="en">
  I am learning HTML.
</p>
```

Common language codes include:

| Language | Code |
| --- | --- |
| English | `en` |
| Bengali | `bn` |
| Arabic | `ar` |
| French | `fr` |
| Spanish | `es` |
| German | `de` |
| Japanese | `ja` |

Use valid BCP 47 language tags when identifying languages and regional variants.

### 🖆 The `dir` Attribute

The `dir` attribute specifies the direction of text.

Supported values:

| Value | Meaning |
| --- | --- |
| `ltr` | Left to right |
| `rtl` | Right to left |
| `auto` | Determine direction from the content |

Example:

```html
<p dir="ltr">
  Hello World
</p>

<p dir="rtl">
  مرحباً بالعالم
</p>
```

It is especially important when building multilingual applications.

# 🗁 Interaction Attributes

Some global attributes influence how users interact with elements.

### 🖆 The `tabindex` Attribute

The `tabindex` attribute controls whether an element can receive keyboard focus and how it participates in sequential keyboard navigation.

Common values:

| Value | Behavior |
| --- | --- |
| `0` | Makes an element focusable and places it in the natural document tab order |
| `-1` | Allows programmatic focus but excludes the element from sequential keyboard navigation |
| Positive integer | Places the element in an explicit tab order |

Example:

```html
<div tabindex="0">
  This element can receive keyboard focus.
</div>

<div tabindex="-1">
  This element can be focused programmatically.
</div>
```

Use native interactive elements whenever possible.

For example, prefer:

```html
<button type="button">Submit</button>
```

Instead of creating a custom button using a generic `<div>` with `tabindex`.

Avoid positive `tabindex` values because they can create confusing keyboard navigation.

### 🖆 The `contenteditable` Attribute

The `contenteditable` attribute makes an element's content editable by the user.

Possible values:

* `true`: Enables editing.
* `false`: Disables editing.
* `plaintext-only`: Enables editing while preserving plain text rather than rich formatting.

Example:

```html
<div contenteditable="true">
  You can edit this content.
</div>
```

Plain-text editing:

```html
<div contenteditable="plaintext-only">
  Only plain text editing is allowed.
</div>
```

JavaScript can read the edited content:

```javascript
const editor = document.querySelector("[contenteditable]");

console.log(editor.textContent);
```

For production editors, consider selection management, keyboard behavior, sanitization, and accessibility.

### 🖆 The `draggable` Attribute

The `draggable` attribute specifies whether an element can be dragged using the browser's drag-and-drop mechanism.

Possible values:

* `true`
* `false`

Example:

```html
<div draggable="true">
  Drag this element.
</div>
```

The attribute is an enumerated attribute, so explicit values should be used.

Dragging is not automatically implemented just by setting this attribute. JavaScript event handlers are usually needed to define what happens during drag-and-drop operations.

### 🖆 The `spellcheck` Attribute

The `spellcheck` attribute provides a hint about whether spelling and grammar checking should be enabled.

```html
<textarea spellcheck="true">
  Type your message here.
</textarea>
```

Disable spell checking:

```html
<textarea spellcheck="false">
  Enter code or technical content here.
</textarea>
```

Its values are hints to the browser rather than guarantees about a particular spell-checking implementation.

### 🖆 The `accesskey` Attribute

The `accesskey` attribute specifies a keyboard shortcut that can be used to activate or focus an element.

```html
<button accesskey="s" type="button">
  Save
</button>
```

The actual key combination depends on the browser and operating system.

Use this attribute carefully because keyboard shortcuts can conflict with browser or assistive-technology shortcuts.

### 🖆 The `inputmode` Attribute

The `inputmode` attribute provides a hint about the type of virtual keyboard that should be displayed.

```html
<input type="text" inputmode="numeric">
```

Common values include:

* `none`
* `text`
* `decimal`
* `numeric`
* `tel`
* `search`
* `email`
* `url`

It does not validate the input. Use appropriate input types and validation rules when validation is required.

### 🖆 The `autocapitalize` Attribute

The `autocapitalize` attribute provides a hint about automatic capitalization behavior for text input.

```html
<input type="text" autocapitalize="words">
```

Common values include:

* `none`
* `off`
* `sentences`
* `words`
* `characters`

Browser and input-method behavior can vary.

### 🖆 The `enterkeyhint` Attribute

The `enterkeyhint` attribute suggests which action label should appear on a virtual keyboard's Enter key.

```html
<input type="search" enterkeyhint="search">
```

Common values include:

* `enter`
* `done`
* `go`
* `next`
* `previous`
* `search`
* `send`

It changes the suggested keyboard label, not the actual form-submission behavior.

# 🗁 Accessibility-Related Attributes

Some global attributes are particularly useful when building accessible interfaces.

### 🖆 The `inert` Attribute

The `inert` attribute makes an element and its descendants non-interactive.

An inert subtree cannot normally receive focus or user interaction, and its content is excluded from the accessibility tree.

```html
<div inert>
  <button type="button">Unavailable Action</button>
  <a href="/account">Account</a>
</div>
```

This is useful when temporarily disabling interaction with a section of a page.

For example, a modal interface may make the background content inert while the dialog is open. Native modal dialogs opened with `showModal()` already provide this behavior for the rest of the document.

### 🖆 The `role` Attribute

The `role` attribute is a global ARIA attribute that communicates an element's semantic role to assistive technologies.

```html
<div role="status">
  Your changes have been saved.
</div>
```

The `status` role communicates a status message to assistive technologies.

Prefer native HTML elements and semantics instead of assigning ARIA roles unnecessarily.

### 🖆 ARIA Global Attributes

ARIA also defines global attributes that can be applied broadly to HTML elements.

Examples include:

| Attribute | Purpose |
| --- | --- |
| `aria-label` | Provides an accessible name |
| `aria-labelledby` | References an element that provides an accessible name |
| `aria-describedby` | References an element that provides an accessible description |
| `aria-hidden` | Controls exposure of an element to the accessibility tree |
| `aria-live` | Identifies a region where dynamic updates may be announced |
| `aria-disabled` | Communicates a disabled state |
| `aria-expanded` | Communicates whether a control's associated content is expanded |
| `aria-controls` | References an element controlled by the current element |

Example:

```html
<button
  type="button"
  aria-expanded="false"
  aria-controls="menu"
>
  Open Menu
</button>

<nav id="menu" hidden>
  <a href="/">Home</a>
  <a href="/about">About</a>
</nav>
```

ARIA attributes communicate accessibility information; they do not automatically implement the behavior they describe. JavaScript must keep the interface state and ARIA state synchronized.

# 🗁 Custom Data Attributes

The `data-*` attribute family allows developers to store custom data directly on HTML elements.

It is useful for connecting HTML elements with JavaScript application logic.

### 🖆 Basic Syntax

```html
<div data-user-id="101">
  User Information
</div>
```

Custom data attributes:

* Begin with `data-`.
* Can contain lowercase letters, digits, hyphens, and other permitted attribute-name characters.
* Store string values in the DOM.
* Are accessible through JavaScript's `dataset` property.

### 🖆 Multiple Data Attributes

```html
<div
  id="product"
  data-product-id="101"
  data-product-name="Laptop"
  data-category="electronics"
>
  Laptop
</div>
```

### 🖆 Accessing Data Attributes with JavaScript

```javascript
const product = document.querySelector("#product");

console.log(product.dataset.productId);
console.log(product.dataset.productName);
console.log(product.dataset.category);
```

HTML attribute names using kebab-case are exposed through camelCase property names in `dataset`.

For example:

| HTML Attribute | JavaScript Property |
| --- | --- |
| `data-user-id` | `dataset.userId` |
| `data-product-name` | `dataset.productName` |
| `data-user-role` | `dataset.userRole` |
| `data-is-active` | `dataset.isActive` |

### 🖆 Updating Data Attributes

```javascript
const product = document.querySelector("#product");

product.dataset.category = "computers";

console.log(product.dataset.category);
```

### 🖆 Using Data Attributes in CSS

```html
<button data-status="active">
  Active Account
</button>
```

```css
button[data-status="active"] {
  background-color: green;
  color: white;
}
```

### 🖆 Important Considerations

* Data attributes are not a replacement for application state management.
* Their values are strings.
* Do not store sensitive information in them.
* Do not use them as a substitute for semantic HTML attributes.
* Avoid storing large or complex application objects in HTML attributes.

# 🗁 Using Multiple Global Attributes

Global attributes can be combined on a single element.

```html
<article
  id="article-101"
  class="article featured"
  lang="en"
  data-category="technology"
  title="Read the latest technology article"
>
  <h2>Modern Web Development</h2>

  <p>
    Learn HTML, CSS, and JavaScript.
  </p>
</article>
```

Each attribute serves a different purpose:

* `id`: Unique element identification.
* `class`: CSS styling and JavaScript selection.
* `lang`: Language identification.
* `data-category`: Custom application data.
* `title`: Advisory information.

### 🖆 Complete JavaScript Example

```html
<div
  id="user"
  class="profile"
  data-user-id="101"
  data-role="admin"
>
  Muhammad
</div>

<script>
  const user = document.querySelector("#user");

  console.log(user.id);
  console.log(user.className);
  console.log(user.dataset.userId);
  console.log(user.dataset.role);

  user.classList.add("active");
  user.dataset.role = "editor";
</script>
```

This demonstrates how global attributes connect HTML structure with CSS and JavaScript.

# 🗁 Global Attributes Reference

The following list summarizes important global attributes and their primary use cases.

| Attribute | Description |
| --- | --- |
| `accesskey` | Defines a keyboard shortcut hint |
| `autocapitalize` | Suggests automatic capitalization behavior |
| `autofocus` | Requests automatic focus when a page or dialog is shown |
| `class` | Assigns CSS classes |
| `contenteditable` | Makes content editable |
| `data-*` | Stores custom data |
| `dir` | Defines text direction |
| `draggable` | Defines whether an element can be dragged |
| `enterkeyhint` | Suggests an Enter key label |
| `hidden` | Indicates content should not be rendered |
| `id` | Defines a unique identifier |
| `inert` | Disables interaction with an element subtree |
| `inputmode` | Suggests an input keyboard type |
| `is` | Identifies a customized built-in element |
| `itemid` | Defines a microdata item identifier |
| `itemprop` | Defines a microdata property |
| `itemref` | References additional microdata properties |
| `itemscope` | Defines a microdata item |
| `itemtype` | Defines a microdata vocabulary |
| `lang` | Defines content language |
| `nonce` | Provides a security nonce |
| `part` | Exposes a shadow-tree part |
| `popover` | Declares a popover element |
| `role` | Defines an ARIA role |
| `slot` | Assigns content to a Web Component slot |
| `spellcheck` | Controls spelling and grammar checking hints |
| `style` | Applies inline CSS |
| `tabindex` | Controls keyboard focus behavior |
| `title` | Provides advisory information |
| `translate` | Controls translation hints |
| `virtualkeyboardpolicy` | Controls virtual keyboard behavior for supported editable elements |
| `writingsuggestions` | Controls writing suggestions for supported editable elements |

Some attributes have specialized support or applicability requirements. Consult the HTML and ARIA specifications when implementing advanced features.

# 🗁 Best Practices

* Use meaningful IDs and keep them unique within a document.
* Use classes for reusable styling and element grouping.
* Prefer external CSS stylesheets over excessive inline styling.
* Use the `lang` attribute on the root HTML element.
* Specify `dir` appropriately for right-to-left and multilingual content.
* Use `hidden` to represent content that is currently not relevant.
* Use `tabindex="0"` and `tabindex="-1"` only when there is a clear focus-management requirement.
* Prefer native interactive HTML elements instead of recreating their behavior with generic elements.
* Use `data-*` attributes for small application-specific values.
* Do not store passwords, tokens, or other sensitive information in data attributes.
* Use ARIA attributes only when native HTML semantics are insufficient.
* Keep accessibility states synchronized with actual interface behavior.
* Avoid positive `tabindex` values because they make keyboard navigation harder to manage.
* Test keyboard navigation and screen-reader behavior.

# 🗁 Common Mistakes

### 🖆 Using Duplicate IDs

Incorrect:

```html
<div id="content">First</div>
<div id="content">Second</div>
```

IDs should be unique within the document.

Correct:

```html
<div id="content-one">First</div>
<div id="content-two">Second</div>
```

### 🖆 Using Inline Styles Everywhere

Avoid:

```html
<p style="color: red;">First</p>
<p style="color: red;">Second</p>
<p style="color: red;">Third</p>
```

Prefer reusable CSS classes:

```html
<p class="error">First</p>
<p class="error">Second</p>
<p class="error">Third</p>
```

```css
.error {
  color: red;
}
```

### 🖆 Using `title` as the Only Accessible Label

A tooltip is not a reliable replacement for a visible label or accessible name.

For form controls, use `<label>` and appropriate accessible naming.

### 🖆 Misusing `tabindex`

Avoid positive values:

```html
<div tabindex="5">First</div>
<div tabindex="1">Second</div>
```

This can create an unexpected focus sequence.

Prefer natural document order and native interactive elements.

### 🖆 Assuming `data-*` Values Are Numbers or Booleans

Data attributes are exposed as strings.

```html
<div id="item" data-count="10"></div>
```

```javascript
const item = document.querySelector("#item");

const count = Number(item.dataset.count);

console.log(count + 5);
```

Convert values explicitly when a number or boolean is required.

### 🖆 Treating ARIA as Automatic Behavior

Adding `aria-expanded="true"` does not automatically expand an element.

JavaScript and the actual interface state must implement the behavior and keep the accessibility attributes synchronized.

# 🗁 Practical Exercises

### 🖆 Exercise: Create a User Profile

Create a profile component using global attributes.

Requirements:

* Use a unique `id`.
* Assign multiple `class` values.
* Set the language using `lang`.
* Store a user ID using `data-user-id`.
* Use JavaScript to read and update the user information.

### 🖆 Exercise: Build a Content Visibility Toggle

Create a button that shows and hides a content section.

Requirements:

* Use the `hidden` attribute.
* Toggle visibility with JavaScript.
* Keep the button's `aria-expanded` value synchronized with the actual visibility state.

### 🖆 Exercise: Create an Editable Area

Create a text editor using `contenteditable`.

Requirements:

* Enable editing.
* Add a button to read the content.
* Display the edited text in another element.
* Experiment with `plaintext-only`.

### 🖆 Exercise: Implement Keyboard Navigation

Create a simple interface with multiple interactive elements.

Requirements:

* Use native buttons and links.
* Experiment with `tabindex="0"`.
* Experiment with `tabindex="-1"` and JavaScript's `focus()` method.
* Test the interface using only a keyboard.

### 🖆 Exercise: Build a Multilingual Page

Create a page containing English and Bengali content.

Requirements:

* Set the root document language.
* Use `lang="bn"` for Bengali content.
* Use the `dir` attribute for appropriate text direction.
* Inspect the resulting document structure.

# 🗁 Interview Questions

**What are HTML global attributes?**

Global attributes are attributes that can be used on all HTML elements, providing shared functionality such as identification, styling, interaction, and accessibility.

**What is the difference between `id` and `class`?**

An ID identifies a unique element within a document, while classes can be shared by multiple elements.

**What is the purpose of the `title` attribute?**

It provides advisory information about an element, commonly displayed as a tooltip.

**What is the purpose of the `hidden` attribute?**

It indicates that an element is not currently relevant and should not be rendered.

**What is the difference between `hidden` and `display: none`?**

Both can prevent an element from being rendered, but `hidden` is an HTML attribute representing a hidden state, while `display: none` is a CSS presentation rule.

**What does the `lang` attribute do?**

It identifies the language of an element's content, helping browsers and assistive technologies interpret text correctly.

**What is the purpose of `tabindex`?**

It controls whether an element can receive keyboard focus and how it participates in sequential keyboard navigation.

**What is the difference between `tabindex="0"` and `tabindex="-1"`?**

`tabindex="0"` allows sequential keyboard navigation, while `tabindex="-1"` allows programmatic focus but excludes the element from sequential navigation.

**What is the purpose of `contenteditable`?**

It allows users to edit the content of an HTML element directly in the browser.

**What are `data-*` attributes?**

They are custom HTML attributes used to store application-specific data on elements.

**How do you access data attributes using JavaScript?**

Use the element's `dataset` property.

```javascript
element.dataset.userId;
```

**What is the purpose of the `inert` attribute?**

It makes an element and its descendants non-interactive and removes them from the normal accessibility and focus interaction model.

**Are ARIA attributes the same as native HTML attributes?**

ARIA attributes provide accessibility semantics and state information. They do not automatically implement interactive behavior.

**Why should positive `tabindex` values generally be avoided?**

They can create an unexpected keyboard focus order that is difficult to maintain and understand.

# 🗁 References

* [MDN: Global Attributes](https://developer.mozilla.org/en-US/docs/Web/HTML/Reference/Global_attributes)
* [MDN: HTML Attributes](https://developer.mozilla.org/en-US/docs/Web/HTML/Attributes)
* [MDN: `data-*` Attributes](https://developer.mozilla.org/en-US/docs/Web/HTML/How_to/Use_data_attributes)
* [MDN: `tabindex`](https://developer.mozilla.org/en-US/docs/Web/HTML/Reference/Global_attributes/tabindex)
* [MDN: `contenteditable`](https://developer.mozilla.org/en-US/docs/Web/HTML/Reference/Global_attributes/contenteditable)
* [WAI-ARIA Specification](https://www.w3.org/TR/wai-aria/)
* [WHATWG: HTML Living Standard](https://html.spec.whatwg.org/)
