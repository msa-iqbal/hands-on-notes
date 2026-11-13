# Advanced Markdown

Advanced Markdown techniques help developers create professional documentation, technical guides, project manuals, knowledge bases, and maintainable repositories.

This lesson explores advanced Markdown concepts and practical techniques beyond basic syntax.

# 🗁 Markdown Flavors and Specifications

Markdown has several implementations, commonly called Markdown flavors.

Different platforms support different syntax extensions.

**Popular Markdown Implementations**

| Markdown Flavor                | Description                                            |
| ------------------------------ | ------------------------------------------------------ |
| CommonMark                     | Standardized Markdown specification                    |
| GitHub Flavored Markdown (GFM) | Markdown extensions used by GitHub                     |
| Markdown Extra                 | Extension supporting features such as definition lists |
| MultiMarkdown                  | Extended Markdown implementation                       |
| Pandoc Markdown                | Extended syntax used by Pandoc                         |

**Important Differences**

| Feature            | CommonMark | GFM | Pandoc              |
| ------------------ | ---------- | --- | ------------------- |
| Headings           | Yes        | Yes | Yes                 |
| Tables             | No         | Yes | Yes                 |
| Task lists         | No         | Yes | Extension-dependent |
| Footnotes          | No         | Yes | Yes                 |
| Definition lists   | No         | No  | Yes                 |
| Fenced code blocks | Yes        | Yes | Yes                 |
| Strikethrough      | No         | Yes | Yes                 |

**Best practice:** Know which Markdown renderer will process your document before using non-standard syntax.

# 🗁 HTML Integration

Markdown allows HTML integration in many implementations.

HTML is useful when Markdown syntax cannot express the structure or formatting you need.

### 🖆 HTML Headings

```html
<h2>Advanced Documentation</h2>
<h3>Technical Details</h3>
```

### 🖆 HTML Paragraphs

```html
<p>This is an HTML paragraph inside a Markdown document.</p>
```

### 🖆 HTML Line Breaks

```html
First line.<br>
Second line.<br>
Third line.
```

### 🖆 HTML Horizontal Rule

```html
<hr>
```

### 🖆 HTML Details and Summary

```html
<details>
  <summary>Show more information</summary>

  This content is hidden until the section is expanded.

</details>
```

### 🖆 HTML Superscript and Subscript

```html
<p>Water formula: H<sub>2</sub>O</p>

<p>Einstein's equation: E = mc<sup>2</sup></p>
```

Output:

Water formula: H<sub>2</sub>O

Einstein's equation: E = mc<sup>2</sup>

**Important:** HTML support varies by renderer. Some platforms sanitize HTML or restrict specific elements and attributes.

# 🗁 Footnotes

Footnotes provide additional information without interrupting the main text.

They are useful in technical documentation, research notes, and tutorials.

**Syntax**

```markdown
Markdown is a lightweight markup language.[^1]

[^1]: Markdown was originally created by John Gruber.
```

**Example**

Markdown is a lightweight markup language.[^1]

[^1]: Markdown was originally created by John Gruber.

**Note:** Footnotes are not part of CommonMark or GFM core syntax. Support depends on the Markdown implementation.

# 🗁 Definition Lists

Definition lists allow you to define technical terms.

**Syntax**

```markdown
HTML
: HyperText Markup Language.

CSS
: Cascading Style Sheets.

JavaScript
: A programming language commonly used for web development.
```

This syntax is supported by some Markdown implementations, including Pandoc and Markdown Extra, but not by standard CommonMark or GitHub Flavored Markdown.

**Alternative Using Standard Markdown**

```markdown
**HTML**

HyperText Markup Language.

**CSS**

Cascading Style Sheets.

**JavaScript**

A programming language commonly used for web development.
```

The alternative is more portable across Markdown renderers.

# 🗁 Table of Contents

A table of contents (TOC) helps readers navigate long Markdown documents.

### 🖆 Manual Table of Contents

```markdown
# Documentation

## Table of Contents

- [Introduction](#introduction)
- [Installation](#installation)
- [Configuration](#configuration)
- [Usage](#usage)
- [Troubleshooting](#troubleshooting)

## Introduction

Introduction content.

## Installation

Installation instructions.

## Configuration

Configuration details.

## Usage

Usage examples.

## Troubleshooting

Common problems and solutions.
```

### 🖆 Automatic Heading Anchors

GitHub automatically generates heading anchors.

For example:

```markdown
## Installation Guide
```

Usually produces this anchor:

```text
#installation-guide
```

### 🖆 Linking to a Heading

```markdown
[Go to Installation](#installation-guide)
```

### 🖆 Heading Anchor Rules on GitHub

GitHub generally:

1. Converts heading text to lowercase.
2. Removes many punctuation characters.
3. Replaces spaces with hyphens.
4. Generates unique anchors for duplicate headings by adding suffixes.

Example:

```markdown
## Getting Started

## Getting Started
```

The headings receive distinct anchors, typically:

```text
#getting-started
#getting-started-1
```

**Note:** Anchor generation can vary between Markdown platforms. Check the rendered document when writing cross-platform links.

# 🗁 Nested Markdown Structures

Markdown supports combining several elements.

**Example: Lists With Code Blocks**

````markdown
1. Install Node.js.

   ```bash
   node --version
   npm --version
   ```

2. Create a project.

   ```bash
   mkdir my-project
   cd my-project
   ```
````

**Example: Blockquotes With Lists**

```markdown
> Development checklist:
>
> - Create project structure.
> - Configure dependencies.
> - Write application code.
> - Test the application.
```

**Example: Blockquotes With Code**

````markdown
> Example command:
>
> ```bash
> npm install
> ```
````

**💡 Importance Note:**

Use nested structures when they improve organization. Excessively nested Markdown can become difficult to maintain.

# 🗁 Images With Advanced HTML

Markdown image syntax is sufficient for ordinary images, but HTML can provide additional control.

### 🖆 Image Dimensions

```html
<img src="assets/logo.png" alt="Project Logo" width="200">
```

### 🖆 Image Width and Height

```html
<img
  src="assets/banner.png"
  alt="Project Banner"
  width="800"
  height="300"
>
```

### 🖆 Centered Image

```html
<p align="center">
  <img src="assets/logo.png" alt="Project Logo" width="200">
</p>
```

### 🖆 Image With Caption

```html
<figure>
  <img src="assets/dashboard.png" alt="Application dashboard">
  <figcaption>Application dashboard overview</figcaption>
</figure>
```

**Note:** HTML image styling and figure rendering depend on the platform. For accessible documentation, always provide meaningful alternative text.

# 🗁 Markdown Automation

Markdown files can be generated, converted, checked, and published automatically.

### 🖆 Markdown Conversion

#### ⌘ Markdown to HTML

A Markdown converter can transform Markdown into HTML.

For example, with Pandoc:

```bash
pandoc README.md -o README.html
```

#### ⌘ Markdown to PDF

```bash
pandoc README.md -o README.pdf
```

PDF output may require an appropriate PDF engine and additional configuration.

#### ⌘ Markdown to Word

```bash
pandoc README.md -o README.docx
```

### 🖆 Markdown Linting

Markdown linters help identify formatting inconsistencies.

Example using markdownlint-cli2:

```bash
npm install --save-dev markdownlint-cli2
```

Run the linter:

```bash
npx markdownlint-cli2 "**/*.md"
```

A configuration file can be used to customize rules.

Example `.markdownlint-cli2.jsonc`:

```json
{
  "config": {
    "default": true,
    "MD013": false,
    "MD033": false
  }
}
```

### 🖆 Prettier Formatting

Prettier can format Markdown files consistently.

Install:

```bash
npm install --save-dev prettier
```

Format Markdown files:

```bash
npx prettier --write "**/*.md"
```

Check formatting without modifying files:

```bash
npx prettier --check "**/*.md"
```

### 🖆 Tool Comparison

| Tool | Purpose |
| ------ | --------- |
| Pandoc | Convert Markdown into other formats |
| markdownlint-cli2 | Detect Markdown style issues |
| Prettier | Format Markdown consistently |
| GitHub Actions | Automate documentation workflows |
| MkDocs | Generate documentation websites |
| Docusaurus | Build documentation websites |

# 🗁 Markdown in Git Workflows

Markdown files are commonly maintained alongside source code in Git repositories.

### 🖆 Create a Documentation File

```bash
touch README.md
```

### 🖆 Check Changes

```bash
git status
```

### 🖆 Stage Documentation

```bash
git add README.md
```

### 🖆 Commit Changes

```bash
git commit -m "docs: add project documentation"
```

### 🖆 Push Changes

```bash
git push origin main
```

### 🖆 Conventional Commit Examples

| Commit Message | Purpose |
| ---------------- | --------- |
| `docs: add installation guide` | Add documentation |
| `docs: update API reference` | Update API documentation |
| `docs: fix broken links` | Correct documentation links |
| `docs: improve README` | Improve project overview |

The `docs` prefix is a widely used Conventional Commits convention for documentation changes.

# 🗁 Documentation Website Generators

Markdown can be used as the content source for documentation websites.

### 🖆 MkDocs

MkDocs generates documentation websites from Markdown files and a YAML configuration file.

Example structure:

```text
my-docs/
├── docs/
│   ├── index.md
│   ├── installation.md
│   └── configuration.md
└── mkdocs.yml
```

Example `mkdocs.yml`:

```yaml
site_name: My Documentation

nav:
  - Home: index.md
  - Installation: installation.md
  - Configuration: configuration.md
```

### 🖆 Docusaurus

Docusaurus is a documentation website framework that supports Markdown and MDX.

Example structure:

```text
my-docs/
├── docs/
│   ├── intro.md
│   └── installation.md
├── docusaurus.config.js
└── package.json
```

### 🖆 Jekyll

Jekyll can generate static websites from Markdown and other source files.

Example structure:

```text
my-site/
├── _config.yml
├── index.md
├── about.md
└── _posts/
    └── 2026-01-01-first-post.md
```

### 🖆 Comparison

| Tool | Main Purpose |
| ------ | -------------- |
| MkDocs | Technical documentation websites |
| Docusaurus | Product and developer documentation |
| Jekyll | Static websites and blogs |
| Hugo | Fast static site generation |
| GitHub Pages | Hosting static websites |

# 🖆 Accessibility in Markdown

Accessible documentation helps more people understand and use your content.

### 🖆 Meaningful Image Alt Text

Poor:

```markdown
![image](diagram.png)
```

Better:

```markdown
![OSI model showing seven network layers](osi-model.png)
```

### 🖆 Descriptive Links

Poor:

```markdown
[Click here](https://example.com)
```

Better:

```markdown
[Read the installation guide](https://example.com)
```

### 🖆 Logical Heading Hierarchy

Poor:

```markdown
# Project

#### ⌘ Installation

## Configuration
```

Better:

```markdown
# Project

## Installation

## Configuration
```

### 🖆 Clear Tables

Use descriptive column headings and avoid unnecessarily complex table structures.

### 🖆 Accessibility Checklist

* Use meaningful alternative text for informative images.
* Use descriptive link text.
* Maintain logical heading levels.
* Write clear and concise sentences.
* Avoid using color as the only way to communicate meaning.
* Keep tables understandable.
* Avoid excessive decorative formatting.

# 🖆 Markdown Security Considerations

Markdown documents may contain links, images, and embedded HTML.

When processing untrusted Markdown, security matters.

### 🖆 Important Risks

| Risk | Description |
| ------ | ------------- |
| XSS | Malicious HTML or script injection |
| Unsafe URLs | Links using dangerous URL schemes |
| Remote resources | External images or resources may expose information |
| HTML injection | Untrusted HTML may alter rendered content |

### 🖆 Security Best Practices

1. Sanitize HTML when rendering untrusted Markdown.
2. Use a trusted Markdown parser.
3. Validate URLs before rendering links.
4. Restrict unsafe HTML and URL schemes.
5. Avoid embedding secrets in documentation.
6. Review external resources and dependencies.
7. Apply an appropriate Content Security Policy when serving rendered content.

**Example: Untrusted Markdown**

Never assume user-submitted Markdown is safe simply because it is plain text. The rendering pipeline must enforce appropriate sanitization and URL policies.

# 🖆 Reusable Documentation Patterns

Consistent documentation patterns make large projects easier to understand and maintain.

### 🖆 Standard Documentation Template

```markdown
# Topic Name

## Overview

Briefly introduce the topic.

## Learning Objectives

By the end of this lesson, you should understand:

- Objective 1
- Objective 2
- Objective 3

## Explanation

Detailed explanation.

## Syntax

```text
Example syntax
```

#### ⌘ Examples

Practical examples.

#### ⌘ Use Cases

Real-world applications.

#### ⌘ Common Mistakes

Common errors and solutions.

#### ⌘ Best Practices

Recommended approaches.

#### ⌘ Exercises

Practice activities.

#### ⌘ Key Takeaways

Important points to remember.

### 🖆 🕮 API Documentation Template

#### ⌘ API Endpoint

#### ⌘ Endpoint

`GET /api/users`

#### ⌘ Description

Retrieve a list of users.

#### ⌘ Authentication

Bearer token required.

#### ⌘ Request Parameters

| Parameter | Type    | Required |
| --------- | ------- | -------- |
| page      | integer | No       |
| limit     | integer | No       |

#### ⌘ Example Request

```bash
curl -X GET https://example.com/api/users
```

#### ⌘ Example Response

```json
{
  "success": true,
  "data": [
    {
      "id": 1,
      "name": "Rahim"
    }
  ]
}
```

#### ⌘ Response Codes

| Code | Description |
|------|-------------|
| 200 | Success |
| 401 | Unauthorized |
| 500 | Server error |

### 🖆 Configuration Documentation Template

#### ⌘ Configuration

#### ⌘ Environment Variables

| Variable  | Description             | Default     |
| --------- | ----------------------- | ----------- |
| APP_ENV   | Application environment | development |
| APP_DEBUG | Debug mode              | false       |
| APP_PORT  | Application port        | 3000        |

#### ⌘ Example Configuration

```dotenv
APP_ENV=development
APP_DEBUG=true
APP_PORT=3000
```

#### 💡 NOTES

* Never commit production secrets.
* Use environment-specific configuration.
* Validate required variables during startup.

# 🖆 Professional Documentation Best Practices

Professional technical documentation should be accurate, consistent, searchable, and maintainable.

### 🖆 Organization

* Use descriptive file names.
* Organize topics into logical folders.
* Maintain a central README or index.
* Use consistent numbering when a learning sequence requires it.

### 🖆 Content

* Start each document with a clear overview.
* Explain technical terms.
* Include practical examples.
* Add troubleshooting guidance.
* Provide exercises where appropriate.

### 🖆 Formatting

* Maintain a consistent heading hierarchy.
* Use code blocks for technical examples.
* Use tables for structured information.
* Use alerts selectively.
* Avoid unnecessary HTML.

### 🖆 Maintenance

* Review outdated examples.
* Verify links regularly.
* Run Markdown linting.
* Format files consistently.
* Keep documentation synchronized with application changes.
