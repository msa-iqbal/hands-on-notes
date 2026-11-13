# HTML Alternative Text (Alt Text)

Alternative text, commonly called **alt text**, is a textual description provided through the `alt` attribute of an HTML `<img>` element.

Alt text communicates the meaning or purpose of an image to users who cannot see it.

It is particularly important for:

* Screen reader users.
* People with visual impairments.
* Users browsing with images disabled.
* Situations where an image fails to load.
* Assistive technologies that need to understand image content.

Good alt text describes an image according to its context and purpose. It does not necessarily describe every visual detail.

The correct alternative text depends on why the image is included in the webpage.

# 🗁 What Is Alt Text?

Alt text is a textual alternative that describes the purpose or meaning of an image.

It is provided through the `alt` attribute of the `<img>` element.

```html
<img
  src="developer.jpg"
  alt="A developer working on a laptop at a desk"
>
```

When a screen reader encounters this image, it can communicate the alternative text to the user.

Alt text also provides a fallback when the image cannot be displayed.

### 🖆 Basic Syntax

```html
<img src="image.jpg" alt="Description of the image">
```

### 🖆 Important Attributes

| Attribute | Purpose |
| --- | --- |
| `src` | Specifies the image resource |
| `alt` | Provides alternative text |
| `width` | Specifies the image width |
| `height` | Specifies the image height |
| `loading` | Controls loading behavior |
| `decoding` | Provides a hint for image decoding |

The `alt` attribute is essential for accessible image content.

# 🗁 The alt Attribute

The `alt` attribute provides a text alternative for an image.

### 🖆 Image with Alt Text

```html
<img
  src="company-logo.png"
  alt="Example Company"
>
```

### 🖆 Image with Empty Alt Text

```html
<img
  src="decorative-line.svg"
  alt=""
>
```

An empty `alt` attribute indicates that the image is decorative and should generally be ignored by screen readers.

### 🖆 Image Without an Alt Attribute

```html
<img src="company-logo.png">
```

This is different from an empty alt attribute.

When `alt` is omitted, assistive technologies may use the image filename or another fallback to identify it. This can produce an unhelpful experience.

Always provide an appropriate `alt` attribute, even when its value is empty.

# 🗁 Why Alt Text Is Important

### 🖆 Accessibility

Alt text allows screen reader users to understand meaningful image content.

Without alternative text, important visual information may be unavailable to users who cannot see the image.

### 🖆 Better User Experience

If an image fails to load, the browser can display its alternative text.

This helps users understand the intended content.

### 🖆 Search Engine Understanding

Search engines can use alternative text as one signal to understand image content and its relationship to the surrounding page.

### 🖆 Content Availability

Alt text provides a textual alternative that can communicate information independently of the original image.

# 🗁 Types of Images

The correct alt text depends on the role an image plays in a webpage.

Common categories include:

| Image type | Purpose | Alt text approach |
| --- | --- | --- |
| Informative | Communicates information | Describe the relevant information |
| Decorative | Adds visual appearance | Use `alt=""` |
| Functional | Performs an action | Describe the action or destination |
| Text image | Contains meaningful text | Provide the same essential text |
| Complex | Communicates detailed information | Provide a concise alternative and a detailed equivalent |
| Linked image | Acts as a link | Describe the link's purpose |

Understanding these categories helps developers write meaningful alternatives.

# 🗁 Informative Images

Informative images communicate important information that users need to understand the content.

Examples include:

* Product photographs.
* Educational illustrations.
* Diagrams.
* Charts.
* Screenshots containing meaningful information.
* Photographs used to explain a concept.

### 🖆 Example

```html
<img
  src="solar-system.jpg"
  alt="The eight planets arranged in order from the Sun"
>
```

The alternative text communicates the important information represented by the image.

### 🖆 Product Image

```html
<img
  src="laptop.jpg"
  alt="Silver laptop with a 15-inch display and black keyboard"
>
```

Describe relevant characteristics rather than unnecessary visual details.

### 🖆 Educational Image

```html
<img
  src="html-structure.png"
  alt="HTML document structure showing the head and body elements inside the html element"
>
```

The description should communicate the image's educational purpose.

# 🗁 Decorative Images

Decorative images do not provide meaningful information beyond the surrounding content.

Examples include:

* Decorative patterns.
* Visual separators.
* Background ornaments.
* Decorative shapes.
* Icons that repeat nearby text without adding information.

Use an empty `alt` attribute for decorative images.

```html
<img
  src="decorative-pattern.svg"
  alt=""
>
```

This tells assistive technologies that the image does not need to be announced.

### 🖆 Decorative Icon

```html
<button type="button">
  <img src="arrow-right.svg" alt="">
  Continue
</button>
```

The visible button text already communicates the action, so the icon does not need a separate description.

### 🖆 Important Distinction

An image is not decorative simply because it looks attractive.

If it communicates information or performs a function, it needs an appropriate text alternative.

# 🗁 Functional Images

Functional images perform an action or communicate the purpose of an interactive element.

Common examples include:

* Search icons.
* Navigation icons.
* Social media links.
* Image-based buttons.
* Linked logos.

The alternative text should describe the function rather than simply the visual appearance.

### 🖆 Search Button

```html
<button type="submit">
  <img src="search.svg" alt="">
  Search
</button>
```

The button already has the visible text "Search", so the image can have empty alt text.

### 🖆 Icon-Only Search Button

```html
<button type="submit">
  <img src="search.svg" alt="Search">
</button>
```

Here the image provides the button's accessible name.

### 🖆 Linked Company Logo

```html
<a href="/">
  <img
    src="company-logo.png"
    alt="Example Company home"
  >
</a>
```

The alternative text communicates the destination or purpose of the link.

### 🖆 Important Principle

For functional images, describe what happens when the user activates the element.

Do not describe only its appearance.

# 🗁 Images of Text

Some images contain meaningful text.

Examples include:

* Promotional banners.
* Screenshots containing instructions.
* Images containing quotations.
* Graphics with embedded labels.

When the text is important and cannot be provided elsewhere, the alternative text should communicate the essential textual information.

### 🖆 Example

```html
<img
  src="promotion.png"
  alt="Summer Sale: 30 percent off selected products"
>
```

### 🖆 Prefer Real HTML Text

Whenever possible, use actual HTML text instead of embedding important text inside images.

Better:

```html
<div class="promotion">
  <h2>Summer Sale</h2>
  <p>Save 30 percent on selected products.</p>
</div>
```

Actual text is easier to resize, translate, search, and access.

# 🗁 Complex Images

Complex images communicate detailed information that cannot be adequately described in a short alt text.

Examples include:

* Charts.
* Graphs.
* Technical diagrams.
* Network architecture diagrams.
* Scientific illustrations.
* Detailed infographics.

For complex images, provide a concise alternative and a detailed textual explanation elsewhere.

### 🖆 Example: Network Diagram

```html
<figure>
  <img
    src="network-topology.png"
    alt="Network topology diagram showing a router connected to three switches"
    aria-describedby="network-description"
  >

  <figcaption id="network-description">
    The router connects to the core switch.
    The core switch connects to three access switches.
    Each access switch provides connectivity to client devices.
  </figcaption>
</figure>
```

The `alt` attribute provides a concise summary, while the accompanying description communicates additional information.

### 🖆 Example: Sales Chart

```html
<figure>
  <img
    src="sales-chart.png"
    alt="Bar chart showing monthly sales increasing from January to June"
    aria-describedby="sales-description"
  >

  <figcaption id="sales-description">
    Sales increased each month:
    January: $10,000.
    February: $12,000.
    March: $15,000.
    April: $18,000.
    May: $22,000.
    June: $27,000.
  </figcaption>
</figure>
```

For complex data visualizations, provide the underlying data in an accessible table whenever appropriate.

# 🗁 Images Inside Links

When an image is the only content inside a link, its alternative text should communicate the purpose or destination of the link.

### 🖆 Example

```html
<a href="/products">
  <img
    src="products-banner.jpg"
    alt="Explore our products"
  >
</a>
```

The alternative text becomes the accessible name of the link.

### 🖆 Image with Additional Link Text

```html
<a href="/products">
  <img
    src="products.jpg"
    alt=""
  >

  View Products
</a>
```

The visible link text already communicates the purpose, so the image can have empty alt text.

Avoid repeating the same information unnecessarily.

# 🗁 Background Images and CSS

CSS background images are commonly used for decoration and visual presentation.

```html
<div class="hero">
  <h1>Welcome to Our Website</h1>
</div>
```

```css
.hero {
  background-image: url("hero-background.jpg");
  background-size: cover;
  background-position: center;
}
```

CSS background images do not have an HTML `alt` attribute.

If a background image communicates meaningful information, that information must be provided through actual HTML content or another accessible text alternative.

For example:

```html
<div class="hero">
  <h1>Mountain Adventure Tours</h1>

  <p>
    Explore guided hiking experiences across the mountains.
  </p>
</div>
```

The heading and paragraph communicate the essential information independently of the background image.

Use CSS background images primarily for decorative content.

# 🗁 Writing Effective Alt Text

Good alt text communicates the information or function users need from an image.

### 🖆 Describe the Relevant Information

Weak:

```html
<img src="network.png" alt="Image">
```

Better:

```html
<img
  src="network.png"
  alt="Router connected to three network switches"
>
```

### 🖆 Keep It Concise

Avoid writing unnecessarily long descriptions inside the `alt` attribute.

Weak:

```html
<img
  src="laptop.jpg"
  alt="This is a very beautiful silver laptop that has a black keyboard and is placed on a wooden desk in a room with several other objects around it"
>
```

Better:

```html
<img
  src="laptop.jpg"
  alt="Silver laptop with a black keyboard on a wooden desk"
>
```

Include only details relevant to the image's purpose.

### 🖆 Describe Context, Not Just Appearance

Consider the same photograph used in two different situations.

For a photography article:

```html
<img
  src="mountain.jpg"
  alt="Snow-covered mountain under a cloudy sky"
>
```

For a travel advertisement:

```html
<img
  src="mountain.jpg"
  alt="Explore guided hiking tours in the mountains"
>
```

The image is identical, but its purpose differs.

### 🖆 Avoid Unnecessary Phrases

Screen readers generally identify images as images, so phrases such as "Image of" or "Picture of" are usually unnecessary.

Prefer:

```html
<img
  src="dog.jpg"
  alt="Golden retriever playing in a garden"
>
```

However, identifying the type of visual content can be useful when it contributes meaningful context.

### 🖆 Use Empty Alt Text for Decorative Images

```html
<img src="decoration.svg" alt="">
```

Do not omit the `alt` attribute.

### 🖆 Avoid Keyword Stuffing

Alt text should describe the image naturally.

Incorrect:

```html
<img
  src="laptop.jpg"
  alt="Laptop computer best laptop cheap laptop gaming laptop laptop"
>
```

Better:

```html
<img
  src="laptop.jpg"
  alt="Black gaming laptop with a backlit keyboard"
>
```

# 🗁 Alt Text and SEO

Alternative text can help search engines understand image content.

It also provides context about how an image relates to surrounding content.

### 🖆 SEO Best Practices

* Describe the image accurately.
* Use relevant words naturally.
* Keep descriptions concise.
* Avoid keyword stuffing.
* Match the alternative text to the image's context.
* Use meaningful filenames when appropriate.
* Provide actual HTML text for important information.

Example:

```html
<img
  src="ethernet-network-cable.jpg"
  alt="Ethernet network cable connected to a router LAN port"
>
```

Alt text should primarily serve accessibility. It should not be written solely to manipulate search rankings.

# 🗁 Common Mistakes

### 🖆 Missing Alt Attribute

Incorrect:

```html
<img src="profile.jpg">
```

Better:

```html
<img
  src="profile.jpg"
  alt="Profile photograph of the account owner"
>
```

For decorative images, use `alt=""`.

### 🖆 Using Generic Descriptions

Incorrect:

```html
<img src="diagram.png" alt="Diagram">
```

Better:

```html
<img
  src="diagram.png"
  alt="Client devices connected to a router through a network switch"
>
```

### 🖆 Repeating Nearby Text

Incorrect:

```html
<a href="/contact">
  <img src="contact-icon.svg" alt="Contact">
  Contact
</a>
```

Better:

```html
<a href="/contact">
  <img src="contact-icon.svg" alt="">
  Contact
</a>
```

The visible link text already communicates the purpose.

### 🖆 Using Filenames as Alt Text

Incorrect:

```html
<img src="img_12345_final.png" alt="img_12345_final">
```

Better:

```html
<img
  src="img_12345_final.png"
  alt="Company office building entrance"
>
```

### 🖆 Writing Excessively Long Alt Text

Avoid placing an entire article or complex explanation inside the `alt` attribute.

Use a concise alternative and provide detailed information through nearby HTML content.

### 🖆 Using Alt Text for Decorative Images

Incorrect:

```html
<img
  src="decorative-shape.svg"
  alt="Blue decorative curved shape"
>
```

Better:

```html
<img src="decorative-shape.svg" alt="">
```

If the image is purely decorative, it should not create unnecessary announcements.

# 🗁 Testing Alt Text

Testing helps ensure that images have appropriate text alternatives.

### 🖆 Inspect HTML

Review image elements and check their `alt` attributes.

```html
<img src="logo.svg" alt="Example Company">
<img src="decoration.svg" alt="">
<img src="network.png" alt="Router connected to two switches">
```

### 🖆 Screen Reader Testing

Use screen readers to verify how images are announced.

Common tools include:

* NVDA on Windows.
* JAWS on Windows.
* VoiceOver on macOS and iOS.
* TalkBack on Android.

Check whether:

* Informative images communicate their meaning.
* Decorative images are skipped.
* Functional images communicate their actions.
* Linked images provide meaningful link names.
* Complex images have accessible detailed descriptions.

### 🖆 Automated Testing Tools

Useful tools include:

* axe DevTools.
* WAVE.
* Lighthouse.
* Browser accessibility inspectors.

Automated tools can detect missing `alt` attributes, but they cannot reliably determine whether the description is meaningful or appropriate.

Manual review is necessary.

# 🗁 Best Practices

* Always include the `alt` attribute on `<img>` elements.
* Describe informative images according to their purpose.
* Use `alt=""` for purely decorative images.
* Describe the function of interactive images.
* Provide meaningful alternatives for linked images.
* Keep alt text concise and relevant.
* Avoid unnecessary phrases such as "Image of".
* Do not use filenames as descriptions.
* Avoid keyword stuffing.
* Do not use placeholders or surrounding text as a substitute for required image alternatives.
* Provide detailed descriptions for complex images.
* Prefer real HTML text over images containing important text.
* Keep CSS background images decorative whenever possible.
* Test alternative text with screen readers.

# 🗁 Practical Exercises

### 🖆 Exercise: Add Alt Text to Informative Images

Create a webpage containing:

* A company logo.
* A product photograph.
* A network topology diagram.
* A profile photograph.

Write appropriate alternative text for each image.

### 🖆 Exercise: Identify Decorative Images

Create a webpage with:

* A decorative background pattern.
* A decorative separator.
* A decorative icon beside visible text.
* An informative photograph.

Use appropriate `alt` values for each image.

### 🖆 Exercise: Improve Functional Images

Create:

* An image-only search button.
* A linked company logo.
* An icon-only navigation button.
* A social media link containing an icon.

Ensure each interactive element has a meaningful accessible name.

### 🖆 Exercise: Describe a Complex Diagram

Create a network topology diagram showing:

* An internet connection.
* A router.
* A core switch.
* Two access switches.
* Several client devices.

Write concise alt text and provide a detailed description using HTML content.

### 🖆 Exercise: Audit an Existing Website

Inspect a webpage and identify:

* Images without an `alt` attribute.
* Decorative images with unnecessary descriptions.
* Informative images with weak descriptions.
* Functional images without accessible names.
* Complex images without detailed alternatives.

Correct the identified problems.

# 🗁 Interview Questions

**What is alt text in HTML?**

Alt text is an alternative textual description provided through the `alt` attribute of an `<img>` element.

**Why is alt text important?**

It makes meaningful image information available to users who cannot see images, particularly people using screen readers.

**Which HTML attribute provides alternative text?**

The `alt` attribute.

**What is the difference between `alt=""` and omitting the `alt` attribute?**

`alt=""` explicitly identifies an image as decorative, while omitting the attribute leaves the image without an author-provided text alternative.

**When should you use empty alt text?**

When an image is purely decorative or its information is already adequately provided by nearby content.

**What should alt text describe for functional images?**

It should describe the action, purpose, or destination of the interactive element.

**Should every image have descriptive alt text?**

Every `<img>` should have an `alt` attribute, but its value can be empty for decorative images.

**Can CSS background images have alt text?**

No. CSS background images do not support the HTML `alt` attribute. Meaningful information must be provided through HTML content or another accessible text alternative.

**What is the difference between informative and decorative images?**

Informative images communicate meaningful content, while decorative images provide visual presentation without adding essential information.

**How should developers handle complex images?**

Provide concise alt text and an additional detailed description, such as nearby explanatory text or an accessible data table.

**Does alt text improve SEO?**

It can help search engines understand image content, but its primary purpose is accessibility.

**Can automated tools determine whether alt text is meaningful?**

Automated tools can identify missing attributes and some technical problems, but human review is needed to evaluate whether the description accurately communicates the image's purpose.

# 🗁 References

* [MDN — `<img>` Element](https://developer.mozilla.org/en-US/docs/Web/HTML/Reference/Elements/img)
* [W3C WAI — Images Tutorial](https://www.w3.org/WAI/tutorials/images/)
* [W3C WAI — An Alt Decision Tree](https://www.w3.org/WAI/tutorials/images/decision-tree/)
* [W3C WAI — Functional Images](https://www.w3.org/WAI/tutorials/images/functional/)
* [W3C WAI — Complex Images](https://www.w3.org/WAI/tutorials/images/complex/)
