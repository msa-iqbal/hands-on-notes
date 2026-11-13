# HTTP Methods

HTTP methods define the intended action a client wants a server to perform on a resource. They communicate the semantics of a request, such as retrieving data, creating a resource, updating an existing resource, or deleting a resource.

For example:

```http
GET /users/42 HTTP/1.1
Host: api.example.com
```

This request asks the server to retrieve the representation of the user resource identified by `/users/42`.

HTTP methods are fundamental to web development, RESTful API design, browser communication, and server-side application architecture.

# 🗁 Understanding HTTP Methods

An HTTP method is a token in an HTTP request that indicates the requested operation.

A basic HTTP request has the following structure:

```http
METHOD /resource HTTP/1.1
Host: example.com
Header-Name: value

Optional request body
```

For example:

```http
GET /products HTTP/1.1
Host: api.example.com
Accept: application/json
```

Here:

* `GET` is the HTTP method.
* `/products` is the target resource.
* `HTTP/1.1` identifies the HTTP version.
* `Host` identifies the target server.
* `Accept` indicates the preferred response media type.

HTTP methods describe the intended semantics of a request, not necessarily the internal implementation of the server.

For example, a `DELETE` request asks the server to remove a resource, but the server might implement deletion using a database update, soft deletion, or another internal operation.

### 🖆 Method Case Sensitivity

HTTP method names are case-sensitive.

The standard methods are conventionally written in uppercase:

```http
GET /users
POST /users
PUT /users/42
DELETE /users/42
```

Although lowercase method tokens can technically be different valid tokens, servers and applications generally expect the standard uppercase names.

### 🖆 Standard and Custom Methods

HTTP defines several standardized methods, including:

* GET
* HEAD
* POST
* PUT
* PATCH
* DELETE
* OPTIONS
* CONNECT
* TRACE

Applications can also define extension methods, provided they follow HTTP token syntax and are supported by the communicating systems.

# 🗁 Standard HTTP Methods

## 🕮 GET

`GET` requests a representation of a resource from the server.

It is primarily used to retrieve information without intentionally changing the resource's state.

### 🖆 Syntax

```http
GET /users HTTP/1.1
Host: api.example.com
Accept: application/json
```

### 🖆 Example

Request:

```http
GET /users/42 HTTP/1.1
Host: api.example.com
Accept: application/json
```

Response:

```http
HTTP/1.1 200 OK
Content-Type: application/json

{
  "id": 42,
  "name": "John",
  "email": "john@example.com"
}
```

### 🖆 Common Use Cases

* Retrieving user information.
* Fetching product details.
* Listing blog posts.
* Searching resources.
* Loading application data.
* Retrieving public API information.

### 🖆 GET with Query Parameters

GET requests commonly use query parameters to filter, sort, search, and paginate data.

```http
GET /products?category=electronics&sort=price&page=2
```

### 🖆 Important Characteristics

* Safe.
* Idempotent.
* Cacheable.
* Primarily used to retrieve resource representations.
* Request content has no generally defined semantics.

GET request data should normally be included in the URL through path or query parameters rather than a request body.

## 🕮 HEAD

`HEAD` is similar to `GET`, but the server returns response headers without a response body.

It allows clients to inspect resource metadata without downloading the entire representation.

### 🖆 Syntax

```http
HEAD /products HTTP/1.1
Host: api.example.com
```

### 🖆 Example

Request:

```http
HEAD /images/logo.png HTTP/1.1
Host: example.com
```

Response:

```http
HTTP/1.1 200 OK
Content-Type: image/png
Content-Length: 24576
Last-Modified: Mon, 02 Nov 2026 10:00:00 GMT
ETag: "abc123"
```

Notice that the response contains no message body.

### 🖆 Common Use Cases

* Checking whether a resource exists.
* Inspecting content type.
* Checking resource modification information.
* Retrieving metadata before downloading a file.
* Checking cache validators.

### 🖆 Important Characteristics

* Safe.
* Idempotent.
* Cacheable.
* Has the same response headers as the corresponding GET request, subject to permitted differences.
* Does not return response content.

## 🕮 POST

`POST` submits data to a resource for processing.

The server determines what action to perform based on the target resource and the request content.

It is commonly used to create resources, submit forms, authenticate users, and execute application operations.

### 🖆 Syntax

```http
POST /users HTTP/1.1
Host: api.example.com
Content-Type: application/json

{
  "name": "John",
  "email": "john@example.com"
}
```

### 🖆 Example

Request:

```http
POST /users HTTP/1.1
Host: api.example.com
Content-Type: application/json

{
  "name": "John",
  "email": "john@example.com"
}
```

Response:

```http
HTTP/1.1 201 Created
Content-Type: application/json
Location: /users/42

{
  "id": 42,
  "name": "John",
  "email": "john@example.com"
}
```

The server creates a new user and identifies the created resource using the `Location` header.

### 🖆 Common Use Cases

* Creating new users.
* Submitting registration forms.
* Processing payments.
* Uploading files.
* Sending messages.
* Performing searches when the application requires a request body.
* Triggering server-side operations.

### 🖆 Important Characteristics

* Unsafe.
* Not generally idempotent.
* Not generally cacheable, although caching is possible under explicit HTTP conditions.
* Commonly carries request content.
* Server-side processing determines the result.

### 🖆 Repeated POST Requests

Consider:

```http
POST /orders
```

If the same request is submitted multiple times, the server might create multiple orders.

This is why POST is not generally considered idempotent.

Applications that need retry-safe operations can implement mechanisms such as idempotency keys.

## 🕮 PUT

`PUT` requests that the server create or replace the state of the target resource with the representation supplied by the client.

It is commonly used to replace an existing resource completely.

### 🖆 Syntax

```http
PUT /users/42 HTTP/1.1
Host: api.example.com
Content-Type: application/json

{
  "name": "John Smith",
  "email": "johnsmith@example.com"
}
```

### 🖆 Example

Request:

```http
PUT /users/42 HTTP/1.1
Host: api.example.com
Content-Type: application/json

{
  "name": "John Smith",
  "email": "johnsmith@example.com"
}
```

Response:

```http
HTTP/1.1 200 OK
Content-Type: application/json

{
  "id": 42,
  "name": "John Smith",
  "email": "johnsmith@example.com"
}
```

If the target resource did not previously exist and the server creates it, the response can use:

```http
HTTP/1.1 201 Created
```

### 🖆 Common Use Cases

* Replacing a user profile.
* Updating a complete product representation.
* Creating a resource at a client-selected URI.
* Replacing configuration data.

### 🖆 Important Characteristics

* Unsafe.
* Idempotent.
* Not generally cacheable.
* Usually carries a representation in the request body.
* Intended to replace the target resource's state.

### 🖆 PUT and Missing Fields

Suppose a user resource contains:

```json
{
  "name": "John",
  "email": "john@example.com",
  "role": "admin"
}
```

A PUT request containing:

```json
{
  "name": "John Smith",
  "email": "johnsmith@example.com"
}
```

may replace the previous representation. The `role` field might therefore be removed or assigned a default value, depending on the resource's defined representation and server behavior.

Clients should send the complete representation expected by the API.

## 🕮 PATCH

`PATCH` requests partial modifications to a resource.

Unlike PUT, PATCH is designed to apply changes rather than necessarily replace the entire resource representation.

### 🖆 Syntax

```http
PATCH /users/42 HTTP/1.1
Host: api.example.com
Content-Type: application/json

{
  "name": "John Smith"
}
```

### 🖆 Example

Existing resource:

```json
{
  "id": 42,
  "name": "John",
  "email": "john@example.com",
  "role": "admin"
}
```

Request:

```http
PATCH /users/42 HTTP/1.1
Host: api.example.com
Content-Type: application/json

{
  "name": "John Smith"
}
```

Updated resource:

```json
{
  "id": 42,
  "name": "John Smith",
  "email": "john@example.com",
  "role": "admin"
}
```

Only the requested property changes.

### 🖆 Common PATCH Content Types

PATCH does not require a single universal JSON format. The server and client must agree on the patch document format.

Common formats include:

**JSON Merge Patch**

```http
Content-Type: application/merge-patch+json
```

Example:

```json
{
  "name": "John Smith"
}
```

**JSON Patch**

```http
Content-Type: application/json-patch+json
```

Example:

```json
[
  {
    "op": "replace",
    "path": "/name",
    "value": "John Smith"
  }
]
```

JSON Patch supports operations such as:

* `add`
* `remove`
* `replace`
* `move`
* `copy`
* `test`

### 🖆 Important Characteristics

* Unsafe.
* Not inherently idempotent.
* Not generally cacheable.
* Applies partial modifications.
* Can be designed to be idempotent depending on the patch operation.

For example, setting a user's name to a fixed value is naturally idempotent. Incrementing an account balance by a fixed amount is not.

## 🕮 DELETE

`DELETE` requests that the server remove the association between the target resource and its current functionality.

It is commonly used to delete resources.

### 🖆 Syntax

```http
DELETE /users/42 HTTP/1.1
Host: api.example.com
```

### 🖆 Example

Request:

```http
DELETE /users/42 HTTP/1.1
Host: api.example.com
```

Possible response:

```http
HTTP/1.1 204 No Content
```

A `204 No Content` response indicates that the request was successfully processed and no response body is returned.

Another possible response:

```http
HTTP/1.1 200 OK
Content-Type: application/json

{
  "message": "User deleted successfully"
}
```

### 🖆 Common Use Cases

* Deleting user accounts.
* Removing products.
* Deleting comments.
* Removing uploaded files.
* Deleting application resources.

### 🖆 Important Characteristics

* Unsafe.
* Idempotent.
* Not generally cacheable.
* Request content has no generally defined semantics.

### 🖆 Repeated DELETE Requests

Consider:

```http
DELETE /users/42
```

The first request deletes the resource.

A repeated request might return:

```http
HTTP/1.1 404 Not Found
```

The response is different, but the intended effect remains the same: the resource is absent.

This is why DELETE is considered idempotent.

## 🕮 OPTIONS

`OPTIONS` asks the server which communication options are available for a target resource or server.

It is commonly used for capability discovery and browser CORS preflight requests.

### 🖆 Syntax

```http
OPTIONS /users HTTP/1.1
Host: api.example.com
```

### 🖆 Example

Request:

```http
OPTIONS /users HTTP/1.1
Host: api.example.com
```

Response:

```http
HTTP/1.1 204 No Content
Allow: GET, POST, OPTIONS
```

The `Allow` header identifies methods supported by the resource.

### 🖆 Common Use Cases

* Discovering supported HTTP methods.
* Checking server communication capabilities.
* Supporting CORS preflight requests.
* Inspecting resource capabilities.

### 🖆 Important Characteristics

* Safe.
* Idempotent.
* Not generally cacheable.
* Can be used with `*` as the request target to ask about server-wide capabilities.

Example:

```http
OPTIONS * HTTP/1.1
Host: example.com
```

## 🕮 CONNECT

`CONNECT` asks a proxy to establish a tunnel to a destination server.

It is primarily used to establish HTTPS connections through HTTP proxies.

### 🖆 Example

Request:

```http
CONNECT example.com:443 HTTP/1.1
Host: proxy.example.com:8080
```

Possible response:

```http
HTTP/1.1 200 Connection Established
```

After a successful response, the connection becomes a tunnel through which the client and destination server communicate.

### 🖆 Common Use Cases

* HTTPS connections through HTTP proxies.
* Secure communication through proxy infrastructure.
* Establishing tunnels to destination servers.

### 🖆 Important Characteristics

* Not safe.
* Not idempotent.
* Not cacheable.
* Used to establish a tunnel.
* Usually handled by proxy infrastructure.

## 🕮 TRACE

`TRACE` requests a loop-back diagnostic of the request message received by the server.

It can help diagnose how intermediaries modify or forward requests.

### 🖆 Example

Request:

```http
TRACE / HTTP/1.1
Host: example.com
```

A server that supports TRACE may return information about the received request.

### 🖆 Common Use Cases

* Diagnosing request forwarding.
* Inspecting intermediary behavior.
* Troubleshooting HTTP communication.

### 🖆 Important Characteristics

* Safe.
* Idempotent.
* Not cacheable.
* Intended for diagnostic purposes.
* Commonly disabled on production servers because of potential security exposure.

# 🗁 Safe and Unsafe Methods

HTTP defines method safety based on whether the client requests an operation that is intended to change the server's state.

### 🖆 Safe Methods

A safe method is intended to be read-only from the client's perspective.

Safe methods do not request a state-changing operation.

Safe methods include:

* GET
* HEAD
* OPTIONS
* TRACE

For example:

```http
GET /products
```

The client requests product information rather than asking the server to modify products.

However, safe methods can still produce incidental server-side effects, such as:

* Access logging.
* Analytics tracking.
* Metrics collection.
* Cache updates.
* Audit records.

These incidental effects do not make a method unsafe.

### 🖆 Unsafe Methods

Unsafe methods request operations that can change server state.

Examples include:

* POST
* PUT
* PATCH
* DELETE
* CONNECT

For example:

```http
DELETE /products/42
```

The client explicitly requests a state-changing operation.

### 🖆 Safe vs Unsafe Comparison

| Method | Classification | Typical Purpose |
| --- | --- | --- |
| GET | Safe | Retrieve data |
| HEAD | Safe | Retrieve metadata |
| OPTIONS | Safe | Discover capabilities |
| TRACE | Safe | Diagnostic loop-back |
| POST | Unsafe | Submit or process data |
| PUT | Unsafe | Replace resource |
| PATCH | Unsafe | Partially modify resource |
| DELETE | Unsafe | Remove resource |
| CONNECT | Unsafe | Establish tunnel |

# 🗁 Idempotent and Non-Idempotent Methods

Idempotency describes whether repeating the same request produces the same intended effect as performing it once.

### 🖆 Idempotent Methods

A method is idempotent when multiple identical requests have the same intended effect as a single request.

Standard idempotent methods include:

* GET
* HEAD
* PUT
* DELETE
* OPTIONS
* TRACE

Example:

```http
PUT /users/42
```

Sending the same replacement representation multiple times should leave the resource in the same intended state.

Another example:

```http
DELETE /users/42
```

Deleting an already deleted resource does not change the intended final state.

### 🖆 Non-Idempotent Methods

POST is not generally idempotent.

Example:

```http
POST /orders
```

Sending this request multiple times could create multiple orders.

PATCH is not inherently idempotent. Its idempotency depends on the operation being performed.

For example, this operation can be idempotent:

```json
{
  "status": "active"
}
```

Repeatedly setting the status to `active` produces the same intended state.

This operation is not idempotent:

```json
{
  "increment_balance": 100
}
```

Repeatedly applying it increases the balance each time.

### 🖆 Idempotency Comparison

| Method | Idempotent? |
| --- | --- |
| GET | Yes |
| HEAD | Yes |
| POST | No, generally |
| PUT | Yes |
| PATCH | Not inherently |
| DELETE | Yes |
| OPTIONS | Yes |
| CONNECT | No |
| TRACE | Yes |

### 🖆 Idempotency and API Reliability

Idempotency is particularly important when dealing with:

* Network failures.
* Request retries.
* Payment processing.
* Order creation.
* Distributed systems.
* Unreliable connections.

For operations that use POST, applications can implement an idempotency key.

Example:

```http
POST /payments HTTP/1.1
Host: api.example.com
Content-Type: application/json
Idempotency-Key: payment-transaction-123

{
  "amount": 100,
  "currency": "USD"
}
```

The application stores the key and associates it with the operation result. Repeated requests using the same key can return the existing result rather than performing the operation again.

Idempotency-key behavior is an application-level convention, not a universal guarantee provided by HTTP itself.

# 🗁 Cacheable HTTP Methods

Caching allows clients, proxies, and intermediary systems to reuse stored responses instead of repeatedly requesting the same information from the origin server.

HTTP caching depends on method semantics, response status, and caching headers.

### 🖆 GET

GET responses are commonly cached when the response permits caching.

Example:

```http
GET /products HTTP/1.1
Host: api.example.com
```

A server might return:

```http
HTTP/1.1 200 OK
Cache-Control: public, max-age=3600
Content-Type: application/json
```

This allows eligible caches to store the response for the specified freshness lifetime.

### 🖆 HEAD

HEAD responses are cacheable and can be used to obtain metadata or update stored response metadata under HTTP caching rules.

### 🖆 POST

POST responses can be cached when explicit conditions are satisfied, including appropriate freshness information and cache-related requirements.

However, POST responses are not commonly cached by general-purpose HTTP caches.

### 🖆 Other Methods

PUT, PATCH, DELETE, OPTIONS, CONNECT, and TRACE are not generally used as cacheable methods.

### 🖆 Cacheability Summary

| Method | Cacheability |
| --- | --- |
| GET | Yes |
| HEAD | Yes |
| POST | Possible under explicit conditions |
| PUT | No, not generally |
| PATCH | No, not generally |
| DELETE | No, not generally |
| OPTIONS | No, not generally |
| CONNECT | No |
| TRACE | No |

Caching behavior is also influenced by headers such as:

```http
Cache-Control
Expires
ETag
Last-Modified
Vary
```

These headers are explained in the HTTP headers and caching documentation.

# 🗁 HTTP Method Comparison

The following table summarizes the major characteristics of standard HTTP methods.

| Method | Purpose | Safe | Idempotent | Cacheable |
| --- | --- | --- | --- | --- |
| GET | Retrieve resource | Yes | Yes | Yes |
| HEAD | Retrieve metadata | Yes | Yes | Yes |
| POST | Submit or process data | No | No, generally | Conditional |
| PUT | Replace resource | No | Yes | No |
| PATCH | Partially modify resource | No | Not inherently | No |
| DELETE | Remove resource | No | Yes | No |
| OPTIONS | Discover capabilities | Yes | Yes | No, generally |
| CONNECT | Establish tunnel | No | No | No |
| TRACE | Diagnostic loop-back | Yes | Yes | No |

### 🖆 GET vs POST

| GET | POST |
| --- | --- |
| Retrieves resource representations | Submits data for processing |
| Safe | Unsafe |
| Idempotent | Not generally idempotent |
| Query parameters commonly carry input | Request body commonly carries input |
| Commonly cacheable | Cacheable only under explicit conditions |
| Suitable for retrieval and searches | Suitable for submissions and operations |

### 🖆 PUT vs PATCH

| PUT | PATCH |
| --- | --- |
| Replaces the target resource state | Applies partial modifications |
| Usually sends a complete representation | Sends a patch document |
| Idempotent | Not inherently idempotent |
| Suitable for full updates | Suitable for partial updates |

### 🖆 POST vs PUT

| POST | PUT |
| --- | --- |
| Resource processing is determined by the target resource | Replaces or creates the state of the target resource |
| Often used to create resources | Often used to replace resources |
| Not generally idempotent | Idempotent |
| Server commonly assigns the resource URI | Client commonly identifies the target URI |

# 🗁 HTTP Methods in RESTful APIs

RESTful APIs use HTTP methods to communicate operations on resources.

A resource is commonly represented by a URL.

For example:

```text
/api/users
/api/users/42
/api/products
/api/products/15
```

### 🖆 CRUD Operations

CRUD stands for:

* Create
* Read
* Update
* Delete

These operations commonly map to HTTP methods.

| CRUD Operation | HTTP Method | Example |
| --- | --- | --- |
| Create | POST | `POST /users` |
| Read | GET | `GET /users/42` |
| Update completely | PUT | `PUT /users/42` |
| Update partially | PATCH | `PATCH /users/42` |
| Delete | DELETE | `DELETE /users/42` |

### 🖆 Creating a Resource

```http
POST /api/users
Content-Type: application/json

{
  "name": "Alice",
  "email": "alice@example.com"
}
```

### 🖆 Reading Resources

Retrieve all users:

```http
GET /api/users
```

Retrieve a specific user:

```http
GET /api/users/42
```

### 🖆 Replacing a Resource

```http
PUT /api/users/42
Content-Type: application/json

{
  "name": "Alice Smith",
  "email": "alice.smith@example.com"
}
```

### 🖆 Partially Updating a Resource

```http
PATCH /api/users/42
Content-Type: application/json

{
  "name": "Alice Smith"
}
```

### 🖆 Deleting a Resource

```http
DELETE /api/users/42
```

### 🖆 Designing Resource-Oriented Endpoints

Prefer resource-oriented URLs:

```text
GET    /api/products
GET    /api/products/15
POST   /api/products
PUT    /api/products/15
PATCH  /api/products/15
DELETE /api/products/15
```

Avoid unnecessarily embedding CRUD actions in URLs:

```text
GET  /api/getProducts
POST /api/createProduct
POST /api/deleteProduct/15
```

HTTP methods already communicate the intended operation, so resource-oriented URLs generally provide a cleaner API design.

### 🖆 Custom Business Operations

Not every application operation maps naturally to basic CRUD.

For example, an order cancellation might be modeled as:

```http
POST /api/orders/42/cancellation
```

Or as a state transition:

```http
PATCH /api/orders/42
Content-Type: application/json

{
  "status": "cancelled"
}
```

Choose the approach based on the resource model, business rules, and API contract.

# 🗁 HTTP Methods in HTML Forms

HTML forms allow browsers to submit user input to servers.

The standard HTML form `method` attribute supports:

* GET
* POST

### 🖆 GET Form

```html
<form action="/search" method="GET">
  <input type="text" name="q" placeholder="Search..." />

  <button type="submit">Search</button>
</form>
```

When submitted, the browser encodes the form data into the URL query string.

Example:

```text
/search?q=javascript
```

GET forms are suitable for searches, filters, and other retrieval operations.

### 🖆 POST Form

```html
<form action="/users" method="POST">
  <input type="text" name="name" placeholder="Name" />

  <input type="email" name="email" placeholder="Email" />

  <button type="submit">Create User</button>
</form>
```

The browser sends the form data in the request body.

### 🖆 PUT, PATCH, and DELETE in HTML Forms

Native HTML forms do not directly support PUT, PATCH, or DELETE through their `method` attribute.

A form can use POST and an application can implement a method-override convention.

For example:

```html
<form action="/users/42" method="POST">
  <input type="hidden" name="_method" value="PATCH" />

  <input type="text" name="name" value="John Smith" />

  <button type="submit">Update User</button>
</form>
```

The backend must explicitly recognize and process the `_method` field.

This is an application or framework convention, not native browser HTTP method support.

JavaScript applications can also send PUT, PATCH, and DELETE requests directly using the Fetch API.

# 🗁 HTTP Methods and CORS

Cross-Origin Resource Sharing (CORS) is a browser security mechanism that controls how web pages request resources from different origins.

Some cross-origin requests require a preflight request before the browser sends the actual request.

A CORS preflight uses the HTTP `OPTIONS` method.

### 🖆 Example: CORS Preflight Request

Suppose a frontend application runs at:

```text
https://frontend.example.com
```

And its API runs at:

```text
https://api.example.com
```

The frontend wants to send:

```http
DELETE /users/42
```

The browser may first send:

```http
OPTIONS /users/42 HTTP/1.1
Host: api.example.com
Origin: https://frontend.example.com
Access-Control-Request-Method: DELETE
Access-Control-Request-Headers: authorization
```

### 🖆 Preflight Response

The server can respond:

```http
HTTP/1.1 204 No Content
Access-Control-Allow-Origin: https://frontend.example.com
Access-Control-Allow-Methods: GET, POST, PUT, PATCH, DELETE
Access-Control-Allow-Headers: Authorization, Content-Type
```

If the preflight succeeds, the browser can proceed with the actual request, subject to the remaining CORS checks.

### 🖆 Why OPTIONS Is Important

OPTIONS allows the browser to ask whether the server permits the intended cross-origin communication.

CORS preflight is commonly triggered by requests that use:

* Methods other than GET, HEAD, and POST.
* Certain request headers that are not CORS-safelisted.
* Certain content types outside the CORS-safelisted set.

Not every cross-origin request requires preflight.

# 🗁 Testing HTTP Methods with cURL

`curl` is a command-line tool for transferring data using HTTP and other protocols.

It is useful for testing API endpoints and inspecting HTTP requests and responses.

### 🖆 GET Request

```bash
curl https://api.example.com/users
```

### 🖆 GET Request with Headers

```bash
curl -H "Accept: application/json" \
  https://api.example.com/users
```

### 🖆 POST Request

```bash
curl -X POST https://api.example.com/users \
  -H "Content-Type: application/json" \
  -d '{"name":"John","email":"john@example.com"}'
```

### 🖆 PUT Request

```bash
curl -X PUT https://api.example.com/users/42 \
  -H "Content-Type: application/json" \
  -d '{"name":"John Smith","email":"johnsmith@example.com"}'
```

### 🖆 PATCH Request

```bash
curl -X PATCH https://api.example.com/users/42 \
  -H "Content-Type: application/json" \
  -d '{"name":"John Smith"}'
```

### 🖆 DELETE Request

```bash
curl -X DELETE https://api.example.com/users/42
```

### 🖆 HEAD Request

```bash
curl -I https://api.example.com/users
```

The `-I` option sends a HEAD request and displays response headers.

### 🖆 OPTIONS Request

```bash
curl -X OPTIONS https://api.example.com/users -i
```

### 🖆 TRACE Request

```bash
curl -X TRACE https://example.com -i
```

A server may reject TRACE because it is commonly disabled.

### 🖆 Display Request and Response Details

```bash
curl -v https://api.example.com/users
```

The `-v` option displays verbose connection and HTTP communication details.

### 🖆 Display Only the HTTP Status Code

```bash
curl -s -o /dev/null -w "%{http_code}\n" \
  https://api.example.com/users
```

This is useful when writing shell scripts for API testing.

# 🗁 Using HTTP Methods with JavaScript Fetch

The Fetch API allows JavaScript applications to make HTTP requests.

### 🖆 GET Request

```javascript
const response = await fetch("/api/users");

const users = await response.json();

console.log(users);
```

GET is the default method, so specifying it explicitly is optional.

### 🖆 POST Request

```javascript
const response = await fetch("/api/users", {
  method: "POST",
  headers: {
    "Content-Type": "application/json"
  },
  body: JSON.stringify({
    name: "John",
    email: "john@example.com"
  })
});

const user = await response.json();

console.log(user);
```

### 🖆 PUT Request

```javascript
const response = await fetch("/api/users/42", {
  method: "PUT",
  headers: {
    "Content-Type": "application/json"
  },
  body: JSON.stringify({
    name: "John Smith",
    email: "johnsmith@example.com"
  })
});

const updatedUser = await response.json();

console.log(updatedUser);
```

### 🖆 PATCH Request

```javascript
const response = await fetch("/api/users/42", {
  method: "PATCH",
  headers: {
    "Content-Type": "application/json"
  },
  body: JSON.stringify({
    name: "John Smith"
  })
});

const updatedUser = await response.json();

console.log(updatedUser);
```

### 🖆 DELETE Request

```javascript
const response = await fetch("/api/users/42", {
  method: "DELETE"
});

if (response.ok) {
  console.log("User deleted successfully");
}
```

### 🖆 HEAD Request

```javascript
const response = await fetch("/api/users", {
  method: "HEAD"
});

console.log(response.headers.get("content-type"));
console.log(response.headers.get("content-length"));
```

A HEAD response does not provide a readable response body.

### 🖆 OPTIONS Request

```javascript
const response = await fetch("/api/users", {
  method: "OPTIONS"
});

console.log(response.headers.get("allow"));
```

The server must support the requested operation. Browser cross-origin restrictions also apply.

### 🖆 Handling HTTP Errors

Fetch does not automatically reject its promise for HTTP error status codes such as 404 or 500.

Check `response.ok`:

```javascript
async function getUsers() {
  const response = await fetch("/api/users");

  if (!response.ok) {
    throw new Error(`HTTP Error: ${response.status}`);
  }

  return response.json();
}
```

This ensures that application code handles unsuccessful HTTP responses correctly.

# 🗁 Common Mistakes

### 🖆 Using GET to Modify Server State

Avoid using GET for operations that create, update, or delete resources.

Incorrect:

```http
GET /delete-user?id=42
```

Better:

```http
DELETE /users/42
```

Safe methods should remain safe because browsers, crawlers, and intermediary systems may automatically issue or repeat them.

### 🖆 Treating POST as Idempotent

Repeated POST requests can create duplicate resources or execute operations multiple times.

Use an application-level idempotency mechanism when retry safety is required.

### 🖆 Using PUT for Partial Updates Without Defining the Contract

PUT represents replacement semantics. Sending incomplete data may unintentionally remove or reset fields.

Use PATCH when the API is designed for partial modifications.

### 🖆 Assuming PATCH Is Always Idempotent

PATCH can be idempotent or non-idempotent depending on the patch operation.

Define the behavior clearly in the API contract.

### 🖆 Assuming DELETE Must Return 404 on Repeated Requests

HTTP idempotency does not require identical response codes for repeated requests.

A server may return 204 for the first request and 404 for a later request while preserving the same intended final state.

### 🖆 Sending Request Bodies with GET

GET request content has no generally defined semantics. Some servers and intermediaries may reject or ignore it.

Use query parameters or another method with defined request-content semantics.

### 🖆 Assuming Every Method Is Supported by Every Server

Servers and API endpoints can support different sets of methods.

A server may return:

```http
HTTP/1.1 405 Method Not Allowed
Allow: GET, HEAD
```

The `Allow` header identifies the methods supported by that resource.

# 🗁 Best Practices

* Use GET for retrieving resource representations.
* Use POST for resource processing and operations that are not naturally idempotent.
* Use PUT when replacing a resource representation.
* Use PATCH for partial resource modifications.
* Use DELETE when requesting resource removal.
* Use HEAD when only response metadata is required.
* Use OPTIONS for capability discovery and understand its role in CORS.
* Keep safe methods free from intentionally requested state changes.
* Design APIs around resources rather than embedding every action in URLs.
* Consider idempotency when implementing retries and distributed operations.
* Document the expected request body and content type for POST, PUT, and PATCH.
* Return appropriate HTTP status codes.
* Use authentication and authorization to control access to unsafe operations.
* Validate incoming request data on the server.
* Do not rely on frontend validation alone.
* Make method support explicit in API documentation.

# 🗁 Practical Exercises

### 🖆 Exercise: Build a CRUD API

Design an API for managing blog posts.

Use these endpoints:

```text
GET     /api/posts
GET     /api/posts/1
POST    /api/posts
PUT     /api/posts/1
PATCH   /api/posts/1
DELETE  /api/posts/1
```

For each endpoint, define:

* HTTP method.
* Request URL.
* Request headers.
* Request body, if applicable.
* Expected response status.
* Expected response body.

### 🖆 Exercise: Test Methods with cURL

Use a local API or a test server and practice:

```bash
curl -i https://api.example.com/posts
curl -I https://api.example.com/posts
curl -X OPTIONS https://api.example.com/posts -i
curl -X POST https://api.example.com/posts -i
curl -X PUT https://api.example.com/posts/1 -i
curl -X PATCH https://api.example.com/posts/1 -i
curl -X DELETE https://api.example.com/posts/1 -i
```

Observe how the server responds to supported and unsupported methods.

### 🖆 Exercise: Understand Idempotency

Consider these operations:

```text
Set user name to "Alice"
Increment account balance by 100
Delete product 42
Create a new order
```

Determine which operations are idempotent and explain why.

### 🖆 Exercise: Implement a Fetch-Based API Client

Create JavaScript functions for:

```javascript
getUsers()
getUser(id)
createUser(data)
replaceUser(id, data)
updateUser(id, data)
deleteUser(id)
```

Use Fetch, JSON serialization, and appropriate error handling.

# 🗁 Interview Questions

### 🖆 What is an HTTP method?

An HTTP method indicates the intended operation a client wants to perform on a target resource.

### 🖆 What is the difference between GET and POST?

GET retrieves resource representations and is safe and idempotent. POST submits data for processing and is not generally idempotent.

### 🖆 What is the difference between PUT and PATCH?

PUT requests replacement of the target resource state, while PATCH requests partial modifications.

### 🖆 What does idempotency mean?

An operation is idempotent when repeating the same request has the same intended effect as performing it once.

### 🖆 Which HTTP methods are safe?

GET, HEAD, OPTIONS, and TRACE are safe methods.

### 🖆 Which HTTP methods are idempotent?

GET, HEAD, PUT, DELETE, OPTIONS, and TRACE are idempotent. PATCH may be idempotent depending on its operation.

### 🖆 Is POST idempotent?

POST is not generally idempotent because repeated requests can produce additional effects, such as creating multiple resources.

### 🖆 Is DELETE idempotent?

Yes. Repeated DELETE requests have the same intended final effect, even if their response status codes differ.

### 🖆 What is the purpose of the HEAD method?

HEAD retrieves response metadata without returning a response body.

### 🖆 What is the purpose of OPTIONS?

OPTIONS discovers communication options supported by a server or resource. Browsers also use it for CORS preflight requests.

### 🖆 What is the purpose of CONNECT?

CONNECT establishes a tunnel to a destination server, commonly through an HTTP proxy.

### 🖆 What is the purpose of TRACE?

TRACE provides a diagnostic loop-back of the received request and is commonly disabled on production servers.

### 🖆 Why should GET not be used for deleting resources?

GET is defined as a safe method. Using it for deletion violates HTTP semantics and can cause unintended deletion through crawlers, prefetching, or automated requests.

### 🖆 What is an idempotency key?

An idempotency key is an application-level identifier used to recognize repeated requests and prevent duplicate processing.

### 🖆 Can PATCH be idempotent?

Yes. A PATCH operation can be idempotent when repeating the same patch produces the same intended resource state.

# 🗁 References

* [RFC 9110 — HTTP Semantics](https://www.rfc-editor.org/rfc/rfc9110)
* [RFC 5789 — PATCH Method for HTTP](https://www.rfc-editor.org/rfc/rfc5789)
* [MDN — HTTP Request Methods](https://developer.mozilla.org/en-US/docs/Web/HTTP/Reference/Methods)
* [MDN — HTTP](https://developer.mozilla.org/en-US/docs/Web/HTTP)
* [MDN — Fetch API](https://developer.mozilla.org/en-US/docs/Web/API/Fetch_API)
* [MDN — Cross-Origin Resource Sharing (CORS)](https://developer.mozilla.org/en-US/docs/Web/HTTP/Guides/CORS)
* [RFC 9111 — HTTP Caching](https://www.rfc-editor.org/rfc/rfc9111)
