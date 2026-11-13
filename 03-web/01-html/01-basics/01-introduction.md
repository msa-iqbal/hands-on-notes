# Introduction to HTML

HTML stands for **HyperText Markup Language**. It is the standard markup language used to create and structure content on the World Wide Web.

HTML defines the structure and meaning of web content using elements represented by tags. Browsers interpret HTML documents and render them as web pages.

HTML is not a programming language. It is a markup language because it describes the structure and semantics of content rather than implementing programming logic.

# 🗁 What Is HTML?

HTML provides a structured way to organize content such as:

* Headings and paragraphs
* Links and navigation
* Images and multimedia
* Lists and tables
* Forms and user input
* Semantic page sections

For example:

```html
<h1>Welcome to HTML</h1>

<p>HTML is the foundation of web development.</p>

<a href="https://developer.mozilla.org/">Learn HTML</a>
```

The browser interprets these elements and displays a heading, paragraph, and clickable link.

# 🗁 What Does HyperText Mean?

**HyperText** refers to text that contains links connecting one document or resource to another.

HTML allows developers to create hyperlinks using the anchor element (`<a>`).

Example:

```html
<a href="https://www.example.com">
    Visit Example
</a>
```

When a user clicks the link, the browser navigates to the specified resource.

HyperText is one of the fundamental concepts behind the interconnected World Wide Web.

# 🗁 What Does Markup Language Mean?

A **markup language** uses tags or annotations to describe the structure and meaning of content.

HTML uses elements to identify different types of content.

Example:

```html
<h1>My Website</h1>

<p>This is a paragraph.</p>

<strong>Important information</strong>
```

Here:

* `<h1>` represents a top-level heading.
* `<p>` represents a paragraph.
* `<strong>` indicates strong importance.

These elements provide meaning and structure to the content.

# 🗁 What Does HTML Do?

HTML is responsible for defining the content and structure of a web page.

Its primary responsibilities include:

| Responsibility    | Description                                              |
| ----------------- | -------------------------------------------------------- |
| Content Structure | Organizes headings, paragraphs, lists, and sections      |
| Navigation        | Connects documents and resources using links             |
| Media             | Embeds images, audio, and video                          |
| Forms             | Collects user input                                      |
| Tables            | Organizes tabular information                            |
| Semantics         | Describes the meaning of different content sections      |
| Accessibility     | Provides meaningful structure for assistive technologies |

# 🗁 HTML, CSS, and JavaScript

Modern web development relies on three core technologies.

| Technology | Responsibility | Example                             |
| ---------- | -------------- | ----------------------------------- |
| HTML       | Structure      | Heading, paragraph, button          |
| CSS        | Presentation   | Colors, spacing, typography, layout |
| JavaScript | Behavior       | Events, validation, dynamic updates |

### 🖆 How They Work Together

```text
             Web Page
                |
       +--------+--------+
       |        |        |
      HTML     CSS  JavaScript
       |        |        |
   Structure  Style   Behavior
       |        |        |
       +--------+--------+
                |
          User Interface
```

### 🖆 Example

**HTML:**

```html
<button id="messageButton">
    Click Me
</button>

<p id="message"></p>
```

**CSS:**

```css
button {
    background-color: blue;
    color: white;
    padding: 10px 20px;
    border: none;
    cursor: pointer;
}
```

**JavaScript:**

```javascript
const button = document.querySelector("#messageButton");

button.addEventListener("click", () => {
    document.querySelector("#message").textContent =
        "Hello from JavaScript!";
});
```

In this example:

* HTML creates the button and paragraph.
* CSS controls the button's appearance.
* JavaScript handles the click event and updates the paragraph.

# 🗁 How HTML Works

When a user visits a website, the browser processes HTML and other web resources to construct and display the page.

A simplified process:

```text
User Requests a Web Page
          |
          v
Browser Sends HTTP Request
          |
          v
Web Server Returns HTML
          |
          v
Browser Parses HTML
          |
          v
DOM Tree Is Created
          |
          v
CSS Is Applied
          |
          v
Layout and Rendering
          |
          v
Web Page Is Displayed
```

### 🖆 Understanding the DOM

DOM stands for **Document Object Model**.

The browser parses HTML and represents its document as a tree of objects called DOM nodes.

For example:

```html
<!DOCTYPE html>
<html>
<head>
    <title>My Website</title>
</head>
<body>
    <h1>Hello World</h1>
    <p>Welcome to HTML.</p>
</body>
</html>
```

A simplified DOM representation:

```text
Document
└── html
    ├── head
    │   └── title
    │       └── "My Website"
    └── body
        ├── h1
        │   └── "Hello World"
        └── p
            └── "Welcome to HTML."
```

JavaScript can interact with this DOM to read, modify, add, or remove elements.

# 🗁 HTML Versions and Evolution

HTML has evolved over time to support modern web requirements.

| Version / Standard | Description |
| --- | --- |
| HTML | Original markup language for web documents |
| HTML 2.0 | Early standardized HTML specification |
| HTML 3.2 | Added support for more presentation and document features |
| HTML 4.01 | Improved structure, forms, scripting, and style support |
| XHTML | HTML reformulated using XML rules |
| HTML5 | Introduced modern semantic elements, native multimedia, and web platform features |
| HTML Living Standard | Continuously maintained HTML standard |

### 🖆 HTML5 and the Living Standard

HTML5 introduced important capabilities, including:

* Semantic elements such as `<header>`, `<nav>`, and `<article>`.
* Native audio and video support.
* The `<canvas>` element.
* Improved form controls and input types.
* APIs and features that support modern web applications.

HTML is now maintained as a continuously evolving **Living Standard** by WHATWG rather than as a sequence of independently versioned HTML releases.

Official specification:

<https://html.spec.whatwg.org/>

# 🗁 HTML File Extension

HTML documents commonly use either of these extensions:

* `.html`
* `.htm`

Example:

```text
index.html
about.html
contact.html
services.html
```

The `.html` extension is the modern convention.

# 🗁 Creating Your First HTML File

Follow these steps to create your first HTML document.

### 🖆 Create a Project Directory

```text
my-first-website/
└── index.html
```

### 🖆 Write HTML Code

Open `index.html` in your code editor and add:

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">

    <title>My First Website</title>
</head>
<body>
    <h1>Hello, World!</h1>

    <p>
        I am learning HTML and building my first web page.
    </p>
</body>
</html>
```

### 🖆 Open It in a Browser

Open `index.html` using your preferred web browser.

The browser will display the heading and paragraph.

You have successfully created your first HTML page.

# 🗁 HTML Elements and Tags

An HTML element generally consists of an opening tag, content, and a closing tag.

Example:

```html
<p>This is an HTML paragraph.</p>
```

Its parts are:

| Part | Example | Description |
| --- | --- | --- |
| Opening tag | `<p>` | Marks the beginning of the element |
| Content | `This is an HTML paragraph.` | The content inside the element |
| Closing tag | `</p>` | Marks the end of the element |

Some HTML elements are **void elements** and do not have closing tags or contain child content.

Examples:

```html
<img src="image.jpg" alt="A landscape">

<br>

<hr>

<input type="text">
```

# 🗁 Characteristics of HTML

* HTML is a markup language, not a programming language.
* HTML is platform-independent.
* HTML documents are interpreted by web browsers.
* HTML provides semantic structure for web content.
* HTML supports hyperlinks between resources.
* HTML works with CSS and JavaScript.
* HTML supports multimedia and interactive form controls.
* HTML is based on elements, tags, and attributes.
* Modern HTML follows the WHATWG Living Standard.

# 🗁 Best Practices

* Use semantic HTML elements instead of unnecessary generic containers.
* Write clean and readable markup.
* Use meaningful document titles.
* Set the document language with the `lang` attribute.
* Include the character encoding declaration.
* Use the viewport meta element for responsive layouts.
* Provide meaningful alternative text for informative images.
* Keep HTML, CSS, and JavaScript responsibilities organized.
* Validate your HTML markup.
* Follow accessibility best practices from the beginning.

# 🗁 Common Mistakes

| Mistake | Better Approach |
| --- | --- |
| Treating HTML as a programming language | Understand it as a markup language |
| Using HTML only for visual styling | Use CSS for presentation |
| Using generic elements everywhere | Prefer meaningful semantic elements |
| Forgetting the document language | Set the `lang` attribute |
| Omitting image alternative text | Provide appropriate `alt` text |
| Using obsolete HTML elements | Follow current HTML standards |
| Confusing tags with elements | Understand the distinction between them |

# 🗁 Practical Exercises

Try these exercises to reinforce the concepts.

**Exercise: Create a personal introduction page**

Create an HTML document containing:

* Your name as a heading.
* A short introduction paragraph.
* A list of your technical skills.
* A link to your GitHub profile.
* An image with appropriate alternative text.

**Exercise: Create a simple information page**

Create a page about a technology you are learning.

Include:

* A page title.
* Multiple headings.
* Paragraphs explaining the technology.
* Links to useful resources.
* A list of important features.

# 🗁 Interview Questions

**What is HTML?**

HTML is a standard markup language used to structure and describe content on web pages.

**What does HTML stand for?**

HyperText Markup Language.

**Is HTML a programming language?**

No. HTML is a markup language because it describes document structure and semantics rather than implementing programming logic.

**What is HyperText?**

HyperText is text that contains hyperlinks connecting documents or resources.

**What is the purpose of HTML?**

HTML defines the structure and meaning of web content, including headings, paragraphs, links, forms, images, and other elements.

**What is the difference between HTML and HTML5?**

HTML5 introduced modern web platform features such as semantic elements, native multimedia, and improved form controls. Modern HTML is maintained through the WHATWG Living Standard.

**What is the DOM?**

The Document Object Model is a programming interface that represents an HTML document as a tree of objects that scripts can access and manipulate.

**What are the three core technologies of frontend development?**

HTML for structure, CSS for presentation, and JavaScript for behavior.

# 🗁 References

* [MDN — HTML: HyperText Markup Language](https://developer.mozilla.org/en-US/docs/Web/HTML)
* [WHATWG — HTML Living Standard](https://html.spec.whatwg.org/)
* [MDN — HTML Elements Reference](https://developer.mozilla.org/en-US/docs/Web/HTML/Reference/Elements)
* [MDN — Introduction to HTML](https://developer.mozilla.org/en-US/docs/Learn_web_development/Core/Structuring_content)
