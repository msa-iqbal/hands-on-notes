# Jest

Jest is a JavaScript testing framework used to test application behavior, verify expected results, and detect software defects.

It provides built-in features for:

* Unit testing.
* Integration testing.
* Assertions.
* Mock functions.
* Module mocking.
* Setup and teardown.
* Snapshot testing.
* Code coverage.
* Asynchronous testing.

Jest can be used in TypeScript projects through TypeScript transformation tools such as `ts-jest` or Babel.

# 🗁 What Is Jest?

Jest is an open-source testing framework originally developed by Facebook (now Meta).

It is designed to provide a comprehensive testing environment with minimal configuration for many JavaScript projects.

Example:

```typescript
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

# 🗁 Why Use Jest?

Jest provides several useful capabilities:

* Built-in test runner and assertion library.
* Mocking functions and modules.
* Asynchronous test support.
* Code coverage reports.
* Snapshot testing.
* Test grouping.
* Setup and teardown hooks.
* Watch mode.
* Parallel test execution.

Jest is particularly useful for JavaScript applications and established TypeScript projects.

# 🗁 Installing Jest in a TypeScript Project

Create a project:

```bash
mkdir jest-typescript
cd jest-typescript
npm init -y
```

Install TypeScript and Jest:

```bash
npm install --save-dev typescript jest ts-jest @types/jest
```

Initialize TypeScript:

```bash
npx tsc --init
```

Initialize Jest configuration for TypeScript:

```bash
npx ts-jest config:init
```

This generates a Jest configuration file.

A typical project structure:

```text
jest-typescript/
├── src/
│   └── calculator.ts
├── tests/
│   └── calculator.test.ts
├── jest.config.js
├── package.json
└── tsconfig.json
```

# 🗁 Configuring Jest

A typical Jest configuration using `ts-jest`:

**jest.config.js**

```javascript
/** @type {import("jest").Config} */
module.exports = {
  preset: "ts-jest",
  testEnvironment: "node",
  testMatch: [
    "**/tests/**/*.test.ts"
  ],
  clearMocks: true
};
```

Configuration properties:

| Property | Description |
| --- | --- |
| `preset` | Loads a predefined Jest configuration |
| `testEnvironment` | Defines the environment in which tests execute |
| `testMatch` | Specifies patterns for discovering test files |
| `clearMocks` | Clears mock call history before each test |

For projects that use ECMAScript modules, Jest may require additional configuration depending on the TypeScript module settings and Node.js environment.

# 🗁 Configuring npm Scripts

Add the following scripts to `package.json`:

```json
{
  "scripts": {
    "test": "jest",
    "test:watch": "jest --watch",
    "test:coverage": "jest --coverage"
  }
}
```

Run all tests:

```bash
npm test
```

Run tests in watch mode:

```bash
npm run test:watch
```

Generate a coverage report:

```bash
npm run test:coverage
```

# 🗁 Writing Your First Jest Test

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

**tests/calculator.test.ts**

```typescript
import {
  add,
  subtract,
  multiply
} from "../src/calculator";

describe("Calculator", () => {
  test("adds two numbers", () => {
    expect(add(10, 5)).toBe(15);
  });

  test("subtracts two numbers", () => {
    expect(subtract(10, 5)).toBe(5);
  });

  test("multiplies two numbers", () => {
    expect(multiply(10, 5)).toBe(50);
  });
});
```

Run:

```bash
npm test
```

Jest discovers the test file and executes its test cases.

# 🗁 Test and It Functions

Jest provides `test()` and `it()` to define test cases.

Both functions have the same basic purpose.

Using `test()`:

```typescript
test("adds two numbers", () => {
  expect(2 + 3).toBe(5);
});
```

Using `it()`:

```typescript
it("adds two numbers", () => {
  expect(2 + 3).toBe(5);
});
```

Choose one naming convention and use it consistently throughout a project.

# 🗁 Grouping Tests with Describe

The `describe()` function groups related test cases.

Example:

```typescript
describe("String utilities", () => {
  test("converts text to uppercase", () => {
    expect("hello".toUpperCase()).toBe("HELLO");
  });

  test("converts text to lowercase", () => {
    expect("HELLO".toLowerCase()).toBe("hello");
  });
});
```

Nested groups can organize more complex test suites.

```typescript
describe("UserService", () => {
  describe("getUser", () => {
    test("returns an existing user", () => {
      expect(true).toBe(true);
    });
  });

  describe("deleteUser", () => {
    test("deletes an existing user", () => {
      expect(true).toBe(true);
    });
  });
});
```

# 🗁 Jest Assertions

Jest provides many built-in matchers for verifying expected behavior.

Example:

```typescript
test("demonstrates Jest assertions", () => {
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

Jest provides `toThrow()` to verify that a function throws an expected exception.

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
import { divide } from "../src/divide";

test("throws an error when dividing by zero", () => {
  expect(() => divide(10, 0)).toThrow(
    "Division by zero"
  );
});
```

The function must be wrapped in a callback so Jest can capture the thrown exception.

# 🗁 Testing Asynchronous Code

Jest supports testing promises and asynchronous functions.

Consider:

```typescript
export async function getUserName(): Promise<string> {
  return "Alice";
}
```

Test using `async` and `await`:

```typescript
import { getUserName } from "../src/user";

test("returns the user name", async () => {
  const result = await getUserName();

  expect(result).toBe("Alice");
});
```

Test using `.resolves`:

```typescript
test("resolves with the user name", async () => {
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
import { fetchUser } from "../src/fetchUser";

test("rejects when the user is unavailable", async () => {
  await expect(fetchUser()).rejects.toThrow(
    "User not found"
  );
});
```

Always return or await asynchronous assertions so Jest knows when the test has finished.

# 🗁 Setup and Teardown

Jest provides lifecycle hooks for preparing and cleaning up test environments.

| Hook | Purpose |
| --- | --- |
| `beforeAll()` | Runs once before tests in a scope |
| `afterAll()` | Runs once after tests in a scope |
| `beforeEach()` | Runs before every test |
| `afterEach()` | Runs after every test |

Example:

```typescript
let users: string[];

beforeEach(() => {
  users = [];
});

afterEach(() => {
  users.length = 0;
});

test("adds a user", () => {
  users.push("Alice");

  expect(users).toEqual(["Alice"]);
});

test("starts with an empty collection", () => {
  expect(users).toEqual([]);
});
```

Lifecycle hooks help maintain test isolation and predictable state.

# 🗁 Mock Functions

Mock functions replace or observe function behavior during testing.

Jest provides `jest.fn()` to create mock functions.

Example:

```typescript
test("tracks function calls", () => {
  const callback = jest.fn();

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
const getUser = jest.fn();

getUser.mockReturnValue({
  id: 1,
  name: "Alice"
});

test("returns a mocked user", () => {
  expect(getUser()).toEqual({
    id: 1,
    name: "Alice"
  });
});
```

For asynchronous dependencies, use:

```typescript
const fetchUser = jest.fn();

fetchUser.mockResolvedValue({
  id: 1,
  name: "Alice"
});
```

To simulate a rejected promise:

```typescript
const fetchUser = jest.fn();

fetchUser.mockRejectedValue(
  new Error("Network error")
);
```

# 🗁 Spying on Functions

Jest provides `jest.spyOn()` to observe calls to existing object methods.

Example:

```typescript
const logger = {
  log(message: string) {
    console.log(message);
  }
};

test("spies on the logger", () => {
  const spy = jest.spyOn(logger, "log");

  logger.log("Application started");

  expect(spy).toHaveBeenCalledWith(
    "Application started"
  );

  spy.mockRestore();
});
```

Unlike a standalone mock function, a spy can call the original implementation unless its behavior is replaced.

Restore spies after testing to avoid affecting other tests.

# 🗁 Mocking Modules

Module mocking replaces the behavior of an imported module.

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

A service:

**src/userService.ts**

```typescript
import { findUser } from "./userRepository";

export async function getUserName(id: number) {
  const user = await findUser(id);

  return user.name;
}
```

Test:

**tests/userService.test.ts**

```typescript
import { getUserName } from "../src/userService";
import { findUser } from "../src/userRepository";

jest.mock("../src/userRepository", () => ({
  findUser: jest.fn()
}));

const mockedFindUser = jest.mocked(findUser);

test("returns a user name", async () => {
  mockedFindUser.mockResolvedValue({
    id: 1,
    name: "Alice"
  });

  await expect(getUserName(1)).resolves.toBe("Alice");
});
```

Module mocking allows a test to isolate a service from its external dependencies.

# 🗁 Parameterized Tests

Jest provides `test.each()` to execute the same test logic with multiple input values.

Example:

```typescript
function add(a: number, b: number): number {
  return a + b;
}

test.each([
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

Parameterized tests reduce repeated code and improve test coverage across different inputs.

# 🗁 Snapshot Testing

Snapshot testing compares a value against a previously stored snapshot.

It is useful for detecting unintended changes in structured output.

Example:

```typescript
const user = {
  id: 1,
  name: "Alice",
  active: true
};

test("matches the user snapshot", () => {
  expect(user).toMatchSnapshot();
});
```

On the first execution, Jest creates a snapshot file.

Subsequent executions compare the current value with the stored snapshot.

If the output changes intentionally, update snapshots:

```bash
npx jest -u
```

Snapshots should be reviewed carefully. Updating a snapshot does not automatically mean the new output is correct.

# 🗁 Code Coverage

Jest can generate code coverage reports.

Run:

```bash
npx jest --coverage
```

Common coverage metrics:

| Metric | Description |
| --- | --- |
| Statements | Executed statements |
| Branches | Executed conditional branches |
| Functions | Executed functions |
| Lines | Executed source lines |

Example configuration:

```javascript
module.exports = {
  preset: "ts-jest",
  testEnvironment: "node",
  collectCoverage: true,
  collectCoverageFrom: [
    "src/**/*.ts",
    "!src/**/*.d.ts"
  ],
  coverageDirectory: "coverage"
};
```

Coverage reports help identify untested areas of an application.

High coverage does not necessarily mean that tests verify meaningful behavior.

# 🗁 Running Specific Tests

Jest provides several commands for running selected tests.

Run all tests:

```bash
npx jest
```

Run tests in watch mode:

```bash
npx jest --watch
```

Run a specific file:

```bash
npx jest tests/calculator.test.ts
```

Run tests matching a name:

```bash
npx jest -t "adds two numbers"
```

Run tests with coverage:

```bash
npx jest --coverage
```

Run tests serially:

```bash
npx jest --runInBand
```

`--runInBand` executes tests in the current process instead of using Jest's worker pool.

# 🗁 Jest vs Vitest

Jest and Vitest provide similar testing capabilities, but their ecosystems and integrations differ.

| Feature | Jest | Vitest |
| --- | --- | --- |
| Primary ecosystem | JavaScript | Vite and modern JavaScript |
| TypeScript support | Through transformers | Through Vite-compatible transformation |
| Assertions | Built in | Built in |
| Mocking | Built in | Built in |
| Snapshot testing | Yes | Yes |
| Coverage | Yes | Yes |
| Watch mode | Yes | Yes |
| Vite integration | Requires additional integration | Native |
| Configuration | Jest configuration | Vitest configuration |

Choose Jest when an existing project depends on its ecosystem or when its configuration and integrations suit the project.

Choose Vitest when working with Vite-based applications or when its modern development workflow is a better fit.

# 🗁 Best Practices

* Use descriptive test names.
* Keep test cases independent.
* Test normal, invalid, and boundary inputs.
* Use mocks to isolate external dependencies.
* Avoid excessive mocking of internal implementation details.
* Restore spies and mocks when necessary.
* Await asynchronous assertions.
* Keep snapshots small and meaningful.
* Use coverage reports to identify missing tests.
* Run tests automatically in CI/CD pipelines.
* Keep test configuration consistent across environments.

# 🗁 Summary

Jest is a comprehensive JavaScript testing framework that can also be used with TypeScript.

Important features include:

* Test cases and test suites.
* Assertions and matchers.
* Asynchronous testing.
* Setup and teardown hooks.
* Mock functions and module mocking.
* Spying on existing methods.
* Parameterized testing.
* Snapshot testing.
* Code coverage.
* Watch mode and selective test execution.

Jest helps developers build reliable applications by making expected behavior repeatable and automatically verifiable.
