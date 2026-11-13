# HTML Form Validation

HTML form validation ensures that users provide valid input before submitting a form.

Browsers provide built-in validation mechanisms through HTML attributes and JavaScript APIs. These mechanisms help prevent common input errors and improve the user experience.

Form validation is commonly used for:

* Registration forms
* Login forms
* Contact forms
* Payment forms
* Search forms
* Data entry applications

HTML provides native validation without requiring JavaScript for many common validation requirements.

However, client-side validation is not a replacement for server-side validation. Applications must validate submitted data on the server before processing it.

# 🗁 How HTML Form Validation Works

HTML uses constraint validation to check whether form controls satisfy defined requirements.

When a user submits a form:

* The browser checks the validation constraints of eligible form controls.
* If a control is invalid, submission is blocked.
* The browser identifies the invalid control and may display a validation message.
* If all relevant controls are valid, normal form submission proceeds.

### 🖆 Basic Example

```html
<form action="/register" method="POST">
    <label for="username">Username:</label>

    <input
        type="text"
        id="username"
        name="username"
        required
        minlength="3"
        maxlength="20"
    >

    <label for="email">Email:</label>

    <input
        type="email"
        id="email"
        name="email"
        required
    >

    <button type="submit">Register</button>
</form>
```

The browser checks that:

* Username is provided.
* Username contains at least three characters.
* Username does not exceed twenty characters.
* Email contains a syntactically valid email address.

These checks improve input quality before the data reaches the server.

# 🗁 Required Attribute

The `required` attribute makes a form control mandatory.

The user must provide a value before the form can be submitted successfully.

### 🖆 Required Input

```html
<form>
    <label for="username">Username:</label>

    <input
        type="text"
        id="username"
        name="username"
        required
    >

    <button type="submit">Submit</button>
</form>
```

### 🖆 Required Email

```html
<input
    type="email"
    name="email"
    required
>
```

### 🖆 Required Select

```html
<select name="country" required>
    <option value="">Select a country</option>
    <option value="bd">Bangladesh</option>
    <option value="us">United States</option>
</select>
```

For a required single-selection dropdown with the default display size, an empty-value first option acts as a placeholder that does not satisfy the required constraint.

### 🖆 Required Textarea

```html
<textarea
    name="message"
    required
></textarea>
```

An empty required textarea is invalid.

# 🗁 Input Type Validation

HTML input types provide built-in validation for specific data formats.

| Input Type | Validation Behavior |
| --- | --- |
| `email` | Checks for a valid email address format. |
| `url` | Checks for a valid absolute URL format. |
| `number` | Accepts numeric input and supports numeric constraints. |
| `date` | Accepts a valid date value. |
| `time` | Accepts a valid time value. |
| `datetime-local` | Accepts a local date and time value. |
| `month` | Accepts a year and month value. |
| `week` | Accepts a year and week value. |

### 🖆 Email Validation

```html
<label for="email">Email:</label>

<input
    type="email"
    id="email"
    name="email"
    required
>
```

The browser checks the email format.

This does not verify that the email address exists or belongs to the user.

### 🖆 URL Validation

```html
<label for="website">Website:</label>

<input
    type="url"
    id="website"
    name="website"
    placeholder="https://example.com"
>
```

The browser checks whether the entered value matches the expected URL format.

### 🖆 Number Validation

```html
<label for="age">Age:</label>

<input
    type="number"
    id="age"
    name="age"
    min="18"
    max="100"
    required
>
```

The browser checks that the value is numeric and falls within the specified range.

# 🗁 Minlength and Maxlength

The `minlength` and `maxlength` attributes control the allowed text length.

### 🖆 Minlength

Specifies the minimum number of characters.

```html
<input
    type="text"
    name="username"
    minlength="3"
>
```

The username must contain at least three characters to satisfy this constraint.

### 🖆 Maxlength

Specifies the maximum number of characters.

```html
<input
    type="text"
    name="username"
    maxlength="20"
>
```

The user cannot normally enter more than twenty characters through the browser's standard editing interface.

### 🖆 Combining Both

```html
<input
    type="text"
    name="username"
    minlength="3"
    maxlength="20"
    required
>
```

The username must contain between three and twenty characters.

**Important:** HTML length constraints count UTF-16 code units, which may differ from the number of user-perceived characters for some Unicode text.

# 🗁 Min and Max

The `min` and `max` attributes define the allowed minimum and maximum values for supported input types.

They are commonly used with:

* `number`
* `range`
* `date`
* `time`
* `datetime-local`
* `month`
* `week`

### 🖆 Number Example

```html
<label for="quantity">Quantity:</label>

<input
    type="number"
    id="quantity"
    name="quantity"
    min="1"
    max="10"
>
```

The accepted value must be between 1 and 10.

### 🖆 Date Example

```html
<label for="birthday">Birthday:</label>

<input
    type="date"
    id="birthday"
    name="birthday"
    min="2000-01-01"
    max="2026-12-31"
>
```

The selected date must fall within the specified range.

# 🗁 Step Attribute

The `step` attribute specifies the allowed interval between valid values.

It is commonly used with numeric and date/time-related inputs.

### 🖆 Number Example

```html
<label for="price">Price:</label>

<input
    type="number"
    id="price"
    name="price"
    min="0"
    max="100"
    step="5"
>
```

Valid values include:

```text
0
5
10
15
20
...
100
```

### 🖆 Decimal Example

```html
<input
    type="number"
    name="rating"
    min="0"
    max="5"
    step="0.5"
>
```

Valid values include:

```text
0
0.5
1
1.5
2
...
5
```

### 🖆 Any Numeric Value

Use `step="any"` when any value within the permitted range is acceptable.

```html
<input
    type="number"
    name="price"
    min="0"
    step="any"
>
```

# 🗁 Pattern Attribute

The `pattern` attribute defines a regular expression that a text-based input value must match.

It is supported by applicable text input types, including:

* `text`
* `search`
* `tel`
* `url`
* `email`
* `password`

The pattern is matched against the entire value.

### 🖆 Username Validation

```html
<label for="username">Username:</label>

<input
    type="text"
    id="username"
    name="username"
    pattern="[A-Za-z0-9_]{3,20}"
    title="Use 3 to 20 letters, numbers, or underscores."
    required
>
```

This pattern allows:

* Uppercase letters.
* Lowercase letters.
* Numbers.
* Underscores.
* A total length between 3 and 20 characters.

### 🖆 Bangladeshi Mobile Number Example

```html
<label for="phone">Phone Number:</label>

<input
    type="tel"
    id="phone"
    name="phone"
    pattern="01[3-9][0-9]{8}"
    title="Enter a valid 11-digit Bangladeshi mobile number."
    required
>
```

This pattern checks a simplified eleven-digit Bangladeshi mobile number format beginning with `013` through `019`.

### 🖆 Important Pattern Considerations

* Use a regular expression appropriate for the actual data requirements.
* The pattern must match the entire input value.
* Use `title` or another visible instruction to explain the expected format.
* Do not use restrictive patterns for international names, addresses, or other data with legitimate variations.
* A pattern checks format, not whether a value is genuine or available.

# 🗁 Multiple Attribute

The `multiple` attribute allows multiple values in supported controls.

For example, an email input can accept a comma-separated list of email addresses.

```html
<label for="emails">Email Addresses:</label>

<input
    type="email"
    id="emails"
    name="emails"
    multiple
>
```

The browser validates the email address list according to the input's format requirements.

A file input can also use `multiple` to allow selecting several files.

```html
<input
    type="file"
    name="documents"
    multiple
>
```

# 🗁 Disabled and Readonly Controls

Disabled and readonly controls behave differently during validation and form submission.

| Feature | Disabled | Readonly |
| --- | --- | --- |
| User can edit | No | No |
| Focusable through normal keyboard navigation | No | Yes |
| Included in normal form submission | No | Yes |
| Participates in constraint validation | No | No |

### 🖆 Disabled Example

```html
<input
    type="text"
    name="username"
    value="admin"
    disabled
>
```

The disabled control is excluded from normal form submission and constraint validation.

### 🖆 Readonly Example

```html
<input
    type="text"
    name="username"
    value="admin"
    readonly
>
```

The readonly value is submitted, but the control does not participate in constraint validation.

# 🗁 Novalidate Attribute

The `novalidate` attribute disables the browser's automatic interactive constraint validation when a form is submitted.

```html
<form action="/register" method="POST" novalidate>
    <label for="email">Email:</label>

    <input
        type="email"
        id="email"
        name="email"
        required
    >

    <button type="submit">Submit</button>
</form>
```

Even though the email field is required, the browser does not block submission through its normal interactive validation process.

This is useful when an application implements its own validation interface.

It does not remove validation constraints from the controls themselves. JavaScript can still inspect validity using the Constraint Validation API.

# 🗁 Formnovalidate Attribute

The `formnovalidate` attribute disables interactive constraint validation for a particular submit button.

```html
<form action="/article" method="POST">
    <label for="title">Article Title:</label>

    <input
        type="text"
        id="title"
        name="title"
        required
    >

    <button type="submit">
        Publish
    </button>

    <button
        type="submit"
        formnovalidate
        name="action"
        value="draft"
    >
        Save Draft
    </button>
</form>
```

The Publish button performs normal validation.

The Save Draft button bypasses interactive browser validation.

# 🗁 Constraint Validation API

The Constraint Validation API provides JavaScript methods and properties for checking form control validity.

### 🖆 Useful Properties

| Property | Description |
| --- | --- |
| `validity` | Returns the control's validity state. |
| `validationMessage` | Returns the browser's current validation message. |
| `willValidate` | Indicates whether the control participates in constraint validation. |

### 🖆 Useful Methods

| Method | Description |
| --- | --- |
| `checkValidity()` | Checks whether a control or form is valid. |
| `reportValidity()` | Checks validity and displays browser validation feedback. |
| `setCustomValidity()` | Sets a custom validation error message. |

### 🖆 Checking Validity

```html
<input
    type="email"
    id="email"
    required
>

<script>
    const email = document.querySelector("#email");

    console.log(email.checkValidity());
</script>
```

The method returns:

* `true` if the control is valid.
* `false` if the control is invalid.

### 🖆 Checking Form Validity

```html
<form id="registerForm">
    <input type="text" name="username" required>
    <input type="email" name="email" required>

    <button type="submit">Register</button>
</form>

<script>
    const form = document.querySelector("#registerForm");

    form.addEventListener("submit", (event) => {
        if (!form.checkValidity()) {
            event.preventDefault();

            console.log("Form contains invalid fields.");
        }
    });
</script>
```

`checkValidity()` checks the constraints and fires `invalid` events on invalid controls.

It does not automatically display the browser's validation message UI. Use `reportValidity()` when that feedback is desired.

# 🗁 Custom Validation Messages

The `setCustomValidity()` method allows developers to define custom validation messages.

### 🖆 Example

```html
<form id="passwordForm">
    <label for="password">Password:</label>

    <input
        type="password"
        id="password"
        name="password"
        required
    >

    <button type="submit">Submit</button>
</form>

<script>
    const password = document.querySelector("#password");

    password.addEventListener("input", () => {
        if (password.value.length < 8) {
            password.setCustomValidity(
                "Password must contain at least 8 characters."
            );
        } else {
            password.setCustomValidity("");
        }
    });
</script>
```

**Important:** An empty string clears the custom validation error. If a non-empty custom message remains, the control stays invalid.

### 🖆 Using Validity State

The `validity` property exposes individual validation conditions.

```javascript
const email = document.querySelector("#email");

if (email.validity.valueMissing) {
    console.log("Email is required.");
}

if (email.validity.typeMismatch) {
    console.log("Invalid email format.");
}

if (email.validity.tooShort) {
    console.log("Value is too short.");
}

if (email.validity.tooLong) {
    console.log("Value is too long.");
}

if (email.validity.patternMismatch) {
    console.log("Value does not match the required pattern.");
}
```

Common validity properties include:

| Property | Meaning |
| --- | --- |
| `valueMissing` | A required value is missing. |
| `typeMismatch` | The value does not match the expected input type. |
| `patternMismatch` | The value does not match the specified pattern. |
| `tooShort` | The value is shorter than the minimum length. |
| `tooLong` | The value exceeds the maximum length. |
| `rangeUnderflow` | The value is below the minimum. |
| `rangeOverflow` | The value exceeds the maximum. |
| `stepMismatch` | The value does not match the allowed step interval. |
| `badInput` | The browser cannot convert the user's input into the expected value. |
| `customError` | A custom validation error has been set. |
| `valid` | No constraint validation error exists. |

# 🗁 JavaScript Form Validation

JavaScript can be used to implement validation rules that go beyond built-in HTML constraints.

### 🖆 Complete Example

```html
<form id="registrationForm" novalidate>
    <div>
        <label for="username">Username:</label>

        <input
            type="text"
            id="username"
            name="username"
            required
            minlength="3"
        >

        <small id="usernameError"></small>
    </div>

    <div>
        <label for="email">Email:</label>

        <input
            type="email"
            id="email"
            name="email"
            required
        >

        <small id="emailError"></small>
    </div>

    <button type="submit">Register</button>
</form>

<script>
    const form = document.querySelector("#registrationForm");

    const username = document.querySelector("#username");
    const email = document.querySelector("#email");

    const usernameError = document.querySelector("#usernameError");
    const emailError = document.querySelector("#emailError");

    form.addEventListener("submit", (event) => {
        event.preventDefault();

        usernameError.textContent = "";
        emailError.textContent = "";

        let isValid = true;

        if (username.value.trim().length < 3) {
            usernameError.textContent =
                "Username must contain at least 3 characters.";

            isValid = false;
        }

        if (!email.validity.valid) {
            emailError.textContent =
                "Enter a valid email address.";

            isValid = false;
        }

        if (isValid) {
            console.log("Form is valid.");

            // Continue with the submission process.
        }
    });
</script>
```

This example uses `novalidate` to prevent automatic interactive browser validation and implements its own validation interface.

For production applications, also provide clear error styling, associate error messages with their controls, and move focus appropriately when validation fails.

# 🗁 Styling Validation States

CSS provides pseudo-classes for styling valid and invalid form controls.

### 🖆 Using Valid

The `:valid` pseudo-class matches controls that satisfy their validation constraints.

```css
input:valid,
textarea:valid {
    border: 2px solid green;
}
```

### 🖆 Using Invalid

The `:invalid` pseudo-class matches controls that fail their validation constraints.

```css
input:invalid,
textarea:invalid {
    border: 2px solid red;
}
```

### 🖆 Using Required

```css
input:required,
textarea:required {
    background-color: #f8fafc;
}
```

### 🖆 Using Focus

```css
input:focus-visible,
textarea:focus-visible {
    outline: 3px solid #2563eb;
    outline-offset: 2px;
}
```

### 🖆 Avoid Showing Errors Too Early

A required empty field is invalid immediately after page load. Styling every invalid field in red can make an untouched form look broken.

The `:user-invalid` pseudo-class can help show invalid styling after user interaction in browsers that support it.

```css
input:user-invalid,
textarea:user-invalid {
    border-color: red;
}
```

For consistent application behavior, JavaScript can also add an error class after the user attempts submission.

# 🗁 Accessible Validation

Validation errors should be understandable to all users, including people using assistive technologies.

### 🖆 Provide Clear Error Messages

Avoid generic messages such as:

```text
Invalid input.
```

Prefer specific messages:

```text
Email address must contain a valid email format.
```

### 🖆 Associate Error Messages with Inputs

Use `aria-describedby` to connect an input with its help or error text.

```html
<label for="email">Email:</label>

<input
    type="email"
    id="email"
    name="email"
    aria-describedby="emailError"
    required
>

<p id="emailError">
    Enter a valid email address.
</p>
```

### 🖆 Indicate Invalid Fields

The `aria-invalid` attribute can communicate that a control currently contains an invalid value.

```html
<label for="username">Username:</label>

<input
    type="text"
    id="username"
    name="username"
    aria-invalid="true"
    aria-describedby="usernameError"
>

<p id="usernameError">
    Username must contain at least 3 characters.
</p>
```

Set `aria-invalid="true"` when an actual validation error exists. Remove it or set it to `false` when the value becomes valid.

### 🖆 Display an Error Summary

For larger forms, provide a summary of validation errors near the beginning of the form.

```html
<div role="alert">
    <p>Please correct the following errors:</p>

    <ul>
        <li>Username is required.</li>
        <li>Email address is invalid.</li>
    </ul>
</div>
```

For dynamically updated errors, ensure the information is announced appropriately and that users can easily navigate to the corresponding fields.

### 🖆 Accessibility Guidelines

* Associate every input with a visible label.
* Explain expected input formats.
* Provide specific validation messages.
* Connect errors using `aria-describedby`.
* Use `aria-invalid` when a field is invalid.
* Preserve keyboard accessibility.
* Do not communicate errors through color alone.
* Move focus appropriately when a submitted form contains errors.

# 🗁 Client-Side vs Server-Side Validation

Both client-side and server-side validation are important.

| Feature | Client-Side Validation | Server-Side Validation |
| --- | --- | --- |
| Execution | Browser | Server |
| Main purpose | Immediate user feedback | Enforce data integrity and application rules |
| User experience | Faster feedback | Usually requires a request |
| Can be bypassed | Yes | Not through normal client-side manipulation |
| Security boundary | No | Yes, when correctly implemented |

### 🖆 Client-Side Validation

Example:

```html
<input
    type="email"
    name="email"
    required
>
```

The browser checks whether the field is present and has a valid email format.

### 🖆 Server-Side Validation

The server must independently verify submitted values.

For example, a backend should:

* Check required fields.
* Validate data types and formats.
* Enforce length and range limits.
* Check application-specific business rules.
* Verify authorization and ownership.
* Reject unexpected or malformed values.

Never assume that browser validation guarantees trustworthy data.

# 🗁 Best Practices

* Use native HTML validation attributes whenever they satisfy the requirement.
* Use appropriate input types such as `email`, `url`, and `number`.
* Use `required` for mandatory fields.
* Use `minlength` and `maxlength` for text-length constraints.
* Use `min`, `max`, and `step` for numeric and date-related constraints.
* Use `pattern` only when a well-defined format is required.
* Provide meaningful validation messages.
* Use JavaScript for validation rules that HTML cannot express.
* Associate errors with the corresponding controls.
* Avoid showing every validation error before the user interacts with the form.
* Always validate data on the server.
* Keep validation rules consistent between the frontend and backend where practical.

# 🗁 Common Mistakes

### 🖆 Relying Only on JavaScript Validation

Incorrect:

```javascript
if (email.value.includes("@")) {
    console.log("Valid email");
}
```

Checking only for `@` is not sufficient email validation.

Prefer the native email input type and perform proper server-side validation.

### 🖆 Forgetting Required

Incorrect:

```html
<input type="email" name="email">
```

Correct:

```html
<input type="email" name="email" required>
```

Use `required` when the field must not be empty.

### 🖆 Using an Incorrect Pattern

Incorrect:

```html
<input
    type="text"
    pattern="[0-9]"
>
```

This accepts only a single digit.

Correct:

```html
<input
    type="text"
    pattern="[0-9]{10}"
>
```

This requires exactly ten digits.

### 🖆 Forgetting to Clear Custom Validation Errors

Incorrect:

```javascript
input.setCustomValidity("Invalid value.");
```

If the message is never cleared, the field remains invalid.

Correct:

```javascript
input.addEventListener("input", () => {
    input.setCustomValidity("");
});
```

In a real application, clear the message only when the relevant validation condition is resolved.

### 🖆 Treating Client Validation as Security

Browser validation can be bypassed by disabling JavaScript, modifying HTML, or sending requests directly.

Always validate submitted data on the server.

### 🖆 Using Color as the Only Error Indicator

Incorrect:

```css
input:invalid {
    border-color: red;
}
```

A red border alone does not explain the problem.

Provide a descriptive text message and associate it with the input.

# 🗁 Practical Exercises

### 🖆 Exercise: Registration Form Validation

Create a registration form with:

* Username
* Email
* Password
* Confirm Password

Requirements:

* All fields must be required.
* Username must contain at least three characters.
* Email must use the email input type.
* Password must contain at least eight characters.
* Confirm Password must match Password using JavaScript.

### 🖆 Exercise: Number Range Validation

Create a form that accepts a user's age.

Requirements:

* Minimum age: 18.
* Maximum age: 100.
* Field must be required.
* Display a meaningful validation message.

### 🖆 Exercise: Phone Number Validation

Create a phone number field.

Requirements:

* Use `type="tel"`.
* Accept an eleven-digit Bangladeshi mobile number format.
* Use the `pattern` attribute.
* Provide a helpful format instruction.

### 🖆 Exercise: Custom Validation

Create a feedback form.

Requirements:

* Feedback must contain at least ten non-whitespace characters.
* Use `setCustomValidity()`.
* Clear the custom error when the input becomes valid.
* Display browser validation feedback.

### 🖆 Exercise: Accessible Validation

Create a form with multiple required fields.

Requirements:

* Display an error message beside each invalid field.
* Use `aria-describedby`.
* Update `aria-invalid` dynamically.
* Provide an error summary after unsuccessful submission.

# 🗁 Interview Questions

**What is HTML form validation?**

It is the process of checking user input against defined constraints before form submission.

**What is the purpose of the `required` attribute?**

It makes a form control mandatory.

**What is the difference between `minlength` and `maxlength`?**

`minlength` defines the minimum allowed text length, while `maxlength` defines the maximum allowed text length.

**What is the purpose of the `pattern` attribute?**

It specifies a regular expression that the input value must match.

**What is the difference between `min` and `max`?**

`min` defines the minimum permitted value, while `max` defines the maximum permitted value.

**What is the purpose of the `step` attribute?**

It specifies the permitted interval between valid values.

**What is the difference between `novalidate` and `formnovalidate`?**

`novalidate` disables interactive browser validation for an entire form. `formnovalidate` disables it for a particular submit button.

**What is the Constraint Validation API?**

It is a browser API that provides methods and properties for checking and controlling HTML form validation.

**What does `checkValidity()` do?**

It checks whether a control or form satisfies its validation constraints and returns a Boolean result.

**What does `reportValidity()` do?**

It checks validity and displays browser validation feedback for invalid controls.

**What is the purpose of `setCustomValidity()`?**

It sets a custom validation error message for a form control.

**What is the difference between client-side and server-side validation?**

Client-side validation provides immediate feedback in the browser. Server-side validation independently verifies submitted data before the application processes it.

**Can client-side validation be bypassed?**

Yes. It can be bypassed or manipulated, which is why server-side validation is essential.

# 🗁 References

* [MDN: Client-Side Form Validation](https://developer.mozilla.org/en-US/docs/Learn_web_development/Extensions/Forms/Form_validation)
* [MDN: Constraint Validation API](https://developer.mozilla.org/en-US/docs/Web/API/Constraint_validation)
* [MDN: ValidityState](https://developer.mozilla.org/en-US/docs/Web/API/ValidityState)
* [MDN: HTML Input Element](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/input)
* [MDN: HTML Form Element](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/form)
* [WHATWG: Forms](https://html.spec.whatwg.org/multipage/forms.html#forms)
