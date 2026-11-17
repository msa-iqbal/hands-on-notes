# History API

The History API provides access to the browser session history for the current tab.

It allows JavaScript to navigate through previously visited entries and manipulate the current session's history without necessarily loading a new document.

The History API is especially useful for:

* Single-page applications (SPAs).
* Client-side routing.
* Browser back and forward navigation.
* Updating URLs without page reloads.
* Managing navigation state.
* Supporting deep links and application navigation.

Example:

```javascript
console.log(window.history);
```

# 🗁 Understanding Browser History

When users navigate between pages, browsers maintain a session history containing navigation entries.

For example:

```text
/home
   |
   v
/products
   |
   v
/products/101
   |
   v
/cart
```

The browser provides navigation controls that allow users to move backward and forward through this history.

JavaScript can interact with the session history using the `history` object.

```javascript
console.log(history.length);
```

The `length` property returns the number of entries in the session history, including the current entry.

It does not necessarily reveal the complete navigation history or the URLs of previous entries.

# 🗁 Accessing the History Object

The History API is available through `window.history`.

```javascript
const historyObject = window.history;

console.log(historyObject);
```

You can also access it directly:

```javascript
console.log(history);
```

Both references point to the same History object in a browser window.

# 🗁 History Properties

The History object exposes a small number of properties.

| Property | Description |
| --- | --- |
| `length` | Number of entries in the session history |
| `state` | State associated with the current history entry |
| `scrollRestoration` | Controls automatic scroll restoration behavior |

Example:

```javascript
console.log(history.length);
console.log(history.state);
console.log(history.scrollRestoration);
```

# 🗁 history.back()

The `back()` method navigates to the previous session history entry.

```javascript
history.back();
```

It behaves similarly to clicking the browser's Back button.

If there is no previous entry, the method does nothing.

# 🗁 history.forward()

The `forward()` method navigates to the next session history entry.

```javascript
history.forward();
```

It behaves similarly to clicking the browser's Forward button.

If there is no forward entry, the method does nothing.

# 🗁 history.go()

The `go()` method navigates to a specific position relative to the current history entry.

Syntax:

```javascript
history.go(delta);
```

Examples:

```javascript
history.go(-1); // Previous entry

history.go(1);  // Next entry

history.go(-2); // Two entries backward

history.go(0);  // Reload current page
```

A positive number moves forward, while a negative number moves backward.

# 🗁 back() vs forward() vs go()

| Method | Behavior |
| --- | --- |
| `back()` | Moves one entry backward |
| `forward()` | Moves one entry forward |
| `go(-1)` | Moves one entry backward |
| `go(1)` | Moves one entry forward |
| `go(0)` | Reloads the current document |

These methods initiate navigation rather than returning the destination page.

# 🗁 history.pushState()

The `pushState()` method adds a new entry to the session history without immediately loading a new document.

Syntax:

```javascript
history.pushState(state, unused, url);
```

Parameters:

* `state`: Data associated with the new history entry.
* `unused`: Reserved argument. Pass an empty string.
* `url`: Optional new URL for the history entry.

Example:

```javascript
history.pushState(
  { page: "about" },
  "",
  "/about"
);
```

The browser URL changes to `/about`, but the document is not automatically reloaded.

This is a fundamental mechanism used by client-side routers.

# 🗁 Understanding the State Object

The first argument of `pushState()` stores application-specific state associated with a history entry.

```javascript
history.pushState(
  {
    page: "products",
    productId: 101
  },
  "",
  "/products/101"
);
```

Retrieve the current entry's state:

```javascript
console.log(history.state);
```

Example output:

```javascript
{
  page: "products",
  productId: 101
}
```

The state must be serializable by the browser's structured serialization mechanism. Functions and certain other values cannot be stored.

# 🗁 history.replaceState()

The `replaceState()` method modifies the current session history entry instead of creating a new one.

Syntax:

```javascript
history.replaceState(state, unused, url);
```

Example:

```javascript
history.replaceState(
  { page: "dashboard" },
  "",
  "/dashboard"
);
```

The current history entry is updated without adding another entry.

This is useful when correcting or updating the current URL without creating an additional Back-button destination.

# 🗁 pushState() vs replaceState()

| Feature | pushState() | replaceState() |
| --- | --- | --- |
| Creates a new history entry | Yes | No |
| Replaces the current entry | No | Yes |
| Changes the URL | Optional | Optional |
| Reloads the document | No | No |
| Triggers `popstate` immediately | No | No |

Use `pushState()` when a new navigable application state is created.

Use `replaceState()` when updating the existing state without adding another navigation step.

# 🗁 The popstate Event

The `popstate` event fires when the active history entry changes through browser history traversal.

For example, it can fire when the user:

* Clicks the Back button.
* Clicks the Forward button.
* Calls `history.back()`.
* Calls `history.forward()`.
* Calls `history.go()` to traverse history.

Example:

```javascript
window.addEventListener("popstate", (event) => {
  console.log("History state changed:", event.state);
});
```

The event's `state` property contains the state associated with the activated history entry.

Important: Calling `pushState()` or `replaceState()` does not immediately trigger a `popstate` event.

# 🗁 Handling Back and Forward Navigation

A single-page application can listen for `popstate` to update its interface when users navigate through browser history.

```javascript
window.addEventListener("popstate", (event) => {
  const state = event.state;

  if (state) {
    console.log("Current page:", state.page);
  }
});
```

The application must update its displayed content to match the active URL and history state.

# 🗁 The hashchange Event

The `hashchange` event fires when the fragment identifier of the URL changes.

Example:

```javascript
window.addEventListener("hashchange", () => {
  console.log("Current hash:", location.hash);
});
```

It can be triggered by:

```javascript
location.hash = "#about";
```

However, changing the URL through `pushState()` or `replaceState()` does not trigger `hashchange`, even when the fragment changes.

# 🗁 popstate vs hashchange

| Feature | popstate | hashchange |
| --- | --- | --- |
| Trigger | History traversal to another entry | Fragment identifier changes |
| Back and Forward navigation | Yes, when traversing entries | Yes, when fragment navigation occurs |
| `pushState()` | Does not trigger immediately | Does not trigger |
| `replaceState()` | Does not trigger immediately | Does not trigger |
| Event data | Includes history state | Includes old and new URLs |

These events represent different aspects of browser navigation.

# 🗁 Managing Scroll Restoration

The `scrollRestoration` property controls how the browser restores the scroll position during history navigation.

Available values:

* `auto`: Browser handles scroll restoration automatically.
* `manual`: Application controls scroll restoration.

Example:

```javascript
history.scrollRestoration = "manual";
```

Restore automatic behavior:

```javascript
history.scrollRestoration = "auto";
```

This is useful when implementing custom page transitions or client-side routing systems.

# 🗁 Same-Origin Restrictions

The History API restricts which URLs can be used with `pushState()` and `replaceState()`.

The new URL must be same-origin with the current document.

For example, if the current document is:

```text
https://example.com
```

This is allowed:

```javascript
history.pushState({}, "", "/products");
```

This is also allowed:

```javascript
history.pushState(
  {},
  "",
  "https://example.com/about"
);
```

But this is not allowed:

```javascript
history.pushState(
  {},
  "",
  "https://another-example.com"
);
```

Attempting to use a different origin throws a `SecurityError`.

# 🗁 History API and Single-Page Applications

Single-page applications often use the History API to implement navigation without reloading the entire document.

A typical workflow:

* User clicks a navigation link.
* JavaScript prevents normal navigation.
* The application updates the URL using `pushState()`.
* JavaScript renders the corresponding page content.
* Browser Back and Forward navigation is handled through `popstate`.

Example:

```html
<nav>
  <a href="/" data-link>Home</a>
  <a href="/about" data-link>About</a>
  <a href="/contact" data-link>Contact</a>
</nav>

<main id="app"></main>

<script>
  const app = document.querySelector("#app");

  const pages = {
    "/": "Welcome to the Home page",
    "/about": "About our application",
    "/contact": "Contact us"
  };

  function render(path) {
    app.textContent = pages[path] ?? "Page not found";
  }

  function navigate(path) {
    if (!Object.hasOwn(pages, path)) {
      return;
    }

    history.pushState({ path }, "", path);

    render(path);
  }

  document.addEventListener("click", (event) => {
    const link = event.target.closest("a[data-link]");

    if (!link || event.button !== 0) {
      return;
    }

    if (
      event.metaKey ||
      event.ctrlKey ||
      event.shiftKey ||
      event.altKey
    ) {
      return;
    }

    const url = new URL(link.href);

    if (url.origin !== location.origin) {
      return;
    }

    event.preventDefault();

    navigate(url.pathname);
  });

  window.addEventListener("popstate", () => {
    render(location.pathname);
  });

  render(location.pathname);
</script>
```

This is a simplified client-side router.

A production router should also handle query parameters, fragments, route parameters, loading states, accessibility, and server-side fallback configuration.

# 🗁 Practical Example: Simple History Navigation

This example demonstrates navigation between application views without reloading the document.

```html
<nav>
  <button data-page="home">Home</button>
  <button data-page="about">About</button>
  <button data-page="contact">Contact</button>
</nav>

<h2 id="pageTitle"></h2>

<script>
  const pageTitle = document.querySelector("#pageTitle");

  function renderPage(page) {
    pageTitle.textContent = page;
  }

  function navigate(page) {
    history.pushState(
      { page },
      "",
      `/${page}`
    );

    renderPage(page);
  }

  document.querySelector("nav").addEventListener("click", (event) => {
    const button = event.target.closest("button[data-page]");

    if (!button) {
      return;
    }

    navigate(button.dataset.page);
  });

  window.addEventListener("popstate", (event) => {
    renderPage(event.state?.page ?? "home");
  });

  renderPage(history.state?.page ?? "home");
</script>
```

# 🗁 Best Practices

* Use `pushState()` for new application navigation states.
* Use `replaceState()` when updating the current history entry.
* Listen for `popstate` to handle Back and Forward navigation.
* Keep the URL and rendered application state synchronized.
* Store only necessary, serializable data in history state.
* Do not assume `pushState()` automatically loads or renders a page.
* Use `URL` and `URLSearchParams` to construct and manage URLs.
* Respect modified link clicks and normal browser navigation behavior.
* Configure the server to serve the application entry point for supported client-side routes.
* Avoid creating excessive history entries for minor UI changes.
* Consider scroll restoration when implementing custom routing.

# 🗁 Summary

The History API allows JavaScript to interact with the browser's session history.

Important concepts include:

* `history.length` and `history.state`.
* `back()`, `forward()`, and `go()`.
* `pushState()` and `replaceState()`.
* The `popstate` event.
* The `hashchange` event.
* Scroll restoration.
* Same-origin restrictions.
* Client-side routing.

Understanding the History API is essential for building single-page applications and implementing browser navigation without full-page reloads.
