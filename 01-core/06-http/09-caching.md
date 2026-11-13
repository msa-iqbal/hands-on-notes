# HTTP Caching

HTTP caching is a mechanism that stores copies of HTTP responses so they can be reused for subsequent requests instead of repeatedly retrieving the same resources from the origin server.

Caching improves web application performance by reducing:

* Network requests.
* Server processing.
* Response latency.
* Bandwidth consumption.
* Infrastructure costs.

HTTP caching is especially useful for resources that do not change frequently, such as:

* Images.
* CSS files.
* JavaScript files.
* Fonts.
* API responses.
* HTML documents.
* Static documents.

HTTP defines caching behavior through request and response headers that control how responses are stored, reused, validated, and expired.

### 🖆 A Simple Example

A browser requests an image:

```http
GET /images/logo.png HTTP/1.1
Host: example.com
```

The server responds:

```http
HTTP/1.1 200 OK
Content-Type: image/png
Cache-Control: max-age=3600

[Image Data]
```

The browser can store the response for 3600 seconds.

If the browser requests the same resource again while the cached response is fresh, it can reuse the stored response without contacting the server.

# 🗁 Why HTTP Caching Is Important

Without caching, browsers may repeatedly download the same resources.

Consider a website containing:

```text
index.html
styles.css
app.js
logo.png
banner.jpg
```

Every page visit may require downloading several resources.

If those resources are unchanged, repeatedly transferring them wastes bandwidth and increases response time.

Caching solves this problem by allowing reusable responses to be stored closer to the client.

### 🖆 Performance Benefits

| Benefit | Explanation |
| --- | --- |
| Faster loading | Cached resources can be reused locally |
| Lower latency | Fewer network round trips |
| Reduced bandwidth | Less data is transferred |
| Lower server load | Fewer requests reach the origin |
| Better scalability | Infrastructure handles more users |
| Improved availability | Some cached resources remain usable during temporary network problems |

### 🖆 Example Without Caching

```text
Browser
   |
   | GET /styles.css
   v
Origin Server
   |
   | 200 OK + CSS
   v
Browser

Next Visit:

Browser
   |
   | GET /styles.css
   v
Origin Server
   |
   | 200 OK + CSS
   v
Browser
```

The browser downloads the same CSS file again.

### 🖆 Example With Caching

```text
First Request:

Browser
   |
   | GET /styles.css
   v
Origin Server
   |
   | 200 OK + CSS
   v
Browser
   |
   | Store Response in Cache


Next Request:

Browser
   |
   | Check Local Cache
   v
Fresh Cached Response
   |
   | Reuse CSS
   v
Browser
```

The second request does not need to reach the origin server.

# 🗁 Types of HTTP Caches

HTTP caching can happen at different locations throughout the network.

### 🖆 Browser Cache

A browser cache stores HTTP responses locally on the user's device.

Examples:

* Images.
* JavaScript files.
* CSS files.
* Fonts.
* Other cacheable resources.

Advantages:

* Reuses resources without network requests.
* Reduces page loading time.
* Reduces bandwidth usage.

Browser caching is generally private to the individual user's browser profile.

### 🖆 Shared Cache

A shared cache is used by multiple users.

Examples include:

* Reverse proxies.
* CDN edge caches.
* Corporate proxy caches.

A shared cache can reuse an eligible response for multiple requests.

```text
             Users
          /    |    \
         v     v     v
      User A User B User C
         \     |     /
          v    v    v
          Shared Cache
                |
                v
           Origin Server
```

Shared caching can significantly reduce origin server traffic.

### 🖆 Proxy Cache

A proxy cache stores responses while acting as an intermediary between clients and servers.

It may be deployed inside:

* Corporate networks.
* Internet service provider networks.
* Enterprise infrastructure.

A proxy cache can reuse responses for multiple clients when HTTP caching rules permit.

### 🖆 Reverse Proxy Cache

A reverse proxy sits in front of backend servers and can cache selected responses.

Example architecture:

```text
Browser
   |
   v
Reverse Proxy Cache
   |
   v
Application Server
   |
   v
Database
```

The reverse proxy can return cached content without sending every request to the application server.

### 🖆 CDN Cache

A Content Delivery Network (CDN) caches content at geographically distributed edge locations.

Examples include:

* Cloudflare.
* Amazon CloudFront.
* Fastly.
* Akamai.

A user can receive cached content from a nearby CDN location instead of retrieving it directly from the origin server.

```text
              Origin Server
                    |
             CDN Distribution
             /      |       \
            v       v        v
        Edge A   Edge B    Edge C
           |        |         |
           v        v         v
        Users    Users     Users
```

CDN caching improves global content delivery and reduces origin infrastructure load.

# 🗁 Cache-Control Header

`Cache-Control` is the primary HTTP header used to define caching policies.

It supports directives that control whether a response can be stored, how long it remains fresh, and whether it must be validated before reuse.

### 🖆 max-age

`max-age` defines how long a response remains fresh, in seconds.

Example:

```http
Cache-Control: max-age=3600
```

The response can be reused while it remains fresh for approximately one hour.

### 🖆 no-cache

`no-cache` allows a response to be stored but requires successful validation before reuse.

Example:

```http
Cache-Control: no-cache
```

This does not mean the browser cannot store the response.

Instead, it must check with the server before reusing the stored response.

### 🖆 no-store

`no-store` instructs caches not to store the response.

Example:

```http
Cache-Control: no-store
```

This directive is useful for responses that should not be retained by HTTP caches.

It is commonly used for sensitive responses, although it does not replace application-level security controls.

### 🖆 public

`public` indicates that a response can be stored by shared caches, even in situations where it might otherwise be restricted.

Example:

```http
Cache-Control: public, max-age=3600
```

Use this directive only when shared caching is appropriate for the response.

### 🖆 private

`private` indicates that the response is intended for a single user and must not be stored by shared caches.

Example:

```http
Cache-Control: private, max-age=600
```

A browser's private cache can store the response, but a shared cache must not.

### 🖆 must-revalidate

`must-revalidate` instructs caches to validate a stale response before reusing it.

Example:

```http
Cache-Control: max-age=3600, must-revalidate
```

Once the response becomes stale, a cache must successfully validate it before reusing it.

### 🖆 proxy-revalidate

`proxy-revalidate` applies the revalidation requirement to shared caches.

Example:

```http
Cache-Control: max-age=3600, proxy-revalidate
```

### 🖆 s-maxage

`s-maxage` defines the freshness lifetime for shared caches.

Example:

```http
Cache-Control: public, max-age=600, s-maxage=3600
```

In this example:

* Private caches use a freshness lifetime of 600 seconds.
* Shared caches use a freshness lifetime of 3600 seconds.

`s-maxage` overrides `max-age` for shared caches.

### 🖆 no-transform

`no-transform` instructs intermediaries not to transform the response representation.

Example:

```http
Cache-Control: no-transform
```

This can be useful when intermediaries should preserve the representation as provided by the origin.

### 🖆 stale-while-revalidate

`stale-while-revalidate` allows a cache to serve a stale response temporarily while it revalidates the resource in the background.

Example:

```http
Cache-Control: max-age=60, stale-while-revalidate=300
```

The response is fresh for 60 seconds.

After that, the cache may serve the stale response for up to another 300 seconds while attempting to obtain a fresh response.

### 🖆 stale-if-error

`stale-if-error` allows a cache to serve a stale response when an error occurs while attempting to obtain a fresh response.

Example:

```http
Cache-Control: max-age=60, stale-if-error=600
```

This can improve availability when the origin server is temporarily unavailable.

Support for these stale-serving extensions depends on the cache implementation.

### 🖆 Cache-Control Directive Summary

| Directive | Purpose |
| --- | --- |
| `max-age` | Defines freshness lifetime |
| `no-cache` | Requires validation before reuse |
| `no-store` | Prevents HTTP cache storage |
| `public` | Allows shared-cache storage |
| `private` | Restricts storage to private caches |
| `must-revalidate` | Requires validation when stale |
| `proxy-revalidate` | Requires shared-cache validation when stale |
| `s-maxage` | Defines shared-cache freshness |
| `no-transform` | Prevents intermediary transformation |
| `stale-while-revalidate` | Allows temporary stale serving during background validation |
| `stale-if-error` | Allows stale serving when an error occurs |

# 🗁 Cache Validation

A cached response does not necessarily need to be downloaded again when it becomes stale.

HTTP supports conditional requests that allow a client to ask whether its cached representation is still valid.

The server can respond without sending the full resource again.

Two important validation mechanisms are:

* `ETag` and `If-None-Match`.
* `Last-Modified` and `If-Modified-Since`.

### 🖆 ETag

An `ETag` is a validator representing a specific version of a resource.

Example response:

```http
HTTP/1.1 200 OK
Content-Type: text/css
Cache-Control: max-age=0
ETag: "v1-abc123"

body {
    margin: 0;
}
```

The browser stores the response and its ETag.

When it needs to validate the resource, it sends:

```http
GET /styles.css HTTP/1.1
Host: example.com
If-None-Match: "v1-abc123"
```

The server compares the supplied validator with the current representation.

If the resource has not changed:

```http
HTTP/1.1 304 Not Modified
ETag: "v1-abc123"
```

The browser can reuse its stored response body.

If the resource has changed, the server returns a new representation:

```http
HTTP/1.1 200 OK
Content-Type: text/css
ETag: "v2-def456"

body {
    margin: 0;
    padding: 0;
}
```

### 🖆 Strong and Weak ETags

ETags can be strong or weak.

**Strong ETag:**

```http
ETag: "version-123"
```

A strong validator indicates that the representation is byte-for-byte equivalent.

**Weak ETag:**

```http
ETag: W/"version-123"
```

A weak validator indicates semantic equivalence even if the representation is not byte-for-byte identical.

Weak validators are useful when minor representation differences do not matter for cache validation.

### 🖆 Last-Modified

`Last-Modified` indicates when the server believes a resource was last modified.

Example response:

```http
HTTP/1.1 200 OK
Last-Modified: Mon, 09 Nov 2026 08:00:00 GMT
Cache-Control: max-age=0

[Resource Data]
```

The browser can later send:

```http
GET /document.pdf HTTP/1.1
Host: example.com
If-Modified-Since: Mon, 09 Nov 2026 08:00:00 GMT
```

If the resource has not changed since that time, the server can respond:

```http
HTTP/1.1 304 Not Modified
```

Otherwise, it returns the updated representation.

### 🖆 ETag vs Last-Modified

| Feature | ETag | Last-Modified |
| --- | --- | --- |
| Validator | Entity tag | Modification timestamp |
| Precision | Can distinguish versions more precisely | Usually limited to seconds |
| Comparison | Entity-tag comparison | Date comparison |
| Typical use | Version validation | Modification-time validation |
| Strength | Strong or weak | Time-based validator |

When both validators are available, `If-None-Match` takes precedence over `If-Modified-Since` for cache validation.

# 🗁 Conditional Requests

Conditional requests allow HTTP operations to depend on resource state or validators.

Caching commonly uses:

* `If-None-Match`.
* `If-Modified-Since`.

Other conditional headers include:

* `If-Match`.
* `If-Unmodified-Since`.
* `If-Range`.

### 🖆 If-None-Match

The client asks the server to return the resource only if the supplied entity tag does not match the current representation.

Example:

```http
GET /app.js HTTP/1.1
If-None-Match: "release-10"
```

If the representation is unchanged, the server can return `304 Not Modified`.

### 🖆 If-Modified-Since

The client asks whether the resource has changed since the supplied date.

Example:

```http
GET /report.pdf HTTP/1.1
If-Modified-Since: Mon, 09 Nov 2026 08:00:00 GMT
```

If it has not changed, the server can return `304 Not Modified`.

### 🖆 304 Not Modified

`304 Not Modified` is a response indicating that a conditional GET or HEAD request does not require a new representation body.

Example:

```http
HTTP/1.1 304 Not Modified
ETag: "release-10"
Cache-Control: max-age=3600
```

The client reuses the previously stored response body.

A `304` response does not contain the resource representation body.

This reduces unnecessary data transfer.

# 🗁 Cache-Control vs Expires

Both `Cache-Control` and `Expires` can define response freshness.

### 🖆 Expires Header

`Expires` defines an absolute expiration date.

Example:

```http
Expires: Mon, 09 Nov 2026 10:00:00 GMT
```

The response can be reused while it remains fresh according to that expiration time.

### 🖆 Cache-Control max-age

`max-age` defines a relative freshness lifetime.

Example:

```http
Cache-Control: max-age=3600
```

The response remains fresh for approximately 3600 seconds from the response's generation time, accounting for HTTP cache age calculations.

### 🖆 Comparison

| Feature | Cache-Control | Expires |
| --- | --- | --- |
| Time definition | Relative lifetime | Absolute date |
| Header type | Response header | Response header |
| Modern usage | Preferred | Legacy compatibility |
| Priority | `max-age` takes precedence | Used when `max-age` is absent |

When both are present, `Cache-Control: max-age` takes precedence over `Expires` for freshness calculation.

# 🗁 The Age Header

The `Age` header indicates the approximate time in seconds that a response has spent in a shared cache.

Example:

```http
HTTP/1.1 200 OK
Cache-Control: public, max-age=3600
Age: 120
```

This indicates that the response has spent approximately 120 seconds in a shared cache.

The `Age` value contributes to determining how much freshness lifetime remains.

For example, a response with a freshness lifetime of 3600 seconds and an age of 120 seconds has approximately 3480 seconds of freshness remaining.

# 🗁 The Vary Header

The `Vary` header identifies request headers that influence the selected response representation.

This is important when different request headers produce different content.

Example:

```http
HTTP/1.1 200 OK
Content-Type: text/html
Vary: Accept-Language
Cache-Control: max-age=3600
```

The server may return different content based on the `Accept-Language` request header.

```http
GET / HTTP/1.1
Accept-Language: en
```

Response:

```text
English content
```

Another request:

```http
GET / HTTP/1.1
Accept-Language: bn
```

Response:

```text
Bengali content
```

The cache uses the `Vary` information when determining whether a stored response can satisfy a new request.

### 🖆 Common Vary Values

```http
Vary: Accept-Encoding
```

Useful when the server provides different content encodings.

```http
Vary: Accept-Language
```

Useful when content varies by language.

```http
Vary: Accept
```

Useful when the server provides different representations based on accepted media types.

### 🖆 Vary: *

```http
Vary: *
```

Indicates that the request contains characteristics that cannot be adequately represented by ordinary header-field matching for cache selection.

A response containing `Vary: *` cannot be reused to satisfy a later request through ordinary cache matching.

# 🗁 Freshness and Stale Responses

HTTP caches distinguish between fresh and stale responses.

### 🖆 Fresh Response

A response is fresh when its current age does not exceed its freshness lifetime.

Example:

```http
Cache-Control: max-age=3600
```

If the response is 300 seconds old, it is still fresh.

The cache can reuse it without contacting the origin server.

### 🖆 Stale Response

A response becomes stale when its freshness lifetime has expired.

For example:

```http
Cache-Control: max-age=60
```

After the response has exceeded its 60-second freshness lifetime, it is stale.

A cache may need to:

* Validate the response with the origin server.
* Fetch a new representation.
* Serve stale content when permitted by applicable directives or cache policy.

### 🖆 Freshness Lifecycle

```text
       Server Returns Response
                  |
                  v
             Store Response
                  |
                  v
            Fresh Response
                  |
          Freshness Expires
                  |
                  v
            Stale Response
                  |
          +-------+-------+
          |               |
          v               v
       Validate       Fetch New
          |           Representation
          v
     +----+----+
     |         |
     v         v
  304 Not    200 OK
  Modified   Updated Data
     |         |
     v         v
  Reuse      Replace Cache
  Stored
  Response
```

# 🗁 Cache Invalidation

Cache invalidation is the process of ensuring that outdated cached content is not incorrectly reused.

It is one of the most challenging parts of caching.

Consider a website that updates a product price.

The origin server has the new price, but a browser or CDN may still hold an older response.

### 🖆 Expiration-Based Invalidation

A response becomes stale when its freshness lifetime expires.

Example:

```http
Cache-Control: max-age=300
```

The cache must account for freshness and validation rules after the response becomes stale.

### 🖆 Validator-Based Invalidation

ETags and Last-Modified allow caches to verify whether stored content remains valid.

The cache can reuse the old response when the server confirms that it has not changed.

### 🖆 URL Versioning

A common strategy for static assets is to include a version or content hash in the URL.

Example:

```text
/assets/app.abc123.js
/assets/styles.def456.css
```

When the content changes, the filename changes.

The application references the new URL, while old cached files can safely remain available until their expiration.

### 🖆 Cache Purging

CDNs and reverse proxies may provide cache purge or invalidation mechanisms.

Examples include:

* Purging a specific URL.
* Purging a group of URLs by tag.
* Clearing a cache namespace.
* Invalidating an entire distribution.

The available mechanisms depend on the caching platform.

### 🖆 Cache Invalidation Strategies

| Strategy | Description |
| --- | --- |
| Short TTL | Allow content to expire relatively quickly |
| Conditional validation | Use ETag or Last-Modified |
| Versioned URLs | Change URLs when content changes |
| CDN purge | Explicitly remove cached responses |
| Cache tags | Invalidate related groups of resources |

# 🗁 Caching Different HTTP Resources

Different resources require different caching policies.

### 🖆 Static Assets

Examples:

* JavaScript.
* CSS.
* Images.
* Fonts.

Static assets with content-hashed filenames can use long cache lifetimes.

Example:

```http
Cache-Control: public, max-age=31536000, immutable
```

The `immutable` extension indicates that the representation is not expected to change during its freshness lifetime.

Use it only when the URL identifies a versioned, immutable resource.

### 🖆 HTML Documents

HTML often references other resources and may change when an application is deployed.

A common policy is:

```http
Cache-Control: no-cache
```

The browser can store the HTML document but must validate it before reuse.

This helps ensure that users receive updated references to static assets.

The appropriate policy depends on the application's deployment and update strategy.

### 🖆 API Responses

API responses may contain dynamic data.

Example:

```http
Cache-Control: private, max-age=60
```

This permits private caching for 60 seconds while preventing shared caches from storing the response.

For frequently changing or sensitive API data, use a stricter policy.

### 🖆 Sensitive Responses

For sensitive responses that should not be retained by HTTP caches:

```http
Cache-Control: no-store
```

Examples may include:

* Highly sensitive account information.
* Private financial information.
* Responses containing confidential data.

`no-store` controls HTTP caching. It does not guarantee that data cannot be stored by application code, screenshots, browser extensions, or other mechanisms.

# 🗁 HTTP Caching in HTTP/1.1, HTTP/2, and HTTP/3

HTTP caching semantics are largely independent of the underlying HTTP version.

HTTP/1.1, HTTP/2, and HTTP/3 use HTTP caching concepts such as:

* Cache-Control.
* ETag.
* Last-Modified.
* Conditional requests.
* Vary.
* Freshness and validation.

The major differences between these HTTP versions involve transport and protocol behavior rather than the fundamental caching model.

### 🖆 HTTP/1.1

HTTP/1.1 supports HTTP caching through standard request and response headers.

### 🖆 HTTP/2

HTTP/2 uses binary framing and multiplexing, but HTTP caching semantics remain substantially the same.

### 🖆 HTTP/3

HTTP/3 uses QUIC for transport and also follows HTTP caching semantics.

The same cache policies can be applied regardless of whether a resource is delivered through HTTP/1.1, HTTP/2, or HTTP/3.

# 🗁 Practical HTTP Caching Examples

### 🖆 Cache a Static Image

Server response:

```http
HTTP/1.1 200 OK
Content-Type: image/png
Cache-Control: public, max-age=86400

[Image Data]
```

The image can be stored by eligible private and shared caches for one day.

### 🖆 Prevent Cache Storage

```http
HTTP/1.1 200 OK
Content-Type: application/json
Cache-Control: no-store

{
  "account": "private"
}
```

This instructs HTTP caches not to store the response.

### 🖆 Cache Privately

```http
HTTP/1.1 200 OK
Content-Type: application/json
Cache-Control: private, max-age=300

{
  "username": "iqbal"
}
```

This permits private caching for five minutes while preventing shared-cache storage.

### 🖆 Revalidate Every Time

```http
HTTP/1.1 200 OK
Content-Type: text/html
Cache-Control: no-cache
ETag: "html-v1"

[HTML Data]
```

The browser can store the response but must validate it before reuse.

### 🖆 Cache with ETag

Initial response:

```http
HTTP/1.1 200 OK
ETag: "version-1"
Cache-Control: no-cache

[Resource Data]
```

Conditional request:

```http
GET /resource HTTP/1.1
If-None-Match: "version-1"
```

If unchanged:

```http
HTTP/1.1 304 Not Modified
ETag: "version-1"
```

The client reuses the stored response body.

# 🗁 Debugging HTTP Cache Behavior

Caching issues can cause users to receive outdated content or unexpected application behavior.

Browser Developer Tools and command-line tools help identify these problems.

### 🖆 Inspect Response Headers

Use cURL to inspect response headers:

```bash
curl -I https://example.com
```

Look for:

```http
Cache-Control
Expires
ETag
Last-Modified
Age
Vary
Date
```

### 🖆 Inspect Conditional Requests

Send a conditional request manually:

```bash
curl -i \
  -H 'If-None-Match: "version-1"' \
  https://example.com/resource
```

Observe whether the server returns:

* `304 Not Modified`.
* `200 OK` with an updated representation.

### 🖆 Inspect Browser Cache

Open browser Developer Tools.

Navigate to the Network panel.

Inspect a requested resource and review:

* Status code.
* Request headers.
* Response headers.
* Transferred size.
* Whether the resource was served from memory cache or disk cache.

Browser interfaces differ, but the Network panel provides useful information about resource delivery.

### 🖆 Disable Browser Cache During Development

In Chromium-based browsers:

```text
Developer Tools
    |
    +-- Network
            |
            +-- Disable cache
```

This option generally applies while Developer Tools is open.

It is useful when testing resource updates and cache behavior.

# 🗁 Common Caching Mistakes

### 🖆 Confusing no-cache with no-store

`no-cache` allows storage but requires validation before reuse.

`no-store` instructs caches not to store the response.

**Better approach:** Choose the directive based on whether storage is permitted.

### 🖆 Using Long Cache Lifetimes for Frequently Changing Files

A long freshness lifetime can cause users to receive outdated content.

**Better approach:** Use short freshness lifetimes or versioned URLs for frequently changing resources.

### 🖆 Forgetting Vary

If a response changes according to a request header but the cache does not account for that header, the wrong representation may be reused.

**Better approach:** Set an appropriate `Vary` header.

### 🖆 Caching User-Specific Responses Publicly

A shared cache may expose one user's response to another user if the response is incorrectly configured for shared caching.

**Better approach:** Use `private` or `no-store` when shared caching is inappropriate.

### 🖆 Assuming Every 200 Response Is Cached

A successful response is not automatically stored by every cache.

Cacheability depends on HTTP semantics, headers, request method, status code, and implementation-specific rules.

**Better approach:** Inspect caching headers and cache behavior.

### 🖆 Using ETags Incorrectly

An ETag that does not change when the representation changes can cause incorrect validation results.

**Better approach:** Generate validators consistently for the actual representation being served.

### 🖆 Forgetting Cache Invalidation During Deployment

Users may continue receiving old resources after a deployment.

**Better approach:** Use content-hashed asset filenames and an appropriate HTML caching strategy.

### 🖆 Caching Sensitive API Responses

Incorrect shared caching can expose private information.

**Better approach:** Carefully configure `private` or `no-store` according to the data's sensitivity.

# 🗁 Best Practices

* Use `Cache-Control` to define explicit caching policies.
* Use long-lived caching for versioned static assets.
* Use `immutable` only for resources that do not change at the same URL.
* Use `no-cache` when a response may be stored but must be validated before reuse.
* Use `no-store` when HTTP caches must not retain a response.
* Use `private` for user-specific responses that may be cached by the browser but not shared caches.
* Use ETags for efficient resource validation.
* Use Last-Modified when modification timestamps are useful validators.
* Set `Vary` when response representations depend on request headers.
* Configure CDN and reverse proxy caching deliberately.
* Avoid publicly caching personalized or sensitive responses.
* Use content hashing for static assets.
* Design cache invalidation as part of deployment workflows.
* Test caching behavior in browser Developer Tools.
* Monitor cache hit ratios and origin request volume.
* Consider stale-serving policies for resources where availability is important.

# 🗁 Practical Exercises

### 🖆 Exercise: Set a Cache-Control Header

Configure a server to return:

```http
Cache-Control: public, max-age=3600
```

Verify the response using:

```bash
curl -I https://example.com
```

### 🖆 Exercise: Implement ETag Validation

Create an endpoint that returns an ETag.

Send a second request using `If-None-Match`.

Verify that an unchanged resource returns `304 Not Modified`.

### 🖆 Exercise: Test Last-Modified

Configure a resource with a `Last-Modified` header.

Send a request containing `If-Modified-Since`.

Observe the server's response.

### 🖆 Exercise: Compare no-cache and no-store

Create two endpoints:

```text
/cache-no-cache
/cache-no-store
```

Configure one with `Cache-Control: no-cache` and the other with `Cache-Control: no-store`.

Inspect browser caching behavior.

### 🖆 Exercise: Configure Static Asset Caching

Serve a CSS file with:

```http
Cache-Control: public, max-age=31536000, immutable
```

Use a versioned filename.

Verify that the browser reuses the resource.

### 🖆 Exercise: Test Vary

Create an endpoint that returns different content based on `Accept-Language`.

Set:

```http
Vary: Accept-Language
```

Test requests using different language headers.

### 🖆 Exercise: Inspect CDN Caching

Deploy a static resource behind a CDN.

Inspect its response headers and identify:

* Cache-Control.
* Age.
* ETag.
* CDN-specific cache status headers, if available.

Compare the first request with subsequent requests.

# 🗁 Interview Questions

### 🖆 What is HTTP caching?

HTTP caching stores reusable HTTP responses so clients and intermediaries can avoid repeatedly downloading the same resources.

### 🖆 Why is caching important?

It improves performance, reduces latency, decreases bandwidth usage, and reduces origin server load.

### 🖆 What is Cache-Control?

It is an HTTP header that defines caching policies through directives such as `max-age`, `no-cache`, `no-store`, `public`, and `private`.

### 🖆 What is the difference between no-cache and no-store?

`no-cache` permits storage but requires validation before reuse.

`no-store` instructs caches not to store the response.

### 🖆 What is the difference between private and public?

`private` restricts storage to private caches.

`public` allows eligible shared caches to store the response.

### 🖆 What is an ETag?

An ETag is a validator identifying a particular version of a resource representation.

### 🖆 What is the purpose of If-None-Match?

It allows a client to ask whether the current representation matches a previously stored ETag.

### 🖆 What is Last-Modified?

It indicates when a resource was last modified according to the server.

### 🖆 What is 304 Not Modified?

It indicates that a conditional request does not require a new representation body. The client can reuse its stored response.

### 🖆 What is the difference between ETag and Last-Modified?

ETag uses an entity tag to validate a representation, while Last-Modified uses a modification timestamp.

### 🖆 What is the Vary header?

It identifies request headers that affect the selected response representation and therefore influence cache matching.

### 🖆 What is a CDN cache?

A CDN cache stores content at distributed edge locations to serve users from locations closer to them.

### 🖆 What is cache invalidation?

Cache invalidation is the process of preventing outdated cached content from being incorrectly reused.

### 🖆 What is the difference between browser caching and shared caching?

Browser caching stores responses for an individual user's browser.

Shared caching stores reusable responses for multiple users, such as in a CDN or proxy.

### 🖆 What is the Age header?

It indicates the approximate time in seconds that a response has spent in a shared cache.

### 🖆 What is stale-while-revalidate?

It allows a cache to serve a stale response temporarily while fetching or validating an updated response in the background.

# 🗁 References

* [MDN — HTTP Caching](https://developer.mozilla.org/en-US/docs/Web/HTTP/Guides/Caching)
* [MDN — Cache-Control](https://developer.mozilla.org/en-US/docs/Web/HTTP/Reference/Headers/Cache-Control)
* [MDN — ETag](https://developer.mozilla.org/en-US/docs/Web/HTTP/Reference/Headers/ETag)
* [MDN — Last-Modified](https://developer.mozilla.org/en-US/docs/Web/HTTP/Reference/Headers/Last-Modified)
* [MDN — 304 Not Modified](https://developer.mozilla.org/en-US/docs/Web/HTTP/Reference/Status/304)
* [RFC 9111 — HTTP Caching](https://www.rfc-editor.org/rfc/rfc9111)
* [RFC 9110 — HTTP Semantics](https://www.rfc-editor.org/rfc/rfc9110)
* [Cloudflare — Caching](https://developers.cloudflare.com/cache/)
