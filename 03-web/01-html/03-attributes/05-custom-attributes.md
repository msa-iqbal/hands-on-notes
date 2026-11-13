# HTML Custom Attributes

HTML custom attributes allow developers to associate additional information or behavior-related metadata with HTML elements.

HTML provides the standard `data-*` attribute mechanism for storing application-specific data. However, developers may also encounter custom attributes in component-based architectures, custom elements, and web frameworks.

Understanding the difference between standard HTML attributes and custom attributes is important when building maintainable and valid web applications.

This topic covers:

* Standard HTML attributes.
* The `data-*` attribute convention.
* Custom attributes in Web Components.
* Attribute access and manipulation with JavaScript.
* Custom attribute handling in frameworks.
* Best practices and common mistakes.

# 🗁 What Are Custom HTML Attributes?

Custom attributes are attributes introduced for application-specific purposes rather than defined as standard attributes for a particular HTML element.

For example:

```html
<div user-id="101">
  User Profile
</div>
```

Here, `user-id` is not a standard HTML attribute.

For ordinary application-specific data, the recommended approach is to use the `data-*` convention:

```html
<div data-user-id="101">
  User Profile
</div>
```

The `data-*` convention provides a standardized way to store custom data on HTML elements.

# 🗁 Standard Attributes vs Custom Attributes

HTML elements support standard attributes defined by HTML specifications.

Examples include:

```html
<input
  type="email"
  name="email"
  required
>

<a href="https://example.com">
  Visit Website
</a>
```

Custom attributes provide additional application-specific information.

```html
<div
  data-user-id="101"
  data-account-status="active"
>
  User Profile
</div>
```

### 🖆 Comparison

| Attribute Type | Example | Purpose |
| --- | --- | --- |
| Standard HTML | `type="email"` | Defines standard element behavior |
| Global HTML | `id="header"` | Identifies an element |
| Data attribute | `data-user-id="101"` | Stores application-specific data |
| Custom element attribute | `user-id="101"` | Used by a custom component when designed to support it |
| Framework-specific | `v-if="isVisible"` | Provides framework-specific behavior |

Not every non-standard attribute is automatically a valid or useful custom HTML attribute.

# 🗁 The `data-*` Convention

The `data-*` convention is the standard HTML mechanism for attaching custom data to elements.

### 🖆 Syntax

```html
<element data-custom-name="value">
  Content
</element>
```

### 🖆 Example

```html
<div
  id="user-profile"
  data-user-id="101"
  data-role="admin"
  data-account-status="active"
>
  User Profile
</div>
```

Each attribute stores application-specific information.

### 🖆 Accessing Data Attributes

Use JavaScript's `dataset` property.

```javascript
const profile = document.getElementById("user-profile");

console.log(profile.dataset.userId);
console.log(profile.dataset.role);
console.log(profile.dataset.accountStatus);
```

Output:

```text
101
admin
active
```

HTML uses kebab-case, while JavaScript exposes the corresponding properties in camelCase.

### 🖆 Updating Data Attributes

```javascript
const profile = document.getElementById("user-profile");

profile.dataset.accountStatus = "inactive";
profile.dataset.role = "user";
```

### 🖆 Removing Data Attributes

```javascript
delete profile.dataset.role;
```

Use `data-*` attributes when you need to attach small amounts of information to individual elements.

For a complete explanation, see `03-data-attributes.md`.

# 🗁 Custom Attributes in Web Components

Web Components allow developers to create reusable HTML elements with custom behavior.

For example:

```html
<user-profile
  user-id="101"
  username="Muhammad"
>
</user-profile>
```

Here, `user-profile` is a custom element, and `user-id` and `username` are attributes supported by the component's implementation.

Unlike arbitrary attributes on ordinary HTML elements, these attributes have meaning because the custom element explicitly defines how to process them.

### 🖆 Creating a Custom Element

A custom element can be defined using the `customElements.define()` API.

```html
<user-profile username="Muhammad"></user-profile>

<script>
  class UserProfile extends HTMLElement {
    connectedCallback() {
      const username = this.getAttribute("username");

      this.textContent = `Welcome, ${username}!`;
    }
  }

  customElements.define("user-profile", UserProfile);
</script>
```

Output:

```text
Welcome, Muhammad!
```

The browser creates an instance of `UserProfile` when it encounters the custom element.

### 🖆 Custom Element Naming Rules

Custom element names must:

* Contain a hyphen.
* Start with a lowercase ASCII letter.
* Follow the custom element naming requirements defined by HTML.
* Not use names reserved by the specification.

Valid examples:

```html
<user-profile></user-profile>

<product-card></product-card>

<app-navigation></app-navigation>
```

Invalid example:

```html
<userprofile></userprofile>
```

The hyphen distinguishes autonomous custom element names from standard HTML element names.

# 🗁 Observing Custom Element Attributes

Custom elements can observe specific attributes and respond when their values change.

The `observedAttributes` static getter specifies which attributes should be monitored.

### 🖆 Example

```html
<user-profile username="Muhammad"></user-profile>

<script>
  class UserProfile extends HTMLElement {
    static get observedAttributes() {
      return ["username"];
    }

    connectedCallback() {
      this.render();
    }

    attributeChangedCallback(name, oldValue, newValue) {
      if (name === "username" && oldValue !== newValue) {
        this.render();
      }
    }

    render() {
      const username = this.getAttribute("username") || "Guest";

      this.textContent = `Welcome, ${username}!`;
    }
  }

  customElements.define("user-profile", UserProfile);

  const profile = document.querySelector("user-profile");

  profile.setAttribute("username", "John");
</script>
```

When the `username` attribute changes, the browser invokes `attributeChangedCallback()`.

### 🖆 Custom Element Lifecycle Methods

| Lifecycle Method | Purpose |
| --- | --- |
| `connectedCallback()` | Called when the element is connected to the document |
| `disconnectedCallback()` | Called when the element is disconnected |
| `attributeChangedCallback()` | Called when an observed attribute changes |
| `adoptedCallback()` | Called when the element is adopted into another document |

These lifecycle callbacks help custom elements manage their behavior and state.

# 🗁 Attribute Methods in JavaScript

The DOM provides standard methods for working with attributes, including custom attributes.

### 🖆 getAttribute()

Reads an attribute value.

```javascript
const element = document.querySelector("user-profile");

const username = element.getAttribute("username");

console.log(username);
```

### 🖆 setAttribute()

Creates or updates an attribute.

```javascript
element.setAttribute("username", "John");
```

### 🖆 removeAttribute()

Removes an attribute.

```javascript
element.removeAttribute("username");
```

### 🖆 hasAttribute()

Checks whether an attribute exists.

```javascript
if (element.hasAttribute("username")) {
  console.log("Username attribute exists.");
}
```

### 🖆 attributes

The `attributes` property provides access to an element's attribute collection.

```javascript
const element = document.querySelector("user-profile");

for (const attribute of element.attributes) {
  console.log(attribute.name, attribute.value);
}
```

These methods work with standard attributes, data attributes, and custom attributes.

# 🗁 Custom Attributes in JavaScript Frameworks

Modern frontend frameworks may introduce their own conventions for handling attributes and component properties.

These conventions are framework-specific and should not be confused with standard HTML attributes.

### 🖆 Vue.js Example

Vue supports custom attributes and component props.

```html
<template>
  <user-profile
    username="Muhammad"
    data-user-id="101"
  />
</template>
```

A component can explicitly define props:

```javascript
export default {
  props: {
    username: {
      type: String,
      required: true
    }
  }
};
```

Here:

* `username` is a component prop.
* `data-user-id` is a standard HTML data attribute when applied to an appropriate rendered HTML element.

### 🖆 React Example

React uses component props and DOM attributes.

```jsx
function UserProfile({ username }) {
  return (
    <div data-username={username}>
      Welcome, {username}!
    </div>
  );
}
```

Usage:

```jsx
<UserProfile username="Muhammad" />
```

The `username` value is a React component prop, while `data-username` becomes an HTML data attribute on the rendered `div`.

**Important:** Framework component props are not automatically HTML attributes. Their behavior depends on the framework and component implementation.

# 🗁 Custom Attributes and Accessibility

Custom attributes should not replace standard HTML semantics or ARIA attributes.

For example, avoid inventing attributes to describe accessibility states:

Incorrect:

```html
<button custom-expanded="false">
  Open Menu
</button>
```

Correct:

```html
<button aria-expanded="false">
  Open Menu
</button>
```

Use native HTML attributes when they already provide the required functionality.

Use ARIA attributes for supported accessibility semantics and states.

Use `data-*` attributes for application-specific information.

# 🗁 Custom Attributes vs Data Attributes vs Custom Elements

These concepts are related but serve different purposes.

| Concept | Example | Purpose |
| --- | --- | --- |
| Custom attribute | `user-id="101"` | Application-specific attribute |
| Data attribute | `data-user-id="101"` | Standard mechanism for storing custom data |
| Custom element | `<user-profile>` | Developer-defined HTML element |
| Component prop | `username="Muhammad"` | Input passed to a framework or component |

### 🖆 Example Combining These Concepts

```html
<user-profile
  username="Muhammad"
  data-user-id="101"
  data-role="developer"
>
</user-profile>
```

In this example:

* `user-profile` is a custom element.
* `username` is an attribute consumed by the component.
* `data-user-id` and `data-role` are standard data attributes.

# 🗁 Best Practices

* Prefer standard HTML attributes whenever available.
* Use `data-*` for application-specific information on ordinary HTML elements.
* Use descriptive attribute names such as `data-user-id`.
* Avoid inventing attributes on standard elements when `data-*` is more appropriate.
* Define and document the attributes supported by custom elements.
* Use custom element names containing a hyphen.
* Use framework props according to the framework's conventions.
* Use native HTML semantics before introducing custom behavior.
* Use ARIA attributes for appropriate accessibility states and properties.
* Do not store sensitive information in HTML attributes.
* Do not rely on custom attributes as a security mechanism.
* Validate important data on the server rather than trusting browser-side attributes.

# 🗁 Common Mistakes

### 🖆 Using Arbitrary Attributes for Application Data

Avoid:

```html
<div user-id="101"></div>
```

Prefer:

```html
<div data-user-id="101"></div>
```

### 🖆 Treating Component Props as Standard HTML Attributes

A framework prop may be processed by the component and never appear in the rendered HTML.

For example:

```jsx
<UserProfile username="Muhammad" />
```

The `username` value is a component input, not necessarily an HTML attribute on the final DOM element.

### 🖆 Using Custom Attributes Instead of ARIA

Avoid:

```html
<button menu-open="true">
  Menu
</button>
```

Prefer:

```html
<button aria-expanded="true">
  Menu
</button>
```

### 🖆 Forgetting to Define Custom Element Behavior

Declaring a custom element does not automatically create functionality.

```html
<product-card product-id="101"></product-card>
```

The browser needs a registered custom element definition to provide the intended component behavior.

# 🗁 Practical Example

The following example demonstrates a custom HTML element that reads and responds to its attributes.

```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">

  <title>Custom HTML Element</title>

  <style>
    user-profile {
      display: block;
      padding: 20px;
      border: 1px solid #ddd;
      border-radius: 8px;
      margin-bottom: 12px;
    }
  </style>
</head>
<body>

  <h1>User Profiles</h1>

  <user-profile
    username="Muhammad"
    data-role="developer"
  ></user-profile>

  <user-profile
    username="John"
    data-role="designer"
  ></user-profile>

  <script>
    class UserProfile extends HTMLElement {
      static get observedAttributes() {
        return ["username"];
      }

      connectedCallback() {
        this.render();
      }

      attributeChangedCallback() {
        if (this.isConnected) {
          this.render();
        }
      }

      render() {
        const username = this.getAttribute("username") || "Guest";
        const role = this.dataset.role || "Member";

        this.textContent = `${username} — ${role}`;
      }
    }

    customElements.define("user-profile", UserProfile);

    const profile = document.querySelector("user-profile");

    profile.setAttribute("username", "Alex");
  </script>

</body>
</html>
```

**How it works:**

* `user-profile` defines a custom HTML element.
* `username` is a component-specific attribute.
* `data-role` stores additional application-specific information.
* `observedAttributes` monitors changes to the username.
* `connectedCallback()` initializes the component.
* `attributeChangedCallback()` updates the rendered content when the username changes.
* `dataset.role` reads the custom data attribute.

# 🗁 Practice Exercises

* Create a custom attribute named `data-product-id` on a product element.
* Read and update custom attributes using JavaScript.
* Create a custom element named `user-card`.
* Define a custom element that accepts a `username` attribute.
* Use `observedAttributes` to monitor changes to a component attribute.
* Build a product card component that reads `data-category`.
* Create a custom element with multiple instances and different attribute values.
* Compare a framework component prop with a native HTML data attribute.

# 🗁 Interview Questions

**What are custom HTML attributes?**

Custom attributes are application-specific attributes used to provide additional information or behavior to HTML elements.

**What is the recommended way to store custom data in HTML?**

Use the standard `data-*` attribute convention.

**What is the difference between custom attributes and data attributes?**

A custom attribute is a general application-specific attribute, while `data-*` is the standardized HTML mechanism for storing custom data.

**What are Web Components?**

Web Components are browser technologies for creating reusable custom HTML elements and encapsulated components.

**What is a custom element?**

A custom element is a developer-defined HTML element registered through the browser's Custom Elements API.

**Why must custom element names contain a hyphen?**

The hyphen distinguishes custom element names from existing and future standard HTML element names.

**What does `observedAttributes` do?**

It specifies which attributes a custom element monitors for changes.

**What is `attributeChangedCallback()`?**

It is a custom element lifecycle callback invoked when an observed attribute changes.

**What is the difference between an HTML attribute and a framework prop?**

An HTML attribute belongs to the DOM element, while a framework prop is an input passed to a component. A framework may use that prop without rendering it as an HTML attribute.

**Should custom attributes replace ARIA attributes?**

No. Use ARIA attributes for supported accessibility semantics and states, and `data-*` attributes for application-specific data.

# 🗁 Official References

* [MDN — data-* global attributes](https://developer.mozilla.org/en-US/docs/Web/HTML/Reference/Global_attributes/data-*)
* [MDN — Using Custom Elements](https://developer.mozilla.org/en-US/docs/Web/API/Web_components/Using_custom_elements)
* [MDN — CustomElementRegistry.define()](https://developer.mozilla.org/en-US/docs/Web/API/CustomElementRegistry/define)
* [MDN — Element.getAttribute()](https://developer.mozilla.org/en-US/docs/Web/API/Element/getAttribute)
* [MDN — Element.setAttribute()](https://developer.mozilla.org/en-US/docs/Web/API/Element/setAttribute)
* [MDN — Web Components](https://developer.mozilla.org/en-US/docs/Web/API/Web_components)
* [WHATWG — HTML Living Standard](https://html.spec.whatwg.org/)
