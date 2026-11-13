# HTML Header Element

The HTML `<header>` element represents introductory content or a group of navigational and introductory elements for a page or a section.

It commonly contains:

* Website or section titles.
* Logos and branding.
* Navigation links.
* Search forms.
* Author information.
* Introductory content.

The `<header>` element is a semantic HTML element. It describes the purpose of its content rather than simply defining how that content looks.

# 🗁 What Is the Header Element?

The `<header>` element represents introductory content for its nearest relevant sectioning context or the page as a whole.

### 🖆 Syntax

```html
<header>
  <!-- Introductory content -->
</header>
```

### 🖆 Basic Example

```html
<header>
  <h1>My Website</h1>
  <p>Welcome to my website.</p>
</header>
```

The browser recognizes the element as a semantic header, making the document structure more meaningful.

# 🗁 Header for a Website

A page-level header usually contains the website's branding, title, and primary navigation.

### 🖆 Example

```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>My Website</title>
</head>
<body>

  <header>
    <h1>My Website</h1>

    <nav aria-label="Main navigation">
      <a href="/">Home</a>
      <a href="/about">About</a>
      <a href="/services">Services</a>
      <a href="/contact">Contact</a>
    </nav>
  </header>

  <main>
    <h2>Welcome</h2>
    <p>This is the main content of the website.</p>
  </main>

</body>
</html>
```

In this example:

* `<header>` contains the website's introductory content.
* `<h1>` provides the main page title.
* `<nav>` contains primary navigation links.
* `<main>` contains the primary content of the page.

# 🗁 Header for a Section

The `<header>` element can also be used inside sectioning content elements such as `<article>` and `<section>`.

This allows individual sections to have their own introductory content.

### 🖆 Example

```html
<article>
  <header>
    <h2>Understanding HTML</h2>
    <p>Published on November 8, 2026</p>
    <p>Written by Muhammad</p>
  </header>

  <p>
    HTML is the standard markup language used to structure
    content on the web.
  </p>
</article>
```

Here, the header introduces the article rather than the entire website.

### 🖆 Multiple Header Elements

A document can contain multiple `<header>` elements.

```html
<body>

  <header>
    <h1>Technology Blog</h1>
    <nav>
      <a href="/">Home</a>
      <a href="/blog">Blog</a>
    </nav>
  </header>

  <main>
    <article>
      <header>
        <h2>Introduction to CSS</h2>
        <p>Published on November 8, 2026</p>
      </header>

      <p>CSS controls the presentation of HTML documents.</p>
    </article>

    <article>
      <header>
        <h2>Introduction to JavaScript</h2>
        <p>Published on November 8, 2026</p>
      </header>

      <p>JavaScript adds interactivity to web applications.</p>
    </article>
  </main>

</body>
```

Each header introduces its respective content.

# 🗁 Header vs Head

The `<header>` and `<head>` elements have different purposes.

| Feature | `<header>` | `<head>` |
| --- | --- | --- |
| Purpose | Introductory page or section content | Document metadata and resource references |
| Visible content | Usually visible | Not displayed as normal page content |
| Location | Inside the `<body>` | Direct child of `<html>` |
| Common elements | Headings, logos, navigation | `title`, `meta`, `link`, `script`, `style` |
| Semantic role | Banner or section-level introduction | Document metadata container |

### 🖆 Example

```html
<!DOCTYPE html>
<html lang="en">

<head>
  <title>My Website</title>
  <meta charset="UTF-8">
</head>

<body>

  <header>
    <h1>My Website</h1>
    <p>Welcome to our website.</p>
  </header>

</body>
</html>
```

Remember:

* `<head>` contains document metadata.
* `<header>` contains introductory content.

# 🗁 Header vs Section

The `<header>` element introduces content, while `<section>` represents a thematic grouping of content.

### 🖆 Example

```html
<section>
  <header>
    <h2>Our Services</h2>
    <p>Explore the services we provide.</p>
  </header>

  <p>We provide web development and consulting services.</p>
</section>
```

The section groups related content, and its header introduces that content.

# 🗁 Header vs Div

Both elements can contain other HTML elements, but they communicate different meanings.

### 🖆 Using div

```html
<div class="header">
  <h1>My Website</h1>
  <nav>
    <a href="/">Home</a>
    <a href="/about">About</a>
  </nav>
</div>
```

The `<div>` element is a generic container without inherent semantic meaning.

### 🖆 Using header

```html
<header>
  <h1>My Website</h1>
  <nav>
    <a href="/">Home</a>
    <a href="/about">About</a>
  </nav>
</header>
```

The `<header>` element communicates that its content is introductory.

**Recommendation:** Use `<header>` when the content genuinely serves as an introductory header. Use `<div>` when no semantic HTML element appropriately describes the content.

# 🗁 Header and Accessibility

Semantic HTML helps browsers, assistive technologies, and developers understand the document structure.

A page-level `<header>` outside sectioning content generally exposes a banner landmark.

A `<header>` inside an `<article>`, `<aside>`, `<main>`, `<nav>`, or `<section>` does not create a separate banner landmark.

### 🖆 Example

```html
<body>

  <header>
    <h1>Developer Blog</h1>

    <nav aria-label="Main navigation">
      <a href="/">Home</a>
      <a href="/articles">Articles</a>
    </nav>
  </header>

  <main>
    <article>
      <header>
        <h2>Learning Semantic HTML</h2>
        <p>Published by the editorial team.</p>
      </header>

      <p>Semantic HTML improves document structure.</p>
    </article>
  </main>

</body>
```

The page-level header identifies the site's banner region, while the article header introduces the article.

### 🖆 Important Accessibility Practices

* Use meaningful headings inside headers.
* Use `<nav>` for groups of navigation links.
* Provide accessible names for navigation landmarks when multiple navigation regions exist.
* Keep the document's heading hierarchy logical.
* Avoid adding `role="banner"` to a normal page-level `<header>` unnecessarily because the semantic element already provides the appropriate role.

# 🗁 Styling the Header with CSS

The `<header>` element can be styled like any other HTML element.

### 🖆 Example

```html
<header class="site-header">
  <h1>My Website</h1>

  <nav>
    <a href="/">Home</a>
    <a href="/about">About</a>
    <a href="/contact">Contact</a>
  </nav>
</header>
```

```css
.site-header {
  display: flex;
  justify-content: space-between;
  align-items: center;
  padding: 20px 40px;
  background-color: #222;
  color: white;
}

.site-header h1 {
  margin: 0;
}

.site-header nav {
  display: flex;
  gap: 20px;
}

.site-header a {
  color: white;
  text-decoration: none;
}
```

The semantic meaning comes from HTML, while CSS controls the visual presentation.

# 🗁 Common Header Patterns

### 🖆 Website Header with Logo

```html
<header class="site-header">
  <a href="/" class="logo">
    MyBrand
  </a>

  <nav aria-label="Main navigation">
    <a href="/">Home</a>
    <a href="/products">Products</a>
    <a href="/about">About</a>
  </nav>
</header>
```

### 🖆 Article Header

```html
<article>
  <header>
    <h1>Understanding Web Development</h1>

    <p>
      Published:
      <time datetime="2026-11-08">November 8, 2026</time>
    </p>

    <p>Author: Muhammad</p>
  </header>

  <p>Web development involves building websites and applications.</p>
</article>
```

### 🖆 Dashboard Header

```html
<header class="dashboard-header">
  <h1>Dashboard</h1>

  <div class="user-menu">
    <span>Welcome, User</span>
    <button type="button">Profile</button>
  </div>
</header>
```

# 🗁 Best Practices

* Use `<header>` for introductory content.
* Use a page-level header for website branding and primary navigation when appropriate.
* Use section-level headers to introduce articles or thematic sections.
* Remember that multiple `<header>` elements are allowed in a document.
* Use headings to provide meaningful titles.
* Use `<nav>` for navigation links rather than relying on `<header>` alone.
* Use CSS classes for styling instead of choosing elements based only on appearance.
* Do not use `<header>` as a generic wrapper for unrelated content.
* Do not confuse `<header>` with `<head>`.
* Keep accessibility and document structure in mind when organizing page content.

# 🗁 Common Mistakes

### 🖆 Using Header as a Generic Container

Incorrect semantic usage:

```html
<header>
  <p>Product price: $100</p>
  <p>Product description.</p>
  <button type="button">Buy Now</button>
</header>
```

If this content is not introductory, a more appropriate structure may be:

```html
<article class="product">
  <h2>Product Name</h2>
  <p>Product price: $100</p>
  <p>Product description.</p>
  <button type="button">Buy Now</button>
</article>
```

### 🖆 Confusing Header with Head

Incorrect:

```html
<body>
  <head>
    <title>My Website</title>
  </head>
</body>
```

Correct:

```html
<html lang="en">
<head>
  <title>My Website</title>
</head>
<body>
  <header>
    <h1>My Website</h1>
  </header>
</body>
</html>
```

### 🖆 Assuming Every Header Creates a Banner Landmark

A `<header>` inside an article or another sectioning context does not automatically create a page-level banner landmark.

Use the correct semantic structure rather than adding unnecessary ARIA roles.

# 🗁 Practical Example

The following example demonstrates a complete page with a website header and multiple article headers.

```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">

  <title>Technology Blog</title>

  <style>
    * {
      box-sizing: border-box;
    }

    body {
      margin: 0;
      font-family: Arial, sans-serif;
      line-height: 1.6;
    }

    .site-header {
      padding: 20px 40px;
      background-color: #222;
      color: white;
    }

    .site-header h1 {
      margin: 0 0 10px;
    }

    .site-header nav {
      display: flex;
      gap: 20px;
    }

    .site-header a {
      color: white;
      text-decoration: none;
    }

    main {
      max-width: 900px;
      margin: 30px auto;
      padding: 0 20px;
    }

    article {
      padding: 20px;
      margin-bottom: 20px;
      border: 1px solid #ddd;
      border-radius: 8px;
    }

    article header {
      margin-bottom: 15px;
    }

    article h2 {
      margin-bottom: 5px;
    }

    article header p {
      color: #666;
      margin: 0;
    }
  </style>
</head>

<body>

  <header class="site-header">
    <h1>Technology Blog</h1>

    <nav aria-label="Main navigation">
      <a href="/">Home</a>
      <a href="/articles">Articles</a>
      <a href="/about">About</a>
    </nav>
  </header>

  <main>

    <article>
      <header>
        <h2>Introduction to HTML</h2>
        <p>
          Published:
          <time datetime="2026-11-08">November 8, 2026</time>
        </p>
      </header>

      <p>
        HTML provides the structure and meaning of web documents.
      </p>
    </article>

    <article>
      <header>
        <h2>Introduction to CSS</h2>
        <p>
          Published:
          <time datetime="2026-11-08">November 8, 2026</time>
        </p>
      </header>

      <p>
        CSS controls the appearance and layout of web documents.
      </p>
    </article>

  </main>

</body>
</html>
```

# 🗁 Practice Exercises

* Create a website header containing a logo and navigation links.
* Create a blog page with a page-level header and multiple article headers.
* Add a section-level header to an `<article>`.
* Style a header using Flexbox.
* Create a responsive website header with CSS media queries.
* Add an accessible navigation landmark inside the website header.
* Compare the semantic purpose of `<header>`, `<head>`, and `<div>`.
* Inspect the accessibility tree in browser developer tools to understand how page-level and section-level headers are exposed.

# 🗁 Interview Questions

**What is the purpose of the HTML `<header>` element?**

It represents introductory content or a group of introductory and navigational elements for a page or section.

**Can a webpage contain multiple `<header>` elements?**

Yes. A page can have a page-level header and additional headers inside articles or other sectioning contexts.

**What is the difference between `<header>` and `<head>`?**

`<header>` contains introductory page content, while `<head>` contains document metadata and resource references.

**Can `<header>` contain a `<nav>` element?**

Yes. A website header commonly contains primary navigation.

**Does every `<header>` create a banner landmark?**

No. A page-level header generally creates a banner landmark, but a header inside sectioning content or other landmark elements does not create a separate banner landmark.

**Can a `<header>` contain an `<h1>` element?**

Yes. It commonly contains a heading that introduces the page or section.

**What is the difference between `<header>` and `<div>`?**

`<header>` has semantic meaning, while `<div>` is a generic container.

**Can an `<article>` contain its own `<header>`?**

Yes. An article header commonly contains its title, publication date, author, and other introductory information.

**Should CSS be used to create the semantic meaning of a header?**

No. Semantic meaning should come from HTML elements. CSS controls visual presentation.

# 🗁 Official References

* [MDN — The Header Element](https://developer.mozilla.org/en-US/docs/Web/HTML/Reference/Elements/header)
* [MDN — HTML Elements Reference](https://developer.mozilla.org/en-US/docs/Web/HTML/Reference/Elements)
* [MDN — ARIA Landmarks](https://developer.mozilla.org/en-US/docs/Web/Accessibility/ARIA/Roles/landmark_role)
* [WHATWG — The Header Element](https://html.spec.whatwg.org/multipage/sections.html#the-header-element)
* [W3C WAI — Page Structure](https://www.w3.org/WAI/tutorials/page-structure/)
