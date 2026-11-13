# Responsive Images in HTML

Responsive images allow browsers to load and display appropriate image resources based on the device's screen size, pixel density, viewport dimensions, and layout requirements.

They help improve website performance, visual quality, bandwidth efficiency, and user experience across different devices.

HTML provides several attributes and elements for implementing responsive images:

* `srcset` — defines multiple image candidates.
* `sizes` — describes the expected rendered image width.
* `<picture>` — provides alternative image sources for art direction and format selection.
* `media` — defines conditions for selecting image sources.

A responsive image strategy helps solve common problems:

* Large images waste bandwidth on smaller screens.
* Low-resolution images appear blurry on high-density displays.
* Different screen sizes may require different image dimensions.
* Mobile and desktop layouts may need different image compositions.

Responsive images allow the browser to select a suitable resource instead of forcing every device to download the same image.

# 🗁 The `srcset` Attribute

The `srcset` attribute defines a list of image resources that the browser can choose from.

It is commonly used with the `<img>` element.

### 🖆 Basic Syntax

```html
<img
  src="images/photo-800.jpg"
  srcset="
    images/photo-480.jpg 480w,
    images/photo-800.jpg 800w,
    images/photo-1200.jpg 1200w
  "
  alt="A mountain landscape"
>
```

Each candidate contains:

* An image URL.
* A descriptor that identifies its intrinsic width.

The `w` descriptor represents the image's intrinsic width in pixels.

| Candidate | Descriptor |
| --- | --- |
| `photo-480.jpg` | `480w` |
| `photo-800.jpg` | `800w` |
| `photo-1200.jpg` | `1200w` |

The browser considers the available candidates, rendered image size, device pixel ratio, network conditions, and other factors when selecting an image.

### 🖆 Width Descriptors

Width descriptors are useful when providing multiple versions of the same image at different resolutions.

```html
<img
  src="images/product-800.jpg"
  srcset="
    images/product-400.jpg 400w,
    images/product-800.jpg 800w,
    images/product-1200.jpg 1200w,
    images/product-1600.jpg 1600w
  "
  alt="Wireless headphones"
  width="800"
  height="600"
>
```

The browser can select an image that provides sufficient resolution for the rendered size.

**Important:** When using width descriptors (`w`), provide a corresponding `sizes` attribute.

# 🗁 The `sizes` Attribute

The `sizes` attribute tells the browser how wide the image is expected to appear in the layout.

It helps the browser select an appropriate image candidate from `srcset`.

### 🖆 Basic Syntax

```html
<img
  src="images/photo-800.jpg"
  srcset="
    images/photo-400.jpg 400w,
    images/photo-800.jpg 800w,
    images/photo-1200.jpg 1200w
  "
  sizes="100vw"
  alt="Mountain landscape"
>
```

Here, `100vw` means the image is expected to occupy the full viewport width.

### 🖆 Responsive Sizes

```html
<img
  src="images/photo-800.jpg"
  srcset="
    images/photo-400.jpg 400w,
    images/photo-800.jpg 800w,
    images/photo-1200.jpg 1200w,
    images/photo-1600.jpg 1600w
  "
  sizes="
    (max-width: 600px) 100vw,
    (max-width: 1000px) 80vw,
    700px
  "
  alt="Mountain landscape"
>
```

The browser evaluates the `sizes` conditions from left to right.

| Condition | Expected Image Width |
| --- | --- |
| Viewport up to 600px | `100vw` |
| Viewport up to 1000px | `80vw` |
| Otherwise | `700px` |

The first matching condition determines the expected image slot width.

### 🖆 Common Size Values

| Value | Meaning |
| --- | --- |
| `100vw` | Full viewport width |
| `50vw` | Half viewport width |
| `80vw` | 80% of viewport width |
| `400px` | Fixed expected width |
| `calc(100vw - 32px)` | Viewport width minus 32px |

The `sizes` attribute describes the expected layout width. It does not directly apply CSS sizing.

# 🗁 Density Descriptors

Density descriptors allow browsers to select images based on device pixel density.

They use `x` descriptors rather than `w` descriptors.

### 🖆 Basic Example

```html
<img
  src="images/logo.png"
  srcset="
    images/logo.png 1x,
    images/logo-2x.png 2x,
    images/logo-3x.png 3x
  "
  alt="Company logo"
  width="200"
  height="60"
>
```

| Descriptor | Meaning |
| --- | --- |
| `1x` | Standard pixel density |
| `2x` | Twice the pixel density |
| `3x` | Three times the pixel density |

A display with a device pixel ratio of 2 can benefit from the `2x` image.

### 🖆 Device Pixel Ratio

Device Pixel Ratio (DPR) describes the relationship between physical pixels and CSS pixels.

For example:

* A DPR of 1 means one physical pixel per CSS pixel.
* A DPR of 2 means two physical pixels per CSS pixel.
* A DPR of 3 means three physical pixels per CSS pixel.

A 200 CSS-pixel-wide logo displayed at DPR 2 ideally needs approximately 400 physical pixels of image width.

**Important:** Do not mix width descriptors (`w`) and density descriptors (`x`) in the same `srcset` candidate list.

# 🗁 Responsive Images with Picture

The `<picture>` element provides alternative image sources based on format support and media conditions.

It is especially useful when different screen sizes need different image compositions.

### 🖆 Art Direction

Art direction means using different image crops or compositions for different viewing conditions.

```html
<picture>
  <source
    media="(max-width: 600px)"
    srcset="images/product-mobile.jpg"
  >

  <source
    media="(min-width: 601px)"
    srcset="images/product-desktop.jpg"
  >

  <img
    src="images/product-desktop.jpg"
    alt="A wireless headphone displayed on a desk"
    width="1200"
    height="800"
  >
</picture>
```

The browser selects a source based on the matching media condition.

### 🖆 Modern Image Formats

You can combine responsive image selection with modern image formats.

```html
<picture>
  <source
    type="image/avif"
    srcset="
      images/hero-800.avif 800w,
      images/hero-1200.avif 1200w
    "
    sizes="100vw"
  >

  <source
    type="image/webp"
    srcset="
      images/hero-800.webp 800w,
      images/hero-1200.webp 1200w
    "
    sizes="100vw"
  >

  <img
    src="images/hero-1200.jpg"
    alt="A scenic mountain landscape"
    width="1200"
    height="800"
  >
</picture>
```

The browser can select a supported format and an appropriate image candidate.

For detailed coverage of `<picture>`, `<source>`, and format selection, see `04-picture.md`.

# 🗁 Responsive Images with CSS

CSS controls how an image is displayed within its layout container.

The HTML `srcset` and `sizes` attributes control which image resource the browser can download.

### 🖆 Fluid Images

```html
<img
  src="images/landscape.jpg"
  alt="A mountain landscape"
  width="1200"
  height="800"
  class="responsive-image"
>
```

```css
.responsive-image {
  display: block;
  max-width: 100%;
  height: auto;
}
```

This prevents the image from exceeding its container width while preserving its aspect ratio.

### 🖆 Using Object Fit

```css
.product-image {
  width: 100%;
  height: 300px;
  object-fit: cover;
  object-position: center;
}
```

Common `object-fit` values:

| Value | Description |
| --- | --- |
| `contain` | Fits the complete image inside the container. |
| `cover` | Fills the container, potentially cropping the image. |
| `fill` | Stretches the image to fill the container. |
| `none` | Displays the image at its intrinsic size. |
| `scale-down` | Uses the smaller result of `none` or `contain`. |

# 🗁 Responsive Images in Real Projects

### 🖆 Blog Article Image

```html
<article>
  <h2>Understanding Responsive Images</h2>

  <img
    src="images/article-800.jpg"
    srcset="
      images/article-400.jpg 400w,
      images/article-800.jpg 800w,
      images/article-1200.jpg 1200w
    "
    sizes="
      (max-width: 600px) 100vw,
      (max-width: 1000px) 80vw,
      800px
    "
    alt="A developer working on a laptop"
    width="1200"
    height="800"
    loading="lazy"
    decoding="async"
  >

  <p>
    Responsive images help websites deliver suitable image resources
    to different devices.
  </p>
</article>
```

### 🖆 Product Card

```html
<div class="product-card">
  <img
    src="images/headphones-400.jpg"
    srcset="
      images/headphones-200.jpg 200w,
      images/headphones-400.jpg 400w,
      images/headphones-800.jpg 800w
    "
    sizes="(max-width: 600px) 50vw, 300px"
    alt="Black wireless headphones"
    width="800"
    height="800"
    loading="lazy"
  >

  <h3>Wireless Headphones</h3>

  <p>$99.00</p>
</div>
```

### 🖆 Hero Image

```html
<section class="hero">
  <picture>
    <source
      type="image/avif"
      srcset="
        images/hero-800.avif 800w,
        images/hero-1600.avif 1600w
      "
      sizes="100vw"
    >

    <img
      src="images/hero-1600.jpg"
      alt="A panoramic view of a mountain range"
      width="1600"
      height="900"
      fetchpriority="high"
    >
  </picture>
</section>
```

```css
.hero img {
  display: block;
  width: 100%;
  height: auto;
}
```

Use `fetchpriority="high"` only when the image is an important early-loading resource, such as the page's main hero image.

# 🗁 Performance Optimization

### 🖆 Use Appropriate Image Dimensions

Avoid serving unnecessarily large images.

For example, a 400 CSS-pixel-wide card usually does not need a 3000-pixel-wide image on every device.

Provide suitable candidates through `srcset`.

### 🖆 Use Modern Image Formats

Consider AVIF and WebP to reduce image transfer sizes while maintaining acceptable visual quality.

Retain an appropriate fallback when required.

### 🖆 Lazy Loading

Use `loading="lazy"` for images that are below the initial viewport.

```html
<img
  src="images/gallery.jpg"
  alt="A nature photograph"
  width="800"
  height="600"
  loading="lazy"
>
```

Avoid lazy-loading the primary LCP image.

### 🖆 Set Intrinsic Dimensions

Always provide appropriate `width` and `height` attributes when possible.

```html
<img
  src="images/photo.jpg"
  alt="A mountain landscape"
  width="1200"
  height="800"
>
```

These dimensions help the browser reserve layout space before the image finishes loading.

### 🖆 Avoid Unnecessary Image Downloads

Use `srcset` and `sizes` to give the browser information about available resources and expected layout dimensions.

Do not create many duplicate image candidates without a clear resolution or layout requirement.

# 🗁 Best Practices

* Use `srcset` to provide multiple image-resolution candidates.
* Use `sizes` with width-descriptor candidates.
* Use density descriptors when serving the same image at different pixel densities.
* Use `<picture>` for art direction and format selection.
* Keep image dimensions appropriate for their display sizes.
* Provide meaningful alternative text.
* Use `loading="lazy"` for non-critical below-the-fold images.
* Avoid lazy-loading important LCP images.
* Use `fetchpriority="high"` selectively for critical images.
* Optimize image formats and compression.
* Use CSS to control image presentation and layout.
* Test responsive image selection at different viewport widths and pixel densities.

# 🗁 Common Mistakes

### 🖆 Missing the `sizes` Attribute

When using width descriptors, omitting `sizes` can result in an incorrect or unintended image-size assumption.

Correct:

```html
<img
  src="images/photo-800.jpg"
  srcset="
    images/photo-400.jpg 400w,
    images/photo-800.jpg 800w,
    images/photo-1200.jpg 1200w
  "
  sizes="(max-width: 600px) 100vw, 800px"
  alt="Mountain landscape"
>
```

### 🖆 Mixing Width and Density Descriptors

Incorrect:

```html
<img
  srcset="
    images/photo-400.jpg 400w,
    images/photo-800.jpg 2x
  "
  src="images/photo.jpg"
  alt="Example image"
>
```

Use either width descriptors or density descriptors in a single `srcset` candidate list.

### 🖆 Incorrect Image Dimensions

Providing dimensions that do not match the actual image's intrinsic aspect ratio can cause unexpected layout behavior.

Ensure the width and height attributes describe the image's intrinsic dimensions.

### 🖆 Using CSS Instead of Responsive Image Selection

CSS can resize an image visually, but it does not automatically provide alternative source files.

Use `srcset`, `sizes`, or `<picture>` when the browser needs to choose among different image resources.

### 🖆 Lazy Loading Critical Images

Lazy-loading a hero image or another important above-the-fold image can delay its discovery and loading.

Use an appropriate loading strategy for critical images.

# 🗁 Practical Exercises

### 🖆 Exercise: Multiple Image Resolutions

Create an image component with 400px, 800px, and 1200px candidates using `srcset`.

### 🖆 Exercise: Responsive Image Sizes

Create an image that uses:

* Full viewport width on mobile.
* 80% viewport width on tablets.
* A maximum expected width of 800px on desktop.

Implement these rules with `sizes`.

### 🖆 Exercise: High-Density Displays

Create a logo with `1x`, `2x`, and `3x` image candidates.

### 🖆 Exercise: Responsive Product Gallery

Build a product gallery with:

* Multiple image candidates.
* Responsive sizing.
* Appropriate alternative text.
* Lazy loading for images below the fold.

### 🖆 Exercise: Performance-Optimized Hero

Create a hero image using:

* AVIF and JPEG resources.
* Responsive image candidates.
* Appropriate intrinsic dimensions.
* High fetch priority.

# 🗁 Interview Questions

**What are responsive images?**

Responsive images are images delivered in suitable resources or compositions based on device characteristics and layout requirements.

**What is the purpose of `srcset`?**

It defines multiple image candidates that the browser can choose from.

**What is the purpose of `sizes`?**

It describes the expected rendered width of an image so the browser can select an appropriate candidate from a width-descriptor `srcset`.

**What is the difference between `w` and `x` descriptors?**

`w` describes the intrinsic width of an image candidate, while `x` describes its pixel density.

**What is Device Pixel Ratio?**

It is the ratio between physical display pixels and CSS pixels.

**Why should `sizes` be used with width descriptors?**

It tells the browser the expected image slot width, helping it choose an appropriate candidate.

**What is art direction?**

Art direction uses different image compositions or crops for different display conditions.

**What is the difference between `srcset` and `<picture>`?**

`srcset` provides alternative image candidates for resolution selection. `<picture>` provides multiple source definitions and supports conditions such as format and media queries.

**What is the purpose of `loading="lazy"`?**

It allows the browser to defer loading an image until it approaches the visible area.

**How can responsive images improve performance?**

They help prevent unnecessarily large image downloads and deliver resources more appropriate to the device and layout.

# 🗁 Official References

* [MDN — Responsive Images](https://developer.mozilla.org/en-US/docs/Web/HTML/Guides/Responsive_images)
* [MDN — The img Element](https://developer.mozilla.org/en-US/docs/Web/HTML/Reference/Elements/img)
* [MDN — The Picture Element](https://developer.mozilla.org/en-US/docs/Web/HTML/Reference/Elements/picture)
* [MDN — The Source Element](https://developer.mozilla.org/en-US/docs/Web/HTML/Reference/Elements/source)
* [WHATWG — HTML Images](https://html.spec.whatwg.org/multipage/images.html)
