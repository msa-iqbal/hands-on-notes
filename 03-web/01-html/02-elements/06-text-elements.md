# HTML Text Elements

HTML provides several elements for structuring, formatting, and expressing the meaning of text.

These elements help browsers, search engines, developers, and assistive technologies understand how text should be interpreted.

HTML text elements include:

* Text emphasis and importance.
* Highlighted and marked text.
* Quotations and citations.
* Code and technical output.
* Abbreviations and definitions.
* Inserted and deleted text.
* Subscript and superscript.
* Contact information and bidirectional text.

Understanding these elements helps you create meaningful, accessible, and maintainable HTML documents.

# 🗁 Text Formatting Elements

HTML provides elements that communicate the meaning or presentation of text.

| Element | Purpose |
| --- | --- |
| `<strong>` | Indicates strong importance, seriousness, or urgency |
| `<em>` | Indicates stress emphasis |
| `<b>` | Draws attention without adding importance |
| `<i>` | Represents text in an alternate voice or mood |
| `<mark>` | Highlights text relevant to the current context |
| `<small>` | Represents side comments or small print |
| `<del>` | Represents deleted text |
| `<ins>` | Represents inserted text |
| `<sub>` | Represents subscript text |
| `<sup>` | Represents superscript text |

These elements should be selected according to their meaning, not merely their default visual appearance.

# 🗁 Emphasis and Importance

### 🖆 The `<strong>` Element

The `<strong>` element represents content with strong importance, seriousness, or urgency.

Browsers typically render it in bold.

```html
<p>
  <strong>Important:</strong>
  Save your work before closing the application.
</p>
```

Use `<strong>` when the content itself is important.

### 🖆 The `<em>` Element

The `<em>` element represents stress emphasis that can change the meaning of a sentence.

Browsers typically render it in italic.

```html
<p>
  You <em>must</em> complete the form before submitting it.
</p>
```

The emphasis changes which part of the sentence receives stress.

### 🖆 Nested Emphasis

The `<em>` element can be nested to indicate a greater level of stress emphasis.

```html
<p>
  This is <em>very <em>important</em></em>.
</p>
```

### 🖆 Difference Between `<strong>` and `<em>`

| Element | Meaning | Default Appearance |
| --- | --- | --- |
| `<strong>` | Strong importance | Bold |
| `<em>` | Stress emphasis | Italic |

The difference is semantic, not simply visual.

# 🗁 Highlighting Text

### 🖆 The `<mark>` Element

The `<mark>` element represents text that is highlighted because of its relevance in the current context.

Browsers typically display it with a highlighted background.

```html
<p>
  Search results for:
  <mark>JavaScript</mark>
</p>
```

Another example:

```html
<p>
  The most important requirement is
  <mark>accessibility</mark>.
</p>
```

Use `<mark>` for contextual highlighting, such as search results or relevant passages.

Do not use it simply as a replacement for CSS background styling.

### 🖆 The `<small>` Element

The `<small>` element represents side comments and small print, such as legal information, copyright notices, and disclaimers.

```html
<p>
  Product Price: $99.00
  <small>Taxes may apply.</small>
</p>
```

Browsers typically render its text in a smaller font size.

# 🗁 Subscript and Superscript

HTML provides two elements for displaying text below or above the normal text baseline.

### 🖆 The `<sub>` Element

The `<sub>` element represents subscript text.

It is commonly used in chemical formulas and mathematical notation.

```html
<p>
  Water: H<sub>2</sub>O
</p>
```

Another example:

```html
<p>
  Carbon dioxide: CO<sub>2</sub>
</p>
```

### 🖆 The `<sup>` Element

The `<sup>` element represents superscript text.

It is commonly used for mathematical exponents, ordinal indicators, and footnote references.

```html
<p>
  5<sup>2</sup> = 25
</p>
```

Another example:

```html
<p>
  This is the first<sup>st</sup> example.
</p>
```

### 🖆 Comparison

| Element | Position | Example |
|---|---|---|
| `<sub>` | Below the baseline | H₂O |
| `<sup>` | Above the baseline | x² |

# 🗁 Quotations and Citations

HTML provides elements for representing quotations and references.

### 🖆 The `<blockquote>` Element

The `<blockquote>` element represents an extended quotation from another source.

```html
<blockquote cite="https://example.com/article">
  <p>
    The best way to learn programming is through
    consistent practice and experimentation.
  </p>
</blockquote>
```

The `cite` attribute can contain the URL of the source.

Browsers typically indent block quotations by default.

### 🖆 The `<q>` Element

The `<q>` element represents a short inline quotation.

```html
<p>
  The instructor said:
  <q>Practice makes progress.</q>
</p>
```

Browsers typically add quotation marks automatically.

Avoid manually adding quotation marks when the browser already provides them.

### 🖆 The `<cite>` Element

The `<cite>` element represents the title of a creative work, such as a book, article, film, research paper, or other work.

```html
<p>
  My favorite book is
  <cite>Clean Code</cite>.
</p>
```

The `<cite>` element identifies the title of a work, not the name of a person who wrote it.

### 🖆 Combining Quotation Elements

```html
<figure>
  <blockquote>
    <p>
      Programs must be written for people to read,
      and only incidentally for machines to execute.
    </p>
  </blockquote>

  <figcaption>
    — Robert C. Martin, <cite>Clean Code</cite>
  </figcaption>
</figure>
```

# 🗁 Abbreviations and Definitions

### 🖆 The `<abbr>` Element

The `<abbr>` element represents an abbreviation or acronym.

The `title` attribute can provide its expanded meaning.

```html
<p>
  <abbr title="HyperText Markup Language">HTML</abbr>
  is used to structure web pages.
</p>
```

Another example:

```html
<p>
  <abbr title="Application Programming Interface">API</abbr>
  allows software systems to communicate.
</p>
```

When using abbreviations, provide their full meaning when readers may not understand them.

### 🖆 The `<dfn>` Element

The `<dfn>` element represents the defining instance of a term.

```html
<p>
  <dfn>HTML</dfn> is the standard markup language
  used to structure content on the web.
</p>
```

It identifies the term being defined within its surrounding context.

# 🗁 Code and Technical Text

HTML provides several elements for displaying programming code, keyboard input, and computer output.

### 🖆 The `<code>` Element

The `<code>` element represents a fragment of computer code.

```html
<p>
  Use <code>console.log()</code> to print a value
  to the browser console.
</p>
```

It is commonly used for:

* Programming language keywords.
* Function names.
* Variables.
* Commands.
* Code fragments.

### 🖆 The `<pre>` Element

The `<pre>` element represents preformatted text.

Whitespace and line breaks are preserved in its content.

It is commonly combined with `<code>` to display multiline code examples.

```html
<pre><code>function greet() {
  console.log("Hello, World!");
}

greet();</code></pre>
```

Browsers typically display preformatted text using a monospace font.

### 🖆 The `<kbd>` Element

The `<kbd>` element represents user input, typically keyboard input.

```html
<p>
  Press <kbd>Ctrl</kbd> + <kbd>S</kbd> to save the file.
</p>
```

Another example:

```html
<p>
  Press <kbd>Enter</kbd> to continue.
</p>
```

### 🖆 The `<samp>` Element

The `<samp>` element represents sample output from a computer program.

```html
<p>
  Program output:
  <samp>Hello, World!</samp>
</p>
```

### 🖆 The `<var>` Element

The `<var>` element represents a variable in a mathematical expression or programming context.

```html
<p>
  The area of a rectangle is
  <var>width</var> × <var>height</var>.
</p>
```

### 🖆 Technical Elements Comparison

| Element | Purpose | Example |
| --- | --- | --- |
| `<code>` | Programming code | `console.log()` |
| `<pre>` | Preformatted text | Multiline code |
| `<kbd>` | User input | Ctrl + S |
| `<samp>` | Program output | Hello, World! |
| `<var>` | Variable | width |

# 🗁 Inserted and Deleted Text

HTML provides elements for representing changes in a document.

### 🖆 The `<del>` Element

The `<del>` element represents text that has been deleted from a document.

Browsers typically render deleted text with a strikethrough.

```html
<p>
  Original price:
  <del>$120</del>
</p>
```

### 🖆 The `<ins>` Element

The `<ins>` element represents text that has been inserted into a document.

Browsers typically underline inserted text.

```html
<p>
  Updated price:
  <ins>$99</ins>
</p>
```

### 🖆 Combining `<del>` and `<ins>`

```html
<p>
  Product price:
  <del>$120</del>
  <ins>$99</ins>
</p>
```

Both elements support:

* `cite`: URL explaining the change.
* `datetime`: Date and time of the change.

```html
<p>
  Meeting date:
  <del datetime="2026-11-01">November 1</del>
  <ins datetime="2026-11-05">November 5</ins>
</p>
```

# 🗁 Contact Information

### 🖆 The `<address>` Element

The `<address>` element represents contact information for the nearest `<article>` or the document as a whole.

It is commonly used for author or organization contact details.

```html
<address>
  Contact:
  <a href="mailto:contact@example.com">
    contact@example.com
  </a>

  <br>

  <a href="tel:+1234567890">
    +1 234 567 890
  </a>
</address>
```

The `<address>` element should not be used simply to display any arbitrary postal address.

It is intended for contact information associated with the relevant article or page.

# 🗁 Bidirectional Text

Languages such as Arabic and Hebrew are written from right to left, while languages such as English are written from left to right.

HTML provides elements and attributes to handle text direction.

### 🖆 The `dir` Attribute

The `dir` attribute controls the text direction.

Supported values include:

* `ltr`: Left to right.
* `rtl`: Right to left.
* `auto`: Let the browser determine the direction based on the content.

```html
<p dir="ltr">
  Hello, Welcome to HTML.
</p>

<p dir="rtl">
  مرحباً بك في تعلم HTML. (এইচটিএমএল শিখতে স্বাগতম।)
</p>
```

### 🖆 The `<bdi>` Element

The `<bdi>` element isolates a portion of text from the surrounding text direction.

It is useful when displaying user-generated text whose direction is unknown.

```html
<p>
  User:
  <bdi>محمد (মোহাম্মদ)</bdi>
</p>
```

This helps prevent the surrounding text from being affected by the direction of the inserted content.

### 🖆 The `<bdo>` Element

The `<bdo>` element overrides the current text direction.

The `dir` attribute is required.

```html
<p>
  <bdo dir="rtl">Hello World</bdo>
</p>
```

Use `<bdo>` only when you intentionally need to override text direction.

# 🗁 Line Breaks and Thematic Breaks

### 🖆 The `<br>` Element

The `<br>` element represents a line break within text.

```html
<p>
  John Doe<br>
  Software Engineer<br>
  New York, USA
</p>
```

`<br>` is a void element, so it does not have a closing tag.

Use it when a line break is part of the content, such as in an address or poem.

Do not use repeated `<br>` elements to create spacing between sections. Use CSS margins and padding instead.

### 🖆 The `<wbr>` Element

The `<wbr>` element represents a possible line-break opportunity.

The browser may break the line at this position when needed.

```html
<p>
  https://example.com/very<wbr>long<wbr>path
</p>
```

It is useful for long words, URLs, or other text that may overflow a narrow container.

`<wbr>` is also a void element.

### 🖆 The `<hr>` Element

The `<hr>` element represents a thematic break between paragraph-level sections of content.

```html
<h2>Introduction</h2>
<p>Introduction content goes here.</p>

<hr>

<h2>Conclusion</h2>
<p>Conclusion content goes here.</p>
```

Browsers typically render it as a horizontal line.

Although often called a horizontal rule, its semantic purpose is to indicate a thematic change in content.

### 🖆 Comparison

| Element | Purpose |
| --- | --- |
| `<br>` | Line break within text |
| `<wbr>` | Optional line-break opportunity |
| `<hr>` | Thematic break between sections |

# 🗁 Semantic vs Presentational Elements

HTML includes elements that communicate meaning and elements that primarily affect presentation.

Prefer semantic elements when they accurately describe the content.

| Element | Meaning or Purpose |
| --- | --- |
| `<strong>` | Strong importance |
| `<em>` | Stress emphasis |
| `<b>` | Draws attention without adding importance |
| `<i>` | Alternate voice, mood, or technical term |
| `<mark>` | Contextual highlighting |
| `<small>` | Side comments or small print |
| `<del>` | Deleted content |
| `<ins>` | Inserted content |
| `<sub>` | Subscript |
| `<sup>` | Superscript |

### 🖆 Difference Between `<b>` and `<strong>`

`<b>` draws attention to text without indicating that it is more important.

```html
<p>
  The article discusses
  <b>HTML elements</b>
  in detail.
</p>
```

`<strong>` indicates that the text has strong importance.

```html
<p>
  <strong>Warning:</strong>
  Do not disconnect the server during deployment.
</p>
```

### 🖆 Difference Between `<i>` and `<em>`

`<i>` represents text in an alternate voice or mood, such as a technical term or foreign-language phrase.

```html
<p>
  The term <i>homo sapiens</i> refers to modern humans.
</p>
```

`<em>` indicates stress emphasis.

```html
<p>
  You should <em>always</em> test your code.
</p>
```

### 🖆 Styling with CSS

Do not choose semantic elements only because of their default appearance.

Use CSS when the requirement is purely visual.

```html
<p class="important-text">
  This text is styled with CSS.
</p>
```

```css
.important-text {
  font-weight: bold;
  color: darkred;
}
```

# 🗁 Best Practices

* Choose HTML elements according to their meaning.
* Use `<strong>` for strong importance and `<em>` for stress emphasis.
* Use `<mark>` for contextually relevant highlighted text.
* Use `<blockquote>` for extended quotations and `<q>` for inline quotations.
* Use `<cite>` for the title of a creative work.
* Use `<abbr>` to identify abbreviations when useful.
* Use `<code>` for code fragments and `<pre>` for preformatted blocks.
* Use `<kbd>` for keyboard input and `<samp>` for program output.
* Use `<del>` and `<ins>` to represent document changes.
* Use `<address>` for relevant contact information.
* Use `<br>` only when a line break is meaningful to the content.
* Use CSS for visual styling and spacing.
* Ensure right-to-left text is handled correctly with appropriate direction attributes.

# 🗁 Common Mistakes

### 🖆 Using `<br>` for Layout Spacing

Avoid:

```html
<p>First paragraph.</p>
<br><br><br>
<p>Second paragraph.</p>
```

Prefer CSS:

```html
<p class="first-paragraph">First paragraph.</p>
<p>Second paragraph.</p>
```

```css
.first-paragraph {
  margin-bottom: 30px;
}
```

### 🖆 Using `<strong>` Only for Bold Styling

Do not use `<strong>` merely because you want bold text.

Use CSS or `<b>` when the text needs visual attention without conveying strong importance.

### 🖆 Using `<blockquote>` for Indentation

Do not use `<blockquote>` just to indent content.

Its purpose is to represent an extended quotation.

Use CSS for indentation when no quotation is involved.

### 🖆 Using `<cite>` for an Author's Name

The `<cite>` element represents the title of a creative work, not the author.

Prefer:

```html
<p>
  Written by Robert C. Martin in
  <cite>Clean Code</cite>.
</p>
```

### 🖆 Using `<pre>` Without Considering Overflow

Long code lines can overflow narrow screens.

Use CSS to control horizontal scrolling:

```css
pre {
  overflow-x: auto;
  padding: 16px;
}
```

# 🗁 Practical Exercises

### 🖆 Exercise: Format an Article

Create an HTML article using:

* `<strong>` for important information.
* `<em>` for emphasized text.
* `<mark>` for highlighted text.
* `<small>` for additional information.

### 🖆 Exercise: Display Technical Content

Create a programming tutorial containing:

* A `<code>` element for inline code.
* A `<pre>` and `<code>` combination for a code block.
* A `<kbd>` element for keyboard shortcuts.
* A `<samp>` element for program output.
* A `<var>` element for a variable.

### 🖆 Exercise: Create a Quotation Section

Create a quotation section using:

* `<blockquote>` for a long quotation.
* `<q>` for an inline quotation.
* `<cite>` for the title of the source work.

### 🖆 Exercise: Create a Product Price Comparison

Display an original price and a discounted price using `<del>` and `<ins>`.

### 🖆 Exercise: Support Multiple Text Directions

Create a page containing English and Arabic text.

Use `dir`, `<bdi>`, and `<bdo>` where appropriate.

# 🗁 Interview Questions

**What is the difference between `<strong>` and `<b>`?**

`<strong>` indicates strong importance, while `<b>` draws attention without adding importance.

**What is the difference between `<em>` and `<i>`?**

`<em>` represents stress emphasis, while `<i>` represents text in an alternate voice or mood.

**What is the purpose of the `<mark>` element?**

It highlights text that is relevant in the current context, such as a search result.

**What is the difference between `<blockquote>` and `<q>`?**

`<blockquote>` represents an extended quotation, while `<q>` represents a short inline quotation.

**What is the purpose of the `<cite>` element?**

It identifies the title of a creative work, such as a book, article, or film.

**What is the difference between `<code>` and `<pre>`?**

`<code>` represents a fragment of computer code. `<pre>` preserves whitespace and line breaks. They are commonly combined to display multiline code.

**What is the purpose of `<kbd>`?**

It represents user input, typically keyboard input.

**What is the purpose of `<samp>`?**

It represents sample output from a computer program.

**What is the difference between `<sub>` and `<sup>`?**

`<sub>` displays subscript text below the baseline, while `<sup>` displays superscript text above it.

**What is the purpose of the `<abbr>` element?**

It represents an abbreviation or acronym. Its `title` attribute can provide the expanded meaning.

**What is the purpose of `<del>` and `<ins>`?**

`<del>` represents deleted content, while `<ins>` represents inserted content.

**What is the purpose of the `<bdi>` element?**

It isolates text whose direction may differ from the surrounding content.

**What is the difference between `<br>` and `<hr>`?**

`<br>` creates a line break within text, while `<hr>` represents a thematic break between sections of content.

# 🗁 References

* [MDN: HTML Text Fundamentals](https://developer.mozilla.org/en-US/docs/Learn_web_development/Core/Structuring_content/Basic_HTML_syntax)
* [MDN: HTML Elements Reference](https://developer.mozilla.org/en-US/docs/Web/HTML/Reference/Elements)
* [MDN: `<strong>` Element](https://developer.mozilla.org/en-US/docs/Web/HTML/Reference/Elements/strong)
* [MDN: `<em>` Element](https://developer.mozilla.org/en-US/docs/Web/HTML/Reference/Elements/em)
* [MDN: `<code>` Element](https://developer.mozilla.org/en-US/docs/Web/HTML/Reference/Elements/code)
* [WHATWG: HTML Living Standard](https://html.spec.whatwg.org/)
