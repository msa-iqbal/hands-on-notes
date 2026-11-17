# URL API

The URL API provides JavaScript interfaces for creating, parsing, modifying, and working with URLs.

It allows developers to manipulate URLs without manually splitting strings or handling complex encoding rules.

Common use cases include:

* Reading the current page URL.
* Extracting query parameters.
* Updating query parameters.
* Building API request URLs.
* Managing URL paths and fragments.
* Validating and resolving URLs.
* Creating temporary URLs for files and binary data.

The URL API primarily consists of:

* `URL`: Represents and manipulates a URL.
* `URLSearchParams`: Reads and modifies URL query parameters.
* `URL.canParse()`: Checks whether a URL can be parsed.
* `URL.createObjectURL()`: Creates a temporary URL for an object.
* `URL.revokeObjectURL()`: Releases a previously created object URL.

# 🗁 Understanding URL Structure

A URL (Uniform Resource Locator) identifies a resource and describes how to access it.

Consider this URL:

```text
https://example.com:8080/products/101?category=electronics&sort=price#details
```

Its components are:

| Component | Value |
| --- | --- |
| Protocol | `https:` |
| Hostname | `example.com` |
| Port | `8080` |
| Host | `example.com:8080` |
| Pathname | `/products/101` |
| Search | `?category=electronics&sort=price` |
| Hash | `#details` |
| Origin | `https://example.com:8080` |

Understanding these components makes URL manipulation easier.

# 🗁 Creating a URL Object

The `URL` constructor creates a URL object from a URL string.

Syntax:

```javascript
const url = new URL(urlString);
```

Example:

```javascript
const url = new URL(
  "https://example.com/products/101?category=electronics"
);

console.log(url);
```

The constructor parses the URL and exposes its individual components through properties.

# 🗁 Using Relative URLs

The URL constructor also supports relative URLs when a valid base URL is provided.

Syntax:

```javascript
new URL(relativeURL, baseURL);
```

Example:

```javascript
const url = new URL(
  "/products/101",
  "https://example.com"
);

console.log(url.href);
```

Output:

```text
https://example.com/products/101
```

Another example:

```javascript
const url = new URL(
  "../images/photo.jpg",
  "https://example.com/products/item/"
);

console.log(url.href);
```

Output:

```text
https://example.com/products/images/photo.jpg
```

The browser resolves the relative URL against the base URL.

# 🗁 URL Properties

A URL object provides several properties for accessing URL components.

| Property | Description |
| --- | --- |
| `href` | Complete serialized URL |
| `origin` | Protocol, hostname, and port |
| `protocol` | URL scheme, including the colon |
| `username` | Username component |
| `password` | Password component |
| `host` | Hostname and port |
| `hostname` | Hostname only |
| `port` | Port number |
| `pathname` | Path component |
| `search` | Query string, including `?` |
| `hash` | Fragment identifier, including `#` |

Example:

```javascript
const url = new URL(
  "https://example.com:8080/products/101?category=books#details"
);

console.log(url.href);
console.log(url.origin);
console.log(url.protocol);
console.log(url.hostname);
console.log(url.port);
console.log(url.pathname);
console.log(url.search);
console.log(url.hash);
```

# 🗁 href

The `href` property returns or updates the complete serialized URL.

Example:

```javascript
const url = new URL("https://example.com");

console.log(url.href);
```

Update the URL:

```javascript
url.href = "https://example.com/about";

console.log(url.href);
```

# 🗁 origin

The `origin` property returns the URL's origin.

An origin consists of:

* Scheme.
* Hostname.
* Port, when non-default.

Example:

```javascript
const url = new URL(
  "https://example.com:8080/products"
);

console.log(url.origin);
```

Output:

```text
https://example.com:8080
```

The `origin` property is read-only.

# 🗁 protocol

The `protocol` property returns the URL scheme.

Example:

```javascript
const url = new URL("https://example.com");

console.log(url.protocol);
```

Output:

```text
https:
```

Update the protocol:

```javascript
url.protocol = "http:";

console.log(url.href);
```

# 🗁 hostname and host

The `hostname` property returns the hostname without the port.

The `host` property returns the hostname together with the port, if present.

Example:

```javascript
const url = new URL(
  "https://example.com:8080/products"
);

console.log(url.hostname);
console.log(url.host);
```

Output:

```text
example.com
example.com:8080
```

# 🗁 port

The `port` property returns the port number as a string.

Example:

```javascript
const url = new URL(
  "https://example.com:8080"
);

console.log(url.port);
```

Output:

```text
8080
```

Setting the port:

```javascript
url.port = "3000";

console.log(url.href);
```

Default ports are generally omitted from the serialized URL.

For example, the default HTTPS port `443` does not need to appear in the resulting URL.

# 🗁 pathname

The `pathname` property represents the URL path.

Example:

```javascript
const url = new URL(
  "https://example.com/products/101"
);

console.log(url.pathname);
```

Output:

```text
/products/101
```

Update the pathname:

```javascript
url.pathname = "/users/101";

console.log(url.href);
```

# 🗁 search

The `search` property returns the query string, including the leading question mark.

Example:

```javascript
const url = new URL(
  "https://example.com/products?category=books"
);

console.log(url.search);
```

Output:

```text
?category=books
```

Update the query string:

```javascript
url.search = "?category=electronics&sort=price";

console.log(url.href);
```

For more structured query manipulation, use `URLSearchParams`.

# 🗁 hash

The `hash` property represents the URL fragment identifier, including the leading `#`.

Example:

```javascript
const url = new URL(
  "https://example.com/docs#installation"
);

console.log(url.hash);
```

Output:

```text
#installation
```

Update the fragment:

```javascript
url.hash = "examples";

console.log(url.href);
```

# 🗁 username and password

The `username` and `password` properties represent user information embedded in a URL.

Example:

```javascript
const url = new URL(
  "https://user:secret@example.com"
);

console.log(url.username);
console.log(url.password);
```

These URL components are generally discouraged for application authentication because URLs can be exposed through logs, browser history, and other mechanisms.

# 🗁 URLSearchParams

`URLSearchParams` provides methods for working with URL query parameters.

It allows developers to:

* Read query parameters.
* Add parameters.
* Update existing parameters.
* Remove parameters.
* Iterate through parameters.
* Serialize query strings.

Example:

```javascript
const params = new URLSearchParams(
  "name=Iqbal&role=developer"
);

console.log(params.get("name"));
console.log(params.get("role"));
```

Output:

```text
Iqbal
developer
```

# 🗁 Creating URLSearchParams

You can create a `URLSearchParams` object from a query string, an iterable of key-value pairs, or a plain object.

Example using a query string:

```javascript
const params = new URLSearchParams(
  "page=1&limit=10"
);
```

Example using an object:

```javascript
const params = new URLSearchParams({
  page: "1",
  limit: "10"
});
```

Example using key-value pairs:

```javascript
const params = new URLSearchParams([
  ["page", "1"],
  ["limit", "10"]
]);
```

# 🗁 get()

The `get()` method returns the first value associated with a parameter.

```javascript
const params = new URLSearchParams(
  "name=Iqbal&role=developer"
);

console.log(params.get("name"));
```

If the parameter does not exist, it returns `null`.

# 🗁 getAll()

The `getAll()` method returns all values associated with a parameter.

```javascript
const params = new URLSearchParams(
  "tag=javascript&tag=typescript&tag=react"
);

console.log(params.getAll("tag"));
```

Output:

```javascript
["javascript", "typescript", "react"]
```

This is useful when query parameters have repeated names.

# 🗁 set()

The `set()` method adds a parameter or replaces all existing values for that parameter with one value.

```javascript
const params = new URLSearchParams();

params.set("page", "1");

params.set("limit", "20");

console.log(params.toString());
```

Output:

```text
page=1&limit=20
```

If the parameter already exists multiple times, `set()` replaces its values with a single value.

# 🗁 append()

The `append()` method adds another value for a parameter.

```javascript
const params = new URLSearchParams();

params.append("tag", "javascript");

params.append("tag", "typescript");

console.log(params.toString());
```

Output:

```text
tag=javascript&tag=typescript
```

Unlike `set()`, `append()` preserves existing values.

# 🗁 has()

The `has()` method checks whether a parameter exists.

```javascript
const params = new URLSearchParams(
  "page=1&limit=10"
);

console.log(params.has("page"));
console.log(params.has("sort"));
```

Output:

```text
true
false
```

# 🗁 delete()

The `delete()` method removes a parameter and all its associated values.

```javascript
const params = new URLSearchParams(
  "page=1&limit=10&sort=price"
);

params.delete("sort");

console.log(params.toString());
```

Output:

```text
page=1&limit=10
```

# 🗁 toString()

The `toString()` method serializes the parameters into a query string.

```javascript
const params = new URLSearchParams({
  name: "Iqbal",
  role: "developer"
});

console.log(params.toString());
```

Output:

```text
name=Iqbal&role=developer
```

The returned string does not include the leading `?`.

# 🗁 keys(), values(), and entries()

These methods allow you to iterate over query parameters.

**keys():**

```javascript
const params = new URLSearchParams(
  "name=Iqbal&role=developer"
);

for (const key of params.keys()) {
  console.log(key);
}
```

**values():**

```javascript
for (const value of params.values()) {
  console.log(value);
}
```

**entries():**

```javascript
for (const [key, value] of params.entries()) {
  console.log(key, value);
}
```

You can also iterate directly over the object:

```javascript
for (const [key, value] of params) {
  console.log(`${key}: ${value}`);
}
```

# 🗁 sort()

The `sort()` method sorts query parameters by their keys.

It sorts keys according to their UTF-16 code units.

Example:

```javascript
const params = new URLSearchParams(
  "z=1&a=2&m=3"
);

params.sort();

console.log(params.toString());
```

Output:

```text
a=2&m=3&z=1
```

Values associated with equal keys retain their relative order.

# 🗁 Encoding and Decoding

URL query parameters may contain characters that require percent encoding.

`URLSearchParams` automatically handles the encoding of parameter names and values.

Example:

```javascript
const params = new URLSearchParams();

params.set("search", "JavaScript & TypeScript");

console.log(params.toString());
```

Output:

```text
search=JavaScript+%26+TypeScript
```

When reading the parameter:

```javascript
console.log(params.get("search"));
```

Output:

```text
JavaScript & TypeScript
```

Spaces are represented as `+` when serialized as form-style query parameters.

# 🗁 URLSearchParams with URL

A URL object provides a `searchParams` property that returns its associated `URLSearchParams` object.

Example:

```javascript
const url = new URL(
  "https://example.com/products?page=1&limit=10"
);

const params = url.searchParams;

console.log(params.get("page"));
console.log(params.get("limit"));
```

Modify the query:

```javascript
url.searchParams.set("page", "2");

url.searchParams.set("sort", "price");

console.log(url.href);
```

The URL automatically reflects the updated parameters.

# 🗁 Building API URLs

The URL API is particularly useful when constructing API endpoints dynamically.

Example:

```javascript
const url = new URL(
  "https://api.example.com/products"
);

url.searchParams.set("page", "1");

url.searchParams.set("limit", "20");

url.searchParams.set("category", "electronics");

console.log(url.href);
```

Use it with Fetch:

```javascript
async function getProducts() {
  const url = new URL(
    "https://api.example.com/products"
  );

  url.searchParams.set("page", "1");

  url.searchParams.set("limit", "20");

  const response = await fetch(url);

  if (!response.ok) {
    throw new Error(
      `HTTP error: ${response.status}`
    );
  }

  return response.json();
}
```

This approach is easier to maintain than manually concatenating query strings.

# 🗁 URL.canParse()

The `URL.canParse()` static method checks whether a URL can be parsed.

It returns a boolean instead of throwing an exception for an invalid URL.

Example:

```javascript
console.log(
  URL.canParse("https://example.com")
);

console.log(
  URL.canParse("not-a-valid-url")
);
```

Output:

```text
true
false
```

Relative URLs require a valid base URL:

```javascript
console.log(
  URL.canParse("/products", "https://example.com")
);
```

# 🗁 URL.parse()

The `URL.parse()` static method attempts to parse a URL and returns a URL object when successful.

If parsing fails, it returns `null`.

Example:

```javascript
const url = URL.parse(
  "https://example.com/products"
);

console.log(url?.pathname);
```

For an invalid URL:

```javascript
const invalidUrl = URL.parse("invalid-url");

console.log(invalidUrl);
```

Output:

```text
null
```

Relative URLs can also be resolved using a base URL:

```javascript
const url = URL.parse(
  "/products",
  "https://example.com"
);

console.log(url?.href);
```

# 🗁 URL.createObjectURL()

The `URL.createObjectURL()` static method creates a temporary URL representing a `Blob`, `File`, or another supported object.

Example:

```javascript
const blob = new Blob(
  ["Hello, World!"],
  { type: "text/plain" }
);

const objectURL = URL.createObjectURL(blob);

console.log(objectURL);
```

The generated URL typically looks like:

```text
blob:https://example.com/...
```

It can be used to display or download locally available data.

Example:

```javascript
const blob = new Blob(
  ["Hello, World!"],
  { type: "text/plain" }
);

const objectURL = URL.createObjectURL(blob);

const link = document.createElement("a");

link.href = objectURL;

link.download = "message.txt";

link.click();

URL.revokeObjectURL(objectURL);
```

# 🗁 URL.revokeObjectURL()

The `URL.revokeObjectURL()` method releases an object URL created with `URL.createObjectURL()`.

Example:

```javascript
const blob = new Blob(
  ["Temporary data"],
  { type: "text/plain" }
);

const objectURL = URL.createObjectURL(blob);

// Use the object URL

URL.revokeObjectURL(objectURL);
```

Releasing unused object URLs helps prevent unnecessary resource retention.

For downloads, ensure the browser has had an opportunity to use the URL before revoking it.

# 🗁 Displaying Local Images

Object URLs are useful for previewing images selected from the user's device.

HTML:

```html
<input type="file" id="imageInput" accept="image/*">

<img id="preview" alt="Image preview" width="300">
```

JavaScript:

```javascript
const input = document.querySelector("#imageInput");

const preview = document.querySelector("#preview");

let currentObjectURL = null;

input.addEventListener("change", () => {
  const file = input.files[0];

  if (!file) {
    return;
  }

  if (currentObjectURL) {
    URL.revokeObjectURL(currentObjectURL);
  }

  currentObjectURL = URL.createObjectURL(file);

  preview.src = currentObjectURL;
});
```

When replacing an image, the previous object URL is released.

A complete application should also revoke the final object URL when the preview is no longer needed.

# 🗁 URL API vs Location API

Both APIs provide access to URL information, but they serve different purposes.

| Feature | URL API | Location API |
| --- | --- | --- |
| Primary purpose | Parse and manipulate URL objects | Access and control the current document location |
| Create URL objects | Yes | No |
| Read query parameters | Through `searchParams` | Through `location.search` |
| Change current navigation | No, not by itself | Yes |
| Work with arbitrary URLs | Yes | Represents current document location |
| Object URL creation | Yes | No |

Example:

```javascript
const url = new URL(window.location.href);

console.log(url.pathname);

console.log(location.pathname);
```

Use the URL API for structured URL manipulation and the Location API for browser navigation.

# 🗁 Common Mistakes

**Treating query strings as plain objects**

Incorrect:

```javascript
const params = {
  page: 1,
  limit: 10
};

console.log(params.toString());
```

This does not create a URL query string.

Correct:

```javascript
const params = new URLSearchParams({
  page: "1",
  limit: "10"
});

console.log(params.toString());
```

**Forgetting that search includes the question mark**

```javascript
const url = new URL("https://example.com?page=1");

console.log(url.search); // ?page=1
```

**Assuming URLSearchParams.get() returns every matching value**

```javascript
const params = new URLSearchParams(
  "tag=js&tag=ts"
);

console.log(params.get("tag"));    // js
console.log(params.getAll("tag")); // ["js", "ts"]
```

**Manually concatenating query parameters**

Incorrect:

```javascript
const url = "/search?q=" + userInput;
```

Correct:

```javascript
const url = new URL("/search", location.origin);

url.searchParams.set("q", userInput);

console.log(url.href);
```

**Forgetting to revoke object URLs**

Object URLs should be released when they are no longer needed.

```javascript
URL.revokeObjectURL(objectURL);
```

# 🗁 Best Practices

* Use `new URL()` instead of manually parsing URL strings.
* Provide a base URL when resolving relative paths.
* Use `URLSearchParams` for query parameter manipulation.
* Prefer `set()` when a parameter should have only one value.
* Use `append()` when repeated parameter values are intentional.
* Use `getAll()` when query parameters may appear multiple times.
* Use `URL.canParse()` or `URL.parse()` when validating or parsing potentially invalid URLs.
* Avoid placing passwords or sensitive credentials in URLs.
* Use `URL.createObjectURL()` for temporary local file previews.
* Revoke object URLs when they are no longer needed.
* Avoid manually encoding values that are already handled by `URLSearchParams`.
* Validate untrusted URLs before using them for navigation or resource loading.

# 🗁 Summary

The URL API provides structured tools for working with URLs.

Important concepts include:

* `URL` constructor.
* URL components such as `href`, `origin`, `protocol`, `hostname`, `pathname`, `search`, and `hash`.
* Relative URL resolution.
* `URLSearchParams` for query parameters.
* `get()`, `getAll()`, `set()`, `append()`, `has()`, and `delete()`.
* Query string encoding and decoding.
* `URL.canParse()` and `URL.parse()`.
* `URL.createObjectURL()` and `URL.revokeObjectURL()`.
* Differences between URL API and Location API.

The URL API is essential for building reliable API clients, client-side routers, search interfaces, and browser-based applications.
