# HTML Picture Element

The HTML `<picture>` element provides a way to define multiple image sources for different situations, allowing browsers to select the most appropriate image based on supported formats, viewport conditions, and other source-selection criteria.

It is especially useful for **responsive image art direction** and **modern image format selection**.

The `<picture>` element works together with:

* `<source>` — defines alternative image sources.
* `<img>` — displays the selected image and provides the fallback.

Unlike `<img>`, the `<picture>` element does not display an image by itself. It acts as a container that gives the browser multiple image-source options.

# 🗁 Basic Syntax

```html
<picture>
  <source srcset="images/hero.webp" type="image/webp">

  <img
    src="images/hero.jpg"
    alt="A beautiful mountain landscape"
    width="1200"
    height="600"
  >
</picture>
```

The browser evaluates the available `<source>` elements and selects a suitable image. If no source is usable, it displays the image specified by the `<img>` element.

**Important:** The `<img>` element must be the last child inside `<picture>`.

# 🗁 Understanding the Picture Element

### 🖆 The `<picture>` Element

The `<picture>` element groups multiple image-source definitions.

```html
<picture>
  <!-- Alternative image sources -->

  <img src="images/default.jpg" alt="Example image">
</picture>
```

It does not have visual content or styling of its own. The actual image is rendered by the nested `<img>` element.

### 🖆 The `<source>` Element

The `<source>` element specifies an alternative image source.

It is commonly used inside `<picture>` with these attributes:

| Attribute | Description |
| --- | --- |
| `srcset` | Specifies one or more image URLs. |
| `media` | Defines the media condition for selecting the source. |
| `type` | Specifies the image format using a MIME type. |
| `sizes` | Provides image display-size information when using width-descriptor candidates. |

Example:

```html
<picture>
  <source
    srcset="images/photo.webp"
    type="image/webp"
  >

  <img src="images/photo.jpg" alt="Example photograph">
</picture>
```

The browser can select the WebP image when it supports that format. Otherwise, it can use the JPEG fallback.

### 🖆 The `<img>` Element

The `<img>` element is required inside `<picture>`.

It provides:

* The fallback image through `src`.
* Alternative text through `alt`.
* Intrinsic dimensions through `width` and `height`.
* Image-loading and decoding attributes.

Example:

```html
<picture>
  <source srcset="images/photo.avif" type="image/avif">

  <img
    src="images/photo.jpg"
    alt="A person walking through a forest"
    width="1200"
    height="800"
  >
</picture>
```

The `alt` attribute belongs to `<img>`, not `<source>`.

# 🗁 Modern Image Format Selection

Different image formats provide different levels of compression, quality, and browser support.

The `<picture>` element allows you to offer modern formats while retaining a compatible fallback.

### 🖆 AVIF, WebP, and JPEG Fallback

```html
<picture>
  <source
    srcset="images/landscape.avif"
    type="image/avif"
  >

  <source
    srcset="images/landscape.webp"
    type="image/webp"
  >

  <img
    src="images/landscape.jpg"
    alt="A landscape with mountains and a lake"
    width="1200"
    height="800"
  >
</picture>
```

The browser evaluates the sources in document order and uses the first applicable source it can use.

| Format | MIME Type | Common Use |
| --- | --- | --- |
| AVIF | `image/avif` | Highly compressed photographs and graphics |
| WebP | `image/webp` | Efficient photographs and transparent images |
| JPEG | `image/jpeg` | Broadly compatible photographs |
| PNG | `image/png` | Graphics requiring lossless compression or transparency |

The `type` attribute helps the browser determine whether it supports a particular image format without needing to fetch the image first.

### 🖆 Source Order Matters

Place preferred formats before fallback formats.

```html
<picture>
  <source srcset="images/photo.avif" type="image/avif">
  <source srcset="images/photo.webp" type="image/webp">

  <img src="images/photo.jpg" alt="Example photograph">
</picture>
```

The browser checks the sources in order and selects the first applicable, supported source.

# 🗁 Responsive Image Art Direction

Art direction means displaying different image compositions depending on the viewport or media conditions.

For example:

* Mobile devices display a portrait-oriented image.
* Desktop devices display a landscape-oriented image.
* Important visual content remains visible in both compositions.

### 🖆 Mobile and Desktop Images

```html
<picture>
  <source
    media="(max-width: 600px)"
    srcset="images/portrait-mobile.jpg"
  >

  <source
    media="(min-width: 601px)"
    srcset="images/landscape-desktop.jpg"
  >

  <img
    src="images/landscape-desktop.jpg"
    alt="A person standing beside a mountain lake"
    width="1200"
    height="600"
  >
</picture>
```

The browser evaluates the `media` conditions and selects the first applicable source.

The `media` attribute uses CSS media-query syntax.

### 🖆 Multiple Breakpoints

You can define several image sources for different viewport ranges.

```html
<picture>
  <source
    media="(max-width: 480px)"
    srcset="images/hero-small.jpg"
  >

  <source
    media="(max-width: 768px)"
    srcset="images/hero-medium.jpg"
  >

  <source
    media="(min-width: 769px)"
    srcset="images/hero-large.jpg"
  >

  <img
    src="images/hero-large.jpg"
    alt="A scenic mountain landscape"
    width="1600"
    height="900"
  >
</picture>
```

Order the sources from the most specific condition to the broader conditions when their media ranges could overlap.

# 🗁 Understanding `srcset` and `sizes`

The `srcset` attribute provides multiple image candidates.

It can be used with both `<img>` and `<source>` elements.

### 🖆 Width Descriptors

Width descriptors indicate the intrinsic width of each image candidate.

```html
<picture>
  <source
    type="image/webp"
    srcset="
      images/photo-480.webp 480w,
      images/photo-800.webp 800w,
      images/photo-1200.webp 1200w
    "
    sizes="(max-width: 600px) 100vw, 800px"
  >

  <img
    src="images/photo-800.jpg"
    alt="A landscape photograph"
    width="1200"
    height="800"
  >
</picture>
```

The browser considers the available candidates, the rendered image size, device characteristics, and other factors when choosing an appropriate resource.

### 🖆 Density Descriptors

Density descriptors indicate the pixel density of an image candidate.

```html
<picture>
  <source
    srcset="
      images/logo.webp 1x,
      images/logo-2x.webp 2x
    "
    type="image/webp"
  >

  <img
    src="images/logo.png"
    alt="Company logo"
    width="200"
    height="60"
  >
</picture>
```

Density descriptors are useful when providing different-resolution versions of the same image for displays with different pixel densities.

For more detailed coverage of `srcset`, `sizes`, and responsive image selection, see:

`05-responsive-images.md`

# 🗁 Picture vs. Image

Both `<picture>` and `<img>` are used when working with images, but they have different responsibilities.

| Feature | `<picture>` | `<img>` |
| --- | --- | --- |
| Displays the image | No, not by itself | Yes |
| Provides alternative sources | Yes | Supports `srcset` |
| Supports media conditions | Through `<source>` | No `media` attribute |
| Supports format selection | Through `<source type>` | No format-selection container |
| Provides alternative text | No | Yes, through `alt` |
| Provides intrinsic dimensions | No | Yes, through `width` and `height` |

Use `<img>` alone for ordinary images and resolution switching.

Use `<picture>` when you need different image formats or different image compositions based on conditions.

# 🗁 Styling Images Inside Picture

The `<picture>` element is a container. Apply image-related styling to the nested `<img>` element.

### 🖆 Basic Styling

```html
<picture class="hero-image">
  <source
    media="(max-width: 600px)"
    srcset="images/hero-mobile.jpg"
  >

  <img
    src="images/hero-desktop.jpg"
    alt="A mountain landscape"
  >
</picture>
```

```css
.hero-image {
  display: block;
  width: 100%;
}

.hero-image img {
  display: block;
  width: 100%;
  height: auto;
  border-radius: 12px;
}
```

### 🖆 Using Object Fit

When the image is displayed inside a fixed-size container, `object-fit` controls how the image fills that space.

```css
.hero-image img {
  width: 100%;
  height: 400px;
  object-fit: cover;
  object-position: center;
}
```

Common `object-fit` values:

| Value | Description |
| --- | --- |
| `fill` | Stretches the image to fill the box. |
| `contain` | Displays the entire image without cropping. |
| `cover` | Fills the box while potentially cropping the image. |
| `none` | Displays the image at its intrinsic size. |
| `scale-down` | Uses the smaller result of `none` or `contain`. |

# 🗁 Image Loading and Performance

The image-loading attributes belong on the `<img>` element inside `<picture>`.

### 🖆 Lazy Loading

Use `loading="lazy"` for images that are not immediately visible.

```html
<picture>
  <source
    srcset="images/article.avif"
    type="image/avif"
  >

  <img
    src="images/article.jpg"
    alt="An article illustration"
    width="800"
    height="500"
    loading="lazy"
  >
</picture>
```

Lazy loading can reduce unnecessary network requests and improve initial page loading performance.

Avoid lazy-loading important above-the-fold images, especially the page's Largest Contentful Paint (LCP) image.

### 🖆 Image Decoding

The `decoding` attribute provides a hint about how the browser should decode the image.

```html
<img
  src="images/photo.jpg"
  alt="A mountain landscape"
  width="1200"
  height="800"
  decoding="async"
>
```

Common values:

* `sync` — requests synchronous decoding.
* `async` — requests asynchronous decoding.
* `auto` — lets the browser decide.

### 🖆 Prioritizing Important Images

For an important hero image, you can use `fetchpriority="high"`.

```html
<picture>
  <source
    srcset="images/hero.avif"
    type="image/avif"
  >

  <img
    src="images/hero.jpg"
    alt="A beautiful mountain landscape"
    width="1600"
    height="900"
    fetchpriority="high"
  >
</picture>
```

Use high priority selectively for important images. Do not assign it to every image on the page.

### 🖆 Preventing Layout Shifts

Specify the intrinsic dimensions on the `<img>` element.

```html
<picture>
  <source srcset="images/banner.webp" type="image/webp">

  <img
    src="images/banner.jpg"
    alt="Website promotional banner"
    width="1200"
    height="400"
  >
</picture>
```

The browser can reserve the appropriate space before the image loads, helping prevent unexpected layout shifts.

When alternative sources use different aspect ratios, account for that difference in your layout and styling.

# 🗁 Accessibility Considerations

The `<picture>` element does not replace the accessibility responsibilities of `<img>`.

### 🖆 Alternative Text

Provide meaningful alternative text on the nested `<img>` element.

```html
<picture>
  <source
    media="(max-width: 600px)"
    srcset="images/product-mobile.jpg"
  >

  <img
    src="images/product-desktop.jpg"
    alt="Black wireless headphones displayed on a desk"
  >
</picture>
```

The alternative text should communicate the essential content or purpose of the selected image.

### 🖆 Decorative Images

If the image is purely decorative, use an empty alternative text.

```html
<picture>
  <source srcset="images/decoration.webp" type="image/webp">

  <img
    src="images/decoration.jpg"
    alt=""
    width="800"
    height="400"
  >
</picture>
```

### 🖆 Preserve Essential Information

When using art direction, make sure every image variation communicates the necessary information.

For example, if a mobile crop removes an important product detail, consider using a different composition that preserves that information.

# 🗁 Best Practices

* Use `<picture>` when you need art direction or image format selection.
* Always include an `<img>` element as the final child.
* Provide a meaningful `alt` attribute on `<img>`.
* Use `srcset` instead of creating unnecessary `<picture>` structures for ordinary resolution switching.
* Put preferred image formats before fallback formats.
* Use the correct MIME type in the `type` attribute.
* Use `media` conditions carefully and order overlapping conditions correctly.
* Specify image dimensions to reduce layout shifts.
* Lazy-load images that are below the fold.
* Prioritize important above-the-fold images appropriately.
* Ensure alternative image compositions preserve essential visual information.
* Optimize image dimensions and file sizes before serving them.

# 🗁 Common Mistakes

### 🖆 Missing `<img>` Element

Incorrect:

```html
<picture>
  <source srcset="images/photo.webp" type="image/webp">
</picture>
```

Correct:

```html
<picture>
  <source srcset="images/photo.webp" type="image/webp">

  <img src="images/photo.jpg" alt="Example photograph">
</picture>
```

### 🖆 Using `src` on `<source>`

Incorrect:

```html
<picture>
  <source src="images/photo.webp" type="image/webp">
  <img src="images/photo.jpg" alt="Example">
</picture>
```

Correct:

```html
<picture>
  <source srcset="images/photo.webp" type="image/webp">
  <img src="images/photo.jpg" alt="Example">
</picture>
```

The `<source>` element inside `<picture>` uses `srcset`.

### 🖆 Adding `alt` to `<source>`

Incorrect:

```html
<picture>
  <source
    srcset="images/photo.webp"
    alt="Example photograph"
  >

  <img src="images/photo.jpg" alt="Example photograph">
</picture>
```

Correct:

```html
<picture>
  <source srcset="images/photo.webp" type="image/webp">

  <img src="images/photo.jpg" alt="Example photograph">
</picture>
```

### 🖆 Incorrect Source Order

When media conditions overlap, a broader condition placed before a more specific condition may match first and prevent the intended source from being selected.

Place the most specific applicable conditions first.

### 🖆 Using Picture for Every Image

The `<picture>` element is not necessary for every responsive image.

For ordinary resolution switching, use `srcset` and `sizes` directly on `<img>`.

# 🗁 Practical Exercises

### 🖆 Exercise: Modern Image Formats

Create a picture element that provides:

* AVIF as the preferred format.
* WebP as the second option.
* JPEG as the fallback.

### 🖆 Exercise: Responsive Art Direction

Create a responsive image component that displays:

* A portrait image on mobile screens.
* A landscape image on desktop screens.

### 🖆 Exercise: Multiple Breakpoints

Create an image component with separate image sources for:

* Small mobile screens.
* Tablets.
* Desktop screens.

### 🖆 Exercise: Accessible Product Image

Create a product image component with:

* Modern image format support.
* Responsive art direction.
* Meaningful alternative text.
* Intrinsic dimensions.
* Appropriate loading behavior.

# 🗁 Interview Questions

**What is the purpose of the `<picture>` element?**

It provides multiple image sources so the browser can select an appropriate image based on format support and media conditions.

**What is the difference between `<picture>` and `<img>`?**

`<picture>` is a container for alternative image sources, while `<img>` displays the selected image and provides its alternative text and fallback source.

**What is the purpose of the `<source>` element?**

It defines alternative image resources using attributes such as `srcset`, `media`, and `type`.

**Why must `<img>` be included inside `<picture>`?**

It provides the actual image element, fallback resource, and alternative text.

**What is art direction in responsive images?**

Art direction means selecting different image compositions or crops for different viewing conditions.

**What is the purpose of the `type` attribute?**

It identifies the image format using a MIME type, allowing the browser to determine whether it can use that source.

**Can `<picture>` be used without `<source>`?**

Yes, but it provides little benefit. An ordinary `<img>` is generally sufficient when there are no alternative sources.

**Where should the `alt` attribute be placed?**

On the nested `<img>` element.

**Should every responsive image use `<picture>`?**

No. For ordinary resolution switching, `srcset` and `sizes` on `<img>` are generally sufficient.

# 🗁 Official References

* [MDN — The Picture Element](https://developer.mozilla.org/en-US/docs/Web/HTML/Reference/Elements/picture)
* [MDN — The Source Element](https://developer.mozilla.org/en-US/docs/Web/HTML/Reference/Elements/source)
* [MDN — Responsive Images](https://developer.mozilla.org/en-US/docs/Web/HTML/Guides/Responsive_images)
* [WHATWG — HTML Living Standard](https://html.spec.whatwg.org/multipage/embedded-content.html#the-picture-element)
