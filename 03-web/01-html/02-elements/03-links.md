# HTML Links

HTML links, also called hyperlinks, allow users to navigate between webpages, websites, files, and specific locations within a document.

Links are one of the fundamental features of the web. They connect documents and resources, making it possible to navigate between different parts of a website or to external websites.

HTML uses the `<a>` (anchor) element to create hyperlinks.

# 🗁 The Anchor Element

The `<a>` element creates a hyperlink to another resource or location.

**Syntax:**

```html
<a href="URL">Link Text</a>
```

Where:

* `<a>` is the anchor element.
* `href` specifies the link destination.
* `Link Text` is the visible, clickable content.

**Example:**

```html
<a href="https://www.google.com">Visit Google</a>
```

When a user clicks the link, the browser navigates to the specified destination.

The `<a>` element can contain text, images, or other phrasing content.

**Example with an image:**

```html
<a href="/">
    <img src="logo.png" alt="Homepage">
</a>
```

The image becomes a clickable link to the homepage.

# 🗁 Link Syntax

A basic hyperlink consists of an opening anchor tag, link content, and a closing anchor tag.

```html
<a href="https://example.com">Visit Example</a>
```

The `href` attribute is important because it defines where the link navigates.

An anchor element without `href` does not create a hyperlink. It can still represent a placeholder or a scripting-related element, but it is not keyboard-focusable as a normal link by default.

# 🗁 The href Attribute

The `href` attribute specifies the URL or destination of a hyperlink.

**Example:**

```html
<a href="https://developer.mozilla.org">
    MDN Web Docs
</a>
```

The `href` attribute supports different URL schemes and URL formats.

| URL Type | Example | Purpose |
| --- | --- | --- |
| HTTP | `http://example.com` | Access an HTTP resource |
| HTTPS | `https://example.com` | Access a secure HTTP resource |
| Relative path | `about.html` | Navigate to a relative resource |
| Root-relative path | `/about` | Navigate from the website root |
| Fragment | `#contact` | Navigate to a document section |
| Email | `mailto:hello@example.com` | Open an email application |
| Telephone | `tel:+8801234567890` | Initiate a phone call on supported devices |

# 🗁 Types of Links

HTML supports different types of links depending on the destination.

### 🖆 External Links

External links navigate to resources on another website.

```html
<a href="https://www.wikipedia.org">
    Visit Wikipedia
</a>
```

Use external links when referencing documentation, articles, services, or other websites.

### 🖆 Internal Links

Internal links navigate to other pages or resources within the same website.

```html
<a href="/about.html">About Us</a>

<a href="/contact.html">Contact Us</a>
```

These links are useful for website navigation.

### 🖆 Navigation Links

Navigation links are commonly placed inside the `<nav>` element.

```html
<nav aria-label="Main navigation">
    <a href="/">Home</a>
    <a href="/about">About</a>
    <a href="/services">Services</a>
    <a href="/contact">Contact</a>
</nav>
```

The `<nav>` element identifies a section containing major navigation links.

### 🖆 Image Links

An image can be wrapped inside an anchor element to make it clickable.

```html
<a href="/products">
    <img
        src="images/product.jpg"
        alt="View our products"
        width="300"
    >
</a>
```

The image's alternative text should communicate the purpose of the link when the image is its only content.

### 🖆 Empty or Placeholder Links

Avoid using `href="#"` as a general substitute for a real destination.

```html
<!-- Avoid using this for a non-functional link -->
<a href="#">Click Here</a>
```

If an action is required, use a `<button>`. If navigation is required, provide a meaningful destination.

# 🗁 Absolute and Relative URLs

URLs can be written as absolute URLs or relative URLs.

### 🖆 Absolute URLs

An absolute URL contains the complete address of a resource, including its scheme and domain.

**Example:**

```html
<a href="https://developer.mozilla.org/en-US/docs/Web/HTML">
    Learn HTML on MDN
</a>
```

Absolute URLs are commonly used for external websites and resources.

### 🖆 Relative URLs

A relative URL specifies a destination in relation to the current document's URL.

Suppose your project has this structure:

```text
website/
├── index.html
├── about.html
├── contact.html
└── pages/
    └── services.html
```

From `index.html`:

```html
<a href="about.html">About</a>
<a href="contact.html">Contact</a>
<a href="pages/services.html">Services</a>
```

From `pages/services.html`, to navigate to `about.html`:

```html
<a href="../about.html">About</a>
```

The `../` segment moves one directory level upward.

### 🖆 Root-Relative URLs

A root-relative URL begins with `/` and resolves from the website's origin root.

```html
<a href="/about.html">About</a>
```

For example, if the website origin is `https://example.com`, this resolves to:

```text
https://example.com/about.html
```

Root-relative paths are useful when the application is deployed at the domain root. If an application is deployed under a subdirectory, its routing or asset configuration must account for that base path.

# 🗁 Opening Links in New Tabs

The `target` attribute controls where a linked resource is opened.

Common values include:

| Value | Behavior |
| --- | --- |
| `_self` | Opens in the current browsing context |
| `_blank` | Opens in a new tab or window, depending on browser settings |
| `_parent` | Opens in the parent browsing context |
| `_top` | Opens in the top-level browsing context |

**Example:**

```html
<a
    href="https://developer.mozilla.org"
    target="_blank"
    rel="noopener"
>
    Open MDN in a new tab
</a>
```

### 🖆 Understanding rel="noopener"

When using `target="_blank"`, adding `rel="noopener"` prevents the newly opened page from accessing the original page through `window.opener`.

Modern browsers generally provide implicit `noopener` behavior for `target="_blank"`, but explicitly specifying it can make the security intention clear.

### 🖆 Using rel="noreferrer"

The `noreferrer` value prevents the browser from sending the referring page's address in the `Referer` header and also implies `noopener`.

```html
<a
    href="https://example.com"
    target="_blank"
    rel="noopener noreferrer"
>
    Visit Example
</a>
```

Use `noreferrer` when suppressing referrer information is intentional.

# 🗁 Linking to a Specific Section

A fragment identifier allows users to navigate to a specific element within a document.

The destination element needs an `id` attribute.

**Example:**

```html
<nav>
    <a href="#introduction">Introduction</a>
    <a href="#features">Features</a>
    <a href="#contact">Contact</a>
</nav>

<section id="introduction">
    <h2>Introduction</h2>
    <p>Welcome to our website.</p>
</section>

<section id="features">
    <h2>Features</h2>
    <p>Explore our features.</p>
</section>

<section id="contact">
    <h2>Contact</h2>
    <p>Get in touch with us.</p>
</section>
```

When a user clicks a link such as `href="#features"`, the browser navigates to the element whose `id` is `features`.

### 🖆 Linking to a Section on Another Page

You can combine a page URL with a fragment identifier.

```html
<a href="/about.html#team">
    Meet Our Team
</a>
```

This navigates to the `team` section of `about.html`.

### 🖆 Smooth Scrolling

CSS can enable smooth scrolling for fragment navigation.

```css
html {
    scroll-behavior: smooth;
}
```

For users who prefer reduced motion, respect their system preference:

```css
@media (prefers-reduced-motion: reduce) {
    html {
        scroll-behavior: auto;
    }
}
```

# 🗁 Email and Telephone Links

The anchor element supports special URL schemes for email and telephone actions.

### 🖆 Email Links

Use the `mailto:` scheme to create a link that opens the user's configured email application.

```html
<a href="mailto:hello@example.com">
    Send an Email
</a>
```

You can also provide a subject and message body.

```html
<a
    href="mailto:hello@example.com?subject=Website%20Inquiry&body=Hello"
>
    Contact Us
</a>
```

Spaces and special characters should be URL-encoded when included in a URL.

### 🖆 Telephone Links

Use the `tel:` scheme to create a telephone link.

```html
<a href="tel:+8801234567890">
    Call Us
</a>
```

On supported devices, clicking this link can open the phone application.

# 🗁 Download Links

The `download` attribute indicates that the browser should download the linked resource rather than navigate to it normally.

**Example:**

```html
<a href="/files/user-guide.pdf" download>
    Download User Guide
</a>
```

You can suggest a filename:

```html
<a
    href="/files/user-guide.pdf"
    download="web-development-guide.pdf"
>
    Download PDF
</a>
```

The browser may honor or ignore the suggested filename depending on browser behavior, server response headers, and security restrictions.

The `download` attribute generally works for same-origin URLs and supported `blob:` or `data:` URLs. It is not a reliable way to force downloads from arbitrary external websites.

# 🗁 Link Titles and Accessible Names

### 🖆 The title Attribute

The `title` attribute can provide advisory information about a link.

```html
<a
    href="https://developer.mozilla.org"
    title="Read HTML documentation"
>
    MDN Web Docs
</a>
```

Some browsers display the title as a tooltip when a pointer hovers over the link.

However, the `title` attribute should not be the only way to communicate important information because it may not be accessible or convenient for all users and devices.

### 🖆 Descriptive Link Text

Link text should communicate the destination or purpose of the link.

**Poor example:**

```html
<a href="/documentation">Click Here</a>
```

**Better example:**

```html
<a href="/documentation">Read the documentation</a>
```

Descriptive link text helps users understand where a link leads, including users navigating with screen readers.

### 🖆 Accessible Image Links

When an image is the only content inside a link, its `alt` text should describe the link's purpose.

```html
<a href="/">
    <img src="logo.svg" alt="Company homepage">
</a>
```

If an image is decorative and the link has another accessible name, the image can use empty alternative text:

```html
<a href="/products">
    <img src="arrow.svg" alt="">
    View products
</a>
```

# 🗁 Styling Links with CSS

CSS controls the visual appearance of hyperlinks.

Links have several common interaction states:

| Pseudo-Class | Purpose |
| --- | --- |
| `:link` | Unvisited links |
| `:visited` | Previously visited links |
| `:hover` | Pointer is over the link |
| `:active` | Link is being activated |
| `:focus-visible` | Link has keyboard-visible focus |

**Example:**

```html
<a class="custom-link" href="/about">
    About Us
</a>
```

```css
.custom-link {
    color: #2563eb;
    text-decoration: none;
}

.custom-link:hover {
    text-decoration: underline;
}

.custom-link:focus-visible {
    outline: 2px solid #2563eb;
    outline-offset: 3px;
}

.custom-link:active {
    color: #1d4ed8;
}
```

### 🖆 Styling All Links

```css
a:link {
    color: blue;
}

a:visited {
    color: purple;
}

a:hover {
    color: red;
}

a:active {
    color: darkred;
}
```

Preserve a visible distinction between links and surrounding text. Do not rely on color alone to communicate that text is interactive.

# 🗁 Best Practices

* Use `<a>` for navigation and links to resources.
* Provide a meaningful `href` destination.
* Write descriptive link text instead of vague phrases such as "Click Here".
* Use relative URLs for appropriate internal resources.
* Use absolute URLs when linking to external websites.
* Use `target="_blank"` only when opening a new browsing context is useful.
* Use `rel="noopener"` with new-tab links when making the security intention explicit.
* Use `rel="noreferrer"` when referrer information should not be sent.
* Use unique and meaningful `id` values for fragment navigation.
* Use `mailto:` and `tel:` for email and telephone links.
* Use the `download` attribute when a linked resource should be downloaded.
* Maintain visible keyboard focus indicators.
* Ensure links have accessible names.
* Use buttons for actions rather than pretending they are navigation links.
* Do not use empty links or `href="#"` as placeholders in production.
* Ensure links are visually distinguishable from surrounding text.

# 🗁 Common Mistakes

### 🖆 Missing href Attribute

Incorrect:

```html
<a>Visit Website</a>
```

This does not create a normal hyperlink.

Correct:

```html
<a href="https://example.com">Visit Website</a>
```

### 🖆 Using href="#" for Actions

Incorrect:

```html
<a href="#">Open Menu</a>
```

If the intention is to perform an action, use a button.

Correct:

```html
<button type="button">Open Menu</button>
```

### 🖆 Using Vague Link Text

Poor example:

```html
<a href="/guide">Click Here</a>
```

Better:

```html
<a href="/guide">Read the HTML Guide</a>
```

### 🖆 Forgetting to Encode URL Parameters

Incorrect:

```html
<a href="mailto:hello@example.com?subject=Website Inquiry">
    Send Email
</a>
```

The space in the subject should be encoded.

Correct:

```html
<a href="mailto:hello@example.com?subject=Website%20Inquiry">
    Send Email
</a>
```

### 🖆 Using target="_blank" Without Considering Security and UX

When opening external pages in a new tab, consider whether the behavior is useful to users. Use an appropriate `rel` value and make the new-tab behavior clear when necessary.

```html
<a
    href="https://example.com"
    target="_blank"
    rel="noopener"
>
    Visit Example (opens in a new tab)
</a>
```

### 🖆 Using Duplicate IDs

Incorrect:

```html
<h2 id="contact">Contact</h2>
<h2 id="contact">Contact Details</h2>
```

IDs should be unique within a document.

Correct:

```html
<h2 id="contact">Contact</h2>
<h2 id="contact-details">Contact Details</h2>
```

# 🗁 Practical Examples

### 🖆 Website Navigation

```html
<header>
    <h1>My Website</h1>

    <nav aria-label="Main navigation">
        <a href="/">Home</a>
        <a href="/about.html">About</a>
        <a href="/services.html">Services</a>
        <a href="/contact.html">Contact</a>
    </nav>
</header>
```

### 🖆 External Documentation Links

```html
<section>
    <h2>Useful Resources</h2>

    <ul>
        <li>
            <a
                href="https://developer.mozilla.org/en-US/docs/Web/HTML"
                target="_blank"
                rel="noopener"
            >
                MDN HTML Documentation
            </a>
        </li>

        <li>
            <a
                href="https://html.spec.whatwg.org/"
                target="_blank"
                rel="noopener"
            >
                HTML Living Standard
            </a>
        </li>
    </ul>
</section>
```

### 🖆 Page Navigation with Fragment Links

```html
<nav aria-label="Page contents">
    <a href="#overview">Overview</a>
    <a href="#features">Features</a>
    <a href="#pricing">Pricing</a>
</nav>

<main>
    <section id="overview">
        <h2>Overview</h2>
        <p>Learn about our product.</p>
    </section>

    <section id="features">
        <h2>Features</h2>
        <p>Explore the available features.</p>
    </section>

    <section id="pricing">
        <h2>Pricing</h2>
        <p>Review our pricing plans.</p>
    </section>
</main>
```

### 🖆 Email and Telephone Contact Links

```html
<section>
    <h2>Contact Us</h2>

    <p>
        Email:
        <a href="mailto:hello@example.com">
            hello@example.com
        </a>
    </p>

    <p>
        Phone:
        <a href="tel:+8801234567890">
            +880 1234-567890
        </a>
    </p>
</section>
```

### 🖆 Downloading a File

```html
<section>
    <h2>Resources</h2>

    <a
        href="/downloads/html-cheatsheet.pdf"
        download="html-cheatsheet.pdf"
    >
        Download HTML Cheatsheet
    </a>
</section>
```

# 🗁 Practice Exercises

* Create a webpage with navigation links to Home, About, Services, and Contact.
* Add an external link to MDN Web Docs.
* Create relative links between multiple HTML files.
* Create a table of contents using fragment identifiers.
* Add a link that opens an external website in a new tab.
* Create an email link using `mailto:`.
* Create a telephone link using `tel:`.
* Add a downloadable PDF link.
* Create a clickable image that navigates to the homepage.
* Style links using `:hover`, `:visited`, and `:focus-visible`.
* Test all links using keyboard navigation.

# 🗁 Interview Questions

**What is an HTML hyperlink?**

A hyperlink is a reference that allows users to navigate to another webpage, resource, or location within a document.

**Which HTML element creates hyperlinks?**

The `<a>` (anchor) element creates hyperlinks.

**What is the purpose of the href attribute?**

The `href` attribute specifies the destination URL of a hyperlink.

**What is the difference between an absolute URL and a relative URL?**

An absolute URL contains the complete address of a resource, while a relative URL identifies a resource in relation to the current document's URL.

**What is the purpose of target="_blank"?**

It requests that the linked resource open in a new browsing context, commonly a new tab.

**What is the purpose of rel="noopener"?**

It prevents the newly opened page from accessing the original page through `window.opener`.

**What is the difference between href="#" and href="/about"?**

`href="#"` points to the current document's fragment position, typically the top when no matching fragment target exists. `href="/about"` points to the `/about` path at the website origin.

**How do you create a link to a specific section of a webpage?**

Assign an `id` to the destination element and reference it using `href="#id-value"`.

**How do you create an email link?**

Use the `mailto:` URL scheme.

```html
<a href="mailto:hello@example.com">Send Email</a>
```

**How do you create a telephone link?**

Use the `tel:` URL scheme.

```html
<a href="tel:+8801234567890">Call Us</a>
```

**What is the purpose of the download attribute?**

It indicates that the linked resource should be downloaded rather than navigated to normally, subject to browser and origin restrictions.

**What is the difference between an anchor and a button?**

An anchor navigates to a resource or location. A button performs an action, such as submitting a form or opening a menu.

**Can an image be used as a hyperlink?**

Yes. Place an `<img>` element inside an `<a>` element.

# 🗁 References

* [MDN – The Anchor Element](https://developer.mozilla.org/en-US/docs/Web/HTML/Reference/Elements/a)
* [MDN – URL Structure](https://developer.mozilla.org/en-US/docs/Learn_web_development/Howto/Web_mechanics/What_is_a_URL)
* [WHATWG – HTML Living Standard: Links](https://html.spec.whatwg.org/multipage/links.html)
* [W3C WAI – Link Purpose](https://www.w3.org/WAI/WCAG22/Understanding/link-purpose-in-context.html)
