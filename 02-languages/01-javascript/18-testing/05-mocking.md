# JavaScript Mocking

Mocking is a testing technique used to replace real dependencies with controlled substitutes.

It allows developers to isolate the code being tested without relying on databases, APIs, file systems, payment providers, or other external systems.

For example, consider a function that retrieves user information from an API:

```javascript
async function getUser(id) {
  const response = await fetch(
    `https://api.example.com/users/${id}`
  );

  return response.json();
}
```

Testing this function against a real API introduces several problems:

* The API might be unavailable.
* Network requests can make tests slower.
* API responses may change.
* Tests may depend on external credentials.
* Repeated requests may affect production data.

Instead, a test can replace `fetch()` with a controlled mock implementation.

```javascript
const mockFetch = async () => ({
  json: async () => ({
    id: 1,
    name: "John",
  }),
});
```

Mocking makes tests more predictable, isolated, and repeatable.

# 🗁 Why Mocking Is Important

Mocking is useful when testing code that depends on external systems or complex collaborators.

Common use cases include:

* Mocking HTTP requests.
* Mocking database operations.
* Mocking file system operations.
* Mocking authentication services.
* Mocking payment providers.
* Mocking date and time.
* Mocking browser APIs.
* Simulating errors and network failures.

Mocking also helps verify how a component interacts with its dependencies.

# 🗁 Mock, Stub, Spy, and Fake

These terms describe related but different testing techniques.

| Technique | Purpose |
| --- | --- |
| Mock | Replaces a dependency and can verify expected interactions |
| Stub | Provides predefined responses |
| Spy | Observes function calls and their arguments |
| Fake | Provides a simplified working implementation |

These terms are sometimes used interchangeably in testing libraries, but understanding their conceptual differences improves test design.

**Mock**

A mock can replace a real function and allow verification that it was called correctly.

```javascript
const sendEmail = vi.fn();

sendEmail("john@example.com");

expect(sendEmail).toHaveBeenCalledWith(
  "john@example.com"
);
```

**Stub**

A stub returns a predetermined value.

```javascript
const getUser = vi.fn().mockReturnValue({
  id: 1,
  name: "John",
});

console.log(getUser());
```

**Spy**

A spy observes an existing function.

```javascript
const calculator = {
  add(a, b) {
    return a + b;
  },
};

const spy = vi.spyOn(calculator, "add");

calculator.add(2, 3);

expect(spy).toHaveBeenCalledWith(2, 3);
```

**Fake**

A fake provides a simplified implementation of a dependency.

```javascript
class FakeUserRepository {
  constructor() {
    this.users = [];
  }

  async save(user) {
    this.users.push(user);
    return user;
  }

  async findById(id) {
    return this.users.find(
      (user) => user.id === id
    );
  }
}
```

This fake repository can replace a real database repository during tests.

# 🗁 Mocking with Vitest

Vitest provides several mocking utilities.

Import the required functions:

```javascript
import {
  vi,
  describe,
  test,
  expect,
} from "vitest";
```

Common utilities include:

| Utility | Purpose |
| --- | --- |
| `vi.fn()` | Creates a mock function |
| `vi.spyOn()` | Creates a spy on an existing method |
| `vi.mock()` | Mocks an entire module |
| `vi.mocked()` | Provides typed access to a mocked function |
| `vi.clearAllMocks()` | Clears mock call history |
| `vi.resetAllMocks()` | Clears history and resets mock implementations |
| `vi.restoreAllMocks()` | Restores original implementations of spies |

# 🗁 Creating Mock Functions with vi.fn()

`vi.fn()` creates a mock function that records information about its calls.

```javascript
import {
  test,
  expect,
  vi,
} from "vitest";

test("tracks function calls", () => {
  const greet = vi.fn();

  greet("John");

  expect(greet).toHaveBeenCalled();

  expect(greet).toHaveBeenCalledTimes(1);

  expect(greet).toHaveBeenCalledWith("John");
});
```

A mock function can also return values.

```javascript
test("returns a predefined value", () => {
  const getUser = vi.fn();

  getUser.mockReturnValue({
    id: 1,
    name: "John",
  });

  expect(getUser()).toEqual({
    id: 1,
    name: "John",
  });
});
```

# 🗁 Mocking Return Values

Vitest provides different methods for controlling mock return values.

**mockReturnValue()**

Returns a predefined value.

```javascript
const getStatus = vi.fn();

getStatus.mockReturnValue("success");

expect(getStatus()).toBe("success");
```

**mockReturnValueOnce()**

Returns a value for one specific invocation.

```javascript
const getStatus = vi.fn();

getStatus
  .mockReturnValueOnce("pending")
  .mockReturnValueOnce("success");

expect(getStatus()).toBe("pending");
expect(getStatus()).toBe("success");
```

**mockResolvedValue()**

Returns a resolved Promise.

```javascript
const fetchUser = vi.fn();

fetchUser.mockResolvedValue({
  id: 1,
  name: "John",
});

const user = await fetchUser();

expect(user.name).toBe("John");
```

**mockRejectedValue()**

Returns a rejected Promise.

```javascript
const fetchUser = vi.fn();

fetchUser.mockRejectedValue(
  new Error("Network error")
);

await expect(fetchUser()).rejects.toThrow(
  "Network error"
);
```

# 🗁 Mocking Function Implementations

Use `mockImplementation()` when a mock needs custom behavior.

```javascript
const calculate = vi.fn();

calculate.mockImplementation((a, b) => {
  return a * b;
});

expect(calculate(4, 5)).toBe(20);
```

An implementation can also be provided directly to `vi.fn()`.

```javascript
const add = vi.fn((a, b) => a + b);

expect(add(2, 3)).toBe(5);
```

For one specific invocation, use `mockImplementationOnce()`.

```javascript
const getValue = vi.fn();

getValue
  .mockImplementationOnce(() => "first")
  .mockImplementationOnce(() => "second");

expect(getValue()).toBe("first");
expect(getValue()).toBe("second");
```

# 🗁 Spying on Existing Functions

`vi.spyOn()` observes an existing object's method.

```javascript
import {
  test,
  expect,
  vi,
} from "vitest";

test("spies on an object method", () => {
  const logger = {
    log(message) {
      console.log(message);
    },
  };

  const spy = vi.spyOn(logger, "log");

  logger.log("Application started");

  expect(spy).toHaveBeenCalledTimes(1);

  expect(spy).toHaveBeenCalledWith(
    "Application started"
  );

  spy.mockRestore();
});
```

By default, a spy calls the original implementation.

It can also replace the implementation:

```javascript
const spy = vi
  .spyOn(logger, "log")
  .mockImplementation(() => {});
```

This allows a test to observe calls without producing actual console output.

# 🗁 Mocking Entire Modules

Sometimes a module contains dependencies that should be replaced completely.

Consider a user service that depends on an API module.

```javascript
// userApi.js

export async function fetchUser(id) {
  const response = await fetch(
    `/api/users/${id}`
  );

  return response.json();
}
```

The service:

```javascript
// userService.js

import { fetchUser } from "./userApi.js";

export async function getUserName(id) {
  const user = await fetchUser(id);

  return user.name;
}
```

Test the service by mocking its dependency:

```javascript
// userService.test.js

import {
  test,
  expect,
  vi,
} from "vitest";

import { fetchUser } from "./userApi.js";

import { getUserName } from "./userService.js";

vi.mock("./userApi.js", () => ({
  fetchUser: vi.fn(),
}));

test("returns the user's name", async () => {
  vi.mocked(fetchUser).mockResolvedValue({
    id: 1,
    name: "John",
  });

  const result = await getUserName(1);

  expect(result).toBe("John");

  expect(fetchUser).toHaveBeenCalledWith(1);
});
```

Module mocks are commonly registered through `vi.mock()` and are hoisted by Vitest.

Keep module paths consistent with the imports used by the application.

# 🗁 Mocking HTTP Requests

HTTP requests are among the most common dependencies to mock.

One simple approach is to replace `globalThis.fetch`.

```javascript
import {
  test,
  expect,
  vi,
  afterEach,
} from "vitest";

import { getUser } from "./userApi.js";

afterEach(() => {
  vi.unstubAllGlobals();
});

test("fetches a user successfully", async () => {
  vi.stubGlobal(
    "fetch",
    vi.fn().mockResolvedValue({
      ok: true,
      json: async () => ({
        id: 1,
        name: "John",
      }),
    })
  );

  const user = await getUser(1);

  expect(user).toEqual({
    id: 1,
    name: "John",
  });

  expect(fetch).toHaveBeenCalledWith(
    "/api/users/1"
  );
});
```

This technique replaces the global `fetch` function for the test.

For applications with many HTTP interactions, tools such as Mock Service Worker (MSW) can intercept requests at the network boundary and provide more realistic responses.

Documentation: <https://mswjs.io/>

# 🗁 Mocking Timers

Some application functions depend on timers such as:

* `setTimeout()`
* `setInterval()`
* `clearTimeout()`
* `clearInterval()`

Real timers can make tests slow and unreliable.

Vitest provides fake timers to control time.

```javascript
import {
  test,
  expect,
  vi,
} from "vitest";

test("executes a callback after a delay", () => {
  vi.useFakeTimers();

  const callback = vi.fn();

  setTimeout(callback, 1000);

  expect(callback).not.toHaveBeenCalled();

  vi.advanceTimersByTime(1000);

  expect(callback).toHaveBeenCalledTimes(1);

  vi.useRealTimers();
});
```

For asynchronous timer callbacks, use the corresponding asynchronous timer advancement utilities.

Always restore real timers after a test, preferably through a cleanup hook.

# 🗁 Mocking Dates and System Time

Applications sometimes depend on the current date and time.

Examples include:

* Token expiration.
* Subscription renewal.
* Scheduled tasks.
* Time-based access control.
* Date calculations.

Vitest can control system time.

```javascript
import {
  test,
  expect,
  vi,
} from "vitest";

test("uses a controlled system date", () => {
  vi.useFakeTimers();

  vi.setSystemTime(
    new Date("2026-01-01T00:00:00Z")
  );

  expect(Date.now()).toBe(
    new Date("2026-01-01T00:00:00Z").getTime()
  );

  vi.useRealTimers();
});
```

Controlling time makes time-dependent tests deterministic.

# 🗁 Clearing, Resetting, and Restoring Mocks

Mock cleanup is important because mock state can leak between tests.

Vitest provides three important cleanup methods.

| Method | Clears Call History | Resets Implementation | Restores Original |
| --- | --- | --- | --- |
| `vi.clearAllMocks()` | Yes | No | No |
| `vi.resetAllMocks()` | Yes | Yes | No |
| `vi.restoreAllMocks()` | Yes | No | Yes, for restorable spies |

Example:

```javascript
import {
  beforeEach,
  afterEach,
  vi,
} from "vitest";

beforeEach(() => {
  vi.clearAllMocks();
});

afterEach(() => {
  vi.restoreAllMocks();
});
```

Important distinctions:

* Clearing a mock removes recorded calls but preserves its implementation.
* Resetting a mock also resets its configured implementation.
* Restoring a spy returns the original method implementation.

`vi.restoreAllMocks()` does not automatically undo every module mock or global stub. Use the appropriate cleanup utilities for those cases, such as `vi.unmock()`, `vi.unstubAllGlobals()`, or `vi.useRealTimers()`.

# 🗁 Mocking with Jest

Jest provides similar mocking utilities.

Create a mock function:

```javascript
const getUser = jest.fn();

getUser.mockReturnValue({
  id: 1,
  name: "John",
});
```

Mock an asynchronous function:

```javascript
const fetchUser = jest.fn();

fetchUser.mockResolvedValue({
  id: 1,
  name: "John",
});
```

Spy on an existing method:

```javascript
const spy = jest.spyOn(
  console,
  "log"
);

console.log("Hello");

expect(spy).toHaveBeenCalledWith("Hello");

spy.mockRestore();
```

Mock a module:

```javascript
jest.mock("./userApi.js", () => ({
  fetchUser: jest.fn(),
}));
```

The Jest and Vitest APIs are similar, but their module mocking, configuration, and runtime behavior are not identical.

# 🗁 Mocking Best Practices

* Mock external dependencies rather than every internal function.
* Keep mocks simple and predictable.
* Verify meaningful interactions instead of implementation details.
* Avoid sharing mutable mock state between tests.
* Reset or restore mocks when necessary.
* Test both successful and failed dependency responses.
* Use realistic mock data that matches actual application contracts.
* Prefer dependency injection when it simplifies testing.
* Avoid excessive mocking that makes tests disconnected from real behavior.
* Use integration tests to verify important interactions with real dependencies.

# 🗁 Common Mocking Mistakes

| Mistake | Better Approach |
| --- | --- |
| Mocking every dependency | Keep the relevant integration boundary real |
| Forgetting mock cleanup | Clear, reset, or restore mocks appropriately |
| Using unrealistic mock responses | Match real API and service contracts |
| Testing only mock behavior | Verify the actual application behavior |
| Sharing mutable mock data | Create fresh data for each test |
| Mocking implementation details | Focus on observable behavior |
| Mocking the component under test | Mock only its external dependencies |

# 🗁 Summary

Mocking allows developers to replace dependencies with controlled implementations during testing.

Key concepts:

* Mocks replace dependencies and can verify interactions.
* Stubs provide predefined responses.
* Spies observe existing function calls.
* Fakes provide simplified working implementations.
* Vitest and Jest offer built-in mocking utilities.
* HTTP requests, timers, dates, and modules can be mocked.
* Proper cleanup prevents state leakage between tests.
* Excessive mocking can reduce test confidence.
