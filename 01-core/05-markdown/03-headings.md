# Markdown Headings

Markdown headings are used to define titles, sections, and subsections within a document.

They establish a hierarchy that helps readers understand the structure of the content.

Markdown supports six heading levels:

* H1 — Main heading
* H2 — Major section
* H3 — Subsection
* H4 — Sub-subsection
* H5 — Smaller subsection
* H6 — Lowest heading level

# 🗁 ATX Heading Syntax

ATX is the most commonly used Markdown heading style.

It uses the hash (`#`) character followed by a space and the heading text.

**Syntax**

```markdown
# Heading 1
## Heading 2
### Heading 3
#### Heading 4
##### Heading 5
###### Heading 6
```

### Rendered Example

# Heading 1

## Heading 2

### Heading 3

#### Heading 4

##### Heading 5

###### Heading 6

**Important:** Always include a space between the hash characters and heading text.

Correct:

```markdown
# Introduction
```

Incorrect:

```markdown
#Introduction
```

# 🗁 Understanding the Six Heading Levels

### H1 — Main Heading

H1 represents the primary title of a document.

```markdown
# Web Development
```

Typical usage:

* Document title
* Main project title
* Main tutorial title

### H2 — Major Section

H2 represents a major section within a document.

```markdown
## Frontend Development
```

Typical usage:

* Main topics
* Major documentation sections
* Learning modules

### H3 — Subsection

H3 represents a subsection under an H2 section.

```markdown
### HTML Fundamentals
```

Typical usage:

* Individual concepts
* Features
* Subtopics

### H4 — Sub-subsection

H4 provides additional organization within an H3 section.

```markdown
#### HTML Elements
```

### H5 — Detailed Subsection

H5 is useful for more detailed documentation.

```markdown
##### Block Elements
```

### H6 — Lowest Heading Level

H6 is the smallest heading level supported by Markdown.

```markdown
###### Inline Elements
```

# 🗁 Heading Hierarchy

Headings should follow a logical parent-child relationship.

Example:

```markdown
# Web Development

## Frontend Development

### HTML

#### HTML Elements

##### Block Elements

###### Paragraph Element

### CSS

#### CSS Selectors

## Backend Development

### Node.js

### PHP
```

Structure:

```text
H1: Web Development
│
├── H2: Frontend Development
│   ├── H3: HTML
│   │   └── H4: HTML Elements
│   │       └── H5: Block Elements
│   │           └── H6: Paragraph Element
│   │
│   └── H3: CSS
│       └── H4: CSS Selectors
│
└── H2: Backend Development
    ├── H3: Node.js
    └── H3: PHP
```

**Best practice:** Avoid skipping heading levels unnecessarily. For example, move from H2 to H3 rather than jumping directly to H5.

# 🗁 Setext Heading Syntax

Markdown also supports Setext-style headings.

Unlike ATX syntax, Setext uses underline characters.

### H1 Syntax

Use equal signs (`=`) below the heading.

```markdown
Web Development
===============
```

### H2 Syntax

Use hyphens (`-`) below the heading

```markdown
Frontend Development
--------------------
```

Setext supports only H1 and H2.

# 🗁 ATX vs Setext Headings

| Feature | ATX | Setext |
| --- | --- | --- |
| Syntax | `#` | `=` or `-` underline |
| H1 support | Yes | Yes |
| H2 support | Yes | Yes |
| H3–H6 support | Yes | No |
| Flexibility | High | Limited |
| Common usage | Very common | Less common |

**Recommendation:** Prefer ATX syntax for consistent documentation.

# 🗁 Optional Closing Hashes

ATX headings can include optional closing hash characters.

Example:

```markdown
# Introduction #

## Frontend Development ##

### HTML Fundamentals ###
```

The closing hashes do not affect the heading level.

These are equivalent:

```markdown
# Introduction
```

```markdown
# Introduction #
```

Closing hashes are optional and generally unnecessary.

# 🗁 Headings with Formatting

You can use inline Markdown formatting inside headings.

### Bold Heading

```markdown
## **Important Concepts**
```

### Italic Heading

```markdown
## *Advanced Topics*
```

### Combined Formatting

```markdown
## **Frontend** and *Backend*
```

Use formatting sparingly to keep headings readable.

# 🗁 Headings with Special Characters

Headings can contain numbers, punctuation, and other ordinary text.

Examples:

```markdown
## 1. Introduction

## 2. HTML & CSS

## 3. JavaScript (ES6+)

## 4. API Development

## 5. Troubleshooting: Common Issues
```

If a special Markdown character needs to appear literally, escape it when necessary.

# 🗁 Automatic Heading Anchors

Many Markdown platforms generate anchor links for headings.

These anchors allow readers to jump directly to a particular section.

Example heading:

```markdown
## Installation Guide
```

A typical GitHub anchor is:

```text
#installation-guide
```

You can link to it using:

```markdown
[Go to Installation Guide](#installation-guide)
```

Another example:

```markdown
## Getting Started

[Getting Started](#getting-started)
```

**Note:** Anchor-generation rules vary between platforms. Duplicate headings and special characters may affect the generated anchor.

# 🗁 Creating a Table of Contents

A Table of Contents (TOC) provides navigation to document sections.

Example:

```markdown
# JavaScript Guide

## Table of Contents

- [Introduction](#introduction)
- [Variables](#variables)
- [Data Types](#data-types)
- [Functions](#functions)
- [Objects](#objects)

## Introduction

JavaScript is a programming language.

## Variables

Variables store data.

## Data Types

JavaScript supports several data types.

## Functions

Functions organize reusable code.

## Objects

Objects store related properties and methods.
```

A TOC is particularly useful for long technical documents.

# 🖆 Best Practices for Headings

1. Use one clear H1 document title.
2. Use H2 for major sections.
3. Use H3 for subsections.
4. Maintain a logical heading hierarchy.
5. Avoid skipping heading levels unnecessarily.
6. Keep headings short and descriptive.
7. Use consistent capitalization.
8. Avoid using headings only to make text look larger.
9. Use unique and descriptive heading names.
10. Add a Table of Contents to long documents.

# 🖆 Common Mistakes

### Mistake 1: Missing Space

Incorrect:

```markdown
##Installation
```

Correct:

```markdown
## Installation
```

### Mistake 2: Skipping Heading Levels

Less organized:

```markdown
# Networking
##### TCP
```

Better:

```markdown
# Networking

## Transport Layer

### TCP
```

### Mistake 3: Using Multiple Main Titles

Avoid creating several unrelated H1 headings in a single document unless the document structure specifically requires them.

### Mistake 4: Using Headings for Visual Styling

Do not use headings just because you want larger text. Use bold or other appropriate inline formatting instead.
