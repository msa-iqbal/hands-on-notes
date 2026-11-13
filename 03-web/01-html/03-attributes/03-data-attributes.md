# HTML Data Attributes

HTML `data-*` attributes allow developers to store custom, application-specific data directly on HTML elements.

They are global attributes, meaning they can be used on any HTML element.

Data attributes are particularly useful when you need to:

* Store additional information on HTML elements.
* Associate elements with application-specific values.
* Pass information from HTML to JavaScript.
* Build interactive components without adding non-standard HTML attributes.
* Store configuration values for frontend behavior.

The `data-*` attribute provides a standard way to attach custom data to HTML elements.

# 🗁 What Are Data Attributes?

A data attribute is a custom HTML attribute whose name starts with `data-`.

### 🖆 Syntax

```html
<element data-name="value">Content</element>
```

The attribute name must begin with `data-`, followed by a name.

### 🖆 Example

```html
<div
  id="product"
  data-product-id="101"
  data-product-name="Laptop"
  data-price="1200"
>
  Laptop
</div>
```

In this example:

* `data-product-id` stores the product identifier.
* `data-product-name` stores the product name.
* `data-price` stores the product price.

These attributes are valid HTML and can be accessed using JavaScript.

# 🗁 Naming Rules

Data attribute names follow specific naming conventions.

* Attribute names must begin with `data-`.
* The name after `data-` should contain lowercase letters, digits, hyphens, periods, colons, or underscores.
* Attribute names must not contain uppercase ASCII letters.
* Attribute names should describe the data they represent.
* Avoid using names that conflict with standard HTML attributes.

### 🖆 Recommended Naming

```html
<div
  data-user-id="25"
  data-user-name="John"
  data-account-status="active"
  data-is-verified="true"
>
  User Information
</div>
```

Use lowercase kebab-case for readability.

### 🖆 Avoid Non-Standard Custom Attributes

Incorrect approach:

```html
<div user-id="25" product-name="Laptop">
  Product
</div>
```

Recommended approach:

```html
<div data-user-id="25" data-product-name="Laptop">
  Product
</div>
```

The `data-*` convention makes custom attributes recognizable and accessible through standard DOM APIs.

# 🗁 Accessing Data Attributes with JavaScript

JavaScript provides the `dataset` property to read and modify data attributes.

The `dataset` property returns a `DOMStringMap` containing the element's data attributes.

### 🖆 Reading Data Attributes

```html
<div
  id="user"
  data-user-id="101"
  data-user-name="Muhammad"
>
  User Profile
</div>
```

```javascript
const user = document.getElementById("user");

console.log(user.dataset.userId);
console.log(user.dataset.userName);
```

Output:

```text
101
Muhammad
```

### 🖆 Understanding Name Conversion

HTML uses kebab-case for data attribute names, while JavaScript's `dataset` property uses camelCase.

| HTML Attribute | JavaScript Property |
| --- | --- |
| `data-user-id` | `dataset.userId` |
| `data-user-name` | `dataset.userName` |
| `data-product-price` | `dataset.productPrice` |
| `data-is-active` | `dataset.isActive` |
| `data-account-status` | `dataset.accountStatus` |

The conversion removes the hyphen and capitalizes the following lowercase letter.

### 🖆 Modifying Data Attributes

You can assign new values through `dataset`.

```html
<button id="save-button" data-status="pending">
  Save
</button>
```

```javascript
const button = document.getElementById("save-button");

button.dataset.status = "completed";

console.log(button.dataset.status);
```

Output:

```text
completed
```

The DOM attribute is updated as well:

```html
<button id="save-button" data-status="completed">
  Save
</button>
```

### 🖆 Adding Data Attributes

```javascript
const element = document.querySelector(".product");

element.dataset.category = "electronics";
element.dataset.price = "1200";
```

This creates:

```html
<div
  class="product"
  data-category="electronics"
  data-price="1200"
></div>
```

### 🖆 Removing Data Attributes

Use the `delete` operator to remove a data attribute through `dataset`.

```javascript
const element = document.querySelector(".product");

delete element.dataset.price;
```

You can also use `removeAttribute()`:

```javascript
element.removeAttribute("data-category");
```

# 🗁 Accessing Data Attributes with getAttribute()

You can also access data attributes using standard DOM attribute methods.

### 🖆 Reading an Attribute

```html
<div id="product" data-price="1200">
  Laptop
</div>
```

```javascript
const product = document.getElementById("product");

const price = product.getAttribute("data-price");

console.log(price);
```

### 🖆 Setting an Attribute

```javascript
product.setAttribute("data-price", "1500");
```

### 🖆 Removing an Attribute

```javascript
product.removeAttribute("data-price");
```

### 🖆 dataset vs getAttribute()

| Feature | `dataset` | `getAttribute()` |
| --- | --- | --- |
| Access style | JavaScript property | DOM method |
| Naming | camelCase | Original attribute name |
| Reading | `element.dataset.userId` | `element.getAttribute("data-user-id")` |
| Updating | Direct assignment | `setAttribute()` |
| Removing | `delete element.dataset.userId` | `removeAttribute()` |

Both approaches are valid. Use `dataset` when working primarily with `data-*` attributes.

# 🗁 Data Attribute Values Are Strings

One important detail: data attribute values are always exposed as strings through `dataset`.

### 🖆 Example

```html
<div
  id="product"
  data-price="1200"
  data-available="true"
  data-quantity="5"
>
  Product
</div>
```

```javascript
const product = document.getElementById("product");

console.log(typeof product.dataset.price);
console.log(typeof product.dataset.available);
console.log(typeof product.dataset.quantity);
```

Output:

```text
string
string
string
```

Even values that look like numbers or booleans remain strings.

### 🖆 Converting to Numbers

Use `Number()` when you need numeric calculations.

```javascript
const price = Number(product.dataset.price);
const quantity = Number(product.dataset.quantity);

const total = price * quantity;

console.log(total);
```

### 🖆 Converting to Booleans

Compare the string value explicitly.

```javascript
const isAvailable = product.dataset.available === "true";

console.log(isAvailable);
```

Avoid relying on `Boolean()` for string values:

```javascript
Boolean("false"); // true
```

Any non-empty string evaluates to `true`, including `"false"`.

### 🖆 Converting JSON Data

You can store JSON as a data attribute and parse it when needed.

```html
<div
  id="settings"
  data-config='{"theme":"dark","language":"en"}'
>
  Application Settings
</div>
```

```javascript
const settings = document.getElementById("settings");

const config = JSON.parse(settings.dataset.config);

console.log(config.theme);
console.log(config.language);
```

For complex application state, large datasets, or frequently changing data, a dedicated JavaScript data structure is generally more appropriate than storing everything in HTML attributes.

# 🗁 Using Data Attributes with CSS

CSS can select elements based on their data attributes using attribute selectors.

### 🖆 Select an Element with a Specific Attribute

```css
[data-status] {
  border: 1px solid gray;
}
```

This selects every element that has a `data-status` attribute.

### 🖆 Select an Exact Attribute Value

```html
<div data-status="active">Active User</div>
<div data-status="inactive">Inactive User</div>
```

```css
[data-status="active"] {
  color: green;
}

[data-status="inactive"] {
  color: red;
}
```

### 🖆 Select an Attribute Containing a Value

```css
[data-category~="featured"] {
  border: 2px solid blue;
}
```

The `~=` operator matches a whitespace-separated value in the attribute.

### 🖆 Other Common Attribute Selector Operators

| Selector | Meaning |
| --- | --- |
| `[data-status]` | Attribute exists |
| `[data-status="active"]` | Exact value match |
| `[data-status~="active"]` | Contains a whitespace-separated token |
| `[data-name^="user"]` | Value starts with `user` |
| `[data-name$="admin"]` | Value ends with `admin` |
| `[data-name*="profile"]` | Value contains `profile` |

# 🗁 Using Data Attributes for Interactive Components

Data attributes are useful for connecting HTML elements with JavaScript behavior.

### 🖆 Example: Toggle a Button State

```html
<button id="toggle-button" data-state="closed">
  Open
</button>

<script>
  const button = document.getElementById("toggle-button");

  button.addEventListener("click", () => {
    const isOpen = button.dataset.state === "open";

    button.dataset.state = isOpen ? "closed" : "open";

    button.textContent = isOpen ? "Open" : "Close";
  });
</script>
```

The button's `data-state` attribute stores its current state.

### 🖆 Example: Product Information

```html
<div class="product" data-product-id="101" data-price="1200">
  <h2>Laptop</h2>
  <p>Price: $1200</p>

  <button class="add-to-cart">
    Add to Cart
  </button>
</div>

<script>
  const buttons = document.querySelectorAll(".add-to-cart");

  buttons.forEach((button) => {
    button.addEventListener("click", () => {
      const product = button.closest(".product");

      const productId = product.dataset.productId;
      const price = Number(product.dataset.price);

      console.log({
        productId,
        price
      });
    });
  });
</script>
```

The JavaScript retrieves product information from the nearest product container.

This pattern is useful for small frontend interactions and demonstrations. In production applications, important product information such as price and availability must be validated by the backend rather than trusted from client-side attributes.

# 🗁 Common Use Cases

| Use Case | Example |
| --- | --- |
| Product identification | `data-product-id="101"` |
| User identification | `data-user-id="25"` |
| Component state | `data-state="open"` |
| Categories | `data-category="electronics"` |
| Configuration | `data-theme="dark"` |
| Filtering | `data-status="active"` |
| Sorting | `data-sort="price"` |
| Frontend behavior | `data-action="delete"` |

# 🗁 Data Attributes vs ID and Class

These attributes serve different purposes.

| Attribute | Main Purpose | Example |
| --- | --- | --- |
| `id` | Unique element identification | `id="main-header"` |
| `class` | Grouping and reusable styling | `class="card"` |
| `data-*` | Storing custom element data | `data-product-id="101"` |

Example:

```html
<div
  id="featured-product"
  class="product-card featured"
  data-product-id="101"
  data-price="1200"
>
  Laptop
</div>
```

Here:

* `id` uniquely identifies the element.
* `class` identifies its styling and component categories.
* `data-*` stores additional product information.

# 🗁 Best Practices

* Use descriptive names such as `data-user-id` and `data-product-status`.
* Follow lowercase kebab-case naming.
* Use `dataset` for convenient JavaScript access.
* Remember that `dataset` values are strings.
* Convert values explicitly before performing calculations.
* Keep data attributes focused on small, element-specific information.
* Use classes for styling and data attributes for application-specific information.
* Avoid storing passwords, authentication tokens, API secrets, or other sensitive information in HTML attributes.
* Never treat data attributes as a security boundary.
* Do not trust client-side data for authorization, payment calculations, or other sensitive operations.
* Prefer semantic HTML attributes when standard HTML already provides the required functionality.

# 🗁 Common Mistakes

### 🖆 Forgetting the data- Prefix

Incorrect:

```html
<div product-id="101"></div>
```

Correct:

```html
<div data-product-id="101"></div>
```

### 🖆 Assuming dataset Values Are Numbers

Incorrect:

```javascript
const price = product.dataset.price;

console.log(price + 100);
```

If `price` is `"1200"`, the result is `"1200100"` because JavaScript performs string concatenation.

Correct:

```javascript
const price = Number(product.dataset.price);

console.log(price + 100);
```

### 🖆 Treating the String "false" as Boolean False

Incorrect:

```javascript
const isActive = Boolean(element.dataset.active);
```

If the value is `"false"`, the result is still `true`.

Correct:

```javascript
const isActive = element.dataset.active === "true";
```

### 🖆 Storing Sensitive Information

Avoid:

```html
<div data-password="my-secret-password"></div>
```

HTML is accessible to users through browser developer tools and the DOM. Data attributes are not a secure storage mechanism.

# 🗁 Practice Exercises

* Create a product card with `data-product-id`, `data-price`, and `data-category`.
* Read product information using the `dataset` property.
* Update a product's data attributes using JavaScript.
* Remove a data attribute using `delete`.
* Create CSS styles using `[data-status="active"]`.
* Build a button that toggles its `data-state` between `open` and `closed`.
* Create multiple product cards and use JavaScript to retrieve the information from the clicked card.
* Store a small JSON configuration in a data attribute and parse it using `JSON.parse()`.

# 🗁 Interview Questions

**What are HTML data attributes?**

Data attributes are custom HTML attributes beginning with `data-`, used to store application-specific information on HTML elements.

**Why do we use data attributes?**

They allow developers to attach additional information to elements and access that information through JavaScript.

**What is the `dataset` property?**

`dataset` is a DOM property that provides access to an element's `data-*` attributes through a `DOMStringMap`.

**What is the difference between `data-user-id` and `dataset.userId`?**

`data-user-id` is the HTML attribute name, while `dataset.userId` is its JavaScript property representation.

**Are data attribute values strings or numbers?**

They are exposed as strings through `dataset`, even when the original value looks like a number or boolean.

**Can CSS select elements using data attributes?**

Yes. CSS attribute selectors can target elements based on the existence or value of their data attributes.

**What is the difference between `dataset` and `getAttribute()`?**

`dataset` provides convenient camelCase property access, while `getAttribute()` accesses an attribute using its original HTML name.

**Are data attributes secure for storing sensitive information?**

No. Data attributes are visible in the DOM and can be inspected or modified by users.

**Can we modify data attributes using JavaScript?**

Yes. You can use `dataset`, `setAttribute()`, and `removeAttribute()` to modify them.

# 🗁 Official References

* [MDN — data-* global attributes](https://developer.mozilla.org/en-US/docs/Web/HTML/Reference/Global_attributes/data-*)
* [MDN — HTMLElement.dataset](https://developer.mozilla.org/en-US/docs/Web/API/HTMLElement/dataset)
* [MDN — DOMStringMap](https://developer.mozilla.org/en-US/docs/Web/API/DOMStringMap)
* [MDN — CSS Attribute Selectors](https://developer.mozilla.org/en-US/docs/Web/CSS/Attribute_selectors)
* [WHATWG — HTML Living Standard](https://html.spec.whatwg.org/)
