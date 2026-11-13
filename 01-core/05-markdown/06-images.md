# Markdown Images

Markdown allows you to embed images directly into documents.

Images are commonly used for:

* Project logos
* Screenshots
* Architecture diagrams
* Network topology diagrams
* Workflow illustrations
* Application previews
* Tutorials and documentation

Markdown images use syntax similar to Markdown links, with an additional exclamation mark (`!`).

# 🗁 Basic Image Syntax

**Syntax**

```markdown
![Alternative Text](Image URL)
```

 **Example**

```markdown
![Company Logo](images/logo.png)
```

**Explanation**

| Component | Description |
| --- | --- |
| `!` | Identifies an image |
| `[Alternative Text]` | Describes the image |
| `(Image URL)` | Specifies the image location |

The alternative text is important for accessibility and for situations where the image cannot be displayed.

# 🗁 Image with Title

You can add an optional title to an image.

**Syntax**

```markdown
![Alternative Text](Image URL "Image Title")
```

**Example**

```markdown
![Company Logo](images/logo.png "Company Logo")
```

Some Markdown renderers display the title as a tooltip when users hover over the image.

# 🗁 Absolute Image URLs

An absolute image URL points to an image hosted at a complete web address.

**Example**

```markdown
![GitHub Logo](https://github.githubassets.com/images/modules/logos_page/GitHub-Mark.png)
```

**Another Example**

```markdown
![Example Image](https://example.com/images/banner.png)
```

### 🖆 When to Use

Use absolute URLs when:

* Images are hosted on an external server.
* Images come from a CDN.
* Images are publicly accessible online.
* You need to reference an image hosted outside your repository.

# 🗁 Relative Image Paths

Relative paths are commonly used to organize images inside a project.

**Example Project Structure**

```text
project/
│
├── README.md
│
├── docs/
│   └── installation.md
│
└── images/
    ├── logo.png
    ├── dashboard.png
    └── architecture.png
```

# 🗁 Image from the Project Root

Inside `README.md`:

```markdown
![Project Logo](images/logo.png)
```

### 🖆 Image from a Subdirectory

Inside `docs/installation.md`:

```markdown
![Installation Screenshot](../images/dashboard.png)
```

### 🖆 Image from the Same Directory

If an image is in the same directory as the Markdown file:

```markdown
![Screenshot](screenshot.png)
```

**💡 IMPORTANT**

Relative paths are generally preferable for images stored inside your project repository.

They help keep documentation portable when the repository is cloned or moved.

# 🗁 Alternative Text (Alt Text)

Alternative text describes the content or purpose of an image.

**Example**

```markdown
![Network topology showing three routers connected to a central switch](images/network-topology.png)
```

### 🖆 Why Alt Text Matters

1. Improves accessibility.
2. Helps screen readers describe images.
3. Provides context when images fail to load.
4. Makes documentation more understandable.
5. Improves the quality of technical documentation.

### 🖆 Good vs Poor Alt Text

Poor:

```markdown
![Image](images/network.png)
```

Better:

```markdown
![Network diagram showing a router connected to three switches](images/network.png)
```

Write concise descriptions that communicate the important information in the image.

For decorative images, an empty alt attribute may be appropriate:

```markdown
![](images/decorative-divider.png)
```

## 🗁 Image Formats

Markdown itself does not define image formats. Supported formats depend on the browser and hosting platform.

Common formats include:

| Format | Description | Common Use |
| --- | --- | --- |
| PNG | Lossless image format | Screenshots and diagrams |
| JPG/JPEG | Lossy image format | Photographs |
| GIF | Supports animation | Simple animations |
| SVG | Vector graphics | Logos and diagrams |
| WebP | Modern image format | Optimized web images |
| AVIF | Modern image format | High-efficiency images |

Example:

```markdown
![PNG Image](images/logo.png)

![JPEG Image](images/photo.jpg)

![SVG Image](images/diagram.svg)

![WebP Image](images/banner.webp)
```

# 🗁 Image Width and Height

Standard Markdown image syntax does not provide a portable way to specify image width and height.

Some Markdown processors support embedded HTML.

### 🖆 Using HTML Width

```html
<img src="images/logo.png" alt="Company Logo" width="200">
```

### 🖆 Using HTML Width and Height

```html
<img
  src="images/dashboard.png"
  alt="Application Dashboard"
  width="600"
  height="400"
>
```

### 🖆 Responsive Image

```html
<img
  src="images/banner.png"
  alt="Project Banner"
  width="100%"
>
```

HTML support and sanitization rules vary by platform. GitHub supports some HTML elements and attributes but may sanitize unsupported markup.

# 🗁 Clickable Images

You can combine image syntax with link syntax to make an image clickable.

**Syntax**

```markdown
[![Alt Text](Image URL)](Destination URL)
```

**Example**

```markdown
[![GitHub Logo](images/github-logo.png)](https://github.com)
```

When a user clicks the image, the browser navigates to GitHub.

### 🖆 Common Use Cases

* Project badges
* Website previews
* Video thumbnails
* Product images
* Repository banners

# 🗁 Images in GitHub Repositories

GitHub supports images stored inside repositories.

**Recommended Structure**

```text
my-project/
│
├── README.md
│
├── src/
│   └── app.js
│
├── docs/
│   └── guide.md
│
└── assets/
    ├── images/
    │   ├── logo.png
    │   ├── screenshot.png
    │   └── architecture.png
    │
    └── diagrams/
        └── workflow.svg
```

### 🖆 Referencing Images from README.md

```markdown
# My Project

![Project Logo](assets/images/logo.png)

## Application Preview

![Application Screenshot](assets/images/screenshot.png)

## Architecture

![System Architecture](assets/images/architecture.png)
```

### 🖆 Referencing Images from docs/guide.md

```markdown
![Project Screenshot](../assets/images/screenshot.png)
```

# 🗁 Images in Tables

Images can be included inside Markdown tables.

**Example**

```markdown
| Technology | Logo |
|---|---|
| HTML | ![HTML](images/html.png) |
| CSS | ![CSS](images/css.png) |
| JavaScript | ![JavaScript](images/javascript.png) |
```

Tables containing images are useful for technology overviews and comparison documents.

# 🗁 Image Alignment

Standard Markdown does not define image alignment controls.

You can use HTML where supported.

### 🖆 Center Alignment

```html
<p align="center">
  <img src="images/logo.png" alt="Project Logo" width="200">
</p>
```

### 🖆 Left Alignment

```html
<p align="left">
  <img src="images/logo.png" alt="Project Logo" width="200">
</p>
```

### 🖆 Right Alignment

```html
<p align="right">
  <img src="images/logo.png" alt="Project Logo" width="200">
</p>
```

Check the target renderer before relying on HTML alignment.

# 🗁 Animated GIF Images

Markdown can embed animated GIF files.

**Example**

```markdown
![Application Demo](images/application-demo.gif)
```

Common use cases:

* Demonstrating application behavior.
* Showing UI interactions.
* Explaining workflows.
* Presenting short tutorials.

# 🖆 Best Practices

1. Use lowercase filenames.
2. Separate words using hyphens.
3. Use descriptive filenames.
4. Keep images in dedicated directories.
5. Prefer relative paths for repository assets.
6. Optimize large images.
7. Write meaningful alternative text.
8. Avoid unnecessary duplicate images.
9. Verify image paths before publishing.
10. Use appropriate formats for the content.

Use descriptive filenames and organized directories.

Recommended:

```text
assets/
└── images/
    ├── project-logo.png
    ├── login-screen.png
    ├── user-dashboard.png
    └── network-architecture.png
```

Avoid:

```text
images/
├── img1.png
├── new.png
├── final2.png
└── screenshot123.png
```
