# HTML Attributes

HTML attributes provide additional information about HTML elements. They configure an element's behavior, identify it, define its properties, or connect it to external resources.

Attributes are written inside an element's opening tag and usually consist of a name and a value.

For example:

```html
<a href="https://example.com" target="_blank">
    Visit Example
</a>
```

In this example:

* `href` specifies the destination URL.
* `target` specifies where the linked document should open.

Understanding attributes is essential for creating functional, accessible, and well-structured HTML documents.

# 🗁 What Are HTML Attributes?

HTML attributes are name-value pairs that provide additional information about an element.

They are generally written in the opening tag.

### 🖆 Basic Syntax

```html
<element attribute="value">
    Content
</element>
```

Example:

```html
<img src="profile.jpg" alt="Profile photograph">
```

Here:

| Part | Description |
| --- | --- |
| `img` | HTML element |
| `src` | Attribute name |
| `profile.jpg` | Attribute value |
| `alt` | Attribute name |
| `Profile photograph` | Attribute value |

The `<img>` element uses `src` to identify the image resource and `alt` to provide alternative text.

# 🗁 How to Write HTML Attributes

### 🖆 Quoted Attribute Values

The recommended convention is to enclose attribute values in double quotation marks.

```html
<input type="text" name="username">
```

Single quotation marks are also valid:

```html
<input type='text' name='username'>
```

Both forms are supported by HTML.

### 🖆 Unquoted Attribute Values

HTML also permits unquoted attribute values under certain conditions.

```html
<input type=text name=username>
```

However, unquoted values cannot contain spaces or certain characters that would change how the HTML parser interprets the markup.

Prefer quoted values for consistency and readability.

### 🖆 Multiple Attributes

An element can contain multiple attributes.

```html
<a
    href="https://example.com"
    target="_blank"
    rel="noopener noreferrer"
>
    Visit Example
</a>
```

Each attribute is separated by whitespace.

# 🗁 Common HTML Attributes

Here are some frequently used HTML attributes.

| Attribute | Purpose | Example |
| --- | --- | --- |
| `id` | Identifies an element uniquely within a document | `id="header"` |
| `class` | Assigns one or more CSS classes | `class="container"` |
| `style` | Applies inline CSS declarations | `style="color: red"` |
| `title` | Provides advisory information, often shown as a tooltip | `title="More information"` |
| `href` | Specifies a hyperlink destination | `href="/about"` |
| `src` | Specifies an external resource | `src="image.jpg"` |
| `alt` | Provides alternative text for an image | `alt="Mountain view"` |
| `target` | Specifies a browsing context for a link or form | `target="_blank"` |
| `name` | Defines a name used by forms and other features | `name="email"` |
| `value` | Specifies an element's value | `value="Submit"` |
| `type` | Specifies an element's type or behavior | `type="email"` |
| `placeholder` | Provides a short input hint | `placeholder="Enter email"` |
| `disabled` | Disables an element | `disabled` |
| `readonly` | Makes supported form controls non-editable | `readonly` |
| `required` | Requires a value before form submission | `required` |

Not every attribute is valid on every element. Attribute applicability depends on the HTML element and its specification.

# 🗁 Practical Examples

### 🖆 ID Attribute

The `id` attribute assigns an identifier to an element.

```html
<h1 id="page-title">Welcome</h1>
```

An ID can be used by CSS, JavaScript, and URL fragment navigation.

CSS example:

```css
#page-title {
    color: blue;
}
```

JavaScript example:

```javascript
const heading = document.getElementById("page-title");

console.log(heading.textContent);
```

### 🖆 Class Attribute

The `class` attribute assigns one or more class names to an element.

```html
<p class="text-primary">
    Welcome to HTML.
</p>
```

Multiple classes can be assigned to one element:

```html
<div class="container card shadow">
    Content goes here.
</div>
```

CSS example:

```css
.text-primary {
    color: blue;
}

.card {
    padding: 20px;
}
```

### 🖆 Style Attribute

The `style` attribute applies inline CSS declarations.

```html
<p style="color: red; font-size: 18px;">
    This is a styled paragraph.
</p>
```

Inline styles are useful for specific cases, but reusable styling is generally better maintained in CSS stylesheets.

### 🖆 Title Attribute

The `title` attribute provides advisory information about an element.

```html
<button type="button" title="Save your changes">
    Save
</button>
```

Browsers commonly display the title as a tooltip when a user points to the element.

Do not rely on `title` alone to provide essential instructions or accessible labels.

### 🖆 Href Attribute

The `href` attribute specifies the destination of a hyperlink.

```html
<a href="https://developer.mozilla.org/">
    MDN Documentation
</a>
```

It also supports relative URLs and fragment identifiers.

```html
<a href="about.html">About</a>

<a href="#contact">Go to Contact</a>
```

### 🖆 Src Attribute

The `src` attribute specifies the URL of an external resource.

Example:

```html
<img
    src="images/profile.jpg"
    alt="Profile photograph"
>
```

The same attribute is used by elements such as `<script>`, `<iframe>`, `<audio>`, and `<video>`.

### 🖆 Alt Attribute

The `alt` attribute provides alternative text for an image.

```html
<img
    src="images/mountain.jpg"
    alt="Snow-covered mountain beneath a clear sky"
>
```

Alternative text helps users who cannot see the image understand its relevant information.

For decorative images, use an empty alternative text:

```html
<img src="images/decoration.svg" alt="">
```

# 🗁 Attribute Values and Data Types

HTML attributes do not all follow the same value rules.

Common categories include:

| Attribute Category | Description | Example |
| --- | --- | --- |
| Text attributes | Accept text values | `title="Information"` |
| URL attributes | Accept resource URLs | `href="/about"` |
| Enumerated attributes | Accept values from a defined set | `draggable="true"` |
| Boolean attributes | Represent true or false through presence or absence | `disabled` |
| Numeric attributes | Accept numeric values under their rules | `width="300"` |
| Token-list attributes | Accept one or more space-separated tokens | `class="card active"` |

Understanding the expected value type helps prevent invalid markup and unexpected behavior.

# 🗁 Global and Element-Specific Attributes

HTML attributes can be broadly understood by where they are applicable.

### 🖆 Global Attributes

Global attributes can be used on all HTML elements, subject to specific semantic constraints.

Examples:

* `id`
* `class`
* `style`
* `title`
* `lang`
* `hidden`
* `tabindex`
* `contenteditable`

Example:

```html
<section
    id="about"
    class="content-section"
    lang="en"
>
    <h2>About Us</h2>
    <p>Learn more about our organization.</p>
</section>
```

### 🖆 Element-Specific Attributes

Some attributes are designed for particular HTML elements.

Examples:

| Element | Attribute | Purpose |
| --- | --- | --- |
| `<a>` | `href` | Link destination |
| `<img>` | `src` | Image resource |
| `<img>` | `alt` | Alternative text |
| `<input>` | `type` | Input control type |
| `<form>` | `action` | Form submission destination |
| `<form>` | `method` | Form submission method |
| `<script>` | `src` | External JavaScript resource |
| `<link>` | `rel` | Relationship to the linked resource |

# 🗁 Boolean Attributes

Boolean attributes represent a true or false state through their presence or absence.

Example:

```html
<input type="text" disabled>
```

The presence of `disabled` enables the disabled state.

These are equivalent:

```html
<input type="text" disabled>
```

```html
<input type="text" disabled="">
```

```html
<input type="text" disabled="disabled">
```

This does not disable the input:

```html
<input type="text" disabled="false">
```

For a boolean attribute, its presence means true, regardless of the text assigned as its value.

Common boolean attributes include:

* `disabled`
* `checked`
* `required`
* `readonly`
* `multiple`
* `autofocus`
* `hidden`
* `selected`

# 🗁 Data Attributes

Custom data attributes allow developers to store application-specific information in HTML elements.

They use the `data-*` naming convention.

Example:

```html
<button
    type="button"
    data-user-id="101"
    data-role="admin"
>
    View User
</button>
```

JavaScript can access these values through the `dataset` property.

```javascript
const button = document.querySelector("button");

console.log(button.dataset.userId);
console.log(button.dataset.role);
```

Output:

```text
101
admin
```

Data attributes are useful for connecting HTML elements with JavaScript behavior.

They should not be used to store sensitive information because values are accessible in the page's DOM.

# 🗁 Attribute Order and Formatting

HTML does not require a specific order for most attributes.

Both examples are valid:

```html
<a href="/about" class="navigation-link">
    About
</a>
```

```html
<a class="navigation-link" href="/about">
    About
</a>
```

For maintainability, use a consistent formatting convention throughout your project.

For elements with many attributes, multiline formatting improves readability:

```html
<img
    src="images/product.jpg"
    alt="Wireless keyboard"
    width="400"
    height="300"
    loading="lazy"
>
```

# 🗁 Common Mistakes

| Mistake | Better Approach |
| --- | --- |
| Forgetting quotation marks around attribute values | Use double quotes consistently |
| Using an attribute on an unsupported element | Check the element's supported attributes |
| Assigning duplicate IDs | Use unique IDs within the document |
| Writing `disabled="false"` | Remove the boolean attribute to enable the control |
| Forgetting image alternative text | Provide appropriate `alt` text |
| Using inline styles everywhere | Prefer reusable CSS rules |
| Using `title` as the only accessible label | Use visible labels or appropriate accessible naming |
| Storing sensitive information in `data-*` | Keep sensitive information outside client-side markup |

# 🗁 Best Practices

* Use lowercase attribute names.
* Enclose attribute values in double quotation marks.
* Use meaningful and descriptive attribute values.
* Keep IDs unique within each document.
* Use classes for reusable styling and element grouping.
* Provide appropriate alternative text for images.
* Use boolean attributes correctly.
* Prefer external CSS for reusable styling.
* Use `data-*` attributes for application-specific data.
* Avoid unsupported or obsolete attributes.
* Validate your HTML markup.

# 🗁 Practical Exercises

### 🖆 Exercise: Create a Profile Card

Create a profile card using different HTML attributes.

Requirements:

* Add a unique `id` to the card.
* Assign multiple CSS classes.
* Add an image with meaningful alternative text.
* Add a link to a profile page.
* Add a `title` attribute to the link.

Example structure:

```html
<div id="profile-card" class="card profile">
    <img src="profile.jpg" alt="Profile photograph">

    <h2>John Doe</h2>

    <a href="profile.html" title="View full profile">
        View Profile
    </a>
</div>
```

### 🖆 Exercise: Practice Boolean Attributes

Create a form containing:

* A required email input.
* A disabled input.
* A checked checkbox.
* A read-only text input.

Observe how each attribute changes the behavior of its element.

### 🖆 Exercise: Practice Data Attributes

Create three buttons with different `data-*` attributes.

Use JavaScript to read their values when a user clicks them.

# 🗁 Interview Questions

**What are HTML attributes?**

HTML attributes provide additional information about HTML elements and are generally written inside opening tags.

**Where are HTML attributes written?**

They are written inside the opening tag of an HTML element.

**What is the difference between an HTML element and an attribute?**

An element defines a structural or semantic component of a document, while an attribute provides additional configuration or information about that element.

**Can an HTML element have multiple attributes?**

Yes. An element can have multiple applicable attributes, separated by whitespace.

**What is the difference between global and element-specific attributes?**

Global attributes are applicable to all HTML elements, subject to semantic constraints. Element-specific attributes apply only to particular elements.

**What is a boolean attribute?**

A boolean attribute represents a true or false state through its presence or absence.

**What is the purpose of the `id` attribute?**

It provides a unique identifier for an element within an HTML document.

**What is the purpose of the `class` attribute?**

It assigns one or more class names to an element, commonly for CSS styling and JavaScript selection.

**What is the difference between `id` and `class`?**

An ID identifies a particular element and should be unique within the document. A class can be shared by multiple elements.

**What are data attributes?**

Data attributes are custom HTML attributes using the `data-*` naming convention to store application-specific information.

**How can JavaScript access data attributes?**

JavaScript can access them through the element's `dataset` property.

**Does `disabled="false"` enable an HTML element?**

No. Boolean attributes are true when present. To enable the element, remove the `disabled` attribute.

# 🗁 References

* [MDN — HTML Attributes](https://developer.mozilla.org/en-US/docs/Web/HTML/Attributes)
* [MDN — Global Attributes](https://developer.mozilla.org/en-US/docs/Web/HTML/Reference/Global_attributes)
* [MDN — Data Attributes](https://developer.mozilla.org/en-US/docs/Web/HTML/How_to/Use_data_attributes)
* [WHATWG — HTML Living Standard](https://html.spec.whatwg.org/)
