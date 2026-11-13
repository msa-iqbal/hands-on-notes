# HTTP Requests

# 🗁 What Is an HTTP Request?

An HTTP request is a communication message sent from a client to a server using the HTTP protocol.

The client may be:

* A web browser.
* A frontend JavaScript application.
* A mobile application.
* A command-line tool.
* A backend service.
* An API testing application.

The server receives the request, interprets its contents, performs the requested operation, and returns a response.

### 🖆 Basic Example

```http
GET /products HTTP/1.1
Host: example.com
Accept: application/json
```

This request means:

* `GET`: Retrieve a resource.
* `/products`: The requested resource path.
* `HTTP/1.1`: The HTTP version.
* `Host`: The destination hostname.
* `Accept`: The client prefers JSON responses.

### 🖆 Client-Server Communication

```text
       HTTP Client
            |
            | HTTP Request
            v
       HTTP Server
            |
            | Process Request
            |
            v
       HTTP Response
            |
            v
       HTTP Client
```

# 🗁 HTTP Request Structure

An HTTP/1.1 request message consists of several components:

```text
Request Line
Request Headers

Optional Request Body
```

A more complete representation is:

```http
POST /api/users HTTP/1.1
Host: example.com
Content-Type: application/json
Accept: application/json
Authorization: Bearer token123
Content-Length: 42

{
    "name": "Alice",
    "email": "alice@example.com"
}
```

The components are:

| Component | Description |
| --- | --- |
| Request line | Contains the method, request target, and HTTP version |
| Headers | Provide request metadata |
| Empty line | Separates headers from the message body |
| Request body | Contains optional data sent by the client |

HTTP/2 and HTTP/3 use binary framing rather than the textual HTTP/1.1 message format, but preserve the same core HTTP semantics.

# 🗁 Request Line

The request line is the first line of an HTTP/1.1 request.

Its general syntax is:

```text
METHOD REQUEST-TARGET HTTP-VERSION
```

Example:

```http
GET /users HTTP/1.1
```

The request line contains three components.

### 🖆 Method

The method identifies the intended operation.

Examples:

* `GET`
* `POST`
* `PUT`
* `PATCH`
* `DELETE`
* `HEAD`
* `OPTIONS`

### 🖆 Request Target

The request target identifies the resource being accessed.

Example:

```text
/products/10
```

### 🖆 HTTP Version

The version identifies the HTTP protocol version in HTTP/1.1 syntax.

Example:

```text
HTTP/1.1
```

HTTP/2 and HTTP/3 do not use this textual request-line format on the wire. Their protocol framing carries the equivalent information through fields such as `:method`, `:path`, and `:scheme`.

# 🗁 HTTP Methods

An HTTP method describes the intended action for a request.

Common HTTP methods include:

| Method | Purpose | Typical Request Body |
| --- | --- | --- |
| `GET` | Retrieve a resource | Usually absent |
| `POST` | Submit data or trigger processing | Commonly present |
| `PUT` | Create or replace a resource | Commonly present |
| `PATCH` | Partially modify a resource | Commonly present |
| `DELETE` | Delete a resource | Not usually needed |
| `HEAD` | Retrieve response headers without response content | Usually absent |
| `OPTIONS` | Discover communication options | Usually absent |
| `TRACE` | Perform a message loop-back test | Usually absent |

Example:

```http
GET /api/products HTTP/1.1
Host: example.com
```

This requests the product collection.

HTTP method semantics are covered in detail in `04-methods.md`.

# 🗁 Request Target

The request target identifies the resource or destination of an HTTP request.

Its representation depends on the request context.

HTTP/1.1 defines four main request-target forms.

### 🖆 Origin-Form

This is the most common form used when a client directly requests a resource from an origin server.

Syntax:

```text
/path?query
```

Example:

```http
GET /products?id=10 HTTP/1.1
Host: example.com
```

Components:

* Path: `/products`
* Query string: `id=10`

### 🖆 Absolute-Form

Absolute-form contains the complete target URI.

It is commonly used when communicating with a forward proxy.

Example:

```http
GET http://example.com/products HTTP/1.1
Host: example.com
```

### 🖆 Authority-Form

Authority-form is used with the `CONNECT` method to establish a tunnel through a proxy.

Example:

```http
CONNECT example.com:443 HTTP/1.1
Host: example.com:443
```

The authority identifies the target host and port.

### 🖆 Asterisk-Form

Asterisk-form uses `*` as the request target.

It is commonly used with the `OPTIONS` method to request information about the server as a whole.

Example:

```http
OPTIONS * HTTP/1.1
Host: example.com
```

# 🗁 HTTP Version

The HTTP version identifies the protocol version used for communication.

In HTTP/1.1, the version appears in the request line:

```http
GET / HTTP/1.1
Host: example.com
```

Modern HTTP versions use different wire formats:

| Version | Request Representation |
| --- | --- |
| HTTP/1.1 | Text-based request line and headers |
| HTTP/2 | Binary frames and pseudo-headers |
| HTTP/3 | Binary frames over QUIC and pseudo-headers |

HTTP/2 and HTTP/3 use pseudo-headers to represent information that appears in the HTTP/1.1 request line.

Example HTTP/2-style request fields:

```text
:method: GET
:scheme: https
:authority: example.com
:path: /products
accept: application/json
```

Pseudo-headers begin with a colon and provide protocol-level request information.

# 🗁 Request Headers

HTTP request headers carry additional information about the request.

They help the server understand:

* The requested content type.
* The client's preferred response format.
* Authentication credentials.
* Client capabilities.
* Caching preferences.
* Cookies.
* Request metadata.

### 🖆 Common Request Headers

| Header | Purpose |
| --- | --- |
| `Host` | Identifies the target host in HTTP/1.1 |
| `Accept` | Specifies acceptable response media types |
| `Content-Type` | Identifies the request body media type |
| `Content-Length` | Specifies the message body length in octets when applicable |
| `Authorization` | Carries authentication credentials |
| `Cookie` | Sends stored cookies |
| `User-Agent` | Identifies the requesting software |
| `Accept-Language` | Indicates preferred languages |
| `Accept-Encoding` | Indicates supported content codings |
| `Cache-Control` | Provides caching directives |
| `Origin` | Identifies the origin of a request |
| `Referer` | Identifies the referring URL, subject to browser privacy rules |

### 🖆 Example Request with Headers

```http
GET /api/users HTTP/1.1
Host: example.com
Accept: application/json
Accept-Language: en-US
User-Agent: Mozilla/5.0
Authorization: Bearer example-token
```

The server can use this metadata when processing the request.

### 🖆 Authorization Header

The `Authorization` header carries credentials for an authentication scheme.

Example using Bearer authentication:

```http
GET /api/profile HTTP/1.1
Host: example.com
Authorization: Bearer eyJhbGciOi...
```

The server validates the token according to its authentication implementation.

### 🖆 Cookie Header

The `Cookie` header sends previously stored cookies to the server.

Example:

```http
GET /dashboard HTTP/1.1
Host: example.com
Cookie: session_id=abc123
```

The server can use the session identifier to locate the associated session.

Cookies are covered in detail in `07-cookies.md`.

# 🗁 Request Body

The request body contains optional data sent by the client.

It is commonly used with methods such as:

* `POST`
* `PUT`
* `PATCH`

Example:

```http
POST /api/users HTTP/1.1
Host: example.com
Content-Type: application/json

{
    "name": "Alice",
    "email": "alice@example.com"
}
```

The body contains the JSON representation submitted to the server.

### 🖆 JSON Body

JSON is commonly used for frontend-to-backend API communication.

Example:

```json
{
    "name": "Alice",
    "email": "alice@example.com",
    "role": "developer"
}
```

Corresponding header:

```http
Content-Type: application/json
```

### 🖆 Form URL-Encoded Body

HTML forms commonly submit data using the `application/x-www-form-urlencoded` media type.

Example:

```http
POST /login HTTP/1.1
Host: example.com
Content-Type: application/x-www-form-urlencoded

username=alice&password=secret
```

### 🖆 Multipart Form Data

Multipart form data is commonly used for file uploads and form submissions containing different types of data.

Example:

```http
POST /upload HTTP/1.1
Host: example.com
Content-Type: multipart/form-data; boundary=----FormBoundary

------FormBoundary
Content-Disposition: form-data; name="username"

Alice
------FormBoundary
Content-Disposition: form-data; name="avatar"; filename="avatar.png"
Content-Type: image/png

[Binary file content]
------FormBoundary--
```

In real requests, the client generates the multipart boundary and uses it consistently throughout the body.

### 🖆 Raw Text Body

A request body can also contain plain text.

Example:

```http
POST /messages HTTP/1.1
Host: example.com
Content-Type: text/plain

Hello from the HTTP client.
```

### 🖆 Binary Body

HTTP can transfer binary data, including:

* Images.
* Audio files.
* Video files.
* PDF documents.
* Compressed archives.

The request body is not limited to textual formats.

### 🖆 Do All HTTP Methods Support Request Bodies?

HTTP message framing does not generally prohibit a request body based on the method.

However, method semantics matter.

* `GET` has no generally defined semantics for request content.
* `HEAD` has no generally defined semantics for request content.
* `DELETE` does not have generally defined semantics for request content.
* `POST`, `PUT`, and `PATCH` commonly use request bodies.

Clients and servers should not depend on request content for methods where its semantics are undefined unless an application-specific agreement explicitly defines the behavior.

# 🗁 Content-Type and Accept

These headers are important when working with HTTP APIs.

They serve different purposes.

### 🖆 Content-Type

`Content-Type` describes the media type of the message body being sent.

Example:

```http
Content-Type: application/json
```

This tells the server that the request body contains JSON.

Common media types:

| Media Type | Description |
| --- | --- |
| `application/json` | JSON data |
| `application/xml` | XML data |
| `application/x-www-form-urlencoded` | URL-encoded form data |
| `multipart/form-data` | Multipart form data |
| `text/plain` | Plain text |
| `text/html` | HTML document |
| `application/pdf` | PDF document |
| `image/png` | PNG image |

### 🖆 Accept

`Accept` describes the response media types the client can process or prefers.

Example:

```http
Accept: application/json
```

This communicates that the client prefers a JSON response.

### 🖆 Content-Type vs Accept

| Header | Direction of Meaning | Purpose |
| --- | --- | --- |
| `Content-Type` | Describes the current message body | What format is being sent? |
| `Accept` | Describes client preferences | What response formats are acceptable? |

Example:

```http
POST /api/users HTTP/1.1
Host: example.com
Content-Type: application/json
Accept: application/json

{
    "name": "Alice"
}
```

Here:

* `Content-Type` says the request body is JSON.
* `Accept` says the client wants a JSON response.

# 🗁 Query Parameters

Query parameters provide additional information in the URL.

They appear after the question mark (`?`) and are separated by ampersands (`&`).

Example:

```text
https://example.com/products?category=electronics&limit=10
```

Parameters:

| Parameter | Value |
| --- | --- |
| `category` | `electronics` |
| `limit` | `10` |

### 🖆 Filtering

```http
GET /products?category=electronics HTTP/1.1
Host: example.com
```

Requests products in the electronics category.

### 🖆 Pagination

```http
GET /products?page=2&limit=20 HTTP/1.1
Host: example.com
```

Requests the second page with up to 20 results.

### 🖆 Sorting

```http
GET /products?sort=price&order=asc HTTP/1.1
Host: example.com
```

Requests products sorted by price in ascending order.

### 🖆 Searching

```http
GET /products?search=keyboard HTTP/1.1
Host: example.com
```

Requests products matching the search term.

### 🖆 Reading Query Parameters with JavaScript

```javascript
const url = new URL(
    "https://example.com/products?category=electronics&limit=10"
);

const category = url.searchParams.get("category");
const limit = url.searchParams.get("limit");

console.log(category);
console.log(limit);
```

Output:

```text
electronics
10
```

Query parameters are useful for filtering, searching, pagination, and other resource-selection operations.

Avoid placing passwords, access tokens, or other sensitive information in URLs because URLs can be recorded in browser history, server logs, and intermediary systems.

# 🗁 Path Parameters

Path parameters identify specific resources through the URL path.

Example:

```text
GET /api/users/25
```

Here, `25` identifies a particular user.

Another example:

```text
GET /api/products/100/reviews
```

The path identifies the reviews associated with product `100`.

### 🖆 Query Parameters vs Path Parameters

| Feature | Path Parameter | Query Parameter |
| --- | --- | --- |
| Location | URL path | URL query string |
| Common purpose | Identify a resource | Filter or modify resource selection |
| Example | `/users/25` | `/users?role=admin` |
| Required status | Often required by route design | Often optional |

The distinction is an API design convention rather than a strict HTTP requirement.

# 🗁 Request Parameters

HTTP applications commonly receive input through several request locations.

| Parameter Type | Example | Common Purpose |
| --- | --- | --- |
| Path parameter | `/users/25` | Identify a resource |
| Query parameter | `?page=2` | Filtering and pagination |
| Header parameter | `Authorization: Bearer ...` | Metadata and authentication |
| Body parameter | `{"name":"Alice"}` | Submit application data |
| Cookie parameter | `session_id=abc123` | Maintain client-associated state |

Example API request:

```http
POST /api/users/25/orders?notify=true HTTP/1.1
Host: example.com
Content-Type: application/json
Authorization: Bearer example-token
Cookie: session_id=abc123

{
    "product_id": 100,
    "quantity": 2
}
```

This request contains:

* Path parameter: `25`
* Query parameter: `notify=true`
* Header parameter: Bearer token
* Cookie: Session identifier
* Body parameters: `product_id` and `quantity`

The server validates and interprets each input according to its application logic.

# 🗁 HTTP Request Flow

A request passes through multiple stages before the application processes it.

Consider a frontend application requesting user information.

```text
Frontend Application
        |
        | GET /api/users/25
        v
HTTP Client
        |
        | DNS Resolution
        v
Destination Server
        |
        | HTTP Request
        v
Reverse Proxy / Gateway
        |
        | Forward Request
        v
Backend Application
        |
        | Route Matching
        v
Authentication
        |
        | Authorization
        v
Business Logic
        |
        | Database Query
        v
Database
        |
        | User Data
        v
Backend Application
        |
        | HTTP Response
        v
HTTP Client
```

The actual architecture may be simpler or more complex.

### 🖆 Request Processing Stages

**Request initiation**

The client creates a request using a URL, method, headers, and optional body.

**Connection establishment**

The client establishes the required transport connection.

**Request transmission**

The client sends the HTTP request.

**Request routing**

The server or reverse proxy determines which application handler should process it.

**Authentication**

The application verifies the client's identity when authentication is required.

**Authorization**

The application checks whether the authenticated identity has permission to perform the operation.

**Validation**

The application validates request parameters and body data.

**Business logic**

The application performs the requested operation.

**Response generation**

The application produces an HTTP response.

# 🗁 HTTP Request Examples

### 🖆 GET Request

Retrieve a list of users.

```http
GET /api/users HTTP/1.1
Host: example.com
Accept: application/json
```

### 🖆 GET Request with Query Parameters

Retrieve active users.

```http
GET /api/users?status=active&page=1 HTTP/1.1
Host: example.com
Accept: application/json
```

### 🖆 GET Request with a Path Parameter

Retrieve a specific user.

```http
GET /api/users/25 HTTP/1.1
Host: example.com
Accept: application/json
```

### 🖆 POST Request

Create a new user.

```http
POST /api/users HTTP/1.1
Host: example.com
Content-Type: application/json
Accept: application/json

{
    "name": "Alice",
    "email": "alice@example.com"
}
```

### 🖆 PUT Request

Replace a user's resource representation.

```http
PUT /api/users/25 HTTP/1.1
Host: example.com
Content-Type: application/json

{
    "name": "Alice Smith",
    "email": "alice.smith@example.com"
}
```

### 🖆 PATCH Request

Partially update a user.

```http
PATCH /api/users/25 HTTP/1.1
Host: example.com
Content-Type: application/json

{
    "name": "Alice Johnson"
}
```

### 🖆 DELETE Request

Delete a user.

```http
DELETE /api/users/25 HTTP/1.1
Host: example.com
```

### 🖆 Request with Authentication

```http
GET /api/profile HTTP/1.1
Host: example.com
Accept: application/json
Authorization: Bearer example-token
```

### 🖆 Request with Cookies

```http
GET /dashboard HTTP/1.1
Host: example.com
Cookie: session_id=abc123
```

# 🗁 Sending Requests with curl

`curl` is a command-line tool for transferring data using HTTP and other protocols.

Install it on Ubuntu:

```bash
sudo apt update
sudo apt install curl
```

### 🖆 Basic GET Request

```bash
curl https://example.com
```

### 🖆 GET Request with Headers

```bash
curl https://example.com/api/users \
  -H "Accept: application/json"
```

### 🖆 POST Request with JSON

```bash
curl https://example.com/api/users \
  -X POST \
  -H "Content-Type: application/json" \
  -H "Accept: application/json" \
  -d '{"name":"Alice","email":"alice@example.com"}'
```

### 🖆 PUT Request

```bash
curl https://example.com/api/users/25 \
  -X PUT \
  -H "Content-Type: application/json" \
  -d '{"name":"Alice Smith","email":"alice@example.com"}'
```

### 🖆 PATCH Request

```bash
curl https://example.com/api/users/25 \
  -X PATCH \
  -H "Content-Type: application/json" \
  -d '{"name":"Alice Johnson"}'
```

### 🖆 DELETE Request

```bash
curl https://example.com/api/users/25 \
  -X DELETE
```

### 🖆 Request with Authorization

```bash
curl https://example.com/api/profile \
  -H "Authorization: Bearer YOUR_ACCESS_TOKEN"
```

### 🖆 Display Request and Response Details

```bash
curl -v https://example.com
```

### 🖆 Display Response Headers

```bash
curl -I https://example.com
```

### 🖆 Save Response Content to a File

```bash
curl https://example.com/document.pdf \
  -o document.pdf
```

# 🗁 Sending Requests with JavaScript

JavaScript provides the Fetch API for making HTTP requests.

### 🖆 GET Request

```javascript
async function getUsers() {
    const response = await fetch(
        "https://example.com/api/users"
    );

    const users = await response.json();

    console.log(users);
}

getUsers();
```

### 🖆 GET Request with Query Parameters

```javascript
async function searchProducts() {
    const url = new URL(
        "https://example.com/api/products"
    );

    url.searchParams.set("category", "electronics");
    url.searchParams.set("limit", "10");

    const response = await fetch(url);

    const products = await response.json();

    console.log(products);
}

searchProducts();
```

### 🖆 POST Request with JSON

```javascript
async function createUser() {
    const response = await fetch(
        "https://example.com/api/users",
        {
            method: "POST",
            headers: {
                "Content-Type": "application/json",
                "Accept": "application/json"
            },
            body: JSON.stringify({
                name: "Alice",
                email: "alice@example.com"
            })
        }
    );

    const user = await response.json();

    console.log(user);
}

createUser();
```

### 🖆 Request with Authorization

```javascript
async function getProfile(token) {
    const response = await fetch(
        "https://example.com/api/profile",
        {
            method: "GET",
            headers: {
                "Authorization": `Bearer ${token}`,
                "Accept": "application/json"
            }
        }
    );

    return response.json();
}
```

### 🖆 Request with FormData

The `FormData` interface is useful for submitting form fields and files.

```javascript
async function uploadFile(file) {
    const formData = new FormData();

    formData.append("title", "Profile Image");
    formData.append("file", file);

    const response = await fetch(
        "https://example.com/api/upload",
        {
            method: "POST",
            body: formData
        }
    );

    return response.json();
}
```

Do not manually set the `Content-Type` header when sending `FormData` through Fetch. The browser must generate the multipart boundary and include it in the header.

### 🖆 Handling HTTP Errors

Fetch does not reject its promise simply because the server returns an HTTP error status such as `404` or `500`.

Check `response.ok` or `response.status`.

```javascript
async function getUsers() {
    try {
        const response = await fetch(
            "https://example.com/api/users"
        );

        if (!response.ok) {
            throw new Error(
                `Request failed: ${response.status}`
            );
        }

        const users = await response.json();

        return users;
    } catch (error) {
        console.error("Request error:", error);
    }
}
```

This distinction is important when building reliable frontend applications.

# 🗁 Common Request Errors

### 🖆 Missing Content-Type

A client sends JSON but does not specify its media type.

Problematic request:

```http
POST /api/users HTTP/1.1
Host: example.com

{"name":"Alice"}
```

Better request:

```http
POST /api/users HTTP/1.1
Host: example.com
Content-Type: application/json

{"name":"Alice"}
```

### 🖆 Incorrect HTTP Method

A client uses `GET` when the API expects `POST`.

The server may return:

```http
HTTP/1.1 405 Method Not Allowed
```

Use the method documented by the API.

### 🖆 Invalid JSON

Example of invalid JSON:

```json
{
    "name": "Alice",
    "email": 
}
```

The server may reject the request because the body cannot be parsed.

### 🖆 Missing Authentication

The client accesses a protected endpoint without valid credentials.

Possible response:

```http
HTTP/1.1 401 Unauthorized
```

### 🖆 Insufficient Permissions

The client is authenticated but does not have permission to access the requested resource.

Possible response:

```http
HTTP/1.1 403 Forbidden
```

### 🖆 Incorrect URL

A request targets an endpoint that does not exist.

Possible response:

```http
HTTP/1.1 404 Not Found
```

### 🖆 CORS Restrictions

A browser application requests a resource from another origin, but the server does not provide the required Cross-Origin Resource Sharing headers.

The browser may block JavaScript from accessing the response.

CORS is a browser-enforced cross-origin access mechanism, not a general restriction imposed by the HTTP protocol on every client.

# 🗁 Best Practices

* Use the correct HTTP method for each operation.
* Use descriptive and consistent resource paths.
* Keep sensitive information out of URLs.
* Send the correct `Content-Type` header.
* Use `Accept` to communicate response format preferences.
* Validate all client-supplied data on the server.
* Authenticate and authorize protected operations.
* Handle non-success HTTP status codes.
* Use HTTPS for sensitive communication.
* Avoid sending unnecessary request data.
* Use pagination for large collections.
* Apply request size limits where appropriate.
* Use consistent API parameter naming.
* Do not assume every HTTP request has a body.
* Do not assume every HTTP response contains JSON.
* Document required headers, parameters, and request bodies.

# 🗁 Practical Exercises

### 🖆 Exercise: Inspect Request Structure

Use curl to send:

```bash
curl -v https://httpbin.org/get
```

Identify:

* HTTP method.
* Request target.
* HTTP version.
* Request headers.
* Response status.
* Response headers.

### 🖆 Exercise: Send Query Parameters

Run:

```bash
curl -G https://httpbin.org/get \
  --data-urlencode "name=Alice" \
  --data-urlencode "role=developer"
```

Inspect the returned query parameters.

### 🖆 Exercise: Send a JSON Request

Run:

```bash
curl https://httpbin.org/post \
  -X POST \
  -H "Content-Type: application/json" \
  -d '{"name":"Alice","role":"developer"}'
```

Observe how the server receives and returns the submitted JSON.

### 🖆 Exercise: Send Different Methods

Test the following requests:

```bash
curl -i https://httpbin.org/get

curl -i -X POST https://httpbin.org/post

curl -i -X PUT https://httpbin.org/put

curl -i -X PATCH https://httpbin.org/patch

curl -i -X DELETE https://httpbin.org/delete
```

Compare their request and response behavior.

### 🖆 Exercise: Build a User API Request

Create a JavaScript function that:

* Retrieves all users.
* Retrieves one user by ID.
* Creates a user.
* Updates a user.
* Deletes a user.
* Handles HTTP errors.

Use Fetch API and JSON.

### 🖆 Exercise: Upload a File

Build a frontend file-upload form using `FormData`.

Submit the file to a backend endpoint and inspect the request using browser Developer Tools.

# 🗁 Interview Questions

### 🖆 Fundamental Questions

**What is an HTTP request?**

An HTTP request is a message sent by a client to a server to access a resource or perform an operation.

**What are the main components of an HTTP/1.1 request?**

A request line, headers, an empty line, and an optional request body.

**What is a request line?**

It is the first line of an HTTP/1.1 request and contains the method, request target, and HTTP version.

**What is a request target?**

It identifies the resource or destination associated with an HTTP request.

**What are HTTP request headers?**

Headers carry metadata about the request, such as authentication credentials, accepted media types, and client information.

### 🖆 Request Body Questions

**What is an HTTP request body?**

It is optional content sent by a client, commonly containing JSON, form data, text, or binary data.

**What is the difference between Content-Type and Accept?**

`Content-Type` describes the media type of the current message body. `Accept` communicates the response media types preferred or supported by the client.

**Can a GET request contain a body?**

HTTP does not generally define semantics for GET request content. Clients and servers should not rely on it for normal API behavior.

**What is multipart/form-data?**

It is a media type used to submit multiple parts in a single message, commonly for forms and file uploads.

### 🖆 API Design Questions

**What is the difference between path and query parameters?**

Path parameters commonly identify a resource, while query parameters commonly filter, sort, search, or paginate results.

**What is the Authorization header?**

It carries credentials used by the server to authenticate a request according to an authentication scheme.

**Why should sensitive information not be placed in query parameters?**

URLs can be stored in browser history, logs, analytics systems, and intermediary systems. Sensitive credentials should be sent through appropriate secure mechanisms instead.

**What is the difference between a 401 and a 403 response?**

`401 Unauthorized` indicates that valid authentication credentials are missing or invalid. `403 Forbidden` indicates that the server refuses the request, commonly because the client lacks permission.

### 🖆 JavaScript Questions

**What is the Fetch API?**

The Fetch API is a browser and JavaScript runtime interface for making network requests and processing responses.

**Does Fetch reject promises for HTTP 404 or 500 responses?**

No. Fetch normally resolves with a `Response` object. Developers should inspect `response.ok` or `response.status`.

**Why should Content-Type not be set manually when sending FormData?**

The browser must generate the multipart boundary and include it in the Content-Type header.

# 🗁 References

* [RFC 9110 — HTTP Semantics](https://www.rfc-editor.org/rfc/rfc9110)
* [RFC 9112 — HTTP/1.1](https://www.rfc-editor.org/rfc/rfc9112)
* [RFC 9113 — HTTP/2](https://www.rfc-editor.org/rfc/rfc9113)
* [RFC 9114 — HTTP/3](https://www.rfc-editor.org/rfc/rfc9114)
* [MDN — HTTP Messages](https://developer.mozilla.org/en-US/docs/Web/HTTP/Messages)
* [MDN — HTTP Headers](https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers)
* [MDN — Fetch API](https://developer.mozilla.org/en-US/docs/Web/API/Fetch_API)
* [MDN — URL API](https://developer.mozilla.org/en-US/docs/Web/API/URL)
* [curl Documentation](https://curl.se/docs/)
