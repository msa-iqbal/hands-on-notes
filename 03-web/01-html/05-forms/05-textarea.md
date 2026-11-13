# HTML Textarea Element

The HTML `<textarea>` element creates a multi-line text input control.

Unlike the `<input>` element, which is commonly used for single-line input, `<textarea>` allows users to enter longer text across multiple lines.

It is commonly used for:

* Comments and feedback
* Contact messages
* User biographies
* Reviews and descriptions
* Support requests
* Article content

# 🗁 Basic Syntax

The `<textarea>` element defines a multi-line text input control.

```html
<label for="message">Your Message:</label>

<textarea
    id="message"
    name="message"
    rows="5"
    cols="40"
></textarea>
```

**How it works:**

* `<textarea>` creates a multi-line text input.
* `id` identifies the element and connects it to its label.
* `name` identifies the form field during submission.
* `rows` specifies the visible number of text lines.
* `cols` specifies the visible width in average character units.

Unlike `<input>`, `<textarea>` has an opening tag and a closing tag.

# 🗁 Textarea Attributes

The `<textarea>` element supports several attributes to control its behavior.

| Attribute | Description |
| --- | --- |
| `name` | Defines the form field name. |
| `id` | Identifies the element. |
| `rows` | Specifies the visible number of text lines. |
| `cols` | Specifies the visible width in character units. |
| `placeholder` | Displays a temporary hint when the field is empty. |
| `required` | Requires the user to enter a value. |
| `minlength` | Specifies the minimum number of characters. |
| `maxlength` | Specifies the maximum number of characters. |
| `readonly` | Prevents editing while allowing the value to be submitted. |
| `disabled` | Disables the control and excludes it from form submission. |
| `autofocus` | Requests focus when the page loads. |
| `wrap` | Controls how text is wrapped when submitted. |
| `form` | Associates the textarea with a form by its ID. |
| `autocomplete` | Provides an autofill hint. |

### 🖆 Using Required

The `required` attribute makes the textarea mandatory.

```html
<form>
    <label for="message">Message:</label>

    <textarea
        id="message"
        name="message"
        required
    ></textarea>

    <button type="submit">Submit</button>
</form>
```

The browser prevents form submission when the required textarea is empty.

### 🖆 Using Readonly

The `readonly` attribute allows users to view and select text but prevents editing.

```html
<textarea name="terms" readonly>
These are the terms and conditions.
</textarea>
```

A readonly textarea is included in form submission.

### 🖆 Using Disabled

The `disabled` attribute prevents interaction with the textarea.

```html
<textarea name="message" disabled>
This field is unavailable.
</textarea>
```

A disabled textarea is not included in normal form submission.

### 🖆 Readonly vs Disabled

| Feature | `readonly` | `disabled` |
| --- | --- | --- |
| User can edit | No | No |
| User can focus | Yes | No |
| Included in form submission | Yes | No |
| Participates in constraint validation | No | No |
| Typical purpose | Display non-editable submitted text | Temporarily unavailable field |

# 🗁 Rows and Columns

The `rows` and `cols` attributes define the initial visible dimensions of a textarea.

### 🖆 Using Rows

The `rows` attribute specifies how many lines are visible.

```html
<textarea rows="6"></textarea>
```

This creates a textarea approximately six text lines tall.

### 🖆 Using Columns

The `cols` attribute specifies the visible width in average character units.

```html
<textarea cols="50"></textarea>
```

This creates a textarea approximately fifty characters wide.

### 🖆 Using Both

```html
<textarea
    name="description"
    rows="6"
    cols="50"
></textarea>
```

CSS can also control the dimensions of a textarea.

```css
textarea {
    width: 100%;
    min-height: 150px;
}
```

When CSS controls the dimensions, the visual size may differ from the values specified by `rows` and `cols`.

# 🗁 Placeholder Text

The `placeholder` attribute displays a short hint when the textarea is empty.

```html
<label for="message">Message:</label>

<textarea
    id="message"
    name="message"
    placeholder="Write your message here..."
></textarea>
```

The placeholder disappears when the user enters text.

**Important:** Placeholder text is not a replacement for a proper `<label>`. It should provide an example or additional instruction rather than the only description of the field.

# 🗁 Default Text Content

The default value of a textarea is written between its opening and closing tags.

```html
<label for="bio">Biography:</label>

<textarea id="bio" name="bio">
I am a software developer interested in web technologies.
</textarea>
```

The text inside the element becomes its initial value.

Unlike `<input>`, a textarea does not use a `value` attribute to define its initial text.

Incorrect:

```html
<textarea value="Hello World"></textarea>
```

Correct:

```html
<textarea>Hello World</textarea>
```

Whitespace and line breaks in the initial content are significant, so place the text carefully.

# 🗁 Textarea and Forms

A textarea can be used inside a form to collect longer user input.

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
            rows="6"
            placeholder="Write your message..."
            required
        ></textarea>
    </div>

    <button type="submit">Send Message</button>
</form>
```

### 🖆 Understanding Form Submission

When a user submits a form, the textarea value is submitted using its `name`.

For example:

```html
<textarea name="message">Hello World</textarea>
```

The submitted form data contains:

```text
message=Hello+World
```

The actual encoding depends on the form's submission method and encoding type.

A textarea without a `name` attribute does not contribute a named value to normal form submission.

# 🗁 Textarea Validation

HTML provides built-in validation attributes for textarea controls.

### 🖆 Using Required

```html
<textarea
    name="message"
    required
></textarea>
```

The user must enter a non-empty value.

### 🖆 Using Minlength

The `minlength` attribute specifies the minimum number of characters.

```html
<textarea
    name="message"
    minlength="10"
></textarea>
```

The user must enter at least ten characters for the value to satisfy this constraint.

### 🖆 Using Maxlength

The `maxlength` attribute limits the maximum number of characters.

```html
<textarea
    name="message"
    maxlength="500"
></textarea>
```

The user cannot normally enter more than 500 characters through the browser's standard editing interface.

### 🖆 Combining Validation Attributes

```html
<form>
    <label for="feedback">Feedback:</label>

    <textarea
        id="feedback"
        name="feedback"
        rows="5"
        minlength="10"
        maxlength="500"
        required
    ></textarea>

    <button type="submit">Submit Feedback</button>
</form>
```

### 🖆 Custom Validation with JavaScript

The Constraint Validation API can be used to provide custom validation messages.

```html
<form id="messageForm">
    <label for="message">Message:</label>

    <textarea
        id="message"
        name="message"
        required
    ></textarea>

    <button type="submit">Submit</button>
</form>

<script>
    const form = document.querySelector("#messageForm");
    const message = document.querySelector("#message");

    message.addEventListener("input", () => {
        if (message.value.trim().length < 10) {
            message.setCustomValidity(
                "Message must contain at least 10 non-whitespace characters."
            );
        } else {
            message.setCustomValidity("");
        }
    });

    form.addEventListener("submit", (event) => {
        if (!form.reportValidity()) {
            event.preventDefault();
        }
    });
</script>
```

`setCustomValidity()` sets a custom validation message. Passing an empty string clears the custom error.

HTML validation improves the user experience, but the server must validate submitted data independently.

# 🗁 Working with Textarea Using JavaScript

JavaScript provides several ways to read, update, and monitor textarea values.

### 🖆 Reading the Value

Use the `value` property.

```html
<textarea id="message">Hello World</textarea>

<script>
    const message = document.querySelector("#message");

    console.log(message.value);
</script>
```

Output:

```text
Hello World
```

### 🖆 Updating the Value

```javascript
const message = document.querySelector("#message");

message.value = "Welcome to HTML!";
```

This updates the current value of the textarea.

### 🖆 Handling the Input Event

The `input` event fires whenever the user changes the value through an editing action.

```html
<textarea id="message"></textarea>

<p>Character count: <span id="count">0</span></p>

<script>
    const message = document.querySelector("#message");
    const count = document.querySelector("#count");

    message.addEventListener("input", () => {
        count.textContent = message.value.length;
    });
</script>
```

### 🖆 Character Counter with Maximum Length

```html
<label for="description">Description:</label>

<textarea
    id="description"
    maxlength="200"
></textarea>

<p>
    <span id="count">0</span>/200 characters
</p>

<script>
    const description = document.querySelector("#description");
    const count = document.querySelector("#count");

    description.addEventListener("input", () => {
        count.textContent = description.value.length;
    });
</script>
```

### 🖆 Useful Textarea Properties

| Property | Description |
| --- | --- |
| `value` | Gets or sets the current text value. |
| `defaultValue` | Gets or sets the initial/default text value. |
| `rows` | Gets or sets the visible number of rows. |
| `cols` | Gets or sets the visible number of columns. |
| `maxLength` | Gets or sets the maximum character length. |
| `minLength` | Gets or sets the minimum character length. |
| `readOnly` | Indicates whether editing is disabled. |
| `disabled` | Indicates whether the control is disabled. |
| `selectionStart` | Gets or sets the start position of the selected text. |
| `selectionEnd` | Gets or sets the end position of the selected text. |

# 🗁 Styling Textarea

CSS can be used to customize the appearance of textarea controls.

### 🖆 Basic Styling

```html
<label for="message">Your Message:</label>

<textarea
    id="message"
    name="message"
    placeholder="Write something..."
></textarea>
```

```css
textarea {
    width: 100%;
    min-height: 150px;
    padding: 12px;
    border: 1px solid #ccc;
    border-radius: 6px;
    font-family: inherit;
    font-size: 16px;
    line-height: 1.5;
    resize: vertical;
}
```

### 🖆 Resize Property

The `resize` property controls whether users can resize a textarea.

| Value | Description |
| --- | --- |
| `none` | Disables resizing. |
| `both` | Allows horizontal and vertical resizing. |
| `horizontal` | Allows horizontal resizing. |
| `vertical` | Allows vertical resizing. |

Example:

```css
textarea {
    resize: vertical;
}
```

Vertical resizing is often a useful default because it allows users to increase the writing area without breaking the page layout.

### 🖆 Focus Styling

```css
textarea:focus-visible {
    outline: 3px solid #2563eb;
    outline-offset: 2px;
    border-color: #2563eb;
}
```

Visible focus styling helps keyboard users identify the active control.

### 🖆 Preventing Horizontal Overflow

```css
textarea {
    max-width: 100%;
    box-sizing: border-box;
}
```

This helps prevent a textarea from extending beyond its containing element.

# 🗁 Accessibility Best Practices

### 🖆 Always Associate a Label

```html
<label for="comments">Comments:</label>

<textarea
    id="comments"
    name="comments"
></textarea>
```

The `for` attribute must match the textarea's `id`.

### 🖆 Provide Helpful Instructions

Use descriptive text when users need additional guidance.

```html
<label for="username">Short Biography:</label>

<p id="bio-help">
    Write a short biography using no more than 200 characters.
</p>

<textarea
    id="username"
    name="bio"
    maxlength="200"
    aria-describedby="bio-help"
></textarea>
```

### 🖆 Use Fieldset and Legend for Related Fields

```html
<fieldset>
    <legend>Feedback Details</legend>

    <label for="feedback">Your Feedback:</label>

    <textarea
        id="feedback"
        name="feedback"
        required
    ></textarea>
</fieldset>
```

### 🖆 Maintain Keyboard Accessibility

* Keep textarea controls keyboard accessible.
* Preserve visible focus indicators.
* Do not remove browser focus outlines without providing an equivalent.
* Avoid using placeholder text as the only field label.
* Provide clear validation messages.

# 🗁 Best Practices

* Use `<textarea>` for multi-line text input.
* Use `<input>` for single-line input.
* Always associate the control with a meaningful label.
* Provide a `name` attribute when the value must be submitted.
* Use `rows` to define a reasonable initial height.
* Use `maxlength` when a maximum input length is required.
* Use `minlength` when a minimum length is required.
* Use `placeholder` for hints, not as a replacement for labels.
* Use `readonly` for non-editable values that should still be submitted.
* Use `disabled` when the field should not be interactive or submitted.
* Use CSS for responsive sizing and consistent styling.
* Validate submitted data on the server.

# 🗁 Common Mistakes

### 🖆 Using the Value Attribute

Incorrect:

```html
<textarea value="Hello World"></textarea>
```

Correct:

```html
<textarea>Hello World</textarea>
```

The initial textarea content belongs between the opening and closing tags.

### 🖆 Forgetting the Name Attribute

Incorrect:

```html
<textarea id="message"></textarea>
```

Correct:

```html
<textarea id="message" name="message"></textarea>
```

Without `name`, the value is not included as a named field in normal form submission.

### 🖆 Using Placeholder Instead of Label

Incorrect:

```html
<textarea
    name="message"
    placeholder="Enter your message"
></textarea>
```

Correct:

```html
<label for="message">Message:</label>

<textarea
    id="message"
    name="message"
    placeholder="Enter your message"
></textarea>
```

### 🖆 Confusing Readonly and Disabled

Use `readonly` when users should be able to read and submit a value without editing it.

Use `disabled` when users should not interact with the field and its value should not be submitted.

### 🖆 Forgetting to Close the Element

Incorrect:

```html
<textarea>
```

Correct:

```html
<textarea></textarea>
```

The `<textarea>` element requires both opening and closing tags.

# 🗁 Practical Exercises

### 🖆 Exercise: Contact Message Form

Create a contact form containing:

* Full name
* Email address
* Message textarea
* Submit button

Requirements:

* Use appropriate labels.
* Make all fields required.
* Set the textarea to five visible rows.

### 🖆 Exercise: Feedback Form

Create a feedback form with:

* A textarea for feedback.
* A minimum length of 10 characters.
* A maximum length of 500 characters.
* A live character counter.

### 🖆 Exercise: Biography Editor

Create a biography editor.

Requirements:

* Set a maximum length of 200 characters.
* Display the current character count.
* Allow vertical resizing.
* Show a message when the maximum length is reached.

### 🖆 Exercise: Readonly Textarea

Create a textarea containing predefined information.

Requirements:

* Make it readonly.
* Allow users to select and copy its content.
* Submit its value with a form.

# 🗁 Interview Questions

**What is the purpose of the HTML `<textarea>` element?**

It creates a multi-line text input control for collecting longer text from users.

**What is the difference between `<input>` and `<textarea>`?**

`<input>` is generally used for single-line input, while `<textarea>` supports multi-line text.

**How do you define the initial value of a textarea?**

Place the initial text between the opening and closing `<textarea>` tags.

**What is the purpose of the `rows` attribute?**

It specifies the number of visible text lines.

**What is the purpose of the `cols` attribute?**

It specifies the visible width in average character units.

**What is the difference between `readonly` and `disabled`?**

Readonly fields cannot be edited but are included in normal form submission. Disabled fields cannot be interacted with and are excluded from normal form submission.

**What is the purpose of the `maxlength` attribute?**

It limits the maximum number of characters a user can enter.

**What is the difference between `placeholder` and default textarea content?**

Placeholder text is a temporary hint that disappears when the user enters text. Default content is the initial value of the textarea.

**Which JavaScript property retrieves the current textarea value?**

The `value` property.

**Which event is commonly used to detect changes while a user types?**

The `input` event.

# 🗁 References

* [MDN: HTML Textarea Element](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/textarea)
* [MDN: HTMLTextAreaElement](https://developer.mozilla.org/en-US/docs/Web/API/HTMLTextAreaElement)
* [MDN: Constraint Validation](https://developer.mozilla.org/en-US/docs/Web/HTML/Constraint_validation)
* [MDN: HTML Forms](https://developer.mozilla.org/en-US/docs/Learn_web_development/Extensions/Forms)
* [WHATWG: The Textarea Element](https://html.spec.whatwg.org/multipage/form-elements.html#the-textarea-element)
