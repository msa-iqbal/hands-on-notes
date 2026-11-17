# Notifications API

The Notifications API allows web applications to display system-level notifications to users outside the webpage's visible interface.

Notifications can appear even when the user is viewing another browser tab or application.

Common use cases include:

* Chat message alerts.
* Email notifications.
* Order status updates.
* Background task completion.
* Calendar reminders.
* System alerts.
* Real-time application updates.

Example:

```javascript
new Notification("Hello!", {
  body: "Welcome to our application."
});
```

Notifications are displayed through the operating system or browser notification interface.

# 🗁 Understanding the Notifications API

The Notifications API allows websites to request permission and display notifications.

A typical workflow:

```text
User Visits Website
        |
        v
Request Notification Permission
        |
        v
User Grants Permission
        |
        v
Create Notification
        |
        v
Browser Displays Notification
        |
        v
User Interacts with Notification
```

The browser controls whether notifications are permitted.

Websites cannot display notifications without the required permission.

# 🗁 Notification Support

The Notifications API is accessed through the global `Notification` interface.

Check whether the browser supports it:

```javascript
if ("Notification" in window) {
  console.log("Notifications are supported");
} else {
  console.log("Notifications are not supported");
}
```

Feature detection is important because browser capabilities can vary.

# 🗁 Notification Permission

Before displaying notifications, an application must obtain permission from the user.

The `Notification.permission` property indicates the current permission state.

Available values:

| Value | Description |
| --- | --- |
| `default` | Permission has not been granted or denied |
| `granted` | User has allowed notifications |
| `denied` | User has blocked notifications |

Example:

```javascript
console.log(Notification.permission);
```

# 🗁 Requesting Permission

The `Notification.requestPermission()` method requests permission from the user.

It returns a Promise that resolves to the permission result.

Example:

```javascript
Notification.requestPermission()
  .then((permission) => {
    console.log("Permission:", permission);
  });
```

Using `async` and `await`:

```javascript
async function requestNotificationPermission() {
  const permission = await Notification.requestPermission();

  console.log(permission);
}
```

The browser generally expects permission requests to be triggered by a meaningful user interaction, such as clicking a button.

# 🗁 Checking Permission Before Displaying

Applications should check notification permission before creating a notification.

Example:

```javascript
async function enableNotifications() {
  if (!("Notification" in window)) {
    console.log("Notifications are not supported");
    return;
  }

  if (Notification.permission === "granted") {
    console.log("Permission already granted");
    return;
  }

  if (Notification.permission === "denied") {
    console.log("Notifications are blocked");
    return;
  }

  const permission = await Notification.requestPermission();

  if (permission === "granted") {
    console.log("Permission granted");
  }
}
```

This prevents unnecessary permission requests and handles existing permission states.

# 🗁 Creating a Notification

Once permission is granted, a notification can be created using the `Notification` constructor.

Syntax:

```javascript
new Notification(title, options);
```

Parameters:

* `title`: The notification title.
* `options`: Optional notification configuration.

Example:

```javascript
const notification = new Notification("New Message", {
  body: "You have received a new message."
});
```

# 🗁 Notification Options

The Notification constructor supports several options.

| Option | Description |
| --- | --- |
| `body` | Main notification message |
| `icon` | Notification icon |
| `badge` | Small notification badge |
| `image` | Image displayed in the notification |
| `tag` | Identifier used to group or replace notifications |
| `data` | Custom data associated with the notification |
| `dir` | Text direction |
| `lang` | Notification language |
| `timestamp` | Notification timestamp |
| `requireInteraction` | Requests that the notification remain visible until user interaction |
| `silent` | Requests silent notification behavior |
| `renotify` | Requests another alert when replacing a notification with the same tag |
| `vibrate` | Vibration pattern where supported |

Example:

```javascript
const notification = new Notification("Order Update", {
  body: "Your order has been shipped.",
  icon: "/images/logo.png",
  badge: "/images/badge.png",
  tag: "order-101",
  data: {
    orderId: 101
  },
  requireInteraction: true
});
```

Actual display and support for individual options depend on the browser and operating system.

# 🗁 Notification Events

Notifications provide events that allow applications to respond to user interactions and notification lifecycle changes.

Important events include:

| Event | Description |
| --- | --- |
| `show` | Notification has been displayed |
| `click` | User clicks the notification |
| `close` | Notification is closed |
| `error` | An error occurs |

Example:

```javascript
const notification = new Notification("New Message", {
  body: "You have a new message."
});

notification.addEventListener("show", () => {
  console.log("Notification displayed");
});

notification.addEventListener("click", () => {
  console.log("Notification clicked");
});

notification.addEventListener("close", () => {
  console.log("Notification closed");
});

notification.addEventListener("error", (event) => {
  console.error("Notification error:", event);
});
```

# 🗁 Handling Notification Clicks

The `click` event is useful for responding when a user interacts with a notification.

Example:

```javascript
const notification = new Notification("New Message", {
  body: "Click to open your inbox."
});

notification.addEventListener("click", () => {
  window.focus();

  notification.close();
});
```

This example focuses the current browser window and closes the notification.

For navigation, the application can also open or focus the relevant page.

# 🗁 Closing a Notification

The `close()` method closes a notification.

Example:

```javascript
const notification = new Notification("Temporary Alert", {
  body: "This notification will close shortly."
});

setTimeout(() => {
  notification.close();
}, 5000);
```

The notification is closed after five seconds.

Browsers and operating systems may also close notifications automatically.

# 🗁 Notification Tags

The `tag` option identifies notifications that belong to the same logical group.

It can be used to replace an existing notification rather than displaying multiple notifications for the same event.

Example:

```javascript
new Notification("Download Progress", {
  body: "Download started.",
  tag: "download-status"
});

new Notification("Download Progress", {
  body: "Download is 50% complete.",
  tag: "download-status"
});
```

The second notification can replace the previous notification with the same tag.

This is useful for status updates where only the latest notification matters.

# 🗁 Notification Data

The `data` option stores custom information associated with a notification.

Example:

```javascript
const notification = new Notification("New Order", {
  body: "You received a new order.",
  data: {
    orderId: 101,
    customer: "John"
  }
});

notification.addEventListener("click", (event) => {
  console.log(notification.data.orderId);
  console.log(notification.data.customer);
});
```

The `data` property can contain structured-cloneable values.

It is useful for connecting notifications to application-specific records.

# 🗁 Complete Practical Example

This example demonstrates requesting permission and displaying a notification after a button click.

HTML:

```html
<button id="notifyButton">
  Enable Notifications
</button>
```

JavaScript:

```javascript
const button = document.querySelector("#notifyButton");

button.addEventListener("click", async () => {
  if (!("Notification" in window)) {
    alert("Notifications are not supported");

    return;
  }

  let permission = Notification.permission;

  if (permission === "default") {
    permission = await Notification.requestPermission();
  }

  if (permission !== "granted") {
    console.log("Notification permission not granted");

    return;
  }

  const notification = new Notification("Welcome!", {
    body: "Notifications are now enabled.",
    icon: "/images/logo.png",
    tag: "welcome"
  });

  notification.addEventListener("click", () => {
    window.focus();

    notification.close();
  });
});
```

This example checks browser support, requests permission when necessary, and creates a notification only after permission is granted.

# 🗁 Notification API vs Push API

The Notifications API and Push API are related but serve different purposes.

| Feature | Notifications API | Push API |
| --- | --- | --- |
| Primary purpose | Display notifications | Receive push messages |
| Requires permission | Yes | Yes |
| Works with service workers | Yes | Yes |
| Background message delivery | Not by itself | Yes, through service workers |
| Requires push subscription | No | Yes |
| Typical use | Displaying an alert | Receiving server-originated events |

The Notifications API displays notifications.

The Push API enables a web application to receive messages from a push service, even when the application is not currently open.

They are often used together.

# 🗁 Notifications with Service Workers

For background notifications, applications generally use Service Workers and the `showNotification()` method.

A service worker can display a notification using:

```javascript
self.registration.showNotification("New Message", {
  body: "You have received a new message.",
  icon: "/images/logo.png",
  data: {
    url: "/messages"
  }
});
```

This is different from:

```javascript
new Notification("New Message");
```

The `showNotification()` method belongs to the Service Worker registration and is designed for service-worker-controlled notifications.

It is especially important for Push API integrations.

# 🗁 Handling Notification Clicks in a Service Worker

A service worker can listen for notification clicks using the `notificationclick` event.

Example:

```javascript
self.addEventListener("notificationclick", (event) => {
  event.notification.close();

  const targetURL = event.notification.data?.url ?? "/";

  event.waitUntil(
    self.clients.openWindow(targetURL)
  );
});
```

The `waitUntil()` method keeps the service worker event alive while the asynchronous operation completes.

More advanced applications can focus an existing window instead of always opening a new one.

# 🗁 Browser and Platform Considerations

Notification behavior depends on the browser and operating system.

Important considerations:

* Notifications generally require a secure context, such as HTTPS.
* Localhost is generally treated as a trustworthy development origin.
* Users can change notification permissions through browser settings.
* Browser permission prompts should follow meaningful user interactions.
* Some browsers restrict notifications in certain browsing modes.
* Mobile browsers may have different notification capabilities.
* Display options vary across operating systems.
* Browser and operating system settings can suppress notifications.

Applications should not assume that a granted permission guarantees that every notification will be visibly presented.

# 🗁 Security and Privacy Considerations

Notifications can expose information outside the application interface.

For example, a notification may appear on a lock screen.

Best practices include:

* Request permission only when notifications provide clear user value.
* Explain why notifications are useful before requesting permission.
* Avoid repeatedly prompting users who have denied permission.
* Do not expose passwords, authentication tokens, or sensitive personal data.
* Keep notification messages concise.
* Validate notification data before using it.
* Provide a way to manage notification preferences.
* Avoid sending excessive or irrelevant notifications.

# 🗁 Common Mistakes

**Creating a notification without checking permission**

Incorrect:

```javascript
new Notification("Hello");
```

Better:

```javascript
if (Notification.permission === "granted") {
  new Notification("Hello");
}
```

**Requesting permission immediately on page load**

Permission requests should generally follow a meaningful user action.

**Assuming notification support**

Always check:

```javascript
if ("Notification" in window) {
  // Notification API is available
}
```

**Confusing page notifications with service worker notifications**

`new Notification()` and `registration.showNotification()` are different mechanisms with different execution contexts and capabilities.

**Assuming notification delivery is guaranteed**

Browser and operating system settings can affect notification visibility and behavior.

# 🗁 Best Practices

* Check browser support before using the API.
* Request permission after explaining its purpose to the user.
* Handle all three permission states.
* Use meaningful notification titles and messages.
* Use `tag` to replace related notifications.
* Store relevant application identifiers in `data`.
* Close notifications when appropriate.
* Use Service Workers for background notification workflows.
* Avoid displaying sensitive information.
* Respect user notification preferences.
* Keep notifications useful and non-intrusive.

# 🗁 Summary

The Notifications API allows web applications to display system-level notifications.

Important concepts include:

* `Notification` interface.
* `Notification.permission`.
* `Notification.requestPermission()`.
* Notification constructor and options.
* Notification events such as `click`, `show`, and `close`.
* `close()` and notification tags.
* Custom notification data.
* Differences between Notifications API and Push API.
* Service Worker `showNotification()`.
* Notification security and permission handling.

The Notifications API is useful for building interactive applications that keep users informed about important events.
