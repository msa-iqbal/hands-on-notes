# Markdown Syntax

Markdown syntax consists of ordinary text and special characters that represent formatting instructions.

Markdown uses characters such as:

* `#` — Headings
* `*` — Emphasis and lists
* `-` — Lists
* `>` — Blockquotes
* `` ` `` — Inline code
* `[ ]` — Links
* `![ ]` — Images
* `|` — Tables

These characters are interpreted by a Markdown processor to produce formatted output.

Markdown documents are generally built using two types of elements.
### Block Elements

Block elements create separate sections or structures.

Examples:

* Headings
* Paragraphs
* Lists
* Blockquotes
* Code blocks
* Tables
* Horizontal rules

### Inline Elements

Inline elements format text within a block.

Examples:

* Bold text
* Italic text
* Strikethrough
* Inline code
* Links
* Images

# 🗁 Headings

Use the hash (`#`) character to create headings.

```markdown
# Heading 1
## Heading 2
### Heading 3
#### Heading 4
##### Heading 5
###### Heading 6
```

Markdown supports six heading levels.

# 🗁 Paragraph

Write ordinary text to create a paragraph.

```markdown
This is my first paragraph.

This is my second paragraph.
```

A blank line separates paragraphs.

# 🗁 Line Break

There are multiple ways to create a line break.

### Method 1: Two Trailing Spaces

```markdown
First line.  
Second line.
```

### Method 2: Backslash

```markdown
First line.\
Second line.
```

### Method 3: HTML Break Tag

```markdown
First line.<br>
Second line.
```

The availability of HTML depends on the Markdown processor.

# 🗁 Bold Text

Use two asterisks or two underscores.

```markdown
**This is bold text.**

__This is also bold text.__
```

Output: **This is bold text.**

# 🗁 Italic Text

Use one asterisk or one underscore.

```markdown
*This is italic text.*

_This is also italic text._
```

Output: *This is italic text.*

# 🗁 Bold and Italic

Use three asterisks or three underscores.

```markdown
***Bold and italic text.***

___Bold and italic text.___
```

Output: ***Bold and italic text.***

# 🗁 Strikethrough

Use two tildes around the text.

```markdown
~~This text is deleted.~~
```

Output: ~~This text is deleted.~~

This is commonly supported by GitHub Flavored Markdown.

# 🗁 List

Markdown supports ordered and unordered lists.

### Unordered Lists

Use hyphens, asterisks, or plus signs.

```markdown
- HTML
- CSS
- JavaScript
```

```markdown
* HTML
* CSS
* JavaScript
```

```markdown
+ HTML
+ CSS
+ JavaScript
```

### Ordered Lists

Use numbers followed by periods.

```markdown
1. Learn HTML
2. Learn CSS
3. Learn JavaScript
```

# 🗁 Blockquote

Use the greater-than (`>`) character.

```markdown
> This is a blockquote.
```

Nested blockquotes:

```markdown
> First level
>> Second level
>>> Third level
```

# 🗁 Inline Code

Use one backtick around text.

```markdown
Use the `console.log()` function.
```

Output: Use the `console.log()` function.

# 🗁 Code Block

Use three backticks before and after a code block.

````markdown
```javascript
console.log("Hello World");
```
````

You can specify a language identifier for syntax highlighting.

Common identifiers:

* `javascript`
* `typescript`
* `python`
* `php`
* `html`
* `css`
* `bash`
* `json`
* `sql`

# 🗁 Link

Use square brackets for link text and parentheses for the URL.

```markdown
[Visit Google](https://www.google.com)
```

You can also add a title:

```markdown
[Visit Google](https://www.google.com "Google Homepage")
```

# 🗁 Image

Use an exclamation mark followed by square brackets and parentheses.

```markdown
![Alternative text](images/logo.png)
```

With a title:

```markdown
![Company Logo](images/logo.png "Company Logo")
```

# 🗁 Horizontal Rule

Use three or more hyphens, asterisks, or underscores.

```markdown
---
```

```markdown
***
```

```markdown
___
```

These create horizontal separators.

# 🗁 Table Syntax

Use pipes (`|`) to separate columns and hyphens to define the header separator.

```markdown
| Name | Age | Role |
|---|---|---|
| Ahmed | 25 | Developer |
| Rahim | 28 | Designer |
```

Tables are supported by GitHub Flavored Markdown and many other processors.

# 🗁 Escaping Markdown Characters

Sometimes you need to display a Markdown character without activating its formatting behavior.

Use a backslash (`\`) before the character.

Example:

```markdown
\*This is not italic text.\*

\# This is not a heading.

\`This is not inline code.\`
```

Output:

\*This is not italic text.\*

\# This is not a heading.

\`This is not inline code.\`

Common escapable characters include:

```text
\   Backslash
`   Backtick
*   Asterisk
_   Underscore
{ } Braces
[ ] Brackets
< > Angle brackets
( ) Parentheses
#   Hash
+   Plus
-   Hyphen
.   Period
!   Exclamation mark
|   Pipe
```

# 🗁 Whitespace and Blank Lines

Whitespace affects how Markdown processors interpret document structure.

**Important Rules**

1. Use blank lines to separate paragraphs.
2. Use a space after heading markers.
3. Use blank lines around fenced code blocks.
4. Use consistent indentation for nested lists.
5. Avoid unnecessary trailing spaces.

Example:

```markdown
# Web Development

Web development includes frontend and backend development.

## Frontend

- HTML
- CSS
- JavaScript
```

# 🗁 Nested Markdown Elements

Markdown elements can be combined.

Example:

```markdown
# Programming Languages

## Frontend

1. **HTML**
   - Structure
2. **CSS**
   - Styling
3. **JavaScript**
   - Interactivity

> Practice every day to improve your skills.
```

This combines headings, ordered lists, unordered lists, bold formatting, and blockquotes.

# 🖆 Markdown Syntax Compatibility

Not every Markdown processor supports the same syntax.

| Feature | CommonMark | GitHub Flavored Markdown |
| --- | --- | --- |
| Headings | Yes | Yes |
| Paragraphs | Yes | Yes |
| Bold and italic | Yes | Yes |
| Ordered lists | Yes | Yes |
| Unordered lists | Yes | Yes |
| Blockquotes | Yes | Yes |
| Code blocks | Yes | Yes |
| Links | Yes | Yes |
| Images | Yes | Yes |
| Tables | No | Yes |
| Task lists | No | Yes |
| Strikethrough | No | Yes |
| Autolinks | Limited | Extended |

# 🖆 Best Practices

1. Use consistent heading levels.
2. Keep one blank line between block elements.
3. Prefer hyphens for unordered lists.
4. Use descriptive link text.
5. Specify language identifiers in code blocks.
6. Escape special characters when necessary.
7. Use standard Markdown syntax for better compatibility.
8. Test extended syntax in your target Markdown renderer.
