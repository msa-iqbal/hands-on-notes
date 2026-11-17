# Clipboard API

The Clipboard API allows web applications to read data from and write data to the system clipboard.

The clipboard is temporary storage used for operations such as:

* Copying text.
* Pasting text.
* Copying URLs.
* Copying code snippets.
* Copying formatted content.
* Reading copied content.

The Clipboard API provides a modern, Promise-based interface for clipboard operations.

Example:

```javascript
await navigator.clipboard.writeText("Hello, World!");
```

This copies text to the system clipboard.

# 🗁 Understanding the Clipboard API

The Clipboard API is available through:

```javascript
navigator.clipboard
```

It provides methods for reading and writing clipboard data.

Important methods:

| Method | Description |
| --- | --- |
| `writeText()` | Writes plain text to the clipboard |
| `readText()` | Reads plain text from the clipboard |
| `write()` | Writes one or more clipboard items |
| `read()` | Reads clipboard items |

The modern Clipboard API is asynchronous and generally requires a secure context.

# 🗁 Browser Support and Security Requirements

Before using the Clipboard API, check whether it is supported.

```javascript
if (navigator.clipboard) {
  console.log("Clipboard API is supported");
} else {
  console.log("Clipboard API is not supported");
}
```

Important considerations:

* The API generally requires HTTPS.
* Localhost is generally treated as a trustworthy development origin.
* Clipboard access may require user interaction.
* Browser permission policies can restrict clipboard operations.
* Embedded applications may need appropriate iframe permissions.
* Clipboard behavior differs between browsers and operating systems.

# 🗁 Writing Text to the Clipboard

The `writeText()` method copies plain text to the clipboard.

Syntax:

```javascript
navigator.clipboard.writeText(text);
```

It returns a Promise.

Example:

```javascript
async function copyText() {
  try {
    await navigator.clipboard.writeText("Hello, World!");

    console.log("Text copied successfully");
  } catch (error) {
    console.error("Copy failed:", error);
  }
}

copyText();
```

The Promise resolves when the clipboard write operation completes.

# 🗁 Reading Text from the Clipboard

The `readText()` method retrieves plain text from the clipboard.

Syntax:

```javascript
navigator.clipboard.readText();
```

Example:

```javascript
async function readClipboard() {
  try {
    const text = await navigator.clipboard.readText();

    console.log("Clipboard content:", text);
  } catch (error) {
    console.error("Unable to read clipboard:", error);
  }
}

readClipboard();
```

Reading clipboard content may require permission or a recent user interaction, depending on the browser.

# 🗁 Copying Text with a Button

Clipboard operations are commonly triggered by user interactions.

HTML:

```html
<button id="copyButton">
  Copy Text
</button>

<p id="status"></p>
```

JavaScript:

```javascript
const copyButton = document.querySelector("#copyButton");
const status = document.querySelector("#status");

copyButton.addEventListener("click", async () => {
  try {
    await navigator.clipboard.writeText("Copied from JavaScript!");

    status.textContent = "Copied successfully!";
  } catch (error) {
    status.textContent = "Unable to copy text.";

    console.error(error);
  }
});
```

This example copies text when the user clicks the button.

# 🗁 Reading Clipboard Content with a Button

A user-triggered action can also read clipboard content.

HTML:

```html
<button id="pasteButton">
  Paste Text
</button>

<p id="output"></p>
```

JavaScript:

```javascript
const pasteButton = document.querySelector("#pasteButton");
const output = document.querySelector("#output");

pasteButton.addEventListener("click", async () => {
  try {
    const text = await navigator.clipboard.readText();

    output.textContent = text;
  } catch (error) {
    output.textContent = "Unable to read clipboard.";

    console.error(error);
  }
});
```

Using `textContent` helps prevent clipboard text from being interpreted as HTML.

# 🗁 Writing Multiple Clipboard Formats

The `write()` method supports writing clipboard items in different formats.

The `ClipboardItem` interface represents clipboard data with one or more MIME types.

Example:

```javascript
async function copyMultipleFormats() {
  const text = "Hello, Clipboard!";

  const html = "<strong>Hello, Clipboard!</strong>";

  const clipboardItem = new ClipboardItem({
    "text/plain": new Blob([text], {
      type: "text/plain"
    }),

    "text/html": new Blob([html], {
      type: "text/html"
    })
  });

  await navigator.clipboard.write([clipboardItem]);

  console.log("Multiple formats copied");
}
```

The clipboard contains both plain-text and HTML representations.

Applications that paste the content can use an appropriate format.

# 🗁 Reading Multiple Clipboard Formats

The `read()` method retrieves clipboard items.

Example:

```javascript
async function inspectClipboard() {
  try {
    const items = await navigator.clipboard.read();

    for (const item of items) {
      console.log("Available types:", item.types);

      for (const type of item.types) {
        const blob = await item.getType(type);

        console.log("Type:", type);
        console.log("Size:", blob.size);
      }
    }
  } catch (error) {
    console.error("Clipboard read failed:", error);
  }
}
```

Each clipboard item exposes its available MIME types through the `types` property.

The `getType()` method retrieves the corresponding data as a `Blob`.

# 🗁 Copying Images to the Clipboard

The Clipboard API can also write image data.

For example, an application can copy a PNG image.

```javascript
async function copyImage() {
  const response = await fetch("/images/logo.png");

  const blob = await response.blob();

  const clipboardItem = new ClipboardItem({
    [blob.type]: blob
  });

  await navigator.clipboard.write([clipboardItem]);

  console.log("Image copied");
}
```

The browser must support the image format and clipboard operation.

PNG is a commonly supported image format for clipboard writing.

# 🗁 Clipboard Events

The Clipboard API is different from clipboard events.

Clipboard events allow applications to respond to copy, cut, and paste operations.

Important events:

| Event | Description |
| --- | --- |
| `copy` | Triggered when content is copied |
| `cut` | Triggered when content is cut |
| `paste` | Triggered when content is pasted |

Example:

```javascript
document.addEventListener("copy", () => {
  console.log("Copy operation detected");
});

document.addEventListener("cut", () => {
  console.log("Cut operation detected");
});

document.addEventListener("paste", () => {
  console.log("Paste operation detected");
});
```

These events can be used to customize clipboard behavior.

# 🗁 Handling Copy Events

The `copy` event provides access to the clipboard data through `event.clipboardData`.

Example:

```javascript
document.addEventListener("copy", (event) => {
  event.clipboardData.setData(
    "text/plain",
    "Custom copied text"
  );

  event.preventDefault();
});
```

Calling `preventDefault()` prevents the browser's default copy behavior so the application can provide custom clipboard content.

# 🗁 Handling Cut Events

The `cut` event can customize the content placed on the clipboard when a cut operation occurs.

Example:

```javascript
document.addEventListener("cut", (event) => {
  event.clipboardData.setData(
    "text/plain",
    "Custom cut content"
  );

  event.preventDefault();
});
```

A complete cut implementation must also handle removing the selected content from the document when appropriate.

# 🗁 Handling Paste Events

The `paste` event provides access to clipboard data.

Example:

```javascript
document.addEventListener("paste", (event) => {
  const text = event.clipboardData.getData("text/plain");

  console.log("Pasted text:", text);
});
```

The `getData()` method retrieves data using a specified MIME type.

For example:

```javascript
const plainText = event.clipboardData.getData("text/plain");

const htmlContent = event.clipboardData.getData("text/html");
```

Clipboard event data is available through `event.clipboardData`, not through `navigator.clipboard`.

# 🗁 Clipboard API vs Clipboard Events

Both mechanisms interact with the clipboard, but their purposes differ.

| Feature | Clipboard API | Clipboard Events |
| --- | --- | --- |
| Access | `navigator.clipboard` | DOM events |
| Main purpose | Programmatic clipboard access | Responding to copy, cut, and paste |
| Operations | Read and write | Customize or observe clipboard actions |
| Asynchronous | Yes, Promise-based | Event-based |
| Common methods | `readText()`, `writeText()` | `getData()`, `setData()` |

Use the Clipboard API for explicit application actions such as a Copy button.

Use clipboard events when customizing or handling user-initiated copy, cut, and paste operations.

# 🗁 Clipboard Permissions

Clipboard access is controlled by browser security policies.

The Permissions API can be used to inspect clipboard permissions in browsers that support the relevant permission names.

Example:

```javascript
async function checkClipboardPermission() {
  try {
    const permission = await navigator.permissions.query({
      name: "clipboard-read"
    });

    console.log("Clipboard read permission:", permission.state);
  } catch (error) {
    console.log("Permission query is not supported");
  }
}
```

Possible permission states include:

* `granted`
* `denied`
* `prompt`

Permission behavior varies between browsers. A successful permission query does not guarantee that every clipboard operation will succeed.

# 🗁 Handling Errors

Clipboard operations can fail for several reasons:

* The browser does not support the requested operation.
* The page is not running in a secure context.
* Permission has not been granted.
* The operation was not initiated through an acceptable user interaction.
* The clipboard data format is unsupported.
* Browser or operating system restrictions prevent access.

Use `try...catch` to handle failures.

Example:

```javascript
async function copyToClipboard(text) {
  try {
    await navigator.clipboard.writeText(text);

    return {
      success: true,
      message: "Text copied successfully"
    };
  } catch (error) {
    return {
      success: false,
      message: "Unable to access clipboard",
      error
    };
  }
}
```

# 🗁 Practical Example: Copy-to-Clipboard Utility

A reusable utility function can simplify clipboard operations.

```javascript
async function copyToClipboard(text) {
  if (!navigator.clipboard) {
    throw new Error("Clipboard API is unavailable");
  }

  await navigator.clipboard.writeText(text);

  return true;
}
```

Usage:

```javascript
const button = document.querySelector("#copyButton");

button.addEventListener("click", async () => {
  try {
    await copyToClipboard("https://example.com");

    console.log("Link copied!");
  } catch (error) {
    console.error(error.message);
  }
});
```

This approach separates clipboard functionality from UI event handling.

# 🗁 Legacy Clipboard Approach

Older applications sometimes use `document.execCommand()` to perform clipboard operations.

Example:

```javascript
function legacyCopy(text) {
  const textarea = document.createElement("textarea");

  textarea.value = text;

  document.body.appendChild(textarea);

  textarea.select();

  document.execCommand("copy");

  textarea.remove();
}
```

`document.execCommand()` is deprecated and should not be used for new implementations.

Prefer the modern Clipboard API.

A legacy fallback may still be relevant when supporting older environments, but it should be implemented and tested separately.

# 🗁 Security Considerations

Clipboard content can contain untrusted data.

Important practices:

* Never assume clipboard content is safe.
* Avoid inserting pasted HTML directly into the DOM.
* Prefer `textContent` for displaying plain text.
* Validate and sanitize data before processing it.
* Request clipboard access only when necessary.
* Do not read clipboard content unexpectedly.
* Avoid copying sensitive information without clear user intent.
* Handle permission failures gracefully.

Clipboard access should be transparent and controlled by meaningful user actions.

# 🗁 Best Practices

* Prefer `navigator.clipboard` for modern clipboard operations.
* Use `writeText()` for plain-text copying.
* Use `readText()` when plain-text input is required.
* Use `read()` and `write()` for multiple MIME types.
* Trigger clipboard operations from user interactions.
* Check browser support before using clipboard methods.
* Handle Promise rejections.
* Use clipboard events for customized copy, cut, and paste behavior.
* Treat pasted content as untrusted input.
* Avoid relying on deprecated `document.execCommand()`.

# 🗁 Summary

The Clipboard API provides modern access to the system clipboard.

Important concepts include:

* `navigator.clipboard`.
* `writeText()` and `readText()`.
* `write()` and `read()`.
* `ClipboardItem` and MIME types.
* Copying image data.
* Copy, cut, and paste events.
* `event.clipboardData`.
* Clipboard permissions and secure contexts.
* Error handling and security.
* Legacy clipboard methods.

The Clipboard API is useful for building developer tools, editors, forms, dashboards, and applications that support convenient copy-and-paste workflows.
