# ARIA Basics

ARIA stands for **Accessible Rich Internet Applications**. It is a collection of attributes that improves the accessibility of web content, particularly dynamic interfaces and custom interactive components.

ARIA helps developers communicate information about an element to assistive technologies, such as screen readers.

HTML already provides many accessible semantic elements. However, complex interfaces sometimes require additional accessibility information that native HTML cannot express directly.

ARIA provides attributes for:

* Defining element roles.
* Providing accessible names and descriptions.
* Communicating element states.
* Communicating element properties.
* Identifying relationships between elements.
* Improving accessibility in dynamic interfaces.

ARIA does not automatically make an element interactive, keyboard accessible, or visually accessible. Developers must still implement the required behavior.

# 🗁 What Is ARIA?

ARIA is a W3C specification that defines additional attributes for making web content and applications more accessible.

It provides semantic information that browsers expose through accessibility APIs.

ARIA attributes typically use the `aria-` prefix.

Example:

```html
<button
  type="button"
  aria-label="Close dialog"
>
  X
</button>
```

The visible button contains the character `X`, but the `aria-label` attribute provides a more meaningful accessible name.

A screen reader can announce the button as:

```text
Close dialog, button
```

### 🖆 ARIA Attribute Syntax

```html
<element
  role="role-name"
  aria-attribute="value"
>
  Content
</element>
```

Examples:

```html
<div role="alert">
  Your changes could not be saved.
</div>
```

```html
<button
  type="button"
  aria-expanded="false"
  aria-controls="menu"
>
  Menu
</button>
```

```html
<input
  type="text"
  aria-required="true"
  aria-describedby="help-text"
>
```

ARIA adds accessibility semantics, but it does not replace the underlying HTML structure or application behavior.

# 🗁 Why ARIA Is Important

Modern web applications contain interactive components that may not have direct native HTML equivalents.

Examples include:

* Custom dropdown menus.
* Expandable navigation menus.
* Tabs.
* Dialogs.
* Custom autocomplete controls.
* Dynamic notifications.
* Interactive tree views.
* Custom data grids.

ARIA helps communicate the semantics and state of these components to assistive technologies.

### 🖆 Communicating Element Roles

A role identifies what an element represents.

```html
<div role="alert">
  Your session is about to expire.
</div>
```

The `alert` role communicates that the content is an important notification.

### 🖆 Communicating Element States

ARIA can communicate whether an interactive element is expanded, selected, checked, disabled, or otherwise in a particular state.

```html
<button
  type="button"
  aria-expanded="true"
>
  Account Menu
</button>
```

### 🖆 Communicating Relationships

ARIA can establish relationships between elements.

```html
<button
  aria-controls="navigation-menu"
  aria-expanded="false"
>
  Navigation
</button>

<nav id="navigation-menu" hidden>
  <a href="/">Home</a>
  <a href="/about">About</a>
</nav>
```

The button identifies the element it controls.

# 🗁 How ARIA Works

Browsers expose HTML and ARIA semantics through accessibility APIs.

Assistive technologies use these APIs to understand and communicate information about the interface.

A simplified process looks like this:

```text
HTML + ARIA
     |
     v
Browser
     |
     v
Accessibility Tree
     |
     v
Operating System Accessibility API
     |
     v
Screen Reader
     |
     v
User
```

### 🖆 Accessibility Tree

The accessibility tree is a representation of interface elements and their accessibility information.

It can include:

* Accessible roles.
* Accessible names.
* Accessible descriptions.
* States and properties.
* Relationships between elements.

For example:

```html
<button aria-label="Open settings">
  ⚙
</button>
```

The accessibility information may be represented conceptually as:

```text
Role: button
Name: Open settings
```

The accessibility tree is related to the DOM, but it is not identical to the DOM. Some elements may be ignored or combined when the browser creates its accessibility representation.

# 🗁 The Three Main ARIA Concepts

ARIA is commonly understood through three categories:

* Roles.
* States.
* Properties.

### 🖆 Roles

A role communicates what an element is.

Examples:

* `button`
* `navigation`
* `dialog`
* `alert`
* `tab`
* `tablist`
* `checkbox`

```html
<div role="button">
  Save
</div>
```

This declares button semantics, but it does not provide native button behavior.

### 🖆 States

States communicate the current condition of an element.

Examples:

* `aria-expanded`
* `aria-checked`
* `aria-selected`
* `aria-pressed`
* `aria-disabled`

```html
<button
  type="button"
  aria-pressed="true"
>
  Bold
</button>
```

The button communicates that its pressed state is active.

### 🖆 Properties

Properties provide additional information or relationships about an element.

Examples:

* `aria-label`
* `aria-labelledby`
* `aria-describedby`
* `aria-controls`
* `aria-haspopup`

```html
<button
  aria-label="Open navigation"
  aria-controls="main-navigation"
>
  Menu
</button>
```

Some ARIA attributes can communicate either states or properties depending on their definition. The distinction is useful conceptually, but developers should follow the specific semantics of each attribute.

# 🗁 ARIA Roles

A role identifies the semantic purpose of an element.

ARIA defines several role categories.

### 🖆 Landmark Roles

Landmark roles identify important page regions.

| Role | Purpose |
| --- | --- |
| `banner` | Site-wide header |
| `navigation` | Navigation region |
| `main` | Main page content |
| `complementary` | Supporting content |
| `contentinfo` | Site-wide footer |
| `search` | Search region |
| `form` | Important form region |
| `region` | Important named region |

Example:

```html
<nav aria-label="Primary">
  <a href="/">Home</a>
  <a href="/about">About</a>
</nav>
```

Native `<nav>` already provides the navigation landmark, so an explicit `role="navigation"` is unnecessary.

### 🖆 Widget Roles

Widget roles describe interactive interface components.

| Role | Purpose |
| --- | --- |
| `button` | Button control |
| `checkbox` | Checkbox control |
| `radio` | Radio button |
| `switch` | On/off control |
| `tab` | Tab control |
| `tablist` | Collection of tabs |
| `tabpanel` | Tab content |
| `dialog` | Dialog window |
| `menu` | Application-style menu |
| `slider` | Range control |

Example:

```html
<div
  role="checkbox"
  aria-checked="false"
  tabindex="0"
>
  Accept Terms
</div>
```

This example declares checkbox semantics, but the developer must also implement keyboard interaction, focus behavior, and state changes.

A native checkbox is usually a better choice:

```html
<label>
  <input type="checkbox" name="terms">
  Accept Terms
</label>
```

### 🖆 Document Structure Roles

Some roles describe structural content.

Examples include:

* `article`
* `list`
* `listitem`
* `heading`
* `table`
* `row`
* `cell`

Prefer native HTML elements when they provide the required semantics.

# 🗁 ARIA States and Properties

ARIA attributes communicate information about an element's current state or additional properties.

### 🖆 `aria-expanded`

Indicates whether a collapsible component is expanded or collapsed.

```html
<button
  type="button"
  aria-expanded="false"
  aria-controls="faq-answer"
>
  What is HTML?
</button>

<div id="faq-answer" hidden>
  HTML is the standard markup language for web documents.
</div>
```

When the answer becomes visible, update the state:

```html
<button
  type="button"
  aria-expanded="true"
  aria-controls="faq-answer"
>
  What is HTML?
</button>

<div id="faq-answer">
  HTML is the standard markup language for web documents.
</div>
```

The ARIA state must remain synchronized with the actual interface state.

### 🖆 `aria-checked`

Communicates whether a checkbox, radio button, or supported checkable widget is checked.

```html
<div
  role="checkbox"
  aria-checked="true"
>
  Accept Terms
</div>
```

### 🖆 `aria-selected`

Communicates whether an item in a selectable composite widget is selected.

```html
<div role="tablist" aria-label="Account settings">
  <button
    type="button"
    role="tab"
    aria-selected="true"
  >
    Profile
  </button>

  <button
    type="button"
    role="tab"
    aria-selected="false"
  >
    Security
  </button>
</div>
```

A complete tab interface also requires appropriate keyboard behavior, focus management, and relationships between tabs and tab panels.

### 🖆 `aria-pressed`

Communicates the pressed state of a toggle button.

```html
<button
  type="button"
  aria-pressed="false"
>
  Bold
</button>
```

### 🖆 `aria-disabled`

Communicates that an element is unavailable for interaction.

```html
<button
  type="button"
  aria-disabled="true"
>
  Save
</button>
```

Unlike the native `disabled` attribute on a button, `aria-disabled="true"` does not automatically prevent interaction.

The application must enforce the disabled behavior.

### 🖆 `aria-required`

Communicates that a control is required.

```html
<input
  type="email"
  aria-required="true"
>
```

For native form controls, prefer the HTML `required` attribute:

```html
<input
  type="email"
  name="email"
  required
>
```

### 🖆 `aria-invalid`

Indicates that the current value does not satisfy the expected input requirements.

```html
<label for="email">Email Address</label>

<input
  type="email"
  id="email"
  name="email"
  aria-invalid="true"
  aria-describedby="email-error"
>

<p id="email-error">
  Enter a valid email address.
</p>
```

Only set `aria-invalid="true"` when the field is actually invalid.

# 🗁 Accessible Names

An accessible name identifies an element to assistive technologies.

For example, a button might have visible text, an associated label, or an ARIA-provided name.

### 🖆 Using Visible Text

```html
<button type="button">
  Save Changes
</button>
```

The visible text provides the accessible name.

### 🖆 Using `aria-label`

```html
<button
  type="button"
  aria-label="Close dialog"
>
  X
</button>
```

The `aria-label` attribute provides an accessible name.

### 🖆 Using `aria-labelledby`

```html
<h2 id="dialog-heading">
  Delete Account
</h2>

<div
  role="dialog"
  aria-labelledby="dialog-heading"
>
  <p>Are you sure you want to delete your account?</p>
</div>
```

The dialog's accessible name comes from the referenced heading.

### 🖆 Choosing a Naming Technique

| Technique | Usage |
| --- | --- |
| Visible text | Preferred for buttons and many controls |
| `<label>` | Preferred for native form controls |
| `aria-labelledby` | References existing visible text |
| `aria-label` | Provides a name when visible text is unavailable |

Whenever possible, use visible text because it benefits both sighted users and assistive technology users.

# 🗁 Accessible Descriptions

An accessible description provides additional information beyond an element's name.

The `aria-describedby` attribute connects an element to one or more descriptive elements.

```html
<label for="password">
  Password
</label>

<input
  type="password"
  id="password"
  name="password"
  aria-describedby="password-help"
>

<p id="password-help">
  Use at least 12 characters, including letters and numbers.
</p>
```

The label identifies the control, while the description provides additional instructions.

### 🖆 Multiple Description Elements

An element can reference multiple IDs.

```html
<input
  type="password"
  id="password"
  aria-describedby="password-help password-error"
>

<p id="password-help">
  Use at least 12 characters.
</p>

<p id="password-error">
  Your password is too short.
</p>
```

Both referenced elements contribute descriptive information.

# 🗁 Common ARIA Attributes

The following table summarizes frequently used ARIA attributes.

| Attribute | Purpose |
| --- | --- |
| `aria-label` | Provides an accessible name |
| `aria-labelledby` | References elements that provide a name |
| `aria-describedby` | References descriptive information |
| `aria-controls` | Identifies elements controlled by an element |
| `aria-expanded` | Indicates expanded or collapsed state |
| `aria-hidden` | Excludes content from the accessibility tree |
| `aria-live` | Identifies a live region |
| `aria-atomic` | Controls whether the whole live region is announced |
| `aria-relevant` | Identifies relevant live-region changes |
| `aria-checked` | Communicates checked state |
| `aria-selected` | Communicates selection state |
| `aria-pressed` | Communicates toggle-button state |
| `aria-disabled` | Communicates disabled state |
| `aria-required` | Communicates required state |
| `aria-invalid` | Communicates invalid input state |
| `aria-haspopup` | Indicates that an element opens a popup |
| `aria-current` | Identifies the current item in a related set |
| `aria-modal` | Indicates modal dialog behavior |

Not every attribute is appropriate for every role. Always follow the ARIA specification for supported attributes.

# 🗁 ARIA for Interactive Components

ARIA is particularly useful for custom interactive components.

However, ARIA communicates semantics; JavaScript must implement the interaction.

### 🖆 Expandable Content

```html
<button
  type="button"
  aria-expanded="false"
  aria-controls="details"
>
  Show Details
</button>

<div id="details" hidden>
  Additional information appears here.
</div>
```

When the button is activated, JavaScript should:

* Update `aria-expanded`.
* Show or hide the controlled content.
* Preserve appropriate keyboard behavior.

### 🖆 Toggle Button

```html
<button
  type="button"
  aria-pressed="false"
>
  Mute
</button>
```

When toggled, update `aria-pressed` to reflect the actual state.

### 🖆 Dialog

```html
<div
  role="dialog"
  aria-labelledby="dialog-title"
  aria-describedby="dialog-description"
  aria-modal="true"
>
  <h2 id="dialog-title">Confirm Action</h2>

  <p id="dialog-description">
    Are you sure you want to continue?
  </p>

  <button type="button">Cancel</button>
  <button type="button">Confirm</button>
</div>
```

This communicates dialog semantics, but a complete modal dialog also requires:

* Moving focus into the dialog when opened.
* Keeping keyboard focus within the modal while it is open.
* Supporting an appropriate close mechanism.
* Returning focus to the invoking control when closed.
* Preventing interaction with background content when modal behavior requires it.

For native modal dialogs, consider the HTML `<dialog>` element and its `showModal()` method.

### 🖆 Tabs

A tab interface uses several related ARIA roles:

```html
<div role="tablist" aria-label="Account settings">
  <button
    type="button"
    role="tab"
    id="profile-tab"
    aria-selected="true"
    aria-controls="profile-panel"
    tabindex="0"
  >
    Profile
  </button>

  <button
    type="button"
    role="tab"
    id="security-tab"
    aria-selected="false"
    aria-controls="security-panel"
    tabindex="-1"
  >
    Security
  </button>
</div>

<section
  role="tabpanel"
  id="profile-panel"
  aria-labelledby="profile-tab"
>
  Profile settings appear here.
</section>

<section
  role="tabpanel"
  id="security-panel"
  aria-labelledby="security-tab"
  hidden
>
  Security settings appear here.
</section>
```

A complete tab implementation also needs keyboard support, including arrow-key navigation, focus management, selection updates, and panel visibility.

# 🗁 ARIA Live Regions

Live regions communicate dynamic content changes to assistive technologies without requiring users to move focus to the updated content.

They are useful for:

* Form submission messages.
* Validation feedback.
* Loading status updates.
* Shopping cart notifications.
* Background operation results.
* Network connection status.

### 🖆 `aria-live="polite"`

A polite live region generally waits for an appropriate opportunity before announcing an update.

```html
<div aria-live="polite" id="status">
  Ready.
</div>
```

When the content changes, assistive technologies can announce the update.

### 🖆 `aria-live="assertive"`

An assertive live region requests a higher-priority announcement.

```html
<div aria-live="assertive" id="error-message">
  A critical error occurred.
</div>
```

Use assertive announcements sparingly because they can interrupt other speech.

### 🖆 Using `role="status"`

The `status` role provides a polite live region by default.

```html
<div role="status" id="save-status">
  Changes saved successfully.
</div>
```

### 🖆 Using `role="alert"`

The `alert` role is intended for important, time-sensitive information.

```html
<div role="alert">
  Your session has expired.
</div>
```

### 🖆 Additional Live Region Attributes

| Attribute | Purpose |
| --- | --- |
| `aria-live` | Defines announcement priority |
| `aria-atomic` | Determines whether the entire region is announced |
| `aria-relevant` | Identifies which changes should be announced |
| `aria-busy` | Indicates that a region is being updated |

Example:

```html
<div
  aria-live="polite"
  aria-atomic="true"
  id="notification"
>
  Your profile has been updated.
</div>
```

`aria-atomic="true"` requests that the entire region be announced when relevant changes occur.

# 🗁 ARIA Hidden

The `aria-hidden` attribute controls whether an element is exposed in the accessibility tree.

### 🖆 Hiding an Element from Assistive Technologies

```html
<span aria-hidden="true">
  ★
</span>

<span>Five-star rating</span>
```

The decorative star is hidden from assistive technologies because the meaningful information is already available as text.

### 🖆 Important Difference Between `aria-hidden` and `hidden`

| Attribute | Behavior |
| --- | --- |
| `hidden` | Hides content from normal rendering and the accessibility tree |
| `aria-hidden="true"` | Hides content from the accessibility tree but does not normally hide it visually |
| `display: none` | Removes content from visual rendering and normally from the accessibility tree |
| `visibility: hidden` | Hides content visually and normally from the accessibility tree |

### 🖆 Important Rule

Do not apply `aria-hidden="true"` to an element containing interactive controls that users still need to access.

Avoid hiding focusable elements from assistive technologies while leaving them keyboard accessible.

For example, this is problematic:

```html
<div aria-hidden="true">
  <button type="button">
    Submit
  </button>
</div>
```

The button may remain keyboard focusable while being hidden from the accessibility tree.

# 🗁 ARIA and Native HTML

Native HTML elements should be the first choice for accessible interfaces.

### 🖆 Prefer Native Buttons

Avoid:

```html
<div role="button" tabindex="0">
  Save
</div>
```

Prefer:

```html
<button type="button">
  Save
</button>
```

A native button already supports:

* Keyboard focus.
* Enter and Space activation.
* Button semantics.
* Disabled behavior through the `disabled` attribute.
* Standard browser interaction.

### 🖆 Prefer Native Checkboxes

Avoid:

```html
<div
  role="checkbox"
  aria-checked="false"
  tabindex="0"
>
  Subscribe
</div>
```

Prefer:

```html
<label>
  <input type="checkbox" name="subscribe">
  Subscribe
</label>
```

### 🖆 Prefer Native Navigation

Avoid:

```html
<div role="navigation">
  <a href="/">Home</a>
</div>
```

Prefer:

```html
<nav aria-label="Primary">
  <a href="/">Home</a>
</nav>
```

Native HTML provides built-in semantics and behavior, reducing the amount of custom accessibility code required.

# 🗁 The First Rule of ARIA

The first rule of ARIA is commonly summarized as:

**If you can use a native HTML element or attribute with the semantics and behavior you require, do so instead of recreating it with ARIA.**

For example:

```html
<!-- Avoid -->
<div role="button" tabindex="0">
  Submit
</div>

<!-- Prefer -->
<button type="submit">
  Submit
</button>
```

The native button is more accessible by default and requires less custom JavaScript.

ARIA should enhance accessibility when native HTML cannot express the required semantics.

# 🗁 The Five Rules of ARIA

The W3C ARIA Authoring Practices describe five important rules for using ARIA.

### 🖆 Use Native HTML First

Prefer native elements and attributes when they provide the required semantics and behavior.

### 🖆 Do Not Change Native Semantics Unnecessarily

Avoid overriding the role of an element when its native semantics are already appropriate.

For example, do not change a heading into a button simply because it needs special styling.

### 🖆 Make All Interactive ARIA Controls Keyboard Accessible

If an element is interactive, users must be able to operate it with an appropriate keyboard interaction model.

Adding `role="button"` does not automatically provide keyboard support.

### 🖆 Do Not Hide Focusable Elements from Assistive Technologies

Avoid using `aria-hidden="true"` on elements that remain keyboard accessible.

### 🖆 Give Interactive Elements Accessible Names

Every interactive control should have a meaningful accessible name.

For example:

```html
<button type="button" aria-label="Close dialog">
  X
</button>
```

# 🗁 ARIA and Keyboard Accessibility

ARIA does not implement keyboard behavior.

Developers must ensure that custom interactive components support appropriate keyboard interactions.

### 🖆 Native Controls

Native HTML controls already provide standard keyboard behavior.

For example:

```html
<button type="button">
  Submit
</button>
```

A native button supports keyboard activation without custom event handling.

### 🖆 Custom Controls

A custom widget may require:

* `tabindex` management.
* Enter and Space activation.
* Arrow-key navigation.
* Escape-key handling.
* Focus movement.
* Focus restoration.
* Appropriate state updates.

The required behavior depends on the component's interaction pattern.

### 🖆 Example: Custom Button

```html
<div
  role="button"
  tabindex="0"
  aria-label="Save changes"
>
  Save
</div>
```

This element has button semantics and can receive keyboard focus, but it still requires JavaScript to support activation using Enter and Space.

A native `<button>` is preferable.

# 🗁 Common ARIA Mistakes

### 🖆 Using ARIA Instead of Native HTML

Incorrect:

```html
<div role="button">
  Submit
</div>
```

Better:

```html
<button type="submit">
  Submit
</button>
```

### 🖆 Adding Redundant Roles

Incorrect:

```html
<nav role="navigation">
  ...
</nav>
```

Better:

```html
<nav>
  ...
</nav>
```

The native `<nav>` element already provides navigation semantics.

### 🖆 Using `aria-label` on Everything

Do not add `aria-label` to every element.

Visible text and native labels are often sufficient.

### 🖆 Incorrectly Managing ARIA States

Incorrect:

```html
<button aria-expanded="false">
  Menu
</button>

<nav>
  ...
</nav>
```

If the navigation is visible, the declared state is inaccurate.

Keep ARIA state values synchronized with the actual interface.

### 🖆 Using `aria-hidden` on Interactive Content

Incorrect:

```html
<div aria-hidden="true">
  <a href="/account">Account</a>
</div>
```

If the link remains available to keyboard users, hiding it from assistive technologies creates an inconsistent experience.

### 🖆 Assuming ARIA Provides Functionality

Incorrect:

```html
<div
  role="button"
  aria-pressed="false"
>
  Toggle
</div>
```

These attributes communicate semantics and state, but they do not implement activation or state changes.

### 🖆 Using Incorrect Attribute Values

ARIA attributes have defined value types.

For example:

```html
<!-- Incorrect -->
<button aria-expanded="yes">
  Menu
</button>

<!-- Correct -->
<button aria-expanded="true">
  Menu
</button>
```

Follow the allowed values defined by the ARIA specification.

# 🗁 Testing ARIA

ARIA should be tested to ensure that accessibility information is accurate and useful.

### 🖆 Browser Accessibility Inspector

Use browser developer tools to inspect:

* Computed roles.
* Accessible names.
* Accessible descriptions.
* States and properties.
* Relationships between elements.

### 🖆 Screen Reader Testing

Test with commonly used screen readers:

* NVDA.
* JAWS.
* VoiceOver.
* TalkBack.

Verify that controls are announced correctly and that dynamic updates are communicated.

### 🖆 Keyboard Testing

Navigate the entire interface using only a keyboard.

Check:

* Tab navigation.
* Shift + Tab navigation.
* Enter and Space activation.
* Arrow-key interaction where appropriate.
* Escape behavior for dialogs and popup interfaces.
* Visible focus indicators.

### 🖆 Automated Accessibility Tools

Useful tools include:

* axe DevTools.
* WAVE.
* Lighthouse.
* Accessibility Insights.

Automated testing can detect many technical issues, but it cannot fully determine whether an interface is understandable or usable.

Manual testing is essential.

# 🗁 Best Practices

* Prefer native HTML elements over custom ARIA implementations.
* Use ARIA only when native HTML cannot express the required semantics.
* Provide meaningful accessible names for interactive controls.
* Use `aria-labelledby` when existing visible text can name an element.
* Use `aria-describedby` for additional instructions.
* Keep ARIA states synchronized with the actual interface state.
* Use live regions for appropriate dynamic announcements.
* Avoid unnecessary ARIA attributes and redundant roles.
* Do not hide focusable content from assistive technologies.
* Implement keyboard behavior for custom interactive components.
* Follow established ARIA interaction patterns.
* Test with screen readers and keyboard navigation.
* Validate accessibility using automated and manual testing.

# 🗁 Practical Exercises

### 🖆 Exercise: Add Accessible Names

Create a toolbar containing:

* A search button.
* A settings button.
* A close button.
* A notification button.

Give every icon-only button a meaningful accessible name.

### 🖆 Exercise: Build an Expandable Menu

Create a navigation menu that:

* Opens when activated.
* Closes when activated again.
* Updates `aria-expanded`.
* Uses `aria-controls`.
* Supports keyboard interaction.
* Keeps the ARIA state synchronized with visibility.

### 🖆 Exercise: Create a Live Notification

Create a notification area that announces:

* Successful form submission.
* Failed form submission.
* Background operation completion.

Use appropriate live-region semantics.

### 🖆 Exercise: Improve Form Accessibility

Create a form with:

* Required fields.
* Validation messages.
* Accessible labels.
* Additional instructions.
* Invalid field states.

Use `aria-describedby` and `aria-invalid` appropriately.

### 🖆 Exercise: Audit ARIA Usage

Inspect an existing webpage and identify:

* Redundant ARIA roles.
* Missing accessible names.
* Incorrect ARIA states.
* Incorrect `aria-hidden` usage.
* Custom controls without keyboard support.
* Dynamic content without appropriate announcements.

Correct the identified issues.

# 🗁 Interview Questions

**What does ARIA stand for?**

Accessible Rich Internet Applications.

**What is the purpose of ARIA?**

ARIA provides additional semantic information to assistive technologies, particularly for dynamic interfaces and custom interactive components.

**What are the three main ARIA concepts?**

Roles, states, and properties.

**What is an ARIA role?**

A role identifies the semantic purpose of an element, such as a button, dialog, navigation region, or checkbox.

**What is the difference between `aria-label` and `aria-labelledby`?**

`aria-label` directly provides an accessible name, while `aria-labelledby` references another element whose content provides that name.

**What is the purpose of `aria-describedby`?**

It associates additional descriptive information with an element, such as instructions or validation messages.

**What does `aria-expanded` do?**

It communicates whether a collapsible component is expanded or collapsed.

**What is the purpose of `aria-live`?**

It identifies a region where dynamic content changes can be announced by assistive technologies.

**What is the difference between `aria-hidden` and `hidden`?**

`aria-hidden="true"` hides content from the accessibility tree while normally leaving it visually rendered. The HTML `hidden` attribute hides content from normal rendering and the accessibility tree.

**Does ARIA make a custom element keyboard accessible?**

No. ARIA communicates semantics, but developers must implement keyboard interaction and focus behavior.

**Why should native HTML be preferred over ARIA?**

Native HTML provides built-in semantics and behavior, reducing custom implementation complexity and accessibility errors.

**What is the first rule of ARIA?**

If an appropriate native HTML element or attribute already provides the required semantics and behavior, use it instead of recreating it with ARIA.

**Can ARIA replace semantic HTML?**

No. ARIA supplements HTML semantics and does not replace proper document structure or native interaction behavior.

# 🗁 References

* [W3C — WAI-ARIA 1.2 Specification](https://www.w3.org/TR/wai-aria-1.2/)
* [W3C — ARIA Authoring Practices Guide](https://www.w3.org/WAI/ARIA/apg/)
* [W3C — ARIA Authoring Practices: Read Me First](https://www.w3.org/WAI/ARIA/apg/practices/read-me-first/)
* [W3C — ARIA Practices: Keyboard Interface](https://www.w3.org/WAI/ARIA/apg/practices/keyboard-interface/)
* [MDN — ARIA](https://developer.mozilla.org/en-US/docs/Web/Accessibility/ARIA)
* [MDN — ARIA Attributes](https://developer.mozilla.org/en-US/docs/Web/Accessibility/ARIA/Attributes)
