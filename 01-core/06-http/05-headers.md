# HTTP Headers

HTTP headers are key-value pairs included in HTTP requests and responses. They provide additional information about the communication between a client and a server.

Headers describe details such as:

* The content type of a request or response.
* The content length.
* Authentication credentials.
* Caching instructions.
* Accepted response formats.
* Cookies and session information.
* Compression capabilities.
* Cross-origin resource sharing permissions.
* Connection and transfer behavior.

For example:

```http
GET /api/users HTTP/1.1
Host: api.example.com
Accept: application/json
Authorization: Bearer your-access-token
User-Agent: Mozilla/5.0
```

In this request:

* `Host` identifies the target server.
* `Accept` indicates the preferred response format.
* `Authorization` provides authentication credentials.
* `User-Agent` identifies the client software.

HTTP headers are essential for understanding how browsers, APIs, servers, proxies, and other intermediaries communicate.

# 🗁 Understanding HTTP Headers

HTTP headers carry metadata about HTTP requests and responses.

They allow clients and servers to exchange information without placing everything inside the message body.

Consider this request:

```http
POST /api/users HTTP/1.1
Host: api.example.com
Content-Type: application/json
Accept: application/json
Authorization: Bearer example-token
Content-Length: 44

{
  "name": "John",
  "email": "john@example.com"
}
```

The headers describe the request, while the body contains the actual user data.

The server can use these headers to determine:

* How to interpret the request body.
* Which response format the client accepts.
* Whether the request includes authentication credentials.
* How much content is being transmitted.

### 🖆 Why HTTP Headers Matter

HTTP headers support several important communication mechanisms:

| Purpose | Example Headers |
| --- | --- |
| Identify the target server | `Host` |
| Describe request content | `Content-Type` |
| Specify accepted formats | `Accept` |
| Authenticate requests | `Authorization` |
| Manage caching | `Cache-Control` |
| Maintain sessions | `Cookie`, `Set-Cookie` |
| Control browser access | `Access-Control-Allow-Origin` |
| Protect applications | `Content-Security-Policy` |
| Compress content | `Accept-Encoding`, `Content-Encoding` |
| Validate resources | `ETag`, `If-None-Match` |

# 🗁 HTTP Header Structure

An HTTP header consists of a field name and a field value.

General format:

```http
Header-Name: Header-Value
```

Example:

```http
Content-Type: application/json
```

Here:

* `Content-Type` is the field name.
* `application/json` is the field value.

Multiple headers can appear in the same HTTP message.

```http
GET /products HTTP/1.1
Host: api.example.com
Accept: application/json
Accept-Language: en-US
User-Agent: Mozilla/5.0
```

### 🖆 Header Field Names

HTTP header field names are case-insensitive.

These represent the same header:

```http
Content-Type: application/json
content-type: application/json
CONTENT-TYPE: application/json
```

However, conventional capitalization is commonly used for readability.

### 🖆 Header Values

Header values depend on the field definition.

Examples:

```http
Content-Type: application/json
Content-Length: 1024
Cache-Control: max-age=3600
Accept-Encoding: gzip, br
```

Some headers support multiple values or structured parameters.

For example:

```http
Accept: text/html, application/json
```

### 🖆 Headers and HTTP Versions

HTTP/1.1 uses textual header fields in its message format.

HTTP/2 and HTTP/3 preserve HTTP header semantics but use binary framing and compressed header representations.

* HTTP/2 uses HPACK for header compression.
* HTTP/3 uses QPACK for header compression.

The application-level meaning of common headers remains largely consistent across these HTTP versions.

# 🗁 Request Headers

Request headers are sent by the client to provide information about the request, the client, and its capabilities.

### 🖆 Host

Identifies the target host and, when necessary, port.

```http
Host: api.example.com
```

In HTTP/1.1, `Host` is required.

HTTP/2 and HTTP/3 use the `:authority` pseudo-header to convey the target authority, although `Host` can also appear in some messages.

### 🖆 User-Agent

Identifies the client software.

```http
User-Agent: Mozilla/5.0
```

Browsers commonly include information about their browser engine and platform.

Servers sometimes use this information for compatibility or analytics.

### 🖆 Accept

Specifies the media types the client can process.

```http
Accept: application/json, text/html
```

This tells the server that JSON and HTML representations are acceptable.

### 🖆 Accept-Language

Specifies the client's preferred natural languages.

```http
Accept-Language: en-US,en;q=0.9
```

The `q` parameter indicates relative preference.

### 🖆 Accept-Encoding

Specifies supported content-coding formats.

```http
Accept-Encoding: gzip, br, zstd
```

The server can select a supported encoding and identify it using `Content-Encoding`.

### 🖆 Authorization

Contains credentials used to authenticate a request.

```http
Authorization: Bearer example-access-token
```

Other authentication schemes include Basic and Digest.

### 🖆 Cookie

Sends cookies previously stored by the browser or another HTTP client.

```http
Cookie: session_id=abc123; theme=dark
```

The server can use cookies to identify sessions, preferences, or other application state.

### 🖆 Referer

Indicates the referring URL from which the current request originated.

```http
Referer: https://example.com/products
```

The spelling `Referer` is part of the standardized field name.

The actual value may be restricted by the browser's referrer policy.

### 🖆 Origin

Identifies the origin making a request.

```http
Origin: https://frontend.example.com
```

An origin consists of:

* Scheme.
* Host.
* Port.

Origin is especially important in browser CORS communication.

### 🖆 Range

Requests a specific portion of a resource.

```http
Range: bytes=0-999
```

This requests the first 1,000 bytes of a representation.

Servers can respond with `206 Partial Content` when the range is successfully served.

### 🖆 If-None-Match

Makes a conditional request using an entity tag.

```http
If-None-Match: "abc123"
```

If the selected representation has not changed, the server can respond with:

```http
HTTP/1.1 304 Not Modified
```

### 🖆 Common Request Headers

| Header | Purpose |
| --- | --- |
| `Host` | Identifies the target host in HTTP/1.1 |
| `User-Agent` | Identifies the client |
| `Accept` | Specifies accepted media types |
| `Accept-Language` | Specifies preferred languages |
| `Accept-Encoding` | Specifies supported content codings |
| `Authorization` | Provides authentication credentials |
| `Cookie` | Sends stored cookies |
| `Referer` | Identifies the referring URL |
| `Origin` | Identifies the request origin |
| `Range` | Requests part of a representation |
| `If-None-Match` | Performs an ETag-based conditional request |

# 🗁 Response Headers

Response headers are sent by the server to provide information about the response, the returned representation, and server behavior.

### 🖆 Server

Identifies information about the server software.

```http
Server: nginx
```

Servers may omit or restrict this header to avoid exposing unnecessary implementation details.

### 🖆 Date

Indicates when the response message was originated.

```http
Date: Mon, 09 Nov 2026 04:00:00 GMT
```

HTTP date values use the standardized HTTP-date format.

### 🖆 Location

Identifies a resource location or redirect target.

```http
Location: https://example.com/new-page
```

Commonly used with redirect responses such as:

```http
HTTP/1.1 301 Moved Permanently
Location: https://example.com/new-page
```

It is also used to identify newly created resources in some successful responses.

### 🖆 Set-Cookie

Instructs the client to store a cookie.

```http
Set-Cookie: session_id=abc123; HttpOnly; Secure; SameSite=Lax
```

Browsers process this header according to cookie rules and attributes.

### 🖆 Cache-Control

Defines caching instructions.

```http
Cache-Control: public, max-age=3600
```

This allows eligible caches to store the response for up to 3,600 seconds while it remains fresh.

### 🖆 ETag

Provides an entity tag that identifies a particular representation version.

```http
ETag: "abc123"
```

Clients can use this value for conditional requests.

### 🖆 Last-Modified

Indicates when the selected representation was last modified.

```http
Last-Modified: Sun, 08 Nov 2026 12:00:00 GMT
```

Clients can use this value with `If-Modified-Since`.

### 🖆 Allow

Identifies the HTTP methods supported by a resource.

```http
Allow: GET, HEAD, OPTIONS
```

This header commonly appears in a `405 Method Not Allowed` response.

### 🖆 Retry-After

Indicates how long a client should wait before retrying.

```http
Retry-After: 120
```

It can also contain an HTTP date.

This header is useful with responses such as `429 Too Many Requests` and `503 Service Unavailable`.

### 🖆 Common Response Headers

| Header | Purpose |
| --- | --- |
| `Server` | Identifies server software |
| `Date` | Indicates response origination time |
| `Location` | Identifies a resource or redirect target |
| `Set-Cookie` | Stores a cookie in the client |
| `Cache-Control` | Controls caching behavior |
| `ETag` | Identifies a representation version |
| `Last-Modified` | Indicates modification time |
| `Allow` | Lists supported methods |
| `Retry-After` | Indicates when to retry |

# 🗁 Representation Headers

Representation headers describe the representation being transferred in an HTTP message.

They provide information about the media type, language, encoding, and other characteristics of the representation.

### 🖆 Content-Type

Identifies the media type of the message content.

```http
Content-Type: application/json
```

Common values include:

| Content-Type | Description |
| --- | --- |
| `application/json` | JSON data |
| `text/html` | HTML document |
| `text/plain` | Plain text |
| `text/css` | CSS stylesheet |
| `text/javascript` | JavaScript source |
| `image/png` | PNG image |
| `image/jpeg` | JPEG image |
| `application/pdf` | PDF document |
| `application/xml` | XML document |
| `multipart/form-data` | Multipart form data |

### 🖆 Content-Length

Specifies the content length in bytes when applicable to the message framing.

```http
Content-Length: 1024
```

It is important not to confuse Content-Length with the number of characters in a string. Text encoding determines the number of transmitted bytes.

### 🖆 Content-Encoding

Identifies the content coding applied to the representation.

```http
Content-Encoding: gzip
```

The recipient decodes the representation according to the specified coding.

### 🖆 Content-Language

Identifies the natural language or languages of the representation.

```http
Content-Language: en-US
```

### 🖆 Content-Disposition

Provides instructions about how content should be presented.

Example:

```http
Content-Disposition: attachment; filename="report.pdf"
```

This commonly instructs browsers to download the file using the specified filename.

Another example:

```http
Content-Disposition: inline
```

This indicates that the content can be displayed inline when supported.

### 🖆 Content-Range

Identifies the portion of a representation included in a partial response.

```http
Content-Range: bytes 0-999/5000
```

This indicates that bytes 0 through 999 of a 5,000-byte representation are included.

### 🖆 Representation Header Summary

| Header | Purpose |
| --- | --- |
| `Content-Type` | Describes media type |
| `Content-Length` | Describes content length |
| `Content-Encoding` | Describes content coding |
| `Content-Language` | Describes natural language |
| `Content-Disposition` | Provides presentation instructions |
| `Content-Range` | Identifies a partial representation range |

# 🗁 Content Negotiation Headers

Content negotiation allows a client and server to select an appropriate representation of a resource.

For example, the same resource might be available in JSON and XML.

The client expresses its preferences through request headers.

### 🖆 Accept

Specifies acceptable media types.

```http
Accept: application/json, application/xml
```

### 🖆 Accept-Language

Specifies preferred natural languages.

```http
Accept-Language: en-US,en;q=0.9,bn;q=0.8
```

### 🖆 Accept-Encoding

Specifies supported content codings.

```http
Accept-Encoding: gzip, br
```

### 🖆 Quality Values

Quality values use the `q` parameter to express relative preference.

Example:

```http
Accept: text/html, application/json;q=0.9, */*;q=0.5
```

The preferences are:

* `text/html`: quality 1.0.
* `application/json`: quality 0.9.
* Any other media type: quality 0.5.

### 🖆 Vary

The server can use the `Vary` response header to identify request headers that influenced representation selection.

Example:

```http
Vary: Accept-Language
```

A cache must account for the specified request header when selecting a stored response.

# 🗁 Authentication Headers

Authentication headers allow clients to provide credentials and servers to communicate authentication requirements.

### 🖆 Authorization

The `Authorization` header sends credentials with a request.

Example using Bearer authentication:

```http
GET /api/profile HTTP/1.1
Host: api.example.com
Authorization: Bearer eyJhbGciOi...
```

Bearer tokens are commonly used with OAuth 2.0-based systems and other token authentication schemes.

### 🖆 Basic Authentication

Basic authentication sends a Base64-encoded username and password.

Example:

```http
Authorization: Basic dXNlcjpwYXNzd29yZA==
```

Base64 is an encoding mechanism, not encryption.

### 🖆 WWW-Authenticate

A server uses `WWW-Authenticate` to indicate the authentication scheme required to access a resource.

Example:

```http
HTTP/1.1 401 Unauthorized
WWW-Authenticate: Bearer realm="api"
```

### 🖆 Proxy-Authenticate

A proxy uses this header to identify the authentication scheme required to access it.

```http
HTTP/1.1 407 Proxy Authentication Required
Proxy-Authenticate: Basic realm="Proxy"
```

### 🖆 Proxy-Authorization

Provides credentials to authenticate with a proxy.

```http
Proxy-Authorization: Basic dXNlcjpwYXNzd29yZA==
```

### 🖆 Authentication Header Summary

| Header | Direction | Purpose |
| --- | --- | --- |
| `Authorization` | Request | Sends origin-server credentials |
| `WWW-Authenticate` | Response | Describes origin-server authentication requirements |
| `Proxy-Authorization` | Request | Sends proxy credentials |
| `Proxy-Authenticate` | Response | Describes proxy authentication requirements |

# 🗁 Caching Headers

HTTP caching headers control how responses are stored, reused, validated, and revalidated.

They are important for application performance and reducing unnecessary network requests.

### 🖆 Cache-Control

The most important HTTP caching header.

Example:

```http
Cache-Control: public, max-age=3600
```

Common directives include:

| Directive | Meaning |
| --- | --- |
| `public` | Allows shared caches to store the response |
| `private` | Restricts storage to private caches |
| `no-cache` | Requires validation before reuse |
| `no-store` | Prohibits storing the response |
| `max-age` | Defines freshness lifetime in seconds |
| `s-maxage` | Defines freshness lifetime for shared caches |
| `must-revalidate` | Requires revalidation once stale |
| `immutable` | Indicates that the representation will not change during its freshness lifetime |

### 🖆 Expires

Specifies an absolute expiration time for a response.

```http
Expires: Mon, 09 Nov 2026 05:00:00 GMT
```

Cache-Control generally takes precedence when both define freshness.

### 🖆 ETag

Identifies a representation version.

```http
ETag: "version-42"
```

Clients can use it to validate cached content.

### 🖆 Last-Modified

Indicates when a representation was last modified.

```http
Last-Modified: Sun, 08 Nov 2026 12:00:00 GMT
```

### 🖆 If-None-Match

The client sends an ETag validator:

```http
If-None-Match: "version-42"
```

If the representation has not changed, the server can return:

```http
HTTP/1.1 304 Not Modified
```

### 🖆 If-Modified-Since

The client sends a modification date:

```http
If-Modified-Since: Sun, 08 Nov 2026 12:00:00 GMT
```

If the resource has not changed since that date, the server can return `304 Not Modified`.

### 🖆 Example: Cache Validation

Initial response:

```http
HTTP/1.1 200 OK
Content-Type: application/json
Cache-Control: public, max-age=60
ETag: "user-v1"

{
  "id": 42,
  "name": "John"
}
```

After the response becomes stale, the client can send:

```http
GET /api/users/42 HTTP/1.1
Host: api.example.com
If-None-Match: "user-v1"
```

If the representation is unchanged:

```http
HTTP/1.1 304 Not Modified
ETag: "user-v1"
```

The client can reuse its stored representation.

# 🗁 Cookie Headers

Cookies allow servers to store small pieces of state in the browser.

They are commonly used for session management, authentication, preferences, and tracking.

### 🖆 Set-Cookie

The server uses `Set-Cookie` to create or update a cookie.

```http
Set-Cookie: session_id=abc123; HttpOnly; Secure; SameSite=Lax
```

### 🖆 Cookie

The browser sends applicable cookies back to the server.

```http
Cookie: session_id=abc123
```

### 🖆 Cookie Attributes

| Attribute | Purpose |
| --- | --- |
| `Expires` | Defines an expiration date |
| `Max-Age` | Defines lifetime in seconds |
| `Domain` | Defines applicable domain scope |
| `Path` | Defines applicable URL path scope |
| `Secure` | Restricts transmission to secure channels |
| `HttpOnly` | Prevents JavaScript access through `document.cookie` |
| `SameSite` | Controls cross-site cookie sending behavior |
| `Partitioned` | Enables partitioned cookie storage where supported |

Example:

```http
Set-Cookie: session_id=abc123; Max-Age=3600; Path=/; Secure; HttpOnly; SameSite=Lax
```

### 🖆 Multiple Cookies

A server can send multiple `Set-Cookie` fields:

```http
Set-Cookie: session_id=abc123; HttpOnly; Secure
Set-Cookie: theme=dark; Path=/
```

Clients send applicable cookies in the `Cookie` request header:

```http
Cookie: session_id=abc123; theme=dark
```

`Set-Cookie` is a special case and should not be combined into a single comma-separated field because cookie attributes can contain commas.

Cookie behavior is covered in more detail in the cookies and sessions documentation.

# 🗁 Security Headers

Security headers allow servers to communicate browser security policies and other security-related instructions.

They are important for reducing common web application vulnerabilities.

### 🖆 Strict-Transport-Security

HTTP Strict Transport Security (HSTS) instructs browsers to use HTTPS for future requests to a domain.

```http
Strict-Transport-Security: max-age=31536000; includeSubDomains
```

The browser remembers this policy for the specified duration.

### 🖆 Content-Security-Policy

Content Security Policy (CSP) controls which sources the browser is permitted to load content from.

```http
Content-Security-Policy: default-src 'self'; script-src 'self'
```

This example restricts default resources and scripts to the current origin.

CSP can help mitigate cross-site scripting and other content injection attacks.

### 🖆 X-Content-Type-Options

Prevents browsers from MIME-sniffing a response away from its declared content type.

```http
X-Content-Type-Options: nosniff
```

### 🖆 Referrer-Policy

Controls how much referrer information browsers include in outgoing requests.

```http
Referrer-Policy: strict-origin-when-cross-origin
```

### 🖆 Permissions-Policy

Controls access to selected browser features and APIs.

```http
Permissions-Policy: camera=(), microphone=(), geolocation=()
```

This example disables the listed features for the document and its embedded contexts unless a different policy is established.

### 🖆 Cross-Origin-Opener-Policy

Controls how a document interacts with cross-origin documents through browsing context relationships.

```http
Cross-Origin-Opener-Policy: same-origin
```

### 🖆 Cross-Origin-Resource-Policy

Controls whether other origins can load a resource.

```http
Cross-Origin-Resource-Policy: same-origin
```

### 🖆 Security Header Summary

| Header | Purpose |
| --- | --- |
| `Strict-Transport-Security` | Enforces HTTPS for future requests |
| `Content-Security-Policy` | Restricts permitted content sources |
| `X-Content-Type-Options` | Prevents MIME sniffing |
| `Referrer-Policy` | Controls referrer information |
| `Permissions-Policy` | Controls browser feature access |
| `Cross-Origin-Opener-Policy` | Isolates browsing context relationships |
| `Cross-Origin-Resource-Policy` | Restricts cross-origin resource loading |

# 🗁 CORS Headers

Cross-Origin Resource Sharing (CORS) uses HTTP headers to communicate which cross-origin requests a server permits.

Browsers enforce CORS policies when JavaScript accesses resources from another origin.

### 🖆 Access-Control-Allow-Origin

Identifies the origin permitted to access the resource.

```http
Access-Control-Allow-Origin: https://frontend.example.com
```

For public resources, a server may use:

```http
Access-Control-Allow-Origin: *
```

Wildcard origin cannot be used with credentialed CORS requests.

### 🖆 Access-Control-Allow-Methods

Identifies permitted HTTP methods for cross-origin requests.

```http
Access-Control-Allow-Methods: GET, POST, PUT, PATCH, DELETE
```

### 🖆 Access-Control-Allow-Headers

Identifies request headers that the browser is permitted to send in the actual request.

```http
Access-Control-Allow-Headers: Content-Type, Authorization
```

### 🖆 Access-Control-Allow-Credentials

Indicates whether the response permits credentialed cross-origin requests.

```http
Access-Control-Allow-Credentials: true
```

### 🖆 Access-Control-Expose-Headers

Identifies response headers that browser JavaScript is allowed to access.

```http
Access-Control-Expose-Headers: ETag, X-Request-ID
```

### 🖆 Access-Control-Max-Age

Defines how long the browser can cache a successful preflight result.

```http
Access-Control-Max-Age: 600
```

### 🖆 Access-Control-Request-Method

Sent by the browser in a CORS preflight request to identify the intended method.

```http
Access-Control-Request-Method: DELETE
```

### 🖆 Access-Control-Request-Headers

Identifies the request headers the browser intends to send.

```http
Access-Control-Request-Headers: authorization, content-type
```

### 🖆 CORS Header Summary

| Header | Direction | Purpose |
| --- | --- | --- |
| `Access-Control-Allow-Origin` | Response | Permitted origin |
| `Access-Control-Allow-Methods` | Response | Permitted methods |
| `Access-Control-Allow-Headers` | Response | Permitted request headers |
| `Access-Control-Allow-Credentials` | Response | Allows credentials |
| `Access-Control-Expose-Headers` | Response | Exposes response headers |
| `Access-Control-Max-Age` | Response | Caches preflight permission |
| `Access-Control-Request-Method` | Request | Identifies intended method |
| `Access-Control-Request-Headers` | Request | Identifies intended headers |

# 🗁 Conditional Request Headers

Conditional requests allow clients to perform operations based on the current state of a resource.

They are useful for caching, concurrency control, and preventing accidental overwrites.

### 🖆 If-Match

The request succeeds only when the current representation matches one of the supplied entity tags.

```http
If-Match: "version-42"
```

This can prevent updates based on outdated resource versions.

### 🖆 If-None-Match

The request proceeds according to whether the current representation does not match the supplied entity tag.

```http
If-None-Match: "version-42"
```

It is commonly used for cache validation.

### 🖆 If-Modified-Since

Allows a client to request a representation only if it has changed since a specified date.

```http
If-Modified-Since: Sun, 08 Nov 2026 12:00:00 GMT
```

### 🖆 If-Unmodified-Since

Allows a request to proceed only if the representation has not been modified since a specified date.

```http
If-Unmodified-Since: Sun, 08 Nov 2026 12:00:00 GMT
```

### 🖆 If-Range

Allows a client to request a range only when the representation still matches the supplied validator.

```http
If-Range: "version-42"
Range: bytes=0-999
```

If the validator matches, the server can return the requested partial content. Otherwise, it can return the complete representation.

### 🖆 Common Conditional Responses

| Status Code | Meaning |
| --- | --- |
| `304 Not Modified` | Stored representation can be reused |
| `412 Precondition Failed` | A request precondition was not satisfied |
| `206 Partial Content` | Requested range was successfully served |
| `200 OK` | Full representation returned |

# 🗁 Connection and Proxy Headers

Connection-related headers describe communication behavior and information about intermediary systems.

### 🖆 Connection

In HTTP/1.1, the `Connection` header identifies connection-specific fields and can control connection persistence.

Example:

```http
Connection: keep-alive
```

Or:

```http
Connection: close
```

Connection-specific header fields are not used in HTTP/2 or HTTP/3. The `Connection` header itself is prohibited in those versions.

### 🖆 Keep-Alive

Historically used with HTTP/1.1 persistent connections to communicate connection parameters.

```http
Keep-Alive: timeout=5, max=100
```

It is not used as an HTTP/2 or HTTP/3 connection-management mechanism.

### 🖆 Forwarded

Provides information about the original client and proxy chain.

```http
Forwarded: for=192.0.2.60;proto=https;host=example.com
```

### 🖆 X-Forwarded-For

A widely used, non-standard header that identifies client IP addresses forwarded through proxies.

```http
X-Forwarded-For: 192.0.2.60, 198.51.100.10
```

### 🖆 X-Forwarded-Proto

Identifies the original protocol used by the client.

```http
X-Forwarded-Proto: https
```

### 🖆 X-Forwarded-Host

Identifies the original host requested by the client.

```http
X-Forwarded-Host: example.com
```

Forwarding headers should only be trusted when they are set or validated by trusted proxy infrastructure.

# 🗁 Custom HTTP Headers

Applications can define their own HTTP header fields to exchange application-specific information.

Examples:

```http
X-Request-ID: req-123456
X-API-Version: 2
X-Client-Type: web
```

Common uses include:

* Request tracing.
* Application version information.
* Correlation identifiers.
* Custom API metadata.
* Debugging information.

### 🖆 The X- Prefix

Historically, custom headers frequently used an `X-` prefix.

Example:

```http
X-Request-ID: abc123
```

Modern HTTP conventions do not require custom fields to use the `X-` prefix.

An application can define:

```http
Request-ID: abc123
```

provided that the field name does not conflict with an existing standardized field and follows HTTP field-name syntax.

### 🖆 Custom Header Naming

Prefer clear, descriptive names:

```http
X-Request-ID: req-123
API-Version: 2
Correlation-ID: trace-456
```

Custom headers should be documented so that clients and servers interpret them consistently.

# 🗁 Inspecting HTTP Headers

Developers frequently inspect HTTP headers while debugging frontend applications, APIs, proxies, and servers.

### 🖆 Using cURL

Display response headers:

```bash
curl -I https://example.com
```

Display response headers and body:

```bash
curl -i https://example.com
```

Display verbose request and response information:

```bash
curl -v https://example.com
```

Send a custom request header:

```bash
curl https://api.example.com/users \
  -H "Accept: application/json"
```

Send an authorization header:

```bash
curl https://api.example.com/profile \
  -H "Authorization: Bearer example-token"
```

### 🖆 Using Browser Developer Tools

Modern browsers provide tools for inspecting HTTP communication.

In Chrome or Chromium-based browsers:

* Open Developer Tools.
* Select the Network tab.
* Reload the page.
* Select a network request.
* Open the Headers section.

You can inspect:

* General request information.
* Request URL.
* Request method.
* Status code.
* Request headers.
* Response headers.
* Query parameters.
* Request payload.

### 🖆 Using JavaScript Fetch

JavaScript can inspect response headers exposed to the browser.

```javascript
const response = await fetch("/api/users");

console.log(response.status);
console.log(response.headers.get("content-type"));
console.log(response.headers.get("cache-control"));
```

Iterate through exposed response headers:

```javascript
for (const [name, value] of response.headers) {
  console.log(`${name}: ${value}`);
}
```

Browser JavaScript cannot access every response header. Cross-origin responses expose only CORS-safelisted response headers and additional headers explicitly allowed through `Access-Control-Expose-Headers`.

### 🖆 Inspecting Request Headers on the Server

A Node.js server using the built-in HTTP module can access request headers:

```javascript
import http from "node:http";

const server = http.createServer((req, res) => {
  console.log(req.headers);

  console.log(req.headers["content-type"]);
  console.log(req.headers["authorization"]);

  res.writeHead(200, {
    "Content-Type": "application/json"
  });

  res.end(JSON.stringify({
    message: "Request received"
  }));
});

server.listen(3000);
```

Node.js normalizes incoming header names to lowercase in `req.headers`.

# 🗁 Common Mistakes

### 🖆 Confusing Content-Type and Accept

These headers serve different purposes.

```http
Content-Type: application/json
Accept: application/json
```

* `Content-Type` describes the content being sent.
* `Accept` describes the response formats the client can accept.

### 🖆 Using Content-Length as a Character Count

Content-Length represents bytes, not necessarily the number of characters.

A Unicode string may require multiple bytes per character.

### 🖆 Assuming Every Header Is Available to JavaScript

Browser security policies restrict access to certain response headers, especially across origins.

Use `Access-Control-Expose-Headers` when a cross-origin application needs access to additional response headers.

### 🖆 Combining Multiple Set-Cookie Headers

Do not combine multiple Set-Cookie fields into one comma-separated value.

Send separate `Set-Cookie` fields.

### 🖆 Trusting Forwarded Headers from Any Client

Headers such as `X-Forwarded-For` can be supplied or manipulated by clients.

Only trust them when the request comes through trusted proxy infrastructure.

### 🖆 Using Connection Headers in HTTP/2 or HTTP/3

Connection-specific headers such as `Connection` and `Keep-Alive` belong to HTTP/1.x connection management and are not valid in HTTP/2 or HTTP/3.

### 🖆 Assuming Header Order Is Significant

HTTP field ordering is generally not significant. Applications should not rely on a particular order of independent header fields.

### 🖆 Exposing Sensitive Information

Avoid placing passwords, private keys, or unnecessary sensitive information in custom headers.

Authentication credentials should be handled carefully and transmitted over HTTPS.

# 🗁 Best Practices

* Use standard HTTP headers whenever an established field already describes the required behavior.
* Use `Content-Type` to identify request and response media types.
* Use `Accept` to express response format preferences.
* Use appropriate cache directives for static assets and dynamic responses.
* Use secure cookie attributes such as `Secure`, `HttpOnly`, and an appropriate `SameSite` policy.
* Configure CORS explicitly for the origins and methods your application needs.
* Use security headers such as CSP and HSTS where appropriate.
* Avoid exposing unnecessary server implementation details.
* Validate and sanitize all untrusted request data.
* Do not trust forwarding headers unless they originate from trusted proxies.
* Avoid custom headers when a standardized header already exists.
* Document custom headers as part of your API contract.
* Remember that browser JavaScript has restricted access to certain response headers.
* Use HTTPS when transmitting authentication credentials and sensitive information.
* Follow HTTP version requirements when configuring connection-related behavior.

# 🗁 Practical Exercises

### 🖆 Exercise: Analyze an HTTP Request

Study the following request:

```http
POST /api/products HTTP/1.1
Host: api.example.com
Content-Type: application/json
Accept: application/json
Authorization: Bearer example-token
User-Agent: Mozilla/5.0

{
  "name": "Keyboard",
  "price": 75
}
```

Identify:

* The request method.
* The target resource.
* The content type.
* The accepted response format.
* The authentication mechanism.
* The request body.

### 🖆 Exercise: Analyze an HTTP Response

Study this response:

```http
HTTP/1.1 200 OK
Content-Type: application/json
Cache-Control: public, max-age=600
ETag: "product-v1"
X-Request-ID: req-123

{
  "id": 1,
  "name": "Keyboard"
}
```

Explain the purpose of each response header.

### 🖆 Exercise: Implement Custom Headers

Create a cURL request containing:

```http
Accept: application/json
Authorization: Bearer test-token
X-Request-ID: exercise-001
```

Use a local or test API and inspect the server response.

### 🖆 Exercise: Implement Cache Validation

Build a small API that returns an ETag.

Send a second request using `If-None-Match` and return `304 Not Modified` when the representation has not changed.

### 🖆 Exercise: Configure Security Headers

Configure a development server to return:

```http
X-Content-Type-Options: nosniff
Referrer-Policy: strict-origin-when-cross-origin
Content-Security-Policy: default-src 'self'
```

Inspect the response using browser Developer Tools or cURL.

# 🗁 Interview Questions

### 🖆 What are HTTP headers?

HTTP headers are key-value fields that carry metadata about HTTP requests and responses.

### 🖆 What is the difference between request headers and response headers?

Request headers are sent by clients to describe requests and client preferences. Response headers are sent by servers to describe responses and server instructions.

### 🖆 What is the difference between Content-Type and Accept?

Content-Type identifies the media type of the message content. Accept specifies the response media types the client can process.

### 🖆 What is the purpose of the Authorization header?

It carries credentials used to authenticate a request to a protected resource.

### 🖆 What is the difference between Cookie and Set-Cookie?

Set-Cookie is sent by the server to create or update cookies. Cookie is sent by the client to return applicable stored cookies.

### 🖆 What is Cache-Control?

Cache-Control defines caching directives that control how HTTP responses are stored and reused.

### 🖆 What is an ETag?

An ETag is an identifier for a particular version of a resource representation. It supports cache validation and conditional requests.

### 🖆 What is the purpose of the Location header?

Location identifies a resource location or redirect target. It is commonly used in redirect responses and resource creation responses.

### 🖆 What is the purpose of the Origin header?

Origin identifies the origin of a request. It is particularly important for browser CORS communication.

### 🖆 What is the difference between Forwarded and X-Forwarded-For?

Forwarded is a standardized HTTP field for proxy-related information. X-Forwarded-For is a widely used, non-standard field that carries forwarded client IP information.

### 🖆 What is the purpose of the Content-Security-Policy header?

CSP defines which sources a browser is allowed to load resources from and helps reduce content injection risks.

### 🖆 What is HSTS?

HTTP Strict Transport Security is a policy that instructs browsers to use HTTPS for future requests to a domain.

### 🖆 What is the Vary header?

Vary identifies request headers that influenced the selected response representation, helping caches distinguish variants.

### 🖆 Can JavaScript access all response headers?

No. Browser JavaScript can access exposed response headers, but cross-origin responses restrict access to safelisted headers and those explicitly exposed by the server.

### 🖆 What is the difference between Content-Length and Transfer-Encoding?

Content-Length describes the content length in bytes when applicable. Transfer-Encoding is an HTTP/1.1 message transfer mechanism, historically used for chunked transfer coding.

# 🗁 References

* [RFC 9110 — HTTP Semantics](https://www.rfc-editor.org/rfc/rfc9110)
* [RFC 9111 — HTTP Caching](https://www.rfc-editor.org/rfc/rfc9111)
* [RFC 9112 — HTTP/1.1](https://www.rfc-editor.org/rfc/rfc9112)
* [RFC 9113 — HTTP/2](https://www.rfc-editor.org/rfc/rfc9113)
* [RFC 9114 — HTTP/3](https://www.rfc-editor.org/rfc/rfc9114)
* [MDN — HTTP Headers](https://developer.mozilla.org/en-US/docs/Web/HTTP/Reference/Headers)
* [MDN — HTTP Headers Reference](https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers)
* [MDN — HTTP Caching](https://developer.mozilla.org/en-US/docs/Web/HTTP/Guides/Caching)
* [MDN — CORS](https://developer.mozilla.org/en-US/docs/Web/HTTP/Guides/CORS)
* [MDN — Content Security Policy](https://developer.mozilla.org/en-US/docs/Web/HTTP/Guides/CSP)
