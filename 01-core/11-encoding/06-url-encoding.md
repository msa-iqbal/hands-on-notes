# URL Encoding

URL encoding, also known as percent-encoding, is a mechanism for representing data safely inside URLs.

It converts bytes into a textual representation using the percent sign (`%`) followed by two hexadecimal digits.

URL encoding is especially important when working with:

* Query parameters.
* Search terms.
* User input.
* API endpoints.
* URL path segments.
* Form submissions.
* Internationalized URLs.
* HTTP requests.

It prevents characters from being interpreted as URL delimiters when they are intended to be part of the data.

# 🗁 What Is URL Encoding?

A URL contains structural characters that separate different components.

For example:

```text
https://example.com/search?q=hello%20world
```

Its components include:

| Component | Value |
| --- | --- |
| Scheme | `https` |
| Host | `example.com` |
| Path | `/search` |
| Query parameter | `q=hello%20world` |

Some characters have special meanings in URLs.

For example:

* `?` introduces a query string.
* `&` separates query parameters.
* `=` separates a parameter name from its value.
* `#` introduces a fragment.
* `/` separates path segments.

If these characters are part of user-provided data, they may need encoding to prevent confusion with URL structure.

Example:

Original search value:

```text
hello world
```

URL-encoded value:

```text
hello%20world
```

Complete URL:

```text
https://example.com/search?q=hello%20world
```

# 🗁 How Percent-Encoding Works

Percent-encoding represents a byte using three characters:

```text
%HH
```

Where:

* `%` identifies an encoded byte.
* `HH` represents the byte's hexadecimal value.

Examples:

| Character | UTF-8 Byte | Percent-Encoding |
| --- | --- | --- |
| Space | `20` | `%20` |
| `!` | `21` | `%21` |
| `#` | `23` | `%23` |
| `$` | `24` | `%24` |
| `%` | `25` | `%25` |
| `&` | `26` | `%26` |
| `+` | `2B` | `%2B` |
| `/` | `2F` | `%2F` |
| `=` | `3D` | `%3D` |
| `?` | `3F` | `%3F` |

Hexadecimal digits can be uppercase or lowercase.

For example:

```text
%2F
%2f
```

Both represent the same byte.

Uppercase hexadecimal is commonly preferred for readability and consistency.

# 🗁 URL Encoding Works on Bytes

One of the most important concepts is that percent-encoding represents bytes, not abstract Unicode characters directly.

For non-ASCII text, the usual process is:

```text
Unicode text
     |
     v
UTF-8 encoding
     |
     v
Bytes
     |
     v
Percent-encoding
     |
     v
URL-safe representation
```

Example:

Bengali character:

```text
অ
```

Unicode code point:

```text
U+0985
```

UTF-8 bytes:

```text
E0 A6 85
```

Percent-encoded representation:

```text
%E0%A6%85
```

Complete URL:

```text
https://example.com/search?q=%E0%A6%85
```

This is why UTF-8 and URL encoding are closely related when handling international text.

# 🗁 Unreserved Characters

RFC 3986 defines a set of characters that do not need percent-encoding in a URI.

These are called unreserved characters.

They include:

**Letters**

```text
A-Z
a-z
```

**Digits**

```text
0-9
```

**Special characters**

```text
- . _ ~
```

Examples:

| Character | Encoding Required? |
| --- | --- |
| `A` | No |
| `z` | No |
| `5` | No |
| `-` | No |
| `_` | No |
| `.` | No |
| `~` | No |
| Space | Yes |
| `&` | Yes |
| `=` | Yes |

Unreserved characters can generally appear directly in URL components without percent-encoding.

# 🗁 Reserved Characters

Reserved characters have structural meanings in URLs.

The reserved character set includes:

```text
: / ? # [ ] @ ! $ & ' ( ) * + , ; =
```

These characters are used for URL syntax, delimiters, and other structural purposes.

Whether a reserved character should be encoded depends on the URL component and the meaning intended.

For example, `&` separates query parameters:

```text
/search?q=hello&category=books
```

If the search value itself contains an ampersand, it should be encoded:

```text
/search?q=Tom%20%26%20Jerry
```

Otherwise, the ampersand could be interpreted as a parameter separator.

# 🗁 Space Encoding: %20 vs +

Spaces have two commonly encountered representations in URL-related data.

```text
%20
```

And:

```text
+
```

These representations are not interchangeable in every context.

**Percent-encoding**

`%20` represents the UTF-8 byte for a space character.

It is the standard percent-encoded representation of a space.

**Form URL encoding**

In `application/x-www-form-urlencoded`, a space is commonly represented using `+`.

Example:

```text
name=John+Doe
```

Both representations can represent a space depending on the decoding rules being used.

However, a literal plus sign in form data must be encoded as:

```text
%2B
```

# 🗁 application/x-www-form-urlencoded

`application/x-www-form-urlencoded` is a format used when submitting HTML form data.

It represents form fields as name-value pairs.

Example:

```text
username=john&email=john%40example.com
```

The rules include:

* Spaces are represented as `+`.
* Special bytes are percent-encoded.
* Parameters are separated using `&`.
* Names and values are separated using `=`.

Example form:

```html
<form action="/login" method="GET">
    <input name="username" value="John Doe">
    <input name="email" value="john@example.com">
    <button type="submit">Submit</button>
</form>
```

A possible encoded query string:

```text
username=John+Doe&email=john%40example.com
```

Modern JavaScript and Python provide dedicated utilities for working with this format.

# 🗁 URL Encoding in JavaScript

JavaScript provides several URL-related functions.

| Function | Purpose |
| --- | --- |
| `encodeURI()` | Encodes a complete URI while preserving its structural characters |
| `encodeURIComponent()` | Encodes an individual URI component |
| `decodeURI()` | Decodes a complete URI while preserving reserved delimiters |
| `decodeURIComponent()` | Decodes an individual URI component |

# 🗁 encodeURI()

`encodeURI()` is intended for encoding a complete URI.

It preserves URL structural characters such as:

```text
: / ? # & =
```

Example:

```javascript
const url = "https://example.com/search?q=hello world";

const encoded = encodeURI(url);

console.log(encoded);
```

Output:

```text
https://example.com/search?q=hello%20world
```

Notice that the URL structure remains intact.

# 🗁 encodeURIComponent()

`encodeURIComponent()` is intended for encoding an individual URL component, such as a query parameter value.

Example:

```javascript
const value = "Tom & Jerry";

const encoded = encodeURIComponent(value);

console.log(encoded);
// Tom%20%26%20Jerry
```

The ampersand is encoded because it is part of the value, not a query parameter separator.

Build a URL:

```javascript
const search = "Tom & Jerry";

const url =
    "https://example.com/search?q=" +
    encodeURIComponent(search);

console.log(url);
```

Result:

```text
https://example.com/search?q=Tom%20%26%20Jerry
```

For individual query parameter values, `encodeURIComponent()` is generally more appropriate than `encodeURI()`.

# 🗁 decodeURI()

`decodeURI()` decodes a complete URI.

Example:

```javascript
const encoded =
    "https://example.com/search?q=hello%20world";

const decoded = decodeURI(encoded);

console.log(decoded);
```

Output:

```text
https://example.com/search?q=hello world
```

It preserves certain reserved encodings because they may represent URL structure.

# 🗁 decodeURIComponent()

`decodeURIComponent()` decodes an individual URI component.

Example:

```javascript
const encoded = "Tom%20%26%20Jerry";

const decoded = decodeURIComponent(encoded);

console.log(decoded);
// Tom & Jerry
```

Unicode example:

```javascript
const encoded = "%E0%A6%85";

console.log(decodeURIComponent(encoded));
// অ
```

Malformed percent-encoded sequences can cause a `URIError`.

# 🗁 URL API in JavaScript

JavaScript provides the `URL` class for parsing, constructing, and manipulating URLs.

Example:

```javascript
const url = new URL("https://example.com/search");

url.searchParams.set("q", "hello world");
url.searchParams.set("category", "books");

console.log(url.toString());
```

Output:

```text
https://example.com/search?q=hello+world&category=books
```

The URL API handles the required query parameter encoding.

Read parameters:

```javascript
const url = new URL(
    "https://example.com/search?q=hello+world"
);

console.log(url.searchParams.get("q"));
// hello world
```

Update parameters:

```javascript
const url = new URL("https://example.com/search");

url.searchParams.set("q", "Tom & Jerry");

console.log(url.toString());
```

Remove parameters:

```javascript
url.searchParams.delete("q");
```

Check whether a parameter exists:

```javascript
console.log(url.searchParams.has("q"));
```

# 🗁 URLSearchParams

`URLSearchParams` provides a convenient interface for working with query strings.

Example:

```javascript
const params = new URLSearchParams({
    name: "John Doe",
    email: "john@example.com",
    city: "Dhaka"
});

console.log(params.toString());
```

Output:

```text
name=John+Doe&email=john%40example.com&city=Dhaka
```

Read values:

```javascript
const params = new URLSearchParams(
    "name=John+Doe&city=Dhaka"
);

console.log(params.get("name"));
// John Doe

console.log(params.get("city"));
// Dhaka
```

Handle repeated parameters:

```javascript
const params = new URLSearchParams();

params.append("tag", "javascript");
params.append("tag", "nodejs");

console.log(params.toString());
// tag=javascript&tag=nodejs

console.log(params.getAll("tag"));
// ["javascript", "nodejs"]
```

`URLSearchParams` uses form-style query serialization, where spaces are represented as `+`.

# 🗁 URL Encoding in Python

Python provides URL encoding utilities through:

```python
urllib.parse
```

Import:

```python
from urllib.parse import (
    quote,
    quote_plus,
    unquote,
    unquote_plus,
    urlencode
)
```

# 🗁 quote()

`quote()` percent-encodes a string using UTF-8 by default.

Example:

```python
from urllib.parse import quote

text = "hello world"

encoded = quote(text)

print(encoded)
# hello%20world
```

Unicode example:

```python
from urllib.parse import quote

text = "বাংলা"

encoded = quote(text)

print(encoded)
```

Output:

```text
%E0%A6%AC%E0%A6%BE%E0%A6%82%E0%A6%B2%E0%A6%BE
```

By default, `/` is considered safe by `quote()`.

You can change this behavior using the `safe` parameter:

```python
from urllib.parse import quote

path = "products/mobile phones"

encoded = quote(path, safe="")

print(encoded)
# products%2Fmobile%20phones
```

# 🗁 quote_plus()

`quote_plus()` is intended for form-style URL encoding.

It converts spaces into `+`.

Example:

```python
from urllib.parse import quote_plus

text = "hello world"

encoded = quote_plus(text)

print(encoded)
# hello+world
```

Compare:

```python
from urllib.parse import quote, quote_plus

text = "hello world"

print(quote(text))
# hello%20world

print(quote_plus(text))
# hello+world
```

# 🗁 unquote()

`unquote()` decodes percent-encoded data.

Example:

```python
from urllib.parse import unquote

encoded = "hello%20world"

decoded = unquote(encoded)

print(decoded)
# hello world
```

Unicode example:

```python
from urllib.parse import unquote

encoded = "%E0%A6%AC%E0%A6%BE%E0%A6%82%E0%A6%B2%E0%A6%BE"

print(unquote(encoded))
# বাংলা
```

# 🗁 unquote_plus()

`unquote_plus()` decodes form-style URL data.

It converts `+` into a space and decodes percent-encoded bytes.

Example:

```python
from urllib.parse import unquote_plus

encoded = "hello+world"

print(unquote_plus(encoded))
# hello world
```

Unlike `unquote()`, `unquote_plus()` treats plus signs as spaces.

# 🗁 urlencode()

`urlencode()` converts a mapping or sequence of key-value pairs into a query string.

Example:

```python
from urllib.parse import urlencode

params = {
    "name": "John Doe",
    "email": "john@example.com",
    "city": "Dhaka"
}

query = urlencode(params)

print(query)
```

Output:

```text
name=John+Doe&email=john%40example.com&city=Dhaka
```

Build a complete URL:

```python
from urllib.parse import urlencode

params = {
    "q": "Tom & Jerry",
    "category": "cartoons"
}

url = "https://example.com/search?" + urlencode(params)

print(url)
```

Handle repeated parameters:

```python
from urllib.parse import urlencode

params = [
    ("tag", "javascript"),
    ("tag", "nodejs")
]

query = urlencode(params)

print(query)
# tag=javascript&tag=nodejs
```

# 🗁 URL Encoding Path Segments

URL paths use `/` as a segment separator.

Consider:

```text
/products/mobile-phones
```

Here, the path contains two segments:

```text
products
mobile-phones
```

If a path segment contains a slash as actual data, encode it so that it is not interpreted as a separator.

Example:

```javascript
const productName = "mobile/phones";

const url =
    "/products/" + encodeURIComponent(productName);

console.log(url);
// /products/mobile%2Fphones
```

In Python:

```python
from urllib.parse import quote

product_name = "mobile/phones"

encoded = quote(product_name, safe="")

url = "/products/" + encoded

print(url)
# /products/mobile%2Fphones
```

This is useful when a route contains dynamic values that must remain within one path segment.

However, server frameworks and reverse proxies may handle encoded slashes differently, so verify the behavior of the complete routing stack.

# 🗁 URL Encoding Query Parameters

Query parameters should be encoded individually rather than encoding the entire URL indiscriminately.

Example:

```javascript
const params = new URLSearchParams();

params.set("name", "John & Jane");
params.set("city", "Dhaka");

const url = "https://example.com/users?" + params.toString();

console.log(url);
```

Output:

```text
https://example.com/users?name=John+%26+Jane&city=Dhaka
```

The `&` inside the name is encoded, while the `&` separating parameters remains part of the URL structure.

# 🗁 URL Encoding vs HTML Encoding

URL encoding and HTML encoding solve different problems.

**URL encoding**

Protects data when it is placed inside URL components.

Example:

```text
Tom & Jerry
```

Encoded:

```text
Tom%20%26%20Jerry
```

**HTML encoding**

Represents special characters safely inside HTML text or attributes.

Example:

```html
<p>Tom &amp; Jerry</p>
```

Common HTML entities include:

| Character | HTML Entity |
| --- | --- |
| `&` | `&amp;` |
| `<` | `&lt;` |
| `>` | `&gt;` |
| `"` | `&quot;` |
| `'` | `&#39;` |

These mechanisms are not interchangeable.

Always use the correct encoding for the context where data is inserted.

# 🗁 URL Encoding vs Base64

Both URL encoding and Base64 can represent data using text characters, but their purposes differ.

| Feature | URL Encoding | Base64 |
| --- | --- | --- |
| Main purpose | Safely represent URL component data | Represent binary data as text |
| Mechanism | Percent-encoded bytes | 6-bit alphabet mapping |
| Output | `%HH` sequences and unreserved characters | Base64 alphabet |
| Common usage | URLs and form data | Binary transport and embedded data |
| Size overhead | Depends on encoded bytes | Approximately 33.33% for large inputs |

URL encoding should be used for URL components.

Base64 should be used when binary data needs a text representation.

# 🗁 Double URL Encoding

Double encoding happens when already percent-encoded data is encoded again.

Example:

Original:

```text
hello world
```

First encoding:

```text
hello%20world
```

Second encoding:

```text
hello%2520world
```

Why?

The percent character `%` is encoded as:

```text
%25
```

Therefore:

```text
%20 → %2520
```

This can cause unexpected behavior when different application layers decode the value a different number of times.

Example in JavaScript:

```javascript
const original = "hello world";

const first = encodeURIComponent(original);
const second = encodeURIComponent(first);

console.log(first);
// hello%20world

console.log(second);
// hello%2520world
```

Decode once:

```javascript
console.log(decodeURIComponent(second));
// hello%20world
```

Decode again:

```javascript
console.log(decodeURIComponent(
    decodeURIComponent(second)
));
// hello world
```

Encode and decode data at the appropriate application boundary. Do not repeatedly encode or decode values without a clear reason.

# 🗁 Malformed Percent-Encoding

Percent-encoded bytes must use two hexadecimal digits after `%`.

Valid examples:

```text
%20
%2F
%E0%A6%85
```

Invalid examples:

```text
%2
%ZZ
%GG
```

JavaScript decoding functions can throw an error when they encounter malformed percent-encoded sequences.

Example:

```javascript
try {
    console.log(decodeURIComponent("%ZZ"));
} catch (error) {
    console.error("Invalid URL encoding");
}
```

Python's `urllib.parse.unquote()` is more permissive by default and may preserve malformed sequences rather than raising an exception.

For strict validation, use an explicit validation strategy appropriate to your application.

# 🗁 URL Encoding and Security

Correct URL encoding is important for application security, but encoding alone does not make input safe.

Important considerations include:

**Input validation**

Validate user input according to the expected data type and application rules.

**Canonicalization**

Different encoded representations may resolve to equivalent data.

Normalize and interpret data consistently across application layers.

**Double decoding**

Repeated decoding can expose delimiters that were previously encoded.

Avoid inconsistent decoding between proxies, web servers, frameworks, and application code.

**Path traversal**

Encoded path characters such as `%2E` and `%2F` may be interpreted differently by different components.

Validate resolved paths and enforce directory boundaries rather than relying on encoding alone.

**Open redirects**

Do not assume that an encoded URL is a trustworthy destination.

Validate redirect targets against application policy.

**SQL injection**

URL encoding is not a replacement for parameterized SQL queries.

**XSS**

URL encoding is not a replacement for context-appropriate HTML output encoding.

Use appropriate validation, parsing, authorization, and output encoding at each boundary.

# 🗁 Common Mistakes

* Encoding an entire URL when only one component should be encoded.
* Using `encodeURI()` for individual query parameter values.
* Treating `+` and `%20` as interchangeable in every context.
* Forgetting to encode literal plus signs in form-style data.
* Encoding Unicode without first considering its UTF-8 byte representation.
* Applying URL encoding multiple times.
* Decoding data repeatedly without understanding the application flow.
* Confusing URL encoding with HTML encoding.
* Assuming encoded input is automatically safe.
* Constructing query strings through manual concatenation instead of using URL utilities.

# 🗁 Practical Examples

**Build a search URL in JavaScript**

```javascript
const search = "বাংলা programming";

const url = new URL("https://example.com/search");

url.searchParams.set("q", search);

console.log(url.toString());
```

**Build a search URL in Python**

```python
from urllib.parse import urlencode

search = "বাংলা programming"

query = urlencode({
    "q": search
})

url = "https://example.com/search?" + query

print(url)
```

**Encode and decode a value in JavaScript**

```javascript
const original = "Tom & Jerry + cartoons";

const encoded = encodeURIComponent(original);
const decoded = decodeURIComponent(encoded);

console.log("Original:", original);
console.log("Encoded:", encoded);
console.log("Decoded:", decoded);
```

**Encode and decode a value in Python**

```python
from urllib.parse import quote, unquote

original = "Tom & Jerry + cartoons"

encoded = quote(original, safe="")
decoded = unquote(encoded)

print("Original:", original)
print("Encoded:", encoded)
print("Decoded:", decoded)
```

**Compare form-style and percent encoding**

```python
from urllib.parse import quote, quote_plus

value = "hello world"

print(quote(value))
# hello%20world

print(quote_plus(value))
# hello+world
```

# 🗁 Best Practices

* Encode individual URL components rather than blindly encoding complete URLs.
* Use `encodeURIComponent()` for individual JavaScript URI components.
* Use `URL` and `URLSearchParams` to construct and manipulate URLs.
* Use Python's `urllib.parse` utilities instead of manually replacing characters.
* Understand the difference between `%20` and `+`.
* Encode Unicode text using UTF-8 before percent-encoding its bytes.
* Avoid double encoding and unnecessary decoding.
* Validate input independently of its encoded representation.
* Use context-appropriate output encoding for HTML and other formats.
* Handle malformed percent-encoded input according to application requirements.
* Test URL behavior across proxies, servers, frameworks, and browsers.
* Keep parsing and canonicalization behavior consistent across application layers.

# 🗁 Summary

URL encoding represents data safely inside URL components using percent-encoded bytes.

Its main characteristics include:

* Uses `%HH` to represent individual bytes.
* Commonly uses UTF-8 before percent-encoding Unicode text.
* Leaves unreserved characters unchanged.
* Encodes reserved characters when they are data rather than URL delimiters.
* Uses `%20` for spaces in percent-encoding.
* Uses `+` for spaces in `application/x-www-form-urlencoded`.
* JavaScript provides `encodeURIComponent()`, `URL`, and `URLSearchParams`.
* Python provides `quote()`, `quote_plus()`, `unquote()`, and `urlencode()`.
* Double encoding and inconsistent decoding can cause application errors and security problems.

Understanding URL encoding is essential for building web applications, APIs, search functionality, dynamic routes, and reliable HTTP communication.
