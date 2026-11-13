# HTTP Status Codes

HTTP status codes are three-digit numbers returned by a server to indicate the result of an HTTP request.

They communicate whether a request was successful, whether additional action is required, or whether an error occurred.

For example:

```http
HTTP/1.1 200 OK
Content-Type: application/json

{
  "message": "Request successful"
}
```

In this response:

* `200` is the HTTP status code.
* `OK` is its reason phrase.
* `Content-Type` describes the response body.

HTTP status codes are essential for web development, API design, debugging, caching, authentication, and error handling.

# 🗁 Understanding HTTP Status Codes

An HTTP status code indicates the outcome of a request.

A server includes a status code in the response status line in HTTP/1.1.

Example:

```http
HTTP/1.1 200 OK
```

The status code communicates the result independently of the response body.

For example, an API might return:

```http
HTTP/1.1 404 Not Found
Content-Type: application/json

{
  "error": "User not found"
}
```

The status code tells the client that the requested resource was not found. The response body provides additional application-specific information.

### 🖆 Status Code Structure

HTTP status codes contain three digits.

The first digit identifies the response category:

```text
1xx → Informational
2xx → Successful
3xx → Redirection
4xx → Client Error
5xx → Server Error
```

For example:

```text
200 → Success
301 → Permanent Redirect
400 → Bad Request
404 → Not Found
500 → Internal Server Error
```

### 🖆 Reason Phrases

HTTP/1.1 responses traditionally include a reason phrase after the status code.

```http
HTTP/1.1 404 Not Found
```

Here, `Not Found` is the reason phrase.

Reason phrases are not the primary mechanism for interpreting responses. Clients should rely on the numeric status code.

HTTP/2 and HTTP/3 do not use the HTTP/1.1 textual status line. They communicate the status using the `:status` pseudo-header.

Example:

```text
:status: 200
```

# 🗁 Status Code Categories

HTTP status codes are divided into five categories.

| Category | Range | Meaning |
| --- | --- | --- |
| Informational | 100–199 | Request received; communication continues |
| Successful | 200–299 | Request successfully processed |
| Redirection | 300–399 | Additional action is needed |
| Client Error | 400–499 | Request cannot be fulfilled due to a client-side issue |
| Server Error | 500–599 | Server failed to fulfill an apparently valid request |

### 🖆 Informational Responses

Indicate that the request has been received and the process is continuing.

Example:

```http
HTTP/1.1 100 Continue
```

### 🖆 Successful Responses

Indicate that the request was successfully received, understood, and processed.

Example:

```http
HTTP/1.1 200 OK
```

### 🖆 Redirection Responses

Indicate that the client needs to take additional action, commonly by requesting another URL.

Example:

```http
HTTP/1.1 301 Moved Permanently
Location: https://example.com/new-page
```

### 🖆 Client Error Responses

Indicate that the request cannot be fulfilled because of a problem with the request or the client's authorization.

Example:

```http
HTTP/1.1 404 Not Found
```

### 🖆 Server Error Responses

Indicate that the server failed to fulfill an apparently valid request.

Example:

```http
HTTP/1.1 500 Internal Server Error
```

# 🗁 1xx Informational Responses

Informational status codes indicate that the server has received the request and the communication process is continuing.

They are interim responses rather than final results.

### 🖆 100 Continue

Indicates that the server has received the request headers and the client can continue sending the request body.

Example:

```http
HTTP/1.1 100 Continue
```

This is useful when a client wants to avoid uploading a large request body if the server is likely to reject the request based on its headers.

The client can send:

```http
POST /upload HTTP/1.1
Host: example.com
Content-Length: 5000000
Expect: 100-continue
```

If the server responds with `100 Continue`, the client proceeds with the body.

### 🖆 101 Switching Protocols

Indicates that the server agrees to switch protocols as requested by the client.

Example:

```http
HTTP/1.1 101 Switching Protocols
Upgrade: websocket
Connection: Upgrade
```

This is commonly associated with the traditional HTTP/1.1 WebSocket handshake.

### 🖆 102 Processing

A WebDAV status code indicating that the server has received and is processing the request, but no final response is available yet.

### 🖆 103 Early Hints

Allows the server to send preliminary response headers before the final response.

It can help browsers begin loading important resources earlier.

Example:

```http
HTTP/1.1 103 Early Hints
Link: </styles.css>; rel=preload; as=style
```

### 🖆 Informational Status Summary

| Code | Name | Purpose |
| --- | --- | --- |
| 100 | Continue | Client can continue sending request content |
| 101 | Switching Protocols | Protocol upgrade accepted |
| 102 | Processing | Server is still processing |
| 103 | Early Hints | Sends preliminary response information |

# 🗁 2xx Successful Responses

Successful status codes indicate that the request was successfully received and processed.

They do not all mean the same thing. Each code communicates a specific result.

### 🖆 200 OK

<badge color="success">Common</badge>

Indicates that the request succeeded.

It is commonly used for successful GET requests and other operations that return a representation.

Example:

```http
HTTP/1.1 200 OK
Content-Type: application/json

{
  "id": 42,
  "name": "John"
}
```

Common use cases:

* Retrieving user information.
* Fetching product details.
* Returning search results.
* Successfully processing an operation with a response body.

### 🖆 201 Created

Indicates that the request successfully created one or more resources.

It is commonly returned after a successful POST request.

Example:

```http
HTTP/1.1 201 Created
Content-Type: application/json
Location: /api/users/42

{
  "id": 42,
  "name": "John"
}
```

The `Location` header identifies the newly created resource.

### 🖆 202 Accepted

Indicates that the request has been accepted for processing, but processing has not necessarily completed.

Example:

```http
HTTP/1.1 202 Accepted
Content-Type: application/json

{
  "message": "Your report is being generated",
  "job_id": "job-123"
}
```

Common use cases:

* Background jobs.
* Video processing.
* Large file processing.
* Asynchronous operations.
* Scheduled tasks.

A `202 Accepted` response does not guarantee that the operation will eventually succeed.

### 🖆 203 Non-Authoritative Information

Indicates that the request was successful, but the returned metadata may have been modified by a transforming proxy.

It is less commonly used in typical web application development.

### 🖆 204 No Content

Indicates that the request succeeded, but there is no additional response content.

Example:

```http
HTTP/1.1 204 No Content
```

Common use cases:

* Successful DELETE operations.
* Successful updates where no response representation is needed.
* Operations that do not need to return a response body.

A 204 response must not contain response content.

### 🖆 205 Reset Content

Indicates that the server successfully processed the request and asks the client to reset the document view that caused the request.

It is uncommon in modern API development.

### 🖆 206 Partial Content

Indicates that the server successfully returned the requested portion of a resource.

It is commonly used for range requests and resumable downloads.

Request:

```http
GET /videos/movie.mp4 HTTP/1.1
Host: example.com
Range: bytes=0-999
```

Response:

```http
HTTP/1.1 206 Partial Content
Content-Type: video/mp4
Content-Range: bytes 0-999/5000000
Content-Length: 1000
```

The response contains only the requested byte range.

### 🖆 207 Multi-Status

A WebDAV response indicating that multiple independent operations produced different results.

It commonly uses an XML response body.

### 🖆 208 Already Reported

A WebDAV status code used inside a multi-status response to avoid repeatedly reporting the same resource.

### 🖆 226 IM Used

Indicates that the server successfully fulfilled a GET request using instance manipulation.

It is rarely encountered in ordinary web application development.

### 🖆 Successful Status Summary

| Code | Name | Typical Use |
| --- | --- | --- |
| 200 | OK | Successful request |
| 201 | Created | Resource created |
| 202 | Accepted | Processing accepted |
| 203 | Non-Authoritative Information | Modified metadata from a proxy |
| 204 | No Content | Successful request without response content |
| 205 | Reset Content | Reset client document view |
| 206 | Partial Content | Partial resource returned |
| 207 | Multi-Status | Multiple WebDAV operation results |
| 208 | Already Reported | Avoid repeated WebDAV resource reporting |
| 226 | IM Used | Instance manipulation applied |

# 🗁 3xx Redirection Responses

Redirection status codes indicate that the client needs to take additional action to complete the request.

Most commonly, the client requests another URL using the `Location` response header.

### 🖆 300 Multiple Choices

Indicates that multiple representations or choices are available for the requested resource.

The response can provide information that allows the client to select an appropriate representation.

### 🖆 301 Moved Permanently

Indicates that the resource has permanently moved to another URL.

Example:

```http
HTTP/1.1 301 Moved Permanently
Location: https://example.com/new-page
```

Common use cases:

* Permanent URL changes.
* Domain migrations.
* Redirecting HTTP traffic to HTTPS.
* Consolidating duplicate URLs.

Search engines generally treat this as a permanent redirect.

### 🖆 302 Found

Indicates that the resource is temporarily available at another URL.

Example:

```http
HTTP/1.1 302 Found
Location: https://example.com/temporary-page
```

It is commonly used for temporary redirects.

Historically, clients may change a POST request to GET when following a 302 redirect. Use 307 when preserving the original method and request content is important.

### 🖆 303 See Other

Indicates that the client should retrieve another resource using GET.

Example:

```http
HTTP/1.1 303 See Other
Location: /orders/42
```

A common use case is the Post/Redirect/Get pattern.

After processing a POST request, the server redirects the browser to a page that displays the result.

### 🖆 304 Not Modified

Indicates that a conditional GET or HEAD request can reuse a stored representation because the resource has not changed.

Example request:

```http
GET /products HTTP/1.1
Host: api.example.com
If-None-Match: "products-v1"
```

Response:

```http
HTTP/1.1 304 Not Modified
ETag: "products-v1"
Cache-Control: max-age=3600
```

A 304 response does not contain response content.

### 🖆 305 Use Proxy

Historically indicated that the client should access the resource through a specified proxy.

This status code is deprecated and should not be used.

### 🖆 306 Unused

Reserved and not currently used.

### 🖆 307 Temporary Redirect

Indicates that the resource is temporarily available at another URL.

Unlike 302, a client must not change the request method when following a 307 redirect.

Example:

```http
HTTP/1.1 307 Temporary Redirect
Location: https://example.com/new-endpoint
```

If the original request was POST, the redirected request must also use POST.

### 🖆 308 Permanent Redirect

Indicates that the resource has permanently moved to another URL.

Unlike 301, a client must not change the request method when following a 308 redirect.

Example:

```http
HTTP/1.1 308 Permanent Redirect
Location: https://example.com/new-endpoint
```

### 🖆 Redirect Status Comparison

| Code | Meaning | Permanent? | Method Preservation |
| --- | --- | --- | --- |
| 301 | Moved Permanently | Yes | Historical clients may change POST to GET |
| 302 | Found | No | Historical clients may change POST to GET |
| 303 | See Other | No | Follow-up request uses GET |
| 307 | Temporary Redirect | No | Preserved |
| 308 | Permanent Redirect | Yes | Preserved |

### 🖆 Redirection Status Summary

| Code | Name | Purpose |
| --- | --- | --- |
| 300 | Multiple Choices | Multiple representations available |
| 301 | Moved Permanently | Permanent redirect |
| 302 | Found | Temporary redirect |
| 303 | See Other | Retrieve another resource using GET |
| 304 | Not Modified | Reuse cached representation |
| 305 | Use Proxy | Deprecated |
| 306 | Unused | Reserved |
| 307 | Temporary Redirect | Temporary redirect preserving method |
| 308 | Permanent Redirect | Permanent redirect preserving method |

# 🗁 4xx Client Error Responses

Client error status codes indicate that the server cannot or will not fulfill the request because of a problem with the request or the client's authorization.

These errors can be caused by invalid input, missing resources, authentication failures, or access restrictions.

### 🖆 400 Bad Request

Indicates that the server cannot process the request because it considers the request malformed or invalid.

Example:

```http
HTTP/1.1 400 Bad Request
Content-Type: application/json

{
  "error": "Invalid request format"
}
```

Common causes:

* Invalid JSON.
* Malformed request syntax.
* Invalid parameters.
* Missing required fields.
* Invalid request framing.

### 🖆 401 Unauthorized

Indicates that the request lacks valid authentication credentials for the target resource.

Despite its name, this status code concerns authentication rather than authorization.

Example:

```http
HTTP/1.1 401 Unauthorized
WWW-Authenticate: Bearer realm="api"
Content-Type: application/json

{
  "error": "Authentication required"
}
```

Common causes:

* Missing access token.
* Invalid credentials.
* Expired token.
* Invalid authentication scheme.

### 🖆 402 Payment Required

Reserved for future use.

Some applications use it for payment-related features, but its semantics are not universally standardized for those application-specific purposes.

### 🖆 403 Forbidden

Indicates that the server understood the request but refuses to fulfill it.

The client may be authenticated but still lack permission.

Example:

```http
HTTP/1.1 403 Forbidden
Content-Type: application/json

{
  "error": "You do not have permission to access this resource"
}
```

Common causes:

* Insufficient permissions.
* Restricted resources.
* Access denied by application policy.
* Requests blocked by security rules.

### 🖆 404 Not Found

<badge color="success">Common</badge>

Indicates that the server cannot find a current representation of the target resource or is unwilling to disclose that one exists.

Example:

```http
HTTP/1.1 404 Not Found
Content-Type: application/json

{
  "error": "User not found"
}
```

Common causes:

* Incorrect URL.
* Resource does not exist.
* Deleted resource.
* Incorrect API endpoint.

### 🖆 405 Method Not Allowed

Indicates that the server recognizes the method but does not support it for the target resource.

Example:

```http
HTTP/1.1 405 Method Not Allowed
Allow: GET, HEAD
```

The `Allow` header identifies the methods supported by the resource.

### 🖆 406 Not Acceptable

Indicates that the server cannot provide a response representation matching the client's acceptable representation preferences.

Example request:

```http
GET /products HTTP/1.1
Host: api.example.com
Accept: application/xml
```

If the server cannot provide XML, it may return:

```http
HTTP/1.1 406 Not Acceptable
```

### 🖆 407 Proxy Authentication Required

Indicates that authentication with a proxy is required.

Example:

```http
HTTP/1.1 407 Proxy Authentication Required
Proxy-Authenticate: Basic realm="Corporate Proxy"
```

### 🖆 408 Request Timeout

Indicates that the server did not receive a complete request within the time it was prepared to wait.

### 🖆 409 Conflict

Indicates that the request conflicts with the current state of the target resource.

Example:

```http
HTTP/1.1 409 Conflict
Content-Type: application/json

{
  "error": "A user with this email already exists"
}
```

Common causes:

* Duplicate resource creation.
* Conflicting updates.
* Resource state conflicts.
* Concurrent modifications.

### 🖆 410 Gone

Indicates that the target resource is no longer available and that the condition is likely permanent.

Unlike 404, it communicates that the resource was intentionally removed and is not expected to return.

### 🖆 411 Length Required

Indicates that the server requires a valid Content-Length field for the request.

### 🖆 412 Precondition Failed

Indicates that one or more request preconditions were not satisfied.

Example:

```http
PUT /users/42 HTTP/1.1
Host: api.example.com
If-Match: "version-1"
Content-Type: application/json

{
  "name": "Alice"
}
```

If the resource's current ETag does not match the supplied validator, the server can return:

```http
HTTP/1.1 412 Precondition Failed
```

This helps prevent updates based on outdated resource versions.

### 🖆 413 Content Too Large

Indicates that the request content exceeds a limit the server is willing or able to process.

Example:

```http
HTTP/1.1 413 Content Too Large
Content-Type: application/json

{
  "error": "Uploaded file exceeds the maximum size"
}
```

The older name was `Payload Too Large`.

### 🖆 414 URI Too Long

Indicates that the request target is longer than the server is willing to interpret.

Common causes include excessively long query strings or URLs.

### 🖆 415 Unsupported Media Type

Indicates that the server refuses the request because the content format is unsupported.

Example:

```http
POST /api/users HTTP/1.1
Host: api.example.com
Content-Type: text/plain

{"name":"John"}
```

If the API only accepts JSON, it might respond:

```http
HTTP/1.1 415 Unsupported Media Type
```

### 🖆 416 Range Not Satisfiable

Indicates that the requested range cannot be satisfied.

Example:

```http
HTTP/1.1 416 Range Not Satisfiable
Content-Range: bytes */5000
```

This might happen when a client requests bytes beyond the resource's current length.

### 🖆 417 Expectation Failed

Indicates that the server cannot meet the expectation specified in the request's `Expect` header.

### 🖆 418 Unused

Originally introduced as an April Fools' joke in RFC 2324.

It is not a standard general-purpose error response for normal HTTP application behavior. Some implementations still use it for playful or custom purposes.

### 🖆 421 Misdirected Request

Indicates that the request was directed to a server that cannot produce an authoritative response for the target URI.

It can occur in connection reuse scenarios involving HTTP/2.

### 🖆 422 Unprocessable Content

Indicates that the server understands the request content type and syntax, but cannot process the contained instructions.

Example:

```http
HTTP/1.1 422 Unprocessable Content
Content-Type: application/json

{
  "error": "Validation failed",
  "details": {
    "email": "Invalid email address"
  }
}
```

Common use cases:

* Field validation failures.
* Invalid business rules.
* Semantically invalid request content.

### 🖆 423 Locked

A WebDAV status code indicating that the resource is locked.

### 🖆 424 Failed Dependency

A WebDAV status code indicating that an operation failed because another operation it depended on failed.

### 🖆 425 Too Early

Indicates that the server is unwilling to process a request that might be replayed.

It is associated with preventing risks involving early data in TLS connections.

### 🖆 426 Upgrade Required

Indicates that the server requires the client to switch to another protocol.

Example:

```http
HTTP/1.1 426 Upgrade Required
Upgrade: HTTP/2
```

The appropriate upgrade mechanism depends on the protocol and deployment.

### 🖆 428 Precondition Required

Indicates that the server requires the request to be conditional.

It is commonly used to prevent lost updates.

Example:

```http
HTTP/1.1 428 Precondition Required
Content-Type: application/json

{
  "error": "An If-Match header is required"
}
```

### 🖆 429 Too Many Requests

<badge color="success">Common</badge>

Indicates that the client has sent too many requests within a given period.

It is commonly used for rate limiting.

Example:

```http
HTTP/1.1 429 Too Many Requests
Retry-After: 60
Content-Type: application/json

{
  "error": "Rate limit exceeded"
}
```

The `Retry-After` header can tell the client when to retry.

### 🖆 431 Request Header Fields Too Large

Indicates that the server refuses to process the request because its header fields are too large.

### 🖆 451 Unavailable For Legal Reasons

Indicates that access to the resource is denied for legal reasons.

Example:

```http
HTTP/1.1 451 Unavailable For Legal Reasons
```

### 🖆 Client Error Status Summary

| Code | Name | Purpose |
| --- | --- | --- |
| 400 | Bad Request | Invalid or malformed request |
| 401 | Unauthorized | Authentication required or invalid |
| 402 | Payment Required | Reserved |
| 403 | Forbidden | Access refused |
| 404 | Not Found | Resource unavailable |
| 405 | Method Not Allowed | Unsupported method for resource |
| 406 | Not Acceptable | No acceptable representation |
| 407 | Proxy Authentication Required | Proxy authentication required |
| 408 | Request Timeout | Request not received in time |
| 409 | Conflict | Conflict with resource state |
| 410 | Gone | Resource permanently unavailable |
| 411 | Length Required | Content-Length required |
| 412 | Precondition Failed | Request condition not satisfied |
| 413 | Content Too Large | Request content exceeds limit |
| 414 | URI Too Long | Request target too long |
| 415 | Unsupported Media Type | Unsupported request format |
| 416 | Range Not Satisfiable | Requested range unavailable |
| 417 | Expectation Failed | Request expectation not supported |
| 418 | Unused | Historically associated with an April Fools' joke |
| 421 | Misdirected Request | Request sent to an unsuitable server |
| 422 | Unprocessable Content | Semantically invalid content |
| 423 | Locked | Resource locked |
| 424 | Failed Dependency | Dependent operation failed |
| 425 | Too Early | Potentially replayable request rejected |
| 426 | Upgrade Required | Protocol upgrade required |
| 428 | Precondition Required | Conditional request required |
| 429 | Too Many Requests | Rate limit exceeded |
| 431 | Request Header Fields Too Large | Request headers too large |
| 451 | Unavailable For Legal Reasons | Access denied for legal reasons |

# 🗁 5xx Server Error Responses

Server error status codes indicate that the server failed to fulfill an apparently valid request.

The problem may originate from the application, infrastructure, database, upstream services, or other server-side components.

### 🖆 500 Internal Server Error

<badge color="success">Common</badge>

Indicates that the server encountered an unexpected condition that prevented it from fulfilling the request.

Example:

```http
HTTP/1.1 500 Internal Server Error
Content-Type: application/json

{
  "error": "An unexpected error occurred"
}
```

Common causes:

* Unhandled application exceptions.
* Database failures.
* Programming errors.
* Unexpected runtime conditions.
* Incorrect server configuration.

### 🖆 501 Not Implemented

Indicates that the server does not support the functionality required to fulfill the request.

It is commonly used when a server does not recognize or implement a required method or functionality.

### 🖆 502 Bad Gateway

Indicates that a server acting as a gateway or proxy received an invalid response from an upstream server.

Example:

```http
HTTP/1.1 502 Bad Gateway
```

Common causes:

* Upstream application crashes.
* Invalid upstream responses.
* Reverse proxy communication failures.
* Gateway configuration problems.

### 🖆 503 Service Unavailable

<badge color="success">Common</badge>

Indicates that the server is temporarily unable to handle the request.

Example:

```http
HTTP/1.1 503 Service Unavailable
Retry-After: 120
Content-Type: application/json

{
  "error": "Service temporarily unavailable"
}
```

Common causes:

* Server maintenance.
* Overloaded infrastructure.
* Temporary service outages.
* Resource exhaustion.

### 🖆 504 Gateway Timeout

Indicates that a gateway or proxy did not receive a timely response from an upstream server.

Example:

```http
HTTP/1.1 504 Gateway Timeout
```

Common causes:

* Slow database queries.
* Unresponsive upstream services.
* Network delays.
* Incorrect proxy timeout configuration.

### 🖆 505 HTTP Version Not Supported

Indicates that the server does not support the HTTP version used in the request.

### 🖆 506 Variant Also Negotiates

Indicates an internal server configuration error involving transparent content negotiation.

### 🖆 507 Insufficient Storage

A WebDAV status code indicating that the server cannot store the representation needed to complete the request.

### 🖆 508 Loop Detected

A WebDAV status code indicating that the server detected an infinite loop while processing a request.

### 🖆 510 Not Extended

Indicates that further extensions to the request are required for the server to fulfill it.

### 🖆 511 Network Authentication Required

Indicates that the client needs to authenticate to gain network access.

It is commonly associated with captive portals.

### 🖆 Server Error Status Summary

| Code | Name | Purpose |
| --- | --- | --- |
| 500 | Internal Server Error | Unexpected server failure |
| 501 | Not Implemented | Required functionality unsupported |
| 502 | Bad Gateway | Invalid upstream response |
| 503 | Service Unavailable | Server temporarily unavailable |
| 504 | Gateway Timeout | Upstream response timed out |
| 505 | HTTP Version Not Supported | HTTP version unsupported |
| 506 | Variant Also Negotiates | Content negotiation configuration error |
| 507 | Insufficient Storage | Insufficient storage |
| 508 | Loop Detected | Infinite loop detected |
| 510 | Not Extended | Required request extensions missing |
| 511 | Network Authentication Required | Network authentication required |

# 🗁 Common HTTP Status Codes

The following codes are particularly important in everyday web development and API design.

| Code | Name | Common Scenario |
| --- | --- | --- |
| 200 | OK | Data retrieved successfully |
| 201 | Created | New resource created |
| 202 | Accepted | Background task accepted |
| 204 | No Content | Operation succeeded without a body |
| 301 | Moved Permanently | Permanent URL change |
| 302 | Found | Temporary redirect |
| 304 | Not Modified | Cached representation is still valid |
| 307 | Temporary Redirect | Temporary redirect preserving method |
| 308 | Permanent Redirect | Permanent redirect preserving method |
| 400 | Bad Request | Invalid request data |
| 401 | Unauthorized | Missing or invalid authentication |
| 403 | Forbidden | Insufficient permission |
| 404 | Not Found | Resource does not exist or is undisclosed |
| 405 | Method Not Allowed | Unsupported HTTP method |
| 409 | Conflict | Resource state conflict |
| 412 | Precondition Failed | Conditional request failed |
| 413 | Content Too Large | Upload exceeds configured limit |
| 415 | Unsupported Media Type | Unsupported request content type |
| 422 | Unprocessable Content | Validation or semantic error |
| 429 | Too Many Requests | Rate limit exceeded |
| 500 | Internal Server Error | Unexpected server failure |
| 502 | Bad Gateway | Upstream server failure |
| 503 | Service Unavailable | Temporary service outage |
| 504 | Gateway Timeout | Upstream server timeout |

# 🗁 Status Codes in REST APIs

RESTful APIs use HTTP status codes to communicate the result of operations on resources.

Using appropriate status codes makes APIs easier to understand and integrate.

### 🖆 GET: Retrieve Resources

Request:

```http
GET /api/users/42
```

Possible responses:

```http
200 OK
```

The user was found.

```http
404 Not Found
```

The user could not be found.

### 🖆 POST: Create Resources

Request:

```http
POST /api/users
Content-Type: application/json

{
  "name": "Alice",
  "email": "alice@example.com"
}
```

Possible responses:

```http
201 Created
Location: /api/users/43
```

The user was created.

```http
400 Bad Request
```

The request is malformed or invalid.

```http
409 Conflict
```

A conflicting resource already exists.

```http
422 Unprocessable Content
```

The request content is syntactically valid but fails application validation.

### 🖆 PUT: Replace Resources

Request:

```http
PUT /api/users/42
Content-Type: application/json

{
  "name": "Alice Smith",
  "email": "alice.smith@example.com"
}
```

Possible responses:

```http
200 OK
```

The resource was replaced and a representation is returned.

```http
204 No Content
```

The resource was replaced without returning content.

```http
404 Not Found
```

The server requires the resource to exist but could not find it.

A server can also create a resource through PUT when the target URI identifies a resource that does not yet exist.

### 🖆 PATCH: Partially Update Resources

Request:

```http
PATCH /api/users/42
Content-Type: application/json

{
  "name": "Alice Smith"
}
```

Possible responses:

```http
200 OK
```

The update succeeded and the updated representation is returned.

```http
204 No Content
```

The update succeeded without a response body.

```http
404 Not Found
```

The resource does not exist.

```http
412 Precondition Failed
```

A request precondition was not satisfied.

### 🖆 DELETE: Remove Resources

Request:

```http
DELETE /api/users/42
```

Possible responses:

```http
204 No Content
```

The deletion succeeded.

```http
200 OK
```

The deletion succeeded and a response representation is returned.

```http
404 Not Found
```

The target resource could not be found.

### 🖆 Example API Response Structure

An application can use a consistent JSON structure for errors.

```json
{
  "error": {
    "code": "VALIDATION_ERROR",
    "message": "The submitted data is invalid",
    "details": {
      "email": "A valid email address is required"
    }
  }
}
```

The HTTP status code communicates the general result, while the JSON body provides application-specific details.

# 🗁 Handling Status Codes with JavaScript Fetch

The Fetch API returns a `Response` object containing the HTTP response status and other metadata.

### 🖆 Checking Response Status

```javascript
const response = await fetch("/api/users");

console.log(response.status);
console.log(response.statusText);
console.log(response.ok);
```

Important properties:

| Property | Description |
| --- | --- |
| `status` | Numeric HTTP status code |
| `statusText` | Status message where available |
| `ok` | `true` when the status is between 200 and 299 |
| `headers` | Response headers |
| `redirected` | Indicates whether the response followed a redirect |
| `url` | Final response URL |

### 🖆 Using response.ok

```javascript
async function getUsers() {
  const response = await fetch("/api/users");

  if (!response.ok) {
    throw new Error(`Request failed: ${response.status}`);
  }

  return response.json();
}
```

This is important because Fetch does not reject its promise simply because the server returns a 400 or 500 response.

### 🖆 Handling Specific Status Codes

```javascript
async function getUser(id) {
  const response = await fetch(`/api/users/${id}`);

  if (response.status === 404) {
    return null;
  }

  if (response.status === 401) {
    throw new Error("Authentication required");
  }

  if (response.status === 403) {
    throw new Error("Access denied");
  }

  if (!response.ok) {
    throw new Error(`HTTP Error: ${response.status}`);
  }

  return response.json();
}
```

### 🖆 Handling Errors with try/catch

```javascript
async function loadUsers() {
  try {
    const response = await fetch("/api/users");

    if (!response.ok) {
      throw new Error(`HTTP ${response.status}`);
    }

    const users = await response.json();

    console.log(users);
  } catch (error) {
    console.error("Request failed:", error.message);
  }
}
```

The `catch` block handles network errors and exceptions thrown by application code. HTTP error status codes must be checked explicitly.

### 🖆 Handling 204 No Content

A 204 response has no body, so do not attempt to parse it as JSON.

```javascript
async function deleteUser(id) {
  const response = await fetch(`/api/users/${id}`, {
    method: "DELETE"
  });

  if (!response.ok) {
    throw new Error(`Delete failed: ${response.status}`);
  }

  if (response.status === 204) {
    return;
  }

  return response.json();
}
```

# 🗁 Testing Status Codes with cURL

cURL is useful for inspecting server responses and debugging API behavior.

### 🖆 Display Response Headers and Body

```bash
curl -i https://api.example.com/users
```

The `-i` option includes response headers in the output.

### 🖆 Display Only Response Headers

```bash
curl -I https://api.example.com/users
```

This sends a HEAD request.

### 🖆 Display HTTP Status Code

```bash
curl -s -o /dev/null -w "%{http_code}\n" \
  https://api.example.com/users
```

Example output:

```text
200
```

### 🖆 Display Status Code and Response Time

```bash
curl -s -o /dev/null \
  -w "HTTP Status: %{http_code}\nResponse Time: %{time_total}s\n" \
  https://api.example.com/users
```

### 🖆 Test a POST Request

```bash
curl -i -X POST https://api.example.com/users \
  -H "Content-Type: application/json" \
  -d '{"name":"Alice","email":"alice@example.com"}'
```

### 🖆 Test a Missing Resource

```bash
curl -i https://api.example.com/users/999999
```

### 🖆 Follow Redirects

```bash
curl -L -i https://example.com
```

The `-L` option instructs cURL to follow redirects.

### 🖆 Inspect Redirect Without Following

```bash
curl -I https://example.com
```

Inspect the response status and `Location` header to understand the redirect behavior.

# 🗁 Choosing the Correct Status Code

Selecting the correct status code is an important part of API design.

### 🖆 Successful Operations

| Situation | Recommended Code |
| --- | --- |
| Resource retrieved | 200 |
| Resource created | 201 |
| Request accepted for asynchronous processing | 202 |
| Operation succeeded without response content | 204 |
| Partial representation returned | 206 |

### 🖆 Redirects

| Situation | Recommended Code |
| --- | --- |
| Permanent redirect | 301 or 308 |
| Temporary redirect | 302 or 307 |
| Redirect to a GET representation after processing | 303 |
| Conditional request indicates no change | 304 |

Choose 307 or 308 when the original method and request content must be preserved during redirection.

### 🖆 Client Errors

| Situation | Recommended Code |
| --- | --- |
| Malformed request | 400 |
| Missing or invalid authentication | 401 |
| Authenticated client lacks permission | 403 |
| Resource unavailable | 404 |
| Unsupported method | 405 |
| Resource state conflict | 409 |
| Failed request precondition | 412 |
| Request content too large | 413 |
| Unsupported content type | 415 |
| Semantic validation failure | 422 |
| Rate limit exceeded | 429 |

### 🖆 Server Errors

| Situation | Recommended Code |
| --- | --- |
| Unexpected application failure | 500 |
| Invalid upstream response | 502 |
| Temporary service unavailability | 503 |
| Upstream response timeout | 504 |

### 🖆 401 vs 403

These two status codes are frequently confused.

| 401 Unauthorized | 403 Forbidden |
| --- | --- |
| Authentication is missing or invalid | Server refuses the request |
| Client may need to authenticate | Authentication alone does not grant access |
| Commonly includes `WWW-Authenticate` | Does not require an authentication challenge |

Example:

A user without a valid access token requests a protected endpoint:

```http
401 Unauthorized
```

An authenticated user attempts to access an administrator-only endpoint:

```http
403 Forbidden
```

### 🖆 400 vs 422

| 400 Bad Request | 422 Unprocessable Content |
| --- | --- |
| Request cannot be processed due to invalid or malformed request conditions | Request content is understood but its instructions cannot be processed |
| Often used for malformed JSON or invalid request syntax | Often used for semantic validation errors |
| General client request error | More specific content-processing error |

Example of 400:

```json
{
  "name": "Alice",
```

The JSON is malformed.

Example of 422:

```json
{
  "name": "",
  "email": "not-an-email"
}
```

The JSON is valid, but the values fail application validation.

### 🖆 404 vs 410

| 404 Not Found | 410 Gone |
| --- | --- |
| Resource is unavailable or its existence is not disclosed | Resource is known to be permanently unavailable |
| Does not necessarily indicate permanent removal | Communicates likely permanent removal |

# 🗁 Common Mistakes

### 🖆 Returning 200 for Every Response

Some applications return `200 OK` even when an operation fails.

For example:

```http
HTTP/1.1 200 OK
Content-Type: application/json

{
  "success": false,
  "error": "User not found"
}
```

This makes it harder for clients, monitoring systems, and HTTP intermediaries to interpret the actual result.

Use an appropriate status code:

```http
HTTP/1.1 404 Not Found
```

### 🖆 Confusing 401 and 403

Use 401 when valid authentication credentials are missing or unavailable.

Use 403 when the server refuses access to the requested resource.

### 🖆 Returning 404 Instead of 405

If the resource exists but does not support the requested method, return:

```http
HTTP/1.1 405 Method Not Allowed
Allow: GET, HEAD
```

Do not incorrectly report that the resource does not exist.

### 🖆 Returning a Body with 204

A 204 response must not contain response content.

Use 200 when the server needs to return a response representation.

### 🖆 Using 500 for Every Application Error

Do not treat validation errors, missing resources, and authentication failures as internal server errors.

Use the appropriate 4xx status codes for client-related request failures.

### 🖆 Confusing 502 and 504

* 502 means the gateway received an invalid response from an upstream server.
* 504 means the gateway did not receive a timely response from an upstream server.

### 🖆 Ignoring Retry-After

When returning 429 or 503, consider including `Retry-After` to communicate when a client should retry.

### 🖆 Assuming All Redirects Preserve the Request Method

301 and 302 have historical behavior that can allow POST-to-GET conversion.

Use 307 or 308 when preserving the method and request content is required.

# 🗁 Best Practices

* Return status codes that accurately describe the result of each request.
* Use 2xx responses for successful operations.
* Use 3xx responses for redirection and conditional response behavior.
* Use 4xx responses for request-related and access-related failures.
* Use 5xx responses for server-side failures.
* Use 201 Created when a request creates a resource.
* Use 204 No Content when success does not require a response body.
* Use 401 for missing or invalid authentication.
* Use 403 for denied access.
* Use 404 for unavailable resources.
* Use 405 when a resource does not support the requested method.
* Use 409 for conflicts with the current resource state.
* Use 422 for semantically invalid request content when appropriate.
* Use 429 for rate limiting.
* Use 503 for temporary service unavailability.
* Include useful error details in response bodies without exposing internal implementation information.
* Keep API error response structures consistent.
* Document status codes in API specifications.
* Handle unsuccessful HTTP responses explicitly in frontend code.
* Avoid relying on reason phrases instead of numeric status codes.

# 🗁 Practical Exercises

### 🖆 Exercise: Identify Status Codes

Identify the most appropriate status code for each situation.

* A user successfully retrieves their profile.
* A new product is successfully created.
* A request is accepted for background processing.
* A user requests a resource that does not exist.
* An authenticated user accesses a restricted resource.
* A request contains malformed JSON.
* A client sends an unsupported HTTP method.
* A user exceeds the API rate limit.
* A reverse proxy receives an invalid response from an upstream service.
* A gateway does not receive an upstream response in time.

### 🖆 Exercise: Build a REST API

Design an API for managing products.

Implement the following endpoints:

```text
GET    /api/products
GET    /api/products/42
POST   /api/products
PUT    /api/products/42
PATCH  /api/products/42
DELETE /api/products/42
```

Define appropriate status codes for:

* Successful operations.
* Invalid request content.
* Missing products.
* Duplicate product creation.
* Authentication failures.
* Permission failures.
* Unexpected server errors.

### 🖆 Exercise: Handle Errors with Fetch

Write a JavaScript function that:

* Sends a GET request.
* Checks the HTTP status.
* Handles 401, 403, and 404 separately.
* Handles other unsuccessful responses.
* Parses JSON only when a response body is available.

### 🖆 Exercise: Inspect Redirects

Use cURL to inspect a website that redirects from HTTP to HTTPS.

```bash
curl -I http://example.com
```

Identify:

* The original response status.
* The Location header.
* The destination URL.

Then follow the redirect:

```bash
curl -L -I http://example.com
```

### 🖆 Exercise: Simulate API Errors

Create API endpoints that return:

```text
400 Bad Request
401 Unauthorized
403 Forbidden
404 Not Found
409 Conflict
422 Unprocessable Content
429 Too Many Requests
500 Internal Server Error
503 Service Unavailable
```

Test each response using cURL and inspect its status code and response body.

# 🗁 Interview Questions

### 🖆 What are HTTP status codes?

HTTP status codes are three-digit numbers returned by a server to communicate the result of an HTTP request.

### 🖆 How many HTTP status code categories are there?

There are five categories: informational, successful, redirection, client error, and server error.

### 🖆 What does 200 OK mean?

It indicates that the request was successfully processed.

### 🖆 What is the difference between 200 and 201?

200 indicates a successful request. 201 specifically indicates that the request created one or more resources.

### 🖆 What is the difference between 200 and 204?

200 can return a response representation. 204 indicates success without response content.

### 🖆 What does 202 Accepted mean?

The server accepted the request for processing, but processing may not be complete.

### 🖆 What is the difference between 301 and 302?

301 indicates a permanent redirect, while 302 indicates a temporary redirect.

### 🖆 What is the difference between 302 and 307?

Both indicate temporary redirection, but 307 requires the client to preserve the original request method and content.

### 🖆 What is the difference between 301 and 308?

Both indicate permanent redirection, but 308 requires the client to preserve the original request method and content.

### 🖆 What does 304 Not Modified mean?

It indicates that a conditional request can reuse a stored representation because the resource has not changed.

### 🖆 What is the difference between 401 and 403?

401 indicates missing or invalid authentication credentials. 403 indicates that the server refuses to fulfill the request.

### 🖆 What does 404 Not Found mean?

The server cannot find a current representation of the target resource or is unwilling to disclose that one exists.

### 🖆 What is the difference between 400 and 422?

400 indicates a malformed or otherwise invalid request. 422 indicates that the request content is understood but its instructions cannot be processed.

### 🖆 What does 409 Conflict mean?

It indicates that the request conflicts with the current state of the target resource.

### 🖆 What does 429 Too Many Requests mean?

The client has sent too many requests within a given period, commonly because of rate limiting.

### 🖆 What is the difference between 502 and 504?

502 indicates an invalid upstream response. 504 indicates that an upstream response was not received within the required time.

### 🖆 Does Fetch throw an error when the server returns 404?

No. Fetch normally resolves with a Response object. Developers must inspect `response.ok` or `response.status`.

### 🖆 What is the purpose of the Retry-After header?

It tells the client how long to wait before retrying a request or provides a date when retrying is appropriate.

### 🖆 Can an HTTP response contain a status code without a response body?

Yes. For example, 204 No Content and 304 Not Modified do not contain response content.

# 🗁 References

* [RFC 9110 — HTTP Semantics](https://www.rfc-editor.org/rfc/rfc9110)
* [RFC 9111 — HTTP Caching](https://www.rfc-editor.org/rfc/rfc9111)
* [RFC 4918 — WebDAV](https://www.rfc-editor.org/rfc/rfc4918)
* [RFC 6585 — Additional HTTP Status Codes](https://www.rfc-editor.org/rfc/rfc6585)
* [RFC 7725 — HTTP 451](https://www.rfc-editor.org/rfc/rfc7725)
* [RFC 8470 — Using Early Data in HTTP](https://www.rfc-editor.org/rfc/rfc8470)
* [MDN — HTTP Response Status Codes](https://developer.mozilla.org/en-US/docs/Web/HTTP/Reference/Status)
* [MDN — HTTP](https://developer.mozilla.org/en-US/docs/Web/HTTP)
