# Vitest

Vitest is a modern JavaScript and TypeScript testing framework designed to integrate closely with Vite.

It provides a complete testing environment with features such as:

* Unit testing.
* Integration testing.
* Assertions.
* Mock functions.
* Module mocking.
* Asynchronous testing.
* Snapshot testing.
* Code coverage.
* Setup and teardown hooks.
* Watch mode.

Vitest is particularly useful for modern frontend applications and TypeScript projects.

# 🗁 What Is Vitest?

Vitest is a testing framework built around Vite's transformation and module-resolution infrastructure.

It provides a familiar testing API similar to Jest while offering native integration with Vite configurations.

Example:

```typescript
import { expect, test } from "vitest";

function add(a: number, b: number): number {
  return a + b;
}

test("adds two numbers", () => {
  expect(add(2, 3)).toBe(5);
});
```

In this example:

* `test()` defines a test case.
* `expect()` creates an assertion.
* `toBe()` verifies the expected result.

# 🗁 Why Use Vitest?

Vitest offers several advantages:

* Native Vite integration.
* Fast test execution.
* TypeScript and JSX transformation.
* Jest-compatible testing APIs.
* Built-in mocking capabilities.
* Watch mode.
* Code coverage.
* Snapshot testing.
* Parallel test execution.
* Support for browser and Node.js testing environments.

# 🗁 Installing Vitest

Install Vitest as a development dependency:

```bash
npm install --save-dev vitest
```

For a basic TypeScript project, install TypeScript as well:

```bash
npm install --save-dev typescript
```

For Vite projects, Vitest can reuse the existing Vite configuration.

A typical project structure:

```text
my-app/
├── src/
│   ├── calculator.ts
│   └── calculator.test.ts
├── package.json
├── tsconfig.json
└── vite.config.ts
```

# 🗁 Configuring Vitest

Vitest can use an existing Vite configuration file.

Example:

**vite.config.ts**

```typescript
import { defineConfig } from "vite";

export default defineConfig({
  test: {
    globals: false,
    environment: "node",
    include: ["src/**/*.test.ts"],
    clearMocks: true
  }
});
```

The `test` configuration requires Vitest's configuration extension.

Install Vitest and use `defineConfig` from `vitest/config`:

```typescript
import { defineConfig } from "vitest/config";

export default defineConfig({
  test: {
    globals: false,
    environment: "node",
    include: ["src/**/*.test.ts"],
    clearMocks: true
  }
});
```

Important configuration properties:

| Property | Description |
| --- | --- |
| `globals` | Makes testing APIs available globally |
| `environment` | Selects the test execution environment |
| `include` | Defines test file discovery patterns |
| `clearMocks` | Clears mock call history before each test |
| `setupFiles` | Specifies files executed before each test file |
| `coverage` | Configures code coverage reporting |

For projects that do not use Vite, Vitest can also be configured through `vitest.config.ts`.

# 🗁 Configuring TypeScript

Vitest provides type definitions for its testing APIs.

A typical `tsconfig.json` configuration:

```json
{
  "compilerOptions": {
    "target": "ES2022",
    "module": "ESNext",
    "moduleResolution": "Bundler",
    "strict": true,
    "noEmit": true,
    "types": ["vitest/globals"]
  },
  "include": [
    "src",
    "vite.config.ts"
  ]
}
```

When `globals` is disabled, import testing functions explicitly:

```typescript
import { describe, expect, it } from "vitest";
```

In that case, `vitest/globals` is not required unless the project uses global test APIs elsewhere.

# 🗁 Configuring npm Scripts

Add scripts to `package.json`:

```json
{
  "scripts": {
    "dev": "vite",
    "build": "vite build",
    "test": "vitest",
    "test:run": "vitest run",
    "test:coverage": "vitest run --coverage"
  }
}
```

Run tests in watch mode:

```bash
npm test
```

Run tests once:

```bash
npm run test:run
```

Generate a coverage report:

```bash
npm run test:coverage
```

# 🗁 Writing Your First Vitest Test

Create a function:

**src/calculator.ts**

```typescript
export function add(a: number, b: number): number {
  return a + b;
}

export function subtract(a: number, b: number): number {
  return a - b;
}

export function multiply(a: number, b: number): number {
  return a * b;
}
```

Create a test file:

**src/calculator.test.ts**

```typescript
import { describe, expect, it } from "vitest";

import {
  add,
  subtract,
  multiply
} from "./calculator";

describe("Calculator", () => {
  it("adds two numbers", () => {
    expect(add(10, 5)).toBe(15);
  });

  it("subtracts two numbers", () => {
    expect(subtract(10, 5)).toBe(5);
  });

  it("multiplies two numbers", () => {
    expect(multiply(10, 5)).toBe(50);
  });
});
```

Run:

```bash
npm run test:run
```

Vitest discovers the test file and executes its test cases.

# 🗁 Test and It Functions

Vitest provides `test()` and `it()` to define test cases.

Both functions have the same basic purpose.

Using `test()`:

```typescript
import { expect, test } from "vitest";

test("adds two numbers", () => {
  expect(2 + 3).toBe(5);
});
```

Using `it()`:

```typescript
import { expect, it } from "vitest";

it("adds two numbers", () => {
  expect(2 + 3).toBe(5);
});
```

Choose one naming convention and use it consistently.

# 🗁 Grouping Tests with Describe

The `describe()` function groups related test cases.

Example:

```typescript
import { describe, expect, it } from "vitest";

describe("String utilities", () => {
  it("converts text to uppercase", () => {
    expect("hello".toUpperCase()).toBe("HELLO");
  });

  it("converts text to lowercase", () => {
    expect("HELLO".toLowerCase()).toBe("hello");
  });
});
```

Nested groups can organize larger test suites:

```typescript
describe("UserService", () => {
  describe("getUser", () => {
    it("returns an existing user", () => {
      expect(true).toBe(true);
    });
  });

  describe("deleteUser", () => {
    it("deletes an existing user", () => {
      expect(true).toBe(true);
    });
  });
});
```

# 🗁 Vitest Assertions

Vitest provides built-in matchers for verifying expected results.

Example:

```typescript
import { expect, it } from "vitest";

it("demonstrates common assertions", () => {
  expect(10).toBe(10);

  expect({ id: 1 }).toEqual({ id: 1 });

  expect(true).toBeTruthy();

  expect(false).toBeFalsy();

  expect(null).toBeNull();

  expect(undefined).toBeUndefined();

  expect([1, 2, 3]).toContain(2);

  expect(10).toBeGreaterThan(5);

  expect(5).toBeLessThan(10);
});
```

Common matchers:

| Matcher | Purpose |
| --- | --- |
| `toBe()` | Checks strict equality |
| `toEqual()` | Checks deep equality |
| `toStrictEqual()` | Checks strict deep equality |
| `toBeTruthy()` | Checks truthiness |
| `toBeFalsy()` | Checks falsiness |
| `toBeNull()` | Checks for `null` |
| `toBeUndefined()` | Checks for `undefined` |
| `toContain()` | Checks collection membership |
| `toMatch()` | Checks string or regular expression matching |
| `toThrow()` | Checks whether a function throws |
| `toHaveLength()` | Checks collection or string length |
| `toHaveProperty()` | Checks object properties |

# 🗁 Testing Exceptions

Use `toThrow()` to verify that a function throws an expected error.

Example:

```typescript
export function divide(a: number, b: number): number {
  if (b === 0) {
    throw new Error("Division by zero");
  }

  return a / b;
}
```

Test:

```typescript
import { expect, it } from "vitest";
import { divide } from "./divide";

it("throws an error when dividing by zero", () => {
  expect(() => divide(10, 0)).toThrow(
    "Division by zero"
  );
});
```

Wrap the function call in a callback so Vitest can capture the exception.

# 🗁 Testing Asynchronous Code

Vitest supports promises and asynchronous functions.

Example:

```typescript
export async function getUserName(): Promise<string> {
  return "Alice";
}
```

Test using `async` and `await`:

```typescript
import { expect, it } from "vitest";
import { getUserName } from "./user";

it("returns the user name", async () => {
  const result = await getUserName();

  expect(result).toBe("Alice");
});
```

Test using `.resolves`:

```typescript
it("resolves with the user name", async () => {
  await expect(getUserName()).resolves.toBe("Alice");
});
```

Test rejected promises:

```typescript
export async function fetchUser(): Promise<never> {
  throw new Error("User not found");
}
```

```typescript
import { expect, it } from "vitest";
import { fetchUser } from "./fetchUser";

it("rejects when the user is unavailable", async () => {
  await expect(fetchUser()).rejects.toThrow(
    "User not found"
  );
});
```

Always await asynchronous assertions.

# 🗁 Setup and Teardown

Vitest provides lifecycle hooks for preparing and cleaning up test environments.

| Hook | Purpose |
| --- | --- |
| `beforeAll()` | Runs once before tests in a scope |
| `afterAll()` | Runs once after tests in a scope |
| `beforeEach()` | Runs before every test |
| `afterEach()` | Runs after every test |

Example:

```typescript
import {
  afterEach,
  beforeEach,
  describe,
  expect,
  it
} from "vitest";

let users: string[];

describe("User collection", () => {
  beforeEach(() => {
    users = [];
  });

  afterEach(() => {
    users.length = 0;
  });

  it("adds a user", () => {
    users.push("Alice");

    expect(users).toEqual(["Alice"]);
  });

  it("starts with an empty collection", () => {
    expect(users).toEqual([]);
  });
});
```

Lifecycle hooks help maintain test isolation and predictable state.

# 🗁 Mock Functions

Vitest provides `vi.fn()` to create mock functions.

Example:

```typescript
import { expect, it, vi } from "vitest";

it("tracks function calls", () => {
  const callback = vi.fn();

  callback("Hello");

  expect(callback).toHaveBeenCalled();

  expect(callback).toHaveBeenCalledTimes(1);

  expect(callback).toHaveBeenCalledWith("Hello");
});
```

Mock functions are useful when testing callbacks, event handlers, and dependencies.

# 🗁 Mock Return Values

Mock functions can return predefined values.

Example:

```typescript
import { vi } from "vitest";

const getUser = vi.fn();

getUser.mockReturnValue({
  id: 1,
  name: "Alice"
});
```

For asynchronous dependencies:

```typescript
const fetchUser = vi.fn();

fetchUser.mockResolvedValue({
  id: 1,
  name: "Alice"
});
```

To simulate a rejected promise:

```typescript
const fetchUser = vi.fn();

fetchUser.mockRejectedValue(
  new Error("Network error")
);
```

# 🗁 Spying on Functions

Vitest provides `vi.spyOn()` to observe calls to existing object methods.

Example:

```typescript
import { expect, it, vi } from "vitest";

const logger = {
  log(message: string) {
    console.log(message);
  }
};

it("spies on the logger", () => {
  const spy = vi.spyOn(logger, "log");

  logger.log("Application started");

  expect(spy).toHaveBeenCalledWith(
    "Application started"
  );

  spy.mockRestore();
});
```

A spy calls the original implementation by default unless its behavior is replaced.

Restore spies after testing to avoid affecting other tests.

# 🗁 Mocking Modules

Vitest provides `vi.mock()` to replace imported module behavior.

Consider:

**src/userRepository.ts**

```typescript
export async function findUser(id: number) {
  return {
    id,
    name: "Alice"
  };
}
```

Service:

**src/userService.ts**

```typescript
import { findUser } from "./userRepository";

export async function getUserName(id: number) {
  const user = await findUser(id);

  return user.name;
}
```

Test:

**src/userService.test.ts**

```typescript
import { beforeEach, expect, it, vi } from "vitest";
import { getUserName } from "./userService";
import { findUser } from "./userRepository";

vi.mock("./userRepository", () => ({
  findUser: vi.fn()
}));

const mockedFindUser = vi.mocked(findUser);

beforeEach(() => {
  vi.clearAllMocks();
});

it("returns a user name", async () => {
  mockedFindUser.mockResolvedValue({
    id: 1,
    name: "Alice"
  });

  await expect(getUserName(1)).resolves.toBe("Alice");
});
```

Module mocking isolates a service from external dependencies.

# 🗁 Parameterized Tests

Vitest provides `it.each()` and `test.each()` to execute the same test logic with multiple input values.

Example:

```typescript
import { expect, it } from "vitest";

function add(a: number, b: number): number {
  return a + b;
}

it.each([
  [2, 3, 5],
  [10, 5, 15],
  [-2, 2, 0],
  [0, 0, 0]
])(
  "adds %i and %i to produce %i",
  (a, b, expected) => {
    expect(add(a, b)).toBe(expected);
  }
);
```

Parameterized tests reduce duplication and improve test coverage across multiple inputs.

# 🗁 Snapshot Testing

Snapshot testing compares a value against a previously stored snapshot.

Example:

```typescript
import { expect, it } from "vitest";

const user = {
  id: 1,
  name: "Alice",
  active: true
};

it("matches the user snapshot", () => {
  expect(user).toMatchSnapshot();
});
```

On the first execution, Vitest creates a snapshot file.

Subsequent executions compare the current output with the stored snapshot.

Update snapshots when changes are intentional:

```bash
npx vitest -u
```

Snapshots should be reviewed carefully. Updating a snapshot does not automatically mean the new output is correct.

# 🗁 Code Coverage

Vitest supports code coverage through coverage providers.

Install the V8 coverage provider:

```bash
npm install --save-dev @vitest/coverage-v8
```

Run coverage:

```bash
npx vitest run --coverage
```

Example configuration:

**vite.config.ts**

```typescript
import { defineConfig } from "vitest/config";

export default defineConfig({
  test: {
    coverage: {
      provider: "v8",
      reporter: ["text", "html"],
      include: ["src/**/*.ts"],
      exclude: ["src/**/*.test.ts"]
    }
  }
});
```

Common coverage metrics:

| Metric | Description |
| --- | --- |
| Statements | Executed statements |
| Branches | Executed conditional branches |
| Functions | Executed functions |
| Lines | Executed source lines |

Coverage reports help identify areas that need additional testing.

High coverage does not automatically guarantee meaningful tests.

# 🗁 Testing Different Environments

Vitest supports multiple testing environments.

**Node.js**

Suitable for backend logic, utilities, and server-side modules.

```typescript
test: {
  environment: "node"
}
```

**jsdom**

Provides a simulated browser-like DOM environment.

Install:

```bash
npm install --save-dev jsdom
```

Configure:

```typescript
test: {
  environment: "jsdom"
}
```

Useful for testing DOM-dependent code and frontend components.

**happy-dom**

Another lightweight browser-like environment.

Install:

```bash
npm install --save-dev happy-dom
```

Configure:

```typescript
test: {
  environment: "happy-dom"
}
```

Choose the environment based on the APIs your application needs during testing.

# 🗁 Running Specific Tests

Run all tests:

```bash
npx vitest
```

Run tests once:

```bash
npx vitest run
```

Run a specific file:

```bash
npx vitest run src/calculator.test.ts
```

Run tests matching a name:

```bash
npx vitest run -t "adds two numbers"
```

Run tests in watch mode:

```bash
npx vitest
```

Run tests with coverage:

```bash
npx vitest run --coverage
```

# 🗁 Vitest vs Jest

Both frameworks provide powerful testing capabilities.

| Feature | Vitest | Jest |
| --- | --- | --- |
| Primary ecosystem | Vite and modern JavaScript | JavaScript |
| TypeScript transformation | Built-in through Vite infrastructure | Requires a transformer or compatible setup |
| Assertions | Built in | Built in |
| Mocking | Built in | Built in |
| Snapshot testing | Yes | Yes |
| Coverage | Yes | Yes |
| Watch mode | Yes | Yes |
| Vite integration | Native | Requires additional integration |
| Configuration | Vite-compatible | Jest-specific |

Vitest is a natural choice for Vite-based applications.

Jest remains a strong option for projects already using its ecosystem and integrations.

# 🗁 Best Practices

* Reuse the existing Vite configuration when appropriate.
* Keep test files close to the source files or organize them consistently.
* Use descriptive test names.
* Test normal, invalid, and boundary inputs.
* Keep tests independent and deterministic.
* Use mocks only when isolation is necessary.
* Restore spies and mock state appropriately.
* Await asynchronous assertions.
* Review snapshot changes before accepting them.
* Use coverage reports to identify untested areas.
* Run tests automatically in CI/CD pipelines.

# 🗁 Summary

Vitest is a modern testing framework that integrates closely with Vite and supports JavaScript and TypeScript projects.

Important features include:

* Test cases and test suites.
* Assertions and matchers.
* Asynchronous testing.
* Setup and teardown hooks.
* Mock functions and module mocking.
* Spying on existing methods.
* Parameterized tests.
* Snapshot testing.
* Code coverage.
* Multiple testing environments.
* Watch mode and selective test execution.

Vitest provides a productive testing workflow for modern TypeScript and frontend applications.
