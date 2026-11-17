# Location API

The Location API in the browser is primarily accessed through the `window.location` object.

It provides information about the current document URL and allows JavaScript to navigate to different resources.

The Location API is useful for:

* Reading the current URL.
* Extracting URL components.
* Accessing query strings and fragments.
* Navigating to another page.
* Reloading the current page.
* Redirecting users.
* Updating the current browsing location.

Example:

```javascript
console.log(window.location.href);
```

# 🗁 Understanding the Location Object

The `window.location` property returns a `Location` object representing the current document's URL.

```javascript
console.log(window.location);
```

The `location` object is also available directly:

```javascript
console.log(location);
```

Both expressions refer to the same object in a browser window.

# 🗁 URL Structure

Consider the following URL:

```text
https://example.com:8080/products?id=101&category=books#details
```

Its components are:

| Component | Value |
| --- | --- |
| Protocol | `https:` |
| Hostname | `example.com` |
| Port | `8080` |
| Host | `example.com:8080` |
| Pathname | `/products` |
| Search | `?id=101&category=books` |
| Hash | `#details` |
| Origin | `https://example.com:8080` |

Understanding these components makes it easier to work with URLs programmatically.

# 🗁 Location Properties

The `Location` object exposes several properties for accessing URL components.

**href**

Returns the complete URL.

```javascript
console.log(location.href);
```

It can also navigate to another URL when assigned:

```javascript
location.href = "https://example.com";
```

**protocol**

Returns the URL protocol, including the trailing colon.

```javascript
console.log(location.protocol);
```

Example:

```text
https:
```

**hostname**

Returns the hostname without the port.

```javascript
console.log(location.hostname);
```

Example:

```text
example.com
```

**host**

Returns the hostname and port, if present.

```javascript
console.log(location.host);
```

Example:

```text
example.com:8080
```

**port**

Returns the port number as a string.

```javascript
console.log(location.port);
```

Example:

```text
8080
```

If no explicit port is present, it returns an empty string.

**pathname**

Returns the path portion of the URL.

```javascript
console.log(location.pathname);
```

Example:

```text
/products
```

**search**

Returns the query string, including the leading question mark.

```javascript
console.log(location.search);
```

Example:

```text
?id=101&category=books
```

**hash**

Returns the fragment identifier, including the leading hash.

```javascript
console.log(location.hash);
```

Example:

```text
#details
```

**origin**

Returns the URL origin, consisting of the scheme, hostname, and port.

```javascript
console.log(location.origin);
```

Example:

```text
https://example.com:8080
```

# 🗁 Reading URL Parameters

The `location.search` property provides the query string.

The `URLSearchParams` API makes it easy to read individual parameters.

Example URL:

```text
https://example.com/products?id=101&category=books
```

Read parameters:

```javascript
const params = new URLSearchParams(location.search);

console.log(params.get("id"));
console.log(params.get("category"));
```

Output:

```text
101
books
```

Common `URLSearchParams` methods:

| Method | Description |
| --- | --- |
| `get(name)` | Returns the first value |
| `getAll(name)` | Returns all values |
| `set(name, value)` | Sets a parameter |
| `append(name, value)` | Adds another value |
| `delete(name)` | Removes a parameter |
| `has(name)` | Checks whether a parameter exists |
| `entries()` | Returns key-value pairs |
| `keys()` | Returns parameter names |
| `values()` | Returns parameter values |
| `toString()` | Serializes parameters |

Example:

```javascript
const params = new URLSearchParams(location.search);

console.log(params.has("id"));

params.set("page", "2");

params.delete("category");

console.log(params.toString());
```

# 🗁 Updating Query Parameters

You can update the query string and navigate to the resulting URL.

```javascript
const url = new URL(location.href);

url.searchParams.set("page", "2");

location.href = url.href;
```

This navigates to the updated URL.

To add multiple parameters:

```javascript
const url = new URL(location.href);

url.searchParams.set("category", "books");
url.searchParams.set("sort", "price");

location.href = url.href;
```

To remove a parameter:

```javascript
const url = new URL(location.href);

url.searchParams.delete("category");

location.href = url.href;
```

# 🗁 Modifying the Hash

The `hash` property accesses or changes the fragment identifier.

```javascript
console.log(location.hash);
```

Update the fragment:

```javascript
location.hash = "reviews";
```

The browser navigates to the corresponding fragment within the current document.

This is commonly used for:

* In-page navigation.
* Section links.
* Simple client-side routing.
* Identifying UI states in URLs.

Example:

```javascript
location.hash = "#contact";
```

# 🗁 Navigation with assign()

The `location.assign()` method navigates to a new URL.

```javascript
location.assign("https://example.com/about");
```

The previous page remains in the browser session history, allowing the user to navigate back.

# 🗁 Navigation with replace()

The `location.replace()` method navigates to a new URL without retaining the current document as a separate history entry.

```javascript
location.replace("https://example.com/dashboard");
```

This is useful when the current page should not remain as a normal back-navigation destination.

# 🗁 Reloading the Current Page

The `location.reload()` method reloads the current document.

```javascript
location.reload();
```

It is commonly used after operations that require refreshing the current page.

# 🗁 assign() vs replace() vs href

These approaches can all initiate navigation.

| Approach | Behavior |
| --- | --- |
| `location.href = url` | Navigates to a new URL |
| `location.assign(url)` | Navigates and retains the previous history entry |
| `location.replace(url)` | Navigates without retaining the current entry |
| `location.reload()` | Reloads the current document |

Use `assign()` or `href` for ordinary navigation and `replace()` when replacing the current history entry is intentional.

# 🗁 Creating URLs with the URL Constructor

The `URL` constructor provides a structured way to parse and manipulate URLs.

```javascript
const url = new URL("https://example.com/products?id=101");

console.log(url.hostname);
console.log(url.pathname);
console.log(url.searchParams.get("id"));
```

It can also resolve relative URLs against a base URL.

```javascript
const url = new URL(
  "/products",
  "https://example.com"
);

console.log(url.href);
```

Output:

```text
https://example.com/products
```

This is generally more reliable than manually concatenating URL strings.

# 🗁 URLSearchParams with URL

The `URL` object exposes its query parameters through `searchParams`.

```javascript
const url = new URL("https://example.com/products");

url.searchParams.set("category", "books");
url.searchParams.set("page", "1");

console.log(url.href);
```

Output:

```text
https://example.com/products?category=books&page=1
```

# 🗁 Same-Origin Considerations

A URL's origin is determined by its scheme, hostname, and port.

For example:

```text
https://example.com
https://example.com:443
```

These represent the same origin because HTTPS uses port 443 by default.

However:

```text
http://example.com
https://example.com
```

These have different origins because their schemes differ.

The browser's same-origin policy restricts how documents from different origins interact with each other.

You can inspect the current origin:

```javascript
console.log(location.origin);
```

# 🗁 Practical Example: URL Information

This example displays the current URL components.

```html
<div>
  <h2>Current URL Information</h2>

  <p>Full URL: <span id="href"></span></p>
  <p>Protocol: <span id="protocol"></span></p>
  <p>Hostname: <span id="hostname"></span></p>
  <p>Pathname: <span id="pathname"></span></p>
  <p>Search: <span id="search"></span></p>
  <p>Hash: <span id="hash"></span></p>
  <p>Origin: <span id="origin"></span></p>
</div>

<script>
  const elements = {
    href: document.querySelector("#href"),
    protocol: document.querySelector("#protocol"),
    hostname: document.querySelector("#hostname"),
    pathname: document.querySelector("#pathname"),
    search: document.querySelector("#search"),
    hash: document.querySelector("#hash"),
    origin: document.querySelector("#origin")
  };

  function displayLocation() {
    elements.href.textContent = location.href;
    elements.protocol.textContent = location.protocol;
    elements.hostname.textContent = location.hostname;
    elements.pathname.textContent = location.pathname;
    elements.search.textContent = location.search;
    elements.hash.textContent = location.hash;
    elements.origin.textContent = location.origin;
  }

  displayLocation();
</script>
```

# 🗁 Practical Example: Managing URL Parameters

This example reads, updates, and removes query parameters.

```javascript
const url = new URL(location.href);

// Read parameters
const page = url.searchParams.get("page");

console.log("Current page:", page);

// Update parameters
url.searchParams.set("page", "2");
url.searchParams.set("sort", "newest");

// Add a parameter
url.searchParams.append("tag", "javascript");

// Remove a parameter
url.searchParams.delete("old");

// Display updated URL
console.log(url.href);
```

To navigate to the updated URL:

```javascript
location.assign(url.href);
```

# 🗁 Best Practices

* Use `URL` instead of manually parsing URL strings.
* Use `URLSearchParams` for query string operations.
* Prefer `location.assign()` for ordinary navigation.
* Use `location.replace()` only when replacing the current history entry is intended.
* Avoid manually concatenating untrusted input into URLs.
* Validate and normalize URLs when accepting them from external sources.
* Remember that changing `location.hash` can trigger fragment navigation.
* Do not confuse a URL's origin with its hostname.
* Use `location.origin` when comparing origins.
* Avoid assuming that query parameters are trustworthy or valid application data.

# 🗁 Summary

The Location API provides access to the current document URL and browser navigation.

Important concepts include:

* The `Location` object.
* URL properties such as `href`, `protocol`, `hostname`, and `pathname`.
* Query parameters using `URLSearchParams`.
* Fragment identifiers.
* Navigation with `assign()` and `replace()`.
* Reloading documents.
* URL construction and manipulation.
* Same-origin concepts.

Understanding the Location API is essential for navigation, URL-based application state, and client-side routing.
