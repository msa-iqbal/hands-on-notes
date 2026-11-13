# HTML Images

HTML images allow developers to display visual content such as photographs, illustrations, icons, diagrams, and graphics on web pages.

The `<img>` element is used to embed an image into an HTML document. It is a void element, meaning it does not have a closing tag.

Images improve visual communication, explain complex information, and make web interfaces more engaging.

However, images should be implemented with appropriate alternative text, dimensions, loading behavior, and file formats to ensure accessibility and performance.

# 🗁 Introduction to HTML Images

Images are an important part of modern websites.

Common use cases include:

* Product images in e-commerce applications.
* Profile pictures and avatars.
* Blog post thumbnails.
* Company logos.
* Infographics and diagrams.
* Website banners.
* Illustrations and decorative graphics.

HTML provides the `<img>` element to embed images.

### 🖆 Basic Example

```html
<img src="images/profile.jpg" alt="Profile photograph">
```

The browser loads the image specified by `src` and displays it in the document.

# 🗁 The `<img>` Element

The `<img>` element embeds an image into an HTML page.

### 🖆 Syntax

```html
<img src="image-path" alt="Image description">
```

### 🖆 Common Attributes

| Attribute | Purpose |
| --- | --- |
| `src` | Specifies the image resource |
| `alt` | Provides alternative text |
| `width` | Specifies the image width |
| `height` | Specifies the image height |
| `loading` | Controls loading behavior |
| `decoding` | Provides a decoding hint |
| `fetchpriority` | Provides a fetch priority hint |
| `srcset` | Specifies alternative image resources |
| `sizes` | Describes the expected rendered image size |
| `crossorigin` | Configures cross-origin image fetching |
| `usemap` | Associates the image with a client-side image map |

### 🖆 Example

```html
<img
    src="images/product.jpg"
    alt="Black wireless headphones"
    width="400"
    height="300"
>
```

The image has a source, alternative text, and explicit dimensions.

# 🗁 The `src` Attribute

The `src` attribute specifies the location of an image resource.

It accepts a URL pointing to an image file.

### 🖆 Relative Path

A relative path references an image based on the current document's location.

```html
<img src="images/logo.png" alt="Company logo">
```

### 🖆 Parent Directory

Use `../` to move one directory upward.

```html
<img src="../assets/images/banner.jpg" alt="Website banner">
```

### 🖆 Absolute URL

An absolute URL points to a resource using its complete URL.

```html
<img
    src="https://example.com/images/product.jpg"
    alt="Product image"
>
```

### 🖆 Local Project Structure

```text
project/
├── index.html
├── about.html
└── images/
    ├── logo.png
    ├── profile.jpg
    └── banner.webp
```

From `index.html`:

```html
<img src="images/logo.png" alt="Company logo">
<img src="images/profile.jpg" alt="User profile">
<img src="images/banner.webp" alt="Website banner">
```

Image paths should be correct relative to the HTML document's URL, not necessarily relative to the CSS or JavaScript file.

# 🗁 The `alt` Attribute

The `alt` attribute provides alternative text for an image.

Screen readers can use this text to communicate the image's meaning to users who cannot see it.

It is also displayed when an image cannot be loaded in some browser situations.

### 🖆 Meaningful Alternative Text

```html
<img
    src="images/developer.jpg"
    alt="Developer working on a laptop at a desk"
>
```

The alternative text communicates the meaningful content of the image.

### 🖆 Decorative Images

Decorative images do not provide meaningful information.

Use an empty `alt` attribute for these images.

```html
<img src="images/decorative-line.png" alt="">
```

An empty `alt` tells assistive technologies that the image can be ignored.

### 🖆 Images That Act as Links

When an image is the only content inside a link, its alternative text should describe the link's purpose.

```html
<a href="/home">
    <img src="images/logo.png" alt="Company homepage">
</a>
```

### 🖆 Images Containing Text

If an image contains important text, provide an equivalent text alternative.

```html
<img
    src="images/promotion.jpg"
    alt="Summer sale: 30 percent off selected products"
>
```

Whenever possible, use real HTML text instead of embedding text inside an image.

### 🖆 Complex Images

Charts, diagrams, and infographics may require more than a short `alt` description.

Provide a concise alternative text and a longer description elsewhere in the document.

```html
<figure>
    <img
        src="images/sales-chart.png"
        alt="Bar chart showing monthly sales increasing throughout the year"
        aria-describedby="sales-chart-description"
    >

    <figcaption id="sales-chart-description">
        Sales increased from $10,000 in January to $25,000 in December.
        The largest increase occurred between September and October.
    </figcaption>
</figure>
```

### 🖆 Important Rules for `alt`

* Describe the image's purpose and meaningful content.
* Keep alternative text concise and contextual.
* Do not repeat nearby captions unnecessarily.
* Do not start with phrases such as "Image of" unless they add useful context.
* Use `alt=""` for purely decorative images.
* Do not omit `alt` when using `<img>`.

# 🗁 Image Dimensions

The `width` and `height` attributes specify an image's intrinsic dimensions in CSS pixels.

```html
<img
    src="images/product.jpg"
    alt="Wireless headphones"
    width="600"
    height="400"
>
```

Providing dimensions helps the browser calculate the image's aspect ratio and reserve space before the image loads.

This reduces unexpected layout shifts.

### 🖆 Aspect Ratio

The aspect ratio describes the proportional relationship between width and height.

For example:

```text
Width:  600px
Height: 400px

Aspect ratio: 3:2
```

When an image has a width of 600 pixels and a height of 400 pixels, its aspect ratio is 3:2.

### 🖆 Responsive Image Dimensions

CSS can control how images fit their containers.

```css
img {
    max-width: 100%;
    height: auto;
}
```

This prevents images from exceeding the width of their containing elements while preserving their aspect ratio.

### 🖆 Fixed Aspect Ratio

Use `aspect-ratio` when a consistent visual shape is required.

```css
.product-image {
    width: 100%;
    aspect-ratio: 4 / 3;
    object-fit: cover;
}
```

The `object-fit` property controls how replaced content such as images fits inside its specified dimensions.

Common values:

| Value | Behavior |
| --- | --- |
| `fill` | Stretches content to fill the box |
| `contain` | Fits the entire image inside the box |
| `cover` | Fills the box while preserving aspect ratio, cropping when necessary |
| `none` | Uses the image's intrinsic size |
| `scale-down` | Uses the smaller result of `none` and `contain` |

# 🗁 Image File Formats

Different image formats serve different purposes.

| Format | Description | Common Use |
| --- | --- | --- |
| JPEG / JPG | Lossy compression | Photographs |
| PNG | Lossless compression with transparency | Logos and graphics |
| GIF | Supports simple animation | Small animated graphics |
| SVG | Vector-based graphics | Icons and logos |
| WebP | Modern image format supporting lossy and lossless compression | General web images |
| AVIF | Modern image format with efficient compression | High-quality optimized images |

### 🖆 JPEG

JPEG is commonly used for photographs and complex images.

```html
<img src="images/photo.jpg" alt="Mountain landscape">
```

### 🖆 PNG

PNG supports transparency and lossless compression.

```html
<img src="images/logo.png" alt="Company logo">
```

### 🖆 SVG

SVG uses XML-based vector graphics and can scale without losing sharpness.

```html
<img src="images/icon.svg" alt="Search icon">
```

For simple decorative SVG icons, inline SVG or CSS-based approaches may be appropriate depending on the use case.

### 🖆 WebP

WebP provides efficient compression for many web image use cases.

```html
<img src="images/product.webp" alt="Product photograph">
```

### 🖆 AVIF

AVIF can provide excellent compression and image quality.

```html
<img src="images/banner.avif" alt="Website banner">
```

When supporting different formats, use the `<picture>` element with fallback sources. This is covered in the picture element chapter.

# 🗁 Image Paths

Understanding image paths is essential when organizing website assets.

### 🖆 Relative Paths

```html
<img src="images/logo.png" alt="Logo">
```

The browser resolves the path relative to the current document URL.

### 🖆 Parent Directory Paths

```html
<img src="../images/logo.png" alt="Logo">
```

The `../` segment moves to the parent directory.

### 🖆 Root-Relative Paths

```html
<img src="/assets/images/logo.png" alt="Logo">
```

A path beginning with `/` is resolved relative to the website's origin root.

### 🖆 Absolute URLs

```html
<img
    src="https://cdn.example.com/assets/logo.png"
    alt="Company logo"
>
```

Absolute URLs are useful when loading images from external servers or content delivery networks.

# 🗁 Responsive Image Basics

Responsive images adapt to different screen sizes and display conditions.

A basic responsive image can use CSS:

```html
<img
    src="images/banner.jpg"
    alt="Mountain landscape"
    width="1200"
    height="600"
>
```

```css
img {
    display: block;
    max-width: 100%;
    height: auto;
}
```

This ensures the image does not overflow its container.

For more advanced responsive image delivery, HTML provides `srcset` and `sizes`.

These attributes allow the browser to select an appropriate image resource based on viewport conditions and device characteristics.

They are covered in detail in the responsive images chapter.

# 🗁 Image Loading

Image loading behavior affects website performance.

HTML provides the `loading` attribute to control when an image should be loaded.

### 🖆 Eager Loading

```html
<img
    src="images/hero.jpg"
    alt="Main website banner"
    loading="eager"
>
```

`loading="eager"` requests the image without lazy-loading behavior.

It is the default behavior when the attribute is omitted.

### 🖆 Lazy Loading

```html
<img
    src="images/product.jpg"
    alt="Product photograph"
    loading="lazy"
>
```

`loading="lazy"` allows the browser to defer loading an image until it approaches the visible area.

It is useful for images below the initial viewport.

Avoid lazy-loading important above-the-fold images, especially the main hero image or likely Largest Contentful Paint (LCP) element.

### 🖆 Image Decoding

The `decoding` attribute provides the browser with a decoding hint.

```html
<img
    src="images/article.jpg"
    alt="Article illustration"
    decoding="async"
>
```

Common values:

| Value | Description |
| --- | --- |
| `sync` | Requests synchronous decoding |
| `async` | Requests asynchronous decoding |
| `auto` | Allows the browser to choose |

The browser may make its own scheduling decisions.

### 🖆 Fetch Priority

The `fetchpriority` attribute provides a relative priority hint for fetching a resource.

```html
<img
    src="images/hero.jpg"
    alt="Main website banner"
    fetchpriority="high"
>
```

Common values:

* `high`
* `low`
* `auto`

Use high priority selectively for important images, such as a prominent LCP image. Do not assign high priority to every image.

### 🖆 Combined Example

```html
<img
    src="images/hero.webp"
    alt="Mountain landscape at sunrise"
    width="1600"
    height="800"
    loading="eager"
    fetchpriority="high"
    decoding="async"
>
```

# 🗁 Image Styling with CSS

CSS controls image appearance, dimensions, alignment, and cropping.

### 🖆 Basic Styling

```css
img {
    max-width: 100%;
    height: auto;
    display: block;
}
```

### 🖆 Rounded Images

```css
.profile-image {
    width: 150px;
    height: 150px;
    border-radius: 50%;
    object-fit: cover;
}
```

### 🖆 Image Borders

```css
.product-image {
    width: 300px;
    border: 2px solid #ddd;
    border-radius: 8px;
}
```

### 🖆 Object Fit

```css
.product-image {
    width: 300px;
    height: 200px;
    object-fit: cover;
}
```

### 🖆 Object Position

```css
.product-image {
    width: 300px;
    height: 200px;
    object-fit: cover;
    object-position: center top;
}
```

`object-position` controls which part of the image is visible when the image is cropped.

### 🖆 Image Hover Effect

```css
.product-image {
    transition: transform 0.3s ease;
}

.product-image:hover {
    transform: scale(1.05);
}
```

For interfaces with motion effects, respect the user's reduced-motion preference.

```css
@media (prefers-reduced-motion: reduce) {
    .product-image {
        transition: none;
    }
}
```

# 🗁 Accessibility Considerations

Images should communicate meaningful information to all users.

### 🖆 Provide Alternative Text

Every meaningful image should have an appropriate `alt` value.

```html
<img
    src="images/team.jpg"
    alt="Three team members collaborating around a laptop"
>
```

### 🖆 Use Empty Alt for Decorative Images

```html
<img src="images/decoration.svg" alt="">
```

### 🖆 Use `<figure>` and `<figcaption>`

Use these elements when an image needs a visible caption.

```html
<figure>
    <img
        src="images/architecture.jpg"
        alt="Modern building with a glass exterior"
    >

    <figcaption>
        Modern architectural design.
    </figcaption>
</figure>
```

### 🖆 Avoid Text Embedded in Images

Text embedded in images can become difficult to read, resize, translate, and access.

Use HTML text whenever possible.

### 🖆 Maintain Sufficient Contrast

When images contain text or meaningful visual information, ensure that users can distinguish the content.

For text placed over background images, provide sufficient contrast using overlays, backgrounds, or alternative layouts.

# 🗁 Best Practices

* Use descriptive and meaningful alternative text.
* Use `alt=""` for decorative images.
* Provide explicit `width` and `height` dimensions.
* Optimize images before uploading them.
* Select an appropriate image format.
* Use responsive image techniques for different devices.
* Lazy-load images that are below the initial viewport.
* Avoid lazy-loading important above-the-fold images.
* Use `fetchpriority="high"` selectively for critical images.
* Use CSS for image presentation and layout.
* Use `<figure>` and `<figcaption>` when captions are appropriate.
* Keep image filenames meaningful and organized.
* Avoid unnecessarily large image files.
* Test image accessibility with screen readers.

# 🗁 Common Mistakes

### 🖆 Missing Alternative Text

Incorrect:

```html
<img src="images/profile.jpg">
```

Better:

```html
<img src="images/profile.jpg" alt="User profile photograph">
```

### 🖆 Using Alternative Text for Decorative Images

Incorrect:

```html
<img src="images/decoration.png" alt="Decorative image">
```

Better:

```html
<img src="images/decoration.png" alt="">
```

### 🖆 Missing Image Dimensions

Without dimensions, the browser may not reserve the correct amount of space before an image loads.

Better:

```html
<img
    src="images/banner.jpg"
    alt="Website banner"
    width="1200"
    height="600"
>
```

### 🖆 Lazy-Loading Every Image

Do not automatically apply `loading="lazy"` to every image.

Important images near the top of the page should generally load eagerly.

### 🖆 Using Oversized Images

A very large image wastes bandwidth and can slow down page loading.

Resize and compress images according to their actual display requirements.

### 🖆 Stretching Images

Incorrect:

```css
img {
    width: 300px;
    height: 300px;
}
```

This can distort images if their aspect ratios differ.

Better:

```css
img {
    width: 300px;
    height: auto;
}
```

Or use `object-fit` when a fixed-size image container is required.

# 🗁 Practical Exercises

### 🖆 Exercise: Profile Image

Create a profile section containing:

* A profile photograph.
* A meaningful `alt` attribute.
* Explicit image dimensions.
* Rounded image styling.

### 🖆 Exercise: Product Gallery

Create a product gallery with:

* At least four product images.
* Descriptive alternative text.
* Responsive image dimensions.
* Rounded corners and consistent spacing.

### 🖆 Exercise: Blog Article Image

Create a blog article image using:

* `<figure>`.
* `<img>`.
* `<figcaption>`.
* Appropriate alternative text.
* Explicit image dimensions.

### 🖆 Exercise: Image Loading Optimization

Create a page containing several images.

Requirements:

* Load the main hero image eagerly.
* Use `fetchpriority="high"` only when justified.
* Lazy-load below-the-fold images.
* Provide image dimensions.
* Use optimized image formats.

# 🗁 Interview Questions

**What is the purpose of the `<img>` element?**

It embeds an image resource into an HTML document.

**What is the purpose of the `src` attribute?**

It specifies the URL of the image resource.

**What is the purpose of the `alt` attribute?**

It provides alternative text describing the image's meaningful content or function.

**Why should decorative images use `alt=""`?**

An empty alternative text allows assistive technologies to ignore images that provide no meaningful information.

**What is the difference between `width` and `max-width`?**

The HTML `width` attribute provides an image's intended intrinsic display width in CSS pixels. CSS `max-width` limits the rendered width so the image does not exceed its container.

**What is the purpose of `loading="lazy"`?**

It allows the browser to defer loading an image until it approaches the visible area.

**Why should important hero images not usually be lazy-loaded?**

Lazy-loading an important above-the-fold image can delay its discovery and loading, negatively affecting perceived performance and potentially LCP.

**What is the purpose of `object-fit`?**

It controls how an image fits inside its specified content box.

**What is the difference between JPEG and PNG?**

JPEG uses lossy compression and is commonly used for photographs. PNG uses lossless compression and supports transparency.

**What is the difference between raster and vector images?**

Raster images consist of pixels, while vector images use mathematical shapes and paths that can scale without losing sharpness.

**What is the purpose of `fetchpriority`?**

It provides the browser with a hint about the relative fetching priority of a resource.

**Why should image dimensions be specified?**

They help browsers reserve layout space and reduce unexpected layout shifts while images load.

# 🗁 References

* [MDN: HTML `<img>` Element](https://developer.mozilla.org/en-US/docs/Web/HTML/Reference/Elements/img)
* [MDN: Images in HTML](https://developer.mozilla.org/en-US/docs/Web/HTML/Guides/Images)
* [MDN: Image Loading](https://developer.mozilla.org/en-US/docs/Web/Performance/Guides/Lazy_loading)
* [MDN: Responsive Images](https://developer.mozilla.org/en-US/docs/Web/HTML/Guides/Responsive_images)
* [W3C WAI: Images Tutorial](https://www.w3.org/WAI/tutorials/images/)
* [WHATWG: The Image Element](https://html.spec.whatwg.org/multipage/embedded-content.html#the-img-element)
