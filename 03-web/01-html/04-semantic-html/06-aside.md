# HTML Aside (`<aside>`)

The HTML `<aside>` element represents content that is indirectly related to the surrounding content.

It is commonly used for complementary information, supporting content, and secondary information that enhances the main content without being part of its central flow.

Common examples include:

* Sidebars
* Related articles
* Author biographies
* Additional resources
* Advertisements
* Glossaries
* Pull quotes
* Supplementary information

The `<aside>` element is part of semantic HTML. It helps browsers, search engines, and assistive technologies understand that certain content is complementary rather than primary.

# 🗁 Basic Syntax

```html
<aside>
  <h2>Related Articles</h2>

  <ul>
    <li><a href="/html">HTML Guide</a></li>
    <li><a href="/css">CSS Guide</a></li>
    <li><a href="/javascript">JavaScript Guide</a></li>
  </ul>
</aside>
```

### 🖆 Syntax Explanation

* `<aside>` defines complementary content.
* The heading describes the purpose of the content.
* The list contains related links.
* `</aside>` closes the element.

The content inside `<aside>` should be related to the surrounding content, but it should not be essential to its primary meaning.

# 🗁 Understanding the Aside Element

Consider a blog article about JavaScript.

The article contains the main discussion, while a sidebar provides additional resources and related information.

```html
<main>
  <article>
    <h1>Introduction to JavaScript</h1>

    <p>
      JavaScript is a programming language used
      to create interactive web applications.
    </p>

    <p>
      It runs in browsers and can also be used
      on servers through environments such as Node.js.
    </p>
  </article>

  <aside>
    <h2>Related Topics</h2>

    <ul>
      <li><a href="/html">HTML</a></li>
      <li><a href="/css">CSS</a></li>
      <li><a href="/typescript">TypeScript</a></li>
    </ul>
  </aside>
</main>
```

The article contains the primary information. The aside provides supplementary resources.

# 🗁 Aside Inside Main

An `<aside>` can be placed inside `<main>` when its content is complementary to the primary page content.

```html
<main>
  <article>
    <h1>Learning HTML</h1>

    <p>
      HTML is the standard markup language
      used to structure web pages.
    </p>
  </article>

  <aside>
    <h2>Learning Resources</h2>

    <ul>
      <li><a href="/html-elements">HTML Elements</a></li>
      <li><a href="/html-attributes">HTML Attributes</a></li>
      <li><a href="/html-forms">HTML Forms</a></li>
    </ul>
  </aside>
</main>
```

The aside is part of the primary page content area, but it remains semantically complementary to the article.

# 🗁 Aside Inside Article

An `<aside>` can also appear inside an `<article>` when the supplementary content is specifically related to that article.

For example:

```html
<article>
  <h1>Understanding CSS Flexbox</h1>

  <p>
    Flexbox is a CSS layout model designed
    for arranging elements in one dimension.
  </p>

  <aside>
    <h2>Quick Tip</h2>

    <p>
      Use <code>display: flex</code> on a container
      to establish a flex formatting context.
    </p>
  </aside>

  <p>
    Flexbox provides properties for alignment,
    distribution, and flexible sizing.
  </p>
</article>
```

In this example, the aside contains supporting information directly related to the article.

# 🗁 Sidebar Layout

The `<aside>` element is frequently used to create a sidebar layout.

HTML defines the semantic structure, while CSS controls the visual arrangement.

```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Article with Sidebar</title>

  <style>
    .page-layout {
      display: grid;
      grid-template-columns: minmax(0, 1fr) 280px;
      gap: 2rem;
      max-width: 1100px;
      margin: 2rem auto;
      padding: 1rem;
    }

    article {
      min-width: 0;
    }

    aside {
      padding: 1rem;
      background: #f2f2f2;
      border-radius: 8px;
    }

    @media (max-width: 700px) {
      .page-layout {
        grid-template-columns: 1fr;
      }
    }
  </style>
</head>
<body>

  <main class="page-layout">
    <article>
      <h1>Web Development</h1>

      <p>
        Web development involves creating
        websites and web applications.
      </p>

      <p>
        HTML, CSS, and JavaScript are fundamental
        technologies of frontend development.
      </p>
    </article>

    <aside>
      <h2>Related Topics</h2>

      <ul>
        <li><a href="/html">HTML</a></li>
        <li><a href="/css">CSS</a></li>
        <li><a href="/javascript">JavaScript</a></li>
      </ul>
    </aside>
  </main>

</body>
</html>
```

### 🖆 Understanding the CSS

| Property | Purpose |
| --- | --- |
| `display: grid` | Creates a grid formatting context |
| `grid-template-columns` | Defines the article and sidebar columns |
| `gap` | Adds space between columns |
| `minmax(0, 1fr)` | Allows the main column to shrink appropriately |
| `@media` | Changes the layout at smaller viewport widths |

The semantic meaning of `<aside>` does not depend on its visual position. An aside can appear beside, above, or below the primary content.

# 🗁 Aside with Related Content

A related content area can provide links to resources that help users explore a topic further.

```html
<main>
  <article>
    <h1>Introduction to TypeScript</h1>

    <p>
      TypeScript extends JavaScript with
      a static type system.
    </p>
  </article>

  <aside>
    <h2>Related Documentation</h2>

    <ul>
      <li>
        <a href="/javascript-fundamentals">
          JavaScript Fundamentals
        </a>
      </li>

      <li>
        <a href="/typescript-types">
          TypeScript Types
        </a>
      </li>

      <li>
        <a href="/typescript-interfaces">
          TypeScript Interfaces
        </a>
      </li>
    </ul>
  </aside>
</main>
```

Other appropriate uses include:

* An author biography related to an article.
* A glossary defining technical terms.
* A supporting quotation.
* Additional context about the current topic.

# 🗁 Aside vs Section

Both `<aside>` and `<section>` organize content, but they communicate different meanings.

| Feature | `<aside>` | `<section>` |
| --- | --- | --- |
| Purpose | Complementary content | Thematic grouping |
| Relationship | Indirectly related to surrounding content | Groups content around a common topic |
| Typical usage | Sidebars, related links, supporting information | Chapters, topics, page sections |
| Can contain headings | Yes | Yes |
| Can appear inside article | Yes | Yes |

### 🖆 Aside Example

```html
<aside>
  <h2>Related Articles</h2>
  <p>Explore additional resources.</p>
</aside>
```

### 🖆 Section Example

```html
<section>
  <h2>HTML Fundamentals</h2>
  <p>Learn the basic concepts of HTML.</p>
</section>
```

Use `<aside>` for complementary information and `<section>` for a thematic grouping of content.

# 🗁 Aside vs Div

| Feature | `<aside>` | `<div>` |
| --- | --- | --- |
| Semantic meaning | Complementary content | Generic container |
| Document structure | Identifies supporting content | Does not express a specific content role |
| Accessibility | Can expose complementary landmark semantics | Usually has no semantic role |
| Typical usage | Sidebars and related information | Layout and styling |

### 🖆 Using Aside

```html
<aside>
  <h2>Related Resources</h2>
  <a href="/documentation">Documentation</a>
</aside>
```

### 🖆 Using Div

```html
<div class="card">
  <h2>Product Information</h2>
  <p>Explore product details.</p>
</div>
```

Use `<div>` when a generic container is sufficient. Use `<aside>` when the content has a complementary semantic purpose.

# 🗁 Accessibility

The `<aside>` element has complementary landmark semantics when it is exposed as an appropriately named complementary region.

This can help screen reader users navigate supporting content.

### 🖆 Use Descriptive Headings

Provide a heading that explains the purpose of the aside.

```html
<aside>
  <h2>Related Articles</h2>

  <ul>
    <li><a href="/html">HTML Guide</a></li>
    <li><a href="/css">CSS Guide</a></li>
  </ul>
</aside>
```

### 🖆 Provide an Accessible Name When Needed

When multiple complementary landmarks exist, give them distinct accessible names.

```html
<aside aria-label="Related articles">
  <h2>Related Articles</h2>
  <a href="/html">HTML Guide</a>
</aside>

<aside aria-label="Author information">
  <h2>About the Author</h2>
  <p>Written by Alex.</p>
</aside>
```

You can also use `aria-labelledby` to reference an existing heading.

```html
<aside aria-labelledby="resources-heading">
  <h2 id="resources-heading">Additional Resources</h2>

  <a href="/documentation">Documentation</a>
</aside>
```

Do not add accessible names to every aside automatically. Use them when they help distinguish complementary landmarks.

### 🖆 Keep Important Content in the Main Flow

Do not place information essential to understanding the primary content exclusively inside an aside.

Users who navigate directly through the main content should not miss critical information.

### 🖆 Avoid Unnecessary ARIA

The `<aside>` element already communicates complementary semantics.

```html
<!-- Unnecessary -->
<aside role="complementary">
  <h2>Related Links</h2>
</aside>

<!-- Preferred -->
<aside>
  <h2>Related Links</h2>
</aside>
```

# 🗁 Best Practices

* Use `<aside>` for complementary or supporting content.
* Keep its content related to the surrounding page or article.
* Use descriptive headings.
* Use CSS Grid or Flexbox for sidebar layouts.
* Do not rely on visual positioning to communicate semantic meaning.
* Provide distinct accessible names when multiple complementary landmarks need to be distinguished.
* Keep essential information in the primary content flow.
* Use `<section>` for thematic content groupings.
* Use `<div>` for generic layout containers.
* Avoid using `<aside>` simply because content appears on the side of a screen.

# 🗁 Common Mistakes

### 🖆 Using Aside for Every Sidebar

Not every sidebar is semantically complementary.

If a sidebar contains primary navigation or essential application functionality, choose the appropriate semantic element instead.

### 🖆 Using Aside for Unrelated Content

```html
<!-- Avoid -->
<article>
  <h1>HTML Fundamentals</h1>
  <p>Learn the basics of HTML.</p>
</article>

<aside>
  <h2>Unrelated Topic</h2>
  <p>Information with no meaningful relationship to HTML.</p>
</aside>
```

An aside should have a meaningful relationship with its surrounding content.

### 🖆 Putting Essential Information Only in Aside

```html
<!-- Avoid when this information is essential -->
<article>
  <h1>Product Details</h1>
  <p>Product description.</p>
</article>

<aside>
  <h2>Important Product Warning</h2>
  <p>Essential information about product usage.</p>
</aside>
```

If information is necessary to understand or use the primary content, place it in the main content flow.

### 🖆 Using Aside Only for Visual Positioning

The word "aside" does not mean every visually positioned sidebar should use the `<aside>` element.

Choose the element according to the content's semantic relationship.

# 🗁 Practical Exercises

### 🖆 Exercise: Create a Blog Sidebar

Build a blog page containing:

* A main article
* Related articles
* Popular posts
* Author information

Use `<article>` and `<aside>` appropriately.

### 🖆 Exercise: Create a Documentation Page

Build a documentation page with:

* Main documentation content
* A related resources sidebar
* Additional learning links
* A glossary section

Use semantic elements to distinguish primary and complementary content.

### 🖆 Exercise: Build a Responsive Layout

Create a two-column layout using CSS Grid.

Requirements:

* Main content occupies the larger column.
* Aside occupies the smaller column.
* Both columns have appropriate spacing.
* On smaller screens, the layout becomes a single column.

# 🗁 Interview Questions

**What is the purpose of the `<aside>` element?**

It represents content that is indirectly related to the surrounding content, such as sidebars and related information.

**Where is `<aside>` commonly used?**

It is commonly used for sidebars, related articles, author information, advertisements, and supplementary resources.

**What is the difference between `<aside>` and `<section>`?**

`<aside>` represents complementary content, while `<section>` represents a thematic grouping of related content.

**Can `<aside>` be placed inside `<article>`?**

Yes. It can contain supplementary information related specifically to the article.

**Can a page contain multiple `<aside>` elements?**

Yes. Multiple asides are allowed when they represent different complementary content areas.

**Does `<aside>` always appear visually on the side of a page?**

No. CSS controls its visual position. Its semantic purpose is complementary content.

**What accessibility role does `<aside>` provide?**

It provides complementary semantics and can be exposed as a complementary landmark.

**Is `role="complementary"` required on `<aside>`?**

No. The semantic element already provides the appropriate role.

# 🗁 References

* [MDN: `<aside>` Element](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/aside)
* [WHATWG: The `aside` Element](https://html.spec.whatwg.org/multipage/sections.html#the-aside-element)
* [W3C WAI: Landmark Regions](https://www.w3.org/WAI/ARIA/apg/practices/landmark-regions/)
* [MDN: ARIA Landmark Roles](https://developer.mozilla.org/en-US/docs/Web/Accessibility/ARIA/Roles/complementary_role)
