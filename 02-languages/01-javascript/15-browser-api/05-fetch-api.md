# Fetch API

The Fetch API provides a modern JavaScript interface for making HTTP requests and handling HTTP responses.

It allows web applications to communicate with servers, REST APIs, and other network resources.

Common use cases include:

* Retrieving data from REST APIs.
* Sending form submissions.
* Creating, updating, and deleting server resources.
* Uploading and downloading files.
* Communicating with backend services.
* Loading external resources asynchronously.

The Fetch API uses Promises, making it compatible with `async` and `await`.

Example:

```javascript
fetch("https://jsonplaceholder.typicode.com/users")
  .then((response) => response.json())
  .then((users) => console.log(users))
  .catch((error) => console.error(error));
```

# 🗁 Understanding HTTP Requests

An HTTP request is a message sent from a client to a server.

A request generally contains:

* URL: The resource being requested.
* Method: The intended operation.
* Headers: Additional request information.
* Body: Optional data sent to the server.

Example:

```http
GET /users HTTP/1.1
Host: api.example.com
Accept: application/json
```

The server processes the request and returns an HTTP response.

A response generally contains:

* Status code.
* Response headers.
* Response body.

The Fetch API provides access to these request and response components.

# 🗁 Basic Fetch Syntax

```javascript
fetch(resource, options);
```

Parameters:

* `resource`: The URL or Request object representing the resource.
* `options`: Optional configuration for the request.

The method returns a Promise that fulfills with a `Response` object when a response is available.

Example:

```javascript
const response = await fetch(
  "https://api.example.com/users"
);
```

The Promise does not reject simply because the server returns an HTTP error status such as `404` or `500`.

Network failures, invalid requests, and certain other conditions can cause rejection.

# 🗁 Making a GET Request

GET requests retrieve resources from a server.

Example:

```javascript
async function getUsers() {
  const response = await fetch(
    "https://jsonplaceholder.typicode.com/users"
  );

  const users = await response.json();

  console.log(users);
}

getUsers();
```

GET is the default HTTP method, so it does not need to be specified explicitly.

# 🗁 Understanding the Response Object

A successful Fetch operation provides a `Response` object.

Important properties include:

| Property | Description |
| --- | --- |
| `status` | HTTP status code |
| `statusText` | HTTP status message |
| `ok` | Whether status is in the 200–299 range |
| `headers` | Response headers |
| `url` | Final response URL |
| `redirected` | Whether the response followed a redirect |
| `type` | Response type |

Example:

```javascript
const response = await fetch(
  "https://jsonplaceholder.typicode.com/users"
);

console.log(response.status);
console.log(response.statusText);
console.log(response.ok);
console.log(response.url);
console.log(response.headers);
```

# 🗁 response.ok

The `ok` property indicates whether the HTTP status code represents success.

It returns:

* `true` for status codes from `200` to `299`.
* `false` for other HTTP status codes.

Example:

```javascript
const response = await fetch(
  "https://api.example.com/users"
);

if (response.ok) {
  console.log("Request successful");
} else {
  console.log("HTTP error:", response.status);
}
```

Checking `response.ok` is important because Fetch does not automatically reject its Promise for HTTP error responses.

# 🗁 Reading JSON Responses

The `json()` method reads the response body and parses it as JSON.

Example:

```javascript
const response = await fetch(
  "https://jsonplaceholder.typicode.com/users"
);

const users = await response.json();

console.log(users);
```

The method returns a Promise.

Important: A response body can normally be consumed only once. Calling `json()` reads the body stream, so subsequent attempts to read the same body will fail.

# 🗁 Other Response Body Methods

The Response object provides several methods for reading response bodies.

| Method | Description |
| --- | --- |
| `json()` | Parses the body as JSON |
| `text()` | Reads the body as text |
| `blob()` | Reads the body as a Blob |
| `arrayBuffer()` | Reads the body as an ArrayBuffer |
| `formData()` | Parses form data |
| `bytes()` | Reads the body as a byte array |

Example:

```javascript
const response = await fetch("/message.txt");

const text = await response.text();

console.log(text);
```

Reading a JSON response:

```javascript
const response = await fetch("/api/users");

const users = await response.json();
```

Reading a binary response:

```javascript
const response = await fetch("/images/photo.jpg");

const imageBlob = await response.blob();

console.log(imageBlob);
```

Choose the method based on the expected response format.

# 🗁 Handling HTTP Errors

Fetch does not reject its Promise when the server returns an HTTP error status.

For example, a `404 Not Found` response can still fulfill the Fetch Promise.

A common pattern is to check `response.ok`.

```javascript
async function getUsers() {
  const response = await fetch(
    "https://jsonplaceholder.typicode.com/users"
  );

  if (!response.ok) {
    throw new Error(
      `HTTP error: ${response.status}`
    );
  }

  return response.json();
}

getUsers()
  .then((users) => console.log(users))
  .catch((error) => console.error(error));
```

This ensures that unsuccessful HTTP responses are handled explicitly.

# 🗁 Handling Network Errors

A Fetch request can fail because of:

* Network connectivity problems.
* DNS resolution failures.
* Connection errors.
* CORS restrictions.
* Request cancellation.

Use `try...catch` with `async` and `await`.

```javascript
async function getUsers() {
  try {
    const response = await fetch(
      "https://api.example.com/users"
    );

    if (!response.ok) {
      throw new Error(
        `HTTP error: ${response.status}`
      );
    }

    const users = await response.json();

    console.log(users);
  } catch (error) {
    console.error("Request failed:", error);
  }
}
```

This example handles both Fetch failures and errors explicitly thrown for unsuccessful HTTP responses.

# 🗁 Making a POST Request

POST requests are commonly used to create resources or submit data.

The `method`, `headers`, and `body` options configure the request.

Example:

```javascript
async function createUser() {
  const response = await fetch(
    "https://api.example.com/users",
    {
      method: "POST",

      headers: {
        "Content-Type": "application/json"
      },

      body: JSON.stringify({
        name: "Iqbal",
        email: "iqbal@example.com"
      })
    }
  );

  if (!response.ok) {
    throw new Error(
      `HTTP error: ${response.status}`
    );
  }

  const user = await response.json();

  console.log(user);
}
```

The request body is serialized into JSON, and the `Content-Type` header identifies the submitted format.

# 🗁 Making a PUT Request

PUT requests generally replace the representation of an existing resource.

Example:

```javascript
async function updateUser() {
  const response = await fetch(
    "https://api.example.com/users/1",
    {
      method: "PUT",

      headers: {
        "Content-Type": "application/json"
      },

      body: JSON.stringify({
        name: "Iqbal",
        email: "new@example.com"
      })
    }
  );

  if (!response.ok) {
    throw new Error(
      `HTTP error: ${response.status}`
    );
  }

  const updatedUser = await response.json();

  console.log(updatedUser);
}
```

PUT is commonly used when the client submits a complete replacement representation.

# 🗁 Making a PATCH Request

PATCH requests partially update an existing resource.

Example:

```javascript
async function updateUserEmail() {
  const response = await fetch(
    "https://api.example.com/users/1",
    {
      method: "PATCH",

      headers: {
        "Content-Type": "application/json"
      },

      body: JSON.stringify({
        email: "updated@example.com"
      })
    }
  );

  if (!response.ok) {
    throw new Error(
      `HTTP error: ${response.status}`
    );
  }

  const updatedUser = await response.json();

  console.log(updatedUser);
}
```

Unlike a typical PUT operation, PATCH does not require replacing the entire resource representation.

The exact update behavior depends on the API.

# 🗁 Making a DELETE Request

DELETE requests ask the server to remove a resource.

Example:

```javascript
async function deleteUser() {
  const response = await fetch(
    "https://api.example.com/users/1",
    {
      method: "DELETE"
    }
  );

  if (!response.ok) {
    throw new Error(
      `HTTP error: ${response.status}`
    );
  }

  console.log("User deleted successfully");
}
```

A successful DELETE request may return `200 OK` or `204 No Content`.

A `204` response has no response body, so do not attempt to parse it as JSON.

# 🗁 HTTP Methods Reference

| Method | Typical Purpose |
| --- | --- |
| GET | Retrieve a resource |
| POST | Create or submit data |
| PUT | Replace a resource |
| PATCH | Partially update a resource |
| DELETE | Remove a resource |
| HEAD | Retrieve response headers without the response body |
| OPTIONS | Discover communication options |

# 🗁 Request Headers

Headers provide additional information about an HTTP request.

Example:

```javascript
const response = await fetch(
  "https://api.example.com/users",
  {
    headers: {
      "Accept": "application/json",
      "Authorization": "Bearer YOUR_TOKEN"
    }
  }
);
```

Common request headers include:

| Header | Purpose |
| --- | --- |
| `Accept` | Expected response media type |
| `Content-Type` | Format of the request body |
| `Authorization` | Authentication credentials |
| `Cache-Control` | Caching directives |
| `Accept-Language` | Preferred response language |

Do not hardcode real credentials into publicly accessible frontend source code.

# 🗁 Working with Response Headers

The `headers` property provides access to response headers.

Example:

```javascript
const response = await fetch(
  "https://api.example.com/users"
);

console.log(
  response.headers.get("Content-Type")
);

console.log(
  response.headers.get("Cache-Control")
);
```

You can also iterate through available headers:

```javascript
response.headers.forEach((value, key) => {
  console.log(`${key}: ${value}`);
});
```

Some response headers may not be accessible to frontend JavaScript because of browser security policies, including CORS restrictions.

# 🗁 Query Parameters

Query parameters are commonly used for filtering, searching, sorting, and pagination.

The `URL` and `URLSearchParams` APIs make it easier to construct URLs.

Example:

```javascript
const url = new URL(
  "https://api.example.com/users"
);

url.searchParams.set("page", "1");

url.searchParams.set("limit", "10");

url.searchParams.set("search", "Iqbal");

const response = await fetch(url);

const users = await response.json();

console.log(users);
```

Generated URL:

```text
https://api.example.com/users?page=1&limit=10&search=Iqbal
```

Using `URLSearchParams` avoids manually encoding query parameter values.

# 🗁 Sending Form Data

The `FormData` interface allows you to submit form fields and files.

Example:

```html
<form id="userForm">
  <input name="username" placeholder="Username">

  <input name="email" type="email">

  <button type="submit">Submit</button>
</form>
```

JavaScript:

```javascript
const form = document.querySelector("#userForm");

form.addEventListener("submit", async (event) => {
  event.preventDefault();

  const formData = new FormData(form);

  try {
    const response = await fetch("/api/users", {
      method: "POST",
      body: formData
    });

    if (!response.ok) {
      throw new Error(
        `HTTP error: ${response.status}`
      );
    }

    const result = await response.json();

    console.log(result);
  } catch (error) {
    console.error(error);
  }
});
```

When sending `FormData`, do not manually set the `Content-Type` header to `multipart/form-data`.

The browser automatically sets the correct content type, including the required multipart boundary.

# 🗁 Uploading Files

The Fetch API supports file uploads using `FormData`.

HTML:

```html
<input type="file" id="fileInput">

<button id="uploadButton">Upload</button>
```

JavaScript:

```javascript
const fileInput = document.querySelector("#fileInput");

const uploadButton = document.querySelector("#uploadButton");

uploadButton.addEventListener("click", async () => {
  const file = fileInput.files[0];

  if (!file) {
    console.log("Select a file first");
    return;
  }

  const formData = new FormData();

  formData.append("file", file);

  try {
    const response = await fetch("/api/upload", {
      method: "POST",
      body: formData
    });

    if (!response.ok) {
      throw new Error(
        `Upload failed: ${response.status}`
      );
    }

    console.log("Upload completed");
  } catch (error) {
    console.error(error);
  }
});
```

The server must support the expected multipart request format and process the uploaded file.

# 🗁 Downloading Files

The Fetch API can retrieve binary data using `blob()`.

Example:

```javascript
async function downloadFile() {
  const response = await fetch("/files/report.pdf");

  if (!response.ok) {
    throw new Error(
      `Download failed: ${response.status}`
    );
  }

  const blob = await response.blob();

  const url = URL.createObjectURL(blob);

  const link = document.createElement("a");

  link.href = url;

  link.download = "report.pdf";

  link.click();

  URL.revokeObjectURL(url);
}
```

The `Blob` object represents binary data, while `URL.createObjectURL()` creates a temporary URL that can be used to access it.

For large files, consider browser-native navigation or streaming approaches instead of loading the entire response into memory.

# 🗁 Request Configuration Options

The Fetch API supports several request configuration options.

| Option | Description |
| --- | --- |
| `method` | HTTP method |
| `headers` | Request headers |
| `body` | Request body |
| `mode` | Request mode |
| `credentials` | Credential handling |
| `cache` | Cache behavior |
| `redirect` | Redirect handling |
| `referrer` | Referrer information |
| `signal` | Request cancellation signal |
| `integrity` | Subresource integrity metadata |
| `keepalive` | Allows certain requests to outlive page navigation |

Example:

```javascript
const response = await fetch(
  "https://api.example.com/users",
  {
    method: "GET",

    mode: "cors",

    credentials: "same-origin",

    cache: "no-cache",

    redirect: "follow",

    headers: {
      Accept: "application/json"
    }
  }
);
```

Choose options according to the application's networking requirements.

# 🗁 Fetch Credentials

The `credentials` option controls whether credentials such as cookies and HTTP authentication information are included with a request.

Available values:

| Value | Behavior |
| --- | --- |
| `omit` | Never include credentials |
| `same-origin` | Include credentials for same-origin requests |
| `include` | Include credentials for cross-origin requests when permitted |

Example:

```javascript
const response = await fetch(
  "https://api.example.com/profile",
  {
    credentials: "include"
  }
);
```

For cross-origin credentialed requests, the server must explicitly allow the requesting origin and credentials through the appropriate CORS response headers.

# 🗁 AbortController

`AbortController` provides a way to cancel Fetch requests.

It is useful when:

* A user navigates away from a page.
* A search query becomes outdated.
* A component is removed.
* A request takes too long.
* A newer request replaces an older request.

Example:

```javascript
const controller = new AbortController();

fetch("/api/users", {
  signal: controller.signal
})
  .then((response) => response.json())
  .then((users) => console.log(users))
  .catch((error) => {
    if (error.name === "AbortError") {
      console.log("Request cancelled");
    } else {
      console.error(error);
    }
  });

// Cancel the request
controller.abort();
```

Calling `abort()` causes the associated Fetch operation to reject with an abort-related error.

# 🗁 Request Timeout

Fetch does not provide a dedicated timeout option.

A common approach is to use `AbortSignal.timeout()`.

Example:

```javascript
async function getUsers() {
  try {
    const response = await fetch("/api/users", {
      signal: AbortSignal.timeout(5000)
    });

    if (!response.ok) {
      throw new Error(
        `HTTP error: ${response.status}`
      );
    }

    return await response.json();
  } catch (error) {
    if (error.name === "TimeoutError") {
      console.error("Request timed out");
    } else {
      console.error(error);
    }
  }
}
```

The timeout value is specified in milliseconds.

# 🗁 Combining Abort Signals

`AbortSignal.any()` allows multiple abort signals to control a single request.

Example:

```javascript
const controller = new AbortController();

const timeoutSignal = AbortSignal.timeout(5000);

const signal = AbortSignal.any([
  controller.signal,
  timeoutSignal
]);

fetch("/api/users", { signal })
  .then((response) => response.json())
  .then(console.log)
  .catch(console.error);

// Cancel manually if necessary
controller.abort();
```

The request is aborted when any of the supplied signals is aborted.

# 🗁 CORS and Fetch

Cross-Origin Resource Sharing (CORS) is a browser security mechanism that controls whether frontend JavaScript can access resources from another origin.

For example:

```javascript
fetch("https://api.example.com/users");
```

If the frontend and API have different origins, the browser may require the server to return appropriate CORS headers.

Example response header:

```http
Access-Control-Allow-Origin: https://frontend.example.com
```

Some requests trigger a preflight request using the `OPTIONS` method.

The preflight request checks whether the server permits the intended cross-origin operation.

Important:

* CORS is enforced by browsers.
* Frontend JavaScript cannot bypass CORS restrictions.
* The server must configure appropriate CORS response headers.
* CORS is not an authentication or authorization mechanism.

# 🗁 Understanding Fetch and HTTP Status Codes

Common HTTP response statuses include:

| Status | Meaning |
| --- | --- |
| `200 OK` | Request successful |
| `201 Created` | Resource created |
| `202 Accepted` | Request accepted for processing |
| `204 No Content` | Request successful with no response body |
| `301 Moved Permanently` | Permanent redirect |
| `302 Found` | Temporary redirect |
| `400 Bad Request` | Invalid request |
| `401 Unauthorized` | Authentication required or invalid |
| `403 Forbidden` | Access denied |
| `404 Not Found` | Resource not found |
| `409 Conflict` | Request conflicts with resource state |
| `422 Unprocessable Content` | Request content cannot be processed |
| `429 Too Many Requests` | Rate limit exceeded |
| `500 Internal Server Error` | Server error |
| `502 Bad Gateway` | Invalid upstream response |
| `503 Service Unavailable` | Service unavailable |
| `504 Gateway Timeout` | Upstream request timed out |

The exact response behavior depends on the server implementation.

# 🗁 Handling a 204 No Content Response

A `204 No Content` response does not contain a response body.

Do not call `response.json()` on such a response.

Example:

```javascript
async function deleteUser(id) {
  const response = await fetch(`/api/users/${id}`, {
    method: "DELETE"
  });

  if (!response.ok) {
    throw new Error(
      `HTTP error: ${response.status}`
    );
  }

  if (response.status === 204) {
    return null;
  }

  return response.json();
}
```

This avoids attempting to parse an empty response body as JSON.

# 🗁 Reusing Fetch Logic

Applications often need consistent request handling.

A reusable API function can centralize error checking and response parsing.

```javascript
async function apiRequest(url, options = {}) {
  const response = await fetch(url, {
    ...options,

    headers: {
      Accept: "application/json",
      ...options.headers
    }
  });

  if (!response.ok) {
    throw new Error(
      `HTTP error: ${response.status}`
    );
  }

  if (response.status === 204) {
    return null;
  }

  return response.json();
}
```

Usage:

```javascript
const users = await apiRequest("/api/users");

const user = await apiRequest("/api/users/1");

const createdUser = await apiRequest("/api/users", {
  method: "POST",

  headers: {
    "Content-Type": "application/json"
  },

  body: JSON.stringify({
    name: "Iqbal"
  })
});
```

This pattern reduces duplicated request-handling code.

# 🗁 Fetch API vs XMLHttpRequest

`XMLHttpRequest` is an older browser API for making HTTP requests.

| Feature | Fetch API | XMLHttpRequest |
| --- | --- | --- |
| Programming model | Promise-based | Event-based |
| Async/await support | Native | Requires wrapping |
| Request configuration | Options object | Methods and properties |
| Response handling | Response object | XMLHttpRequest object |
| Cancellation | AbortController | `abort()` |
| Progress events | No general built-in upload/download progress events | Supports progress events |
| Modern application development | Preferred | Useful for specific legacy and progress-tracking requirements |

The Fetch API is generally preferred for modern JavaScript applications.

# 🗁 Best Practices

* Use `async` and `await` for readable asynchronous request handling.
* Always check `response.ok` when unsuccessful HTTP statuses should be treated as errors.
* Handle network errors using `try...catch`.
* Use the correct HTTP method for each operation.
* Set appropriate request headers.
* Serialize JSON request bodies using `JSON.stringify()`.
* Do not manually set multipart boundaries when submitting `FormData`.
* Handle `204 No Content` responses separately.
* Use `AbortController` when requests need cancellation.
* Use timeouts for operations that should not wait indefinitely.
* Construct query parameters using `URLSearchParams`.
* Keep sensitive credentials out of publicly accessible frontend source code.
* Configure CORS on the server.
* Avoid assuming that a successful HTTP response always contains valid JSON.
* Avoid automatically retrying non-idempotent operations without a deliberate retry strategy.

# 🗁 Summary

The Fetch API is the modern browser interface for HTTP communication.

Important concepts include:

* `fetch()` for sending requests.
* `Response` for handling HTTP responses.
* `response.ok` and `response.status`.
* `json()`, `text()`, `blob()`, and other body methods.
* GET, POST, PUT, PATCH, and DELETE requests.
* Request and response headers.
* Query parameters and FormData.
* File uploads and downloads.
* `AbortController` and request timeouts.
* CORS and credential handling.
* Error handling and reusable API functions.
