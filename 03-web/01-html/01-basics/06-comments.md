# HTML Comments

HTML comments allow developers to add notes and explanations directly inside an HTML document. They are useful for documenting markup, temporarily disabling sections during development, and making complex code easier to understand.

Comments are not rendered as visible page content, but they remain part of the HTML source and can be inspected through browser developer tools.

# 🗁 Comment Syntax

HTML comments begin with `<!--` and end with `-->`.

**Syntax:**

```html
<!-- This is an HTML comment -->
```

The browser does not display the comment as visible page content.

**Example:**

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>HTML Comments</title>
</head>
<body>
    <!-- Main page heading -->
    <h1>Welcome to HTML</h1>

    <!-- Page introduction -->
    <p>HTML is used to structure web pages.</p>
</body>
</html>
```

The comments describe the purpose of different parts of the document without affecting the visible content.

# 🗁 Single-Line Comments

A single-line comment is useful when you need to explain a specific element or section.

**Example:**

```html
<!-- Website header -->
<header>
    <h1>My Website</h1>
</header>

<!-- Main navigation -->
<nav>
    <a href="/">Home</a>
    <a href="/about">About</a>
</nav>
```

You can write a comment on the same line as an element, provided it is outside the tag syntax:

```html
<h1>Welcome</h1> <!-- Main page heading -->
```

# 🗁 Multi-Line Comments

HTML comments can span multiple lines. Use them when an explanation requires more than one line.

**Example:**

```html
<!--
    Website Header

    This section contains:
    - Website logo
    - Main navigation
    - Header actions
-->

<header>
    <h1>My Website</h1>
</header>
```

There is no separate multi-line comment syntax in HTML. The same `<!--` and `-->` delimiters are used.

# 🗁 Commenting Out HTML

You can temporarily disable a section of HTML by wrapping it in a comment.

**Example:**

```html
<body>
    <h1>Welcome to My Website</h1>

    <!--
    <section>
        <h2>Upcoming Features</h2>
        <p>This section is under development.</p>
    </section>
    -->

</body>
```

The commented-out section will not be rendered as normal page content.

This technique can be useful during development, but it should not replace proper version control or removal of unused code.

# 🗁 Comments in Browser Developer Tools

HTML comments are not visible on the rendered page, but they remain in the document's source and DOM.

You can inspect them using browser developer tools.

**How to inspect comments:**

* Open a webpage in your browser.
* Right-click and select **Inspect**.
* Open the **Elements** panel.
* Find the relevant HTML section.
* Look for comment nodes in the document tree.

You can also use the browser console:

```javascript
const walker = document.createTreeWalker(
    document,
    NodeFilter.SHOW_COMMENT
);

let comment;

while ((comment = walker.nextNode())) {
    console.log(comment.textContent);
}
```

This example finds comment nodes in the current document.

**Important:** Never store passwords, API keys, access tokens, private information, or other secrets inside HTML comments. Anyone who can inspect the page source may be able to read them.

# 🗁 HTML Comments vs CSS and JavaScript Comments

Different languages use different comment syntaxes.

| Language | Single-Line Comment | Multi-Line Comment |
| --- | --- | --- |
| HTML | `<!-- comment -->` | `<!-- comment -->` |
| CSS | Not supported separately | `/* comment */` |
| JavaScript | `// comment` | `/* comment */` |

**HTML comment:**

```html
<!-- This is an HTML comment -->
<h1>Hello World</h1>
```

**CSS comment:**

```css
/* This is a CSS comment */

h1 {
    color: blue;
}
```

**JavaScript comment:**

```javascript
// This is a JavaScript comment

const message = "Hello World";

/*
    This is a
    multi-line JavaScript comment.
*/
```

Use the comment syntax belonging to the language in which you are writing.

# 🗁 TODO and FIXME Comments

Developers often use keywords such as `TODO`, `FIXME`, and `NOTE` to organize development tasks.

These are plain-text conventions, not special HTML comment keywords.

**TODO example:**

```html
<!-- TODO: Add responsive navigation -->
```

**FIXME example:**

```html
<!-- FIXME: Correct the image dimensions -->
```

**NOTE example:**

```html
<!-- NOTE: This section is generated dynamically -->
```

Common conventions:

| Keyword | Purpose |
| --- | --- |
| TODO | Indicates work that needs to be completed |
| FIXME | Identifies something that needs correction |
| NOTE | Provides additional information |
| HACK | Documents a temporary workaround |
| REVIEW | Marks code that needs further review |

# 🗁 Best Practices

* Write comments that explain the purpose or intent of complex markup.
* Keep comments short, clear, and meaningful.
* Use comments to identify major sections when they improve readability.
* Remove outdated comments and unnecessary commented-out code.
* Avoid explaining obvious HTML elements.
* Never place passwords, API keys, or sensitive information inside comments.
* Use TODO and FIXME conventions consistently across your project.
* Keep comments synchronized with the actual implementation.
* Prefer version control when preserving previous code instead of leaving large commented-out sections.

**Good example:**

```html
<!-- Primary navigation for the website -->
<nav aria-label="Primary navigation">
    <a href="/">Home</a>
    <a href="/services">Services</a>
    <a href="/contact">Contact</a>
</nav>
```

**Poor example:**

```html
<!-- This is a nav element -->
<nav>
    <a href="/">Home</a>
</nav>
```

The second comment simply repeats what the HTML already communicates.

# 🗁 Common Mistakes

### 🖆 Forgetting the closing delimiter

Incorrect:

```html
<!-- This comment is not properly closed
<h1>Hello World</h1>
```

Correct:

```html
<!-- This comment is properly closed -->
<h1>Hello World</h1>
```

### 🖆 Attempting to nest HTML comments

HTML comments cannot be nested reliably.

Incorrect:

```html
<!-- Outer comment
    <!-- Inner comment -->
-->
```

The first `-->` terminates the comment. The remaining markup may be interpreted differently than expected.

Correct:

```html
<!-- Outer comment with an explanation -->
```

### 🖆 Placing comments inside tag syntax

Incorrect:

```html
<div <!-- section comment --> class="container">
</div>
```

Correct:

```html
<!-- Main content container -->
<div class="container">
</div>
```

Comments should be placed between markup constructs, not inside an opening tag or attribute value.

### 🖆 Using HTML comments to hide sensitive information

Incorrect:

```html
<!-- API_KEY=secret-value -->
```

Correct:

```html
<!-- API configuration is managed by the application environment -->
```

Comments are not a security mechanism. Anything delivered to the browser should be considered accessible to the user.

### 🖆 Using HTML comments inside CSS or JavaScript

Incorrect:

```html
<style>
    <!-- This is not a CSS comment -->
    h1 {
        color: red;
    }
</style>
```

Correct:

```html
<style>
    /* This is a CSS comment */
    h1 {
        color: red;
    }
</style>
```

Use `/* ... */` for CSS and `//` or `/* ... */` for JavaScript.

# 🗁 Conditional Comments

Conditional comments were a historical Microsoft Internet Explorer feature used to deliver browser-specific markup or styles.

Example of legacy syntax:

```html
<!--[if IE]>
    <p>This content was intended for Internet Explorer.</p>
<![endif]-->
```

These comments are obsolete and are not supported by modern browsers. Do not use them in new projects.

Modern browser compatibility should be handled through feature detection, progressive enhancement, and appropriate CSS or JavaScript techniques.

# 🗁 Practical Examples

### 🖆 Documenting a webpage

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">

    <!-- Browser tab title -->
    <title>My Portfolio</title>
</head>
<body>

    <!-- Website header -->
    <header>
        <h1>My Portfolio</h1>
    </header>

    <!-- Main page content -->
    <main>
        <section>
            <h2>About Me</h2>
            <p>I am a web developer.</p>
        </section>
    </main>

    <!-- Website footer -->
    <footer>
        <p>&copy; 2026 My Portfolio</p>
    </footer>

</body>
</html>
```

### 🖆 Temporarily disabling a section

```html
<main>
    <h1>Our Services</h1>

    <p>Explore the services we provide.</p>

    <!--
    <section>
        <h2>Coming Soon</h2>
        <p>New services will be available soon.</p>
    </section>
    -->
</main>
```

# 🗁 Practice Exercises

* Create an HTML document with at least five meaningful comments.
* Add single-line comments before the header, navigation, main content, and footer.
* Write a multi-line comment explaining a webpage section.
* Temporarily comment out a paragraph and observe the rendered result.
* Open browser developer tools and locate your HTML comments.
* Write TODO, FIXME, and NOTE comments in a sample HTML document.
* Find and correct an incorrectly nested HTML comment.

# 🗁 Interview Questions

**What are HTML comments?**

HTML comments are annotations written inside HTML source code. They are not displayed as normal page content.

**What is the syntax for an HTML comment?**

```html
<!-- This is an HTML comment -->
```

**Are HTML comments visible in the browser?**

They are not displayed on the rendered page, but they can be inspected through page source and developer tools.

**Can HTML comments be nested?**

No. HTML comments cannot be nested reliably because the first closing delimiter terminates the comment.

**Can comments be written across multiple lines?**

Yes. Use the same opening and closing delimiters around multiple lines.

**Can HTML comments contain sensitive information?**

No. Comments are delivered to the browser and can be inspected by users.

**What is the difference between HTML and CSS comments?**

HTML uses `<!-- ... -->`, while CSS uses `/* ... */`.

**What are conditional comments?**

They were a legacy Internet Explorer feature used to conditionally include markup. They are obsolete and should not be used in modern development.

**Can comments be placed inside HTML attributes?**

No. HTML comments should be placed between markup constructs, not inside tag syntax or attribute values.

# 🗁 References

* [MDN – HTML Comments](https://developer.mozilla.org/en-US/docs/Web/HTML/Guides/Comments)
* [WHATWG – HTML Living Standard](https://html.spec.whatwg.org/)
* [MDN – HTML Elements Reference](https://developer.mozilla.org/en-US/docs/Web/HTML/Reference/Elements)
