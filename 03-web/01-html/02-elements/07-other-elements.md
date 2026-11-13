# HTML Other Elements

HTML provides many additional elements that help structure content, display interactive information, handle user interactions, and represent specialized types of content.

Beyond headings, paragraphs, links, lists, and text-formatting elements, HTML includes elements for:

* Grouping and organizing content.
* Displaying expandable information.
* Showing progress and measurements.
* Representing dates and times.
* Handling mathematical and scientific notation.
* Creating interactive disclosure components.
* Displaying dialog boxes.
* Supporting bidirectional and specialized content.

This chapter covers useful HTML elements that do not fit into the previous basic element categories.

# 🗁 Figure and Figcaption

The `<figure>` and `<figcaption>` elements represent self-contained content with an optional caption.

They are commonly used for:

* Images.
* Diagrams.
* Illustrations.
* Code examples.
* Charts.
* Other content referenced from the main document.

### 🖆 The `<figure>` Element

The `<figure>` element represents self-contained content that can be moved independently from the main document without affecting its overall meaning.

```html
<figure>
  <img src="network.png" alt="Basic computer network diagram">
</figure>
```

### 🖆 The `<figcaption>` Element

The `<figcaption>` element provides a caption or legend for its parent `<figure>`.

```html
<figure>
  <img src="network.png" alt="Basic computer network diagram">

  <figcaption>
    Figure: Basic computer network architecture.
  </figcaption>
</figure>
```

A `<figcaption>` can appear as the first or last child of its `<figure>`.

### 🖆 Complete Example

```html
<figure>
  <img
    src="html-structure.png"
    alt="Diagram showing the structure of an HTML document"
  >

  <figcaption>
    The basic structure of an HTML document.
  </figcaption>
</figure>
```

# 🗁 Details and Summary

The `<details>` and `<summary>` elements create a native expandable and collapsible disclosure component.

They are useful for:

* Frequently asked questions.
* Additional information.
* Documentation sections.
* Expandable explanations.
* Optional content.

### 🖆 The `<details>` Element

The `<details>` element creates a disclosure widget that users can open or close.

```html
<details>
  <p>HTML stands for HyperText Markup Language.</p>
</details>
```

By default, the content is hidden until the user opens the disclosure widget.

### 🖆 The `<summary>` Element

The `<summary>` element provides the visible heading or label for the disclosure widget.

It should be the first child of `<details>`.

```html
<details>
  <summary>What is HTML?</summary>

  <p>
    HTML is the standard markup language used
    to structure content on the web.
  </p>
</details>
```

### 🖆 Open by Default

Use the `open` attribute to make the disclosure widget expanded initially.

```html
<details open>
  <summary>HTML Information</summary>

  <p>
    HTML defines the structure and meaning
    of web content.
  </p>
</details>
```

### 🖆 Multiple Disclosure Widgets

```html
<section>
  <h2>Frequently Asked Questions</h2>

  <details>
    <summary>What is HTML?</summary>
    <p>HTML structures web content.</p>
  </details>

  <details>
    <summary>What is CSS?</summary>
    <p>CSS controls the presentation of web content.</p>
  </details>

  <details>
    <summary>What is JavaScript?</summary>
    <p>JavaScript adds behavior and interactivity.</p>
  </details>
</section>
```

### 🖆 Styling Disclosure Widgets

```css
details {
  padding: 12px;
  border: 1px solid #ddd;
  border-radius: 6px;
  margin-bottom: 10px;
}

summary {
  cursor: pointer;
  font-weight: bold;
}
```

### 🖆 Important Attributes

| Attribute | Purpose                                                                                         |
| --------- | ----------------------------------------------------------------------------------------------- |
| `open`    | Controls whether the details content is expanded initially                                      |
| `name`    | Groups disclosure widgets so that opening one can automatically close another in the same group |

Example of grouped disclosure widgets:

```html
<details name="faq">
  <summary>Question One</summary>
  <p>Answer One</p>
</details>

<details name="faq">
  <summary>Question Two</summary>
  <p>Answer Two</p>
</details>
```

The `name` attribute enables an exclusive accordion-like behavior for grouped disclosure widgets.

# 🗁 Dialog

The `<dialog>` element represents a dialog box or other interactive component, such as a modal window.

It can be used for:

* Confirmation dialogs.
* Login dialogs.
* Notifications.
* Forms.
* Important user interactions.

### 🖆 Basic Dialog

```html
<dialog id="myDialog">
  <p>Hello! This is a dialog.</p>
  <button type="button">Close</button>
</dialog>
```

A dialog without the `open` attribute is initially closed.

### 🖆 Opening a Dialog

Use JavaScript's `show()` or `showModal()` method.

```html
<button id="openDialog" type="button">
  Open Dialog
</button>

<dialog id="myDialog">
  <p>Welcome to the website!</p>

  <button id="closeDialog" type="button">
    Close
  </button>
</dialog>

<script>
  const dialog = document.querySelector("#myDialog");

  document
    .querySelector("#openDialog")
    .addEventListener("click", () => {
      dialog.showModal();
    });

  document
    .querySelector("#closeDialog")
    .addEventListener("click", () => {
      dialog.close();
    });
</script>
```

### 🖆 `show()` vs `showModal()`

| Method | Behavior |
| --- | --- |
| `show()` | Opens a non-modal dialog |
| `showModal()` | Opens a modal dialog above the rest of the page |
| `close()` | Closes the dialog |

A modal dialog prevents interaction with the rest of the page while it is open.

### 🖆 Dialog with a Form

The `method="dialog"` form submission method closes the dialog when the form is submitted.

```html
<dialog id="confirmDialog">
  <form method="dialog">
    <h2>Confirm Action</h2>

    <p>Are you sure you want to continue?</p>

    <button value="cancel">Cancel</button>
    <button value="confirm">Confirm</button>
  </form>
</dialog>
```

The submitted button's value becomes the dialog's return value.

### 🖆 Dialog Events and Properties

Useful dialog features include:

* `open`: Indicates whether the dialog is open.
* `returnValue`: Contains the value submitted when the dialog closes.
* `close`: Event fired when the dialog closes.
* `cancel`: Event associated with a cancellation request, such as pressing Escape on a modal dialog.

# 🗁 Time

The `<time>` element represents a specific period in time or a date.

It can provide machine-readable date and time information through the `datetime` attribute.

### 🖆 Basic Example

```html
<p>
  The meeting is scheduled for
  <time datetime="2026-11-08">November 8, 2026</time>.
</p>
```

### 🖆 Representing Time

```html
<p>
  The event starts at
  <time datetime="14:30">2:30 PM</time>.
</p>
```

### 🖆 Representing Date and Time Together

```html
<p>
  Published:
  <time datetime="2026-11-08T14:30:00+06:00">
    November 8, 2026, at 2:30 PM
  </time>
</p>
```

### 🖆 Machine-Readable Values

| Attribute | Purpose |
|---|---|
| `datetime` | Provides a machine-readable date or time value |

The `datetime` value should follow an appropriate valid date, time, or duration format.

# 🗁 Progress

The `<progress>` element represents the completion progress of a task.

It is commonly used for:

* File uploads.
* Downloads.
* Installation processes.
* Form completion.
* Background operations.

### 🖆 Determinate Progress

A determinate progress bar has a known maximum and current value.

```html
<label for="download">Download Progress:</label>

<progress id="download" value="70" max="100">
  70%
</progress>
```

Here:

* `value` represents the current progress.
* `max` represents the maximum progress value.

The progress is 70% complete.

### 🖆 Indeterminate Progress

Remove the `value` attribute to indicate that progress is ongoing but cannot currently be measured.

```html
<label for="loading">Loading:</label>

<progress id="loading"></progress>
```

### 🖆 Updating Progress with JavaScript

```html
<label for="upload">Upload Progress:</label>

<progress id="upload" value="0" max="100"></progress>

<p id="progressText">0%</p>

<script>
  const progress = document.querySelector("#upload");
  const progressText = document.querySelector("#progressText");

  let value = 0;

  const interval = setInterval(() => {
    value += 10;

    progress.value = value;
    progressText.textContent = `${value}%`;

    if (value >= 100) {
      clearInterval(interval);
    }
  }, 500);
</script>
```

The example simulates progress. Real upload progress should be connected to the actual upload operation.

# 🗁 Meter

The `<meter>` element represents a scalar measurement within a known range.

It is useful for:

* Storage usage.
* Battery levels.
* Disk capacity.
* Scores.
* Ratings.
* Resource utilization.

### 🖆 Basic Example

```html
<label for="storage">Storage Usage:</label>

<meter
  id="storage"
  value="75"
  min="0"
  max="100"
>
  75%
</meter>
```

### 🖆 Important Attributes

| Attribute | Purpose |
| --- | --- |
| `value` | Current measurement |
| `min` | Minimum allowed range |
| `max` | Maximum allowed range |
| `low` | Upper boundary of the low range |
| `high` | Lower boundary of the high range |
| `optimum` | Preferred or optimal value |

### 🖆 Complete Example

```html
<label for="disk">Disk Usage:</label>

<meter
  id="disk"
  min="0"
  max="100"
  low="30"
  high="80"
  optimum="20"
  value="65"
>
  65%
</meter>
```

### 🖆 Progress vs Meter

| Feature | `<progress>` | `<meter>` |
| --- | --- | --- |
| Purpose | Task completion | Scalar measurement |
| Example | File upload progress | Disk usage |
| Represents a task | Yes | No |
| Represents a measurement | Not its primary purpose | Yes |

Use `<progress>` when tracking the completion of an operation and `<meter>` when displaying a measurement within a known range.

# 🗁 Output

The `<output>` element represents the result of a calculation or the result of a user action.

It is commonly used with forms and JavaScript calculations.

### 🖆 Basic Example

```html
<form oninput="result.value = Number(a.value) + Number(b.value)">
  <input type="number" id="a" value="10">
  +
  <input type="number" id="b" value="20">
  =
  <output name="result" for="a b">30</output>
</form>
```

### 🖆 Using JavaScript

```html
<form id="calculator">
  <label for="numberOne">First Number:</label>
  <input type="number" id="numberOne" value="10">

  <label for="numberTwo">Second Number:</label>
  <input type="number" id="numberTwo" value="20">

  <output id="result">30</output>
</form>

<script>
  const first = document.querySelector("#numberOne");
  const second = document.querySelector("#numberTwo");
  const result = document.querySelector("#result");

  function calculate() {
    result.value = Number(first.value) + Number(second.value);
  }

  first.addEventListener("input", calculate);
  second.addEventListener("input", calculate);
</script>
```

The `<output>` element is designed to represent calculated or user-generated results.

# 🗁 Data

The `<data>` element associates human-readable content with a machine-readable value.

It can be useful when a displayed value needs a corresponding standardized or application-specific representation.

### 🖆 Basic Example

```html
<p>
  Product:
  <data value="SKU-101">Wireless Headphones</data>
</p>
```

The visible text is `Wireless Headphones`, while the machine-readable value is `SKU-101`.

### 🖆 Product Example

```html
<ul>
  <li>
    <data value="1001">Laptop</data>
  </li>

  <li>
    <data value="1002">Keyboard</data>
  </li>

  <li>
    <data value="1003">Mouse</data>
  </li>
</ul>
```

The `value` attribute provides the machine-readable representation.

For date and time information, prefer `<time>` rather than `<data>`.

# 🗁 Ruby Annotation

Ruby annotations are used to provide pronunciation or reading information for East Asian typography.

HTML provides three elements:

* `<ruby>`: Represents ruby annotations.
* `<rt>`: Represents the annotation text.
* `<rp>`: Provides fallback parentheses for browsers that do not support ruby annotations.

### 🖆 Basic Example

```html
<ruby>
  漢 <rt>かん</rt>
</ruby>
```

### 🖆 Using Fallback Parentheses

```html
<ruby>
  漢
  <rp>(</rp>
  <rt>かん</rt>
  <rp>)</rp>
</ruby>
```

Ruby annotations are especially useful in Japanese and other East Asian writing systems.

# 🗁 Template

The `<template>` element holds HTML content that is not rendered immediately when the page loads.

It is useful for creating reusable HTML structures that JavaScript can clone and insert into the document.

### 🖆 Basic Example

```html
<template id="userTemplate">
  <div class="user-card">
    <h2 class="user-name"></h2>
    <p class="user-email"></p>
  </div>
</template>
```

The template's content is stored separately and is not displayed by default.

### 🖆 Using Template with JavaScript

```html
<div id="userList"></div>

<template id="userTemplate">
  <div class="user-card">
    <h2 class="user-name"></h2>
    <p class="user-email"></p>
  </div>
</template>

<script>
  const template = document.querySelector("#userTemplate");
  const userList = document.querySelector("#userList");

  const users = [
    {
      name: "John Doe",
      email: "john@example.com"
    },
    {
      name: "Jane Smith",
      email: "jane@example.com"
    }
  ];

  users.forEach((user) => {
    const clone = template.content.cloneNode(true);

    clone.querySelector(".user-name").textContent = user.name;
    clone.querySelector(".user-email").textContent = user.email;

    userList.appendChild(clone);
  });
</script>
```

### 🖆 Important Concepts

* Template content is not rendered as part of the normal document.
* Its content is available through the `content` property.
* `cloneNode(true)` creates a deep copy of the template content.
* JavaScript can modify the cloned content before inserting it into the document.

Use `textContent` when inserting untrusted text instead of assigning it to `innerHTML`.

# 🗁 Slot

The `<slot>` element is a placeholder used inside a Web Component's Shadow DOM.

It allows content supplied by the component consumer to appear at a defined location inside the component.

### 🖆 Basic Example

```html
<user-card>
  <span slot="name">John Doe</span>
</user-card>
```

A simplified custom element might use this template:

```html
<template id="userCardTemplate">
  <div class="card">
    <h2>
      <slot name="name">Default Name</slot>
    </h2>
  </div>
</template>
```

The `<slot>` element is primarily used with Web Components and Shadow DOM.

It is not a general-purpose replacement for `<div>` or `<span>`.

# 🗁 Math Elements

HTML provides the `<math>` element as an entry point for MathML, a markup language for representing mathematical notation.

Modern browsers support MathML Core for rendering mathematical expressions.

### 🖆 Basic Example

```html
<math>
  <mfrac>
    <mn>1</mn>
    <mn>2</mn>
  </mfrac>
</math>
```

This represents the fraction one-half.

### 🖆 Mathematical Expression

```html
<math display="block">
  <msup>
    <mi>x</mi>
    <mn>2</mn>
  </msup>

  <mo>+</mo>

  <msup>
    <mi>y</mi>
    <mn>2</mn>
  </msup>

  <mo>=</mo>

  <msup>
    <mi>z</mi>
    <mn>2</mn>
  </msup>
</math>
```

This represents:

x² + y² = z²

MathML is useful when mathematical notation needs to be represented structurally rather than inserted as an image.

# 🗁 Interactive Elements

HTML provides native elements that support user interaction without requiring custom JavaScript implementations for every behavior.

Some important examples include:

| Element | Purpose |
| --- | --- |
| `<details>` | Expandable disclosure widget |
| `<summary>` | Visible label for a details widget |
| `<dialog>` | Dialog or modal interface |
| `<button>` | User-triggered action |
| `<select>` | Selection control |
| `<input>` | Form input control |

### 🖆 Native HTML Interaction

```html
<details>
  <summary>Show more information</summary>

  <p>
    This content becomes visible when the
    disclosure widget is expanded.
  </p>
</details>
```

Native HTML interaction generally provides better keyboard and accessibility behavior than custom elements built entirely from generic containers.

# 🗁 Best Practices

* Use `<figure>` and `<figcaption>` for self-contained content with captions.
* Use `<details>` and `<summary>` for native expandable information.
* Use `<dialog>` for dialog interfaces instead of building every modal from generic containers.
* Use `<time>` with a machine-readable `datetime` value when representing dates and times.
* Use `<progress>` for task completion.
* Use `<meter>` for scalar measurements.
* Use `<output>` for calculated results.
* Use `<data>` when visible content needs a machine-readable value.
* Use `<template>` for reusable HTML structures.
* Use `<slot>` when building Web Components with Shadow DOM.
* Use MathML when mathematical expressions require structured markup.
* Prefer native HTML functionality before introducing custom JavaScript behavior.
* Test interactive elements with a keyboard and assistive technologies.

# 🗁 Common Mistakes

### 🖆 Using `<meter>` for Task Progress

Incorrect:

```html
<meter value="70" max="100"></meter>
```

If the value represents an upload or download operation, use `<progress>` instead.

```html
<progress value="70" max="100"></progress>
```

### 🖆 Using `<progress>` for Storage Capacity

Storage usage is a measurement, not necessarily a task being completed.

Use `<meter>`:

```html
<meter value="75" min="0" max="100">
  75%
</meter>
```

### 🖆 Using `<details>` Without `<summary>`

The `<summary>` element should provide the visible label for a disclosure widget.

```html
<details>
  <summary>More information</summary>
  <p>Additional information goes here.</p>
</details>
```

### 🖆 Treating `<template>` as Visible Content

Template content is not displayed automatically.

JavaScript must clone and insert the content when it is needed.

### 🖆 Using `<address>` for Every Postal Address

The `<address>` element is intended for contact information associated with the nearest article or the document.

Use ordinary text or another suitable structure for unrelated postal addresses.

# 🗁 Practical Exercises

### 🖆 Exercise: Build an FAQ Section

Create an FAQ section using `<details>` and `<summary>`.

Requirements:

* Include at least five questions.
* Provide an answer for each question.
* Group the disclosure widgets using the `name` attribute.
* Style the disclosure components with CSS.

### 🖆 Exercise: Create a Progress Dashboard

Build a dashboard containing:

* A file upload progress bar using `<progress>`.
* A storage usage indicator using `<meter>`.
* A text label showing the current percentage.

### 🖆 Exercise: Build a Dialog Component

Create a confirmation dialog using `<dialog>`.

Requirements:

* Open the dialog with a button.
* Include Confirm and Cancel buttons.
* Close the dialog after either action.
* Display the selected result on the page.

### 🖆 Exercise: Create a Dynamic User List

Use `<template>` and JavaScript to render a list of users.

Requirements:

* Create a reusable user-card template.
* Store user data in a JavaScript array.
* Clone the template for each user.
* Insert each user into the page using DOM methods.

### 🖆 Exercise: Display Mathematical Content

Create a page that displays mathematical expressions using MathML.

Include:

* A fraction.
* An exponent.
* A square root.
* A simple equation.

# 🗁 Interview Questions

**What is the purpose of the `<figure>` element?**

It represents self-contained content, such as an image, diagram, or code example, that can be referenced independently from the main content.

**What is the purpose of `<figcaption>`?**

It provides a caption or legend for a `<figure>` element.

**What are `<details>` and `<summary>` used for?**

They create a native expandable and collapsible disclosure component.

**What is the difference between `show()` and `showModal()`?**

`show()` opens a non-modal dialog, while `showModal()` opens a modal dialog that blocks interaction with the rest of the page.

**What is the purpose of the `<time>` element?**

It represents dates or times and can provide machine-readable values through the `datetime` attribute.

**What is the difference between `<progress>` and `<meter>`?**

`<progress>` represents task completion, while `<meter>` represents a measurement within a known range.

**What is the purpose of `<output>`?**

It represents the result of a calculation or user action.

**What is the purpose of `<data>`?**

It associates human-readable content with a machine-readable value.

**What is the purpose of `<template>`?**

It stores HTML content that is not rendered immediately and can be cloned and inserted into the document using JavaScript.

**What is the purpose of `<slot>`?**

It provides a placeholder for content supplied to a Web Component's Shadow DOM.

**What is the purpose of MathML?**

MathML represents mathematical notation using structured markup.

**Can `<details>` work without JavaScript?**

Yes. It provides native expandable and collapsible behavior without requiring JavaScript.

# 🗁 References

* [MDN: HTML Elements Reference](https://developer.mozilla.org/en-US/docs/Web/HTML/Reference/Elements)
* [MDN: `<figure>` Element](https://developer.mozilla.org/en-US/docs/Web/HTML/Reference/Elements/figure)
* [MDN: `<details>` Element](https://developer.mozilla.org/en-US/docs/Web/HTML/Reference/Elements/details)
* [MDN: `<dialog>` Element](https://developer.mozilla.org/en-US/docs/Web/HTML/Reference/Elements/dialog)
* [MDN: `<progress>` Element](https://developer.mozilla.org/en-US/docs/Web/HTML/Reference/Elements/progress)
* [MDN: `<meter>` Element](https://developer.mozilla.org/en-US/docs/Web/HTML/Reference/Elements/meter)
* [MDN: `<template>` Element](https://developer.mozilla.org/en-US/docs/Web/HTML/Reference/Elements/template)
* [MDN: `<slot>` Element](https://developer.mozilla.org/en-US/docs/Web/HTML/Reference/Elements/slot)
* [WHATWG: HTML Living Standard](https://html.spec.whatwg.org/)
* [W3C: MathML Core](https://www.w3.org/TR/mathml-core/)
