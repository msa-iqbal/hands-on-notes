# 📝 Hands-on HTML5 Notes

A concise collection of self-made HTML5 notes, code snippets, and best practices.

Maintained by [Md. Shah Alam Iqbal](https://github.com/msa-iqbal) as part of the [T-Insights](https://github.com/thetinsights) initiative.

Perfect for learners and developers who need a quick, practical HTML5 reference and fast lookup guide.

- Covers key HTML5 topics with clear explanations
- Copy-ready code snippets
- Best practices for accessibility & SEO
- Beginner-friendly and continuously updated

---

# Table of Contents

| #   | Topic                                                      | Subtopics                                         |
| --- | ---------------------------------------------------------- | ------------------------------------------------- |
| 1   | [Introduction](#introduction)                              | Overview, Features, Structure, Tags, Attributes   |
| 2   | [HTML5 Head Tags](#html5-head-tags)                        | Metadata, SEO, Stylesheets, Icons, Fonts, Scripts |
| 3   | [CSS in HTML](#css-styles-in-html)                         | Inline, Internal, External                        |
| 4   | [Block vs Inline Elements](#html-block-vs-inline-elements) | Block, Inline                                     |
| 5   | [Comments](#commenting-in-html)                            | Single-line, Multi-line                           |
| 6   | [HTML Entities](#html-entities)                            | Character, Numeric, Named                         |
| 7   | [Semantic Elements](#semantic-elements)                    | All Semantic Elements                             |
| 8   | [Text & Inline Elements](#text--inline-elements)           | Headings, Text, Superscript, Subscript, Code      |
| 9   | [Links](#anchor-text--hyperlinks)                          | Internal, External                                |
| 10  | [Colors](#colors-in-html--css)                             | Named, RGB, RGBA, HEX, HSL, Gradient              |
| 11  | [Images](#image)                                           | Attributes, Favicon, Image Map                    |
| 12  | [Canvas](#html-canvas)                                     | Drawing, Shapes, Animations, Interactivity        |
| 13  | [Audio & Video](#audio--video)                             | Audio, Video, Subtitles, Captions                 |
| 14  | [Lists](#lists)                                            | Ordered, Unordered, Definition, Nested            |
| 15  | [Tables](#tables)                                          | Attributes, Sections, Merging, Captions           |
| 16  | [Forms](#forms)                                            | Inputs, Textarea, Dropdowns                       |
| 17  | [iFrame](#html-iframe)                                     | Internal, External                                |
| 18  | [File Paths](#html-file-path)                              | Absolute, Relative, Root-relative                 |
| 19  | [Web Components](#html-web-components)                     | Templates, Slots, Shadow DOM                      |
| 20  | [HTML5 APIs](#html5-apis)                                  | Overview                                          |
| 21  | [Global Attributes](#global-attributes)                    | Overview                                          |
| 22  | [Web Accessibility](#web-accessibility)                    | Guidelines, Best Practices                        |

---

# Introduction

HTML5 is the latest version of **HyperText Markup Language**, the standard for structuring and presenting content on the web. It introduces **semantic elements**, **built-in multimedia support**, **graphics APIs**, **offline storage**, and **powerful APIs** that make modern web apps possible.

### ၊၊||၊ HTML5 Features

- **Semantic Elements:** New elements like `<header>`, `<footer>`, `<nav>`, `<article>`, and `<section>` provide more meaningful structure to web pages.
- **Multimedia:** Built-in support for audio and video with `<audio>` and `<video>` tags.
- **Graphics:** `<canvas>` element for drawing graphics and animations, and SVG for scalable vector graphics.
- **Offline Storage:** Local storage and IndexedDB for storing data locally on the user's device.
- **Web APIs:** Geolocation, web sockets, drag-and-drop, and more.

### ၊၊||၊ Document Structure & Boilerplate

```html
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <meta http-equiv="X-UA-Compatible" content="IE=edge" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <meta name="description" content="Advanced HTML5 Page" />
    <title>HTML5 Cheat Sheet</title>
    <link rel="stylesheet" href="styles.css" />
  </head>
  <body>
    <!-- Content Here -->

    <script src="script.js"></script>
  </body>
</html>
```

**Explanation:**

- The `<!DOCTYPE html>` declaration defines this document to be HTML5
- The `<html>` element is the root element of an HTML page
- The `<head>` element contains meta information about the document
- The `<body>` element contains the visible page content

### ၊၊||၊ HTML Tag & Attributes

In HTML, **tags** define the structure and type of content on a webpage, while **attributes** provide additional information or modify the behavior of those elements.

**Example**

```html
<img src="logo.png" alt="Company Logo" width="150" height="150" />
```

**Explanation:**

- `<img>` — The image tag (self-closing element).
- `src="logo.png"` — Attribute specifying the image source file.
- `alt="Company Logo"` — Attribute that provides alternative text for accessibility.
- `width="150"` and `height="150"` — Attributes that define the display size of the image.

<!-- START "Jump to Top"-->
<p align="right">
  <a href="#table-of-contents">Jump to Top ▲</a>
</p>
<!-- END "Jump to Top" -->

# HTML5 Head Tags

The `<head>` section of an HTML5 document contains metadata, links to resources, and information used by browsers, search engines, and social platforms. Below is a structured overview of commonly used `<head>` elements.

### 1. Document Metadata

```html
<!DOCTYPE html>
<!-- Defines document as HTML5 -->
<html lang="en">
  <!-- Declares the language -->
  <head>
    <meta charset="UTF-8" />
    <!-- Character encoding -->
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <!-- Ensures responsiveness on all devices -->
    <meta http-equiv="X-UA-Compatible" content="IE=edge" />
    <!-- Compatibility with older IE versions -->
  </head>
</html>
```

### 2. SEO and General Information

```html
<title>Page Title</title>
<!-- Title displayed in browser tab -->
<meta name="description" content="Concise description of the page content" />
<!-- Improves SEO ranking -->
<meta name="keywords" content="HTML5, Web Development, Tutorial" />
<!-- Keywords for search engines (less used today) -->
<meta name="author" content="Your Name" />
<!-- Author or organization name -->
```

### 3. Social Media (Open Graph & Twitter Cards)

```html
<!-- Open Graph -->
<meta property="og:title" content="Page Title" />
<meta
  property="og:description"
  content="Concise description of the page content"
/>
<meta property="og:image" content="https://example.com/image.jpg" />
<meta property="og:url" content="https://example.com/page.html" />
<meta property="og:type" content="website" />

<!-- Twitter Cards -->
<meta name="twitter:card" content="summary_large_image" />
<meta name="twitter:title" content="Page Title" />
```

### 4. Stylesheets, Icons, and Fonts

```html
<link rel="stylesheet" href="styles.css" />
<!-- External CSS -->
<link rel="icon" type="image/png" href="favicon.png" />
<!-- Favicon for browser tab -->
<link rel="preconnect" href="https://fonts.googleapis.com" />
<!-- Improves font loading speed -->
<link rel="stylesheet" href="https://fonts.googleapis.com/css?family=Roboto" />
<!-- Example of Google Fonts -->
```

### 5. Scripts and Base URL

```html
<head>
  <script src="script.js" defer></script>
  <!-- External JS (deferred for performance) -->
  <base href="https://example.com/" />
  <!-- Base URL for relative links -->
</head>
```

<!-- START "Jump to Top"-->
<p align="right">
  <a href="#table-of-contents">Jump to Top ▲</a>
</p>
<!-- END "Jump to Top" -->

# CSS Styles in HTML

**CSS (Cascading Style Sheets)** is used to control the presentation and layout of HTML elements. Styles can be applied to HTML in three main ways:

### 1. Inline CSS

Applied directly to an element using the `style` attribute.

```html
<p style="color: blue; font-size: 18px;">This is inline styling.</p>
```

✅ Quick and easy for single elements.  
❌ Not recommended for large projects (hard to maintain).

### 2. Internal CSS

Defined inside the `<style>` tag within the `<head>` section.

```html
<head>
  <style>
    p {
      color: green;
      font-size: 20px;
    }
  </style>
</head>
<body>
  <p>This paragraph uses internal CSS.</p>
</body>
```

✅ Good for small projects or single-page designs.  
❌ Styles are limited to that specific HTML file.

### 3. External CSS

Stored in a separate `.css` file and linked to HTML using the `<link>` tag.

```html
<head>
  <link rel="stylesheet" href="styles.css" />
</head>
<body>
  <p>This paragraph uses external CSS.</p>
</body>
```

✅ Best for large projects with multiple pages.  
✅ Easy to maintain and update.  
✅ Styles can be reused across multiple HTML files.

<!-- START "Jump to Top"-->
<p align="right">
  <a href="#table-of-contents">Jump to Top ▲</a>
</p>
<!-- END "Jump to Top" -->

# HTML Block vs Inline Elements

HTML elements are categorized based on how they behave in the document flow: **block-level** or **inline.**

### 1. Block-Level Elements

- Always start on a new line.
- Take up the full width of their parent container by default.
- Can contain other block-level or inline elements.

**Common Examples:**
`<div>`, `<p>`, `<h1> – <h6>`, `<section>`, `<article>`, `<header>`, `<footer>`, `<ul>`, `<ol>`, `<li>`

**Example**

```html
<div>
  <h1>Heading</h1>
  <p>This is a paragraph inside a div.</p>
</div>
```

### 2. Inline Elements

- Do not start on a new line.
- Only take up as much width as necessary.
- Cannot contain block-level elements.

**Common Examples:**
`<span>`, `<a>`, `<img>`, `<strong>`, `<em>`, `<small>`, `<sub>`, `<sup>`, `<code>`, `<b>`, `<i>`

**Example**

```html
<p>This is a <span>span</span> inside a paragraph.</p>
```

### ❏❏❏ Display Property

The `display` property in CSS can be used to change the default behavior of an element, making it block-level or inline-level.

**Example**

```css
/* Block-level */
display: block;

/* Inline-level */
display: inline;
```

<!-- START "Jump to Top"-->
<p align="right">
  <a href="#table-of-contents">Jump to Top ▲</a>
</p>
<!-- END "Jump to Top" -->

# Commenting in HTML

Comments in HTML are written between `<!--` and `-->` and are ignored by browsers. They are used to explain code, organize structure, or temporarily disable elements without deleting them. Good comments improve readability, collaboration, and maintenance of HTML documents.

**Rules & Characteristics:**

- A comment starts with `<!--` and ends with `-->`.
- The browser `ignores` everything inside the comment.
- Comments can span `single` or `multiple` lines.
- Avoid using `--` inside comments (invalid in HTML).

### ၊၊||၊ Single-line comment

```html
<!-- This is a single-line comment -->
```

### ၊၊||၊ Multi-line comment

```html
<!--
  This is a multi-line comment.
  It spans multiple lines.
-->
```

### ၊၊||၊ Commenting out code

```html
<!-- <img src="logo.png" alt="Logo"> -->
```

<!-- START "Jump to Top"-->
<p align="right">
  <a href="#table-of-contents">Jump to Top ▲</a>
</p>
<!-- END "Jump to Top" -->

# HTML Entities

HTML entities are special sequences of characters that represent characters that cannot be directly typed into HTML code. They are used to display special characters, symbols, and reserved characters in HTML documents.

### 1. Character Entities

These entities represent characters that cannot be typed directly in HTML.

**Example**

```html
<p>This is an ampersand: &amp;</p>
<!-- it's looks like: & -->
<p>This is a less-than sign: &lt;</p>
<!-- it's looks like: < -->
<p>This is a greater-than sign: &gt;</p>
<!-- it's looks like: > -->
<p>This is a double quote: &quot;</p>
<!-- it's looks like: " -->
<p>This is an apostrophe: &apos;</p>
<!-- it's looks like: ' -->
```

### 2. Numeric Entities

These entities represent characters using their Unicode numeric code.

**Example**

```html
<p>This is an ampersand: &#38;</p>
<!-- it's looks like: & -->
```

### 3. Named Entities

These entities represent characters using their named code.

**Example**

```html
<p>This is an ampersand: &amp;</p>
<!-- it's looks like: & -->
```

### ❏❏❏ Common Entities

| Character            | Entity Code         | Output |
| -------------------- | ------------------- | ------ |
| Less than            | `&lt;`              | <      |
| Greater than         | `&gt;`              | >      |
| Ampersand            | `&amp;`             | &      |
| Quotation mark       | `&quot;`            | "      |
| Apostrophe           | `&apos;` or `&#39;` | '      |
| Non-breaking space   | `&nbsp;`            | ␣      |
| Copyright            | `&copy;`            | ©      |
| Registered Trademark | `&reg;`             | ®      |
| Trademark            | `&trade;`           | ™      |
| Euro                 | `&euro;`            | €      |
| Dollar               | `&#36;`             | $      |
| Pound                | `&pound;`           | £      |
| Yen                  | `&yen;`             | ¥      |
| Left Arrow           | `&larr;`            | ←      |
| Up Arrow             | `&uarr;`            | ↑      |
| Right Arrow          | `&rarr;`            | —      |
| Down Arrow           | `&darr;`            | ↓      |
| Section              | `&sect;`            | §      |
| Degree               | `&deg;`             | °      |
| Bullet               | `&bull;`            | •      |
| Middle dot           | `&middot;`          | ·      |

#### [✦ See more examples](https://gist.github.com/msa-iqbal/4e5a5964148ddb4bacf274e9305de9a0)

<!-- START "Jump to Top"-->
<p align="right">
  <a href="#table-of-contents">Jump to Top ▲</a>
</p>
<!-- END "Jump to Top" -->

# Semantic Elements

Semantic elements clearly describe their **meaning and purpose** both to the browser and to developers. Using semantic elements improves **readability, accessibility, and SEO.**

| Element        | Purpose                                |
| -------------- | -------------------------------------- |
| `<header>`     | Page or section header                 |
| `<footer>`     | Page or section footer                 |
| `<nav>`        | Primary navigation                     |
| `<main>`       | Main content area                      |
| `<article>`    | Independent self-contained content     |
| `<section>`    | Thematic grouping of content           |
| `<aside>`      | Side content like ads, tips            |
| `<figure>`     | Illustrations, diagrams, media         |
| `<figcaption>` | Caption for `<figure>`                 |
| `<details>`    | Disclosure widget for details          |
| `<summary>`    | Visible heading for `<details>`        |
| `<mark>`       | Highlighted text                       |
| `<time>`       | Machine-readable date/time             |
| `<dialog>`     | Modal or pop-up window                 |
| `<template>`   | Reusable HTML chunks (inactive)        |
| `<slot>`       | Web Components content insertion point |

#### [⿻ Live Example](https://thetinsights.github.io/web-quickref/live/html/html-semantic-tags.html)

#### ⿻ Example Usage —

```html
<body>
  <header>
    <h1>My Blog</h1>
    <nav>
      <a href="#">Home</a> | <a href="#">Articles</a> |
      <a href="#">About</a>
    </nav>
  </header>

  <main>
    <section>
      <h2>Articles</h2>
      <article>
        <h3>Hello World</h3>
        <p>This is my first blog post.</p>
      </article>
    </section>

    <aside>Sidebar content</aside>
  </main>

  <footer>&copy; 2025 My Blog</footer>
</body>
```

> [!NOTE]
>
> - **Accessibility:** Screen readers and assistive devices can better understand content structure.
> - **SEO-Friendly:** Search engines can interpret the page more accurately.
> - **Maintainability:** Makes code easier to read and organize.
> - **Consistency:** Helps in structuring modern web layouts effectively.

<!-- START "Jump to Top"-->
<p align="right">
  <a href="#table-of-contents">Jump to Top ▲</a>
</p>
<!-- END "Jump to Top" -->

# Text & Inline Elements

Inline elements are used to format or emphasize text content without breaking the flow of a paragraph or block. They **do not start on a new line** and occupy only the width needed.

### ၊၊||၊ Headings

| Tag              | Description                       | Example               |
| ---------------- | --------------------------------- | --------------------- |
| `<h1>` to `<h6>` | Headings from largest to smallest | `<h1>Main Title</h1>` |

### ၊၊||၊ Text & Emphasis

| Tag                 | Description              | Example                                        |
| ------------------- | ------------------------ | ---------------------------------------------- |
| `<p>`               | Paragraph                | `<p>This is a paragraph.</p>`                  |
| `<br>`              | Line break               | `Hello<br />World`                             |
| `<hr>`              | Horizontal rule          | `<hr />`                                       |
| `<strong>`          | Strong importance (bold) | `<strong>Important</strong>`                   |
| `<em>`              | Emphasis (italic)        | `<em>Note</em>`                                |
| `<b>`, `<i>`, `<u>` | Styling without meaning  | `<b>Bold</b>, <i>Italic</i>, <u>Underline</u>` |
| `<small>`           | Smaller text             | `<small>Fine print</small>`                    |
| `<mark>`            | Highlight text           | `<mark>Highlighted</mark>`                     |

### ၊၊||၊ Superscript & Subscript

| Tag     | Description | Example              |
| ------- | ----------- | -------------------- |
| `<sup>` | Superscript | `E = mc<sup>2</sup>` |
| `<sub>` | Subscript   | `H<sub>2</sub>O`     |

### ၊၊||၊ References & Definitions

| Tag      | Description               | Example                                                   |
| -------- | ------------------------- | --------------------------------------------------------- |
| `<abbr>` | Abbreviation with tooltip | `<abbr title="HyperText Markup Language">HTML</abbr>`     |
| `<dfn>`  | Defines a term            | `<dfn>API</dfn> means Application Programming Interface.` |
| `<cite>` | Citation source           | `— <cite>Albert Einstein</cite>`                          |

### ၊၊||၊ Code & Output

| Tag      | Description       | Example                          |
| -------- | ----------------- | -------------------------------- |
| `<code>` | Inline code       | `<code>console.log('hi')</code>` |
| `<pre>`  | Preformatted text | `<pre>Line 1\n Line 2</pre>`     |
| `<kbd>`  | Keyboard input    | `<kbd>Ctrl</kbd> + <kbd>C</kbd>` |
| `<samp>` | Sample output     | `<samp>Error</samp>`             |
| `<var>`  | Variable          | `<var>x</var> = 10`              |

### ၊၊||၊ Miscellaneous

| Tag            | Description         | Example                                     |
| -------------- | ------------------- | ------------------------------------------- |
| `<wbr>`        | Optional word break | `Supercalifragilistic<wbr />expialidocious` |
| `<blockquote>` | Long quotation      | `<blockquote>“Quote”</blockquote>`          |

#### [⿻ Live Example](https://thetinsights.github.io/web-quickref/live/html/html-text-and-inline-elements.html)

<!-- START "Jump to Top"-->
<p align="right">
  <a href="#table-of-contents">Jump to Top ▲</a>
</p>
<!-- END "Jump to Top" -->

# Anchor Text & Hyperlinks

The `<a>` (anchor) element is used to create **hyperlinks**, which allow users to navigate from one page to another, or to specific sections within a page. The **anchor text** is the visible, clickable text of the link.

**Syntax**

```html
<a href="https://example.com" target="_blank" title="title text"> Visit </a>
```

#### Attributes of Hyperlink

| Attribute  | Details                                                                                                                                                                                                                                                                                                                         |
| ---------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `href`     | Specifies the destination address. It can be an **absolute URL**, a **relative URL**, or the **ID of an anchor** within the same page.                                                                                                                                                                                          |
| `hreflang` | Specifies the language of the linked resource. Uses language codes from [BCP 47](https://www.ietf.org/rfc/bcp/bcp47.txt) for HTML5 and [RFC 1766](https://www.ietf.org/rfc/rfc1766.txt) for HTML4.                                                                                                                              |
| `rel`      | Specifies the relationship between the current document and the linked resource. Values must be [defined in the HTML5 specification](https://www.w3.org/TR/2014/REC-html5-20141028/links.html#linkTypes) or [registered in the Microformats wiki](http://microformats.org/wiki/existing-rel-values#HTML5_link_type_extensions). |
| `target`   | Specifies where to open the linked document (e.g., same tab, new tab, frame).                                                                                                                                                                                                                                                   |
| `title`    | Provides extra information about the link, shown as a tooltip on hover.                                                                                                                                                                                                                                                         |
| `download` | Instructs the browser to download the linked resource instead of navigating to it. The value can specify the default file name.                                                                                                                                                                                                 |

#### Parameters of `target` Attribute (for Hyperlinks)

| **Parameter** | **Attribute** | **Details**                                                                            |
| ------------- | ------------- | -------------------------------------------------------------------------------------- |
| `_blank`      | `target`      | Opens the linked document in a **new window or tab**.                                  |
| `_self`       | `target`      | Opens the linked document in the **same window/tab** (default).                        |
| `_parent`     | `target`      | Opens the linked document in the **parent frame**.                                     |
| `_top`        | `target`      | Opens the linked document in the **full body of the window** (breaking out of frames). |
| _framename_   | `target`      | Opens the linked document in a **specific named frame**.                               |

#### [🕮 Live Example](https://thetinsights.github.io/web-quickref/live/html/html-links-and-anchors.html)

#### ⿻ Example Usage —

ⵌ Link to another website

```html
<a href="https://abc.com" target="_blank" rel="noopener noreferrer"> Visit </a>
```

ⵌ Link to an email address

```html
<a href="mailto:info@example.com">Contact Us</a>
```

ⵌ Link to a phone number

```html
<a href="tel:+1234567890">Call Us</a>
```

ⵌ Download a file

```html
<a href="file.pdf" download>Download File</a>
```

ⵌ Link to a specific part of same page using a named anchor

```html
<a href="#contact">Go to Contact Section</a>

<section id="contact">
  <h2>Contact Us</h2>
</section>
```

ⵌ Link to a specific part of another page

```html
<a href="https://www.example.com#section1">Go to Section 1 on Example</a>
```

> [!TIP]
>
> - Use `target="_blank"` **only when necessary** (external links, downloads).
> - Combine `target="_blank"` with `rel="noopener noreferrer"` for security.
> - Keep URLs clean, short, and human-readable when possible.
> - Keep anchor text as short but clear — avoid long sentences as links.
> - Ensure the link anchor text **matches the destination content** for good UX and SEO.
> - Use keywords in anchor text naturally (helpful for accessibility and SEO).
> - Avoid generic phrases like "link," "here," or "this" unless context is clear.

<!-- START "Jump to Top"-->
<p align="right">
  <a href="#table-of-contents">Jump to Top ▲</a>
</p>
<!-- END "Jump to Top" -->

# Colors in HTML & CSS

Colors in HTML define the appearance of text, backgrounds, borders, and other elements on a webpage. They are typically applied using CSS and can be defined in several formats:

### 1. Named Colors

140+ predefined color names (e.g., red, blue, green, yellow, etc.).

```html
<p style="color: red;">This text is red</p>
```

[Explore colors —](https://www.w3schools.com/colors/colors_names.asp)

### 2. RGB Colors

Defines colors using **`Red`, `Green`, `Blue`** values (0–255).

```html
<p style="color: rgba(255, 0, 0, 1);">This text is red</p>
```

[Explore colors —](https://www.w3schools.com/css/css_colors_rgb.asp)

### 3. RGBA Colors

Same as RGB but includes **Alpha** (opacity).

```html
<p style="color: rgba(255, 0, 0, 0.5);">This text is red with 50% opacity</p>
```

[Explore colors —](https://www.w3schools.com/css/css_colors_rgb.asp)

### 4. HEX Colors

Hexadecimal (6 or 3 digits) represents **`Red`, `Green`, `Blue`** (RRGGBB).

```html
<p style="color: #ff0000;">This text is red</p>
```

[Explore colors —](https://htmlcolorcodes.com/)

### 5. CMYK Colors

CMYK (**`Cyan`, `Magenta`, `Yellow`, `Black`**) is used in printing, while screens use RGB.

Note: CMYK isn’t supported in HTML but is proposed for CSS4.

```html
<p style="color: cmyk(0, 100%, 100%, 0);">This text is red</p>
```

[Explore colors —](https://www.w3schools.com/colors/colors_cmyk.asp)

### 6. HSL Colors

Hue (0–360), Saturation (0–100%), Lightness (0–100%).

```html
<p style="color: hsl(0, 100%, 50%);">This text is red</p>
```

[Explore colors —](https://www.w3schools.com/html/html_colors_hsl.asp)

### 7. HSLA Colors

Same as HSL but includes **Alpha** (opacity).

```html
<p style="color: hsla(0, 100%, 50%, 0.5);">This text is red with 50% opacity</p>
```

[Explore colors —](https://www.w3schools.com/html/html_colors_hsl.asp)

### 8. Gradient Colors

Linear, radial, conic, etc. (CSS3).

```html
<p style="background: linear-gradient(to right, red, blue);">
  This text has a gradient background
</p>
```

[Explore gradients —](https://www.w3schools.com/css/css3_gradients.asp)

#### [⿻ Live Example](https://thetinsights.github.io/web-quickref/live/html/html-colors.html)

### ❏❏❏ CSS Opacity

The **`opacity`** property in CSS controls the transparency of an entire element — including its background, text, and content.

- Value must be between **`0.0`** (fully transparent) and **`1.0`** (fully opaque).

**Example**

```html
<p style="background-color: #FF0000; opacity: 0.4;">Bangladesh</p>
```

> [!TIP]
>
> - Use **named colors** for simplicity and readability.
> - Use **RGB/RGBA** for precise control and transparency on screens.
> - Use **HEX** for consistency, compatibility, and branding.
> - Use **HSL/HSLA** for easy adjustments and better accessibility.
> - Use **gradients** for modern, visually appealing designs.
> - Use **CMYK** for printing (not supported in HTML).
> - Use **opacity** to control transparency.
> - Always check **contrast** to ensure readability.

<!-- START "Jump to Top"-->
<p align="right">
  <a href="#table-of-contents">Jump to Top ▲</a>
</p>
<!-- END "Jump to Top" -->

# Image

Images can improve the design and the appearance of a web page.

#### Common Image Formats

Here are the most common image file types, which are supported in all browsers.

| Abbreviation | File Format                           | File Extension                             |
| ------------ | ------------------------------------- | ------------------------------------------ |
| JPEG         | Joint Photographic Expert Group image | `.jpg`, `.jpeg`, `.jfif`, `.pjpeg`, `.pjp` |
| PNG          | Portable Network Graphics             | `.png`                                     |
| APNG         | Animated Portable Network Graphics    | `.apng`                                    |
| GIF          | Graphics Interchange Format           | `.gif`                                     |
| ICO          | Microsoft Icon                        | `.ico`, `.cur`                             |
| SVG          | Scalable Vector Graphics              | `.svg`                                     |

**Syntax**

```html
<img src="img.jpg" alt="Description" width="600" height="400" loading="lazy" />
```

The `<img>` tag is **empty** (no closing tag) and only contains attributes.

- `src` — Specifies the path to the image
- `alt` — Provides alternative text for accessibility
- `width` — Specifies the width of the image
- `height` — Specifies the height of the image
- `loading` — Specifies if the image should be loaded immediately or on demand
- `decoding` — Specifies the decoding algorithm to use for the image
- `fetchpriority` — Specifies the loading priority of the image
- `referrerpolicy` — Specifies the referrer policy to use for the image
- `crossorigin` — Specifies the CORS settings for the image
- `sizes` — Specifies the sizes of the image
- `srcset` — Specifies the source set of the image
- `usemap` — Specifies the map of the image
- `ismap` — Specifies if the image is a map
- `longdesc` — Specifies the long description of the image

**Examples Usage —**

ⵌ Local image

```html
<img src="img_chania.jpg" alt="Flowers in Chania" />
```

ⵌ Image in another folder

```html
<img src="/image/img_chania.jpg" alt="Flowers in Chania" />
```

ⵌ Image from another server/website

```html
<img src="https://www.example.com/images/rose.jpg" alt="example.com" />
```

ⵌ Image loading attributes

```html
<!-- Loads the image immediately -->
<img src="img_chania.jpg" alt="Flowers in Chania" loading="eager" />

<!-- Loads the image lazily when it enters the viewport -->
<img src="img_chania.jpg" alt="Flowers in Chania" loading="lazy" />
```

#### [⿻ Live Example](https://thetinsights.github.io/web-quickref/live/html/html-image.html)

## ၊၊||၊ Favicon Icon

A **favicon** is a small icon associated with a website, displayed in browser tabs, bookmarks, and shortcuts. It helps users identify your site quickly.

Adding a Favicon

Favicons are added using the `<link>` tag inside the `<head>` section of your HTML:

```html
<!DOCTYPE html>
<html>
  <head>
    <title>My Page Title</title>
    <link rel="icon" type="image/x-icon" href="/images/favicon.ico" />
  </head>
  <body>
    <h1>This is a Heading</h1>
    <p>This is a paragraph.</p>
  </body>
</html>
```

#### Examples Usage —

ⵌ Standard favicon

```html
<link rel="icon" href="favicon.ico" type="image/x-icon" />
```

ⵌ PNG Format

```html
<link rel="icon" href="favicon.png" type="image/png" />
```

ⵌ SVG Format

```html
<link rel="icon" href="favicon.svg" type="image/svg+xml" />
```

ⵌ Apple touch icon for iOS devices

```html
<link rel="apple-touch-icon" href="apple-touch-icon.png" />
```

> [!TIP]
>
> - Use **PNG** for transparency and better quality.
> - Use **SVG** for scalability and modern designs.
> - Use **ICO** for best browser compatibility.
> - Use **Apple touch icon** for iOS devices.
> - Use **sizes** attribute to specify the size of the icon.
> - The icon is usually 16x16 or 32x32 pixels.
> - Place the favicon file in your root directory for automatic detection by some browsers.
> - Multiple sizes can be provided for different devices (desktop, mobile, tablets).

#### [⿻ Live Example](https://thetinsights.github.io/web-quickref/live/html/html-favicon.html)

## ၊၊||၊ HTML Image Map

An **HTML image map** allows you to define clickable areas within a single image, linking different parts of the image to different destinations.

It is created using the `<map>` element together with one or more `<area>` elements.

**Syntax**

```html
<img src="image.jpg" alt="Example Image" usemap="#mapName" />

<map name="mapName">
  <area shape="rect" coords="34,44,270,350" href="link1.html" alt="Rectangle" />
  <area shape="circle" coords="477,300,100" href="link2.html" alt="Circle" />
  <area
    shape="poly"
    coords="290,172,333,250,300,330"
    href="link3.html"
    alt="Polygon"
  />
</map>
```

**Key Attributes of `<area>`**

- `shape` – Defines the clickable area shape:
  - `rect` — Rectangle (x1, y1, x2, y2)
  - `circle` — Circle (x, y, radius)
  - `poly` — Polygon (x1, y1, x2, y2, x3, y3, …)
- `coords` – Coordinates for the shape.
- `href` – Destination link for the area.
- `alt` – Alternative text describing the area.

#### [⿻ Live Example](https://thetinsights.github.io/web-quickref/live/html/html-image-map.html)

#### [⿻ Image Maps Generator](https://www.image-map.net)

#### Example Usage —

```html
<img src="workplace.jpg" alt="Workplace" usemap="#workmap" />

<map name="workmap">
  <!-- Rectangle for the computer -->
  <area shape="rect" coords="34,44,270,350" href="book.html" alt="book" />

  <!-- Rectangle for the phone -->
  <area shape="rect" coords="290,172,333,250" href="phone.html" alt="Phone" />

  <!-- Circle for the coffee cup -->
  <area shape="circle" coords="477,300,75" href="coffee.html" alt="Coffee" />
</map>
```

> [!NOTE]
>
> - The `usemap` attribute in `<img>` must match the `name` of the `<map>`.
> - Always include `alt` attributes for accessibility.
> - Image maps should be used carefully — they are not mobile-friendly if not designed properly.

<!-- START "Jump to Top"-->
<p align="right">
  <a href="#table-of-contents">Jump to Top ▲</a>
</p>
<!-- END "Jump to Top" -->

# HTML Canvas

The `<canvas>` element in HTML is a **powerful API** used to draw graphics directly in the browser via JavaScript. It allows for dynamic, scriptable rendering of **2D shapes, images, and animations.** Unlike static images, the canvas is **pixel-based** and provides fine-grained control over drawing operations.

**Syntax**

```html
<canvas id="myCanvas" width="400" height="300">
  Your browser does not support the canvas element.
</canvas>
```

Attributes:

- `id` — unique identifier for targeting with JavaScript.
- `width` — width of the canvas in pixels (default: 300px).
- `height` — height of the canvas in pixels (default: 150px).
- Content inside `<canvas>` is fallback text for unsupported browsers.

### ၊၊||၊ Working Process

1. **Create the Canvas**: Define the `<canvas>` element in your HTML.
2. **Get the Canvas Context**: Use JavaScript to get the 2D rendering context of the canvas.
3. **Draw on the Canvas**: Use the context object to draw shapes, images, and text on the canvas.

```html
<!DOCTYPE html>
<html>
  <head>
    <title>Canvas Example</title>
  </head>
  <body>
    <canvas id="myCanvas" width="400" height="300">
      Your browser does not support the canvas element.
    </canvas>

    <script>
      // Get the canvas element
      const canvas = document.getElementById("myCanvas");

      // Get the 2D rendering context
      const ctx = canvas.getContext("2d");

      // Draw a red rectangle
      ctx.fillStyle = "red";
      ctx.fillRect(50, 50, 100, 100);

      // Draw a blue circle
      ctx.beginPath();
      ctx.arc(200, 200, 50, 0, Math.PI * 2);
      ctx.fillStyle = "blue";
      ctx.fill();
    </script>
  </body>
</html>
```

### ၊၊||၊ 1. Drawing Shapes

#### 1.1. Rectangles

```javascript
ctx.fillStyle = "blue"; // fill color
ctx.fillRect(50, 50, 100, 75); // x, y, width, height

ctx.strokeStyle = "red"; // border color
ctx.strokeRect(200, 50, 100, 75);

ctx.clearRect(60, 60, 20, 20); // erase a part
```

#### 1.2. Circles and Arcs

```javascript
ctx.beginPath();
ctx.arc(200, 200, 50, 0, 2 * Math.PI); // x, y, radius, startAngle, endAngle
ctx.fillStyle = "green";
ctx.fill();
ctx.stroke();
```

#### 1.3. Lines

```javascript
ctx.beginPath();
ctx.moveTo(50, 50); // start point
ctx.lineTo(200, 50); // end point
ctx.strokeStyle = "black";
ctx.lineWidth = 5;
ctx.stroke();
```

#### 1.4. Paths

```javascript
ctx.beginPath(); // start a new path
ctx.moveTo(50, 150); // starting point
ctx.lineTo(150, 150); // draw line
ctx.lineTo(100, 250); // draw another line
ctx.closePath(); // close path
ctx.stroke(); // render outline
ctx.fillStyle = "green";
ctx.fill(); // fill the shape
```

#### 1.5. Clipping

```javascript
ctx.beginPath();
ctx.arc(200, 200, 100, 0, Math.PI * 2);
ctx.clip();

ctx.fillStyle = "red";
ctx.fillRect(0, 0, 400, 400);
```

### ၊၊||၊ 2. Drawing Text

```javascript
ctx.font = "30px Arial"; // font style
ctx.fillStyle = "purple"; // text color
ctx.fillText("Hello, Canvas!", 50, 50); // text, x, y

ctx.strokeStyle = "blue"; // outline color
ctx.strokeText("Hello, Canvas!", 50, 100);

/* Text Alignment */
ctx.textAlign = "center"; // left, center, right
ctx.textBaseline = "middle"; // top, middle, bottom, alphabetic
```

### ၊၊||၊ 3. Transformations

#### 3.1. Translation, Rotation, Scaling

```javascript
ctx.translate(200, 150); // move origin
ctx.rotate(Math.PI / 4); // rotate 45 degrees
ctx.scale(1.5, 1.5); // enlarge

ctx.fillStyle = "green";
ctx.fillRect(-50, -50, 100, 100);
```

#### 3.2. Save and Restore State

```javascript
ctx.save(); // save current state
ctx.fillStyle = "red";
ctx.fillRect(50, 50, 100, 100);

ctx.restore(); // restore to previous state
ctx.fillStyle = "blue";
ctx.fillRect(50, 50, 100, 100);
```

### ၊၊||၊ 4. Images

```javascript
const img = new Image();
img.src = "image.jpg";
img.onload = function () {
  ctx.drawImage(img, 50, 50, 200, 200); // image, x, y, width, height
};

/* drawImage can also crop images using 9 parameters: */
ctx.drawImage(img, sx, sy, sWidth, sHeight, dx, dy, dWidth, dHeight);
```

### ၊၊||၊ 5. Colors, Gradients, and Patterns

#### 5.1. Solid Colors

```javascript
ctx.fillStyle = "rgba(255,0,0,0.5)"; // semi-transparent red
ctx.fillRect(0, 0, 100, 100);
```

#### 5.2. Gradients

```javascript
const gradient = ctx.createLinearGradient(0, 0, 200, 0);
gradient.addColorStop(0, "red");
gradient.addColorStop(1, "blue");
ctx.fillStyle = gradient;
ctx.fillRect(0, 0, 200, 200);
```

#### 5.3. Patterns

```javascript
const img = new Image();
img.src = "pattern.png";
img.onload = () => {
  const pattern = ctx.createPattern(img, "repeat");
  ctx.fillStyle = pattern;
  ctx.fillRect(0, 0, 400, 300);
};
```

### ၊၊||၊ 6. Pixel Manipulation

```javascript
const imageData = ctx.getImageData(0, 0, canvas.width, canvas.height);
const data = imageData.data;

for (let i = 0; i < data.length; i += 4) {
  data[i] = 255 - data[i]; // red
  data[i + 1] = 255 - data[i + 1]; // green
  data[i + 2] = 255 - data[i + 2]; // blue
}

ctx.putImageData(imageData, 0, 0);
```

### ၊၊||၊ 7. Animation

```javascript
let x = 0;
let y = 0;

function draw() {
  ctx.clearRect(0, 0, canvas.width, canvas.height);

  ctx.fillStyle = "red";
  ctx.fillRect(x, y, 50, 50);

  x += 1;
  y += 1;
  requestAnimationFrame(draw);
}

draw();
```

#### [⿻ Live Example](https://thetinsights.github.io/web-quickref/live/html/html-canvas.html)

> [!TIP]
>
> - **Always set width & height** on the canvas — prevents stretching.
> - **Use `requestAnimationFrame`** for smooth animations.
> - **Minimize redraws** — only clear/redraw areas that change.
> - **Separate logic from rendering** — keeps code clean.
> - **Fallback text** is important for accessibility.

<!-- START "Jump to Top"-->
<p align="right">
  <a href="#table-of-contents">Jump to Top ▲</a>
</p>
<!-- END "Jump to Top" -->

# Audio & Video

HTML provides built-in elements to embed **audio** and **video** files directly into web pages without relying on external plugins.

These elements are `<audio>` and `<video>`, each with a variety of attributes and controls.

## ၊၊||၊ HTML Audio

The `<audio>` element is used to embed sound content, such as music or podcasts.

**Syntax**

```html
<audio src="audio.mp3" controls>
  Your browser does not support the audio element.
</audio>
```

**Common Attributes**

- `src` — Path to the audio file
- `controls` — Displays playback controls (play, pause, volume)
- `autoplay` — Starts playing automatically when the page loads
- `loop` — Repeats the audio indefinitely
- `muted` — Starts audio muted
- `preload` — Hints how the browser should load the audio (`auto`, `metadata`, `none`)

### Multiple Audio Sources

```html
<audio controls>
  <source src="audio.mp3" type="audio/mpeg" />
  <source src="audio.ogg" type="audio/ogg" />
  Your browser does not support the audio element.
</audio>
```

#### [⿻ Live Example](https://thetinsights.github.io/web-quickref/live/html/html-audio-video.html)

## ၊၊||၊ HTML Video

The `<video>` element is used to embed video content.

```html
<video src="video.mp4" controls width="640" height="360">
  Your browser does not support the video element.
</video>
```

**Common Attributes**

- `src` — Path to the video file
- `controls` — Displays playback controls (play, pause, volume, fullscreen)
- `autoplay` — Starts playing automatically when the page loads
- `loop` — Repeats the video indefinitely
- `muted` — Starts video muted
- `poster` — Image displayed before the video starts playing
- `width` / `height` — Sets the dimensions of the video
- `preload` — Hints how the browser should load the video (`auto`, `metadata`, `none`)

### Multiple Video Sources

```html
<video controls width="640" height="360">
  <source src="video.mp4" type="video/mp4" />
  <source src="video.webm" type="video/webm" />
  Your browser does not support the video element.
</video>
```

Use CSS for responsiveness (Optional)

```css
video {
  max-width: 100%;
  height: auto;
}
```

### Subtitle & Captions

Subtitles / Captions: Use the `<track>` element.

```html
<style>
  video {
    max-width: 100%;
    height: auto;
  }
</style>

<video controls width="640" height="360">
  <track src="subtitles_en.vtt" kind="subtitles" srclang="en" label="English" />
</video>
```

**Key <track> attributes**

- `kind="subtitles"` — Specifies that the track provides subtitles
- `srclang="en"` — Language of the subtitles (English)
- `label="English"` — Human-readable label shown to users

#### [⿻ Live Example](https://thetinsights.github.io/web-quickref/live/html/html-audio-video.html)

<!-- START "Jump to Top"-->
<p align="right">
  <a href="#table-of-contents">Jump to Top ▲</a>
</p>
<!-- END "Jump to Top" -->

# Lists

Lists in HTML group related items in a structured way, improving readability and organizing content like menus, instructions, or references.

There are **four main types of lists:**

### 1. Ordered List — `<ol>`

An ordered list displays items in a numbered sequence, and each item is wrapped in `<li>` (list item).

**Syntax**

```html
<ol>
  <li>First Item</li>
  <li>Second Item</li>
  <li>Third Item</li>
</ol>

<!-- Output:
1. First Item
2. Second Item
3. Third Item
 -->
```

**Example**

```html
<ol type="A" start="3" reversed>
  <li>Item A</li>
  <li>Item B</li>
  <li>Item C</li>
</ol>

<!-- Output:
C. Item A
B. Item B
A. Item C
 -->
```

Explanation of example

- `type` — Defines numbering style (`1`, `A`, `a`, `I`, `i`)
- `start` — Defines starting number
- `reversed` — Displays list in descending order

### 2. Unordered List — `<ul>`

An unordered list displays items with bullet points.

Syntax

```html
<ul>
  <li>Apples</li>
  <li>Bananas</li>
  <li>Cherries</li>
</ul>

<!-- Output:
• Apples
• Bananas
• Cherries
 -->
```

> [!TIP]
> You can style list bullets in CSS using `list-style-type`, with common values such as `disc`, `circle`, `square`, and `none`.

### 3. Description List — `<dl>`

A description list defines terms and their descriptions using `<dl>`, `<dt>`, and `<dd>` elements.

- `<dt>` — Definition term
- `<dd>` — Definition description

**Syntax**

```html
<dl>
  <dt>HTML</dt>
  <dd>A markup language for structuring web pages.</dd>

  <dt>CSS</dt>
  <dd>Used to style and format web pages.</dd>
</dl>

<!-- Output:
 HTML
    A markup language for structuring web pages.
CSS
    Used to style and format web pages.
 -->
```

### 4. Nested Lists

Lists can be nested inside each other.

**Example**

```html
<ul>
  <li>Item A</li>
  <li>
    Item B
    <ul>
      <li>Sub-item B1</li>
      <li>Sub-item B2</li>
    </ul>
  </li>
  <li>Item C</li>
</ul>

<!-- Output:
• Item A
• Item B
    ⚬ Sub-item B1
    ⚬ Sub-item B2
• Item C
 -->
```

### ❏❏❏ Special Attributes & Styling

CSS provides properties to control the appearance of lists, including bullet alignment and custom bullet images.

- `list-style-position` — Controls bullet alignment (inside or outside)
- `list-style-image` — Replaces bullets with a custom image.

**Example**

```css
ul {
  list-style-position: inside; /* Align bullets inside the text */
  list-style-image: url("checkmark.png"); /* Use a custom image as bullet */
}
```

#### [⿻ Live Example](https://thetinsights.github.io/web-quickref/live/html/html-lists.html)

<!-- START "Jump to Top"-->
<p align="right">
  <a href="#table-of-contents">Jump to Top ▲</a>
</p>
<!-- END "Jump to Top" -->

# Tables

HTML tables are used to organize and display data in a tabular format using rows and columns. They are widely used for presenting structured information like schedules, financial data, and comparison charts.

### ၊၊||၊ Basic Table Structure

A table is created using the `<table>` element. It consists of rows (`<tr>`), table headers (`<th>`), and table data cells (`<td>`).

**Syntax**

```html
<table>
  <tr>
    <th>Header 1</th>
    <th>Header 2</th>
  </tr>
  <tr>
    <td>Data 1</td>
    <td>Data 2</td>
  </tr>
</table>
```

**Explanation of example**

- `<table>` — Defines the table
- `<tr>` — Defines a table row
- `<th>` — Defines a table header cell
- `<td>` — Defines a table data cell

### ၊၊||၊ Table Attributes

HTML tables can have various attributes to control their appearance and behavior.

- `border` — Sets the border width (e.g., `border="1"`)
- `cellpadding` — Adds space between cell content and its border (e.g., `cellpadding="10"`)
- `cellspacing` — Adds space between cells (e.g., `cellspacing="10"`)
- `width` — Defines the width of the table (e.g., `width="500"`)
- `height` — Defines the height of the table (e.g., `height="200"`)

**Example**

```html
<table border="1" cellpadding="10" cellspacing="5" width="500" height="200">
  <tr>
    <th>Name</th>
    <th>Age</th>
    <th>City</th>
  </tr>
  <tr>
    <td>John</td>
    <td>25</td>
    <td>New York</td>
  </tr>
  <tr>
    <td>Sara</td>
    <td>22</td>
    <td>London</td>
  </tr>
</table>
```

### ၊၊||၊ Table Sections (Layout)

HTML tables can be divided into logical sections for better structure and readability:

- `<thead>` — Groups the header content of the table. Usually contains column headings.
- `<tbody>` — Groups the main body content (actual data rows).
- `<tfoot>` — Groups the footer content, often used for totals or summaries.

These elements improve accessibility, maintainability, and styling using CSS.

**Example**

```html
<table border="1" cellpadding="8" cellspacing="0">
  <thead>
    <tr>
      <th>Product</th>
      <th>Price</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>Pen</td>
      <td>$1</td>
    </tr>
    <tr>
      <td>Notebook</td>
      <td>$2</td>
    </tr>
  </tbody>
  <tfoot>
    <tr>
      <td>Total</td>
      <td>$3</td>
    </tr>
  </tfoot>
</table>
```

### ၊၊||၊ Merging Cells

In HTML tables, cells can be merged horizontally or vertically using the `colspan` and `rowspan` attributes.

- `colspan` — Merges cells across columns (horizontally).
- `rowspan` — Merges cells across rows (vertically).

**Example**

```html
<table border="1" cellpadding="8" cellspacing="0">
  <tr>
    <th>Product</th>
    <th colspan="2">Details</th>
  </tr>
  <tr>
    <td rowspan="2">Notebook</td>
    <td>Price</td>
    <td>$2</td>
  </tr>
  <tr>
    <td>Stock</td>
    <td>50</td>
  </tr>
</table>
```

**Explanation of example**

- `colspan="2"` — The "Details" cell spans across **two columns**.
- `rowspan="2"` — The "Notebook" cell spans across **two rows**.

### ၊၊||၊ HTML caption

The `<caption>` element **provides a title or description for a table.** It helps users and screen readers understand the table's purpose. The caption is **displayed above the table by default.**

Key Points

- Must be the first child of the `<table>` element.
- Only one `<caption>` is allowed per table.
- Can contain text or inline HTML (like `<strong>`).

**Example**

```html
<table border="1">
  <caption>
    Monthly Sales Report
  </caption>
  <tr>
    <th>Month</th>
    <th>Sales</th>
  </tr>
  <tr>
    <td>January</td>
    <td>$500</td>
  </tr>
  <tr>
    <td>February</td>
    <td>$650</td>
  </tr>
</table>
```

### ❏❏❏ Table Styling with CSS

Instead of HTML attributes, CSS can style tables for better control.

**Example**

```html
<head>
  <style>
    table {
      border-collapse: collapse;
      width: 100%;
    }
    th,
    td {
      border: 1px solid #000;
      padding: 8px;
      text-align: left;
    }
    th {
      background-color: #f2f2f2;
    }
  </style>
</head>
<body>
  <h2>Product Table</h2>
  <table>
    <tr>
      <th>Product</th>
      <th>Price</th>
    </tr>
    <tr>
      <td>Pen</td>
      <td>$1</td>
    </tr>
    <tr>
      <td>Notebook</td>
      <td>$2</td>
    </tr>
  </table>
</body>
```

> [!TIP]
>
> - Use tables only for tabular data, not for layout.
> - Always include `<thead>` and `<tbody>` for accessibility.
> - Provide captions using `<caption>` to describe the table.

#### [⿻ Live Example](https://thetinsights.github.io/web-quickref/live/html/html-tables.html)

<!-- START "Jump to Top"-->
<p align="right">
  <a href="#table-of-contents">Jump to Top ▲</a>
</p>
<!-- END "Jump to Top" -->

# Forms

HTML forms are used to collect user input and send it to a server for processing. They are essential for web applications such as login pages, registration forms, surveys, and search bars.

The `<form>` element is the container for all form controls (inputs, buttons, etc.).

**Syntax**

```html
<form>
  <!-- Form controls go here -->
</form>
```

Attributes of `<form>`

- `action` — URL of the server-side script that processes the form data.
- `method` — HTTP method: `get` (data in URL) or `post` (data in request body).
- `enctype` — Encoding type for file uploads: `application/x-www-form-urlencoded` (default), `multipart/form-data`, `text/plain`.
- `target` — Where to display the response: `_self`,`_blank`, `_parent`,`_top`.
- `autocomplete` — Enables/disables automatic input completion: `on` or `off`.

## 1. Form Input Elements

HTML provides multiple **input types** to collect different kinds of user data. Each input type has specific behavior and validation.

### 1.1 Text Input

Collects single-line text.

```html
<form>
  <label for="username">Name:</label>
  <input
    type="text"
    id="username"
    name="username"
    placeholder="Enter your name"
    required
    maxlength="50"
  />
</form>
```

**Common Attributes:** `name`, `value`, `placeholder`, `required`, `maxlength`, `pattern`.

### 1.2. Password Input

Masks user input for privacy.

```html
<form>
  <label for="password">Password:</label>
  <input type="password" id="password" name="password" required />
</form>
```

### 1.3. Email Input

Validates email format automatically.

```html
<form>
  <label for="email">Email:</label>
  <input
    type="email"
    id="email"
    name="email"
    required
    placeholder="you@example.com"
  />
</form>
```

### 1.4. Number Input

Restricts input to numeric values within a range.

```html
<form>
  <!-- Number Input -->
  <label for="age">Age (0–120):</label>
  <input type="number" id="age" name="age" min="0" max="120" step="1" />

  <!-- Range Input -->
  <label for="volume">Volume (0–100):</label>
  <input type="range" id="volume" name="volume" min="0" max="100" step="10" />
</form>
```

### 1.5. Radio Buttons

Allows **only one selection** in a group with the same `name`.

```html
<form>
  <p>Gender:</p>
  <input type="radio" id="male" name="gender" value="male" />
  <label for="male">Male</label>
  <input type="radio" id="female" name="gender" value="female" />
  <label for="female">Female</label>
</form>
```

### 1.6. Checkboxes

Allows **multiple selections**.

```html
<form>
  <p>Subscribe to:</p>
  <input type="checkbox" id="newsletter" name="subscribe" value="newsletter" />
  <label for="newsletter">Newsletter</label>
  <input type="checkbox" id="offers" name="subscribe" value="offers" />
  <label for="offers">Special Offers</label>
</form>
```

### 1.7. Date & Time Inputs

Collects date, time, or both.

```html
<form>
  <h3>Schedule Form</h3>

  <!-- Date -->
  <label for="dob">Date of Birth:</label>
  <input type="date" id="dob" name="dob" /><br /><br />

  <!-- Time -->
  <label for="alarm">Set Alarm:</label>
  <input type="time" id="alarm" name="alarm" /><br /><br />

  <!-- Date & Time -->
  <label for="meeting">Meeting Time:</label>
  <input type="datetime-local" id="meeting" name="meeting" /><br /><br />

  <!-- Month -->
  <label for="salary">Salary Month:</label>
  <input type="month" id="salary" name="salary" /><br /><br />

  <!-- Week -->
  <label for="week">Project Week:</label>
  <input type="week" id="week" name="week" /><br /><br />

  <button type="submit">Submit</button>
</form>
```

### 1.8. URL, Telephone, and Search Inputs

Specialized inputs for URLs, phone numbers, or search queries.

```html
<form>
  <label for="website">Website:</label>
  <input
    type="url"
    id="website"
    name="website"
    placeholder="https://example.com"
  />

  <label for="phone">Phone:</label>
  <input type="tel" id="phone" name="phone" placeholder="+1 123-456-7890" />

  <label for="query">Search:</label>
  <input type="search" id="query" name="query" placeholder="Search here..." />
</form>
```

### 1.9. File Input

Allows users to `upload files`. Requires the form to use `enctype="multipart/form-data"`.

```html
<form action="upload.js" method="post" enctype="multipart/form-data">
  <label for="resume">Upload Resume:</label>
  <input type="file" id="resume" name="resume" />
  <input type="submit" value="Submit" />
</form>
```

### 1.10. Color Input

Allows users to select a color.

```html
<form>
  <label for="favcolor">Select a color:</label>
  <input type="color" id="favcolor" name="favcolor" value="#0000ff" />
</form>
```

### 1.11. Hidden Input in HTML

The `<input type="hidden">` element is used to store **invisible data** in a form.

- It is **not displayed** to the user on the page.
- Its value is still **sent to the server** when the form is submitted.
- Commonly used for passing IDs, tokens, or metadata that should not be edited by users.

**Example**

```html
<form>
  <!-- Hidden field (user ID) -->
  <input type="hidden" name="user_id" value="12345" />

  <!-- Visible field -->
  <label for="name">Name:</label>
  <input type="text" id="name" name="name" required />
</form>

<!-- Explanation:
 When submitted, the form will send both:
 
 user_id=12345
 name=John
 -->
```

## 2. Textarea — Multi-line Text Input

The `<textarea>` element allows users to enter multi-line text, unlike `<input type="text">` which is single-line. It is commonly used for comments, messages, or any input requiring multiple lines.

**Syntax**

```html
<form>
  <textarea
    name="message"
    rows="5"
    cols="30"
    placeholder="Your message"
  ></textarea>
</form>
```

Key Attributes

- `name` — Identifies the field when the form is submitted.
- `rows` — Sets the visible number of text lines.
- `cols` — Sets the visible width of the textarea in character units.
- `maxlength` — Limits the maximum number of characters that can be entered.
- `placeholder` — Shows temporary hint text inside the textarea.
- `required` — Ensures the field must be filled before form submission.
- `readonly` — Makes the textarea content non-editable.
- `disabled` — Prevents the user from interacting with the textarea.
- `wrap` — Controls text wrapping (`soft` or `hard`).

**Example**

```html
<form>
  <label for="message">Message:</label>
  <textarea
    id="message"
    name="message"
    rows="6"
    cols="40"
    maxlength="250"
    placeholder="Enter your message here"
    required
  ></textarea>
  <button type="submit">Submit</button>
</form>
```

Explanation of example

- `rows="6"` — shows 6 lines by default.
- `cols="40"` — sets width to 40 characters.
- `maxlength="250"` — user cannot type more than 250 characters.
- `placeholder="Enter your message here"` — provides a hint inside the box.
- `required` — ensures user cannot submit an empty message.

## 3. Dropdown List — Select Input

The `<select>` element creates a **drop-down list** that allows users to choose **one or more options** from a list of predefined choices. It is commonly used in forms for selecting countries, languages, categories, or any fixed set of options.

**Syntax**

```html
<select name="options">
  <option value="option1">Option 1</option>
  <option value="option2">Option 2</option>
  <option value="option3">Option 3</option>
</select>
```

Attributes of `<select>`

- `name` — Identifies the dropdown field in the form submission.
- `id` — Associates the dropdown with a `<label>` for accessibility.
- `multiple` — Allows multiple selections. Users can select more than one option.
- `size` — Specifies the number of visible options. Often used with `multiple`.
- `disabled` — Disables the dropdown so the user cannot select options.
- `required` — Makes selection mandatory before form submission.
- `autofocus` — Automatically focuses on the dropdown when the page loads.

Attributes of `<option>`

- `value` — The value sent to the server when selected.
- `selected` — Marks an option as pre-selected when the page loads.
- `disabled` — Makes the option unselectable.
- `label` — Alternative text for the option (used for accessibility or styling).

### 3.1. Single Selection Dropdown

```html
<form>
  <label for="country">Choose a country:</label>
  <select name="country" id="country" required>
    <option value="">--Select--</option>
    <option value="usa">United States</option>
    <option value="uk">United Kingdom</option>
    <option value="canada">Canada</option>
    <option value="australia">Australia</option>
  </select>
</form>
```

Explanation

- `required` ensures the user selects a country.
- First `<option>` with empty value acts as a placeholder.
- `value` is what gets sent to the server upon submission.

### 3.2. Multiple Selection Dropdown

```html
<form>
  <label for="fruits">Select your favorite fruits:</label>
  <select name="fruits[]" id="fruits" multiple size="4">
    <option value="apple">Apple</option>
    <option value="banana">Banana</option>
    <option value="orange">Orange</option>
    <option value="mango">Mango</option>
  </select>
</form>
```

Explanation

- `multiple` — allows selecting more than one fruit.
- `size="4"` — displays 4 options at once without scrolling.
- `name="fruits[]"` — uses square brackets to send an array of selected options to the server.

### 3.3. Grouping Options with `<optgroup>`

```html
<form>
  <select name="cars">
    <optgroup label="Electric">
      <option value="tesla">Tesla</option>
      <option value="nio">NIO</option>
    </optgroup>
    <optgroup label="Gasoline">
      <option value="ford">Ford</option>
      <option value="toyota">Toyota</option>
    </optgroup>
  </select>
</form>
```

Explanation

- `<optgroup>` — groups related options under a label for better organization.
- Users can still select options within the group.

## 4. Buttons

Buttons in HTML forms are interactive elements used to **trigger actions** such as submitting data, resetting fields, or executing custom scripts. Buttons can be created using the `<button>` element or the `<input>` element with `type` specified.

#### Button Types

1. `submit` — Sends form data to the server (default for `<button>` if `type` is not specified).
2. `reset` — Resets all form fields to their initial values.
3. `button` — A generic button used for custom JavaScript actions.

**Example**

```html
<form>
  <!-- Submit Button -->
  <button type="submit">Submit</button>

  <!-- Reset Button -->
  <button type="reset">Reset</button>

  <!-- Custom JavaScript Button -->
  <button type="button" onclick="alert('Custom action triggered!')">
    Custom Action
  </button>
</form>
```

> [!IMPORTANT]
>
> - If no `type` attribute is specified, `<button>` defaults to `type="submit"`.
> - Buttons can also be styled using **CSS** to improve appearance.
> - `<input type="submit">`, `<input type="reset">`, and `<input type="button">` provide simpler alternatives, but `<button>` is more flexible since it allows **HTML content** (like icons or styled text) inside.

## 5. Labels

Labels in HTML improve **form accessibility** and **usability**. A `<label>` is associated with a form control, allowing users to click on the label text to focus or activate the input field.

**Purpose of Labels**

- Improve accessibility (especially for screen readers).
- Increase usability by making the input clickable through its label.
- Provide a clear description of the input field’s purpose.

**Syntax**

```html
<form>
  <label for="input_id">Label Text</label>
  <input type="text" id="input_id" name="field_name" />
</form>
```

Explanation

- The `for` attribute of `<label>` links to the `id` of the input field.
- When the label is clicked, the linked input field is focused.

### 5.1. Input with Label

```html
<!-- Username Input with Label -->

<form>
  <label for="username">Username:</label>
  <input type="text" id="username" name="username" />
</form>
```

### 5.2. Wrapping Input Inside Label

Instead of using for and id, you can wrap the input directly inside the label:

```html
<form>
  <label>
    Email:
    <input type="email" name="email" />
  </label>
</form>
```

> [!NOTE]
>
> - Always use `<label>` for better accessibility.
> - Use `for` + `id` when labels and inputs are separate.
> - Use wrapping method when you prefer a simpler structure.

## 6. Fieldsets and Legends

The `<fieldset>` and `<legend>` elements are used to group related form controls for better organization and accessibility. They help users understand which inputs belong together, especially in larger forms.

**Purpose**

- Visually groups related form fields.
- Provides context with a descriptive title.
- Improves accessibility by giving screen readers additional context.

**Syntax**

```html
<fieldset>
  <legend>Group Title</legend>
  <!-- Form controls go here -->
</fieldset>
```

Explanation

- `<fieldset>` — Creates a box around related inputs.
- `<legend>` — Provides a caption or title for the group.

**Example**

```html
<form>
  <fieldset>
    <legend>Personal Information</legend>

    <label for="name">Name:</label>
    <input type="text" id="name" name="name" /><br /><br />

    <label for="email">Email:</label>
    <input type="email" id="email" name="email" /><br /><br />

    <label for="phone">Phone:</label>
    <input type="tel" id="phone" name="phone" />
  </fieldset>
</form>
```

_ツ The fieldset groups all personal info inputs together, and the legend clearly describes the group._

> [!NOTE]
>
> - A form can contain multiple fieldsets (e.g., "Personal Information", "Payment Details", "Preferences").
> - `<legend>` is optional, but recommended for clarity.
> - By default, browsers render `<fieldset>` with a border and `<legend>` as a title at the top-left.

## 7. Form Validation

HTML5 provides a wide range of **built-in validation attributes** that ensure users enter valid data before submitting a form. These validations happen **client-side**, improving user experience and reducing server load.

**Purpose**

- Ensures users provide **valid and complete data**.
- Reduces errors before submitting to the server.
- Enhances accessibility and usability with clear browser error messages.

### 7.1. Required Field

Ensures that the user cannot leave the field empty.

```html
<form>
  <label>Full Name:</label>
  <input type="text" name="fullname" required />
  <button type="submit">Submit</button>
</form>
```

### 7.2. Length Validation

Restricts the **minimum and maximum number of characters**.

```html
<form>
  <label>Username:</label>
  <input type="text" name="username" minlength="3" maxlength="15" required />
  <button type="submit">Submit</button>
</form>
```

### 7.3. Value Range Validation (Numbers & Dates)

Ensures input is within a valid numeric or date range.

```html
<form>
  <label>Age:</label>
  <input type="number" name="age" min="18" max="100" required /><br /><br />

  <label>Event Date:</label>
  <input type="date" name="event" min="2024-01-01" max="2025-12-31" required />

  <button type="submit">Submit</button>
</form>
```

### 7.4. Step Validation

Forcing input to follow a **step increment**.

```html
<form>
  <label>Score (step of 5):</label>
  <input type="number" name="score" min="0" max="100" step="5" required />
  <button type="submit">Submit</button>
</form>
```

### 7.5. Pattern Matching (Regex)

Custom rules with **regular expressions**.

```html
<form>
  <label>ZIP Code (5 digits):</label>
  <input
    type="text"
    name="zip"
    pattern="\d{5}"
    title="Enter a 5-digit ZIP code"
    required
  />
  <button type="submit">Submit</button>
</form>
```

### 7.6. Type-Based Validation

Some input types validate automatically.

```html
<form>
  <label>Email:</label>
  <input type="email" name="email" required /><br /><br />

  <label>Website:</label>
  <input type="url" name="website" required /><br /><br />

  <label>Phone (10 digits):</label>
  <input
    type="tel"
    name="phone"
    pattern="\d{10}"
    title="Enter a 10-digit phone number"
    required
  />

  <button type="submit">Submit</button>
</form>
```

### 7.7. Date & Time Inputs

Special validation for date & time.

```html
<form>
  <label>Date of Birth:</label>
  <input type="date" name="dob" required /><br /><br />

  <label>Appointment Time:</label>
  <input type="time" name="appt" required /><br /><br />

  <label>Meeting:</label>
  <input type="datetime-local" name="meeting" required /><br /><br />

  <label>Birth Month:</label>
  <input type="month" name="birthmonth" required /><br /><br />

  <label>Week Number:</label>
  <input type="week" name="weeknum" required />

  <button type="submit">Submit</button>
</form>
```

### 7.8. File Input Validation

Restricts allowed file types & supports multiple selection.

```html
<form enctype="multipart/form-data">
  <label>Upload Resume (PDF only):</label>
  <input type="file" name="resume" accept=".pdf" required /><br /><br />

  <label>Upload Photos (Images only):</label>
  <input type="file" name="photos" accept="image/*" multiple />

  <button type="submit">Submit</button>
</form>
```

### 7.9. Other Useful Validation Attributes

#### Readonly & Disabled

- `readonly` — user can see but not edit
- `disabled` — input is uneditable & not submitted

```html
<form>
  <label>User ID (readonly):</label>
  <input type="text" value="USR12345" readonly /><br /><br />

  <label>Disabled Field:</label>
  <input type="text" value="Can't edit or submit" disabled />
</form>
```

#### Multiple Values

Allows multiple values (emails/files).

```html
<form>
  <label>Emails:</label>
  <input type="email" name="emails" multiple required />
  <button type="submit">Submit</button>
</form>
```

#### Autocomplete Control

Controls browser auto-suggestions.

```html
<form>
  <label>Search:</label>
  <input type="search" name="query" autocomplete="off" required />
  <button type="submit">Search</button>
</form>
```

#### Form Without Validation

Disables validation for the entire form.

```html
<form novalidate>
  <label>Email:</label>
  <input type="email" name="email" required />
  <button type="submit">Submit (No Validation)</button>
</form>
```

> [!IMPORTANT]
>
> - **Server-side validation is always required** for security, since client-side can be bypassed.
> - Default browser error messages can be overridden with JavaScript (`setCustomValidity()`).
> - **Browsers provide default error messages**, but `title` can add hints.

## 8. Form Submission Methods

When a form is submitted, data is sent to the server using the method defined in the `<form>` element.
The two most common methods are **GET** and **POST**.

### 8.1. GET Method

- Appends form data to the **URL** as a query string.
- Data is visible in the browser’s address bar.
- Best for **non-sensitive data** (e.g., search queries, filters).
- Limited data length depending on browser/server.
- Results can be bookmarked and shared.

**Example**

```html
<form action="search.php" method="get">
  <label>Search:</label>
  <input type="text" name="query" placeholder="Enter keyword" />
  <button type="submit">Search</button>
</form>
```

URL After Submission:

```plaintext
https://example.com/search.php?query=HTML
```

### 8.2. POST Method

- Sends form data in the **HTTP request body**, not in the URL.
- Data is **not visible** in the browser’s address bar.
- Suitable for **sensitive data** (passwords, login info) or **large data** (file uploads, long text).
- Cannot be bookmarked with the form data.
- No strict size limit like GET.

**Example**

```html
<form action="submit.php" method="post">
  <label>Username:</label>
  <input type="text" name="username" required /><br /><br />

  <label>Password:</label>
  <input type="password" name="password" required /><br /><br />

  <button type="submit">Login</button>
</form>
```

Request Body Sent:

```plaintext
username=John&password=12345
```

> [!IMPORTANT]
>
> - Use **GET** for retrieving or querying data that is not sensitive.
> - Use **POST** for creating, updating, or sending sensitive/large data.
> - **Quick rule of thumb:**
>   - `GET` = “read” operations
>   - `POST` = “write” operations.

### ❏❏❏ Form Layout and Styling

Forms play a crucial role in user interaction. To make them user-friendly and visually appealing, we use CSS for layout and styling.

**Example**

```html
<head>
  <style>
    form {
      max-width: 400px;
      margin: auto;
    }
    input,
    textarea,
    select {
      width: 100%;
      margin: 5px 0;
      padding: 8px;
    }
    button {
      padding: 8px;
      background: #4caf50;
      color: #fff;
      cursor: pointer;
    }
  </style>
</head>
<body>
  <form action="submit.js" method="post">
    <input type="text" name="name" placeholder="Name" />
    <input type="email" name="email" placeholder="Email" />
    <textarea name="message" rows="4" placeholder="Message"></textarea>
    <select name="topic">
      <option>General</option>
      <option>Feedback</option>
      <option>Support</option>
    </select>
    <button type="submit">Submit</button>
  </form>
</body>
```

> [!TIP]
>
> - Use `<label>` with for — improves accessibility.
> - Set correct type (`email`, `number`, `date`, etc.) for built-in validation.
> - Always include `name` — required for data submission.
> - Add validation attributes — `required`, `min`, `max`, `pattern`.
> - Keep `placeholder` as hint, not label.
> - Group inputs with `<fieldset>` + `<legend>`.
> - Choose right `method`:
>   - `GET` = search/non-sensitive.
>   - `POST` = sensitive/large data.
> - File upload needs `enctype="multipart/form-data"`.
> - Specify button types (`submit`, `reset`, `button`).
> - Keep forms short & mobile-friendly.

#### [⿻ Live Example](https://thetinsights.github.io/web-quickref/live/html/html-forms.html)

<!-- START "Jump to Top"-->
<p align="right">
  <a href="#table-of-contents">Jump to Top ▲</a>
</p>
<!-- END "Jump to Top" -->

# HTML iFrame

The `<iframe>` (Inline Frame) is an **HTML element** used to embed another HTML page, web resource, or interactive media inside the current webpage. It acts like a "window" to external or internal content.

- Introduced in HTML 4.0, widely used in modern web development.
- Often used for videos, maps, advertisements, forms, dashboards, and widgets.

### ၊၊||၊ Use Cases

- `Videos & Multimedia` – Embedding YouTube, Vimeo, or other players.
- `Interactive Maps` – Google Maps or OpenStreetMap.
- `Third-party Widgets` – Ads, weather widgets, chatbots.
- `Documents` – Embedding PDFs or other HTML docs.
- `Dashboards/Analytics` – Displaying isolated apps or graphs.
- `Forms` – Embedding payment gateways or third-party forms.

**Syntax**

```html
<iframe src="URL" width="600" height="400" title="Embedded Content"></iframe>
```

#### Explanation of Key Attributes

- `src`: The URL of the content to be embedded.
- `width` and `height`: Dimensions of the iframe.
- `title`: Provides a title for the iframe, useful for accessibility.

#### Other Attributes

- **`title`** – Provides a text description of the content (important for accessibility).
- **`name`** – Assigns an identifier to the iframe (useful as a target for links/forms).
- **`frameborder`** _(deprecated)_ – Controls border visibility (better handled with CSS).
- **`allowfullscreen`** – Enables fullscreen mode for embedded media.
- **`loading="lazy"`** – Delays loading until the iframe is visible (improves performance).
- **`referrerpolicy`** – Controls what referrer information is sent to the iframe.
- **`sandbox`** – Adds security restrictions. Options include `allow-scripts`, `allow-forms`, `allow-same-origin`, etc.
- **`allow`** – Defines permissions such as camera, autoplay, geolocation, fullscreen, etc.

### ၊၊||၊ Types of `<iframe>` Usage

#### 1. Embedding an Internal HTML File

Internal pages are hosted on the same domain and can be referenced via relative paths.

```html
<iframe
  src="pages/about.html"
  width="600"
  height="400"
  title="About Page"
></iframe>
```

#### 2. Embedding an External Website

External websites can be embedded to display content from a different domain.

> **⚠️ Security note:**
> Embedding external sites can introduce risks like clickjacking or XSS. Always consider `sandbox` and `referrerpolicy`.

```html
<iframe
  src="https://www.wikipedia.org"
  width="800"
  height="600"
  title="Wikipedia"
></iframe>
```

#### 3. Embedding a YouTube Video

YouTube videos can be embedded using the iframe embed code provided by YouTube, allowing customization of features like `autoplay`, `controls`, and `fullscreen` access.

```html
<iframe
  width="560"
  height="315"
  src="https://www.youtube.com/embed/dQw4w9WgXcQ"
  title="YouTube video player"
  frameborder="0"
  allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture"
  allowfullscreen
>
</iframe>
```

#### 4. Embedding Google Maps

Google Maps can be embedded for interactive location displays, with attributes like `style="border:0"` to remove borders and `loading="lazy"` to improve performance.

```html
<iframe
  src="https://www.google.com/maps/embed?pb=!1m18..."
  width="600"
  height="450"
  style="border:0;"
  allowfullscreen
  loading="lazy"
  title="Google Maps"
>
</iframe>
```

### 🜲 Best Practices

- Always use `sandbox` for untrusted content.
- Limit permissions with `allow` (e.g., disable autoplay).
- Use `referrerpolicy="no-referrer"` to prevent URL leaks.
- Avoid embedding unknown or suspicious sources.

#### Performance Considerations

- Iframes increase HTTP requests → may slow page load.
- Use `loading="lazy"` to improve performance.
- Avoid nesting multiple iframes (heavy rendering).
- Consider alternatives: AJAX, fetch API, server-side rendering.

#### SEO Impact

- Search engines don’t fully index iframe content.
- Avoid iframes for core content (bad for ranking & accessibility).
- Best for supplemental content: maps, videos, ads.

### ၊၊||၊ Advantages & Disadvantages

✅ **Advantages**

- Easy embedding of external/internal resources.
- Isolates CSS/JS from parent page (no conflicts).
- Useful for widgets, ads, and third-party services.

❌ **Disadvantages**

- Slower performance (extra HTTP requests).
- Poor SEO (content not indexed well).
- Accessibility challenges (screen readers may struggle).
- Security vulnerabilities if used carelessly.

### ❏❏❏ Alternatives to `<iframe>`

- Embed multimedia — Use `<video>` or `<audio>` instead.
- Embed objects — Use `<embed>` or `<object>`.
- Dynamic loading — Use Fetch API or AJAX to fetch and inject content.
- Server-side rendering — Fetch and serve external content on your server.

<!-- START "Jump to Top"-->
<p align="right">
  <a href="#table-of-contents">Jump to Top ▲</a>
</p>
<!-- END "Jump to Top" -->

# HTML File Path

In HTML and web development, a **file path** specifies the location of resources (HTML, CSS, JS, images, videos, documents) so that the browser can load them. Correct path usage ensures that webpages render properly and resources are accessible.

### ၊၊||၊ Types of Paths

#### 1. Absolute Path

A complete path from the root of the file system or full URL.

```html
<!-- External website -->
<a href="https://www.example.com/page.html">Visit Example</a>

<!-- Absolute path on server -->
<img src="https://www.example.com/images/logo.png" alt="Logo" />
```

#### 2. Relative Path

Path relative to the location of the current HTML file.

```html
<!-- File in the same folder -->
<img src="logo.png" alt="Logo" />

<!-- File in a subfolder -->
<img src="images/logo.png" alt="Logo" />

<!-- File in a parent folder -->
<img src="../logo.png" alt="Logo" />
```

#### 3. Root-Relative Path

Path starts from the root directory of the website.

```html
<img src="/assets/images/logo.png" alt="Logo" />
<a href="/pages/about.html">About Us</a>
```

### ၊၊||၊ Path Notations

| Symbol | Meaning           | Example                                |
| ------ | ----------------- | -------------------------------------- |
| `./`   | Current directory | `<a href="./contact.html">Contact</a>` |
| `../`  | Parent directory  | `<a href="../index.html">Home</a>`     |
| `/`    | Root directory    | `<img src="/images/logo.png">`         |

**Example**

```html
<!DOCTYPE html>
<html>
  <head>
    <!-- CSS (Relative) -->
    <link rel="stylesheet" href="style.css" />

    <!-- CSS (Root-relative) -->
    <link rel="stylesheet" href="/css/style.css" />

    <!-- CSS (Absolute) -->
    <link rel="stylesheet" href="https://example.com/x.css" />
  </head>
  <body>
    <!-- Link (Relative) -->
    <a href="about.html">About</a>

    <!-- Link (Root-relative) -->
    <a href="/contact.html">Contact</a>

    <!-- Link (Absolute) -->
    <a href="https://example.com">External</a>

    <!-- JavaScript (Relative) -->
    <script src="app.js"></script>

    <!-- JavaScript (Root-relative) -->
    <script src="/js/app.js"></script>

    <!-- JavaScript (Absolute) -->
    <script src="https://example.com/app.js"></script>
  </body>
</html>
```

<!-- START "Jump to Top"-->
<p align="right">
  <a href="#table-of-contents">Jump to Top ▲</a>
</p>
<!-- END "Jump to Top" -->

# HTML Web Components

Web Components are a set of web platform features that enable developers to build **reusable, encapsulated, and modular UI elements.**

They are built using **standard browser APIs** — no framework dependencies — and can be used in any web application.

Web Components rely on **three main technologies:**

- `<template>` — Defines reusable, non-rendered HTML fragments.
- `<slot>` — Defines placeholders for user-provided content.
- **Shadow DOM** — Provides encapsulation of markup and styles using `attachShadow()`.

### ၊၊||၊ Working Process

##### HTML Template with Slot

```html
<template id="card-template">
  <div class="card"><slot></slot></div>
</template>
```

Explanation of example

- `<template>` stores HTML content that will not render until explicitly used.
- `<slot>` acts as a placeholder where external content can be injected.

**Using the Template Inside a Custom Element**

```html
<script>
  class CardComponent extends HTMLElement {
    constructor() {
      super();

      // Attach shadow DOM
      const shadow = this.attachShadow({ mode: "open" });

      // Get template and clone its content
      const template = document.getElementById("card-template");
      const content = template.content.cloneNode(true);

      // Append cloned content into shadow DOM
      shadow.appendChild(content);
    }
  }

  // Register custom element
  customElements.define("custom-card", CardComponent);
</script>

<!-- Usage -->
<custom-card>
  <h3>Reusable Card</h3>
  <p>This content is passed through the slot.</p>
</custom-card>
```

_✅ Result → A reusable `<custom-card>` component with isolated styles and a content slot._

### ၊၊||၊ Core Elements in Web Components

| Element        | Description                                                                                                |
| -------------- | ---------------------------------------------------------------------------------------------------------- |
| `<template>`   | Defines markup that is not rendered immediately. Contents are inert until cloned with JavaScript.          |
| `<slot>`       | Placeholder inside a shadow DOM where light DOM content (user content) can be injected.                    |
| **Shadow DOM** | Encapsulated DOM tree attached to a host element using `attachShadow()`. Provides style and DOM isolation. |

### ၊၊||၊ Web Components Architecture

- **Template Definition** – Store markup in `<template>` tags. Content remains hidden until cloned.
- **Custom Element Creation** – Extend `HTMLElement` and register with `customElements.define()`.
- **Shadow DOM Encapsulation** – Attach with `this.attachShadow({mode: 'open'})`, add styles & HTML without leaking outside.
- **Slots for Content Projection** – Define placeholders (`<slot>`) for flexible user-provided content.

### ၊၊||၊ Lifecycle Callbacks of Custom Elements

- `connectedCallback()` – Runs when element is inserted into DOM.
- `disconnectedCallback()` – Runs when removed.
- `attributeChangedCallback(name, oldVal, newVal)` – Runs when observed attributes change.
- `adoptedCallback()` – Runs when moved to a new document.

### ၊၊||၊ Advantages & Disadvantages

✅ **Advantages**

- Encapsulation – Shadow DOM isolates styles and DOM.
- Reusability – Build once, use across pages or projects.
- Framework Agnostic – Works in plain HTML/JS or with React, Angular, Vue.
- Custom APIs – Components expose attributes, methods, and events.
- Performance – Native browser features (no heavy framework runtime).

❌ **Disadvantages**

- SEO challenges – Content inside shadow DOM may not be indexed well.
- Learning curve – Developers used to frameworks may need to adapt.
- Tooling – Smaller ecosystem compared to frameworks like React or Angular.
- Older browser support – Needs polyfills for IE11.

### ၊၊||၊ Real-World Use Cases

- **UI Design Systems** (Google’s Material Web Components).
- **Widgets** (chatbots, analytics, maps).
- **Micro-frontends** (components shared across multiple apps).
- **Reusable UI Components** (cards, modals, buttons, forms).

> [!TIP]
>
> - Use Shadow DOM for true encapsulation.
> - Keep components small and modular.
> - Use slots for flexible content injection.
> - Provide clear APIs via attributes & events.
> - Name custom elements with kebab-case (e.g., `<user-profile>`).
> - Use ES Modules to organize component logic.

<!-- START "Jump to Top"-->
<p align="right">
  <a href="#table-of-contents">Jump to Top ▲</a>
</p>
<!-- END "Jump to Top" -->

# HTML5 APIs

HTML5 introduced a variety of **JavaScript APIs** that extend what the browser can do, making web applications more powerful and interactive.

| API                    | Description                                                                                  | Example Use Case                   |
| ---------------------- | -------------------------------------------------------------------------------------------- | ---------------------------------- |
| **Geolocation API**    | Retrieves user’s geographical position (latitude/longitude). Requires user permission.       | Maps, location-based apps          |
| **Web Storage API**    | Provides `localStorage` (persistent) and `sessionStorage` (per session) key-value storage.   | Saving preferences, shopping carts |
| **Drag & Drop API**    | Allows elements to be draggable and dropped into defined zones.                              | File upload, rearranging lists     |
| **Web Workers**        | Run scripts in the background without blocking the main UI thread.                           | Heavy computation, data processing |
| **Canvas API**         | Provides 2D drawing surface for shapes, text, images, and animations.                        | Games, charts, image manipulation  |
| **WebSockets**         | Enables real-time, full-duplex communication between client and server.                      | Chat apps, live updates            |
| **Notifications API**  | Allows web apps to display system notifications. Requires user permission.                   | Alerts, reminders                  |
| **Fullscreen API**     | Enables elements (e.g., video, canvas) to display in fullscreen mode.                        | Media players, presentations       |
| **Battery Status API** | Provides info about device battery level and charging status. ⚠️ Limited/Deprecated support. | Power-saving apps                  |
| **WebRTC**             | Enables peer-to-peer communication (audio, video, data sharing).                             | Video conferencing, screen sharing |

> [!TIP]
>
> - Always check for browser support (`if ("geolocation" in navigator) { ... }`).
> - Ask for **user permissions** clearly (Geolocation, Notifications).
> - Use APIs responsibly to avoid harming performance or privacy.

<!-- START "Jump to Top"-->
<p align="right">
  <a href="#table-of-contents">Jump to Top ▲</a>
</p>
<!-- END "Jump to Top" -->

# Global Attributes

**Global Attributes** are HTML attributes that can be applied to _most_ elements, providing identification, styling, interactivity, or metadata.

| Attribute         | Description                             | Example                                               |
| ----------------- | --------------------------------------- | ----------------------------------------------------- |
| `id`              | Unique identifier for an element.       | `<div id="main"></div>`                               |
| `class`           | Defines one or more CSS classes.        | `<p class="highlight"></p>`                           |
| `style`           | Inline CSS styles.                      | `<span style="color:red;">Hi</span>`                  |
| `title`           | Tooltip text on hover.                  | `<abbr title="Hypertext Markup Language">HTML</abbr>` |
| `lang`            | Defines content language.               | `<html lang="en">`                                    |
| `dir`             | Text direction (`ltr` or `rtl`).        | `<p dir="rtl">مرحبا</p>`                              |
| `hidden`          | Hides element from display.             | `<div hidden></div>`                                  |
| `data-*`          | Custom data attributes for JS logic.    | `<div data-user="123"></div>`                         |
| `draggable`       | Enables drag-and-drop behavior.         | `<img src="img.png" draggable="true">`                |
| `tabindex`        | Controls keyboard tab navigation order. | `<button tabindex="1">Click</button>`                 |
| `contenteditable` | Makes element text editable by user.    | `<p contenteditable="true">Edit me</p>`               |
| `accesskey`       | Assigns a keyboard shortcut.            | `<button accesskey="s">Save</button>`                 |
| `translate`       | Specifies if text should be translated. | `<span translate="no">BrandName</span>`               |

> [!TIP]
>
> - Use **semantic IDs and classes** for clarity.
> - Prefer external stylesheets over `style` for maintainability.
> - Use `data-*` attributes to store custom metadata, not as a replacement for state management.

<!-- START "Jump to Top"-->
<p align="right">
  <a href="#table-of-contents">Jump to Top ▲</a>
</p>
<!-- END "Jump to Top" -->

# Web Accessibility

**ARIA (Accessible Rich Internet Applications)** attributes help make web content usable by **assistive technologies** (like screen readers). They provide additional semantic meaning beyond native HTML.

| Attribute                         | Description                                                          | Example                                                            |
| --------------------------------- | -------------------------------------------------------------------- | ------------------------------------------------------------------ |
| `role`                            | Defines the element’s role (e.g., `button`, `dialog`, `navigation`). | `<div role="dialog">`                                              |
| `aria-label`                      | Provides a text label for screen readers.                            | `<button aria-label="Close menu">X</button>`                       |
| `aria-hidden`                     | Hides element from assistive technologies.                           | `<span aria-hidden="true">*</span>`                                |
| `aria-live`                       | Announces dynamic changes (`polite`, `assertive`).                   | `<div aria-live="polite">New message</div>`                        |
| `aria-expanded` / `aria-controls` | Describes state of expandable elements (accordions, dropdowns).      | `<button aria-expanded="false" aria-controls="menu">Menu</button>` |
| `aria-pressed`                    | Indicates toggle button state (pressed or not).                      | `<button aria-pressed="true">Bold</button>`                        |

> [!TIP]
>
> - Use **native HTML elements** (`<button>`, `<nav>`, `<input>`) whenever possible before adding ARIA.
> - Keep ARIA attributes in sync with actual element states.
> - Test accessibility with screen readers.

<p align="center">──── That’s all for now. Happy coding! 🚀 ────</p>

<!-- START "Jump to Top"-->
<p align="right">
  <a href="#table-of-contents">Jump to Top ▲</a>
</p>
<!-- END "Jump to Top" -->

## 🖥️ Live Examples

- **Structure:** [HTML Semantic Tags](https://thetinsights.github.io/web-quickref/live/html/html-semantic-tags.html) • [HTML Favicon](https://thetinsights.github.io/web-quickref/live/html/html-favicon.html)
- **Text & Media:** [HTML Text & Inline Elements](https://thetinsights.github.io/web-quickref/live/html/html-text-and-inline-elements.html) • [HTML Links & Anchors](https://thetinsights.github.io/web-quickref/live/html/html-links-and-anchors.html) • [HTML Colors](https://thetinsights.github.io/web-quickref/live/html/html-colors.html)
- **Graphics:** [HTML Image](https://thetinsights.github.io/web-quickref/live/html/html-image.html) • [HTML Image Map](https://thetinsights.github.io/web-quickref/live/html/html-image-map.html) • [HTML Canvas](https://thetinsights.github.io/web-quickref/live/html/html-canvas.html)
- **Media:** [HTML Audio & Video](https://thetinsights.github.io/web-quickref/live/html/html-audio-video.html)
- **Data:** [HTML Lists](https://thetinsights.github.io/web-quickref/live/html/html-lists.html) • [HTML Tables](https://thetinsights.github.io/web-quickref/live/html/html-tables.html) • [HTML Forms](https://thetinsights.github.io/web-quickref/live/html/html-forms.html)
