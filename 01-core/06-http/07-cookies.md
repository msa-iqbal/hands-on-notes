# 🗁 HTTP Cookies

HTTP cookies are small pieces of data that a web server asks a browser to store and send back with subsequent HTTP requests.

Cookies provide a mechanism for maintaining information between requests, even though HTTP is fundamentally a stateless protocol.

They are commonly used for:

* User authentication.
* Session management.
* Remembering user preferences.
* Shopping carts.
* Tracking and analytics.
* Personalizing website experiences.
* Maintaining application state.

A cookie is generally stored by the browser and associated with a particular domain and path.

The browser automatically includes eligible cookies in subsequent HTTP requests.

### 🖆 A Simple Example

A server sends the following response header:

```http
Set-Cookie: session_id=abc123; HttpOnly; Secure; SameSite=Lax
```

The browser stores the cookie.

When the user makes another eligible request to the same website, the browser sends:

```http
Cookie: session_id=abc123
```

The server can use this session identifier to recognize the user's session.

# 🗁 Why Cookies Exist

### 🖆 HTTP Is Stateless

HTTP does not inherently remember previous requests.

Consider this interaction:

```text
Request 1:
GET /login

Response 1:
200 OK

Request 2:
GET /dashboard

Response 2:
401 Unauthorized
```

Even if the user successfully logged in during the first request, the server does not automatically know that the second request belongs to the same authenticated user.

Each HTTP request is independent unless an additional mechanism connects related requests.

Cookies help solve this problem by allowing browsers to store and return information across requests.

### 🖆 Maintaining State with Cookies

A typical authentication workflow looks like this:

```text
Browser                    Server
   |                          |
   |------ Login Request ---->|
   |                          |
   |<----- Set-Cookie --------|
   |                          |
   |   Store Session Cookie   |
   |                          |
   |----- Dashboard Request ->|
   |   Cookie: session_id     |
   |                          |
   |<------ Dashboard --------|
```

The server recognizes the session identifier and retrieves the corresponding session information.

# 🗁 How Cookies Work

Cookies primarily use two HTTP headers:

* `Set-Cookie`: Sent by the server to instruct the browser to store or update a cookie.
* `Cookie`: Sent by the browser to return eligible cookies to the server.

### 🖆 Setting a Cookie

The server sends a response containing `Set-Cookie`:

```http
HTTP/1.1 200 OK
Content-Type: text/html
Set-Cookie: username=iqbal; Path=/; Max-Age=3600
```

The browser processes the header and stores the cookie according to its attributes.

### 🖆 Sending a Cookie

On a subsequent eligible request, the browser automatically includes the cookie:

```http
GET /dashboard HTTP/1.1
Host: example.com
Cookie: username=iqbal
```

The server receives the cookie and can use its value to process the request.

### 🖆 Complete Cookie Lifecycle

```text
             Browser                  Server
                |                        |
                |------ Request -------->|
                |                        |
                |<----- Set-Cookie ------|
                |                        |
                |  Store Cookie          |
                |                        |
                |------ Request -------->|
                |  Cookie: name=value    |
                |                        |
                |<------ Response -------|
                |                        |
```

Important points:

* Cookies are stored and managed by the browser.
* The server sets cookies through response headers.
* The browser sends eligible cookies through request headers.
* Cookies are automatically attached to matching requests.
* Cookie attributes determine their scope, lifetime, and security behavior.

# 🗁 Cookie Syntax

### 🖆 Set-Cookie Header

The general syntax is:

```http
Set-Cookie: name=value; Attribute1; Attribute2
```

Example:

```http
Set-Cookie: theme=dark; Max-Age=86400; Path=/; Secure; SameSite=Lax
```

This creates a cookie named `theme` with the value `dark`.

It is configured to:

* Remain valid for 86,400 seconds.
* Be sent to matching requests under `/`.
* Be transmitted over secure connections.
* Use `Lax` cross-site request behavior.

### 🖆 Cookie Request Header

The browser sends cookies using:

```http
Cookie: name1=value1; name2=value2
```

Example:

```http
Cookie: theme=dark; session_id=abc123; language=en
```

Unlike `Set-Cookie`, the request `Cookie` header does not contain attributes such as `Path`, `Secure`, or `HttpOnly`.

Those attributes control browser behavior and are not included in the request header.

# 🗁 Cookie Attributes

Cookie attributes control how a browser stores, sends, and protects cookies.

### 🖆 Expires

`Expires` defines an absolute expiration date and time.

Example:

```http
Set-Cookie: username=iqbal; Expires=Wed, 31 Dec 2026 23:59:59 GMT
```

The browser can retain the cookie until the specified expiration time.

If the expiration date is in the past, the browser should remove the cookie.

### 🖆 Max-Age

`Max-Age` defines the cookie lifetime in seconds.

Example:

```http
Set-Cookie: session_id=abc123; Max-Age=3600
```

The cookie remains valid for approximately one hour after being set.

Common values:

| Value | Meaning |
| --- | --- |
| `Max-Age=60` | 1 minute |
| `Max-Age=3600` | 1 hour |
| `Max-Age=86400` | 1 day |
| `Max-Age=604800` | 7 days |
| `Max-Age=0` | Delete immediately |

When both `Max-Age` and `Expires` are specified, `Max-Age` takes precedence in browsers that support it.

### 🖆 Domain

`Domain` determines which hosts can receive a cookie.

Example:

```http
Set-Cookie: session_id=abc123; Domain=example.com
```

The cookie can be sent to:

```text
example.com
www.example.com
api.example.com
```

If the `Domain` attribute is omitted, the cookie becomes a host-only cookie.

For example, a cookie set by `www.example.com` without `Domain` is not automatically sent to `api.example.com`.

Browsers reject cookies attempting to use invalid domain scopes, including public suffixes such as `com`.

### 🖆 Path

`Path` determines which URL paths can receive a cookie.

Example:

```http
Set-Cookie: admin_token=abc123; Path=/admin
```

The cookie is sent to matching paths such as:

```text
/admin
/admin/users
/admin/settings
```

It is not normally sent to unrelated paths such as:

```text
/dashboard
/profile
```

The default path is generally derived from the URL path of the request that created the cookie.

`Path` controls cookie delivery, but it is not a strong security boundary between applications or endpoints.

### 🖆 Secure

The `Secure` attribute instructs the browser to send the cookie only over secure connections, normally HTTPS.

Example:

```http
Set-Cookie: session_id=abc123; Secure
```

Without this attribute, a cookie may be transmitted over an unencrypted HTTP connection when its other matching conditions are satisfied.

For authentication cookies, use `Secure`.

### 🖆 HttpOnly

The `HttpOnly` attribute prevents JavaScript from accessing the cookie through browser APIs such as `document.cookie`.

Example:

```http
Set-Cookie: session_id=abc123; HttpOnly
```

JavaScript cannot read this cookie:

```javascript
console.log(document.cookie);
```

However, the browser can still automatically send it with eligible HTTP requests.

This helps protect session identifiers from being directly read by injected JavaScript.

It does not prevent all consequences of cross-site scripting (XSS).

### 🖆 SameSite

`SameSite` controls whether a browser sends a cookie with cross-site requests.

It is particularly important for reducing Cross-Site Request Forgery (CSRF) risks.

There are three primary values.

**Strict**

```http
Set-Cookie: session_id=abc123; SameSite=Strict
```

The browser generally sends the cookie only for same-site requests.

This provides stronger cross-site restrictions but can affect navigation from external websites.

**Lax**

```http
Set-Cookie: session_id=abc123; SameSite=Lax
```

The browser allows cookies in same-site requests and certain cross-site top-level navigations using safe methods, typically GET.

This is a common choice for session cookies.

**None**

```http
Set-Cookie: session_id=abc123; SameSite=None; Secure
```

The browser allows the cookie in cross-site contexts, subject to browser privacy policies and other restrictions.

Modern browsers require `Secure` when `SameSite=None` is explicitly used.

| Attribute | Cross-site behavior |
| --- | --- |
| `Strict` | Generally blocks cross-site cookie sending |
| `Lax` | Allows certain top-level safe navigations |
| `None` | Allows cross-site use, subject to browser policies |

The concept of a site is not identical to an origin. An origin includes scheme, host, and port, whereas site calculations generally use the scheme and registrable domain.

### 🖆 Partitioned

The `Partitioned` attribute is associated with CHIPS (Cookies Having Independent Partitioned State).

It allows a cookie to be partitioned by the top-level site, limiting its sharing across different top-level browsing contexts.

Example:

```http
Set-Cookie: widget_id=123; Secure; SameSite=None; Partitioned
```

This can support embedded services that need separate cookie state for different top-level websites.

Browser support and privacy policies determine actual availability.

# 🗁 Cookie Types

### 🖆 Session Cookies

Session cookies do not specify `Expires` or `Max-Age`.

Example:

```http
Set-Cookie: session_id=abc123; HttpOnly; Secure
```

They are generally removed when the browser session ends.

However, browser session restoration features may preserve session cookies across browser restarts.

### 🖆 Persistent Cookies

Persistent cookies include an expiration attribute.

Example:

```http
Set-Cookie: theme=dark; Max-Age=2592000; Path=/
```

The browser can retain this cookie for approximately 30 days.

Common use cases include:

* Remembering user preferences.
* Remembering language settings.
* Maintaining persistent login experiences.
* Saving non-sensitive personalization settings.

### 🖆 Comparison

| Feature | Session Cookie | Persistent Cookie |
| --- | --- | --- |
| Expiration | No explicit lifetime | Has `Expires` or `Max-Age` |
| Storage duration | Browser-session-based | Until expiration or deletion |
| Common usage | Session management | Preferences and persistent state |
| Browser restart | May be removed | Usually retained |

# 🗁 Cookie Scope

Cookie scope determines when a browser includes a cookie in an HTTP request.

The main factors are:

* Domain.
* Path.
* Secure transport requirements.
* SameSite rules.
* Expiration.
* Browser privacy restrictions.

### 🖆 Host-Only Cookies

A host-only cookie is created when the server omits the `Domain` attribute.

Example:

```http
Set-Cookie: session_id=abc123; Path=/; Secure
```

If the response comes from:

```text
app.example.com
```

The cookie is restricted to that host.

It is not automatically shared with:

```text
api.example.com
```

Host-only cookies are generally preferable when sharing across subdomains is unnecessary.

### 🖆 Domain Cookies

A domain cookie explicitly specifies a domain.

Example:

```http
Set-Cookie: preference=dark; Domain=example.com; Path=/
```

The browser can send it to the specified domain and its eligible subdomains.

This is useful when multiple subdomains need access to the same cookie.

However, sharing cookies across subdomains increases the scope of exposure.

### 🖆 Path Matching

A cookie configured with:

```http
Set-Cookie: admin=enabled; Path=/admin
```

can be sent to matching paths under `/admin`.

A cookie configured with:

```http
Set-Cookie: general=enabled; Path=/
```

can be sent to matching paths throughout the website.

Cookie path matching is not a substitute for server-side authorization.

# 🗁 Accessing Cookies with JavaScript

JavaScript can read and modify cookies that are accessible to it.

The primary browser API is:

```javascript
document.cookie
```

### 🖆 Reading Cookies

```javascript
console.log(document.cookie);
```

Example output:

```text
theme=dark; language=en; username=iqbal
```

Only cookies accessible to the current document are included.

Cookies marked `HttpOnly` are excluded.

### 🖆 Creating a Cookie

```javascript
document.cookie = "theme=dark; Path=/; Max-Age=86400";
```

This creates or updates a cookie named `theme`.

### 🖆 Updating a Cookie

Assigning a cookie with the same name and matching scope updates its value.

```javascript
document.cookie = "theme=light; Path=/; Max-Age=86400";
```

### 🖆 Deleting a Cookie

Set its expiration in the past or use `Max-Age=0`.

```javascript
document.cookie = "theme=; Path=/; Max-Age=0";
```

The deletion must match the original cookie's name, domain scope, and path.

### 🖆 A Cookie Helper

```javascript
function setCookie(name, value, maxAge = 3600) {
  document.cookie = [
    `${encodeURIComponent(name)}=${encodeURIComponent(value)}`,
    `Max-Age=${maxAge}`,
    "Path=/",
    "SameSite=Lax",
  ].join("; ");
}

function getCookie(name) {
  const encodedName = encodeURIComponent(name);

  const cookie = document.cookie
    .split("; ")
    .find((item) => item.startsWith(`${encodedName}=`));

  return cookie
    ? decodeURIComponent(cookie.substring(encodedName.length + 1))
    : null;
}

function deleteCookie(name) {
  document.cookie = [
    `${encodeURIComponent(name)}=`,
    "Max-Age=0",
    "Path=/",
    "SameSite=Lax",
  ].join("; ");
}
```

Usage:

```javascript
setCookie("theme", "dark", 86400);

console.log(getCookie("theme"));

deleteCookie("theme");
```

This helper is intended for JavaScript-accessible cookies. It cannot create an `HttpOnly` cookie or read an existing `HttpOnly` cookie.

For authentication cookies, prefer setting them through the server.

# 🗁 Cookies and Authentication

Cookies are commonly used to maintain authenticated sessions.

A typical application stores a session identifier in a cookie rather than storing the user's complete authentication information in the browser.

### 🖆 Authentication Workflow

```text
Browser                    Backend
   |                          |
   |------ Login Request ---->|
   |                          |
   |   Validate Credentials   |
   |                          |
   |<----- Set-Cookie --------|
   |                          |
   | Store Session ID         |
   |                          |
   |---- Protected Request -->|
   | Cookie: session_id       |
   |                          |
   | Validate Session         |
   |                          |
   |<----- Protected Data ----|
```

### 🖆 Example Authentication Cookie

```http
Set-Cookie: session_id=random-session-id; HttpOnly; Secure; SameSite=Lax; Path=/
```

The browser stores the session identifier.

For subsequent requests, the browser automatically includes it when cookie rules permit.

The backend validates the session identifier and retrieves the corresponding session.

### 🖆 Why Use an Opaque Session Identifier?

An opaque session identifier is a random value that has no meaningful user information embedded in it.

The server uses it to locate session data stored in a database, cache, or session store.

Advantages include:

* Sensitive session data remains server-side.
* The server can invalidate sessions.
* Session expiration can be controlled centrally.
* Authentication state can be updated without changing browser-stored data.

Session identifiers should be generated using cryptographically secure randomness.

# 🗁 Cookies and CSRF Protection

Cross-Site Request Forgery (CSRF) is an attack in which an attacker causes a user's browser to send an unwanted request to a website where the user is already authenticated.

Cookies create an important part of this risk because browsers automatically attach eligible cookies to requests.

### 🖆 Example CSRF Scenario

Imagine a user is logged in to:

```text
https://bank.example
```

An attacker creates a page that submits a request to:

```text
https://bank.example/transfer
```

If the browser includes the user's authentication cookie and the server does not properly validate the request, an unauthorized transfer may occur.

### 🖆 SameSite as a Defense

A session cookie configured with:

```http
Set-Cookie: session_id=abc123; Secure; HttpOnly; SameSite=Lax; Path=/
```

restricts many cross-site cookie submissions.

However, SameSite alone should not be treated as a complete CSRF defense for every application.

### 🖆 Additional CSRF Defenses

Applications can use:

* CSRF tokens.
* Origin header validation.
* Referer validation where appropriate.
* SameSite cookie attributes.
* Proper authorization checks.
* Custom request headers for API interactions.

The server should validate that state-changing requests originate from an expected and authorized context.

# 🗁 Cookie Security

Cookies frequently contain session identifiers and other information that affects application security.

Incorrect cookie configuration can expose authentication sessions or create opportunities for attacks.

### 🖆 Secure and HttpOnly

For authentication cookies, a common configuration is:

```http
Set-Cookie: session_id=abc123; Secure; HttpOnly; SameSite=Lax; Path=/
```

Each attribute serves a different purpose:

| Attribute | Purpose |
| --- | --- |
| `Secure` | Restricts transmission to secure connections |
| `HttpOnly` | Prevents JavaScript access |
| `SameSite=Lax` | Restricts many cross-site requests |
| `Path=/` | Makes the cookie available throughout the site |

### 🖆 Protecting Against XSS

Cross-Site Scripting (XSS) allows attackers to execute malicious JavaScript in a website's context.

Without `HttpOnly`, injected JavaScript may be able to read accessible cookies.

Using `HttpOnly` prevents direct JavaScript access to the cookie value.

However, XSS can still perform actions through the victim's authenticated browser, even when the session cookie is HttpOnly.

Therefore, applications should also use:

* Context-aware output encoding.
* Input validation.
* Content Security Policy (CSP).
* Secure frontend development practices.
* Dependency security checks.

### 🖆 Avoid Sensitive Cookie Values

Do not store sensitive information such as:

* Passwords.
* Raw authentication credentials.
* Private API keys.
* Sensitive personal information.
* Unprotected access tokens.

Prefer storing an opaque, unpredictable session identifier.

### 🖆 Cookie Size and Limits

Cookies are intended for small pieces of data.

Browsers impose limits on cookie size, the number of cookies, and storage per domain. These limits vary by browser and implementation.

Large cookies can increase request overhead because matching cookies are repeatedly transmitted with HTTP requests.

Keep cookie values small and avoid using cookies as general-purpose application storage.

# 🗁 Cookie Prefixes

Cookie prefixes allow browsers to enforce additional requirements when accepting certain cookies.

### 🖆 __Secure-

A cookie beginning with `__Secure-` must be set from a secure context and include the `Secure` attribute.

Example:

```http
Set-Cookie: __Secure-session=abc123; Secure; Path=/
```

### 🖆 __Host-

A cookie beginning with `__Host-` has stricter requirements.

It must:

* Include `Secure`.
* Use `Path=/`.
* Not include a `Domain` attribute.
* Be set from a secure context.

Example:

```http
Set-Cookie: __Host-session=abc123; Secure; HttpOnly; Path=/; SameSite=Lax
```

This creates a host-only cookie that cannot be shared with subdomains through a `Domain` attribute.

For security-sensitive session cookies, `__Host-` can provide stronger scope guarantees when its requirements fit the application.

# 🗁 First-Party and Third-Party Cookies

### 🖆 First-Party Cookies

First-party cookies are associated with the website the user is visiting.

For example, when visiting:

```text
https://example.com
```

a cookie set by `example.com` is generally considered first-party in that context.

Common uses include:

* Authentication.
* Session management.
* Preferences.
* Shopping carts.

### 🖆 Third-Party Cookies

Third-party cookies are associated with a different site from the top-level site being visited.

For example:

```text
Top-level website:
https://news.example

Embedded service:
https://analytics.other
```

If the embedded service sets a cookie, it may be treated as a third-party cookie.

Common historical uses include:

* Cross-site advertising.
* Tracking users across websites.
* Third-party analytics.
* Embedded services.

### 🖆 Modern Browser Restrictions

Browser privacy protections increasingly restrict third-party cookie access.

Behavior differs between browsers, configurations, and privacy settings.

Developers should not assume third-party cookies are available.

Partitioned cookies, privacy-preserving APIs, and first-party integrations are among the approaches used to support certain cross-site use cases.

# 🗁 Cookies vs Web Storage

Browsers provide several mechanisms for client-side storage.

The main options include:

* Cookies.
* `localStorage`.
* `sessionStorage`.

### 🖆 Comparison

| Feature | Cookies | localStorage | sessionStorage |
| --- | --- | --- | --- |
| Storage location | Browser | Browser | Browser tab session |
| Automatically sent with HTTP requests | Yes, when eligible | No | No |
| JavaScript access | Yes, unless HttpOnly | Yes | Yes |
| Expiration | Configurable | Until cleared | Usually until tab session ends |
| Typical capacity | Small | Larger | Larger |
| Server access | Through HTTP headers | Requires application code | Requires application code |
| Common use | Sessions and preferences | Client-side persistent state | Temporary tab state |

### 🖆 When to Use Cookies

Use cookies when information needs to be automatically included in matching HTTP requests.

Examples:

* Session identifiers.
* Authentication state.
* Server-managed preferences.
* CSRF-related state.

### 🖆 When to Use localStorage

Use `localStorage` for client-side data that should remain available across browser sessions.

Examples:

* Non-sensitive UI preferences.
* Theme settings.
* Client-side application configuration.

### 🖆 When to Use sessionStorage

Use `sessionStorage` for temporary data associated with a particular browsing tab.

Examples:

* Temporary form state.
* Multi-step workflow progress.
* Tab-specific UI state.

Neither `localStorage` nor `sessionStorage` automatically sends its contents with HTTP requests.

# 🗁 Working with Cookies

### 🖆 Setting a Cookie Using cURL

You can inspect cookie behavior using cURL.

Send a request to a server that sets a cookie:

```bash
curl -i https://example.com
```

The response may include:

```http
Set-Cookie: session_id=abc123; Path=/; HttpOnly; Secure
```

### 🖆 Saving Cookies to a File

Use the `-c` option to save cookies:

```bash
curl -c cookies.txt https://example.com
```

The cookie information is stored in `cookies.txt`.

### 🖆 Sending Stored Cookies

Use the `-b` option to send cookies from a file:

```bash
curl -b cookies.txt https://example.com/dashboard
```

### 🖆 Saving and Sending Cookies Together

```bash
curl -c cookies.txt -b cookies.txt https://example.com
```

This allows cURL to reuse cookies during subsequent requests.

### 🖆 Inspecting Cookies in Browser Developer Tools

Most modern browsers provide cookie inspection through Developer Tools.

In Chrome or Chromium-based browsers:

```text
Developer Tools
    |
    +-- Application
            |
            +-- Storage
                    |
                    +-- Cookies
```

You can inspect:

* Cookie name.
* Value.
* Domain.
* Path.
* Expiration.
* Secure status.
* HttpOnly status.
* SameSite policy.
* Partitioned status, where supported.

The exact interface may vary between browsers.

### 🖆 Setting Cookies from a Node.js Backend

Example using Express:

```javascript
import express from "express";

const app = express();

app.get("/login", (req, res) => {
  res.cookie("session_id", "random-session-id", {
    httpOnly: true,
    secure: true,
    sameSite: "lax",
    path: "/",
    maxAge: 60 * 60 * 1000,
  });

  res.json({
    message: "Cookie has been set",
  });
});

app.listen(3000);
```

This example demonstrates cookie configuration. A production application should generate a cryptographically secure session identifier and associate it with server-side session data.

### 🖆 Clearing Cookies with Express

```javascript
app.post("/logout", (req, res) => {
  res.clearCookie("session_id", {
    httpOnly: true,
    secure: true,
    sameSite: "lax",
    path: "/",
  });

  res.json({
    message: "Logged out",
  });
});
```

The cookie's scope must match the original cookie configuration for reliable deletion.

# 🗁 Common Mistakes

### 🖆 Storing Sensitive Data in Cookies

Cookies are stored on the client side and may be exposed depending on their configuration and the browser environment.

**Better approach:** Store an opaque session identifier and keep sensitive session data on the server.

### 🖆 Forgetting the Secure Attribute

Authentication cookies without `Secure` may be transmitted over unencrypted connections.

**Better approach:** Use HTTPS and configure authentication cookies with `Secure`.

### 🖆 Assuming HttpOnly Prevents XSS

`HttpOnly` prevents JavaScript from reading the cookie directly, but it does not stop malicious JavaScript from making authenticated requests.

**Better approach:** Combine HttpOnly with XSS prevention and appropriate authorization controls.

### 🖆 Using SameSite=None Without Secure

Modern browsers require `Secure` when `SameSite=None` is explicitly configured.

**Better approach:**

```http
Set-Cookie: widget_id=123; SameSite=None; Secure
```

### 🖆 Incorrect Cookie Deletion

Deleting a cookie with a different path or domain may leave the original cookie untouched.

**Better approach:** Use the same cookie name and matching domain/path scope, then set `Max-Age=0`.

### 🖆 Assuming Cookies Are Shared Across Subdomains

A host-only cookie is not automatically available to sibling subdomains.

**Better approach:** Configure `Domain` only when cross-subdomain sharing is required.

### 🖆 Using Cookies for Large Data

Cookies are included in matching requests and can increase HTTP request size.

**Better approach:** Keep cookies small and use appropriate storage mechanisms for larger client-side data.

### 🖆 Treating Path as a Security Boundary

A cookie's `Path` attribute controls when it is sent, but it does not isolate applications running on the same host.

**Better approach:** Enforce access control and authorization on the server.

# 🗁 Best Practices

* Use HTTPS across the entire application.
* Set `Secure` on authentication cookies.
* Set `HttpOnly` on session identifiers.
* Configure `SameSite` explicitly.
* Prefer host-only cookies when subdomain sharing is unnecessary.
* Consider the `__Host-` prefix for sensitive host-only cookies.
* Use short and appropriate session expiration periods.
* Generate unpredictable session identifiers using cryptographically secure randomness.
* Regenerate session identifiers after authentication and privilege changes.
* Invalidate server-side sessions during logout.
* Avoid storing sensitive information directly in cookies.
* Use CSRF tokens where appropriate.
* Validate authorization on every protected server-side operation.
* Keep cookie values small.
* Avoid relying on third-party cookies for essential application functionality.
* Use appropriate output encoding and XSS protections.
* Review cookie scope and attributes during security testing.

# 🗁 Practical Exercises

### 🖆 Exercise: Create a Cookie

Create a cookie using JavaScript:

```javascript
document.cookie = "username=iqbal; Path=/; Max-Age=3600";
```

Verify it using:

```javascript
console.log(document.cookie);
```

### 🖆 Exercise: Create a Persistent Cookie

Create a cookie that expires after seven days.

```javascript
document.cookie = "theme=dark; Path=/; Max-Age=604800";
```

### 🖆 Exercise: Delete a Cookie

Delete the previously created cookie.

```javascript
document.cookie = "theme=; Path=/; Max-Age=0";
```

### 🖆 Exercise: Inspect Cookie Attributes

Open browser Developer Tools and inspect cookies for a website.

Identify:

* Name and value.
* Domain and path.
* Expiration.
* Secure.
* HttpOnly.
* SameSite.

### 🖆 Exercise: Test Cookie Behavior with cURL

Save cookies:

```bash
curl -c cookies.txt https://example.com
```

Reuse them:

```bash
curl -b cookies.txt https://example.com
```

Inspect the request and response headers.

### 🖆 Exercise: Build an Authentication Cookie

Create a backend endpoint that sets a session cookie with:

* `HttpOnly`.
* `Secure`.
* `SameSite=Lax`.
* `Path=/`.
* An appropriate expiration.

Implement a logout endpoint that invalidates the server-side session and expires the cookie.

# 🗁 Interview Questions

### 🖆 What are HTTP cookies?

Cookies are small pieces of data stored by browsers and associated with websites. They help maintain state between HTTP requests.

### 🖆 Why are cookies needed?

HTTP is stateless. Cookies allow browsers to return information that helps servers recognize sessions, users, and preferences across requests.

### 🖆 What is the difference between Set-Cookie and Cookie?

`Set-Cookie` is a response header used by servers to set cookies.

`Cookie` is a request header used by browsers to send eligible stored cookies.

### 🖆 What is the difference between session and persistent cookies?

Session cookies have no explicit expiration and are generally removed when the browser session ends.

Persistent cookies have an expiration defined by `Expires` or `Max-Age`.

### 🖆 What does HttpOnly do?

It prevents JavaScript from accessing the cookie through APIs such as `document.cookie`.

### 🖆 What does Secure do?

It restricts cookie transmission to secure connections, normally HTTPS.

### 🖆 What is SameSite?

It controls whether cookies are included in cross-site requests. Its values are `Strict`, `Lax`, and `None`.

### 🖆 What is the difference between host-only and domain cookies?

Host-only cookies are restricted to the host that created them.

Domain cookies can be shared with the specified domain and its eligible subdomains.

### 🖆 Does JavaScript have access to all cookies?

No. Cookies marked `HttpOnly` cannot be accessed through JavaScript APIs such as `document.cookie`.

### 🖆 Can JavaScript set HttpOnly cookies?

No. `HttpOnly` cookies must be set through an HTTP response from the server.

### 🖆 What is CSRF, and how do cookies relate to it?

CSRF is an attack that tricks a browser into sending unwanted authenticated requests. Automatically attached cookies can make these attacks possible. SameSite policies and CSRF tokens help defend against them.

### 🖆 What is the difference between cookies and localStorage?

Cookies can be automatically included in matching HTTP requests. `localStorage` is client-side storage and is not automatically transmitted to the server.

### 🖆 What is the __Host- cookie prefix?

It requires a secure context, the `Secure` attribute, `Path=/`, and the absence of a `Domain` attribute.

### 🖆 Can a cookie be shared across subdomains?

Yes, if it is configured with an appropriate `Domain` attribute. Host-only cookies are not shared across sibling subdomains.

### 🖆 What happens when Max-Age and Expires are both specified?

`Max-Age` takes precedence over `Expires` in browsers that support it.

# 🗁 References

* [MDN — HTTP Cookies](https://developer.mozilla.org/en-US/docs/Web/HTTP/Guides/Cookies)
* [MDN — Set-Cookie Header](https://developer.mozilla.org/en-US/docs/Web/HTTP/Reference/Headers/Set-Cookie)
* [MDN — Cookie Header](https://developer.mozilla.org/en-US/docs/Web/HTTP/Reference/Headers/Cookie)
* [MDN — Document.cookie](https://developer.mozilla.org/en-US/docs/Web/API/Document/cookie)
* [RFC 6265 — HTTP State Management Mechanism](https://www.rfc-editor.org/rfc/rfc6265)
* [OWASP — Session Management Cheat Sheet](https://cheatsheetseries.owasp.org/cheatsheets/Session_Management_Cheat_Sheet.html)
* [OWASP — Cross-Site Request Forgery Prevention Cheat Sheet](https://cheatsheetseries.owasp.org/cheatsheets/Cross-Site_Request_Forgery_Prevention_Cheat_Sheet.html)
* [Chrome — Cookies Having Independent Partitioned State (CHIPS)](https://developers.google.com/privacy-sandbox/cookies/chips)
