# Markdown Links

Markdown links allow you to connect text to another webpage, document, file, or section.

A standard Markdown link consists of:

* Link text
* URL or destination

**Basic Syntax**

```markdown
[Link Text](URL)
```

**Example**

```markdown
[Visit Google](https://www.google.com)
```

Rendered output:

[Visit Google](https://www.google.com)

# 🗁 Absolute URLs

An absolute URL contains the complete web address of a resource.

It generally includes:

* Protocol
* Domain
* Path
* Optional query parameters or fragments

**Syntax**

```markdown
[GitHub](https://github.com)
```

**More Examples**:

```markdown
[Google](https://www.google.com)

[YouTube](https://www.youtube.com)

[MDN Web Docs](https://developer.mozilla.org)

[GitHub Documentation](https://docs.github.com)
```

### 🖆 When to Use

Use absolute URLs when linking to external websites or resources.

# 🗁 Relative URLs

Relative URLs point to resources based on the location of the current document or the hosting platform's resolution rules.

They are particularly useful for linking files within a project.

**Example Project Structure**

```text
project/
│
├── README.md
│
├── docs/
│   ├── installation.md
│   ├── configuration.md
│   └── troubleshooting.md
│
├── images/
│   └── logo.png
│
└── examples/
    └── sample.md
```

### 🖆 Link to a File in the Same Directory

```markdown
[Installation Guide](installation.md)
```

### 🖆 Link to a File in a Subdirectory

From `README.md`:

```markdown
[Installation Guide](docs/installation.md)
```

### 🖆 Link to a Parent Directory

From `docs/installation.md`:

```markdown
[Project README](../README.md)
```

### 🖆 Link to a File in Another Directory

From `docs/installation.md`:

```markdown
[Sample File](../examples/sample.md)
```

### 🖆 Link to an Image File

```markdown
[View Logo](../images/logo.png)
```

### 🖆 When to Use

Relative URLs are recommended for internal project documentation because they usually remain valid when the project is moved or cloned.

# 🗁 Links with Titles

You can add a title to a link.

The title may appear as a tooltip when the user hovers over the link in supported renderers.

**Syntax**

```markdown
[Link Text](URL "Link Title")
```

**Example**

```markdown
[Visit GitHub](https://github.com "GitHub Homepage")
```

**Another Example**

```markdown
[MDN Documentation](https://developer.mozilla.org "Web Development Documentation")
```

# 🗁 Reference-Style Links

Reference-style links separate link text from the URL definition.

This is useful when a document contains repeated links or long URLs.

**Syntax**

```markdown
[Link Text][reference]

[reference]: https://example.com
```

**Example**

```markdown
Visit [GitHub][github] to explore repositories.

You can learn web development from [MDN][mdn].

[github]: https://github.com
[mdn]: https://developer.mozilla.org
```

### 🖆 Shortcut Reference Links

When the reference label matches the link text, you can use:

```markdown
[GitHub][]

[GitHub]: https://github.com
```

### 🖆 Collapsed Reference Links

You can also omit the reference label from the link:

```markdown
[GitHub]

[GitHub]: https://github.com
```

Reference labels are generally case-insensitive and whitespace-normalized under CommonMark.

# 🗁 Internal Links (Anchor Links)

Internal links allow readers to navigate to different sections within the same document.

They commonly use heading-generated anchors.

**Syntax**

```markdown
[Link Text](#heading-id)
```

**Example**

```markdown
# JavaScript Guide

## Introduction

JavaScript is a programming language.

## Variables

Variables store data.

## Functions

Functions organize reusable code.

[Go to Variables](#variables)

[Go to Functions](#functions)
```

### 🖆 Common GitHub Anchor Rules

GitHub commonly generates heading anchors by:

1. Converting heading text to lowercase.
2. Removing certain punctuation.
3. Replacing spaces with hyphens.
4. Handling duplicate headings with numeric suffixes.

Example:

```markdown
## Getting Started
```

Typical GitHub anchor:

```text
#getting-started
```

Another example:

```markdown
## API Reference
```

Typical GitHub anchor:

```text
#api-reference
```

**Note:** Anchor-generation rules differ between platforms. Verify anchors in the target renderer.

# 🗁 Links to Other Markdown Files

You can connect different Markdown documents within a project.

**Example Project Structure**

```text
documentation/
│
├── README.md
│
├── fundamentals/
│   ├── basics.md
│   ├── syntax.md
│   └── headings.md
│
└── advanced/
    ├── extensions.md
    └── examples.md
```

### 🖆 Links from README.md

```markdown
# Documentation

## Fundamentals

- [Basics](fundamentals/basics.md)
- [Syntax](fundamentals/syntax.md)
- [Headings](fundamentals/headings.md)

## Advanced

- [Extensions](advanced/extensions.md)
- [Examples](advanced/examples.md)
```

This technique is useful for building documentation indexes and knowledge bases.

# 🗁 Email Links

You can create clickable email addresses using the `mailto:` protocol.

**Syntax**

```markdown
[Email Us](mailto:info@example.com)
```

**Example**

```markdown
[Contact Support](mailto:support@example.com)
```

### 🖆 Email with Subject

```markdown
[Send Feedback](mailto:feedback@example.com?subject=Website%20Feedback)
```

This opens the user's default email application when supported.

# 🗁 Telephone Links

You can create clickable telephone links using the `tel:` protocol.

**Syntax**

```markdown
[Call Us](tel:+8801700000000)
```

**Example**

```markdown
[Contact Support](tel:+8801700000000)
```

Telephone-link behavior depends on the user's device and application.

# 🗁 Links with Special Characters

URLs may contain characters that have special meanings.

For example:

* Spaces
* Parentheses
* Query parameters
* Ampersands
* Fragments

Use URL encoding where necessary.

**Example: URL with Spaces**

```markdown
[Documentation](https://example.com/my%20documents)
```

**Example: Query Parameters**

```markdown
[Search Google](https://www.google.com/search?q=markdown)
```

**Example: Parentheses in URLs**

Angle brackets can help delimit URLs containing parentheses:

```markdown
[Example](<https://example.com/page_(guide)>)
```

Always test complex links in your target Markdown renderer.

# 🗁 Automatic Links

CommonMark supports automatic links for URLs enclosed in angle brackets.

**Example**

```markdown
<https://github.com>

<https://www.google.com>

<developer@example.com>
```

These are rendered as clickable links in compatible processors.

GitHub Flavored Markdown additionally supports extended autolinking for many bare URLs and email addresses.

# 🗁 Links with Formatting

You can apply inline formatting to link text.

### 🖆 Bold Link

```markdown
[**GitHub**](https://github.com)
```

### 🖆 Italic Link

```markdown
[*MDN Documentation*](https://developer.mozilla.org)
```

### 🖆 Combined Link

```markdown
[***Web Development Guide***](https://developer.mozilla.org)
```

# 🗁 Linking to Specific Lines or Files on GitHub

GitHub allows you to link to particular files and line ranges in repositories.

### 🖆 Link to a Repository File

```markdown
[View Source Code](https://github.com/username/repository/blob/main/README.md)
```

### 🖆 Link to a Specific Line

```markdown
[View Line 10](https://github.com/username/repository/blob/main/README.md#L10)
```

### 🖆 Link to a Line Range

```markdown
[View Lines 10-20](https://github.com/username/repository/blob/main/README.md#L10-L20)
```

Replace the example repository information with your actual repository path.

# 🗁 Creating a Table of Contents with Links

Internal links can create a navigable Table of Contents.

**Example**

```markdown
# Networking Documentation

## Table of Contents

1. [Introduction](#introduction)
2. [Network Types](#network-types)
3. [Network Devices](#network-devices)
4. [Network Security](#network-security)
5. [Conclusion](#conclusion)

## Introduction

Introduction to computer networking.

## Network Types

LAN, WAN, MAN, and PAN.

## Network Devices

Routers, switches, and firewalls.

## Network Security

Network protection concepts.

## Conclusion

Summary of networking fundamentals.
```

# 🖆 Best Practices

1. Use descriptive link text.
2. Avoid meaningless text such as "click here."
3. Prefer relative links for internal project files.
4. Use absolute URLs for external websites.
5. Verify links before publishing.
6. Use reference-style links when URLs are repeated.
7. Use anchor links for long documents.
8. Avoid excessively long visible URLs.
9. Keep link destinations relevant.
10. Check for broken links periodically.
