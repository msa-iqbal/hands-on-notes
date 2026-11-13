# HTML Landmarks

HTML landmarks are semantic regions of a webpage that help users navigate its structure, particularly users who rely on screen readers and other assistive technologies.

Landmarks identify important page areas such as:

* The page header
* Navigation menus
* Main content
* Complementary content
* Search areas
* The page footer

Screen readers can use landmarks to jump directly between these regions without requiring users to navigate through every element.

HTML provides native semantic elements that expose landmark roles to accessibility APIs. Using these elements correctly improves accessibility, document structure, and maintainability.

# 🗁 What Are Landmarks?

A landmark is an identifiable region of a webpage that represents an important area of its structure.

Landmarks are exposed through the browser's accessibility tree, allowing assistive technologies to recognize and navigate important page regions.

For example, a typical webpage may contain:

```html
<header>
  <!-- Site header -->
</header>

<nav aria-label="Primary">
  <!-- Main navigation -->
</nav>

<main>
  <!-- Main page content -->
</main>

<aside>
  <!-- Related information -->
</aside>

<footer>
  <!-- Site footer -->
</footer>
```

These semantic elements communicate the purpose of each region without requiring developers to manually assign ARIA roles.

### 🖆 Landmark Roles

Common landmark roles include:

| Landmark role | Purpose |
| --- | --- |
| `banner` | Site-wide header |
| `navigation` | Navigation links |
| `main` | Primary page content |
| `complementary` | Supporting content |
| `contentinfo` | Site-wide footer information |
| `search` | Search functionality |
| `form` | Important named form |
| `region` | Important named section |

# 🗁 Why Landmarks Matter

Landmarks improve the browsing experience for people using assistive technologies.

### 🖆 Screen Reader Navigation

Screen readers can provide landmark navigation commands that allow users to jump directly to important regions.

For example, a screen reader user can:

* Jump directly to the main content.
* Navigate between navigation regions.
* Find the search area.
* Locate complementary content.
* Move to the page footer.

### 🖆 Better Page Structure

Landmarks make the purpose of different page regions explicit.

Instead of relying entirely on generic containers such as `<div>`, developers can communicate the document's structure through semantic HTML.

### 🖆 Improved Maintainability

Semantic landmarks also make HTML easier for developers to understand, maintain, and debug.

# 🗁 HTML Elements That Create Landmarks

HTML provides several native elements that map to landmark roles.

### 🖆 `<header>` — Banner Landmark

A top-level `<header>` generally creates a `banner` landmark when it represents the site-wide header.

```html
<header>
  <a href="/">
    <img src="logo.svg" alt="Company Name">
  </a>

  <p>Welcome to our website</p>
</header>
```

Important distinction:

A `<header>` inside an `<article>`, `<section>`, or another sectioning context does not normally create a page-level `banner` landmark.

It represents the introductory content of its containing section.

### 🖆 `<nav>` — Navigation Landmark

The `<nav>` element identifies a major navigation section.

```html
<nav aria-label="Primary navigation">
  <ul>
    <li><a href="/">Home</a></li>
    <li><a href="/about">About</a></li>
    <li><a href="/services">Services</a></li>
    <li><a href="/contact">Contact</a></li>
  </ul>
</nav>
```

Use `<nav>` for important navigation groups rather than every collection of links.

### 🖆 `<main>` — Main Landmark

The `<main>` element identifies the dominant content of the document.

```html
<main>
  <h1>Welcome to Our Website</h1>

  <p>This is the primary content of the page.</p>
</main>
```

A document should generally have one visible `<main>` landmark.

The main content should exclude repeated content such as global navigation, site headers, and site footers.

### 🖆 `<aside>` — Complementary Landmark

The `<aside>` element represents content that is indirectly related to the surrounding content.

When it represents a complementary region at the appropriate document level, it maps to a `complementary` landmark.

```html
<aside>
  <h2>Related Articles</h2>

  <ul>
    <li><a href="/html-basics">HTML Basics</a></li>
    <li><a href="/css-basics">CSS Basics</a></li>
  </ul>
</aside>
```

Typical use cases include:

* Related articles
* Sidebars
* Supporting information
* Related resources

### 🖆 `<footer>` — Contentinfo Landmark

A top-level `<footer>` generally creates a `contentinfo` landmark when it represents information about the website or document as a whole.

```html
<footer>
  <p>&copy; 2026 Example Company</p>

  <a href="/privacy">Privacy Policy</a>
</footer>
```

A `<footer>` inside an `<article>` or another sectioning context represents the footer of that particular section. It does not automatically become the page-level `contentinfo` landmark.

### 🖆 `<form>` — Form Landmark

A `<form>` creates a `form` landmark when it has an accessible name.

```html
<form aria-labelledby="contact-heading">
  <h2 id="contact-heading">Contact Us</h2>

  <label for="email">Email</label>
  <input type="email" id="email" name="email">

  <button type="submit">Send</button>
</form>
```

An unnamed form does not automatically create a form landmark.

### 🖆 `<search>` — Search Landmark

The `<search>` element identifies a section containing search functionality.

```html
<search aria-label="Site search">
  <form action="/search" method="get">
    <label for="site-search">Search</label>
    <input type="search" id="site-search" name="q">

    <button type="submit">Search</button>
  </form>
</search>
```

The `<search>` element provides a native semantic search landmark.

# 🗁 Page-Level and Section-Level Landmarks

Not every semantic HTML element creates a landmark.

Understanding the difference helps prevent unnecessary or confusing accessibility structures.

| HTML element | Accessibility mapping |
| --- | --- |
| `<header>` | `banner` in the appropriate page-level context |
| `<nav>` | `navigation` |
| `<main>` | `main` |
| `<aside>` | `complementary` in the appropriate context |
| `<footer>` | `contentinfo` in the appropriate page-level context |
| `<search>` | `search` |
| Named `<form>` | `form` |
| Named `<section>` | `region` |
| `<article>` | `article`, not a landmark by itself |
| `<div>` | Generic container |
| `<span>` | Generic inline container |

### 🖆 Section Elements and Region Landmarks

A `<section>` represents a thematic grouping of content.

When it has an accessible name, it can expose a `region` landmark.

```html
<section aria-labelledby="features-heading">
  <h2 id="features-heading">Our Features</h2>

  <p>Explore the features available on our platform.</p>
</section>
```

An unnamed `<section>` does not automatically become a region landmark.

Do not turn every section into a landmark. Reserve region landmarks for important sections that users may benefit from navigating to directly.

### 🖆 Article Elements

An `<article>` represents self-contained content.

```html
<article>
  <h2>Understanding HTML Landmarks</h2>

  <p>HTML landmarks improve accessibility.</p>
</article>
```

An article is not automatically a landmark region. It has its own semantic role.

# 🗁 Naming Landmarks

When multiple landmarks have the same role, accessible names help users distinguish between them.

For example, a website might contain:

* Primary navigation
* Footer navigation
* Breadcrumb navigation

Without accessible names, users may hear several navigation landmarks with no clear distinction.

### 🖆 Using `aria-label`

Use `aria-label` to provide a concise accessible name.

```html
<nav aria-label="Primary navigation">
  <a href="/">Home</a>
  <a href="/about">About</a>
</nav>

<nav aria-label="Footer navigation">
  <a href="/privacy">Privacy</a>
  <a href="/terms">Terms</a>
</nav>
```

### 🖆 Using `aria-labelledby`

Use `aria-labelledby` when an existing visible heading or text element can name the landmark.

```html
<section aria-labelledby="news-heading">
  <h2 id="news-heading">Latest News</h2>

  <p>Read the latest updates.</p>
</section>
```

This avoids repeating the same accessible name in multiple places.

### 🖆 Avoid Redundant Names

Avoid names that repeat the landmark's role unnecessarily.

```html
<!-- Avoid -->
<nav aria-label="Navigation navigation">
  ...
</nav>

<!-- Better -->
<nav aria-label="Primary">
  ...
</nav>
```

The screen reader already communicates the landmark role.

# 🗁 Multiple Navigation Landmarks

A website may contain multiple navigation landmarks when each represents a distinct navigation area.

For example:

```html
<nav aria-label="Primary">
  <a href="/">Home</a>
  <a href="/products">Products</a>
  <a href="/contact">Contact</a>
</nav>

<nav aria-label="Breadcrumb">
  <a href="/">Home</a>
  <span aria-hidden="true">/</span>
  <a href="/products">Products</a>
  <span aria-hidden="true">/</span>
  <span>Details</span>
</nav>

<nav aria-label="Footer">
  <a href="/privacy">Privacy Policy</a>
  <a href="/terms">Terms of Service</a>
</nav>
```

Each navigation landmark has a meaningful accessible name.

Use distinct names when multiple landmarks have the same role and need to be differentiated.

# 🗁 Search and Form Landmarks

Search and important forms can provide additional navigation points.

### 🖆 Search Landmark Example

```html
<search aria-label="Product search">
  <form action="/products" method="get">
    <label for="product-search">Find a product</label>

    <input
      type="search"
      id="product-search"
      name="q"
    >

    <button type="submit">Search</button>
  </form>
</search>
```

### 🖆 Form Landmark Example

```html
<form aria-labelledby="registration-heading">
  <h2 id="registration-heading">Create an Account</h2>

  <label for="username">Username</label>
  <input
    type="text"
    id="username"
    name="username"
    autocomplete="username"
  >

  <label for="email">Email Address</label>
  <input
    type="email"
    id="email"
    name="email"
    autocomplete="email"
  >

  <button type="submit">Register</button>
</form>
```

A named form becomes easier to locate through landmark navigation.

# 🗁 Region Landmarks

A region landmark represents an important section of a page that users may want to navigate to independently.

A named `<section>` can create a region landmark.

```html
<section aria-labelledby="popular-heading">
  <h2 id="popular-heading">Popular Articles</h2>

  <ul>
    <li><a href="/article-1">Article One</a></li>
    <li><a href="/article-2">Article Two</a></li>
    <li><a href="/article-3">Article Three</a></li>
  </ul>
</section>
```

Use region landmarks when a section contains meaningful, independently navigable content.

Avoid assigning a region role to every visual grouping or container.

# 🗁 ARIA Landmark Roles

Native HTML elements should be preferred over manually assigning ARIA landmark roles.

ARIA roles can be useful when an appropriate native element cannot be used.

| ARIA role | Native HTML equivalent |
| --- | --- |
| `banner` | Appropriate page-level `<header>` |
| `navigation` | `<nav>` |
| `main` | `<main>` |
| `complementary` | Appropriate `<aside>` |
| `contentinfo` | Appropriate page-level `<footer>` |
| `search` | `<search>` |
| `form` | Named `<form>` |
| `region` | Named `<section>` |

### 🖆 Example Using ARIA Roles

```html
<div role="banner">
  <h1>Website Name</h1>
</div>

<div role="navigation" aria-label="Primary">
  <a href="/">Home</a>
  <a href="/about">About</a>
</div>

<div role="main">
  <h1>Welcome</h1>
  <p>Main page content.</p>
</div>

<div role="contentinfo">
  <p>Copyright information</p>
</div>
```

Although this example provides landmark roles, native semantic elements are generally better:

```html
<header>
  <h1>Website Name</h1>
</header>

<nav aria-label="Primary">
  <a href="/">Home</a>
  <a href="/about">About</a>
</nav>

<main>
  <h1>Welcome</h1>
  <p>Main page content.</p>
</main>

<footer>
  <p>Copyright information</p>
</footer>
```

Using native elements provides built-in semantics and reduces unnecessary ARIA usage.

# 🗁 Complete Page Example

The following example demonstrates a well-structured page with meaningful landmarks.

```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">

  <title>HTML Landmarks Example</title>
</head>
<body>

  <!-- Site-wide header -->
  <header>
    <a href="/">
      <img src="logo.svg" alt="Example Company">
    </a>

    <h1>Example Company</h1>
  </header>

  <!-- Primary navigation -->
  <nav aria-label="Primary">
    <ul>
      <li><a href="/">Home</a></li>
      <li><a href="/about">About</a></li>
      <li><a href="/services">Services</a></li>
      <li><a href="/contact">Contact</a></li>
    </ul>
  </nav>

  <!-- Search area -->
  <search aria-label="Site search">
    <form action="/search" method="get">
      <label for="search-input">Search</label>
      <input type="search" id="search-input" name="q">
      <button type="submit">Search</button>
    </form>
  </search>

  <!-- Primary page content -->
  <main>

    <h1>Welcome to Our Website</h1>

    <section aria-labelledby="about-heading">
      <h2 id="about-heading">About Us</h2>
      <p>Learn more about our company.</p>
    </section>

    <section aria-labelledby="services-heading">
      <h2 id="services-heading">Our Services</h2>
      <p>Explore the services we provide.</p>
    </section>

    <article>
      <header>
        <h2>Latest Article</h2>
        <p>Published on November 8, 2026</p>
      </header>

      <p>Explore our latest technical articles.</p>

      <footer>
        <p>Article author: Example Author</p>
      </footer>
    </article>

  </main>

  <!-- Complementary content -->
  <aside aria-labelledby="related-heading">
    <h2 id="related-heading">Related Resources</h2>

    <ul>
      <li><a href="/html">HTML Guide</a></li>
      <li><a href="/css">CSS Guide</a></li>
    </ul>
  </aside>

  <!-- Site-wide footer -->
  <footer>
    <p>&copy; 2026 Example Company</p>

    <nav aria-label="Footer">
      <a href="/privacy">Privacy Policy</a>
      <a href="/terms">Terms</a>
    </nav>
  </footer>

</body>
</html>
```

### 🖆 Important Observations

* The top-level `<header>` represents the site-wide banner.
* The primary `<nav>` identifies the main navigation.
* The `<search>` element identifies the search area.
* The `<main>` element identifies the primary page content.
* Named `<section>` elements can expose region landmarks.
* The `<article>` represents independent content, not a landmark by itself.
* The article's internal `<header>` and `<footer>` do not become page-level banner and contentinfo landmarks.
* The `<aside>` identifies complementary content.
* The top-level `<footer>` represents the site-wide content information.

# 🗁 Testing Landmarks

Testing helps ensure that semantic HTML is exposed correctly to assistive technologies.

### 🖆 Screen Reader Testing

Use screen reader landmark navigation to inspect the page structure.

Common tools include:

* NVDA on Windows
* JAWS on Windows
* VoiceOver on macOS and iOS
* TalkBack on Android

Check whether users can identify and navigate between the page's major regions.

### 🖆 Browser Accessibility Tree

Browser developer tools expose the accessibility tree, where you can inspect the roles and accessible names of HTML elements.

For example, verify that:

* `<main>` exposes the main landmark.
* `<nav>` exposes a navigation landmark.
* `<search>` exposes a search landmark.
* Named forms expose form landmarks.
* Named sections expose region landmarks.

### 🖆 Accessibility Testing Tools

Useful testing tools include:

* axe DevTools
* WAVE
* Lighthouse
* Browser accessibility inspectors

Automated tools can detect some accessibility problems, but manual testing is necessary to verify whether landmark organization is meaningful and usable.

# 🗁 Best Practices

* Use native semantic HTML elements whenever possible.
* Include one visible `<main>` landmark for the primary page content.
* Use `<header>` and `<footer>` appropriately for page-level and section-level content.
* Use `<nav>` for major navigation sections.
* Give multiple navigation landmarks distinct accessible names.
* Use `<search>` for search functionality.
* Provide accessible names for important forms.
* Use named sections only when they deserve independent navigation.
* Avoid excessive landmarks and unnecessary ARIA roles.
* Ensure landmark names describe their purpose.
* Keep the document structure logical and predictable.
* Test landmark navigation using actual screen readers.

# 🗁 Common Mistakes

### 🖆 Using `<div>` for Every Page Region

Incorrect:

```html
<div class="header">...</div>
<div class="navigation">...</div>
<div class="main">...</div>
<div class="footer">...</div>
```

Better:

```html
<header>...</header>
<nav aria-label="Primary">...</nav>
<main>...</main>
<footer>...</footer>
```

### 🖆 Creating Multiple Unnamed Navigation Landmarks

Incorrect:

```html
<nav>...</nav>
<nav>...</nav>
<nav>...</nav>
```

Better:

```html
<nav aria-label="Primary">...</nav>
<nav aria-label="Breadcrumb">...</nav>
<nav aria-label="Footer">...</nav>
```

### 🖆 Using Too Many Region Landmarks

Avoid assigning `role="region"` to every section or container.

Only important, independently navigable sections should become region landmarks.

### 🖆 Confusing Article Headers With Page Headers

A `<header>` inside an `<article>` is the article's introductory content. It does not automatically represent the site's banner landmark.

### 🖆 Adding ARIA Roles Unnecessarily

Avoid this:

```html
<main role="main">
  ...
</main>
```

The `<main>` element already provides the appropriate native semantics.

# 🗁 Practical Exercises

### 🖆 Exercise: Build a Semantic Page

Create a webpage containing:

* A site-wide header.
* A primary navigation menu.
* A search area.
* A main content area.
* Two named content sections.
* A complementary sidebar.
* A site-wide footer.

### 🖆 Exercise: Multiple Navigation Regions

Create three navigation landmarks:

* Primary navigation.
* Breadcrumb navigation.
* Footer navigation.

Give each an appropriate accessible name.

### 🖆 Exercise: Named Form Landmark

Create a registration form containing:

* A visible heading.
* Username input.
* Email input.
* Password input.
* Submit button.

Make sure the form has an accessible name.

### 🖆 Exercise: Inspect the Accessibility Tree

Open browser developer tools and inspect your webpage's accessibility tree.

Identify:

* Banner landmark.
* Navigation landmarks.
* Main landmark.
* Search landmark.
* Complementary landmark.
* Contentinfo landmark.
* Region landmarks.

# 🗁 Interview Questions

**What is an HTML landmark?**

A landmark is a semantic page region exposed through accessibility APIs, allowing assistive technology users to navigate important areas of a webpage.

**Why are landmarks important?**

They allow screen reader users to jump directly to important page regions without navigating through every element.

**Which HTML element creates the main landmark?**

The `<main>` element.

**Which HTML element creates a navigation landmark?**

The `<nav>` element.

**Does every `<section>` create a region landmark?**

No. A `<section>` generally needs an accessible name to expose a region landmark.

**Does an `<article>` automatically create a landmark?**

No. It has an article semantic role but is not automatically a landmark.

**Can a webpage contain multiple navigation landmarks?**

Yes. Each should have a meaningful accessible name when multiple navigation landmarks need to be distinguished.

**Does every `<form>` create a form landmark?**

No. A form needs an accessible name to expose a form landmark.

**What is the difference between `<header>` and the banner landmark?**

A `<header>` is an HTML element. It maps to the banner landmark when used in the appropriate page-level context, but a header inside an article or section does not automatically create a page-level banner.

**Should developers use ARIA roles instead of semantic HTML?**

No. Native semantic HTML should be preferred. ARIA roles are useful when an appropriate native element is unavailable.

# 🗁 References

* [MDN — HTML Elements Reference](https://developer.mozilla.org/en-US/docs/Web/HTML/Reference/Elements)
* [W3C WAI — ARIA Landmark Regions](https://www.w3.org/WAI/ARIA/apg/practices/landmark-regions/)
* [W3C WAI — Page Structure Tutorial](https://www.w3.org/WAI/tutorials/page-structure/)
* [W3C — HTML Accessibility API Mappings](https://www.w3.org/TR/html-aam/)
* [W3C — Accessible Rich Internet Applications (WAI-ARIA)](https://www.w3.org/TR/wai-aria/)
