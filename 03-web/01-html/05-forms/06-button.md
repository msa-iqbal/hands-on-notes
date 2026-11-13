# HTML Button Element

The HTML `<button>` element creates a clickable button that users can interact with to perform actions.

Buttons are commonly used for:

* Submitting forms
* Resetting form fields
* Triggering JavaScript functions
* Opening dialogs
* Expanding or collapsing content
* Navigating application workflows

The `<button>` element is more flexible than using an `<input>` element with `type="button"` because it can contain text, HTML elements, and other phrasing content.

# 🗁 Basic Syntax

The `<button>` element defines a clickable button.

```html
<button type="button">Click Me</button>
```

A button can also contain other HTML elements.

```html
<button type="button">
    <strong>Save</strong> Changes
</button>
```

**How it works:**

* `<button>` creates the button.
* `type` specifies the button's behavior.
* The content between the opening and closing tags defines its visible content.

Unlike `<input>`, the `<button>` element has an opening tag and a closing tag.

# 🗁 Button Types

The `<button>` element supports three main types:

* `submit`
* `reset`
* `button`

### 🖆 Submit Button

The `submit` type submits its associated form.

```html
<form action="/register" method="POST">
    <label for="username">Username:</label>

    <input
        type="text"
        id="username"
        name="username"
        required
    >

    <button type="submit">Register</button>
</form>
```

When the user clicks Register, the browser performs form validation and submits the form if it is valid.

### 🖆 Reset Button

The `reset` type resets the form controls to their initial values.

```html
<form>
    <label for="username">Username:</label>

    <input
        type="text"
        id="username"
        name="username"
        value="John"
    >

    <button type="reset">Reset Form</button>
</form>
```

Clicking Reset Form restores the input to its initial value.

Reset does not necessarily clear every field. It restores controls to their default states.

### 🖆 Regular Button

The `button` type creates a button without a default form-submission or reset action.

It is commonly used with JavaScript.

```html
<button type="button" id="showMessage">
    Show Message
</button>

<script>
    const button = document.querySelector("#showMessage");

    button.addEventListener("click", () => {
        alert("Hello World!");
    });
</script>
```

### 🖆 Button Type Comparison

| Type | Behavior | Common Use |
| --- | --- | --- |
| `submit` | Submits the associated form | Registration, login, contact forms |
| `reset` | Restores form controls to their initial values | Resetting a form |
| `button` | Has no default form action | JavaScript interactions |

**Important:** A `<button>` element associated with a form defaults to `submit` when its type is omitted. Always specify the `type` attribute to avoid unintended form submission.

# 🗁 Button Attributes

The `<button>` element supports several attributes.

| Attribute | Description |
| --- | --- |
| `type` | Defines the button's behavior. |
| `name` | Defines the button's form field name. |
| `value` | Defines the button's submitted value. |
| `disabled` | Disables the button. |
| `autofocus` | Requests focus when the page loads. |
| `form` | Associates the button with a form by its ID. |
| `formaction` | Overrides the form's submission URL. |
| `formenctype` | Overrides the form's encoding type. |
| `formmethod` | Overrides the form's submission method. |
| `formnovalidate` | Submits the form without constraint validation. |
| `formtarget` | Overrides where the response is displayed. |

### 🖆 Using Disabled

The `disabled` attribute prevents users from interacting with the button.

```html
<button type="submit" disabled>
    Submit
</button>
```

A disabled button cannot be activated by normal user interaction and does not receive focus through standard keyboard navigation.

### 🖆 Using Name and Value

A submit button can contribute its own name/value pair to form submission when it is the button used to submit the form.

```html
<form action="/save" method="POST">
    <input type="text" name="username">

    <button type="submit" name="action" value="save">
        Save
    </button>

    <button type="submit" name="action" value="publish">
        Publish
    </button>
</form>
```

If the user clicks Publish, the submitted data includes:

```text
action=publish
```

The clicked submit button can therefore communicate which action the user selected.

### 🖆 Using Form

The `form` attribute associates a button with a form elsewhere in the document.

```html
<form id="registrationForm" action="/register" method="POST">
    <input type="text" name="username">
</form>

<button type="submit" form="registrationForm">
    Register
</button>
```

The button submits the form even though it is outside the form element.

### 🖆 Using Formaction and Formmethod

These attributes override the associated form's submission settings.

```html
<form action="/save" method="POST">
    <input type="text" name="content">

    <button type="submit">
        Save
    </button>

    <button
        type="submit"
        formaction="/preview"
        formmethod="GET"
    >
        Preview
    </button>
</form>
```

The Save button submits to `/save` using POST.

The Preview button submits to `/preview` using GET.

### 🖆 Using Formnovalidate

The `formnovalidate` attribute allows a submit button to bypass the browser's built-in constraint validation.

```html
<form action="/register" method="POST">
    <input
        type="email"
        name="email"
        required
    >

    <button type="submit">
        Register
    </button>

    <button type="submit" formnovalidate>
        Save Draft
    </button>
</form>
```

Register performs normal validation. Save Draft bypasses interactive constraint validation.

Server-side validation should still be applied.

# 🗁 Button and Forms

Buttons are commonly used to control form submission and other form-related actions.

### 🖆 Complete Form Example

```html
<form action="/contact" method="POST">
    <div>
        <label for="name">Full Name:</label>

        <input
            type="text"
            id="name"
            name="name"
            required
        >
    </div>

    <div>
        <label for="email">Email:</label>

        <input
            type="email"
            id="email"
            name="email"
            required
        >
    </div>

    <div>
        <label for="message">Message:</label>

        <textarea
            id="message"
            name="message"
            required
        ></textarea>
    </div>

    <button type="submit">
        Send Message
    </button>

    <button type="reset">
        Clear Form
    </button>
</form>
```

### 🖆 Multiple Submit Buttons

Different submit buttons can trigger different actions.

```html
<form action="/article" method="POST">
    <input
        type="text"
        name="title"
        required
    >

    <button type="submit" name="action" value="draft">
        Save Draft
    </button>

    <button type="submit" name="action" value="publish">
        Publish
    </button>
</form>
```

The server can inspect the submitted `action` value to determine which operation the user requested.

# 🗁 Button vs Input Button

HTML provides both `<button>` and `<input>` elements for creating buttons.

### 🖆 Using Button

```html
<button type="submit">
    Submit Form
</button>
```

### 🖆 Using Input

```html
<input type="submit" value="Submit Form">
```

### 🖆 Comparison

| Feature | `<button>` | `<input>` |
| --- | --- | --- |
| Closing tag | Required | Void element |
| Custom HTML content | Supported | Not supported |
| Visible label | Element content | `value` attribute |
| CSS styling | Supported | Supported |
| Icon and text combination | Supported | Not directly supported |

Use `<button>` when you need flexible content such as text, icons, or other phrasing elements.

# 🗁 Working with Buttons Using JavaScript

JavaScript allows buttons to trigger custom behavior.

### 🖆 Handling Click Events

```html
<button type="button" id="clickButton">
    Click Me
</button>

<script>
    const button = document.querySelector("#clickButton");

    button.addEventListener("click", () => {
        console.log("Button clicked!");
    });
</script>
```

### 🖆 Changing Button Text

```html
<button type="button" id="changeButton">
    Original Text
</button>

<script>
    const button = document.querySelector("#changeButton");

    button.addEventListener("click", () => {
        button.textContent = "Text Changed!";
    });
</script>
```

### 🖆 Disabling a Button

```html
<button type="button" id="disableButton">
    Disable Me
</button>

<script>
    const button = document.querySelector("#disableButton");

    button.addEventListener("click", () => {
        button.disabled = true;
    });
</script>
```

### 🖆 Toggle Button State

```html
<button
    type="button"
    id="toggleButton"
    aria-pressed="false"
>
    Enable Notifications
</button>

<script>
    const button = document.querySelector("#toggleButton");

    button.addEventListener("click", () => {
        const isPressed = button.getAttribute("aria-pressed") === "true";

        button.setAttribute("aria-pressed", String(!isPressed));

        button.textContent = isPressed
            ? "Enable Notifications"
            : "Disable Notifications";
    });
</script>
```

`aria-pressed` communicates the current state of a toggle button to assistive technologies.

### 🖆 Preventing Default Form Submission

A regular button inside a form should use `type="button"` when it is not intended to submit the form.

```html
<form>
    <input type="text" name="username">

    <button type="button" id="checkButton">
        Check Username
    </button>
</form>

<script>
    const button = document.querySelector("#checkButton");

    button.addEventListener("click", () => {
        console.log("Checking username...");
    });
</script>
```

This prevents the button from accidentally submitting the form.

# 🗁 Creating Different Button Styles

CSS can be used to customize button appearance.

### 🖆 Basic Styling

```html
<button type="button" class="primary-button">
    Get Started
</button>
```

```css
.primary-button {
    padding: 10px 20px;
    border: none;
    border-radius: 6px;
    background-color: #2563eb;
    color: white;
    font-size: 16px;
    cursor: pointer;
}

.primary-button:hover {
    background-color: #1d4ed8;
}

.primary-button:focus-visible {
    outline: 3px solid #93c5fd;
    outline-offset: 3px;
}

.primary-button:disabled {
    opacity: 0.5;
    cursor: not-allowed;
}
```

### 🖆 Button with Icon

```html
<button type="button">
    <span aria-hidden="true">+</span>
    Add Item
</button>
```

The decorative plus symbol is hidden from assistive technologies because the button already has a meaningful text label.

### 🖆 Full-Width Button

```css
.full-width-button {
    width: 100%;
    padding: 12px;
}
```

```html
<button type="submit" class="full-width-button">
    Submit
</button>
```

# 🗁 Accessible Buttons

Buttons are interactive controls, so their purpose and behavior must be clear.

### 🖆 Use Meaningful Button Text

Avoid unclear labels such as:

```html
<button type="button">Click Here</button>
```

Prefer descriptive labels:

```html
<button type="button">Download Report</button>
```

### 🖆 Use Native Button Elements

Use `<button>` for actions rather than making a generic `<div>` or `<span>` behave like a button.

Native buttons provide built-in keyboard interaction, focus behavior, and accessibility semantics.

### 🖆 Accessible Icon-Only Buttons

When a button contains only an icon, provide an accessible name.

```html
<button type="button" aria-label="Close dialog">
    <span aria-hidden="true">×</span>
</button>
```

### 🖆 Use ARIA for Toggle State

```html
<button
    type="button"
    aria-pressed="false"
>
    Favorite
</button>
```

Update `aria-pressed` whenever the toggle state changes.

### 🖆 Preserve Focus Visibility

```css
button:focus-visible {
    outline: 3px solid #2563eb;
    outline-offset: 3px;
}
```

Keyboard users must be able to identify which control currently has focus.

# 🗁 Best Practices

* Always specify the `type` attribute.
* Use `type="submit"` for form submission.
* Use `type="reset"` only when resetting the form is an intentional user action.
* Use `type="button"` for JavaScript interactions.
* Use descriptive button labels.
* Use native `<button>` elements instead of clickable `<div>` elements.
* Use `disabled` when an action is temporarily unavailable.
* Use `aria-label` for icon-only buttons when visible text is unavailable.
* Use `aria-pressed` for toggle buttons.
* Preserve visible keyboard focus indicators.
* Avoid nesting interactive elements such as links or buttons inside a button.
* Keep button behavior predictable and consistent.

# 🗁 Common Mistakes

### 🖆 Forgetting the Button Type

Incorrect:

```html
<form>
    <button>Check Status</button>
</form>
```

The button defaults to form submission.

Correct:

```html
<form>
    <button type="button">Check Status</button>
</form>
```

### 🖆 Using a Button Without an Accessible Name

Incorrect:

```html
<button type="button">
    <span aria-hidden="true">×</span>
</button>
```

Correct:

```html
<button type="button" aria-label="Close">
    <span aria-hidden="true">×</span>
</button>
```

### 🖆 Using Div as a Button

Incorrect:

```html
<div onclick="saveData()">Save</div>
```

Correct:

```html
<button type="button" onclick="saveData()">
    Save
</button>
```

A native button provides keyboard and accessibility behavior without requiring custom implementation.

### 🖆 Using Reset Unintentionally

A reset button restores controls to their initial values. It does not simply clear the form.

Use it only when that behavior is useful and clear to the user.

### 🖆 Nesting Interactive Elements

Incorrect:

```html
<button type="button">
    <a href="/dashboard">Dashboard</a>
</button>
```

Correct:

```html
<a href="/dashboard" class="button">
    Dashboard
</a>
```

Use a link for navigation and a button for actions.

# 🗁 Practical Exercises

### 🖆 Exercise: Registration Form

Create a registration form with:

* Username
* Email
* Password
* Submit button
* Reset button

Requirements:

* Use appropriate input types.
* Make required fields mandatory.
* Use correct button types.

### 🖆 Exercise: JavaScript Button

Create a button that displays a message when clicked.

Requirements:

* Use `type="button"`.
* Handle the `click` event with JavaScript.
* Display the message in the browser console.

### 🖆 Exercise: Toggle Button

Create a notification toggle button.

Requirements:

* Use `aria-pressed`.
* Change the button text when toggled.
* Update the pressed state after every click.

### 🖆 Exercise: Multiple Form Actions

Create an article form with two submit buttons:

* Save Draft
* Publish

Requirements:

* Use the same form.
* Give each button a different `name` and `value`.
* Identify the selected action through submitted form data.

# 🗁 Interview Questions

**What is the purpose of the HTML `<button>` element?**

It creates an interactive button that can submit forms, reset form controls, or trigger JavaScript actions.

**What are the three button types?**

`submit`, `reset`, and `button`.

**What is the default type of a button associated with a form?**

The default type is `submit` when the `type` attribute is omitted.

**What is the difference between `type="button"` and `type="submit"`?**

A regular button has no default form action, while a submit button submits its associated form.

**What does the reset button do?**

It restores form controls to their initial values and states.

**What is the difference between `<button>` and `<input type="button">`?**

A `<button>` can contain HTML content, while an input button uses its `value` attribute as its visible label.

**What is the purpose of the `disabled` attribute?**

It prevents users from interacting with the button.

**What is the purpose of `formaction`?**

It overrides the associated form's `action` URL for a particular submit button.

**What is the purpose of `formnovalidate`?**

It allows a submit button to bypass the browser's built-in constraint validation.

**How do you handle button clicks using JavaScript?**

Use `addEventListener()` with the `click` event.

**What is the purpose of `aria-pressed`?**

It communicates whether a toggle button is currently pressed or not.

# 🗁 References

* [MDN: HTML Button Element](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/button)
* [MDN: HTMLButtonElement](https://developer.mozilla.org/en-US/docs/Web/API/HTMLButtonElement)
* [MDN: Button Types](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/button#type)
* [MDN: Form Validation](https://developer.mozilla.org/en-US/docs/Web/HTML/Constraint_validation)
* [WHATWG: The Button Element](https://html.spec.whatwg.org/multipage/form-elements.html#the-button-element)
