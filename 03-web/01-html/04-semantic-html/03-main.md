# HTML Main Content (`<main>`)

The HTML `<main>` element represents the dominant content of the document's `<body>`.

It identifies the primary content that is directly related to the central purpose of a page.

Examples include:

* An article's main content
* A product details page
* A dashboard's primary content
* A blog post
* A search results page
* A documentation page

The `<main>` element provides a semantic landmark that helps browsers, search engines, and assistive technologies understand the page structure.

# 🗁 Basic Syntax

```html
<main>
  <h1>Welcome to My Website</h1>

  <p>
    This is the primary content of the page.
  </p>
</main>
```

### 🖆 Syntax Explanation

* `<main>` identifies the primary content of the document.
* The content inside `<main>` should represent the central topic or functionality of the page.
* `</main>` closes the main content section.

The `<main>` element should not be used as a generic container for every element on a page.

# 🗁 Understanding the Main Element

A typical webpage contains several types of content:

* Site header and branding
* Primary navigation
* Main page content
* Supporting information
* Footer content

The `<main>` element identifies the content that is unique to the current page.

For example, a website may use the same header, navigation, and footer across many pages, while the main content changes.

```html
<body>
  <header>
    <h1>My Website</h1>

    <nav aria-label="Main navigation">
      <a href="/">Home</a>
      <a href="/about">About</a>
      <a href="/contact">Contact</a>
    </nav>
  </header>

  <main>
    <h1>About Us</h1>

    <p>
      Learn more about our company, our mission,
      and the services we provide.
    </p>
  </main>

  <footer>
    <p>&copy; 2026 My Website</p>
  </footer>
</body>
```

# 🗁 Complete HTML Example

The following example demonstrates how `<main>` works with other semantic HTML elements.

```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>HTML Main Element</title>
</head>
<body>

  <header>
    <h1>Tech Blog</h1>

    <nav aria-label="Main navigation">
      <ul>
        <li><a href="/">Home</a></li>
        <li><a href="/articles">Articles</a></li>
        <li><a href="/about">About</a></li>
      </ul>
    </nav>
  </header>

  <main>
    <article>
      <h2>Understanding HTML</h2>

      <p>
        HTML is the standard markup language used
        to structure content on the web.
      </p>
    </article>

    <section>
      <h2>Related Topics</h2>

      <ul>
        <li><a href="/html-elements">HTML Elements</a></li>
        <li><a href="/html-attributes">HTML Attributes</a></li>
        <li><a href="/semantic-html">Semantic HTML</a></li>
      </ul>
    </section>
  </main>

  <footer>
    <p>&copy; 2026 Tech Blog</p>
  </footer>

</body>
</html>
```

### 🖆 Understanding the Structure

| Element | Responsibility |
| --- | --- |
| `<header>` | Introduces the website or page |
| `<nav>` | Provides major navigation links |
| `<main>` | Contains the primary page content |
| `<article>` | Represents self-contained content |
| `<section>` | Groups related content |
| `<footer>` | Contains footer information |

# 🗁 Main Element and Other Semantic Elements

The `<main>` element works alongside other semantic elements, but each has a different purpose.

| Element | Purpose |
| --- | --- |
| `<header>` | Introductory content |
| `<nav>` | Major navigation links |
| `<main>` | Primary content of the document |
| `<section>` | Thematic grouping of content |
| `<article>` | Independent, self-contained content |
| `<aside>` | Complementary or supporting content |
| `<footer>` | Footer information |

### 🖆 Example: Main Content with Sidebar

```html
<body>
  <header>
    <h1>Programming Blog</h1>
  </header>

  <nav aria-label="Main navigation">
    <a href="/">Home</a>
    <a href="/tutorials">Tutorials</a>
    <a href="/about">About</a>
  </nav>

  <main>
    <article>
      <h1>Learning JavaScript</h1>

      <p>
        JavaScript is a programming language
        used to create interactive web applications.
      </p>
    </article>

    <aside>
      <h2>Related Articles</h2>
      <ul>
        <li><a href="/html">HTML Guide</a></li>
        <li><a href="/css">CSS Guide</a></li>
      </ul>
    </aside>
  </main>

  <footer>
    <p>Copyright 2026</p>
  </footer>
</body>
```

The `<main>` element can contain other semantic elements, including `<article>`, `<section>`, and `<aside>`.

# 🗁 Multiple Main Elements

A document should have only one active `<main>` element.

The HTML standard allows multiple `<main>` elements in a document when all but one are hidden using the `hidden` attribute.

However, the recommended approach is to use one visible `<main>` element for the page's primary content.

```html
<body>
  <main>
    <h1>Home Page</h1>
    <p>Welcome to the website.</p>
  </main>
</body>
```

Avoid placing multiple visible `<main>` elements in the same document.

# 🗁 Main Element with Hidden Content

The `hidden` attribute can be used to hide an element from presentation and normal interaction.

```html
<main>
  <h1>Dashboard</h1>
  <p>Welcome to your dashboard.</p>
</main>

<main hidden>
  <h1>Settings</h1>
  <p>Dashboard settings content.</p>
</main>
```

Only the visible main content should represent the active primary content.

For application interfaces that switch between views, ensure the visibility state is managed correctly so that users and assistive technologies encounter the intended content.

# 🗁 Accessibility

The `<main>` element provides an important accessibility landmark.

Screen reader users can use landmarks to navigate directly to the primary content without repeatedly moving through the header and navigation.

### 🖆 Use One Main Landmark

A typical document should contain one visible `<main>` element.

```html
<main>
  <h1>Documentation</h1>
  <p>Learn how to build web applications.</p>
</main>
```

### 🖆 Include a Skip Navigation Link

A skip link allows keyboard users to bypass repeated navigation and move directly to the main content.

```html
<body>
  <a href="#main-content" class="skip-link">
    Skip to main content
  </a>

  <header>
    <h1>My Website</h1>

    <nav aria-label="Main navigation">
      <a href="/">Home</a>
      <a href="/about">About</a>
    </nav>
  </header>

  <main id="main-content">
    <h1>Welcome</h1>
    <p>This is the primary content.</p>
  </main>
</body>
```

Example CSS:

```css
.skip-link {
  position: absolute;
  top: -100px;
  left: 1rem;
  padding: 0.75rem 1rem;
  background: #000;
  color: #fff;
  z-index: 1000;
}

.skip-link:focus {
  top: 1rem;
}
```

### 🖆 Use Meaningful Headings

Provide a clear heading structure inside `<main>`.

```html
<main>
  <h1>Web Development</h1>

  <section>
    <h2>HTML</h2>
    <p>HTML provides the structure of web pages.</p>
  </section>

  <section>
    <h2>CSS</h2>
    <p>CSS controls the presentation of web pages.</p>
  </section>
</main>
```

A descriptive heading helps users understand the purpose of the page's primary content.

### 🖆 Avoid Unnecessary ARIA

The `<main>` element already provides main landmark semantics.

```html
<!-- Unnecessary -->
<main role="main">
  <h1>Welcome</h1>
</main>

<!-- Preferred -->
<main>
  <h1>Welcome</h1>
</main>
```

# 🗁 Best Practices

* Use `<main>` for the primary content of the document.
* Keep one visible `<main>` element per page.
* Place shared header, primary navigation, and footer content outside `<main>` when they are page-level elements.
* Use semantic elements inside `<main>` to organize content.
* Provide a meaningful heading for the main content.
* Add a skip navigation link for keyboard users.
* Avoid using `<main>` as a generic layout container.
* Do not use `role="main"` unnecessarily.
* Ensure hidden application views are managed correctly.

# 🗁 Common Mistakes

### 🖆 Using Multiple Visible Main Elements

```html
<!-- Avoid -->
<main>
  <h1>Home</h1>
</main>

<main>
  <h1>About</h1>
</main>
```

Use one visible main landmark for the active document content.

### 🖆 Placing the Entire Website Inside Main

```html
<!-- Avoid -->
<main>
  <header>
    <h1>My Website</h1>
  </header>

  <nav>
    <a href="/">Home</a>
  </nav>

  <p>Main content</p>

  <footer>
    <p>Copyright</p>
  </footer>
</main>
```

Place shared page-level header, navigation, and footer content outside `<main>`.

### 🖆 Using Main as a Generic Container

```html
<!-- Avoid -->
<main class="container">
  <div class="card">
    <h2>Product</h2>
  </div>
</main>
```

The `<main>` element should represent the document's primary content, not merely a visual layout wrapper. A `<div>` is more appropriate for a generic container.

### 🖆 Missing Skip Navigation

Websites with repeated navigation can make keyboard users traverse many links before reaching the primary content.

Provide a skip link when it improves navigation efficiency.

# 🗁 Practical Exercises

### 🖆 Exercise: Create a Blog Page

Build a blog page containing:

* A website header
* Primary navigation
* A main content area
* An article
* A related topics section
* A footer

Use semantic HTML elements appropriately.

### 🖆 Exercise: Create a Dashboard

Build a dashboard layout with:

* A header
* Sidebar navigation
* Main dashboard content
* Supporting information
* A footer

Ensure the primary dashboard content is inside `<main>`.

### 🖆 Exercise: Add Skip Navigation

Create a skip link that moves keyboard users directly to the main content.

Use an `id` on the `<main>` element and an anchor targeting that ID.

# 🗁 Interview Questions

**What is the purpose of the `<main>` element?**

It represents the dominant content of the document's `<body>`.

**How many visible `<main>` elements should a page contain?**

A typical document should contain one visible `<main>` element.

**Can `<main>` contain `<article>` and `<section>` elements?**

Yes. These elements can organize the primary content inside `<main>`.

**What is the difference between `<main>` and `<body>`?**

`<body>` contains all visible document content, while `<main>` identifies the primary content within the body.

**Can `<main>` be placed inside `<header>` or `<footer>`?**

No. A page-level `<main>` element should not be a descendant of `<header>`, `<footer>`, or `<nav>`.

**What is a main landmark?**

It is an accessibility landmark that identifies the primary content area of a page.

**Why is a skip navigation link useful?**

It allows keyboard users to bypass repeated navigation and reach the main content directly.

**Is `role="main"` required on `<main>`?**

No. The semantic `<main>` element already provides the main landmark role.

# 🗁 References

* [MDN: `<main>` Element](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/main)
* [WHATWG: The `main` Element](https://html.spec.whatwg.org/multipage/grouping-content.html#the-main-element)
* [W3C WAI: Landmarks](https://www.w3.org/WAI/ARIA/apg/practices/landmark-regions/)
* [W3C WAI: Bypass Blocks](https://www.w3.org/WAI/WCAG22/Understanding/bypass-blocks.html)
