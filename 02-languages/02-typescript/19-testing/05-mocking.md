# Mocking in TypeScript

Mocking is a testing technique used to replace real dependencies with controlled substitutes during a test.

A mock allows you to simulate the behavior of external services, APIs, databases, modules, or functions without executing their real implementations.

Mocking is useful when testing code that depends on:

* HTTP requests and external APIs.
* Database queries.
* File system operations.
* Third-party services.
* Time-dependent functions.
* Random values.
* Complex or expensive dependencies.

The goal is to isolate the unit being tested and make tests predictable.

# 🗁 Why Mock Dependencies?

Consider a function that retrieves a user from an API:

```typescript
async function getUser(id: number) {
  const response = await fetch(
    `https://api.example.com/users/${id}`
  );

  return response.json();
}
```

Testing this function directly may require:

* An active internet connection.
* A working API server.
* Valid API credentials.
* A predictable API response.

A test that depends on these conditions can become unreliable.

Mocking allows us to simulate the API response without making an actual network request.

# 🗁 Types of Test Doubles

Test doubles are objects or functions that replace real dependencies during testing.

Common categories include:

| Type | Purpose |
| --- | --- |
| Dummy | Passed into code but not actually used |
| Stub | Returns predefined responses |
| Mock | Records interactions and allows assertions about them |
| Spy | Observes calls to an existing function |
| Fake | Provides a simplified working implementation |

These terms describe different testing roles. Some testing frameworks combine their functionality.

# 🗁 Mocking with Vitest

Vitest provides several utilities for creating and managing mocks.

Common utilities include:

* `vi.fn()` — creates a mock function.
* `vi.spyOn()` — observes or replaces an existing method.
* `vi.mock()` — mocks an entire module.
* `vi.mocked()` — provides TypeScript-aware typing for mocked functions.
* `vi.restoreAllMocks()` — restores original implementations of spies.

Import the required utilities:

```typescript
import { describe, it, expect, vi, afterEach } from "vitest";
```

# 🗁 Creating a Mock Function

Use `vi.fn()` to create a mock function.

```typescript
import { describe, it, expect, vi } from "vitest";

describe("mock functions", () => {
  it("creates a mock function", () => {
    const greet = vi.fn();

    greet("Alice");

    expect(greet).toHaveBeenCalled();
    expect(greet).toHaveBeenCalledWith("Alice");
  });
});
```

A mock function records information about its calls.

You can inspect:

* Whether it was called.
* How many times it was called.
* Which arguments were passed.
* What values it returned.

# 🗁 Mock Return Values

Use `mockReturnValue()` to configure a mock function to return a specific value.

```typescript
const getUsername = vi.fn();

getUsername.mockReturnValue("Alice");

expect(getUsername()).toBe("Alice");
```

For asynchronous functions, use `mockResolvedValue()`:

```typescript
const fetchUser = vi.fn();

fetchUser.mockResolvedValue({
  id: 1,
  name: "Alice",
});

const user = await fetchUser();

expect(user.name).toBe("Alice");
```

Use `mockRejectedValue()` to simulate a rejected promise:

```typescript
const fetchUser = vi.fn();

fetchUser.mockRejectedValue(
  new Error("Network error")
);

await expect(fetchUser()).rejects.toThrow(
  "Network error"
);
```

# 🗁 Mock Implementations

Use `mockImplementation()` when a mock needs custom behavior.

```typescript
const calculate = vi.fn();

calculate.mockImplementation(
  (a: number, b: number) => a + b
);

expect(calculate(10, 20)).toBe(30);
```

This is useful when a test needs more than a fixed return value.

# 🗁 Mock Function Call Information

Vitest stores information about mock function calls.

```typescript
const add = vi.fn(
  (a: number, b: number) => a + b
);

add(10, 20);
add(5, 15);

expect(add).toHaveBeenCalledTimes(2);

expect(add).toHaveBeenNthCalledWith(
  1,
  10,
  20
);

expect(add).toHaveBeenLastCalledWith(
  5,
  15
);

expect(add.mock.results[0].value).toBe(30);
```

Important mock properties include:

| Property | Description |
| --- | --- |
| `mock.calls` | Arguments passed to each call |
| `mock.results` | Results of function calls |
| `mock.instances` | Instances created through constructor calls |
| `mock.contexts` | `this` values used during calls |
| `mock.lastCall` | Arguments from the latest call |

# 🗁 Spying on Existing Methods

Use `vi.spyOn()` to observe calls to an existing object method.

```typescript
const logger = {
  info(message: string) {
    console.log(message);
  },
};

const spy = vi.spyOn(logger, "info");

logger.info("Application started");

expect(spy).toHaveBeenCalledWith(
  "Application started"
);
```

Unlike a standalone mock, a spy calls the original implementation by default.

You can replace its implementation when necessary:

```typescript
const spy = vi
  .spyOn(logger, "info")
  .mockImplementation(() => {});
```

This prevents the original method from executing.

# 🗁 Mocking Modules

Use `vi.mock()` to replace an entire module.

Consider this project structure:

```text
src/
├── services/
│   └── user-service.ts
├── repositories/
│   └── user-repository.ts
└── tests/
    └── user-service.test.ts
```

Repository implementation:

```typescript
// user-repository.ts

export async function findUser(id: number) {
  // Real database operation
  return {
    id,
    name: "Alice",
  };
}
```

Service implementation:

```typescript
// user-service.ts

import { findUser } from "./user-repository";

export async function getUserName(id: number) {
  const user = await findUser(id);

  return user.name;
}
```

Test the service without accessing the real database:

```typescript
// user-service.test.ts

import { describe, it, expect, vi } from "vitest";

import { getUserName } from "../services/user-service";
import { findUser } from "../repositories/user-repository";

vi.mock("../repositories/user-repository", () => ({
  findUser: vi.fn(),
}));

describe("getUserName", () => {
  it("returns the user's name", async () => {
    vi.mocked(findUser).mockResolvedValue({
      id: 1,
      name: "Alice",
    });

    const result = await getUserName(1);

    expect(result).toBe("Alice");
    expect(findUser).toHaveBeenCalledWith(1);
  });
});
```

The repository module is replaced with a mock implementation.

`vi.mocked()` does not create a mock itself. It provides TypeScript-aware typing for a function that has already been mocked.

# 🗁 Mocking HTTP Requests

Mocking HTTP requests allows you to test API-dependent code without contacting an external server.

Consider this implementation:

```typescript
// user-api.ts

export async function fetchUser(id: number) {
  const response = await fetch(
    `https://api.example.com/users/${id}`
  );

  if (!response.ok) {
    throw new Error("Failed to fetch user");
  }

  return response.json();
}
```

Test it by mocking `fetch`:

```typescript
import { describe, it, expect, vi, afterEach } from "vitest";
import { fetchUser } from "./user-api";

afterEach(() => {
  vi.unstubAllGlobals();
});

describe("fetchUser", () => {
  it("returns user data", async () => {
    const mockFetch = vi.fn().mockResolvedValue({
      ok: true,
      json: async () => ({
        id: 1,
        name: "Alice",
      }),
    });

    vi.stubGlobal("fetch", mockFetch);

    const user = await fetchUser(1);

    expect(user).toEqual({
      id: 1,
      name: "Alice",
    });

    expect(mockFetch).toHaveBeenCalledWith(
      "https://api.example.com/users/1"
    );
  });
});
```

`vi.stubGlobal()` temporarily replaces a global variable or function.

`vi.unstubAllGlobals()` restores the original global values.

# 🗁 Mocking Time

Time-dependent logic can be difficult to test because the actual clock changes.

Vitest provides fake timers to control timer-related behavior.

```typescript
import { describe, it, expect, vi, afterEach } from "vitest";

function delayedMessage(callback: () => void) {
  setTimeout(callback, 2000);
}

describe("delayedMessage", () => {
  afterEach(() => {
    vi.useRealTimers();
  });

  it("executes after two seconds", () => {
    vi.useFakeTimers();

    const callback = vi.fn();

    delayedMessage(callback);

    expect(callback).not.toHaveBeenCalled();

    vi.advanceTimersByTime(2000);

    expect(callback).toHaveBeenCalledOnce();
  });
});
```

Important timer utilities:

| Method | Description |
| --- | --- |
| `vi.useFakeTimers()` | Enables fake timers |
| `vi.useRealTimers()` | Restores real timers |
| `vi.advanceTimersByTime()` | Advances timers by a specified duration |
| `vi.runAllTimers()` | Executes all pending timers |
| `vi.runOnlyPendingTimers()` | Executes currently pending timers |
| `vi.setSystemTime()` | Sets the system time used by fake timers |

Fake timers are useful for testing:

* Debouncing.
* Throttling.
* Delayed callbacks.
* Intervals.
* Scheduled tasks.
* Time-dependent application behavior.

# 🗁 Clearing, Resetting, and Restoring Mocks

Vitest provides several methods for managing mock state.

| Method | Behavior |
| --- | --- |
| `mockClear()` | Clears call history but preserves the implementation |
| `mockReset()` | Clears call history and resets the mock implementation |
| `mockRestore()` | Restores the original implementation of a spy |
| `vi.clearAllMocks()` | Clears call history for all mocks |
| `vi.resetAllMocks()` | Resets all mocks |
| `vi.restoreAllMocks()` | Restores original implementations of spies |

Example:

```typescript
import { describe, it, expect, vi } from "vitest";

describe("mock management", () => {
  it("clears mock history", () => {
    const greet = vi.fn().mockReturnValue("Hello");

    greet();

    expect(greet).toHaveBeenCalledOnce();

    greet.mockClear();

    expect(greet).not.toHaveBeenCalled();

    expect(greet()).toBe("Hello");
  });
});
```

A key distinction:

* Clearing removes recorded calls.
* Resetting also removes configured mock behavior.
* Restoring puts a spied method back to its original implementation.

A standalone `vi.fn()` has no original implementation to restore.

# 🗁 Mocking Best Practices

Follow these practices when using mocks:

* Mock external dependencies rather than every internal function.
* Keep mock behavior predictable and minimal.
* Verify meaningful interactions instead of implementation details.
* Restore spies and global stubs after tests.
* Avoid sharing mutable mock state between unrelated tests.
* Use realistic mock data that matches actual application types.
* Test error responses as well as successful responses.
* Avoid excessive mocking that makes tests unrelated to real application behavior.

# 🗁 Common Mistakes

**Forgetting to restore spies**

A spy can affect later tests if its original implementation is not restored.

```typescript
afterEach(() => {
  vi.restoreAllMocks();
});
```

**Confusing clearing with resetting**

`mockClear()` preserves the implementation, while `mockReset()` resets it.

**Mocking too much**

Mocking every dependency can produce tests that pass even when the real application integration is broken.

**Using incorrect mock return types**

Mocked values should match the actual return type of the dependency.

For example, an asynchronous function returning `Promise<User>` should be configured with a compatible resolved value.

**Assuming a spy prevents execution**

`vi.spyOn()` calls the original method by default. Use `mockImplementation()` when you need to prevent that behavior.

# 🗁 Key Takeaways

* Mocking replaces dependencies with controlled test implementations.
* Test doubles include dummies, stubs, mocks, spies, and fakes.
* `vi.fn()` creates mock functions.
* `vi.spyOn()` observes or replaces existing methods.
* `vi.mock()` replaces module dependencies.
* `vi.mocked()` improves TypeScript typing for mocked functions.
* `vi.stubGlobal()` replaces global variables and functions.
* Fake timers make time-dependent tests predictable.
* Clearing, resetting, and restoring mocks are different operations.
* Effective mocking isolates units without hiding important integration problems.
