# DOM Attributes

HTML attributes provide additional information and configuration for HTML elements.

For example:

```html
<input
  id="username"
  type="text"
  class="form-control"
  placeholder="Enter your username"
  required
>
```

This input element contains several attributes:

* `id` identifies the element.
* `type` specifies the input type.
* `class` defines CSS classes.
* `placeholder` provides a hint to the user.
* `required` indicates that the field must be completed.

JavaScript provides DOM APIs for reading, adding, modifying, checking, and removing HTML attributes dynamically.

# 🗁 HTML Attributes vs DOM Properties

HTML attributes and DOM properties are related, but they are not the same thing.

**HTML attributes** are values defined in the HTML markup.

**DOM properties** are values exposed by JavaScript objects representing HTML elements.

Example:

```html
<input id="username" value="Alice">
```

Accessing the attribute:

```javascript
const input = document.getElementById("username");

console.log(input.getAttribute("value"));
```

Accessing the property:

```javascript
console.log(input.value);
```

Both initially return:

```text
Alice
```

However, changing the property does not necessarily change the original HTML attribute.

```javascript
input.value = "Bob";

console.log(input.value); // Bob

console.log(input.getAttribute("value")); // Alice
```

The `value` property represents the input's current value, while the `value` attribute generally represents its initial/default value.

# 🗁 getAttribute()

The `getAttribute()` method retrieves the value of an HTML attribute.

Syntax:

```javascript
element.getAttribute(attributeName);
```

Example:

```html
<input
  id="username"
  type="text"
  placeholder="Enter username"
>
```

```javascript
const input = document.getElementById("username");

const placeholder = input.getAttribute("placeholder");

console.log(placeholder);
```

Output:

```text
Enter username
```

If the specified attribute does not exist, `getAttribute()` returns `null`.

```javascript
console.log(input.getAttribute("disabled")); // null
```

Attribute names are generally case-insensitive for HTML elements in HTML documents.

# 🗁 setAttribute()

The `setAttribute()` method adds a new attribute or updates an existing attribute.

Syntax:

```javascript
element.setAttribute(attributeName, value);
```

Example:

```html
<input id="username" type="text">
```

```javascript
const input = document.getElementById("username");

input.setAttribute("placeholder", "Enter username");

input.setAttribute("maxlength", "30");
```

Result:

```html
<input
  id="username"
  type="text"
  placeholder="Enter username"
  maxlength="30"
>
```

If an attribute already exists, its value is replaced.

```javascript
input.setAttribute("type", "email");
```

Attribute values are represented as strings.

```javascript
input.setAttribute("maxlength", 50);

console.log(input.getAttribute("maxlength")); // "50"
```

# 🗁 removeAttribute()

The `removeAttribute()` method removes an attribute from an element.

Syntax:

```javascript
element.removeAttribute(attributeName);
```

Example:

```html
<input
  id="username"
  type="text"
  placeholder="Enter username"
  disabled
>
```

```javascript
const input = document.getElementById("username");

input.removeAttribute("disabled");

input.removeAttribute("placeholder");
```

The input is no longer disabled and no longer has a placeholder attribute.

Removing an attribute that does not exist does not throw an error.

# 🗁 hasAttribute()

The `hasAttribute()` method checks whether an element contains a specified attribute.

Syntax:

```javascript
element.hasAttribute(attributeName);
```

Example:

```html
<button id="submit" disabled>
  Submit
</button>
```

```javascript
const button = document.getElementById("submit");

console.log(button.hasAttribute("disabled")); // true

console.log(button.hasAttribute("required")); // false
```

It returns a Boolean value:

* `true` if the attribute exists.
* `false` if the attribute does not exist.

For Boolean HTML attributes, the presence of the attribute is what matters, not its textual value.

# 🗁 toggleAttribute()

The `toggleAttribute()` method adds an attribute if it does not exist or removes it if it already exists.

Syntax:

```javascript
element.toggleAttribute(attributeName);
```

Example:

```html
<button id="submit">Submit</button>
```

```javascript
const button = document.getElementById("submit");

button.toggleAttribute("disabled");
```

The button becomes disabled.

Calling it again removes the attribute:

```javascript
button.toggleAttribute("disabled");
```

You can force a particular state using the second parameter.

```javascript
button.toggleAttribute("disabled", true);

button.toggleAttribute("disabled", false);
```

* `true`: Ensures the attribute exists.
* `false`: Ensures the attribute is removed.

The method returns `true` if the attribute is present after the operation and `false` otherwise.

# 🗁 Working with Boolean Attributes

Some HTML attributes are Boolean attributes.

Examples include:

* `disabled`
* `checked`
* `selected`
* `required`
* `readonly`
* `multiple`
* `autofocus`
* `hidden`

For these attributes, presence means true and absence means false.

Example:

```html
<input id="email" type="email" required>
```

Checking:

```javascript
const email = document.getElementById("email");

console.log(email.hasAttribute("required")); // true
```

Removing:

```javascript
email.removeAttribute("required");
```

Adding:

```javascript
email.setAttribute("required", "");
```

The following markup also represents a present Boolean attribute:

```html
<input required="false">
```

Despite the text `"false"`, the attribute is present, so the Boolean HTML attribute is considered true.

For Boolean DOM properties, use actual Boolean values:

```javascript
email.required = false;

email.disabled = true;
```

# 🗁 Attribute Properties

Many standard HTML attributes have corresponding DOM properties.

Example:

```html
<a
  id="website"
  href="https://example.com"
  target="_blank"
>
  Visit Website
</a>
```

```javascript
const link = document.getElementById("website");

console.log(link.id);

console.log(link.href);

console.log(link.target);
```

You can also modify them directly:

```javascript
link.href = "https://developer.mozilla.org/";

link.target = "_self";

link.id = "documentation-link";
```

The browser exposes many common attributes as properties on specific HTML element interfaces.

Examples:

| HTML attribute | DOM property |
| --- | --- |
| `id` | `element.id` |
| `class` | `element.className` |
| `href` | `element.href` |
| `src` | `element.src` |
| `alt` | `element.alt` |
| `title` | `element.title` |
| `disabled` | `element.disabled` |
| `checked` | `element.checked` |
| `value` | `element.value` |
| `required` | `element.required` |

Some property values may be normalized by the browser. For example, an anchor element's `href` property generally returns an absolute URL even if the HTML attribute contains a relative URL.

# 🗁 getAttribute() vs DOM Properties

Consider this HTML:

```html
<a id="link" href="/about">About</a>
```

JavaScript:

```javascript
const link = document.getElementById("link");

console.log(link.getAttribute("href"));

console.log(link.href);
```

The results may differ:

```text
/about
https://your-domain.com/about
```

The attribute returns the value written in the markup.

The property returns the browser-resolved URL.

Use `getAttribute()` when you need the actual attribute value. Use the corresponding DOM property when you need the element's current JavaScript-facing value.

# 🗁 Working with class and id Attributes

The `class` and `id` attributes are commonly accessed through dedicated properties.

Example:

```html
<div id="container" class="box active"></div>
```

Accessing the ID:

```javascript
const container = document.getElementById("container");

console.log(container.id); // container
```

Updating the ID:

```javascript
container.id = "main-container";
```

Accessing the class:

```javascript
console.log(container.className); // box active
```

Updating the class:

```javascript
container.className = "card highlighted";
```

For individual class operations, prefer `classList`:

```javascript
container.classList.add("responsive");

container.classList.remove("active");

container.classList.toggle("visible");

console.log(container.classList.contains("card"));
```

# 🗁 Custom Data Attributes

HTML supports custom attributes that begin with `data-`.

They are useful for storing application-specific metadata directly on HTML elements.

Example:

```html
<button
  id="delete-user"
  data-user-id="42"
  data-action="delete"
>
  Delete User
</button>
```

JavaScript exposes these attributes through the `dataset` property.

```javascript
const button = document.getElementById("delete-user");

console.log(button.dataset.userId); // "42"

console.log(button.dataset.action); // "delete"
```

Updating values:

```javascript
button.dataset.userId = "100";

button.dataset.action = "archive";
```

Adding a new data attribute:

```javascript
button.dataset.role = "admin";
```

The resulting HTML attributes are:

```html
<button
  data-user-id="100"
  data-action="archive"
  data-role="admin"
>
  Delete User
</button>
```

**Data attribute naming convention**

HTML uses kebab-case:

```html
<div data-user-id="42"></div>
```

JavaScript uses camelCase:

```javascript
element.dataset.userId;
```

The conversion works in both directions.

Data attribute values are always strings.

```javascript
element.dataset.userId = 42;

console.log(typeof element.dataset.userId); // "string"
```

Convert values when a different data type is needed:

```javascript
const userId = Number(element.dataset.userId);
```

# 🗁 Working with Attributes Collection

The `attributes` property returns a live `NamedNodeMap` containing an element's attributes.

Example:

```html
<div id="container" class="box" data-role="admin"></div>
```

```javascript
const container = document.getElementById("container");

console.log(container.attributes);
```

You can inspect individual attributes:

```javascript
console.log(container.attributes[0].name);

console.log(container.attributes[0].value);
```

You can iterate over the attributes:

```javascript
Array.from(container.attributes).forEach((attribute) => {
  console.log(attribute.name, attribute.value);
});
```

You can also access an attribute by name:

```javascript
const classAttribute = container.attributes.getNamedItem("class");

console.log(classAttribute.value);
```

For normal attribute operations, `getAttribute()`, `setAttribute()`, and related methods are generally easier to use.

# 🗁 Synchronizing Attributes and Properties

Some HTML elements maintain both an initial attribute value and a current DOM property value.

Form controls are a common example.

Consider:

```html
<input id="username" type="text" value="Alice">
```

```javascript
const input = document.getElementById("username");
```

Initially:

```javascript
console.log(input.getAttribute("value")); // "Alice"

console.log(input.value); // "Alice"
```

Change the current value:

```javascript
input.value = "Bob";
```

Now:

```javascript
console.log(input.getAttribute("value")); // "Alice"

console.log(input.value); // "Bob"
```

Update the default value through its property:

```javascript
input.defaultValue = "Charlie";
```

The `defaultValue` property reflects the initial/default value represented by the `value` content attribute.

For checkboxes, a similar distinction exists:

```html
<input id="subscribe" type="checkbox" checked>
```

```javascript
const checkbox = document.getElementById("subscribe");

console.log(checkbox.checked); // true

console.log(checkbox.defaultChecked); // true
```

Changing the current checked state:

```javascript
checkbox.checked = false;
```

The `checked` content attribute may still be present, while the current `checked` property is false.

This distinction is important when handling forms and user interactions.

# 🗁 Practical Example: Dynamic Attribute Management

HTML:

```html
<div id="app">
  <input id="username" type="text">

  <button id="enable">Enable Input</button>

  <button id="disable">Disable Input</button>

  <button id="update">Update Placeholder</button>
</div>
```

JavaScript:

```javascript
const username = document.getElementById("username");

const enableButton = document.getElementById("enable");

const disableButton = document.getElementById("disable");

const updateButton = document.getElementById("update");

enableButton.addEventListener("click", () => {
  username.removeAttribute("disabled");
});

disableButton.addEventListener("click", () => {
  username.setAttribute("disabled", "");
});

updateButton.addEventListener("click", () => {
  username.setAttribute("placeholder", "Enter your username");
});
```

This example demonstrates how JavaScript can dynamically manage attributes in response to user interactions.

# 🗁 Practical Example: Data Attributes and Event Handling

HTML:

```html
<div id="products">
  <button data-product-id="101">Product A</button>
  <button data-product-id="102">Product B</button>
  <button data-product-id="103">Product C</button>
</div>
```

JavaScript:

```javascript
const products = document.getElementById("products");

products.addEventListener("click", (event) => {
  const button = event.target.closest("button[data-product-id]");

  if (!button || !products.contains(button)) {
    return;
  }

  const productId = Number(button.dataset.productId);

  console.log("Selected product:", productId);
});
```

Each button stores its product ID in a custom data attribute.

The event handler reads the value through `dataset`.

This approach is useful when working with dynamically generated elements and event delegation.

# 🗁 Common Mistakes

**Treating Boolean attributes as ordinary string values**

```javascript
button.setAttribute("disabled", "false");
```

This still disables the button because the `disabled` attribute exists.

Use:

```javascript
button.disabled = false;
```

**Expecting attributes and properties to always match**

```javascript
input.value = "Bob";

console.log(input.getAttribute("value"));
```

The attribute may still contain the initial value.

Use the appropriate property depending on whether you need the current or default value.

**Forgetting that dataset values are strings**

```javascript
const quantity = element.dataset.quantity;

console.log(quantity + 5);
```

If `quantity` is `"10"`, the result is `"105"` because string concatenation occurs.

Convert it first:

```javascript
const quantity = Number(element.dataset.quantity);

console.log(quantity + 5); // 15
```

**Using setAttribute() for every DOM property**

```javascript
input.setAttribute("value", "Alice");
```

For current form state, prefer:

```javascript
input.value = "Alice";
```

**Using invalid attribute names**

```javascript
element.setAttribute("user id", "42");
```

Attribute names must follow valid naming rules.

For custom metadata, use `data-*` attributes.

# 🗁 Best Practices

* Use `getAttribute()` when you need the actual HTML attribute value.
* Use DOM properties when you need the current element state.
* Use `setAttribute()` and `removeAttribute()` for dynamic attribute management.
* Use `hasAttribute()` to check whether an attribute exists.
* Use `toggleAttribute()` when an attribute should switch between present and absent.
* Use Boolean DOM properties for Boolean element state.
* Use `dataset` for custom `data-*` attributes.
* Convert dataset values when numeric or Boolean types are required.
* Prefer `classList` for managing CSS classes.
* Avoid storing sensitive information in data attributes because they are visible in the DOM.

# 🗁 Key Takeaways

* HTML attributes provide configuration and metadata for elements.
* DOM properties expose element state and behavior through JavaScript.
* `getAttribute()` retrieves an attribute value.
* `setAttribute()` adds or updates an attribute.
* `removeAttribute()` removes an attribute.
* `hasAttribute()` checks attribute existence.
* `toggleAttribute()` switches an attribute between present and absent.
* Boolean attributes are true when present and false when absent.
* `dataset` provides convenient access to custom `data-*` attributes.
* Form properties such as `value`, `checked`, and `selected` often represent current state rather than initial markup values.

# 🗁 References

* [MDN — Element.getAttribute()](https://developer.mozilla.org/en-US/docs/Web/API/Element/getAttribute)
* [MDN — Element.setAttribute()](https://developer.mozilla.org/en-US/docs/Web/API/Element/setAttribute)
* [MDN — Element.removeAttribute()](https://developer.mozilla.org/en-US/docs/Web/API/Element/removeAttribute)
* [MDN — Element.hasAttribute()](https://developer.mozilla.org/en-US/docs/Web/API/Element/hasAttribute)
* [MDN — Element.toggleAttribute()](https://developer.mozilla.org/en-US/docs/Web/API/Element/toggleAttribute)
* [MDN — HTMLElement.dataset](https://developer.mozilla.org/en-US/docs/Web/API/HTMLElement/dataset)
* [MDN — Element.attributes](https://developer.mozilla.org/en-US/docs/Web/API/Element/attributes)
* [MDN — HTML Attributes](https://developer.mozilla.org/en-US/docs/Web/HTML/Attributes)
