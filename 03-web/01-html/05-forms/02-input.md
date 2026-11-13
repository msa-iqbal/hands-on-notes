# HTML Input Element (`<input>`)

The HTML `<input>` element is used to collect data from users through interactive form controls.

It is one of the most commonly used elements in HTML forms.

The `<input>` element supports different types of controls depending on its `type` attribute, including:

* Text fields.
* Password fields.
* Email fields.
* Number fields.
* Checkboxes.
* Radio buttons.
* Date and time pickers.
* File uploads.
* Search fields.
* Submit and reset buttons.

The `<input>` element is a void element, meaning it does not have a closing tag.

# 🗁 Introduction

The `<input>` element creates an interactive control that allows users to enter or select information.

Its behavior is determined primarily by the `type` attribute.

Example:

```html
<input type="text" name="username">
```

This creates a single-line text input.

Changing the type changes the control's behavior.

```html
<input type="email" name="email">

<input type="password" name="password">

<input type="checkbox" name="subscribe">

<input type="date" name="birthday">
```

# 🗁 Basic Syntax

```html
<input
  type="text"
  id="username"
  name="username"
  placeholder="Enter your username"
>
```

### 🖆 Important Parts

| Attribute | Purpose |
| --- | --- |
| `type` | Defines the input control type |
| `id` | Provides a unique identifier |
| `name` | Defines the form submission key |
| `placeholder` | Displays a short hint inside the input |
| `value` | Defines the initial value |
| `required` | Makes the field mandatory |

The `<input>` element is a void element.

Correct:

```html
<input type="text" name="username">
```

Incorrect:

```html
<input type="text" name="username"></input>
```

# 🗁 The Type Attribute

The `type` attribute determines how an input behaves and how browsers display it.

```html
<input type="text">
```

If the `type` attribute is omitted, the default type is `text`.

HTML provides several input types for different data requirements.

# 🗁 Text Input Types

### 🖆 text

Creates a single-line text field.

```html
<label for="fullname">Full Name</label>

<input
  type="text"
  id="fullname"
  name="fullname"
  placeholder="Enter your full name"
>
```

Common uses:

* Names.
* Usernames.
* Titles.
* Short text values.

### 🖆 password

Creates a field that visually obscures the entered characters.

```html
<label for="password">Password</label>

<input
  type="password"
  id="password"
  name="password"
  required
>
```

Password input does not encrypt the value by itself. Secure transmission requires HTTPS.

### 🖆 email

Creates an input intended for email addresses.

```html
<label for="email">Email Address</label>

<input
  type="email"
  id="email"
  name="email"
  placeholder="user@example.com"
  required
>
```

The browser performs basic email-format validation when the form is submitted.

This does not verify that the email address exists or belongs to the user.

### 🖆 search

Creates a search-oriented text field.

```html
<label for="search">Search</label>

<input
  type="search"
  id="search"
  name="query"
  placeholder="Search tutorials..."
>
```

Browsers may provide search-specific visual features.

### 🖆 tel

Creates a field intended for telephone numbers.

```html
<label for="phone">Phone Number</label>

<input
  type="tel"
  id="phone"
  name="phone"
  placeholder="+8801XXXXXXXXX"
>
```

The `tel` type does not enforce a universal telephone-number format. Use an appropriate `pattern` or application-level validation when required.

### 🖆 url

Creates a field intended for URLs.

```html
<label for="website">Website</label>

<input
  type="url"
  id="website"
  name="website"
  placeholder="https://example.com"
>
```

The browser validates whether the entered value has an acceptable URL format.

# 🗁 Selection Input Types

### 🖆 checkbox

Creates a checkbox that allows users to select or deselect an option.

```html
<label>
  <input
    type="checkbox"
    name="newsletter"
    value="yes"
  >

  Subscribe to our newsletter
</label>
```

A checkbox can be used independently or as part of a group.

Multiple checkboxes can share the same name when users are allowed to select multiple values.

```html
<p>Select your interests:</p>

<label>
  <input type="checkbox" name="interests" value="html">
  HTML
</label>

<label>
  <input type="checkbox" name="interests" value="css">
  CSS
</label>

<label>
  <input type="checkbox" name="interests" value="javascript">
  JavaScript
</label>
```

Only checked checkboxes are included in normal form submission.

### 🖆 radio

Creates a radio button that allows users to select one option from a group.

Radio buttons belong to the same group when they share the same `name` within the relevant form ownership scope.

```html
<p>Select your gender:</p>

<label>
  <input type="radio" name="gender" value="male">
  Male
</label>

<label>
  <input type="radio" name="gender" value="female">
  Female
</label>

<label>
  <input type="radio" name="gender" value="other">
  Other
</label>
```

Only one radio button in the same group can be selected at a time.

### 🖆 Using Fieldset and Legend

Use `<fieldset>` and `<legend>` to group related radio buttons or checkboxes.

```html
<fieldset>
  <legend>Select your preferred language</legend>

  <label>
    <input type="radio" name="language" value="en">
    English
  </label>

  <label>
    <input type="radio" name="language" value="bn">
    Bengali
  </label>
</fieldset>
```

# 🗁 Date and Time Input Types

HTML provides several date and time-related input types.

### 🖆 date

Allows users to select a calendar date.

```html
<label for="birthday">Date of Birth</label>

<input
  type="date"
  id="birthday"
  name="birthday"
>
```

The submitted value uses the format:

```text
YYYY-MM-DD
```

### 🖆 time

Allows users to select a time.

```html
<label for="meeting">Meeting Time</label>

<input
  type="time"
  id="meeting"
  name="meeting"
>
```

The typical value format is:

```text
HH:MM
```

### 🖆 datetime-local

Allows users to select a local date and time without specifying a timezone.

```html
<label for="appointment">Appointment</label>

<input
  type="datetime-local"
  id="appointment"
  name="appointment"
>
```

### 🖆 month

Allows users to select a month and year.

```html
<label for="billingMonth">Billing Month</label>

<input
  type="month"
  id="billingMonth"
  name="billingMonth"
>
```

Typical value:

```text
2026-11
```

### 🖆 week

Allows users to select a week and year.

```html
<label for="week">Select a Week</label>

<input
  type="week"
  id="week"
  name="week"
>
```

### 🖆 Date and Time Constraints

The `min` and `max` attributes restrict acceptable values.

```html
<label for="appointment">Appointment Date</label>

<input
  type="date"
  id="appointment"
  name="appointment"
  min="2026-01-01"
  max="2026-12-31"
>
```

# 🗁 Numeric Input Types

### 🖆 number

Creates an input for numeric values.

```html
<label for="age">Age</label>

<input
  type="number"
  id="age"
  name="age"
  min="1"
  max="100"
  step="1"
>
```

Important attributes:

| Attribute | Purpose |
| --- | --- |
| `min` | Minimum permitted value |
| `max` | Maximum permitted value |
| `step` | Valid increment |
| `value` | Initial value |

Example with decimal values:

```html
<label for="price">Price</label>

<input
  type="number"
  id="price"
  name="price"
  min="0"
  step="0.01"
>
```

### 🖆 range

Creates a slider for selecting a numeric value within a range.

```html
<label for="volume">Volume</label>

<input
  type="range"
  id="volume"
  name="volume"
  min="0"
  max="100"
  value="50"
>
```

A range input is useful when approximate selection is more important than entering an exact number.

# 🗁 File Input

The `file` input allows users to select files from their device.

```html
<label for="document">Upload Document</label>

<input
  type="file"
  id="document"
  name="document"
>
```

### 🖆 Multiple File Selection

Use the `multiple` attribute to allow selecting more than one file.

```html
<input
  type="file"
  name="documents"
  multiple
>
```

### 🖆 Restricting File Types

Use the `accept` attribute to suggest which file types the browser should allow users to select.

```html
<input
  type="file"
  name="image"
  accept="image/*"
>
```

Specific file types can also be specified.

```html
<input
  type="file"
  name="document"
  accept=".pdf,.doc,.docx"
>
```

The `accept` attribute is not a security mechanism. The server must validate uploaded file types and contents.

### 🖆 File Upload Form

```html
<form
  action="/upload"
  method="post"
  enctype="multipart/form-data"
>
  <label for="profileImage">Profile Image</label>

  <input
    type="file"
    id="profileImage"
    name="profileImage"
    accept="image/*"
    required
  >

  <button type="submit">Upload</button>
</form>
```

# 🗁 Button Input Types

The `<input>` element also supports several button-related types.

### 🖆 submit

Creates a button that submits the associated form.

```html
<input type="submit" value="Submit Form">
```

### 🖆 reset

Creates a button that resets form controls to their initial values.

```html
<input type="reset" value="Reset Form">
```

### 🖆 button

Creates a generic button without a built-in form submission action.

```html
<input type="button" value="Click Me">
```

It is commonly used with JavaScript.

### 🖆 image

Creates an image-based submit button.

```html
<input
  type="image"
  src="/images/submit.png"
  alt="Submit form"
>
```

When activated, it submits the form and includes the click coordinates in the submitted data.

For most modern interfaces, a regular `<button>` element offers more flexibility.

# 🗁 Hidden Input

The `hidden` type creates an input that is not displayed to the user but can still submit a value.

```html
<input
  type="hidden"
  name="formVersion"
  value="v1"
>
```

Hidden inputs are useful for values such as:

* Form identifiers.
* Record identifiers.
* Workflow metadata.
* CSRF tokens.

Hidden values are not secret or trusted. Users can inspect and modify them, so the server must validate them.

# 🗁 Color Input

The `color` type provides a browser color picker.

```html
<label for="themeColor">Choose a Theme Color</label>

<input
  type="color"
  id="themeColor"
  name="themeColor"
  value="#3366ff"
>
```

The submitted value is generally a hexadecimal color value.

# 🗁 Common Input Attributes

The `<input>` element supports many attributes that control its behavior.

| Attribute | Description |
| --- | --- |
| `type` | Defines the input type |
| `id` | Provides a unique identifier |
| `name` | Defines the submitted field name |
| `value` | Defines the current or initial value |
| `placeholder` | Displays an input hint |
| `required` | Makes the field mandatory |
| `readonly` | Prevents user editing |
| `disabled` | Disables the input |
| `autocomplete` | Controls autocomplete behavior |
| `autofocus` | Requests automatic focus when the page loads |
| `checked` | Initially checks a checkbox or radio button |
| `multiple` | Allows multiple values where supported |
| `min` | Defines the minimum value |
| `max` | Defines the maximum value |
| `step` | Defines the permitted increment |
| `minlength` | Defines minimum text length |
| `maxlength` | Defines maximum text length |
| `pattern` | Defines a regular-expression constraint |
| `accept` | Suggests acceptable file types |
| `form` | Associates an input with a form by ID |
| `list` | Associates an input with a datalist |

### 🖆 value

Defines the input's initial value.

```html
<input
  type="text"
  name="username"
  value="John"
>
```

### 🖆 placeholder

Displays a short hint inside the input.

```html
<input
  type="text"
  name="username"
  placeholder="Enter username"
>
```

A placeholder should not replace a visible label.

### 🖆 readonly

Prevents the user from editing the input value.

```html
<input
  type="text"
  name="username"
  value="john"
  readonly
>
```

A readonly input is generally focusable and its value is submitted with the form if it is otherwise eligible.

### 🖆 disabled

Disables the input.

```html
<input
  type="text"
  name="username"
  value="john"
  disabled
>
```

Disabled inputs cannot normally receive focus or user interaction, and they are not included in form submission.

### 🖆 checked

Initially selects a checkbox or radio button.

```html
<input
  type="checkbox"
  name="subscribe"
  value="yes"
  checked
>
```

### 🖆 autofocus

Requests that the browser automatically focus an input when the page loads.

```html
<input
  type="search"
  name="query"
  autofocus
>
```

Use autofocus carefully because automatically moving focus can disrupt users, especially those using assistive technologies.

# 🗁 Input Validation Attributes

HTML provides built-in constraint validation for many input types.

### 🖆 required

Makes a field mandatory.

```html
<input type="email" name="email" required>
```

### 🖆 minlength and maxlength

Control the permitted text length.

```html
<input
  type="text"
  name="username"
  minlength="3"
  maxlength="20"
>
```

### 🖆 min and max

Restrict numeric or date-related values.

```html
<input
  type="number"
  name="age"
  min="18"
  max="60"
>
```

### 🖆 step

Defines valid increments for numeric inputs.

```html
<input
  type="number"
  name="quantity"
  min="0"
  max="100"
  step="5"
>
```

Valid values include 0, 5, 10, 15, and so on.

### 🖆 pattern

Defines a regular-expression constraint for applicable text-like input types.

```html
<label for="username">Username</label>

<input
  type="text"
  id="username"
  name="username"
  pattern="[A-Za-z0-9_]{3,20}"
  title="Use 3 to 20 letters, numbers, or underscores."
  required
>
```

The browser checks whether the entire input value matches the pattern.

Client-side constraints improve usability but must not replace server-side validation.

# 🗁 Input Accessibility

Accessible input controls make forms easier to use for everyone, including people using screen readers and keyboard navigation.

### 🖆 Use Labels

Every input should have an accessible name.

```html
<label for="email">Email Address</label>

<input
  type="email"
  id="email"
  name="email"
>
```

### 🖆 Group Related Controls

Use `<fieldset>` and `<legend>` for related controls.

```html
<fieldset>
  <legend>Notification Preferences</legend>

  <label>
    <input type="checkbox" name="notifications" value="email">
    Email notifications
  </label>

  <label>
    <input type="checkbox" name="notifications" value="sms">
    SMS notifications
  </label>
</fieldset>
```

### 🖆 Provide Instructions

If an input has specific requirements, explain them clearly.

```html
<label for="password">Password</label>

<p id="passwordHelp">
  Use at least 8 characters.
</p>

<input
  type="password"
  id="password"
  name="password"
  minlength="8"
  aria-describedby="passwordHelp"
  required
>
```

### 🖆 Preserve Keyboard Accessibility

Native input elements already support standard keyboard interaction.

Avoid replacing them with custom controls unless there is a clear requirement.

# 🗁 Working with JavaScript

JavaScript can read, update, and respond to input values.

### 🖆 Accessing Input Values

```html
<input
  type="text"
  id="username"
  placeholder="Enter username"
>

<button id="showButton" type="button">
  Show Value
</button>

<script>
  const usernameInput = document.querySelector("#username");
  const showButton = document.querySelector("#showButton");

  showButton.addEventListener("click", () => {
    console.log(usernameInput.value);
  });
</script>
```

### 🖆 Listening for Input Changes

The `input` event fires when the user changes an input's value.

```html
<input type="text" id="username">

<p id="output"></p>

<script>
  const input = document.querySelector("#username");
  const output = document.querySelector("#output");

  input.addEventListener("input", () => {
    output.textContent = input.value;
  });
</script>
```

### 🖆 Checking a Checkbox

```html
<input type="checkbox" id="subscribe">

<button id="checkButton" type="button">
  Check Subscription
</button>

<script>
  const checkbox = document.querySelector("#subscribe");
  const button = document.querySelector("#checkButton");

  button.addEventListener("click", () => {
    console.log(checkbox.checked);
  });
</script>
```

The `checked` property returns a Boolean value.

### 🖆 Reading Multiple Selected Values

```html
<form id="interestForm">
  <label>
    <input type="checkbox" name="interests" value="html">
    HTML
  </label>

  <label>
    <input type="checkbox" name="interests" value="css">
    CSS
  </label>

  <label>
    <input type="checkbox" name="interests" value="javascript">
    JavaScript
  </label>

  <button type="submit">Submit</button>
</form>

<script>
  const form = document.querySelector("#interestForm");

  form.addEventListener("submit", (event) => {
    event.preventDefault();

    const formData = new FormData(form);

    console.log(formData.getAll("interests"));
  });
</script>
```

# 🗁 Complete Form Example

The following example combines several input types and attributes.

```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">

  <title>Registration Form</title>
</head>
<body>

  <main>
    <h1>Registration Form</h1>

    <form action="/register" method="post">

      <div>
        <label for="fullname">Full Name</label>

        <input
          type="text"
          id="fullname"
          name="fullname"
          autocomplete="name"
          required
        >
      </div>

      <div>
        <label for="email">Email</label>

        <input
          type="email"
          id="email"
          name="email"
          autocomplete="email"
          required
        >
      </div>

      <div>
        <label for="password">Password</label>

        <input
          type="password"
          id="password"
          name="password"
          minlength="8"
          autocomplete="new-password"
          required
        >
      </div>

      <div>
        <label for="age">Age</label>

        <input
          type="number"
          id="age"
          name="age"
          min="18"
          max="100"
        >
      </div>

      <fieldset>
        <legend>Preferred Contact Method</legend>

        <label>
          <input
            type="radio"
            name="contactMethod"
            value="email"
            required
          >
          Email
        </label>

        <label>
          <input
            type="radio"
            name="contactMethod"
            value="phone"
          >
          Phone
        </label>
      </fieldset>

      <div>
        <label for="profileImage">Profile Image</label>

        <input
          type="file"
          id="profileImage"
          name="profileImage"
          accept="image/*"
        >
      </div>

      <label>
        <input
          type="checkbox"
          name="terms"
          value="accepted"
          required
        >
        I agree to the terms and conditions.
      </label>

      <button type="submit">Register</button>

    </form>
  </main>

</body>
</html>
```

Because this form includes a file input, a production implementation must use `enctype="multipart/form-data"` on the form.

# 🗁 Best Practices

* Select the most appropriate input type for each field.
* Use meaningful `name` attributes for submitted values.
* Associate inputs with visible labels.
* Use `required` only for genuinely mandatory fields.
* Use `min`, `max`, `step`, and `pattern` where appropriate.
* Use `autocomplete` values to improve form usability.
* Use `readonly` and `disabled` according to their different behaviors.
* Use checkboxes for independent selections.
* Use radio buttons for mutually exclusive choices.
* Use `<fieldset>` and `<legend>` to group related controls.
* Use `accept` to guide file selection, not to enforce file security.
* Validate all submitted values on the server.
* Avoid excessive use of autofocus.
* Provide clear instructions and accessible validation feedback.

# 🗁 Common Mistakes

### 🖆 Missing the Name Attribute

Incorrect:

```html
<input type="text" id="username">
```

Correct:

```html
<input
  type="text"
  id="username"
  name="username"
>
```

Without a `name`, the input's value is generally not submitted with the form.

### 🖆 Using Placeholder Instead of Label

Incorrect:

```html
<input
  type="email"
  name="email"
  placeholder="Email Address"
>
```

Correct:

```html
<label for="email">Email Address</label>

<input
  type="email"
  id="email"
  name="email"
  placeholder="user@example.com"
>
```

### 🖆 Using Checkbox Instead of Radio Buttons

Use checkboxes when multiple independent selections are allowed.

Use radio buttons when only one option should be selected from a group.

### 🖆 Assuming Disabled Values Are Submitted

Disabled inputs are excluded from normal form submission.

Use `readonly` when users should not edit a value but the value should remain eligible for submission.

### 🖆 Trusting Client-Side Validation

HTML validation can be bypassed. Always validate submitted values on the server.

### 🖆 Forgetting Multipart Encoding for File Uploads

File upload forms require:

```html
<form method="post" enctype="multipart/form-data">
```

# 🗁 Practical Exercises

* Create a registration form using text, email, password, and number inputs.
* Build a login form with appropriate autocomplete attributes.
* Create a checkbox group for selecting multiple programming languages.
* Create a radio group for selecting one payment method.
* Build a date-of-birth field with appropriate minimum and maximum values.
* Create a file upload field that accepts images.
* Add validation using `required`, `minlength`, `maxlength`, and `pattern`.
* Use JavaScript to display input values as the user types.
* Use `FormData.getAll()` to retrieve multiple checkbox values.

# 🗁 Interview Questions

**What is the purpose of the `<input>` element?**

It creates interactive form controls for collecting user input.

**Is `<input>` a void element?**

Yes. It does not require a closing tag.

**What is the default input type?**

`text`.

**What is the difference between `readonly` and `disabled`?**

Readonly inputs cannot be edited but are generally focusable and submitted. Disabled inputs cannot normally be interacted with and are excluded from form submission.

**What is the difference between checkbox and radio inputs?**

Checkboxes allow independent selections, while radio buttons allow one selection within a group.

**What is the purpose of the `name` attribute?**

It defines the key used to submit the input's value.

**What is the purpose of the `required` attribute?**

It makes an input mandatory during native form validation.

**What is the difference between `type="number"` and `type="range"`?**

Number inputs allow direct numeric entry, while range inputs provide a slider for selecting a value.

**Does `type="email"` verify that an email address exists?**

No. It performs basic format validation only.

**What is the purpose of the `accept` attribute?**

It suggests acceptable file types for a file input.

**What is the difference between `value` and `placeholder`?**

`value` represents the input's actual value, while `placeholder` provides a temporary hint when the input is empty.

**How can JavaScript detect input changes?**

By listening for the `input` event.

# 🗁 References

* [MDN — `<input>` Element](https://developer.mozilla.org/en-US/docs/Web/HTML/Reference/Elements/input)
* [MDN — HTML Input Types](https://developer.mozilla.org/en-US/docs/Web/HTML/Reference/Elements/input#input_types)
* [MDN — HTML Form Validation](https://developer.mozilla.org/en-US/docs/Learn_web_development/Extensions/Forms/Form_validation)
* [WHATWG — The Input Element](https://html.spec.whatwg.org/multipage/input.html)
* [W3C WAI — Forms Tutorial](https://www.w3.org/WAI/tutorials/forms/)
