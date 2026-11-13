# HTTP Basics

HTTP (Hypertext Transfer Protocol) is an application-layer protocol that defines how clients and servers communicate to exchange resources over a network.

It is one of the fundamental technologies behind the World Wide Web. Every time a browser loads a webpage, a frontend application communicates with an API, or a backend service requests data from another service, HTTP is commonly involved.

HTTP defines:

* How clients request resources.
* How servers respond to requests.
* How request and response data is structured.
* How communication methods are interpreted.
* How status codes represent processing results.
* How metadata is exchanged through headers.
* How caching and content negotiation work.

HTTP is designed as a general-purpose protocol. It can transfer HTML documents, JSON data, images, videos, files, and many other resource representations.

# 🗁 What Is HTTP?

HTTP stands for **Hypertext Transfer Protocol**.

It is an application-layer protocol used to transfer representations of resources between HTTP clients and HTTP servers.

A resource can be almost anything that a server makes available, including:

* An HTML webpage.
* A JSON API response.
* An image.
* A video.
* A CSS stylesheet.
* A JavaScript file.
* A downloadable document.
* Application data.

HTTP defines the communication rules, not the internal implementation of the client or server.

For example, when a browser requests a webpage:

```text
Browser
   |
   | HTTP Request
   v
Web Server
   |
   | HTTP Response
   v
Browser
   |
   | Render HTML
   v
Displayed Webpage
```

The browser does not need to know whether the server uses Node.js, PHP, Python, Java, or another technology. Both sides communicate according to HTTP semantics.

### 🖆 Example of an HTTP Request

```http
GET /products HTTP/1.1
Host: example.com
Accept: application/json
```

This request communicates that the client wants to retrieve the `/products` resource and can accept JSON data.

### 🖆 Example of an HTTP Response

```http
HTTP/1.1 200 OK
Content-Type: application/json

{
    "products": [
        {
            "id": 1,
            "name": "Laptop"
        },
        {
            "id": 2,
            "name": "Keyboard"
        }
    ]
}
```

The server returns a successful response containing a JSON representation of the requested resource.

# 🗁 Why HTTP Is Important

HTTP is essential because it provides a standardized communication mechanism for distributed applications.

Without a common protocol, every client and server implementation would need custom communication rules.

HTTP makes it possible for different technologies to communicate with each other.

### 🖆 Web Browsing

Browsers use HTTP to retrieve:

* HTML documents.
* CSS stylesheets.
* JavaScript files.
* Images and fonts.
* Other webpage resources.

### 🖆 Backend API Communication

Frontend applications communicate with backend services through HTTP APIs.

Example:

```javascript
const response = await fetch(
    "https://api.example.com/users"
);

const users = await response.json();

console.log(users);
```

The browser sends an HTTP request and processes the returned JSON representation.

### 🖆 Microservices Communication

Different backend services can communicate through HTTP-based APIs.

For example:

```text
Order Service
      |
      | HTTP Request
      v
Payment Service
      |
      | HTTP Request
      v
Notification Service
```

### 🖆 File Transfer

HTTP supports transferring files such as:

* Images.
* PDFs.
* Videos.
* Software packages.
* Compressed archives.

### 🖆 Cloud and Distributed Systems

HTTP is widely used by:

* Cloud platforms.
* API gateways.
* Reverse proxies.
* Content delivery networks.
* Authentication services.
* Monitoring systems.

# 🗁 HTTP Architecture

HTTP uses a client-server architecture.

The client initiates communication, and the server provides the requested resource or processes the requested operation.

### 🖆 Client

An HTTP client sends requests to HTTP servers.

Common HTTP clients include:

| Client | Purpose |
| --- | --- |
| Web browser | Loads websites and web resources |
| JavaScript `fetch()` | Sends HTTP requests from browser or supported runtimes |
| Axios | HTTP client library |
| curl | Command-line HTTP client |
| HTTPie | Human-friendly command-line HTTP client |
| Postman | API testing and development |
| Backend service | Communicates with another service |

Example using JavaScript:

```javascript
fetch("https://example.com/api/users")
    .then(response => response.json())
    .then(data => console.log(data))
    .catch(error => console.error(error));
```

### 🖆 Server

An HTTP server receives requests, processes them, and sends responses.

Common HTTP server technologies include:

| Technology | Description |
| --- | --- |
| Nginx | Web server and reverse proxy |
| Apache HTTP Server | Web server |
| Node.js | JavaScript runtime used to build HTTP servers |
| Express.js | Node.js web framework |
| Django | Python web framework |
| Laravel | PHP web framework |
| Spring Boot | Java application framework |
| ASP.NET Core | .NET web framework |

A server can perform different operations depending on the request.

For example:

* Retrieve information from a database.
* Validate submitted data.
* Authenticate a user.
* Process a payment.
* Generate an HTML document.
* Return a JSON response.

### 🖆 Intermediaries

HTTP communication may involve intermediary systems between the client and the origin server.

Common intermediaries include:

* Forward proxies.
* Reverse proxies.
* Gateways.
* Load balancers.
* Caches.
* Content delivery networks.

Example:

```text
          Client
             |
             v
        CDN / Cache
             |
             v
        Load Balancer
             |
             v
        Reverse Proxy
             |
             v
       Application Server
             |
             v
          Database
```

Not every HTTP request passes through all these components. The architecture depends on the application and infrastructure.

# 🗁 How HTTP Works

HTTP communication typically follows a series of steps.

### 🖆 URL Identification

The client starts with a URL identifying the resource it wants to access.

Example:

```text
https://example.com/products?id=10
```

The URL contains several components:

| Component | Value | Description |
| --- | --- | --- |
| Scheme | `https` | Communication scheme |
| Host | `example.com` | Target hostname |
| Path | `/products` | Resource path |
| Query | `id=10` | Additional request parameter |

### 🖆 DNS Resolution

The client needs to determine the IP address associated with the hostname.

DNS resolves:

```text
example.com
      |
      v
DNS Resolution
      |
      v
93.184.216.34
```

The returned address is used to establish communication with the destination server.

DNS resolution may be satisfied by browser caches, operating-system caches, local DNS services, or recursive DNS resolvers.

### 🖆 Connection Establishment

The client establishes a connection using the transport mechanism required by the HTTP version.

| HTTP Version | Transport |
| --- | --- |
| HTTP/1.1 | TCP |
| HTTP/2 | TCP |
| HTTP/3 | QUIC over UDP |

For HTTPS, TLS security is also established. HTTP/3 integrates TLS 1.3 into the QUIC connection establishment process.

### 🖆 Request Transmission

The client sends an HTTP request to the server.

Example:

```http
GET /products?id=10 HTTP/1.1
Host: example.com
Accept: application/json
```

The request communicates:

* The requested operation.
* The target resource.
* The HTTP version in HTTP/1.1 syntax.
* The client's preferences.

### 🖆 Server Processing

The server receives the request and performs the required processing.

For a typical backend application, this may involve:

```text
Incoming Request
       |
       v
Route Matching
       |
       v
Authentication
       |
       v
Input Validation
       |
       v
Business Logic
       |
       v
Database Operation
       |
       v
Response Generation
```

The exact processing sequence depends on the server implementation.

### 🖆 Response Transmission

The server sends an HTTP response.

Example:

```http
HTTP/1.1 200 OK
Content-Type: application/json

{
    "id": 10,
    "name": "Laptop"
}
```

The response contains a status code, metadata, and optionally a message body.

### 🖆 Client Response Processing

The client interprets the response.

For example:

* A browser renders an HTML response.
* A JavaScript application parses JSON.
* A download manager saves a file.
* An API client displays the response status and body.

# 🗁 HTTP Communication Model

HTTP follows a request-response model.

The client initiates a request, and the server returns a corresponding response.

```text
       HTTP Client
            |
            | Request
            |
            v
       HTTP Server
            |
            | Processing
            |
            v
       HTTP Server
            |
            | Response
            |
            v
       HTTP Client
```

### 🖆 Request

A request communicates what the client wants the server to do.

Example:

```http
GET /api/users HTTP/1.1
Host: example.com
Accept: application/json
```

### 🖆 Response

A response communicates the result of processing the request.

Example:

```http
HTTP/1.1 200 OK
Content-Type: application/json

[
    {
        "id": 1,
        "name": "Alice"
    }
]
```

### 🖆 Multiple Requests

A webpage often requires multiple HTTP requests.

For example:

```text
Browser
   |
   | GET /index.html
   v
Web Server
   |
   | HTML Response
   v
Browser
   |
   | GET /style.css
   v
Web Server
   |
   | CSS Response
   v
Browser
   |
   | GET /app.js
   v
Web Server
   |
   | JavaScript Response
   v
Browser
```

The browser discovers additional resources while processing the initial HTML document.

With modern applications, these requests may be processed concurrently using HTTP/2 or HTTP/3.

# 🗁 HTTP Message Fundamentals

HTTP messages carry information between clients and servers.

The logical structure of an HTTP message includes:

* Start line.
* Headers.
* Empty line.
* Optional message body.

HTTP/1.1 represents messages using a textual format. HTTP/2 and HTTP/3 use binary framing to transmit HTTP fields and content.

### 🖆 Request Message

An HTTP/1.1 request contains a request line followed by headers and an optional body.

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

Components:

| Component | Example | Purpose |
| --- | --- | --- |
| Method | `POST` | Requested operation |
| Request target | `/api/users` | Target resource |
| Version | `HTTP/1.1` | HTTP version |
| Header | `Content-Type` | Message metadata |
| Body | JSON object | Submitted data |

### 🖆 Response Message

An HTTP/1.1 response contains a status line followed by headers and an optional body.

```http
HTTP/1.1 201 Created
Content-Type: application/json
Location: /api/users/15

{
    "id": 15,
    "name": "Alice",
    "email": "alice@example.com"
}
```

Components:

| Component | Example | Purpose |
| --- | --- | --- |
| Version | `HTTP/1.1` | HTTP version |
| Status code | `201` | Processing result |
| Reason phrase | `Created` | Human-readable description |
| Header | `Content-Type` | Response metadata |
| Body | JSON object | Returned data |

The reason phrase is part of HTTP/1.1's status-line syntax. HTTP/2 and HTTP/3 transmit the status code through the `:status` pseudo-header instead.

# 🗁 HTTP Statelessness

HTTP is a stateless protocol.

Statelessness means that HTTP does not inherently require the server to remember previous requests when processing a new request.

Each request contains the information needed for the server to interpret that request.

Consider the following requests:

```http
GET /products HTTP/1.1
Host: example.com
```

```http
GET /orders HTTP/1.1
Host: example.com
```

The second request does not automatically inherit application state from the first request.

The server processes each request according to its available information and application logic.

### 🖆 Why HTTP Is Stateless

Stateless communication offers several advantages:

* Requests can be processed independently.
* Servers can scale horizontally more easily.
* Load balancing becomes simpler.
* Failures are easier to recover from.
* Intermediaries can process requests without maintaining application-specific session state.

### 🖆 Maintaining Application State

Web applications often need to remember information across requests.

Common mechanisms include:

| Mechanism | Purpose |
| --- | --- |
| Cookies | Store client-side state |
| Sessions | Maintain server-side user state |
| Access tokens | Represent authenticated authorization |
| Database | Persist application information |
| Client-side storage | Store application data in the browser |

For example, an application may issue a session cookie after login.

The browser sends that cookie with subsequent requests, allowing the server to identify the associated session.

```text
Login Request
      |
      v
Server Creates Session
      |
      v
Server Sends Session Cookie
      |
      v
Browser Stores Cookie
      |
      v
Browser Sends Cookie
      |
      v
Server Identifies Session
```

Cookies and sessions are covered in detail in:

* `07-cookies.md`
* `08-sessions.md`

# 🗁 HTTP Resources and URLs

HTTP operates on resources identified by URIs.

A URL (Uniform Resource Locator) is a type of URI that identifies a resource and provides a way to locate it.

Example:

```text
https://api.example.com:443/v1/users?id=25#profile
```

### 🖆 URL Components

| Component | Value | Description |
| --- | --- | --- |
| Scheme | `https` | Communication scheme |
| Host | `api.example.com` | Server hostname |
| Port | `443` | Destination port |
| Path | `/v1/users` | Resource path |
| Query | `id=25` | Additional parameters |
| Fragment | `profile` | Client-side resource fragment |

The fragment is generally processed by the client and is not included in the HTTP request target.

### 🖆 Resource Identification

A resource represents an entity that can be accessed or manipulated through HTTP.

Examples:

```text
GET /users
GET /users/10
GET /products
GET /products/25
GET /articles/http-basics
```

The server determines how these resource identifiers map to application functionality.

### 🖆 Query Parameters

Query parameters provide additional information to a request.

Example:

```text
GET /products?category=electronics&limit=10
```

Common uses include:

* Filtering.
* Searching.
* Pagination.
* Sorting.
* Selecting resource representations.

Example using JavaScript:

```javascript
const url = new URL(
    "https://example.com/api/products"
);

url.searchParams.set("category", "electronics");
url.searchParams.set("limit", "10");

console.log(url.toString());
```

Output:

```text
https://example.com/api/products?category=electronics&limit=10
```

# 🗁 HTTP Versions

HTTP has evolved through several versions.

### 🖆 HTTP/0.9

HTTP/0.9 was the earliest version of HTTP.

Its design was extremely simple:

* Supported basic GET requests.
* Returned HTML documents.
* Had no request headers.
* Had no response status codes.
* Did not support the modern message structure.

It was designed for the early World Wide Web.

### 🖆 HTTP/1.0

HTTP/1.0 introduced several important improvements:

* Request and response headers.
* Status codes.
* Content types.
* Different HTTP methods.
* Support for different resource representations.

Connections were commonly closed after each request-response exchange unless additional behavior was implemented.

### 🖆 HTTP/1.1

HTTP/1.1 introduced persistent connections by default.

Important features include:

* Host-based virtual hosting.
* Persistent connections.
* Chunked transfer coding.
* Improved caching.
* Content negotiation.
* Range requests.
* Additional request methods.

Persistent connections allow multiple exchanges to reuse the same TCP connection.

### 🖆 HTTP/2

HTTP/2 introduced a binary framing layer.

Important features include:

* Multiplexing.
* Header compression using HPACK.
* Concurrent streams over a connection.
* Stream prioritization mechanisms.
* Reduced application-layer request blocking.

HTTP/2 generally uses TLS in web browsers, although the protocol specification does not require encryption in every deployment.

### 🖆 HTTP/3

HTTP/3 uses QUIC as its transport protocol.

Important features include:

* QUIC over UDP.
* TLS 1.3 integration.
* Independent streams.
* Reduced transport-level head-of-line blocking between streams.
* Connection migration.

HTTP/3 improves transport behavior for modern network environments, especially when connections experience packet loss or network changes.

### 🖆 Version Comparison

| Feature | HTTP/1.1 | HTTP/2 | HTTP/3 |
| --- | --- | --- | --- |
| Transport | TCP | TCP | QUIC over UDP |
| Message transmission | Text-based syntax | Binary framing | Binary framing |
| Multiplexing | No native multiplexing | Yes | Yes |
| Header compression | No built-in compression mechanism | HPACK | QPACK |
| TLS | Optional | Optional by specification; commonly used | Integrated into QUIC |
| Independent streams | No | Limited by TCP transport ordering | Yes |

# 🗁 HTTP and Other Protocols

HTTP does not operate independently. It relies on other network protocols and services.

### 🖆 HTTP and TCP

HTTP/1.1 and HTTP/2 commonly use TCP.

TCP provides:

* Reliable data delivery.
* Ordered byte streams.
* Retransmission of lost data.
* Flow control.
* Congestion control.

HTTP defines application-level communication semantics, while TCP provides transport-level delivery.

### 🖆 HTTP and QUIC

HTTP/3 uses QUIC instead of TCP.

QUIC provides:

* Reliable stream delivery.
* Stream multiplexing.
* Connection establishment.
* Integrated TLS 1.3 security.
* Connection migration.

QUIC uses UDP as its underlying transport.

### 🖆 HTTP and DNS

DNS translates hostnames into IP addresses.

Example:

```text
example.com
     |
     v
    DNS
     |
     v
93.184.216.34
     |
     v
HTTP Connection
```

DNS helps the client locate the destination server before communication begins.

### 🖆 HTTP and TLS

TLS provides cryptographic security for HTTP communication.

It helps provide:

* Confidentiality.
* Integrity.
* Server authentication.

HTTP over TLS is commonly called HTTPS.

### 🖆 HTTP and IP

IP provides addressing and packet routing between network devices.

The relationship can be represented using the TCP/IP model:

```text
Application Layer:
    HTTP

Transport Layer:
    TCP / QUIC

Internet Layer:
    IP

Network Access Layer:
    Ethernet / Wi-Fi
```

# 🗁 HTTP Ports

HTTP services commonly use the following default ports:

| Protocol | Default Port | Transport |
| --- | ---: | --- |
| HTTP | 80 | TCP |
| HTTPS | 443 | TCP |
| HTTP/3 | 443 | UDP |

HTTP/3 uses QUIC over UDP, commonly on port 443.

A server can use a non-default port if its configuration and client URL specify that port.

Example:

```text
http://localhost:3000
https://api.example.com:8443
```

In these examples:

* `3000` is commonly used by development servers.
* `8443` is often used as an alternative HTTPS port.

The port number alone does not determine which protocol is running. The server configuration determines the service behavior.

# 🗁 HTTP Features

HTTP provides several features that make it suitable for modern distributed applications.

### 🖆 Extensibility

HTTP supports extensible headers and method definitions.

Applications can define custom metadata through additional headers when necessary.

Example:

```http
X-Request-ID: req-123456
```

For new application designs, standardized headers or application-specific fields should be preferred over inventing unnecessary custom headers.

### 🖆 Content Negotiation

Clients and servers can negotiate resource representations.

Example:

```http
GET /api/users HTTP/1.1
Host: example.com
Accept: application/json
```

The `Accept` header indicates that the client prefers JSON.

### 🖆 Authentication

HTTP supports authentication-related mechanisms through headers and associated authentication schemes.

Example:

```http
Authorization: Bearer eyJhbGciOi...
```

The server validates the supplied credentials or token according to its authentication system.

### 🖆 Caching

HTTP defines mechanisms for storing and reusing response representations.

Caching can improve performance and reduce server load.

### 🖆 Range Requests

HTTP supports requesting only part of a resource.

Example:

```http
Range: bytes=0-999
```

Range requests are useful for:

* Resumable downloads.
* Large files.
* Media streaming.
* Partial resource retrieval.

### 🖆 Redirection

HTTP allows servers to instruct clients to access another resource.

Example:

```http
HTTP/1.1 301 Moved Permanently
Location: https://example.com/new-page
```

The client can follow the supplied location according to the response semantics and its redirect policy.

# 🗁 HTTP Limitations

HTTP is flexible and widely used, but some characteristics require additional technologies or application-level design.

### 🖆 Statelessness

HTTP does not automatically maintain user-specific application state.

Applications need cookies, sessions, tokens, or other mechanisms when state is required.

### 🖆 No Built-in Encryption

Plain HTTP does not provide confidentiality or cryptographic integrity.

HTTPS adds TLS to protect communication.

### 🖆 Request-Response Communication

Traditional HTTP communication follows a request-response model.

Applications requiring continuous bidirectional communication may use technologies such as:

* WebSocket.
* Server-Sent Events.
* WebRTC data channels.

### 🖆 Application-Level Security

HTTP does not automatically provide application-level authorization or input validation.

Developers must implement appropriate authentication, authorization, validation, and access-control logic.

### 🖆 Performance Considerations

Poorly designed HTTP applications can generate excessive requests, transfer unnecessary data, or use inefficient caching strategies.

HTTP/2, HTTP/3, caching, compression, and application optimization help address these issues.

# 🗁 Practical Examples

### 🖆 Send an HTTP GET Request Using curl

Install curl on Ubuntu:

```bash
sudo apt update
sudo apt install curl
```

Send a GET request:

```bash
curl https://example.com
```

This sends a request to the specified URL and displays the response body.

### 🖆 Inspect Response Headers

```bash
curl -I https://example.com
```

This sends a HEAD request and displays the response headers.

### 🖆 Display Detailed Communication

```bash
curl -v https://example.com
```

Verbose mode provides diagnostic information about connection establishment, request transmission, and response processing.

### 🖆 Send a JSON POST Request

```bash
curl -X POST https://example.com/api/users \
  -H "Content-Type: application/json" \
  -H "Accept: application/json" \
  -d '{"name":"Alice","email":"alice@example.com"}'
```

This sends JSON data to the API endpoint.

### 🖆 Inspect HTTP Status and Headers

```bash
curl -i https://example.com
```

The `-i` option displays response headers along with the response body.

### 🖆 Test an API Using JavaScript

```javascript
async function getUsers() {
    try {
        const response = await fetch(
            "https://jsonplaceholder.typicode.com/users"
        );

        if (!response.ok) {
            throw new Error(
                `HTTP Error: ${response.status}`
            );
        }

        const users = await response.json();

        console.log(users);
    } catch (error) {
        console.error(error);
    }
}

getUsers();
```

This example demonstrates:

* Sending an HTTP request.
* Receiving an HTTP response.
* Checking the response status.
* Parsing JSON.
* Handling request failures.

# 🗁 Common Misconceptions

### 🖆 HTTP and HTML Are the Same

They are different technologies.

* HTTP is a communication protocol.
* HTML is a markup language used to structure web documents.

HTTP can transfer HTML, JSON, images, and many other representations.

### 🖆 HTTP Is a Programming Language

HTTP is a protocol, not a programming language.

It defines communication semantics and message behavior.

### 🖆 HTTPS Is a Completely Different Application Protocol

HTTPS uses HTTP semantics over a secure transport connection established through TLS or, for HTTP/3, through QUIC with integrated TLS 1.3.

### 🖆 HTTP Always Uses TCP

This is incorrect.

* HTTP/1.1 uses TCP.
* HTTP/2 uses TCP.
* HTTP/3 uses QUIC over UDP.

### 🖆 Every HTTP Request Creates a New Connection

This is incorrect.

HTTP/1.1 supports persistent connections, HTTP/2 multiplexes streams over a connection, and HTTP/3 supports multiple streams over a QUIC connection.

### 🖆 HTTP Is Always Stateless at the Application Level

HTTP itself is stateless, but applications can maintain state through cookies, sessions, tokens, databases, and other mechanisms.

# 🗁 Best Practices

* Prefer HTTPS for application communication.
* Use appropriate HTTP methods for resource operations.
* Return meaningful HTTP status codes.
* Set correct `Content-Type` headers.
* Validate and sanitize incoming data.
* Implement authentication and authorization correctly.
* Avoid exposing sensitive information in URLs.
* Configure caching intentionally.
* Use request identifiers for distributed tracing.
* Handle redirects and error responses correctly.
* Use persistent connections and modern HTTP versions where supported.
* Avoid unnecessary request and response payload sizes.
* Document API endpoints and their expected behavior.

# 🗁 Practical Exercises

### 🖆 Exercise: Inspect a Website

Choose a public website and inspect its HTTP communication using browser Developer Tools.

Identify:

* Request URLs.
* HTTP methods.
* Status codes.
* Request headers.
* Response headers.
* Response content types.
* Protocol versions where visible.

### 🖆 Exercise: Test Different HTTP Methods

Use curl to send requests using:

```bash
curl -X GET https://httpbin.org/get

curl -X POST https://httpbin.org/post \
  -H "Content-Type: application/json" \
  -d '{"name":"Alice"}'

curl -X PUT https://httpbin.org/put \
  -H "Content-Type: application/json" \
  -d '{"name":"Bob"}'

curl -X DELETE https://httpbin.org/delete
```

Observe how the server responds to each method.

### 🖆 Exercise: Analyze HTTP Status Codes

Use a testing service to request different status codes:

```bash
curl -i https://httpbin.org/status/200

curl -i https://httpbin.org/status/404

curl -i https://httpbin.org/status/500
```

Compare the returned status codes and response behavior.

### 🖆 Exercise: Build a Simple HTTP Server

Create a basic Node.js HTTP server:

```javascript
const http = require("node:http");

const server = http.createServer((req, res) => {
    if (req.url === "/" && req.method === "GET") {
        res.writeHead(200, {
            "Content-Type": "text/plain"
        });

        res.end("Hello, HTTP!");
        return;
    }

    res.writeHead(404, {
        "Content-Type": "text/plain"
    });

    res.end("Not Found");
});

server.listen(3000, () => {
    console.log("Server running at http://localhost:3000");
});
```

Run the application:

```bash
node server.js
```

Test it:

```bash
curl -i http://localhost:3000
```

This exercise demonstrates how a server receives an HTTP request and generates an HTTP response.

# 🗁 Interview Questions

### 🖆 Fundamental Questions

**What is HTTP?**

HTTP is an application-layer protocol used for communication between clients and servers to exchange resource representations.

**What does HTTP stand for?**

Hypertext Transfer Protocol.

**Why is HTTP called an application-layer protocol?**

Because it defines communication semantics for application-level data exchange rather than handling network routing or transport-level delivery.

**What is the HTTP request-response model?**

It is a communication model in which a client sends a request and a server returns a response.

**What is an HTTP resource?**

A resource is an entity identified by a URI and made available through HTTP.

### 🖆 Architecture Questions

**What is an HTTP client?**

A client is software that initiates HTTP requests, such as a browser, API client, or backend service.

**What is an HTTP server?**

A server receives HTTP requests, processes them, and returns HTTP responses.

**What are HTTP intermediaries?**

Intermediaries are systems such as proxies, gateways, caches, and load balancers that participate in HTTP communication.

**What is the role of DNS in HTTP communication?**

DNS helps resolve a hostname into an IP address so the client can locate the destination server.

### 🖆 Protocol Questions

**Why is HTTP considered stateless?**

Because the protocol does not inherently require the server to retain application state between independent requests.

**Can HTTP maintain user sessions?**

Yes. Applications commonly use cookies, session identifiers, and tokens to maintain state across requests.

**What is the difference between HTTP and HTTPS?**

HTTPS provides HTTP communication protected by TLS, including encryption, integrity protection, and server authentication.

**What is the difference between HTTP/2 and HTTP/3?**

HTTP/2 uses TCP, while HTTP/3 uses QUIC over UDP. Both support multiplexed streams, but HTTP/3 avoids TCP-level head-of-line blocking between independent streams.

**Does HTTP require a specific programming language?**

No. HTTP is language-independent and can be implemented using many programming languages and technologies.

# 🗁 References

* [MDN — An Overview of HTTP](https://developer.mozilla.org/en-US/docs/Web/HTTP/Overview)
* [MDN — HTTP](https://developer.mozilla.org/en-US/docs/Web/HTTP)
* [RFC 9110 — HTTP Semantics](https://www.rfc-editor.org/rfc/rfc9110)
* [RFC 9112 — HTTP/1.1](https://www.rfc-editor.org/rfc/rfc9112)
* [RFC 9113 — HTTP/2](https://www.rfc-editor.org/rfc/rfc9113)
* [RFC 9114 — HTTP/3](https://www.rfc-editor.org/rfc/rfc9114)
* [HTTP Working Group](https://httpwg.org/)
