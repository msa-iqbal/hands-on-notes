# HTML Boolean Attributes

HTML boolean attributes represent a true-or-false condition on an HTML element.

A boolean attribute is considered **true when it is present** and **false when it is absent**.

Unlike regular HTML attributes, boolean attributes do not require a meaningful value.

They are commonly used to control element behavior, including:

* Disabling form controls.
* Making inputs required.
* Preventing users from editing content.
* Automatically focusing an element.
* Enabling form validation.
* Controlling media playback.

# 🗁 What Are Boolean Attributes?

A boolean attribute is an HTML attribute whose presence represents a true state.

### 🖆 Syntax

```html
<element attribute></element>
```

You can also write:

```html
<element attribute=""></element>
```

Or:

```html
<element attribute="attribute"></element>
```

All three forms represent the same true state for a standard HTML boolean attribute.

### 🖆 Example

```html
<input type="text" required>

<button disabled>Submit</button>

<input type="checkbox" checked>
```

In this example:

* `required` makes the input mandatory.
* `disabled` prevents interaction with the button.
* `checked` makes the checkbox initially checked.

# 🗁 How Boolean Attributes Work

Boolean attributes follow a simple rule:

| Attribute State | Meaning |
| --- | --- |
| Attribute present | `true` |
| Attribute absent | `false` |

### 🖆 Attribute Present

```html
<button disabled>
  Submit
</button>
```

The button is disabled.

### 🖆 Attribute Absent

```html
<button>
  Submit
</button>
```

The button is enabled.

### 🖆 Important: The String "false" Does Not Mean False

A common mistake is assigning the string `"false"` to a boolean attribute.

Incorrect:

```html
<button disabled="false">
  Submit
</button>
```

The button is still disabled because the `disabled` attribute is present.

Correct:

```html
<button>
  Submit
</button>
```

To make a boolean attribute false, remove the attribute.

# 🗁 Common HTML Boolean Attributes

HTML provides many built-in boolean attributes.

| Attribute | Purpose | Common Elements |
| --- | --- | --- |
| `disabled` | Disables an element | `button`, `input`, `select`, `textarea` |
| `checked` | Marks a checkbox or radio input as checked | `input` |
| `required` | Makes a form control mandatory | `input`, `select`, `textarea` |
| `readonly` | Prevents editing of a form control | `input`, `textarea` |
| `multiple` | Allows multiple values or selections | `input`, `select` |
| `autofocus` | Requests automatic focus when the page loads | Form controls, `button` |
| `selected` | Marks an option as selected | `option` |
| `hidden` | Indicates that an element is not currently relevant and should not be rendered | Most HTML elements |
| `open` | Indicates that a disclosure or dialog element is open | `details`, `dialog` |
| `autoplay` | Requests automatic media playback | `audio`, `video` |
| `controls` | Displays media controls | `audio`, `video` |
| `loop` | Repeats media playback | `audio`, `video` |
| `muted` | Sets media to muted | `audio`, `video` |
| `novalidate` | Disables built-in form validation on submission | `form` |
| `formnovalidate` | Disables validation for a particular submit button | `button`, `input` |
| `reversed` | Reverses the numbering direction of an ordered list | `ol` |
| `defer` | Requests deferred execution of an external classic script | `script` |
| `async` | Requests asynchronous execution of an external classic script | `script` |

The exact behavior of an attribute depends on the HTML element where it is used.

# 🗁 The disabled Attribute

The `disabled` attribute makes a form control unavailable for user interaction.

### 🖆 Example

```html
<button disabled>
  Submit
</button>

<input type="text" disabled>

<select disabled>
  <option>Option One</option>
</select>
```

Disabled controls generally:

* Cannot receive normal user interaction.
* Cannot receive keyboard focus.
* Are excluded from form submission.
* Do not participate in normal constraint validation.

### 🖆 Enabling and Disabling with JavaScript

```html
<button id="submit-button" disabled>
  Submit
</button>

<button id="enable-button">
  Enable Submit
</button>

<script>
  const submitButton = document.getElementById("submit-button");
  const enableButton = document.getElementById("enable-button");

  enableButton.addEventListener("click", () => {
    submitButton.disabled = false;
  });
</script>
```

The `disabled` DOM property provides convenient access to the boolean state.

# 🗁 The checked Attribute

The `checked` attribute specifies that a checkbox or radio button is initially checked.

### 🖆 Example

```html
<label>
  <input type="checkbox" checked>
  Accept Terms
</label>

<label>
  <input type="radio" name="gender" value="male" checked>
  Male
</label>

<label>
  <input type="radio" name="gender" value="female">
  Female
</label>
```

The checkbox and the first radio button are initially checked.

### 🖆 Checking and Unchecking with JavaScript

```html
<input type="checkbox" id="subscribe" checked>

<button id="toggle-button">
  Toggle Subscription
</button>

<script>
  const checkbox = document.getElementById("subscribe");
  const button = document.getElementById("toggle-button");

  button.addEventListener("click", () => {
    checkbox.checked = !checkbox.checked;
  });
</script>
```

**Important:** The HTML `checked` attribute represents the default checked state. The DOM `checked` property represents the current checked state, which can change through user interaction or JavaScript.

# 🗁 The required Attribute

The `required` attribute makes a form control mandatory before a form can be submitted successfully.

### 🖆 Example

```html
<form>
  <label for="username">Username</label>
  <input
    type="text"
    id="username"
    name="username"
    required
  >

  <button type="submit">Submit</button>
</form>
```

The browser prevents normal form submission when the required field is empty.

The `required` attribute works with supported form controls such as:

* `input`
* `select`
* `textarea`

# 🗁 The readonly Attribute

The `readonly` attribute prevents users from editing the value of a form control.

### 🖆 Example

```html
<input
  type="text"
  value="Read-only information"
  readonly
>

<textarea readonly>
This content cannot be edited.
</textarea>
```

### 🖆 readonly vs disabled

These attributes have different behaviors.

| Feature | `readonly` | `disabled` |
| --- | --- | --- |
| User can edit | No | No |
| Can receive focus | Yes, generally | No |
| Included in form submission | Yes | No |
| Participates in constraint validation | No | No |
| Applies to | Supported text-like inputs and `textarea` | Many form controls |

Use `readonly` when users should be able to focus on and copy a value without changing it.

Use `disabled` when a control should be unavailable for interaction.

# 🗁 The multiple Attribute

The `multiple` attribute allows users to select or provide multiple values where supported.

### 🖆 Multiple Selection in a Select Element

```html
<label for="skills">Select Your Skills</label>

<select id="skills" name="skills" multiple>
  <option value="html">HTML</option>
  <option value="css">CSS</option>
  <option value="javascript">JavaScript</option>
  <option value="typescript">TypeScript</option>
</select>
```

Users can select multiple options.

### 🖆 Multiple File Upload

```html
<label for="documents">Upload Documents</label>

<input
  type="file"
  id="documents"
  name="documents"
  multiple
>
```

Users can select more than one file.

# 🗁 The selected Attribute

The `selected` attribute specifies the default selected option in a dropdown list.

### 🖆 Example

```html
<select name="country">
  <option value="bd">Bangladesh</option>
  <option value="us" selected>United States</option>
  <option value="uk">United Kingdom</option>
</select>
```

The United States option is initially selected.

For a single-selection dropdown, specifying `selected` on multiple options can lead to the browser choosing the last selected option as the initial selection.

# 🗁 The autofocus Attribute

The `autofocus` attribute requests that an element receive focus automatically when the page or its containing dialog is displayed.

### 🖆 Example

```html
<form>
  <label for="email">Email Address</label>

  <input
    type="email"
    id="email"
    name="email"
    autofocus
  >

  <button type="submit">Submit</button>
</form>
```

Use autofocus carefully. Automatically moving keyboard focus can be disruptive for users of assistive technologies and keyboard navigation.

# 🗁 The hidden Attribute

The `hidden` attribute indicates that an element is not currently relevant and should not be rendered.

### 🖆 Example

```html
<p hidden>
  This content is hidden.
</p>
```

You can also control it with JavaScript.

```html
<p id="message" hidden>
  Welcome back!
</p>

<button id="show-button">
  Show Message
</button>

<script>
  const message = document.getElementById("message");
  const button = document.getElementById("show-button");

  button.addEventListener("click", () => {
    message.hidden = false;
  });
</script>
```

The `hidden` property can be used to show or hide an element.

**Note:** The `hidden` attribute is not the same as `disabled`, and it is not interchangeable with CSS `visibility: hidden` or `display: none` in every context.

# 🗁 The open Attribute

The `open` attribute controls whether a `<details>` element is initially expanded.

### 🖆 Example

```html
<details open>
  <summary>HTML Information</summary>

  <p>
    HTML is the standard markup language for web pages.
  </p>
</details>
```

Without `open`, the details section starts collapsed.

```html
<details>
  <summary>CSS Information</summary>

  <p>
    CSS is used to style web pages.
  </p>
</details>
```

# 🗁 Boolean Attributes with JavaScript

JavaScript provides two common ways to work with boolean attributes:

* DOM properties.
* DOM attribute methods.

### 🖆 Using DOM Properties

```javascript
const button = document.querySelector("button");

button.disabled = true;
button.disabled = false;
```

This is generally the most convenient approach for changing the state of built-in boolean attributes.

### 🖆 Using setAttribute()

```javascript
const button = document.querySelector("button");

button.setAttribute("disabled", "");
```

The attribute is present, so the button is disabled.

### 🖆 Using removeAttribute()

```javascript
button.removeAttribute("disabled");
```

The attribute is removed, so the button is enabled.

### 🖆 Using hasAttribute()

```javascript
if (button.hasAttribute("disabled")) {
  console.log("Button is disabled");
}
```

`hasAttribute()` checks whether an attribute exists.

### 🖆 DOM Property vs Attribute Methods

| Operation | DOM Property | Attribute Method |
| --- | --- | --- |
| Enable button | `button.disabled = false` | `button.removeAttribute("disabled")` |
| Disable button | `button.disabled = true` | `button.setAttribute("disabled", "")` |
| Check state | `button.disabled` | `button.hasAttribute("disabled")` |
| Check attribute presence | Not directly equivalent in every case | `button.hasAttribute("disabled")` |

For built-in HTML boolean attributes, DOM properties are usually the clearest way to manage the current state.

# 🗁 Boolean Attributes vs ARIA Boolean States

Not every attribute that looks like a boolean follows HTML boolean-attribute rules.

ARIA attributes such as `aria-expanded` and `aria-hidden` use explicit string values.

### 🖆 Example

```html
<button aria-expanded="false">
  Open Menu
</button>
```

Here, `aria-expanded="false"` means the expanded state is false.

Removing the attribute is not equivalent to setting it to `"false"`.

Compare:

```html
<button disabled="false">
  Submit
</button>
```

The button is disabled because `disabled` is a native HTML boolean attribute.

```html
<button aria-expanded="false">
  Open Menu
</button>
```

The button's ARIA expanded state is false because the attribute explicitly contains the string `"false"`.

### 🖆 Comparison

| Attribute | Type | False State |
| --- | --- | --- |
| `disabled` | HTML boolean attribute | Remove the attribute |
| `checked` | HTML boolean attribute | Remove the default attribute or set the current DOM property to `false` |
| `required` | HTML boolean attribute | Remove the attribute |
| `hidden` | HTML boolean attribute | Remove the attribute |
| `aria-expanded` | ARIA state attribute | Set its value to `"false"` |
| `aria-hidden` | ARIA state attribute | Set its value to `"false"` |

Do not treat ARIA attributes as native HTML boolean attributes.

# 🗁 Best Practices

* Remember that the presence of a native HTML boolean attribute means true.
* Remove the attribute to represent false.
* Do not write `disabled="false"` expecting to enable a control.
* Prefer DOM properties such as `element.disabled` and `element.checked` for managing current states.
* Use `required` for mandatory form controls.
* Use `readonly` when users should be able to focus on and copy a value without editing it.
* Use `disabled` when a control should be unavailable for interaction.
* Use `hidden` for content that should not currently be rendered.
* Use autofocus only when it provides a clear usability benefit.
* Do not confuse native HTML boolean attributes with ARIA attributes.
* Use server-side validation in addition to browser-side form validation.

# 🗁 Common Mistakes

### 🖆 Assigning the String "false"

Incorrect:

```html
<input type="text" required="false">
```

The input is still required.

Correct:

```html
<input type="text">
```

### 🖆 Using readonly on an Unsupported Input Type

The `readonly` attribute does not apply to every input type.

For example, it is not applicable to checkbox, radio, file, and button-type inputs.

Use `disabled` when those controls need to be unavailable for interaction.

### 🖆 Confusing the checked Attribute with the Current State

The `checked` HTML attribute establishes the default checked state.

Use the JavaScript property to inspect the current state:

```javascript
const checkbox = document.querySelector("input[type='checkbox']");

console.log(checkbox.checked);
```

### 🖆 Confusing HTML and ARIA Boolean Values

Incorrect understanding:

```html
<button aria-expanded>
  Menu
</button>
```

For ARIA states, provide an explicit value:

```html
<button aria-expanded="true">
  Menu
</button>
```

### 🖆 Assuming Disabled Values Are Submitted

Disabled form controls are not included in normal form submission.

If a value must be submitted but should not be editable, consider a readonly control where supported, or use another appropriate form data mechanism.

# 🗁 Practical Example

The following example demonstrates several boolean attributes and JavaScript state management.

```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">

  <title>HTML Boolean Attributes</title>
</head>
<body>

  <h1>Registration Form</h1>

  <form id="registration-form">

    <label for="username">Username</label>
    <input
      type="text"
      id="username"
      name="username"
      required
      autofocus
    >

    <br><br>

    <label for="email">Email</label>
    <input
      type="email"
      id="email"
      name="email"
      required
    >

    <br><br>

    <label>
      <input
        type="checkbox"
        id="terms"
        name="terms"
        required
      >
      Accept Terms and Conditions
    </label>

    <br><br>

    <button type="submit" id="submit-button" disabled>
      Submit
    </button>

  </form>

  <br>

  <button id="enable-button">
    Enable Submit Button
  </button>

  <script>
    const terms = document.getElementById("terms");
    const submitButton = document.getElementById("submit-button");
    const enableButton = document.getElementById("enable-button");

    enableButton.addEventListener("click", () => {
      submitButton.disabled = false;
    });

    terms.addEventListener("change", () => {
      submitButton.disabled = !terms.checked;
    });
  </script>

</body>
</html>
```

**How it works:**

* `required` makes the username, email, and terms checkbox mandatory.
* `autofocus` requests initial focus on the username field.
* `disabled` initially prevents interaction with the submit button.
* JavaScript changes the button's disabled state.
* The checkbox's `checked` property represents its current state.

The browser still performs its normal form validation when the form is submitted.

# 🗁 Practice Exercises

* Create a form with required username and email fields.
* Create a disabled button and enable it using JavaScript.
* Create a checkbox that controls another input's disabled state.
* Create a dropdown with one initially selected option.
* Create a file input that accepts multiple files.
* Create a `<details>` element that starts expanded.
* Hide and show an element using the `hidden` property.
* Compare the behavior of `readonly` and `disabled` form controls.
* Create a button that updates its `aria-expanded` state when a menu opens or closes.

# 🗁 Interview Questions

**What is a boolean attribute in HTML?**

A boolean attribute represents a true-or-false condition. Its presence means true, and its absence means false.

**What happens when you write `disabled="false"`?**

The element remains disabled because the attribute is present.

**How do you remove a boolean attribute using JavaScript?**

Use `removeAttribute()` or update the corresponding DOM property.

**What is the difference between `readonly` and `disabled`?**

Readonly controls cannot be edited but can generally receive focus and are submitted with the form. Disabled controls cannot normally receive focus or interaction and are excluded from form submission.

**What is the difference between the `checked` attribute and the `checked` property?**

The HTML attribute represents the default checked state, while the DOM property represents the current checked state.

**What does the `required` attribute do?**

It makes a supported form control mandatory before normal form submission.

**What is the purpose of the `multiple` attribute?**

It allows multiple selections in supported controls, such as a `<select>` element or file input.

**What does the `hidden` attribute do?**

It indicates that an element is not currently relevant and should not be rendered.

**Are ARIA attributes boolean HTML attributes?**

No. ARIA states such as `aria-expanded` use explicit string values like `"true"` and `"false"`.

**How can you check whether a boolean attribute exists?**

Use `element.hasAttribute("attribute-name")`.

# 🗁 Official References

* [MDN — Boolean Attributes](https://developer.mozilla.org/en-US/docs/Glossary/Boolean/HTML)
* [MDN — HTML Attributes](https://developer.mozilla.org/en-US/docs/Web/HTML/Reference/Attributes)
* [MDN — HTMLInputElement.disabled](https://developer.mozilla.org/en-US/docs/Web/API/HTMLInputElement/disabled)
* [MDN — HTMLInputElement.checked](https://developer.mozilla.org/en-US/docs/Web/API/HTMLInputElement/checked)
* [MDN — HTMLInputElement.readOnly](https://developer.mozilla.org/en-US/docs/Web/API/HTMLInputElement/readOnly)
* [WHATWG — HTML Living Standard](https://html.spec.whatwg.org/)
