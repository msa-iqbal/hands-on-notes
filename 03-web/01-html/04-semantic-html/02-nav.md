# HTML Navigation (`<nav>`)

The HTML `<nav>` element represents a section of a page that contains major navigation links. It helps browsers, search engines, and assistive technologies understand the structure and purpose of navigation areas.

Common examples include:

* Main website navigation
* Sidebar navigation
* Breadcrumb navigation
* Table of contents
* Pagination links

The `<nav>` element is part of semantic HTML and provides a navigation landmark for assistive technologies.

# 🗁 Basic Syntax

```html
<nav>
  <a href="/">Home</a>
  <a href="/about">About</a>
  <a href="/services">Services</a>
  <a href="/contact">Contact</a>
</nav>
```

### 🖆 Syntax Explanation

* `<nav>` defines a navigation section.
* `<a>` creates a hyperlink.
* `href` specifies the destination of a link.
* `</nav>` closes the navigation section.

Not every group of links needs a `<nav>` element. Use it for significant navigation sections rather than wrapping every collection of links.

# 🗁 Main Navigation

Main navigation typically contains links to the primary sections of a website.

```html
<header>
  <h1>My Website</h1>

  <nav aria-label="Main navigation">
    <ul>
      <li><a href="/">Home</a></li>
      <li><a href="/about">About</a></li>
      <li><a href="/services">Services</a></li>
      <li><a href="/contact">Contact</a></li>
    </ul>
  </nav>
</header>
```

### 🖆 Why Use `<nav>` Inside `<header>`?

The `<header>` element represents introductory content for a page or section. It can contain a logo, heading, search form, and navigation.

The `<nav>` element specifically identifies the navigation section.

These elements have different semantic purposes and can be used together.

# 🗁 Navigation with Lists

Using an unordered list inside `<nav>` is a common pattern for organizing navigation links.

```html
<nav aria-label="Main navigation">
  <ul>
    <li>
      <a href="/">Home</a>
    </li>

    <li>
      <a href="/products">Products</a>
    </li>

    <li>
      <a href="/blog">Blog</a>
    </li>

    <li>
      <a href="/contact">Contact</a>
    </li>
  </ul>
</nav>
```

### 🖆 Why Use Lists?

Lists provide a meaningful structure for related navigation items.

Benefits include:

* Clear grouping of navigation links.
* Better document organization.
* Easier styling with CSS.
* More predictable structure for assistive technologies.

A list is not mandatory. Simple navigation links can also be placed directly inside `<nav>`.

# 🗁 Multiple Navigation Landmarks

A website can contain multiple navigation sections.

For example:

* Main navigation
* Footer navigation
* Breadcrumb navigation
* Table of contents

When multiple navigation landmarks exist, give each one a unique accessible name so users of assistive technologies can distinguish them.

```html
<header>
  <nav aria-label="Main navigation">
    <a href="/">Home</a>
    <a href="/about">About</a>
    <a href="/services">Services</a>
  </nav>
</header>

<main>
  <h1>About Us</h1>
  <p>Learn more about our organization.</p>
</main>

<footer>
  <nav aria-label="Footer navigation">
    <a href="/privacy">Privacy Policy</a>
    <a href="/terms">Terms of Service</a>
  </nav>
</footer>
```

### 🖆 Using `aria-label`

The `aria-label` attribute provides an accessible name for a navigation landmark.

```html
<nav aria-label="Main navigation">
  <!-- Main navigation links -->
</nav>

<nav aria-label="Footer navigation">
  <!-- Footer navigation links -->
</nav>
```

If a page has only one navigation landmark, an accessible name is usually unnecessary.

You can also use `aria-labelledby` to reference an existing heading or label.

```html
<h2 id="contents-heading">Table of Contents</h2>

<nav aria-labelledby="contents-heading">
  <ul>
    <li><a href="#introduction">Introduction</a></li>
    <li><a href="#features">Features</a></li>
    <li><a href="#conclusion">Conclusion</a></li>
  </ul>
</nav>
```

# 🗁 Breadcrumb Navigation

Breadcrumbs show the user's current location within a website's hierarchy.

Example:

Home → Documentation → HTML → Navigation

```html
<nav aria-label="Breadcrumb">
  <ol>
    <li><a href="/">Home</a></li>
    <li><a href="/docs">Documentation</a></li>
    <li><a href="/docs/html">HTML</a></li>
    <li>
      <span aria-current="page">Navigation</span>
    </li>
  </ol>
</nav>
```

### 🖆 Understanding `aria-current`

The `aria-current="page"` attribute identifies the current page within a set of navigation links.

For the current page, use a non-link element when navigation to that same page is unnecessary.

You can also use an anchor with `aria-current="page"` when the current item needs to remain a link.

# 🗁 Pagination Navigation

Pagination allows users to navigate between multiple pages of content.

```html
<nav aria-label="Pagination">
  <ul>
    <li>
      <a href="/articles?page=1">Previous</a>
    </li>

    <li>
      <a href="/articles?page=1">1</a>
    </li>

    <li>
      <a href="/articles?page=2" aria-current="page">2</a>
    </li>

    <li>
      <a href="/articles?page=3">3</a>
    </li>

    <li>
      <a href="/articles?page=3">Next</a>
    </li>
  </ul>
</nav>
```

### 🖆 Important Considerations

* Use `aria-label="Pagination"` to identify the navigation landmark.
* Use `aria-current="page"` for the active page.
* Use meaningful link text.
* Do not use `href="#"` as a substitute for a disabled link.
* When a previous or next page is unavailable, render non-interactive text or another appropriate disabled representation.

# 🗁 Internal Page Navigation

The `<nav>` element can also provide navigation to specific sections of the current page.

```html
<nav aria-label="On this page">
  <ul>
    <li><a href="#introduction">Introduction</a></li>
    <li><a href="#examples">Examples</a></li>
    <li><a href="#best-practices">Best Practices</a></li>
  </ul>
</nav>

<main>
  <section id="introduction">
    <h2>Introduction</h2>
    <p>Introduction content goes here.</p>
  </section>

  <section id="examples">
    <h2>Examples</h2>
    <p>Example content goes here.</p>
  </section>

  <section id="best-practices">
    <h2>Best Practices</h2>
    <p>Best practices content goes here.</p>
  </section>
</main>
```

The fragment identifier in each link must match the `id` of its target element.

# 🗁 Styling Navigation with CSS

HTML provides the semantic structure. CSS controls the visual presentation.

```html
<nav class="main-navigation" aria-label="Main navigation">
  <ul>
    <li><a href="/">Home</a></li>
    <li><a href="/about">About</a></li>
    <li><a href="/services">Services</a></li>
    <li><a href="/contact">Contact</a></li>
  </ul>
</nav>
```

```css
.main-navigation ul {
  display: flex;
  gap: 1.5rem;
  list-style: none;
  margin: 0;
  padding: 0;
}

.main-navigation a {
  color: #333;
  text-decoration: none;
}

.main-navigation a:hover {
  text-decoration: underline;
}

.main-navigation a:focus-visible {
  outline: 2px solid currentColor;
  outline-offset: 4px;
}
```

The navigation remains usable without CSS because its HTML structure contains real links.

# 🗁 Accessibility

Accessible navigation helps users understand and move through a website.

### 🖆 Use Semantic Navigation

Use `<nav>` for significant navigation sections.

```html
<nav aria-label="Main navigation">
  <a href="/">Home</a>
  <a href="/about">About</a>
</nav>
```

### 🖆 Provide Accessible Names When Needed

When multiple navigation landmarks exist, distinguish them with `aria-label` or `aria-labelledby`.

```html
<nav aria-label="Main navigation">
  <!-- Links -->
</nav>

<nav aria-label="Footer navigation">
  <!-- Links -->
</nav>
```

### 🖆 Use Meaningful Link Text

Avoid vague link text when the destination is not clear from its context.

```html
<!-- Avoid -->
<a href="/documentation">Click here</a>

<!-- Better -->
<a href="/documentation">Read the documentation</a>
```

### 🖆 Support Keyboard Navigation

Native anchor elements are keyboard accessible.

Users should be able to:

* Navigate between links using the Tab key.
* Activate links using Enter.
* See a visible focus indicator.

Do not remove focus outlines without providing an accessible replacement.

### 🖆 Avoid Unnecessary ARIA

The `<nav>` element already exposes navigation landmark semantics.

```html
<!-- Unnecessary -->
<nav role="navigation">
  <!-- Links -->
</nav>

<!-- Preferred -->
<nav aria-label="Main navigation">
  <!-- Links -->
</nav>
```

# 🗁 `<nav>` vs Other Semantic Elements

| Element | Purpose |
| --- | --- |
| `<nav>` | Major navigation links |
| `<header>` | Introductory content or section header |
| `<main>` | Main content of the document |
| `<footer>` | Footer content for a page or section |
| `<menu>` | A list of commands or menu items |

A `<header>` may contain a `<nav>`, but they are not interchangeable.

The `<menu>` element is not a general replacement for website navigation.

# 🗁 Best Practices

* Use `<nav>` for major navigation sections.
* Use meaningful anchor text.
* Use valid URLs in `href`.
* Use lists when they improve the structure of navigation items.
* Give multiple navigation landmarks distinct accessible names.
* Use `aria-current="page"` to identify the current page.
* Keep navigation understandable without JavaScript.
* Ensure links have visible keyboard focus indicators.
* Use CSS to control presentation without changing semantic meaning.
* Avoid unnecessary ARIA attributes.

# 🗁 Common Mistakes

### 🖆 Using `<nav>` for Every Group of Links

Not every collection of links is a navigation landmark.

Use `<nav>` when the links form a significant navigation section.

### 🖆 Using `href="#"`

```html
<!-- Avoid -->
<a href="#">Products</a>
```

This creates a link that points to the top of the current page rather than a meaningful destination.

Use a valid URL or a button when the intended action is not navigation.

### 🖆 Using `<nav>` as a Generic Container

```html
<!-- Avoid -->
<nav>
  <div class="card">
    <h2>Welcome</h2>
    <p>Welcome to our website.</p>
  </div>
</nav>
```

Use `<div>` or another appropriate semantic element for non-navigation content.

### 🖆 Missing Accessible Names on Multiple Navigation Landmarks

If a page has several navigation landmarks, users may struggle to distinguish them when they have identical or missing names.

Provide meaningful accessible names.

### 🖆 Removing Keyboard Focus Indicators

```css
/* Avoid */
a {
  outline: none;
}
```

Removing focus indicators makes keyboard navigation difficult.

Provide a visible `:focus-visible` style instead.

# 🗁 Practical Exercises

### 🖆 Exercise: Build Main Navigation

Create a navigation bar containing:

* Home
* About
* Services
* Blog
* Contact

Use `<nav>`, `<ul>`, `<li>`, and `<a>`.

### 🖆 Exercise: Build Breadcrumb Navigation

Create breadcrumbs for:

Home → Products → Electronics → Smartphones

Use `aria-label="Breadcrumb"` and identify the current page with `aria-current="page"`.

### 🖆 Exercise: Build Pagination

Create pagination containing:

* Previous
* Page 1
* Page 2
* Page 3
* Next

Mark the active page using `aria-current="page"`.

### 🖆 Exercise: Build a Documentation Table of Contents

Create an on-page navigation section linking to at least four sections of a documentation page.

Use fragment identifiers and matching element IDs.

# 🗁 Interview Questions

**What is the purpose of the `<nav>` element?**

It represents a section of a page containing major navigation links.

**Is `<nav>` required for every group of links?**

No. It should be reserved for significant navigation sections.

**Can a page contain multiple `<nav>` elements?**

Yes. Multiple navigation landmarks can be used for different navigation areas.

**Why use `aria-label` with `<nav>`?**

It provides an accessible name that helps assistive technology users distinguish navigation landmarks.

**What is the difference between `<nav>` and `<menu>`?**

`<nav>` represents a navigation section, while `<menu>` represents a list of commands or menu items.

**Can `<nav>` be placed inside `<header>`?**

Yes. A header often contains the primary navigation of a website.

**What does `aria-current="page"` do?**

It identifies the current page within a navigation set.

**Does `<nav>` automatically make navigation keyboard accessible?**

The landmark itself does not provide keyboard interaction, but native links inside it are keyboard accessible.

# 🗁 References

* [MDN: `<nav>` Element](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/nav)
* [WHATWG: The `nav` Element](https://html.spec.whatwg.org/multipage/sections.html#the-nav-element)
* [W3C WAI: Page Regions](https://www.w3.org/WAI/ARIA/apg/practices/landmark-regions/)
* [MDN: `aria-current`](https://developer.mozilla.org/en-US/docs/Web/Accessibility/ARIA/Reference/Attributes/aria-current)
