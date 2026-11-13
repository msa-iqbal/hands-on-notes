# HTML Footer Element (`<footer>`)

The HTML `<footer>` element represents footer content for its nearest sectioning content or sectioning root.

It commonly contains information about the page, article, or section, such as:

* Copyright information.
* Author or organization details.
* Contact information.
* Related links.
* Legal information.
* Footer navigation.
* Publication metadata.

The `<footer>` element provides semantic meaning to the content it contains. It does not automatically position content at the bottom of a page; CSS controls its visual layout.

# 🗁 Introduction

The `<footer>` element represents information about its nearest ancestor sectioning content or sectioning root.

For example:

* A `<footer>` inside `<body>` can represent the footer for the entire page.
* A `<footer>` inside `<article>` represents footer information for that article.
* A `<footer>` inside `<section>` represents footer information for that section.

A document can contain multiple `<footer>` elements because different sections and articles can have their own footer information.

### 🖆 Basic Syntax

```html
<footer>
  <p>&copy; 2026 My Website. All rights reserved.</p>
</footer>
```

The `<footer>` element is a semantic HTML element. It helps browsers, search engines, and assistive technologies understand the purpose of the content.

# 🗁 Basic Syntax

The `<footer>` element is written using an opening and closing tag.

```html
<footer>
  <!-- Footer content -->
</footer>
```

It can contain ordinary flow content, including:

* Paragraphs.
* Navigation elements.
* Links.
* Lists.
* Contact information.
* Images and other appropriate content.

Example:

```html
<footer>
  <p>Created by John Doe</p>

  <nav aria-label="Footer navigation">
    <a href="/about">About</a>
    <a href="/contact">Contact</a>
    <a href="/privacy">Privacy Policy</a>
  </nav>

  <p>&copy; 2026 My Website.</p>
</footer>
```

# 🗁 Page-Level Footer

A page-level footer contains information related to the entire website or document.

It is commonly placed near the end of the `<body>` element.

Example:

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
  </header>

  <main>
    <h2>Welcome</h2>
    <p>Welcome to our website.</p>
  </main>

  <footer>
    <p>&copy; 2026 My Website. All rights reserved.</p>
  </footer>

</body>
</html>
```

Common page-level footer content includes:

* Website copyright.
* Company information.
* Contact details.
* Privacy policy.
* Terms and conditions.
* Social media links.
* Site navigation.

# 🗁 Article-Level Footer

An article can have its own footer containing information related specifically to that article.

Example:

```html
<article>
  <header>
    <h2>Introduction to HTML</h2>
    <p>Published on November 8, 2026</p>
  </header>

  <p>
    HTML is the standard markup language used
    to structure content on the web.
  </p>

  <footer>
    <p>Written by John Doe</p>
    <p>Category: Web Development</p>
  </footer>
</article>
```

The article footer provides metadata about the article rather than the entire website.

# 🗁 Section-Level Footer

A section can also contain its own footer.

Example:

```html
<section>
  <h2>Latest Tutorials</h2>

  <p>Explore our latest web development tutorials.</p>

  <footer>
    <a href="/tutorials">View All Tutorials</a>
  </footer>
</section>
```

This footer provides additional information or related navigation for the section.

# 🗁 Footer Navigation

A footer frequently contains navigation links to important website pages.

Use the `<nav>` element to identify a group of navigation links.

Example:

```html
<footer>
  <nav aria-label="Footer navigation">
    <ul>
      <li><a href="/about">About Us</a></li>
      <li><a href="/services">Services</a></li>
      <li><a href="/contact">Contact</a></li>
      <li><a href="/privacy">Privacy Policy</a></li>
    </ul>
  </nav>

  <p>&copy; 2026 My Website.</p>
</footer>
```

### 🖆 Multiple Navigation Elements

A website may have navigation in both its header and footer.

Give each navigation landmark a meaningful accessible name so users of assistive technologies can distinguish them.

```html
<header>
  <nav aria-label="Main navigation">
    <a href="/">Home</a>
    <a href="/about">About</a>
  </nav>
</header>

<footer>
  <nav aria-label="Footer navigation">
    <a href="/privacy">Privacy</a>
    <a href="/contact">Contact</a>
  </nav>
</footer>
```

# 🗁 Contact Information

A footer can contain contact information using the `<address>` element.

The `<address>` element represents contact information for the nearest `<article>` or the document as a whole.

Example:

```html
<footer>
  <address>
    Contact us:
    <a href="mailto:info@example.com">info@example.com</a>
  </address>

  <p>&copy; 2026 Example Company.</p>
</footer>
```

The `<address>` element is intended for contact information, not arbitrary postal addresses or general location details.

# 🗁 Footer and Accessibility

Semantic footer elements help users understand the structure of a webpage.

### 🖆 Contentinfo Landmark

A page-level `<footer>` generally exposes a `contentinfo` landmark to assistive technologies.

This helps screen reader users locate important information about the website.

However, a `<footer>` nested inside sectioning content such as an `<article>` or `<section>` does not generally create a separate `contentinfo` landmark.

### 🖆 Use Meaningful Navigation Labels

When multiple navigation elements exist, provide distinct accessible names.

```html
<footer>
  <nav aria-label="Footer navigation">
    <a href="/privacy">Privacy Policy</a>
    <a href="/terms">Terms of Service</a>
  </nav>
</footer>
```

### 🖆 Keep Links Accessible

Footer links should have meaningful text and remain keyboard accessible.

Good:

```html
<a href="/contact">Contact Us</a>
```

Avoid:

```html
<a href="/contact">Click here</a>
```

### 🖆 Avoid Redundant ARIA Roles

A semantic `<footer>` does not normally need an explicit `role="contentinfo"`.

Use native HTML semantics instead of adding unnecessary ARIA attributes.

# 🗁 Footer vs Other Semantic Elements

| Element | Purpose |
| --- | --- |
| `<header>` | Introductory content for a page or section |
| `<main>` | Dominant content of the document |
| `<nav>` | Major navigation links |
| `<section>` | Thematic grouping of content |
| `<article>` | Self-contained, independently meaningful content |
| `<aside>` | Complementary content |
| `<footer>` | Footer information for a page or section |
| `<address>` | Contact information for the nearest article or document |

### 🖆 Footer vs Header

The `<header>` element represents introductory content, while `<footer>` represents footer information.

```html
<article>
  <header>
    <h2>Learning CSS</h2>
  </header>

  <p>CSS controls the presentation of HTML documents.</p>

  <footer>
    <p>Written by John Doe</p>
  </footer>
</article>
```

# 🗁 Best Practices

* Use `<footer>` for genuine footer content.
* Use a page-level footer for website-wide information.
* Use article-level or section-level footers when content belongs to those sections.
* Use `<nav>` for groups of important footer navigation links.
* Use `<address>` for relevant contact information.
* Provide meaningful link text.
* Give multiple navigation landmarks distinct accessible names.
* Keep footer content logically related to its parent section.
* Use CSS to control visual positioning and layout.
* Prefer semantic HTML over unnecessary ARIA roles.

# 🗁 Common Mistakes

### 🖆 Using Footer as a Generic Container

Incorrect:

```html
<footer>
  <h1>Welcome to My Website</h1>
  <p>Main website content.</p>
</footer>
```

The `<footer>` element should represent footer information, not arbitrary page content.

### 🖆 Assuming Footer Must Be at the Visual Bottom

The `<footer>` element does not automatically position itself at the bottom of the viewport or page.

Use CSS for visual positioning.

### 🖆 Using Multiple Unrelated Footer Elements

Multiple footer elements are valid when each belongs to a different section or article.

Avoid using them without a clear semantic purpose.

### 🖆 Using Address for Any Postal Address

Incorrect:

```html
<address>
  123 Main Street, New York
</address>
```

If this is simply a location rather than contact information for the relevant document or article, use an appropriate element such as `<p>`.

### 🖆 Adding Unnecessary ARIA Roles

Avoid:

```html
<footer role="contentinfo">
  <p>&copy; 2026 My Website.</p>
</footer>
```

A page-level `<footer>` already provides the appropriate native semantics in supporting accessibility mappings.

# 🗁 Practical Examples

### 🖆 Complete Website Footer

```html
<footer>
  <div class="footer-content">

    <div class="footer-about">
      <h2>About Us</h2>
      <p>
        We provide practical tutorials
        for developers and technology enthusiasts.
      </p>
    </div>

    <nav aria-label="Footer navigation">
      <h2>Quick Links</h2>

      <ul>
        <li><a href="/">Home</a></li>
        <li><a href="/about">About</a></li>
        <li><a href="/tutorials">Tutorials</a></li>
        <li><a href="/contact">Contact</a></li>
      </ul>
    </nav>

    <div class="footer-contact">
      <h2>Contact</h2>

      <address>
        <a href="mailto:info@example.com">
          info@example.com
        </a>
      </address>
    </div>

  </div>

  <div class="footer-bottom">
    <p>&copy; 2026 Example Company. All rights reserved.</p>
  </div>
</footer>
```

### 🖆 Footer with CSS

```html
<footer class="site-footer">
  <p>&copy; 2026 My Website.</p>

  <nav aria-label="Footer navigation">
    <a href="/privacy">Privacy Policy</a>
    <a href="/terms">Terms</a>
  </nav>
</footer>
```

```css
.site-footer {
  padding: 2rem;
  background-color: #222;
  color: white;
  text-align: center;
}

.site-footer nav {
  display: flex;
  justify-content: center;
  gap: 1rem;
}

.site-footer a {
  color: inherit;
  text-decoration: none;
}

.site-footer a:hover {
  text-decoration: underline;
}
```

# 🗁 Practical Exercises

* Create a webpage with a semantic page-level footer.
* Add copyright information and contact details.
* Create a footer navigation containing links to important pages.
* Add a separate footer inside an article.
* Create multiple navigation landmarks with distinct accessible names.
* Inspect the accessibility tree to understand how page-level and nested footers are exposed.

# 🗁 Interview Questions

**What is the purpose of the `<footer>` element?**

It represents footer information for its nearest sectioning content or sectioning root.

**Can a webpage contain multiple `<footer>` elements?**

Yes. Different sections and articles can have their own footer elements.

**Is `<footer>` a semantic HTML element?**

Yes. It communicates the purpose of footer content.

**Does `<footer>` always appear at the bottom of a webpage?**

No. Its visual position is controlled by CSS.

**What is the difference between a page-level footer and an article-level footer?**

A page-level footer contains information about the entire document, while an article-level footer contains information related to a particular article.

**Does every `<footer>` create a contentinfo landmark?**

No. A page-level footer generally creates a contentinfo landmark, but a footer nested inside sectioning content does not generally create a separate one.

**Can a `<footer>` contain a `<nav>` element?**

Yes. Footer navigation is a common and appropriate use.

**What is the difference between `<footer>` and `<address>`?**

`<footer>` represents footer content, while `<address>` represents contact information for the nearest article or the document.

# 🗁 References

* [MDN — `<footer>` Element](https://developer.mozilla.org/en-US/docs/Web/HTML/Reference/Elements/footer)
* [WHATWG — The Footer Element](https://html.spec.whatwg.org/multipage/sections.html#the-footer-element)
* [MDN — `<address>` Element](https://developer.mozilla.org/en-US/docs/Web/HTML/Reference/Elements/address)
* [W3C WAI — Page Structure](https://www.w3.org/WAI/tutorials/page-structure/)
