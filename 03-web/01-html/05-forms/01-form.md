# HTML Form Element (`<form>`)

The HTML `<form>` element is used to collect user input and submit data to a server or process it using JavaScript.

Forms are essential for interactive websites and web applications. They allow users to:

* Register accounts.
* Log in to applications.
* Search for information.
* Submit contact messages.
* Place orders.
* Upload files.
* Update profile information.

The `<form>` element acts as a container for form controls such as `<input>`, `<textarea>`, `<select>`, and `<button>`.

# 🗁 Introduction

A form provides a structured way to collect information from users.

Example:

```html
<form action="/register" method="post">
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

When the user submits this form, the browser sends the successful form controls to the URL specified by the `action` attribute using the HTTP method specified by `method`.

### 🖆 Form Controls

A form can contain different types of controls:

| Element | Purpose |
| --- | --- |
| `<input>` | Accepts different types of user input |
| `<label>` | Provides a description for a form control |
| `<textarea>` | Accepts multiline text |
| `<select>` | Provides a dropdown selection |
| `<button>` | Creates an interactive button |
| `<fieldset>` | Groups related form controls |
| `<legend>` | Provides a caption for a fieldset |
| `<output>` | Displays the result of a calculation |

# 🗁 Basic Syntax

The basic structure of an HTML form is:

```html
<form action="/submit" method="post">

  <!-- Form controls -->

  <button type="submit">Submit</button>

</form>
```

The form contains three important parts:

* `<form>` defines the form container.
* Form controls collect user input.
* The submit button initiates form submission.

# 🗁 Form Attributes

The `<form>` element supports several attributes that control how form data is submitted.

| Attribute        | Description                                          |
| ---------------- | ---------------------------------------------------- |
| `action`         | Specifies the URL that processes the submitted data  |
| `method`         | Specifies the HTTP method                            |
| `enctype`        | Specifies how form data is encoded                   |
| `target`         | Specifies where the response is displayed            |
| `autocomplete`   | Controls browser autocomplete behavior               |
| `novalidate`     | Disables built-in validation during submission       |
| `name`           | Defines the form's name                              |
| `accept-charset` | Specifies the character encoding used for submission |

# 🗁 Action Attribute

The `action` attribute specifies the URL where the browser sends the form data.

Example:

```html
<form action="/register" method="post">
  <input type="text" name="username">

  <button type="submit">Register</button>
</form>
```

When submitted, the browser sends the form data to:

```text
https://example.com/register
```

The final URL depends on the current document's URL and the relative or absolute value of `action`.

### 🖆 Absolute URL

```html
<form
  action="https://api.example.com/register"
  method="post"
>
  <input type="text" name="username">
  <button type="submit">Submit</button>
</form>
```

### 🖆 Relative URL

```html
<form action="/api/register" method="post">
  <input type="text" name="username">
  <button type="submit">Submit</button>
</form>
```

### 🖆 Empty Action

```html
<form action="" method="get">
  <input type="search" name="query">
  <button type="submit">Search</button>
</form>
```

An empty action submits to the current document URL.

If the `action` attribute is omitted, the form submits to the current document URL.

# 🗁 Method Attribute

The `method` attribute specifies the HTTP method used to submit form data.

HTML forms support these submission methods:

* `get`
* `post`
* `dialog`

### 🖆 GET Method

The GET method appends form data to the URL as query parameters.

Example:

```html
<form action="/search" method="get">
  <input
    type="search"
    name="query"
    placeholder="Search..."
  >

  <button type="submit">Search</button>
</form>
```

If the user enters `HTML tutorial`, the resulting URL may look like:

```text
/search?query=HTML+tutorial
```

GET is commonly used for:

* Search forms.
* Filtering products.
* Sorting data.
* Pagination.
* Read-only requests.

### 🖆 POST Method

The POST method sends form data in the HTTP request body.

Example:

```html
<form action="/register" method="post">
  <input
    type="text"
    name="username"
    placeholder="Username"
  >

  <input
    type="email"
    name="email"
    placeholder="Email"
  >

  <button type="submit">Register</button>
</form>
```

POST is commonly used for:

* User registration.
* Login requests.
* Contact forms.
* Creating resources.
* Updating server-side data.
* File uploads.

POST does not automatically encrypt form data. HTTPS is required to protect data while it travels between the browser and server.

### 🖆 Dialog Method

The `dialog` method is used with a form inside a `<dialog>` element.

Submitting the form closes the dialog instead of sending a normal HTTP request.

Example:

```html
<dialog id="confirmDialog">
  <form method="dialog">
    <p>Are you sure you want to continue?</p>

    <button value="cancel">Cancel</button>
    <button value="confirm">Confirm</button>
  </form>
</dialog>
```

This method is useful for dialog interactions that do not require server submission.

# 🗁 GET vs POST

| Feature | GET | POST |
| --- | --- | --- |
| Data location | URL query string | Request body |
| Visibility | Data appears in URL | Data is not normally displayed in URL |
| Browser history | Query string can be stored | Request body is not stored in the URL |
| Bookmarking | Suitable | Not suitable for reproducing submitted data |
| Caching | Can be cached when appropriate | Not normally cached by default |
| Typical usage | Searching and filtering | Creating or modifying data |
| Data size | Limited by practical URL length limits | Supports larger request bodies |
| Sensitive data | Not appropriate in URL | Still requires HTTPS and server-side protection |

**Important:** POST is not inherently secure. HTTPS protects data in transit, while authentication, authorization, validation, and secure server-side handling protect application data.

# 🗁 Enctype Attribute

The `enctype` attribute specifies how form data is encoded when submitted using POST.

There are three common values.

### 🖆 application/x-www-form-urlencoded

This is the default encoding for standard form submissions.

```html
<form
  action="/submit"
  method="post"
  enctype="application/x-www-form-urlencoded"
>
  <input type="text" name="username">

  <button type="submit">Submit</button>
</form>
```

Form values are encoded as URL-encoded key-value pairs.

Example:

```text
username=john
```

### 🖆 multipart/form-data

This encoding is required when uploading files through a form.

```html
<form
  action="/upload"
  method="post"
  enctype="multipart/form-data"
>
  <input type="file" name="document">

  <button type="submit">Upload</button>
</form>
```

The browser sends the form fields as separate parts of a multipart request.

### 🖆 text/plain

This encoding sends form data as plain text.

```html
<form
  action="/submit"
  method="post"
  enctype="text/plain"
>
  <input type="text" name="username">

  <button type="submit">Submit</button>
</form>
```

This format is rarely used in production applications.

# 🗁 Target Attribute

The `target` attribute specifies where the server response should be displayed.

Common values:

| Value | Description |
| --- | --- |
| `_self` | Current browsing context |
| `_blank` | New browsing context |
| `_parent` | Parent browsing context |
| `_top` | Top-level browsing context |

Example:

```html
<form action="/search" method="get" target="_self">
  <input type="search" name="query">

  <button type="submit">Search</button>
</form>
```

The default behavior is equivalent to `_self`.

# 🗁 Autocomplete Attribute

The `autocomplete` attribute controls whether the browser can automatically fill form values.

Example:

```html
<form action="/register" method="post" autocomplete="on">
  <input
    type="text"
    name="fullname"
    autocomplete="name"
  >

  <input
    type="email"
    name="email"
    autocomplete="email"
  >

  <button type="submit">Register</button>
</form>
```

Common autocomplete values include:

| Value | Purpose |
| --- | --- |
| `on` | Allows browser autocomplete |
| `off` | Requests that autocomplete be disabled |
| `name` | Full name |
| `given-name` | First or given name |
| `family-name` | Last or family name |
| `email` | Email address |
| `username` | Username |
| `current-password` | Existing password |
| `new-password` | New password |
| `tel` | Telephone number |
| `street-address` | Street address |
| `postal-code` | Postal code |

Browsers may still provide autofill behavior even when `autocomplete="off"` is specified, depending on the field and browser.

# 🗁 Form Controls

A form can contain different HTML controls depending on the information being collected.

Example:

```html
<form action="/profile" method="post">

  <input type="text" name="username">

  <input type="email" name="email">

  <textarea name="bio"></textarea>

  <select name="country">
    <option value="bd">Bangladesh</option>
    <option value="us">United States</option>
  </select>

  <button type="submit">Save Profile</button>

</form>
```

Each control should have a meaningful `name` attribute if its value needs to be submitted.

Without a `name`, a form control generally does not contribute its value to the submitted form data.

# 🗁 Form Submission

Form submission occurs when the user activates a submit button or another submission mechanism.

### 🖆 Submit Button

```html
<form action="/contact" method="post">
  <input type="text" name="name">

  <button type="submit">Send Message</button>
</form>
```

A button with `type="submit"` submits its associated form.

### 🖆 Input Submit Button

An `<input>` element can also submit a form.

```html
<form action="/search" method="get">
  <input type="search" name="query">

  <input type="submit" value="Search">
</form>
```

### 🖆 Reset Button

A reset button restores form controls to their initial values.

```html
<form>
  <input type="text" name="username" value="John">

  <button type="reset">Reset</button>
  <button type="submit">Submit</button>
</form>
```

Reset buttons should be used carefully because users can accidentally lose their entered information.

# 🗁 Form Validation

HTML provides built-in form validation through attributes such as:

* `required`
* `minlength`
* `maxlength`
* `min`
* `max`
* `step`
* `pattern`
* Input types such as `email` and `url`

Example:

```html
<form action="/register" method="post">
  <input
    type="email"
    name="email"
    placeholder="Email address"
    required
  >

  <input
    type="password"
    name="password"
    minlength="8"
    required
  >

  <button type="submit">Register</button>
</form>
```

The browser checks the constraints before submitting the form.

The `novalidate` attribute disables built-in constraint validation during form submission.

```html
<form action="/submit" method="post" novalidate>
  <input type="email" name="email" required>

  <button type="submit">Submit</button>
</form>
```

Client-side validation improves usability, but server-side validation is still necessary because browser validation can be bypassed.

# 🗁 Working with JavaScript

JavaScript can intercept form submission and process form data without performing a traditional page navigation.

Example:

```html
<form id="contactForm">
  <input
    type="text"
    name="username"
    placeholder="Username"
    required
  >

  <input
    type="email"
    name="email"
    placeholder="Email"
    required
  >

  <button type="submit">Submit</button>
</form>

<script>
  const form = document.querySelector("#contactForm");

  form.addEventListener("submit", (event) => {
    event.preventDefault();

    const formData = new FormData(form);

    const username = formData.get("username");
    const email = formData.get("email");

    console.log({
      username,
      email
    });
  });
</script>
```

### 🖆 Understanding the Example

* `querySelector()` selects the form.
* `addEventListener()` listens for the submit event.
* `preventDefault()` prevents the browser's default submission behavior.
* `FormData()` collects the form's successful controls.
* `get()` retrieves a value using its field name.

This approach is commonly used when submitting data through `fetch()` or another JavaScript networking API.

# 🗁 Complete Form Example

The following example combines several form attributes and controls.

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
    <h1>Create an Account</h1>

    <form
      action="/register"
      method="post"
      autocomplete="on"
    >

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
        <label for="email">Email Address</label>

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
          autocomplete="new-password"
          minlength="8"
          required
        >
      </div>

      <div>
        <label for="country">Country</label>

        <select id="country" name="country" required>
          <option value="">Select a country</option>
          <option value="bd">Bangladesh</option>
          <option value="us">United States</option>
          <option value="uk">United Kingdom</option>
        </select>
      </div>

      <div>
        <label for="bio">About You</label>

        <textarea
          id="bio"
          name="bio"
          rows="4"
        ></textarea>
      </div>

      <button type="submit">Create Account</button>
      <button type="reset">Clear Form</button>

    </form>
  </main>

</body>
</html>
```

# 🗁 Best Practices

* Always use meaningful `name` attributes for submitted form controls.
* Associate every form control with an appropriate `<label>`.
* Choose GET for retrieving or filtering data.
* Choose POST for submitting data that creates or modifies server-side resources.
* Use `multipart/form-data` for file uploads.
* Use appropriate input types to improve user experience and validation.
* Use `required` and other constraint attributes when appropriate.
* Provide clear instructions and error messages.
* Validate all submitted data on the server.
* Use HTTPS when transmitting sensitive information.
* Use semantic elements such as `<fieldset>` and `<legend>` to group related controls.
* Use JavaScript when custom interactions are needed, without unnecessarily replacing native browser behavior.

# 🗁 Common Mistakes

### 🖆 Missing the Name Attribute

Incorrect:

```html
<form action="/submit" method="post">
  <input type="text" placeholder="Username">

  <button type="submit">Submit</button>
</form>
```

The input has no `name`, so its value is not included in the normal form submission.

Correct:

```html
<input type="text" name="username">
```

### 🖆 Using GET for Sensitive Information

Incorrect:

```html
<form action="/login" method="get">
  <input type="text" name="username">
  <input type="password" name="password">

  <button type="submit">Login</button>
</form>
```

Credentials can appear in the URL.

Use POST over HTTPS for login submissions.

### 🖆 Forgetting the Multipart Encoding

Incorrect for file uploads:

```html
<form action="/upload" method="post">
  <input type="file" name="document">

  <button type="submit">Upload</button>
</form>
```

Correct:

```html
<form
  action="/upload"
  method="post"
  enctype="multipart/form-data"
>
  <input type="file" name="document">

  <button type="submit">Upload</button>
</form>
```

### 🖆 Missing Labels

Incorrect:

```html
<input type="email" name="email" placeholder="Email">
```

Correct:

```html
<label for="email">Email Address</label>
<input type="email" id="email" name="email">
```

A placeholder is not a replacement for a proper label.

### 🖆 Forgetting Server-Side Validation

HTML validation can be bypassed. Never rely exclusively on client-side validation to protect application data.

# 🗁 Practical Exercises

* Create a registration form using GET and POST methods and compare their submitted URLs and request data.
* Build a login form with username and password fields.
* Create a contact form with name, email, subject, and message fields.
* Build a file upload form using `multipart/form-data`.
* Use `autocomplete` values for name, email, and password fields.
* Intercept form submission using JavaScript and inspect the `FormData` object.
* Test native validation using `required`, `minlength`, and `type="email"`.

# 🗁 Interview Questions

**What is the purpose of the `<form>` element?**

It defines a form for collecting user input and submitting it for processing.

**What is the difference between GET and POST?**

GET places form data in the URL query string, while POST sends form data in the HTTP request body.

**What is the purpose of the `action` attribute?**

It specifies the URL that processes the submitted form data.

**What is the purpose of the `method` attribute?**

It specifies the submission method, such as GET or POST.

**What is the purpose of the `enctype` attribute?**

It specifies how form data is encoded during submission.

**Which encoding is required for file uploads?**

`multipart/form-data`.

**Why is the `name` attribute important?**

It provides the key used to include a control's value in submitted form data.

**Does POST encrypt submitted data?**

No. HTTPS provides encryption during transmission.

**What does `novalidate` do?**

It disables the browser's built-in constraint validation when the form is submitted.

**How can JavaScript prevent normal form submission?**

By calling `event.preventDefault()` inside a form submission event handler.

**What is the purpose of `FormData`?**

It provides an API for collecting form control values into key-value entries, including support for file values.

# 🗁 References

* [MDN — `<form>` Element](https://developer.mozilla.org/en-US/docs/Web/HTML/Reference/Elements/form)
* [MDN — Sending Form Data](https://developer.mozilla.org/en-US/docs/Learn_web_development/Extensions/Forms/Sending_and_retrieving_form_data)
* [MDN — FormData API](https://developer.mozilla.org/en-US/docs/Web/API/FormData)
* [WHATWG — Forms](https://html.spec.whatwg.org/multipage/forms.html)
* [W3C WAI — Forms Tutorial](https://www.w3.org/WAI/tutorials/forms/)
