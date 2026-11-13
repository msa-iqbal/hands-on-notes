# HTML Head and Body Elements

Every standard HTML document contains two important structural elements:

* `<head>` — Contains document metadata, resource references, and other information about the page.
* `<body>` — Contains the document's main content, which is presented to users.

Both elements are direct children of the `<html>` root element and serve different purposes.

Understanding their responsibilities helps you organize HTML documents correctly and build maintainable web applications.

# 🗁 Basic Syntax

```html
<!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">

    <title>My Website</title>
</head>

<body>
    <h1>Welcome to My Website</h1>

    <p>This is the main content of the page.</p>
</body>

</html>
```

### 🖆 Document Hierarchy

```text
Document
│
└── html
    │
    ├── head
    │   ├── meta
    │   ├── meta
    │   └── title
    │
    └── body
        ├── h1
        └── p
```

# 🗁 The HTML Head Element

The `<head>` element contains metadata and resource information about an HTML document.

Metadata describes the document rather than representing its main visible content.

### 🖆 Syntax

```html
<head>
    <!-- Document metadata and resource references -->
</head>
```

The `<head>` element is placed inside `<html>` and before `<body>`.

### 🖆 Common Elements Inside Head

| Element | Purpose |
| --- | --- |
| `<title>` | Defines the document title |
| `<meta>` | Provides metadata |
| `<link>` | References external resources |
| `<style>` | Contains embedded CSS |
| `<script>` | Contains or loads JavaScript |
| `<base>` | Defines the base URL for relative URLs |

### 🖆 Title Element

The `<title>` element defines the document's title.

```html
<head>
    <title>My Portfolio Website</title>
</head>
```

The title is commonly displayed in:

* Browser tabs.
* Browser bookmarks.
* Browser history.
* Search engine result titles, where selected.

Best practices:

* Give every page a meaningful title.
* Keep the title relevant to the page content.
* Avoid using the same generic title across every page.

Example:

```html
<title>About Us | My Company</title>
```

### 🖆 Meta Element

The `<meta>` element provides metadata about the document.

It is a void element, so it does not have a closing tag.

**Character encoding:**

```html
<meta charset="UTF-8">
```

Specifies the character encoding used by the document.

UTF-8 supports a wide range of characters and writing systems.

**Viewport configuration:**

```html
<meta name="viewport" content="width=device-width, initial-scale=1.0">
```

Helps browsers display pages using the device's viewport dimensions.

**Description:**

```html
<meta
    name="description"
    content="Learn modern HTML and build accessible web pages."
>
```

Provides a short description of the document. Search engines may use this description in search results.

**Author:**

```html
<meta name="author" content="Website Author">
```

Identifies the document's author.

### 🖆 Link Element

The `<link>` element establishes relationships between the current document and external resources.

It is commonly used to connect CSS stylesheets.

```html
<head>
    <link rel="stylesheet" href="css/style.css">
</head>
```

Other examples include:

**Website icon:**

```html
<link rel="icon" href="favicon.ico">
```

**Preconnect to an external origin:**

```html
<link rel="preconnect" href="https://fonts.googleapis.com">
```

**Alternative document:**

```html
<link
    rel="alternate"
    type="application/rss+xml"
    title="RSS Feed"
    href="/feed.xml"
>
```

### 🖆 Style Element

The `<style>` element contains CSS rules directly inside an HTML document.

```html
<head>
    <style>
        body {
            font-family: Arial, sans-serif;
            background-color: #f5f5f5;
        }

        h1 {
            color: darkblue;
        }
    </style>
</head>
```

For larger projects, external stylesheets are generally easier to maintain and reuse.

### 🖆 Script Element

The `<script>` element is used to embed JavaScript or reference an external JavaScript file.

```html
<head>
    <script src="js/app.js" defer></script>
</head>
```

The `defer` attribute allows a classic external script to download while HTML parsing continues and executes it after parsing has completed.

JavaScript loading strategies are covered in the JavaScript section.

### 🖆 Base Element

The `<base>` element specifies the base URL used to resolve relative URLs in a document.

```html
<head>
    <base href="https://example.com/products/">
</head>
```

For example:

```html
<a href="details.html">Product Details</a>
```

The relative URL resolves to:

```text
https://example.com/products/details.html
```

Important considerations:

* A document can contain at most one `<base>` element.
* It must appear before elements whose URL attributes it affects.
* It can define a default browsing context using the `target` attribute.
* It changes how relative URLs throughout the document are resolved.

# 🗁 The HTML Body Element

The `<body>` element contains the main content of an HTML document.

This is where you define the content users interact with and read.

### 🖆 Syntax

```html
<body>
    <!-- Visible page content -->
</body>
```

Common elements inside `<body>` include:

* Headings and paragraphs.
* Links and navigation.
* Images and multimedia.
* Lists and tables.
* Forms and buttons.
* Semantic page sections.
* Interactive components.

### 🖆 Example

```html
<body>
    <header>
        <h1>My Website</h1>

        <nav>
            <a href="/">Home</a>
            <a href="/about.html">About</a>
            <a href="/contact.html">Contact</a>
        </nav>
    </header>

    <main>
        <section>
            <h2>Welcome</h2>

            <p>
                Welcome to my personal website.
            </p>
        </section>
    </main>

    <footer>
        <p>&copy; 2026 My Website</p>
    </footer>
</body>
```

### 🖆 Common Elements Inside Body

| Element | Purpose |
| --- | --- |
| `<header>` | Introductory content or a section header |
| `<nav>` | Navigation links |
| `<main>` | Dominant content of the document |
| `<section>` | Thematic grouping of content |
| `<article>` | Self-contained content |
| `<aside>` | Complementary content |
| `<footer>` | Footer information |
| `<div>` | Generic block container |
| `<h1>`–`<h6>` | Headings |
| `<p>` | Paragraphs |
| `<a>` | Hyperlinks |
| `<img>` | Images |
| `<form>` | User input forms |

These elements are covered in detail throughout the HTML learning roadmap.

# 🗁 Difference Between Head and Body

| Feature | `<head>` | `<body>` |
| --- | --- | --- |
| Purpose | Document metadata and resources | Main document content |
| Typical content | Title, meta, link, style, script | Headings, paragraphs, forms, images |
| Browser display | Generally not displayed as page content | Rendered as the document's content |
| Position | Before body | After head |
| Main responsibility | Describe and configure the document | Present the document content |

# 🗁 Head and Body in the DOM

Browsers parse HTML and create a Document Object Model (DOM).

You can access the head and body elements through JavaScript.

```javascript
console.log(document.head);
console.log(document.body);
```

You can also access the document title:

```javascript
console.log(document.title);
```

Update the document title:

```javascript
document.title = "Updated Page Title";
```

Add content to the body:

```javascript
const heading = document.createElement("h1");

heading.textContent = "Hello from JavaScript";

document.body.appendChild(heading);
```

The DOM allows JavaScript to interact with the HTML document dynamically.

# 🗁 Complete Practical Example

Consider the following project structure:

```text
my-website/
│
├── index.html
│
├── css/
│   └── style.css
│
├── js/
│   └── app.js
│
└── images/
    └── profile.jpg
```

**index.html**

```html
<!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="UTF-8">

    <meta
        name="viewport"
        content="width=device-width, initial-scale=1.0"
    >

    <meta
        name="description"
        content="A personal portfolio website."
    >

    <title>My Portfolio</title>

    <link rel="icon" href="favicon.ico">

    <link rel="stylesheet" href="css/style.css">

    <script src="js/app.js" defer></script>
</head>

<body>

    <header>
        <h1>My Portfolio</h1>

        <nav>
            <a href="#about">About</a>
            <a href="#projects">Projects</a>
            <a href="#contact">Contact</a>
        </nav>
    </header>

    <main>

        <section id="about">
            <h2>About Me</h2>

            <img
                src="images/profile.jpg"
                alt="Portrait of the portfolio owner"
                width="200"
            >

            <p>
                I am a software developer interested in
                web technologies and software architecture.
            </p>
        </section>

        <section id="projects">
            <h2>My Projects</h2>

            <p>Explore my recent development projects.</p>
        </section>

        <section id="contact">
            <h2>Contact Me</h2>

            <p>Email: developer@example.com</p>
        </section>

    </main>

    <footer>
        <p>&copy; 2026 My Portfolio</p>
    </footer>

</body>

</html>
```

**css/style.css**

```css
body {
    font-family: Arial, sans-serif;
    margin: 0;
    line-height: 1.6;
}

header,
footer {
    background-color: #222;
    color: white;
    padding: 20px;
}

nav a {
    color: white;
    margin-right: 15px;
}

main {
    padding: 20px;
}

section {
    margin-bottom: 30px;
}
```

**js/app.js**

```javascript
console.log("Portfolio website loaded successfully.");
```

This example demonstrates how the head configures the document and loads resources, while the body defines the content presented to visitors.

# 🗁 Common Mistakes

| Mistake | Better Approach |
| --- | --- |
| Placing visible page content inside `<head>` | Put page content inside `<body>` |
| Forgetting the document title | Add a descriptive `<title>` |
| Placing metadata inside `<body>` unnecessarily | Keep document metadata in `<head>` |
| Loading large scripts synchronously in `<head>` without considering performance | Choose an appropriate loading strategy |
| Using inline CSS for every element | Prefer reusable CSS rules |
| Adding multiple `<base>` elements | Use at most one `<base>` element |
| Treating `<head>` as a visible page section | Use semantic body elements for visible content |

# 🗁 Best Practices

* Keep document metadata inside `<head>`.
* Place visible page content inside `<body>`.
* Use a meaningful and unique title for each page.
* Declare UTF-8 character encoding.
* Include viewport metadata for responsive layouts.
* Reference external stylesheets using `<link>`.
* Choose an appropriate JavaScript loading strategy.
* Use semantic HTML elements inside `<body>`.
* Keep the document structure readable and consistently formatted.
* Avoid unnecessary metadata and resource references.

# 🗁 Practical Exercises

### 🖆 Exercise: Create a Document Head

Create an HTML document containing:

* UTF-8 character encoding.
* Viewport configuration.
* A meaningful page title.
* A meta description.
* An external CSS stylesheet.
* An external JavaScript file.

### 🖆 Exercise: Build a Semantic Body

Create a page containing:

* A header with a website title.
* A navigation area.
* A main section.
* Two content sections.
* A footer.

### 🖆 Exercise: Work with the DOM

Use JavaScript to:

* Read the document title.
* Change the document title.
* Access the head element.
* Access the body element.
* Dynamically add a paragraph to the body.

Example:

```javascript
console.log(document.title);

document.title = "My Updated Website";

const paragraph = document.createElement("p");

paragraph.textContent = "This paragraph was created using JavaScript.";

document.body.appendChild(paragraph);
```

# 🗁 Interview Questions

**What is the purpose of the `<head>` element?**

It contains document metadata, resource references, and other information about the HTML document.

**What is the purpose of the `<body>` element?**

It contains the document's main content, which is presented to users.

**What is the difference between `<head>` and `<header>`?**

`<head>` contains document metadata and resource references. `<header>` is a semantic HTML element used inside the body for introductory content or navigation-related material.

**Can we include CSS inside the head?**

Yes. CSS can be included using the `<style>` element or loaded through an external stylesheet using `<link>`.

**Can JavaScript be included inside the head?**

Yes. JavaScript can be embedded or loaded using `<script>`. Attributes such as `defer` and `async` control the loading and execution behavior of external scripts.

**What is the purpose of the `<title>` element?**

It defines the document title, commonly displayed in the browser tab and used by browser history and bookmarks.

**What is the purpose of the `<meta>` element?**

It provides metadata such as character encoding, viewport configuration, description, and author information.

**What is the purpose of the `<base>` element?**

It defines the base URL used to resolve relative URLs in the document.

**Can an HTML document contain multiple `<head>` or `<body>` elements?**

A standard HTML document has one head element and one body element. Browsers may recover from malformed markup, but multiple explicit head or body elements are not the correct document structure.

# 🗁 References

* [MDN — The Head Element](https://developer.mozilla.org/en-US/docs/Web/HTML/Reference/Elements/head)
* [MDN — The Body Element](https://developer.mozilla.org/en-US/docs/Web/HTML/Reference/Elements/body)
* [MDN — The Title Element](https://developer.mozilla.org/en-US/docs/Web/HTML/Reference/Elements/title)
* [MDN — Meta Element](https://developer.mozilla.org/en-US/docs/Web/HTML/Reference/Elements/meta)
* [MDN — Link Element](https://developer.mozilla.org/en-US/docs/Web/HTML/Reference/Elements/link)
* [WHATWG — HTML Living Standard](https://html.spec.whatwg.org/)
