# HTML Article (`<article>`)

The HTML `<article>` element represents a self-contained piece of content that can be independently distributed, reused, or understood outside the context of the surrounding page.

It is commonly used for:

* Blog posts
* News articles
* Forum posts
* Product reviews
* User comments
* Community discussions
* Independent documentation entries

The `<article>` element is part of semantic HTML. It helps browsers, search engines, and assistive technologies understand that a particular piece of content has independent meaning.

# 🗁 Basic Syntax

```html
<article>
  <h2>Understanding HTML</h2>

  <p>
    HTML is the standard markup language used
    to structure content on the web.
  </p>
</article>
```

### 🖆 Syntax Explanation

* `<article>` defines an independent piece of content.
* The heading describes the article's subject.
* The content inside the element belongs to that article.
* `</article>` closes the article element.

An article should make sense independently of the surrounding page.

# 🗁 Understanding the Article Element

Imagine a blog homepage displaying several posts.

Each post has its own title, publication date, author, and content. Each post can be shared or displayed independently.

The `<article>` element is appropriate for representing each post.

```html
<main>
  <h1>Latest Articles</h1>

  <article>
    <h2>Introduction to HTML</h2>
    <p>Learn the fundamentals of HTML.</p>
    <a href="/articles/html-introduction">Read more</a>
  </article>

  <article>
    <h2>Introduction to CSS</h2>
    <p>Learn how CSS styles web pages.</p>
    <a href="/articles/css-introduction">Read more</a>
  </article>

  <article>
    <h2>Introduction to JavaScript</h2>
    <p>Learn how JavaScript adds interactivity.</p>
    <a href="/articles/javascript-introduction">Read more</a>
  </article>
</main>
```

Each article represents an independent piece of content within the page.

# 🗁 Complete Article Example

The following example demonstrates a complete blog article.

```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Understanding Semantic HTML</title>
</head>
<body>

  <header>
    <h1>Developer Blog</h1>

    <nav aria-label="Main navigation">
      <a href="/">Home</a>
      <a href="/articles">Articles</a>
      <a href="/about">About</a>
    </nav>
  </header>

  <main>
    <article>
      <header>
        <h1>Understanding Semantic HTML</h1>

        <p>
          Published on
          <time datetime="2026-11-08">November 8, 2026</time>
        </p>

        <p>By John Doe</p>
      </header>

      <section>
        <h2>Introduction</h2>

        <p>
          Semantic HTML uses meaningful elements
          to describe the structure of web documents.
        </p>
      </section>

      <section>
        <h2>Benefits</h2>

        <p>
          Semantic HTML improves accessibility,
          maintainability, and document organization.
        </p>
      </section>

      <footer>
        <p>Category: Web Development</p>
      </footer>
    </article>
  </main>

  <footer>
    <p>&copy; 2026 Developer Blog</p>
  </footer>

</body>
</html>
```

# 🗁 Article with Header and Footer

An `<article>` can contain its own `<header>` and `<footer>`.

These elements describe the article rather than the entire webpage.

### 🖆 Article Header

The article header may contain:

* Article title
* Author information
* Publication date
* Category
* Introductory information

```html
<article>
  <header>
    <h2>Learning JavaScript</h2>

    <p>
      Written by
      <a href="/authors/alex">Alex</a>
    </p>

    <p>
      Published:
      <time datetime="2026-11-08">November 8, 2026</time>
    </p>
  </header>

  <p>
    JavaScript is a programming language
    used to create interactive web applications.
  </p>
</article>
```

### 🖆 Article Footer

The article footer may contain:

* Author details
* Tags
* Related links
* Copyright information
* Article metadata

```html
<article>
  <h2>Introduction to CSS</h2>

  <p>CSS controls the visual presentation of web pages.</p>

  <footer>
    <p>Tags: HTML, CSS, Frontend</p>
  </footer>
</article>
```

An article's `<footer>` is different from the page-level `<footer>`. Its scope is limited to the article.

# 🗁 Article with Sections

An article can contain multiple `<section>` elements to organize its content into thematic groups.

```html
<article>
  <header>
    <h1>Complete HTML Guide</h1>
    <p>A practical introduction to HTML.</p>
  </header>

  <section>
    <h2>HTML Introduction</h2>
    <p>HTML provides the structure of web pages.</p>
  </section>

  <section>
    <h2>HTML Elements</h2>
    <p>Elements define the structure and meaning of content.</p>
  </section>

  <section>
    <h2>HTML Attributes</h2>
    <p>Attributes provide additional information about elements.</p>
  </section>

  <footer>
    <p>End of article.</p>
  </footer>
</article>
```

Use sections when the article contains distinct topics that benefit from their own headings.

# 🗁 Multiple Articles on a Page

A page can contain multiple independent articles.

This is common on:

* Blog homepages
* News websites
* Product review pages
* Community forums
* Discussion platforms

```html
<main>
  <h1>Latest News</h1>

  <article>
    <h2>New Web Technologies</h2>
    <p>Explore emerging technologies in web development.</p>
    <a href="/news/web-technologies">Read article</a>
  </article>

  <article>
    <h2>Modern CSS Features</h2>
    <p>Discover useful modern CSS capabilities.</p>
    <a href="/news/modern-css">Read article</a>
  </article>

  <article>
    <h2>JavaScript Development</h2>
    <p>Learn about modern JavaScript development practices.</p>
    <a href="/news/javascript">Read article</a>
  </article>
</main>
```

Each article should represent a meaningful, independently identifiable content item.

# 🗁 Nested Articles

An article can contain another article when the nested content is independently meaningful.

For example, a forum discussion can contain a main post and independent replies.

```html
<main>
  <article>
    <h1>How do you structure a web application?</h1>

    <p>
      I am looking for recommendations
      for organizing a frontend project.
    </p>

    <footer>
      <p>Posted by Alex</p>
    </footer>

    <section>
      <h2>Replies</h2>

      <article>
        <h3>Reply from Sam</h3>
        <p>Consider organizing your code by feature.</p>
      </article>

      <article>
        <h3>Reply from Maria</h3>
        <p>A component-based architecture can also help.</p>
      </article>
    </section>
  </article>
</main>
```

Nested articles are appropriate when each reply is a self-contained contribution.

Avoid nesting articles simply to achieve visual layout.

# 🗁 Article vs Section

Both elements organize content, but their purposes are different.

| Feature | `<article>` | `<section>` |
| --- | --- | --- |
| Purpose | Independent content | Thematic content grouping |
| Independence | Can stand alone | Usually belongs to a larger document |
| Typical usage | Blog posts, news, comments | Chapters, topics, page sections |
| Can contain sections | Yes | Yes |
| Can contain articles | Yes | Yes |

### 🖆 Article Example

```html
<article>
  <h2>Understanding JavaScript</h2>
  <p>JavaScript is a programming language.</p>
</article>
```

### 🖆 Section Example

```html
<section>
  <h2>JavaScript Fundamentals</h2>
  <p>Learn variables, functions, and objects.</p>
</section>
```

A useful way to decide:

* Use `<article>` when the content can stand independently.
* Use `<section>` when the content represents a thematic part of a larger document.

# 🗁 Article vs Div

| Feature | `<article>` | `<div>` |
| --- | --- | --- |
| Semantic meaning | Independent content | Generic container |
| Document structure | Identifies self-contained content | Adds no inherent content meaning |
| Typical usage | Blog posts and comments | Layout and styling |
| Accessibility | Exposes article semantics | Usually has no semantic role |

Example:

```html
<!-- Semantic article -->
<article>
  <h2>HTML Fundamentals</h2>
  <p>Learn the basics of HTML.</p>
</article>

<!-- Generic layout container -->
<div class="card">
  <h2>HTML Course</h2>
  <p>Explore our HTML learning resources.</p>
</div>
```

Choose the element based on the meaning of the content, not its appearance.

# 🗁 Accessibility

Semantic articles help assistive technologies identify independent content units.

### 🖆 Use Descriptive Headings

Each article should generally have a meaningful heading.

```html
<article>
  <h2>Understanding Web Accessibility</h2>

  <p>
    Web accessibility helps make websites
    usable by people with different abilities.
  </p>
</article>
```

### 🖆 Use Appropriate Heading Levels

Choose heading levels according to the document hierarchy.

```html
<main>
  <h1>Technology News</h1>

  <article>
    <h2>Latest HTML Features</h2>

    <section>
      <h3>New Elements</h3>
      <p>Explore relevant HTML features.</p>
    </section>
  </article>
</main>
```

Do not select heading levels solely for their default visual appearance.

### 🖆 Use the Time Element for Dates

The `<time>` element provides a machine-readable representation of dates and times.

```html
<article>
  <h2>Web Development Trends</h2>

  <p>
    Published:
    <time datetime="2026-11-08">November 8, 2026</time>
  </p>
</article>
```

The `datetime` attribute contains the machine-readable value, while the element's text provides the human-readable presentation.

### 🖆 Avoid Unnecessary ARIA

The `<article>` element already has article semantics.

```html
<!-- Unnecessary -->
<article role="article">
  <h2>HTML Guide</h2>
</article>

<!-- Preferred -->
<article>
  <h2>HTML Guide</h2>
</article>
```

# 🗁 Best Practices

* Use `<article>` for independently meaningful content.
* Include a descriptive heading.
* Use `<header>` for article introductory information when appropriate.
* Use `<footer>` for article-related metadata or supporting information.
* Use `<section>` to organize an article into thematic parts.
* Use `<time>` for publication dates and times.
* Use nested articles only when the nested content is independently meaningful.
* Keep heading levels consistent with the document hierarchy.
* Use `<div>` for generic layout containers.
* Choose semantic elements based on content meaning rather than visual appearance.

# 🗁 Common Mistakes

### 🖆 Using Article for Every Container

```html
<!-- Avoid -->
<article class="page-container">
  <div class="sidebar">Sidebar</div>
  <div class="content">Content</div>
</article>
```

A generic layout container does not automatically represent an independent article.

Use `<div>`, `<main>`, or other appropriate semantic elements.

### 🖆 Using Section Instead of Article

If a blog post is independently meaningful and can be distributed separately, `<article>` is generally more appropriate than `<section>`.

### 🖆 Missing Article Heading

```html
<!-- Avoid when a descriptive heading is appropriate -->
<article>
  <p>HTML provides the structure of web pages.</p>
</article>
```

Provide a meaningful heading to identify the article's subject.

### 🖆 Confusing Article Footer with Page Footer

An article-level `<footer>` describes its containing article.

A page-level `<footer>` describes the document or its containing section.

Both are valid, but their scope is different.

# 🗁 Practical Exercises

### 🖆 Exercise: Create a Blog Post

Build a blog article containing:

* Article title
* Author information
* Publication date
* Introduction
* Multiple content sections
* Article footer with tags

Use `<article>`, `<header>`, `<section>`, `<footer>`, and `<time>`.

### 🖆 Exercise: Create a News Homepage

Build a news page containing:

* A main page heading
* Three independent news articles
* A title and summary for each article
* A link to read each full article

Use multiple `<article>` elements inside `<main>`.

### 🖆 Exercise: Create a Forum Discussion

Build a forum discussion containing:

* One main discussion post
* Two independent replies
* Author information
* Appropriate article headings

Use nested `<article>` elements where they represent independent contributions.

# 🗁 Interview Questions

**What is the purpose of the `<article>` element?**

It represents self-contained content that can be independently distributed or reused.

**Where is the `<article>` element commonly used?**

It is commonly used for blog posts, news articles, forum posts, comments, and product reviews.

**What is the difference between `<article>` and `<section>`?**

An article represents independent content, while a section represents a thematic grouping of related content.

**Can an article contain sections?**

Yes. Sections can organize the content of an article into meaningful topics.

**Can a page contain multiple article elements?**

Yes. For example, a blog homepage can contain multiple independent article elements.

**Can articles be nested?**

Yes. Nested articles are appropriate when the inner content is independently meaningful, such as replies within a forum discussion.

**What is the purpose of an article-level footer?**

It contains information related to its article, such as author details, tags, or publication metadata.

**Is `<article>` a replacement for `<div>`?**

No. `<article>` represents independent content, while `<div>` is a generic container.

# 🗁 References

* [MDN: `<article>` Element](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/article)
* [WHATWG: The `article` Element](https://html.spec.whatwg.org/multipage/sections.html#the-article-element)
* [MDN: `<time>` Element](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/time)
* [W3C WAI: Page Structure](https://www.w3.org/WAI/tutorials/page-structure/)
