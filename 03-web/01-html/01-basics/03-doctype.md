# HTML DOCTYPE Declaration

DOCTYPE stands for **Document Type Declaration**.

The DOCTYPE declaration tells the browser which document type and HTML standard mode to use when interpreting an HTML document.

In modern HTML, the declaration is:

```html
<!DOCTYPE html>
```

It should appear at the very beginning of an HTML document, before the `<html>` element.

Although it looks similar to an HTML tag, DOCTYPE is not an HTML element. It is a declaration that provides information to the browser's HTML parser.

# 🗁 What Is DOCTYPE?

DOCTYPE is a declaration used to identify the document type.

Historically, different versions of HTML used different document type declarations that referenced formal document definitions, such as DTDs (Document Type Definitions).

Modern HTML uses a simplified declaration:

```html
<!DOCTYPE html>
```

This tells the browser to use standards mode when rendering the document.

# 🗁 Basic Syntax

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>My Website</title>
</head>
<body>
    <h1>Hello, World!</h1>
</body>
</html>
```

### 🖆 Syntax Breakdown

| Part | Description |
| --- | --- |
| `<!DOCTYPE` | Begins the document type declaration |
| `html` | Identifies the document as HTML |
| `>` | Ends the declaration |

The declaration is case-insensitive, so these forms are equivalent:

```html
<!DOCTYPE html>
```

```html
<!doctype html>
```

However, uppercase `DOCTYPE` is the conventional style.

# 🗁 Why Is DOCTYPE Important?

The DOCTYPE declaration primarily controls which rendering mode a browser uses.

Without an appropriate DOCTYPE, browsers may enter quirks mode, where they emulate certain behaviors of older browsers.

DOCTYPE helps ensure that the browser uses standards mode.

Important benefits include:

* Consistent standards-based rendering.
* More predictable CSS layout behavior.
* Reduced legacy compatibility behavior.
* Better interoperability across modern browsers.

# 🗁 Browser Rendering Modes

Browsers commonly use three document rendering modes.

### 🖆 Standards Mode

Standards mode is the normal rendering mode for modern HTML documents.

It follows modern web standards when interpreting HTML and CSS.

Example:

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Standards Mode</title>
</head>
<body>
    <h1>Standards Mode</h1>
</body>
</html>
```

Use this mode for modern web development.

### 🖆 Quirks Mode

Quirks mode exists to maintain compatibility with older web pages that were written before modern browser standards became widely established.

In this mode, browsers reproduce certain historical rendering behaviors.

For example, the CSS box model may behave differently from standards mode.

Documents without a DOCTYPE commonly trigger quirks mode.

### 🖆 Limited Quirks Mode

Limited quirks mode is also known as almost standards mode.

It preserves standards-based rendering for most layout behavior while retaining a small number of legacy compatibility rules.

Certain older HTML and XHTML doctypes can trigger this mode.

### 🖆 Rendering Modes Comparison

| Mode | Behavior | Recommended |
| --- | --- | --- |
| Standards Mode | Follows modern standards | Yes |
| Quirks Mode | Emulates legacy browser behavior | No |
| Limited Quirks Mode | Standards behavior with limited legacy exceptions | Generally unnecessary for new projects |

# 🗁 HTML5 DOCTYPE

Earlier HTML versions used more complicated DOCTYPE declarations.

### 🖆 HTML 4.01 Strict

```html
<!DOCTYPE HTML PUBLIC
    "-//W3C//DTD HTML 4.01//EN"
    "https://www.w3.org/TR/html4/strict.dtd">
```

### 🖆 HTML 4.01 Transitional

```html
<!DOCTYPE HTML PUBLIC
    "-//W3C//DTD HTML 4.01 Transitional//EN"
    "https://www.w3.org/TR/html4/loose.dtd">
```

### 🖆 XHTML 1.0 Strict

```html
<!DOCTYPE html PUBLIC
    "-//W3C//DTD XHTML 1.0 Strict//EN"
    "https://www.w3.org/TR/xhtml1/DTD/xhtml1-strict.dtd">
```

These declarations were associated with older HTML and XHTML specifications.

Modern HTML does not require a DTD reference.

### 🖆 Modern HTML Declaration

```html
<!DOCTYPE html>
```

This is the correct and recommended declaration for modern HTML documents.

# 🗁 DOCTYPE Placement

DOCTYPE must be placed before the root `<html>` element.

Correct:

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <title>Correct Structure</title>
</head>
<body>
    <h1>Hello World</h1>
</body>
</html>
```

Incorrect:

```html
<html lang="en">
<!DOCTYPE html>
<head>
    <title>Incorrect Structure</title>
</head>
<body>
    <h1>Hello World</h1>
</body>
</html>
```

Placing content before the DOCTYPE can cause the browser to select a different rendering mode.

# 🗁 How to Check Browser Rendering Mode

You can check the rendering mode using browser Developer Tools.

Open the browser console and execute:

```javascript
document.compatMode;
```

Possible results:

```javascript
"CSS1Compat"
```

This indicates standards mode.

```javascript
"BackCompat"
```

This indicates quirks mode.

The property does not distinguish limited quirks mode from full standards mode; both generally report `"CSS1Compat"`.

# 🗁 Common DOCTYPE Mistakes

| Mistake | Correct Approach |
| --- | --- |
| Forgetting the DOCTYPE declaration | Start with `<!DOCTYPE html>` |
| Using an outdated HTML 4 declaration for a new project | Use the modern HTML declaration |
| Placing DOCTYPE after the `<html>` element | Place it before `<html>` |
| Treating DOCTYPE as an HTML element | Understand it as a document declaration |
| Assuming DOCTYPE validates all HTML | Use an HTML validator to check markup |
| Using multiple DOCTYPE declarations | Use one declaration per HTML document |

# 🗁 Best Practices

* Always include `<!DOCTYPE html>` in modern HTML documents.
* Place it at the beginning of the document.
* Use the simple HTML5 declaration.
* Avoid legacy DOCTYPE declarations in new projects.
* Do not confuse DOCTYPE with an HTML element.
* Use browser Developer Tools to inspect rendering modes.
* Validate documents using an HTML validator.

# 🗁 Practical Exercises

### 🖆 Exercise: Create a Standards Mode Document

Create an `index.html` file with:

* A DOCTYPE declaration.
* An HTML root element.
* A head element.
* A body element.
* A heading and paragraph.

Open it in your browser and inspect its rendering mode.

### 🖆 Exercise: Test Quirks Mode

Create another HTML file without a DOCTYPE declaration.

Open it in your browser and execute:

```javascript
document.compatMode;
```

Compare the result with your first document.

### 🖆 Exercise: Compare Rendering Modes

Create two HTML documents:

* One with `<!DOCTYPE html>`.
* One without a DOCTYPE declaration.

Apply CSS box-model properties and inspect their behavior in Developer Tools.

# 🗁 Interview Questions

**What is DOCTYPE in HTML?**

DOCTYPE is a document type declaration that tells the browser how to interpret an HTML document.

**What is the correct DOCTYPE declaration in HTML5?**

```html
<!DOCTYPE html>
```

**Is DOCTYPE an HTML element?**

No. It is a declaration that appears before the root HTML element.

**Why do we use DOCTYPE?**

It helps browsers render documents in standards mode rather than quirks mode.

**What happens if we omit DOCTYPE?**

Browsers commonly enter quirks mode, which can cause legacy rendering behavior and unexpected layout differences.

**Where should DOCTYPE be placed?**

It should appear at the beginning of the HTML document, before the `<html>` element.

**What is quirks mode?**

Quirks mode is a browser rendering mode that emulates certain historical browser behaviors for compatibility with older web pages.

**What is the difference between standards mode and quirks mode?**

Standards mode follows modern web standards, while quirks mode reproduces legacy rendering behavior.

**How can you check the browser's rendering mode?**

Execute `document.compatMode` in the browser console. `"CSS1Compat"` indicates standards mode, while `"BackCompat"` indicates quirks mode.

# 🗁 References

* [MDN — Doctype](https://developer.mozilla.org/en-US/docs/Glossary/Doctype)
* [MDN — Quirks Mode and Standards Mode](https://developer.mozilla.org/en-US/docs/Web/HTML/Quirks_Mode_and_Standards_Mode)
* [WHATWG — The DOCTYPE](https://html.spec.whatwg.org/multipage/syntax.html#the-doctype)
