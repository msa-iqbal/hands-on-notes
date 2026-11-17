# Web Storage API

The Web Storage API provides mechanisms for storing key-value data directly in the browser.

It allows web applications to persist data on the client side without requiring a server or database for every operation.

Web Storage provides two main storage mechanisms:

* `localStorage`: Stores data that persists across browser sessions.
* `sessionStorage`: Stores data for the duration of a browser tab's page session.

Common use cases include:

* Saving user preferences.
* Remembering selected themes.
* Storing temporary form data.
* Maintaining simple client-side application state.
* Caching small amounts of application data.

Example:

```javascript
localStorage.setItem("username", "Iqbal");

const username = localStorage.getItem("username");

console.log(username);
```

# 🗁 Understanding Web Storage

Web Storage stores data as key-value pairs.

Each key and value is represented as a string.

Example:

```text
Key          Value
-------------------------
username     Iqbal
theme        dark
language     en
```

Unlike cookies, Web Storage data is not automatically included in HTTP requests.

This makes it useful for client-side data that does not need to be transmitted with every request.

# 🗁 Types of Web Storage

The Web Storage API provides two storage objects.

| Feature | localStorage | sessionStorage |
| --- | --- | --- |
| Lifetime | Persistent | Page session |
| Browser restart | Data usually remains | Data is cleared when the session ends |
| Scope | Same origin | Same origin and top-level browsing context |
| Storage capacity | Browser-dependent | Browser-dependent |
| Data format | Strings | Strings |
| Automatically sent to server | No | No |

Both storage mechanisms are scoped by origin, which includes the protocol, hostname, and port.

# 🗁 Accessing Storage

The storage objects are available through the `window` object.

```javascript
console.log(window.localStorage);

console.log(window.sessionStorage);
```

They can also be accessed directly:

```javascript
console.log(localStorage);

console.log(sessionStorage);
```

# 🗁 localStorage

`localStorage` stores data persistently for a particular origin.

The data normally remains available after:

* Reloading the page.
* Closing the browser tab.
* Closing and reopening the browser.
* Restarting the computer.

Example:

```javascript
localStorage.setItem("theme", "dark");
```

Retrieve the stored value:

```javascript
const theme = localStorage.getItem("theme");

console.log(theme);
```

Output:

```text
dark
```

# 🗁 sessionStorage

`sessionStorage` stores data for the lifetime of a page session.

The data generally remains available while the corresponding tab is open.

It is useful for temporary information that should not persist indefinitely.

Example:

```javascript
sessionStorage.setItem("formStep", "2");
```

Retrieve the stored value:

```javascript
const formStep = sessionStorage.getItem("formStep");

console.log(formStep);
```

When the tab or window session ends, its session storage is cleared.

A duplicated tab or a newly opened tab generally receives a separate session storage area. An initial copy may occur when a tab is opened with an opener, but subsequent changes are independent.

# 🗁 setItem()

The `setItem()` method stores a key-value pair.

Syntax:

```javascript
storage.setItem(key, value);
```

Example:

```javascript
localStorage.setItem("username", "Iqbal");

localStorage.setItem("role", "developer");

sessionStorage.setItem("currentPage", "dashboard");
```

Both arguments are converted to strings.

Example:

```javascript
localStorage.setItem("age", 25);

console.log(localStorage.getItem("age"));
```

Output:

```text
25
```

The retrieved value is the string `"25"`, not the number `25`.

# 🗁 getItem()

The `getItem()` method retrieves a value using its key.

Syntax:

```javascript
storage.getItem(key);
```

Example:

```javascript
localStorage.setItem("username", "Iqbal");

const username = localStorage.getItem("username");

console.log(username);
```

If the key does not exist, `getItem()` returns `null`.

```javascript
const token = localStorage.getItem("token");

console.log(token); // null if missing
```

# 🗁 removeItem()

The `removeItem()` method removes a specific key-value pair.

Syntax:

```javascript
storage.removeItem(key);
```

Example:

```javascript
localStorage.setItem("username", "Iqbal");

localStorage.removeItem("username");

console.log(localStorage.getItem("username")); // null
```

It does not affect other stored entries.

# 🗁 clear()

The `clear()` method removes all entries from the current storage area.

Example:

```javascript
localStorage.setItem("username", "Iqbal");

localStorage.setItem("theme", "dark");

localStorage.clear();
```

After calling `clear()`, the current origin's local storage is empty.

For session storage:

```javascript
sessionStorage.clear();
```

This only clears the current session storage area.

Use `clear()` carefully because it removes every entry belonging to that storage area, including entries created by other parts of the application on the same origin.

# 🗁 key()

The `key()` method retrieves the key at a specified index.

Syntax:

```javascript
storage.key(index);
```

Example:

```javascript
localStorage.setItem("username", "Iqbal");

localStorage.setItem("theme", "dark");

console.log(localStorage.key(0));
console.log(localStorage.key(1));
```

The returned order should not be treated as a reliable application-level ordering mechanism.

If the index does not exist, the method returns `null`.

# 🗁 length

The `length` property returns the number of stored key-value pairs.

Example:

```javascript
localStorage.setItem("username", "Iqbal");

localStorage.setItem("theme", "dark");

console.log(localStorage.length);
```

Output:

```text
2
```

# 🗁 Iterating Over Storage

You can iterate through storage entries using `key()` and `length`.

Example:

```javascript
for (let i = 0; i < localStorage.length; i++) {
  const key = localStorage.key(i);

  const value = localStorage.getItem(key);

  console.log(`${key}: ${value}`);
}
```

You can also use `Object.keys()`:

```javascript
const keys = Object.keys(localStorage);

keys.forEach((key) => {
  console.log(key, localStorage.getItem(key));
});
```

Storage is not a normal JavaScript object, so prefer its dedicated methods for reading and writing data.

# 🗁 Storing Objects and Arrays

Web Storage only stores strings.

To store objects or arrays, convert them into JSON strings using `JSON.stringify()`.

Example:

```javascript
const user = {
  id: 101,
  name: "Iqbal",
  role: "developer"
};

localStorage.setItem("user", JSON.stringify(user));
```

Retrieve and convert the data:

```javascript
const storedUser = localStorage.getItem("user");

const userData = JSON.parse(storedUser);

console.log(userData.name);
```

Output:

```text
Iqbal
```

For arrays:

```javascript
const skills = [
  "JavaScript",
  "TypeScript",
  "React"
];

localStorage.setItem("skills", JSON.stringify(skills));

const storedSkills = JSON.parse(
  localStorage.getItem("skills")
);

console.log(storedSkills);
```

# 🗁 Handling Missing and Invalid JSON

`getItem()` returns `null` when a key does not exist.

Passing `null` to `JSON.parse()` returns `null`, but malformed JSON throws a `SyntaxError`.

A safer approach is to validate and handle parsing errors.

```javascript
function getStoredJSON(key, fallback = null) {
  const value = localStorage.getItem(key);

  if (value === null) {
    return fallback;
  }

  try {
    return JSON.parse(value);
  } catch {
    return fallback;
  }
}
```

Usage:

```javascript
const settings = getStoredJSON("settings", {
  theme: "light"
});

console.log(settings);
```

# 🗁 Storage Events

The `storage` event fires in other documents when a storage area is modified.

It is useful for synchronizing state between tabs or windows belonging to the same origin.

Example:

```javascript
window.addEventListener("storage", (event) => {
  console.log("Storage changed");

  console.log("Key:", event.key);

  console.log("Old value:", event.oldValue);

  console.log("New value:", event.newValue);
});
```

The event provides useful properties:

| Property | Description |
| --- | --- |
| `key` | Changed storage key |
| `oldValue` | Previous value |
| `newValue` | New value |
| `url` | URL of the document that changed storage |
| `storageArea` | Storage object that changed |

Important behavior:

* The event is fired in other documents sharing the same storage area.
* It does not fire in the document that performed the change.
* `event.key` is `null` when `clear()` is called.
* The event is not a general-purpose notification mechanism for arbitrary JavaScript objects.

# 🗁 Example: Synchronizing Theme Across Tabs

```javascript
function setTheme(theme) {
  document.documentElement.dataset.theme = theme;

  localStorage.setItem("theme", theme);
}

window.addEventListener("storage", (event) => {
  if (event.key === "theme" && event.newValue) {
    document.documentElement.dataset.theme = event.newValue;
  }
});
```

When one tab changes the theme, other tabs can update their interface in response.

# 🗁 Storage Limits and Quotas

Web Storage has limited capacity compared with databases or server-side storage.

The exact limits depend on the browser and its storage policies.

Important considerations:

* Do not assume a fixed storage capacity across browsers.
* Storage operations can fail when quotas are exceeded.
* Browser privacy settings may restrict storage.
* Private browsing behavior can differ between browsers.
* Storage access can throw exceptions in restricted environments.

Example of handling storage errors:

```javascript
try {
  localStorage.setItem("theme", "dark");
} catch (error) {
  console.error("Unable to access local storage:", error);
}
```

# 🗁 Security Considerations

Web Storage is accessible to JavaScript running on the same origin.

This has important security implications.

* Do not store passwords in local storage.
* Avoid storing sensitive authentication credentials in JavaScript-accessible storage.
* Never assume browser storage is trusted or tamper-proof.
* Validate stored data before using it.
* Protect applications against Cross-Site Scripting (XSS).
* Do not treat client-side storage as an authorization mechanism.
* Use server-side authorization for protected resources.

An XSS vulnerability can allow injected JavaScript to read or modify Web Storage.

# 🗁 Web Storage vs Cookies

Web Storage and cookies both store client-side data, but they serve different purposes.

| Feature | Web Storage | Cookies |
| --- | --- | --- |
| Data format | String key-value pairs | Name-value pairs |
| Automatically sent with HTTP requests | No | Yes, when applicable |
| JavaScript access | Yes | Depends on cookie attributes |
| Expiration | Defined by storage type | Configurable expiration |
| Server communication | Manual | Automatic according to cookie rules |
| Common use | Client-side preferences and state | Session management and server communication |

Cookies can use security attributes such as:

* `HttpOnly`
* `Secure`
* `SameSite`

An `HttpOnly` cookie cannot be accessed through JavaScript, making it useful for protecting session credentials from direct JavaScript access.

# 🗁 Web Storage vs IndexedDB

IndexedDB is a more powerful browser-side storage mechanism.

| Feature | Web Storage | IndexedDB |
| --- | --- | --- |
| Data model | Key-value strings | Structured records |
| Data types | Strings only | Structured-cloneable values |
| Storage capacity | Relatively limited | Generally much larger |
| API | Synchronous | Asynchronous |
| Transactions | No | Yes |
| Indexing | No | Yes |
| Suitable for | Small preferences and simple state | Larger datasets and offline applications |

Use Web Storage for small, simple data.

Use IndexedDB when an application needs structured storage, larger datasets, indexes, or transactional operations.

# 🗁 Practical Example: Theme Preference

HTML:

```html
<button id="themeToggle">
  Toggle Theme
</button>
```

JavaScript:

```javascript
const button = document.querySelector("#themeToggle");

function applyTheme(theme) {
  document.documentElement.dataset.theme = theme;
}

const savedTheme = localStorage.getItem("theme") ?? "light";

applyTheme(savedTheme);

button.addEventListener("click", () => {
  const currentTheme = localStorage.getItem("theme") ?? "light";

  const nextTheme = currentTheme === "light"
    ? "dark"
    : "light";

  localStorage.setItem("theme", nextTheme);

  applyTheme(nextTheme);
});
```

CSS:

```css
:root {
  color-scheme: light;
  background: white;
  color: black;
}

:root[data-theme="dark"] {
  color-scheme: dark;
  background: #111;
  color: white;
}
```

The selected theme remains available after the browser is reopened.

# 🗁 Best Practices

* Use `localStorage` for small persistent preferences.
* Use `sessionStorage` for temporary tab-specific data.
* Convert objects and arrays using `JSON.stringify()` and `JSON.parse()`.
* Handle missing values and invalid JSON.
* Catch storage access errors.
* Avoid storing sensitive credentials in JavaScript-accessible storage.
* Do not store large datasets in Web Storage.
* Use IndexedDB for complex or large client-side datasets.
* Avoid excessive synchronous storage operations.
* Use the `storage` event when cross-tab synchronization is required.
* Remember that storage is scoped by origin.

# 🗁 Summary

The Web Storage API provides simple client-side persistence through two storage mechanisms.

Important concepts include:

* `localStorage` for persistent storage.
* `sessionStorage` for tab-session storage.
* `setItem()` and `getItem()`.
* `removeItem()` and `clear()`.
* `key()` and `length`.
* JSON serialization for objects and arrays.
* The `storage` event for cross-document synchronization.
* Storage quotas and security considerations.
* Differences between Web Storage, cookies, and IndexedDB.
