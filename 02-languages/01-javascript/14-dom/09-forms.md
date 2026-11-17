# DOM Forms

HTML forms provide a standard way to collect user input and submit data to a server or process it with JavaScript.

JavaScript interacts with forms through the DOM to:

* Read input values.
* Validate user input.
* Handle form submission.
* Display validation messages.
* Update form controls.
* Prevent default submission behavior.
* Collect and serialize form data.

Forms are essential for authentication, registration, search, checkout, contact pages, and other interactive web applications.

# 🗁 Understanding HTML Forms

An HTML form is created using the `<form>` element.

```html
<form id="loginForm">
  <label for="email">Email</label>
  <input type="email" id="email" name="email">

  <label for="password">Password</label>
  <input type="password" id="password" name="password">

  <button type="submit">Login</button>
</form>
```

Important form attributes:

| Attribute | Description |
| --- | --- |
| `action` | URL where form data is submitted |
| `method` | HTTP method used for submission |
| `enctype` | Encoding type for submitted data |
| `target` | Browsing context for the response |
| `novalidate` | Disables native validation during submission |
| `autocomplete` | Controls browser autofill behavior |

# 🗁 Selecting Forms

Forms can be selected using standard DOM methods.

```javascript
const form = document.querySelector("#loginForm");
```

Other approaches:

```javascript
const forms = document.forms;

const firstForm = document.forms[0];

const loginForm = document.forms["loginForm"];
```

The `document.forms` collection provides access to forms in the document.

# 🗁 Accessing Form Elements

The `elements` property provides access to the controls associated with a form.

```javascript
const form = document.querySelector("#loginForm");

console.log(form.elements);
```

Access a control by its name:

```javascript
const email = form.elements.email;
const password = form.elements.password;
```

You can also access controls by index:

```javascript
const firstInput = form.elements[0];
```

Using meaningful control names is generally clearer than relying on indexes.

# 🗁 Reading Input Values

The `value` property retrieves the current value of an input control.

```html
<input type="text" id="username" name="username">

<script>
  const username = document.querySelector("#username");

  console.log(username.value);
</script>
```

To update the value:

```javascript
username.value = "John";
```

Different controls expose values in different ways, but most standard form controls provide a `value` property.

# 🗁 Handling Form Submission

The `submit` event fires when a form is submitted through a user action or certain programmatic submission methods.

```javascript
const form = document.querySelector("#loginForm");

form.addEventListener("submit", (event) => {
  event.preventDefault();

  console.log("Form submitted");
});
```

The `preventDefault()` method prevents the browser's normal form submission behavior.

This allows JavaScript to process the data instead.

# 🗁 Submit Buttons

A form can contain different button types.

```html
<form id="profileForm">
  <input type="text" name="username">

  <button type="submit">Save</button>
  <button type="reset">Reset</button>
  <button type="button">Custom Action</button>
</form>
```

| Button type | Behavior |
| --- | --- |
| `submit` | Submits the form |
| `reset` | Resets form controls to their initial values |
| `button` | Does not perform a form action by default |

Always specify the button type explicitly when its behavior matters.

# 🗁 Reading Form Data with FormData

The `FormData` API provides a convenient way to collect data from form controls.

```javascript
const form = document.querySelector("#loginForm");

form.addEventListener("submit", (event) => {
  event.preventDefault();

  const formData = new FormData(form);

  console.log(formData.get("email"));
  console.log(formData.get("password"));
});
```

The `FormData` constructor collects successful form controls associated with the form.

A control generally needs a `name` attribute to be included.

Disabled controls and unchecked checkboxes or radio buttons are not included.

# 🗁 FormData Methods

Important methods include:

| Method | Description |
| --- | --- |
| `get(name)` | Returns the first value for a name |
| `getAll(name)` | Returns all values associated with a name |
| `set(name, value)` | Sets a value, replacing existing values |
| `append(name, value)` | Adds another value |
| `delete(name)` | Removes values for a name |
| `has(name)` | Checks whether a name exists |
| `entries()` | Returns key-value pairs |
| `keys()` | Returns field names |
| `values()` | Returns field values |

Example:

```javascript
const formData = new FormData(form);

console.log(formData.get("email"));

formData.set("username", "john");

formData.append("role", "developer");

console.log(formData.has("email"));

for (const [key, value] of formData.entries()) {
  console.log(key, value);
}
```

# 🗁 Converting FormData into an Object

`FormData` can be converted into a plain JavaScript object.

```javascript
const formData = new FormData(form);

const data = Object.fromEntries(formData.entries());

console.log(data);
```

Example output:

```javascript
{
  email: "john@example.com",
  password: "secret123"
}
```

Important: `Object.fromEntries()` keeps only one value for each key. If a form contains multiple controls with the same name, use `getAll()` or another collection strategy to preserve all values.

For example:

```html
<form id="skillsForm">
  <input type="checkbox" name="skills" value="JavaScript">
  <input type="checkbox" name="skills" value="TypeScript">
  <input type="checkbox" name="skills" value="Python">
</form>
```

Retrieve all selected values:

```javascript
const formData = new FormData(
  document.querySelector("#skillsForm")
);

const skills = formData.getAll("skills");

console.log(skills);
```

# 🗁 Handling Input Events

The `input` event fires when a user changes the value of an input or textarea.

```html
<input type="text" id="username">

<script>
  const input = document.querySelector("#username");

  input.addEventListener("input", (event) => {
    console.log(event.target.value);
  });
</script>
```

It is useful for:

* Live search.
* Character counters.
* Real-time validation.
* Dynamic previews.
* Updating UI based on user input.

# 🗁 Handling Change Events

The `change` event fires when a control's value is committed.

Its timing depends on the control type.

```html
<select id="country">
  <option value="bd">Bangladesh</option>
  <option value="us">United States</option>
  <option value="uk">United Kingdom</option>
</select>

<script>
  const country = document.querySelector("#country");

  country.addEventListener("change", (event) => {
    console.log("Selected:", event.target.value);
  });
</script>
```

For text inputs, `change` generally fires when the edited value is committed, commonly when focus leaves the control.

# 🗁 Handling Checkbox Values

Checkboxes expose their checked state through the `checked` property.

```html
<label>
  <input type="checkbox" id="terms">
  Accept terms and conditions
</label>

<script>
  const checkbox = document.querySelector("#terms");

  checkbox.addEventListener("change", () => {
    console.log(checkbox.checked);
  });
</script>
```

Read the current state:

```javascript
if (checkbox.checked) {
  console.log("Accepted");
}
```

# 🗁 Handling Radio Buttons

Radio buttons allow users to select one option from a group sharing the same `name`.

```html
<form id="genderForm">
  <label>
    <input type="radio" name="gender" value="male">
    Male
  </label>

  <label>
    <input type="radio" name="gender" value="female">
    Female
  </label>
</form>
```

Retrieve the selected value:

```javascript
const form = document.querySelector("#genderForm");

const selected = form.querySelector(
  'input[name="gender"]:checked'
);

console.log(selected?.value);
```

The optional chaining operator prevents an error when no option is selected.

# 🗁 Resetting Forms

The `reset()` method restores form controls to their default values.

```javascript
const form = document.querySelector("#loginForm");

form.reset();
```

You can also listen for the reset event:

```javascript
form.addEventListener("reset", () => {
  console.log("Form reset");
});
```

The reset event occurs before the controls have finished returning to their default values.

# 🗁 Native HTML Form Validation

HTML provides built-in form validation through input types and attributes.

Common validation attributes:

| Attribute | Description |
| --- | --- |
| `required` | Makes a field mandatory |
| `minlength` | Specifies minimum text length |
| `maxlength` | Specifies maximum text length |
| `min` | Specifies minimum numeric or date value |
| `max` | Specifies maximum numeric or date value |
| `step` | Specifies permitted value increments |
| `pattern` | Specifies a regular expression constraint |
| `type` | Defines the input type and its associated constraints |

Example:

```html
<form id="registrationForm">
  <input
    type="text"
    name="username"
    required
    minlength="3"
    maxlength="20"
  >

  <input
    type="email"
    name="email"
    required
  >

  <input
    type="number"
    name="age"
    min="18"
    max="100"
  >

  <button type="submit">Register</button>
</form>
```

The browser automatically validates these constraints during normal form submission.

# 🗁 Constraint Validation API

JavaScript can inspect and control native form validation using the Constraint Validation API.

Important properties and methods:

| Property or method | Description |
| --- | --- |
| `validity` | Returns the control's validity state |
| `validationMessage` | Returns the browser's validation message |
| `willValidate` | Indicates whether the control participates in validation |
| `checkValidity()` | Checks validity and fires an `invalid` event when invalid |
| `reportValidity()` | Checks validity and displays browser validation feedback |
| `setCustomValidity(message)` | Sets a custom validation error |
| `valid` | Indicates whether the control satisfies its constraints |

Example:

```javascript
const email = document.querySelector("#email");

console.log(email.validity.valid);

if (!email.checkValidity()) {
  console.log(email.validationMessage);
}
```

# 🗁 Custom Form Validation

The `setCustomValidity()` method allows JavaScript to define custom validation rules.

An empty string clears the custom error.

```html
<input type="password" id="password" required>
<button id="validate">Validate</button>

<script>
  const password = document.querySelector("#password");

  document.querySelector("#validate").addEventListener("click", () => {
    if (password.value.length < 8) {
      password.setCustomValidity(
        "Password must contain at least 8 characters."
      );
    } else {
      password.setCustomValidity("");
    }

    password.reportValidity();
  });
</script>
```

The browser considers the control invalid while a non-empty custom validity message is set.

# 🗁 Handling Validation Events

The `invalid` event fires when a form control fails constraint validation.

```javascript
const form = document.querySelector("#registrationForm");

form.addEventListener(
  "invalid",
  (event) => {
    console.log("Invalid field:", event.target.name);
  },
  true
);
```

The `invalid` event does not bubble, so the capturing phase is used to observe invalid controls through a parent form.

# 🗁 Disabling Native Validation

The `novalidate` attribute disables automatic constraint validation when the form is submitted.

```html
<form id="registrationForm" novalidate>
  <input type="email" name="email" required>
  <button type="submit">Register</button>
</form>
```

JavaScript can then control the validation flow.

```javascript
form.addEventListener("submit", (event) => {
  event.preventDefault();

  if (!form.checkValidity()) {
    form.reportValidity();
    return;
  }

  console.log("Form is valid");
});
```

The `novalidate` attribute does not remove validation constraints. Methods such as `checkValidity()` and `reportValidity()` can still be used.

# 🗁 Form Submission with Fetch API

JavaScript can submit form data to a server using the Fetch API.

```javascript
const form = document.querySelector("#registrationForm");

form.addEventListener("submit", async (event) => {
  event.preventDefault();

  if (!form.reportValidity()) {
    return;
  }

  const formData = new FormData(form);

  try {
    const response = await fetch("/api/register", {
      method: "POST",
      body: formData
    });

    if (!response.ok) {
      throw new Error(`Request failed: ${response.status}`);
    }

    const result = await response.json();

    console.log("Registration successful:", result);
  } catch (error) {
    console.error("Registration failed:", error);
  }
});
```

When sending `FormData` through Fetch, do not manually set the `Content-Type` header to `multipart/form-data`. The browser must generate the header with the appropriate boundary.

For JSON APIs, convert the data into an object and serialize it.

```javascript
const formData = new FormData(form);

const data = Object.fromEntries(formData.entries());

const response = await fetch("/api/register", {
  method: "POST",
  headers: {
    "Content-Type": "application/json"
  },
  body: JSON.stringify(data)
});
```

# 🗁 File Inputs

File inputs allow users to select files from their device.

```html
<input type="file" id="fileInput" multiple>

<script>
  const fileInput = document.querySelector("#fileInput");

  fileInput.addEventListener("change", () => {
    const files = fileInput.files;

    for (const file of files) {
      console.log(file.name);
      console.log(file.size);
      console.log(file.type);
    }
  });
</script>
```

Important properties:

| Property | Description |
| --- | --- |
| `files` | Returns a FileList |
| `name` | File name |
| `size` | File size in bytes |
| `type` | MIME type, when available |
| `lastModified` | Last modification timestamp |

A selected file can also be included in `FormData` and uploaded through Fetch.

# 🗁 FormData and File Upload

Example:

```html
<form id="uploadForm">
  <input type="file" name="document" required>
  <button type="submit">Upload</button>
</form>
```

```javascript
const uploadForm = document.querySelector("#uploadForm");

uploadForm.addEventListener("submit", async (event) => {
  event.preventDefault();

  if (!uploadForm.reportValidity()) {
    return;
  }

  const formData = new FormData(uploadForm);

  try {
    const response = await fetch("/api/upload", {
      method: "POST",
      body: formData
    });

    if (!response.ok) {
      throw new Error("Upload failed");
    }

    console.log("Upload successful");
  } catch (error) {
    console.error(error);
  }
});
```

# 🗁 Form Submission Methods

Forms can be submitted programmatically using two important methods.

**submit()**

```javascript
form.submit();
```

This submits the form directly without running the normal interactive constraint validation and without firing the `submit` event.

**requestSubmit()**

```javascript
form.requestSubmit();
```

This behaves like activating a submit button: it performs constraint validation and dispatches the `submit` event if validation succeeds.

To submit through a specific submit button:

```javascript
const submitButton = form.querySelector(
  'button[type="submit"]'
);

form.requestSubmit(submitButton);
```

Use `requestSubmit()` when programmatic submission should follow the normal form submission process.

# 🗁 Best Practices

* Give form controls meaningful `name` attributes.
* Use appropriate input types such as `email`, `number`, and `date`.
* Use native HTML validation whenever it meets the requirements.
* Use JavaScript validation for additional application-specific rules.
* Handle submission through the form's `submit` event.
* Prevent default submission only when JavaScript is taking control.
* Use `FormData` for collecting form values and uploading files.
* Use `textContent` instead of `innerHTML` when displaying user-provided values.
* Provide clear validation feedback.
* Preserve keyboard accessibility and associate labels with their controls.
* Disable duplicate submissions when necessary.
* Always validate submitted data on the server as well; browser-side validation can be bypassed.

# 🗁 Practical Example: Registration Form

This example combines input handling, validation, FormData, and submission.

```html
<form id="registrationForm">
  <div>
    <label for="username">Username</label>
    <input
      type="text"
      id="username"
      name="username"
      required
      minlength="3"
    >
  </div>

  <div>
    <label for="email">Email</label>
    <input
      type="email"
      id="email"
      name="email"
      required
    >
  </div>

  <div>
    <label for="password">Password</label>
    <input
      type="password"
      id="password"
      name="password"
      required
      minlength="8"
    >
  </div>

  <button type="submit">Register</button>

  <p id="message" role="status"></p>
</form>

<script>
  const form = document.querySelector("#registrationForm");
  const message = document.querySelector("#message");

  form.addEventListener("submit", async (event) => {
    event.preventDefault();

    if (!form.reportValidity()) {
      return;
    }

    const formData = new FormData(form);

    const user = Object.fromEntries(formData.entries());

    console.log("Registration data:", user);

    message.textContent = "Registration data is valid.";

    form.reset();
  });
</script>
```

This example demonstrates:

* Form element selection.
* Accessing input values.
* Native constraint validation.
* Handling form submission.
* Collecting form data.
* Converting FormData into an object.
* Updating the DOM with feedback.
* Resetting form controls.

# 🗁 Summary

DOM forms provide JavaScript with a powerful interface for collecting, validating, and processing user input.

Important concepts include:

* Selecting forms and accessing controls.
* Reading and updating input values.
* Handling submit, input, change, and reset events.
* Working with checkboxes and radio buttons.
* Collecting data using FormData.
* Native HTML validation.
* Constraint Validation API.
* Custom validation.
* File uploads.
* Fetch-based form submission.
* Programmatic form submission.

Understanding DOM forms is essential for building interactive applications that reliably collect and process user data.
