# HTML Section (`<section>`)

The HTML `<section>` element represents a thematic grouping of content within a document.

It is used to organize related content into meaningful sections, usually with a heading.

For example, a webpage about web development might contain separate sections for:

* HTML
* CSS
* JavaScript
* TypeScript

Each topic represents a distinct part of the overall document.

The `<section>` element is part of semantic HTML. It communicates the structure and organization of content to browsers, search engines, and assistive technologies.

# 🗁 Basic Syntax

```html
<section>
  <h2>Introduction to HTML</h2>

  <p>
    HTML is the standard markup language used
    to structure content on the web.
  </p>
</section>
```

### 🖆 Syntax Explanation

* `<section>` defines a thematic grouping of content.
* A heading describes the topic of the section.
* The content inside the element belongs to that topic.
* `</section>` closes the section.

A heading is strongly recommended because it communicates the section's purpose to users and assistive technologies.

# 🗁 Understanding the Section Element

A webpage often contains several related topics.

The `<section>` element helps divide a document into logical parts.

Consider a documentation page about web development:

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

  <section>
    <h2>JavaScript</h2>
    <p>JavaScript adds behavior and interactivity.</p>
  </section>
</main>
```

Each section groups content around a specific subject.

# 🗁 Section with Headings

A section should generally have a heading that describes its content.

HTML supports multiple heading levels:

* `<h1>` — highest-level heading
* `<h2>` — second-level heading
* `<h3>` — third-level heading
* `<h4>` — fourth-level heading
* `<h5>` — fifth-level heading
* `<h6>` — sixth-level heading

Example:

```html
<section>
  <h2>Frontend Development</h2>

  <p>
    Frontend development focuses on the user-facing
    part of web applications.
  </p>

  <h3>Core Technologies</h3>

  <p>
    HTML, CSS, and JavaScript are the core technologies
    used in frontend development.
  </p>
</section>
```

Choose heading levels according to the document's hierarchy rather than their default visual appearance.

CSS should control heading appearance.

# 🗁 Multiple Sections

A document can contain multiple sections.

Each section should represent a distinct thematic grouping.

```html
<main>
  <h1>Web Development Guide</h1>

  <section>
    <h2>Frontend Development</h2>
    <p>
      Frontend development focuses on the user interface.
    </p>
  </section>

  <section>
    <h2>Backend Development</h2>
    <p>
      Backend development handles application logic,
      data processing, and server-side operations.
    </p>
  </section>

  <section>
    <h2>Database Development</h2>
    <p>
      Database development focuses on storing,
      organizing, and retrieving application data.
    </p>
  </section>
</main>
```

This structure makes the document easier to understand and maintain.

# 🗁 Nested Sections

A section can contain other sections when the content has a meaningful hierarchy.

```html
<main>
  <h1>Programming Languages</h1>

  <section>
    <h2>Frontend Languages</h2>

    <section>
      <h3>JavaScript</h3>
      <p>JavaScript adds behavior to web pages.</p>
    </section>

    <section>
      <h3>TypeScript</h3>
      <p>TypeScript adds static typing to JavaScript.</p>
    </section>
  </section>

  <section>
    <h2>Backend Languages</h2>

    <section>
      <h3>Python</h3>
      <p>Python is used for backend development and automation.</p>
    </section>

    <section>
      <h3>PHP</h3>
      <p>PHP is a server-side scripting language.</p>
    </section>
  </section>
</main>
```

### 🖆 Important Considerations

* Use nesting only when the content hierarchy requires it.
* Choose heading levels that reflect the document structure.
* Avoid excessive nesting.
* Do not use sections merely to create visual indentation or spacing.

# 🗁 Section Inside Main

The `<section>` element is commonly used inside `<main>` to organize the primary content.

```html
<body>
  <header>
    <h1>Developer Documentation</h1>
  </header>

  <nav aria-label="Main navigation">
    <a href="/">Home</a>
    <a href="/docs">Documentation</a>
  </nav>

  <main>
    <h1>HTML Documentation</h1>

    <section>
      <h2>HTML Introduction</h2>
      <p>Learn the fundamentals of HTML.</p>
    </section>

    <section>
      <h2>HTML Elements</h2>
      <p>Understand HTML elements and their purposes.</p>
    </section>

    <section>
      <h2>HTML Attributes</h2>
      <p>Learn how attributes configure HTML elements.</p>
    </section>
  </main>

  <footer>
    <p>&copy; 2026 Developer Documentation</p>
  </footer>
</body>
```

# 🗁 Section vs Div

Both `<section>` and `<div>` are container elements, but they have different semantic purposes.

| Feature | `<section>` | `<div>` |
| --- | --- | --- |
| Semantic meaning | Thematic grouping | Generic container |
| Document structure | Contributes to content organization | Does not express a specific content role |
| Heading | Generally recommended | Not required |
| Typical usage | Related content grouped by topic | Layout and styling |
| Accessibility | Can create a section in the accessibility structure | Usually no semantic role |

### 🖆 Using Section

```html
<section>
  <h2>Our Services</h2>
  <p>Explore the services we provide.</p>
</section>
```

### 🖆 Using Div

```html
<div class="card">
  <h2>Web Development</h2>
  <p>Learn modern web development.</p>
</div>
```

Use `<section>` when the content represents a meaningful thematic grouping.

Use `<div>` when you only need a generic container for styling, layout, or scripting.

# 🗁 Section vs Article

Both elements organize content, but they represent different concepts.

| Feature | `<section>` | `<article>` |
| --- | --- | --- |
| Purpose | Groups related content by topic | Represents self-contained content |
| Independence | Usually part of a larger document | Can stand independently |
| Typical usage | Chapters, topic groups, page sections | Blog posts, news articles, forum posts |
| Reusability | Usually tied to the surrounding document | Often suitable for independent distribution |

### 🖆 Section Example

```html
<section>
  <h2>Latest Updates</h2>

  <p>Read the latest platform updates.</p>
</section>
```

### 🖆 Article Example

```html
<article>
  <h2>Understanding Semantic HTML</h2>

  <p>
    Semantic HTML gives meaning to the structure
    of web documents.
  </p>
</article>
```

An `<article>` can contain multiple `<section>` elements when its content needs thematic organization.

```html
<article>
  <h2>Learning HTML</h2>

  <section>
    <h3>Introduction</h3>
    <p>Understand the purpose of HTML.</p>
  </section>

  <section>
    <h3>HTML Elements</h3>
    <p>Learn about HTML elements.</p>
  </section>
</article>
```

# 🗁 Accessibility

Semantic sections help users understand the organization of a document.

### 🖆 Use Descriptive Headings

A heading should communicate the purpose of its section.

```html
<section>
  <h2>Contact Information</h2>
  <p>Email: support@example.com</p>
</section>
```

Avoid meaningless headings such as "Section 1" when a descriptive title is possible.

### 🖆 Use Logical Heading Hierarchy

Maintain a meaningful heading structure.

```html
<main>
  <h1>Web Development</h1>

  <section>
    <h2>Frontend Development</h2>

    <section>
      <h3>HTML</h3>
    </section>

    <section>
      <h3>CSS</h3>
    </section>
  </section>
</main>
```

Do not select heading levels only because of their default font size.

### 🖆 Understand Sectioning Semantics

A `<section>` can represent a section in the document structure. When it has an accessible name, it can be exposed as a region landmark.

For example:

```html
<section aria-labelledby="features-heading">
  <h2 id="features-heading">Features</h2>

  <p>Explore the main features of the application.</p>
</section>
```

Do not add `aria-label` or `aria-labelledby` to every section automatically. Use them when a named region improves navigation.

# 🗁 Best Practices

* Use `<section>` for meaningful thematic groupings.
* Include a descriptive heading in most sections.
* Place related content together.
* Use appropriate heading levels.
* Use `<article>` for independently meaningful content.
* Use `<div>` for generic layout and styling.
* Avoid excessive section nesting.
* Use accessible names for sections that need to be exposed as named regions.
* Keep the document structure understandable without CSS.

# 🗁 Common Mistakes

### 🖆 Using Section Only for Styling

```html
<!-- Avoid -->
<section class="container">
  <p>Hello World</p>
</section>
```

If the container has no meaningful thematic purpose, use `<div>` instead.

### 🖆 Missing a Descriptive Heading

```html
<!-- Avoid when a meaningful heading is appropriate -->
<section>
  <p>HTML is a markup language.</p>
</section>
```

A descriptive heading helps communicate the section's purpose.

### 🖆 Excessive Nesting

```html
<!-- Avoid unnecessary nesting -->
<section>
  <section>
    <section>
      <section>
        <p>Content</p>
      </section>
    </section>
  </section>
</section>
```

Use nesting only when it represents a real content hierarchy.

### 🖆 Using Section Instead of Article

Do not use `<section>` when the content is better represented as an independent article.

Choose the element according to the meaning of the content.

# 🗁 Practical Exercises

### 🖆 Exercise: Create a Documentation Page

Build a documentation page with the following sections:

* Introduction
* HTML Fundamentals
* HTML Elements
* HTML Attributes
* Conclusion

Use `<main>` and `<section>` with appropriate headings.

### 🖆 Exercise: Create Nested Sections

Build a programming guide with:

* Frontend Development
  * HTML
  * CSS
  * JavaScript
* Backend Development
  * Node.js
  * Python
  * PHP

Use nested sections where they represent meaningful thematic groups.

### 🖆 Exercise: Compare Section and Div

Create two content containers:

* A thematic section about web development.
* A generic card container for styling.

Choose `<section>` and `<div>` appropriately.

# 🗁 Interview Questions

**What is the purpose of the `<section>` element?**

It represents a thematic grouping of content within a document.

**Should a section have a heading?**

A heading is generally recommended because it describes the section's purpose.

**What is the difference between `<section>` and `<div>`?**

`<section>` represents a meaningful thematic grouping, while `<div>` is a generic container without inherent semantic meaning.

**What is the difference between `<section>` and `<article>`?**

`<section>` groups related content, while `<article>` represents self-contained content that can stand independently.

**Can a section contain another section?**

Yes. Nested sections are valid when they represent a meaningful content hierarchy.

**Can an article contain sections?**

Yes. An article can contain multiple sections to organize its content.

**Does every section create a region landmark?**

No. A section generally becomes a region landmark when it has an accessible name.

**Can CSS replace the semantic purpose of `<section>`?**

No. CSS controls presentation, while HTML communicates document structure and meaning.

# 🗁 References

* [MDN: `<section>` Element](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/section)
* [WHATWG: The `section` Element](https://html.spec.whatwg.org/multipage/sections.html#the-section-element)
* [W3C WAI: Headings](https://www.w3.org/WAI/tutorials/page-structure/headings/)
* [W3C WAI: Landmarks](https://www.w3.org/WAI/ARIA/apg/practices/landmark-regions/)
