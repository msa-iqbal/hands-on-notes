# Accessible HTML

HTML accessibility is the practice of creating web content that can be understood, navigated, and operated by people with different abilities and assistive technologies.

Accessible HTML helps ensure that websites work for users who rely on:

* Screen readers.
* Keyboard navigation.
* Voice recognition software.
* Screen magnification.
* Alternative input devices.
* Other assistive technologies.

Accessibility is not an additional feature added after development. It is an essential part of building usable and inclusive web applications.

# 🗁 Overview

Web accessibility is commonly abbreviated as **a11y**.

The term represents the letters between the first and last characters of the word accessibility.

Accessible HTML focuses on making content:

| Principle | Description |
| --- | --- |
| Perceivable | Users can perceive the information presented. |
| Operable | Users can navigate and interact with the interface. |
| Understandable | Content and interactions are understandable. |
| Robust | Content works with different browsers and assistive technologies. |

These principles are known as **POUR** and form the foundation of the Web Content Accessibility Guidelines (WCAG).

# 🗁 Why HTML Accessibility Matters

Accessible HTML improves the experience for people with disabilities and benefits all users.

### 🖆 Screen Reader Compatibility

Screen readers convert webpage content into speech or Braille.

Semantic HTML helps screen readers identify headings, navigation, links, buttons, forms, and other content.

### 🖆 Keyboard Navigation

Some users cannot operate a mouse and navigate websites using a keyboard.

Accessible HTML allows users to interact with links, buttons, forms, and other controls through keyboard input.

### 🖆 Better User Experience

Meaningful structure makes websites easier to understand and navigate.

### 🖆 Search Engine Understanding

Semantic HTML provides meaningful document structure that can also help search engines interpret page content.

Accessibility and SEO are different disciplines, but semantic markup can benefit both.

# 🗁 Semantic HTML

Semantic HTML means using elements according to their meaning and purpose rather than choosing elements only for their visual appearance.

### 🖆 Semantic Elements

```html
<header>
  <h1>My Website</h1>

  <nav aria-label="Main navigation">
    <a href="/">Home</a>
    <a href="/about">About</a>
  </nav>
</header>

<main>
  <article>
    <h2>Understanding HTML Accessibility</h2>

    <p>
      Semantic HTML helps assistive technologies understand
      webpage structure.
    </p>
  </article>
</main>

<footer>
  <p>&copy; 2026 My Website</p>
</footer>
```

Semantic elements communicate the purpose of different content regions.

### 🖆 Avoid Using Div for Everything

Less meaningful structure:

```html
<div class="header">
  <div class="navigation">
    <a href="/">Home</a>
    <a href="/about">About</a>
  </div>
</div>

<div class="main">
  <div class="article">
    <h1>HTML Accessibility</h1>
    <p>Accessible HTML is important.</p>
  </div>
</div>
```

More meaningful structure:

```html
<header>
  <nav aria-label="Main navigation">
    <a href="/">Home</a>
    <a href="/about">About</a>
  </nav>
</header>

<main>
  <article>
    <h1>HTML Accessibility</h1>
    <p>Accessible HTML is important.</p>
  </article>
</main>
```

Use generic `<div>` and `<span>` elements when no more appropriate semantic element exists.

# 🗁 Heading Structure

Headings organize content into a logical hierarchy.

HTML provides six heading levels, from `<h1>` to `<h6>`.

### 🖆 Correct Heading Hierarchy

```html
<h1>Web Development</h1>

<h2>HTML</h2>

<h3>HTML Fundamentals</h3>

<h3>HTML Elements</h3>

<h2>CSS</h2>

<h3>CSS Fundamentals</h3>
```

A logical heading hierarchy helps users understand the relationships between sections.

### 🖆 Avoid Skipping Heading Levels

Avoid using heading levels only to achieve a particular visual size.

Incorrect:

```html
<h1>Web Development</h1>

<h4>HTML Fundamentals</h4>
```

Better:

```html
<h1>Web Development</h1>

<h2>HTML Fundamentals</h2>
```

Heading levels should reflect the document's content hierarchy.

Use CSS to change heading appearance rather than choosing an inappropriate heading level.

# 🗁 Accessible Images

Images should have alternative text that communicates their relevant content or purpose.

The `alt` attribute provides this text.

### 🖆 Meaningful Alternative Text

```html
<img
  src="images/developer.jpg"
  alt="A developer writing code on a laptop"
>
```

Screen readers can announce the alternative text instead of relying on visual information.

### 🖆 Decorative Images

Decorative images should use an empty `alt` attribute.

```html
<img
  src="images/background-pattern.png"
  alt=""
>
```

An empty alternative text tells assistive technologies that the image does not need to be announced.

### 🖆 Functional Images

When an image acts as a link or control, its alternative text should communicate the action or destination.

```html
<a href="/home">
  <img src="images/home-icon.svg" alt="Home">
</a>
```

The accessible name of the link is provided by the image's alternative text.

# 🗁 Accessible Links

Links should communicate their destination or purpose.

### 🖆 Descriptive Link Text

Good:

```html
<a href="/documentation">
  Read the HTML documentation
</a>
```

Poor:

```html
<a href="/documentation">
  Click here
</a>
```

Descriptive link text helps users understand links, especially when screen readers present links outside their surrounding context.

### 🖆 External Links

```html
<a
  href="https://developer.mozilla.org/"
  target="_blank"
  rel="noopener noreferrer"
>
  Visit MDN Web Docs
</a>
```

When a link opens a new browsing context, consider informing users about that behavior when it is not otherwise obvious.

# 🗁 Accessible Forms

Forms should provide clear labels, instructions, and error information.

### 🖆 Use Labels

```html
<form>
  <label for="email">Email address</label>

  <input
    type="email"
    id="email"
    name="email"
    required
  >

  <button type="submit">Submit</button>
</form>
```

The `for` attribute connects the label to the input through its `id`.

This provides an accessible name and makes the label clickable.

### 🖆 Group Related Fields

Use `<fieldset>` and `<legend>` to group related form controls.

```html
<fieldset>
  <legend>Select your preferred contact method</legend>

  <label>
    <input type="radio" name="contact" value="email">
    Email
  </label>

  <label>
    <input type="radio" name="contact" value="phone">
    Phone
  </label>
</fieldset>
```

### 🖆 Provide Clear Instructions

```html
<label for="password">Password</label>

<input
  type="password"
  id="password"
  name="password"
  aria-describedby="password-help"
  required
>

<p id="password-help">
  Use at least 12 characters.
</p>
```

The `aria-describedby` attribute connects the input to additional descriptive information.

# 🗁 Keyboard Accessibility

Users should be able to navigate and operate interactive elements without relying on a mouse.

### 🖆 Native Interactive Elements

HTML provides keyboard-accessible elements such as:

* `<a>`
* `<button>`
* `<input>`
* `<select>`
* `<textarea>`

Use these native elements instead of creating custom interactive controls unnecessarily.

Example:

```html
<button type="button">
  Open menu
</button>
```

A native button supports keyboard interaction without requiring custom JavaScript keyboard handlers.

### 🖆 Tab Navigation

The `Tab` key moves focus between interactive elements.

The `Shift + Tab` combination moves focus backward.

```html
<a href="/">Home</a>

<button type="button">Open menu</button>

<input type="text" aria-label="Search">
```

These elements can receive keyboard focus through normal browser behavior.

### 🖆 Focus Visibility

Users should be able to identify which element currently has keyboard focus.

```css
:focus-visible {
  outline: 3px solid #2563eb;
  outline-offset: 3px;
}
```

Avoid removing focus indicators without providing an equally clear alternative.

# 🗁 Accessible Buttons and Controls

Use the correct HTML element for each interaction.

### 🖆 Buttons for Actions

```html
<button type="button">
  Save changes
</button>
```

Buttons perform actions such as submitting forms, opening dialogs, and triggering application behavior.

### 🖆 Links for Navigation

```html
<a href="/dashboard">
  Go to dashboard
</a>
```

Links navigate to another location or resource.

Do not use an anchor without a meaningful `href` as a substitute for a button.

### 🖆 Avoid Fake Interactive Elements

Avoid using generic elements as controls without implementing the required semantics and keyboard behavior.

Incorrect:

```html
<div onclick="saveData()">
  Save
</div>
```

Better:

```html
<button type="button" onclick="saveData()">
  Save
</button>
```

Native HTML controls provide built-in interaction behavior and accessibility semantics.

# 🗁 ARIA Basics

ARIA stands for Accessible Rich Internet Applications.

It provides additional accessibility information when native HTML semantics are insufficient.

### 🖆 Common ARIA Attributes

| Attribute | Purpose |
| --- | --- |
| `aria-label` | Provides an accessible name. |
| `aria-labelledby` | References another element that provides the accessible name. |
| `aria-describedby` | References additional descriptive information. |
| `aria-hidden` | Controls whether content is exposed in the accessibility tree. |
| `aria-expanded` | Indicates whether a controlled expandable region is expanded. |
| `aria-controls` | Identifies the element controlled by a control. |
| `aria-live` | Identifies content that may be announced when it changes. |

Example:

```html
<button
  type="button"
  aria-expanded="false"
  aria-controls="menu"
>
  Menu
</button>

<nav id="menu" hidden>
  <a href="/">Home</a>
  <a href="/about">About</a>
</nav>
```

The `aria-expanded` value must be updated when the menu opens or closes.

**Important:** ARIA attributes communicate accessibility information. They do not automatically implement interactive behavior.

Prefer native HTML semantics whenever possible.

# 🗁 Color and Contrast

Color should not be the only way to communicate important information.

### 🖆 Do Not Depend Only on Color

Poor:

```html
<p class="error">
  Invalid email address.
</p>
```

If the error is communicated only through red styling, some users may not recognize it.

Better:

```html
<p class="error">
  Error: Please enter a valid email address.
</p>
```

Use text, icons, and other visual indicators alongside color when appropriate.

### 🖆 Maintain Sufficient Contrast

Text should have sufficient contrast against its background.

WCAG 2.2 AA generally requires:

| Content | Minimum Contrast Ratio |
| --- | ---: |
| Normal text | 4.5:1 |
| Large text | 3:1 |
| Essential non-text UI components and graphical objects | 3:1 |

Large text generally means at least 18pt regular or 14pt bold.

These ratios are minimum requirements for the relevant content categories, not a guarantee that every design will be easy to read.

# 🗁 Accessible Tables

Tables should use semantic markup and provide meaningful relationships between headers and data cells.

### 🖆 Basic Accessible Table

```html
<table>
  <caption>Employee Information</caption>

  <thead>
    <tr>
      <th scope="col">Name</th>
      <th scope="col">Position</th>
      <th scope="col">Department</th>
    </tr>
  </thead>

  <tbody>
    <tr>
      <td>John</td>
      <td>Developer</td>
      <td>Engineering</td>
    </tr>

    <tr>
      <td>Sarah</td>
      <td>Designer</td>
      <td>Design</td>
    </tr>
  </tbody>
</table>
```

Use:

* `<caption>` to describe the table.
* `<th>` for header cells.
* `scope="col"` for column headers.
* `scope="row"` for row headers.
* `<thead>`, `<tbody>`, and `<tfoot>` to organize table sections.

# 🗁 Language and Document Metadata

The document language helps assistive technologies determine how content should be pronounced and interpreted.

### 🖆 Declare the Page Language

```html
<!DOCTYPE html>

<html lang="en">
<head>
  <meta charset="UTF-8">
  <title>Accessible HTML</title>
</head>

<body>
  <h1>Accessible HTML</h1>
</body>
</html>
```

Use the appropriate language tag for the document's primary language.

### 🖆 Mark Language Changes

If a document contains text in another language, use the `lang` attribute where appropriate.

```html
<p>
  The French phrase
  <span lang="fr">Bonjour</span>
  means hello.
</p>
```

This helps screen readers use appropriate pronunciation rules.

# 🗁 Accessibility Testing

Accessibility should be tested throughout development.

### 🖆 Keyboard Testing

Try navigating the page without using a mouse.

Check whether:

* All interactive controls can receive focus.
* Focus order is logical.
* Focus indicators are visible.
* Buttons and links can be activated using the keyboard.
* No keyboard traps prevent users from leaving a component.

### 🖆 Screen Reader Testing

Test important workflows with screen readers.

Examples include:

| Platform | Screen Reader |
| --- | --- |
| Windows | NVDA |
| Windows | JAWS |
| macOS | VoiceOver |
| iOS | VoiceOver |
| Android | TalkBack |

Verify that headings, landmarks, links, form labels, and controls are announced meaningfully.

### 🖆 Automated Accessibility Tools

Useful tools include:

* [axe DevTools](https://www.deque.com/axe/devtools/)
* [WAVE](https://wave.webaim.org/)
* [Lighthouse](https://developer.chrome.com/docs/lighthouse/overview/)
* [Accessibility Insights](https://accessibilityinsights.io/)

Automated tools can identify many accessibility issues, but they cannot reliably evaluate every aspect of accessibility.

Combine automated checks with keyboard testing and manual assistive technology testing.

# 🗁 Best Practices

* Use semantic HTML elements according to their purpose.
* Provide meaningful alternative text for informative images.
* Use empty `alt` text for decorative images.
* Maintain a logical heading hierarchy.
* Use descriptive link text.
* Associate form controls with labels.
* Use native buttons and links for interactive behavior.
* Ensure all functionality is keyboard accessible.
* Preserve visible focus indicators.
* Maintain sufficient text and interface contrast.
* Provide appropriate language metadata.
* Use ARIA only when native HTML semantics are insufficient.
* Test pages with automated tools and manual testing.
* Design accessibility into components rather than adding it at the end.

# 🗁 Common Mistakes

### 🖆 Using Div Instead of Semantic Elements

Generic elements do not automatically communicate the meaning of headers, navigation, articles, or buttons.

Use appropriate semantic HTML.

### 🖆 Missing Alternative Text

Informative images without appropriate alternative text can prevent users from understanding their content.

### 🖆 Missing Form Labels

Placeholder text alone is not a reliable replacement for a persistent form label.

### 🖆 Removing Focus Indicators

Removing outlines without providing a visible replacement makes keyboard navigation difficult.

### 🖆 Using ARIA Incorrectly

Adding ARIA attributes does not automatically make an element accessible.

Incorrect roles, states, or relationships can make the accessibility experience worse.

### 🖆 Communicating Information Only Through Color

Users with color-vision deficiencies may not distinguish important information.

Provide additional visual or textual indicators.

### 🖆 Creating Non-Keyboard Controls

Custom interactive elements must support keyboard access, focus behavior, and appropriate accessibility semantics.

Prefer native controls whenever possible.

# 🗁 Practical Exercises

### 🖆 Exercise: Accessible Webpage

Create a webpage using:

* Semantic HTML5 elements.
* A logical heading hierarchy.
* Descriptive navigation links.
* Meaningful image alternative text.
* A properly structured footer.

### 🖆 Exercise: Accessible Contact Form

Build a contact form containing:

* Name.
* Email address.
* Message.
* Required-field validation.
* Labels and helpful instructions.
* Clear error messages.

### 🖆 Exercise: Keyboard Navigation

Test a webpage using only:

* `Tab`.
* `Shift + Tab`.
* `Enter`.
* `Space`.
* Arrow keys where appropriate.

Verify that users can reach and operate all interactive controls.

### 🖆 Exercise: Screen Reader Testing

Use NVDA or VoiceOver to inspect a webpage.

Check how the screen reader announces:

* Page title.
* Headings.
* Navigation landmarks.
* Links.
* Images.
* Form labels.
* Buttons.

### 🖆 Exercise: Accessibility Audit

Run an automated accessibility audit using Lighthouse or axe.

Fix the identified issues and manually test the page again.

# 🗁 Interview Questions

**What is web accessibility?**

Web accessibility is the practice of designing and developing websites so people with different abilities can perceive, understand, navigate, and interact with them.

**What does a11y mean?**

It is a shortened form of accessibility, using the first and last letters with 11 characters between them.

**What are the four principles of WCAG?**

Perceivable, Operable, Understandable, and Robust (POUR).

**Why is semantic HTML important for accessibility?**

It provides meaningful structure that helps assistive technologies understand the purpose and relationships of webpage content.

**What is a screen reader?**

A screen reader is assistive technology that presents digital content through speech or Braille output.

**Why is the alt attribute important?**

It provides text alternatives for images so users who cannot perceive the visual content can understand its relevant information or purpose.

**What is keyboard accessibility?**

It means users can navigate and operate website functionality using a keyboard without requiring a mouse.

**What is ARIA?**

ARIA is a set of attributes that communicates additional accessibility information about web elements to assistive technologies.

**Should ARIA replace semantic HTML?**

No. Native HTML semantics should be preferred because they provide built-in accessibility behavior.

**What is the purpose of the lang attribute?**

It identifies the language of the document or a particular element so assistive technologies can interpret and pronounce its content appropriately.

**What is the difference between accessibility and usability?**

Accessibility focuses on ensuring people with different abilities can access and operate a system. Usability focuses on how effectively and efficiently users can accomplish tasks.

**What is WCAG?**

Web Content Accessibility Guidelines are international guidelines for making web content more accessible.

# 🗁 Official References

* [W3C — Web Accessibility Initiative](https://www.w3.org/WAI/)
* [W3C — Web Content Accessibility Guidelines (WCAG) 2.2](https://www.w3.org/TR/WCAG22/)
* [MDN — Accessibility](https://developer.mozilla.org/en-US/docs/Web/Accessibility)
* [MDN — HTML Accessibility](https://developer.mozilla.org/en-US/docs/Web/Accessibility/Guides/HTML)
* [W3C — ARIA Authoring Practices Guide](https://www.w3.org/WAI/ARIA/apg/)
