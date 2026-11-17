# Unit Testing

Unit testing is a software testing technique that verifies the behavior of individual units of code in isolation.

A unit is typically a small, independently testable part of an application, such as:

* A function.
* A method.
* A class.
* A small module.

The primary goal of unit testing is to verify that each unit produces the expected result for different inputs and conditions.

Consider a simple function:

```javascript
export function calculateTotal(price, quantity) {
  return price * quantity;
}
```

A unit test verifies its behavior:

```javascript
import test from "node:test";
import assert from "node:assert/strict";

import { calculateTotal } from "./calculateTotal.js";

test("calculates the total price", () => {
  assert.equal(calculateTotal(100, 3), 300);
});
```

The test executes the function and compares its actual result with the expected result.

# 🗁 Characteristics of Unit Tests

Well-designed unit tests generally have these characteristics:

* **Isolation:** Test one unit without depending on unrelated components.
* **Determinism:** Produce consistent results when executed with the same conditions.
* **Repeatability:** Can be executed multiple times without unexpected side effects.
* **Fast execution:** Avoid unnecessary network, database, and filesystem operations.
* **Focused assertions:** Verify specific expected behavior.
* **Independence:** Do not depend on the execution order of other tests.
* **Maintainability:** Remain understandable as the application evolves.

# 🗁 Unit Testing vs Manual Testing

| Unit Testing | Manual Testing |
| --- | --- |
| Usually automated | Performed by a person |
| Tests individual units | Can test complete workflows |
| Executes quickly | Often takes more time |
| Repeatable | May vary between executions |
| Uses assertions | Uses human observation and verification |
| Commonly integrated into CI | Often used for exploratory and usability testing |

Both approaches are useful and complement each other.

# 🗁 Writing Your First Unit Test

Node.js provides a built-in test runner through the `node:test` module.

Create the following project structure:

```text
unit-testing/
├── package.json
├── calculator.js
└── calculator.test.js
```

Configure `package.json`:

```json
{
  "type": "module",
  "scripts": {
    "test": "node --test"
  }
}
```

Create `calculator.js`:

```javascript
export function add(a, b) {
  return a + b;
}

export function subtract(a, b) {
  return a - b;
}

export function multiply(a, b) {
  return a * b;
}

export function divide(a, b) {
  if (b === 0) {
    throw new Error("Cannot divide by zero");
  }

  return a / b;
}
```

Create `calculator.test.js`:

```javascript
import test from "node:test";
import assert from "node:assert/strict";

import {
  add,
  subtract,
  multiply,
  divide
} from "./calculator.js";

test("adds two numbers", () => {
  assert.equal(add(5, 3), 8);
});

test("subtracts two numbers", () => {
  assert.equal(subtract(5, 3), 2);
});

test("multiplies two numbers", () => {
  assert.equal(multiply(5, 3), 15);
});

test("divides two numbers", () => {
  assert.equal(divide(10, 2), 5);
});
```

Run the tests:

```bash
npm test
```

Node.js discovers and executes the test file.

# 🗁 Understanding the Test Structure

A typical unit test contains three stages:

**Arrange**

Prepare the required input values and dependencies.

**Act**

Execute the function or method being tested.

**Assert**

Verify that the actual result matches the expected result.

Example:

```javascript
test("calculates an order total", () => {
  // Arrange
  const price = 100;
  const quantity = 3;

  // Act
  const result = price * quantity;

  // Assert
  assert.equal(result, 300);
});
```

This pattern is commonly known as AAA: Arrange, Act, Assert.

# 🗁 Test Names and Descriptions

Test names should describe the behavior being verified.

Avoid vague names:

```javascript
test("test 1", () => {
  assert.equal(add(2, 3), 5);
});
```

Prefer descriptive names:

```javascript
test("returns the sum of two positive numbers", () => {
  assert.equal(add(2, 3), 5);
});
```

A good test description communicates the expected behavior without requiring the reader to inspect every assertion.

Useful naming patterns include:

* `returns ... when ...`
* `throws ... when ...`
* `calculates ... for ...`
* `rejects ... when ...`
* `handles ... correctly`

# 🗁 Testing Different Input Categories

A unit should be tested with different categories of input.

For a function that calculates a total:

```javascript
export function calculateTotal(price, quantity) {
  return price * quantity;
}
```

Possible test scenarios:

```javascript
test("calculates a normal total", () => {
  assert.equal(calculateTotal(100, 3), 300);
});

test("handles zero quantity", () => {
  assert.equal(calculateTotal(100, 0), 0);
});

test("handles zero price", () => {
  assert.equal(calculateTotal(0, 5), 0);
});

test("handles decimal prices", () => {
  assert.equal(calculateTotal(12.5, 2), 25);
});

test("handles negative values mathematically", () => {
  assert.equal(calculateTotal(-10, 2), -20);
});
```

Whether negative values should be accepted is a business rule. If they are invalid, the function should validate them and the tests should verify that behavior.

# 🗁 Testing Edge Cases

An edge case is an input or condition at the boundary of expected behavior.

Consider:

```javascript
export function getDiscount(price, isMember) {
  if (price < 0) {
    throw new Error("Price cannot be negative");
  }

  if (isMember) {
    return price * 0.1;
  }

  return 0;
}
```

Tests:

```javascript
test("applies a discount to members", () => {
  assert.equal(getDiscount(100, true), 10);
});

test("does not apply a discount to non-members", () => {
  assert.equal(getDiscount(100, false), 0);
});

test("accepts a zero price", () => {
  assert.equal(getDiscount(0, true), 0);
});

test("rejects negative prices", () => {
  assert.throws(
    () => getDiscount(-10, true),
    {
      message: "Price cannot be negative"
    }
  );
});
```

Important edge cases often include:

* Empty strings.
* Empty arrays.
* Empty objects.
* Zero.
* Negative numbers.
* Very large numbers.
* Missing properties.
* `null`.
* `undefined`.
* Invalid data types.
* Boundary values.

Test edge cases according to the function's contract rather than attempting every theoretically possible input.

# 🗁 Testing Exceptions

Use `assert.throws()` to verify that a synchronous function throws an expected error.

Example:

```javascript
function validateAge(age) {
  if (age < 18) {
    throw new Error("Must be at least 18");
  }

  return true;
}
```

Test:

```javascript
test("rejects users under 18", () => {
  assert.throws(
    () => validateAge(16),
    {
      message: "Must be at least 18"
    }
  );
});
```

You can also verify the error type:

```javascript
test("throws a TypeError for invalid input", () => {
  assert.throws(
    () => {
      throw new TypeError("Invalid value");
    },
    TypeError
  );
});
```

For asynchronous functions, use `assert.rejects()` instead.

# 🗁 Testing Asynchronous Functions

JavaScript applications frequently use Promises and `async`/`await`.

The test must wait for asynchronous operations to complete.

Example:

```javascript
export async function getUserName() {
  return "Alice";
}
```

Test:

```javascript
test("returns a user name", async () => {
  const result = await getUserName();

  assert.equal(result, "Alice");
});
```

If the test does not await the asynchronous operation or return its Promise, the test runner may finish before the assertion executes.

# 🗁 Testing Promise Rejections

Use `assert.rejects()` to verify that a Promise rejects as expected.

Example:

```javascript
export async function fetchUser(id) {
  if (!id) {
    throw new Error("User ID is required");
  }

  return {
    id,
    name: "Alice"
  };
}
```

Test:

```javascript
test("rejects when the user ID is missing", async () => {
  await assert.rejects(
    fetchUser(null),
    {
      message: "User ID is required"
    }
  );
});
```

This verifies that the asynchronous function rejects with the expected error.

# 🗁 Testing Objects and Arrays

Use strict deep equality assertions to compare object and array structures.

Example:

```javascript
export function createUser(name, age) {
  return {
    name,
    age,
    active: true
  };
}
```

Test:

```javascript
test("creates a user with the expected properties", () => {
  const result = createUser("Alice", 25);

  assert.deepStrictEqual(result, {
    name: "Alice",
    age: 25,
    active: true
  });
});
```

For arrays:

```javascript
test("returns the expected list of numbers", () => {
  const result = [1, 2, 3];

  assert.deepStrictEqual(result, [1, 2, 3]);
});
```

Strict deep equality checks the structure and values of nested data.

# 🗁 Grouping Related Tests

The Node.js test runner provides `describe()` for organizing related tests.

Example:

```javascript
import test, { describe } from "node:test";
import assert from "node:assert/strict";

import {
  add,
  subtract
} from "./calculator.js";

describe("Calculator", () => {
  describe("add()", () => {
    test("adds positive numbers", () => {
      assert.equal(add(2, 3), 5);
    });

    test("adds negative numbers", () => {
      assert.equal(add(-2, -3), -5);
    });
  });

  describe("subtract()", () => {
    test("subtracts two numbers", () => {
      assert.equal(subtract(5, 3), 2);
    });
  });
});
```

Grouping tests makes larger test suites easier to navigate.

# 🗁 Setup and Teardown Hooks

Setup and teardown operations prepare the test environment and clean it up afterward.

Node.js provides hooks such as:

* `before()` — runs before tests in a scope.
* `after()` — runs after tests in a scope.
* `beforeEach()` — runs before each test.
* `afterEach()` — runs after each test.

Example:

```javascript
import test, {
  beforeEach,
  afterEach
} from "node:test";

import assert from "node:assert/strict";

let users;

beforeEach(() => {
  users = [];
});

afterEach(() => {
  users = [];
});

test("adds a user", () => {
  users.push("Alice");

  assert.equal(users.length, 1);
});

test("starts with an empty user list", () => {
  assert.equal(users.length, 0);
});
```

Each test receives a fresh array because `beforeEach()` resets the state.

Use hooks for shared setup and cleanup, but avoid hiding important test behavior inside complicated setup logic.

# 🗁 Testing Pure Functions

Pure functions are particularly easy to unit test because their results depend only on their arguments.

Example:

```javascript
export function formatName(firstName, lastName) {
  return `${firstName.trim()} ${lastName.trim()}`;
}
```

Tests:

```javascript
test("formats a full name", () => {
  assert.equal(
    formatName("Alice", "Smith"),
    "Alice Smith"
  );
});

test("removes surrounding whitespace", () => {
  assert.equal(
    formatName(" Alice ", " Smith "),
    "Alice Smith"
  );
});
```

Pure functions generally do not require mocks, databases, network requests, or complicated test environments.

# 🗁 Testing Functions with Dependencies

A function may depend on another module or service.

Example:

```javascript
export function getUser(id, repository) {
  return repository.findById(id);
}
```

A test can supply a fake repository:

```javascript
test("retrieves a user from the repository", () => {
  const repository = {
    findById(id) {
      return {
        id,
        name: "Alice"
      };
    }
  };

  const result = getUser(1, repository);

  assert.deepStrictEqual(result, {
    id: 1,
    name: "Alice"
  });
});
```

This verifies the behavior without requiring a real database.

Passing dependencies explicitly is one way to make application code easier to test.

# 🗁 Stubs, Mocks, and Spies

Unit tests often replace external dependencies with test doubles.

**Stub**

Returns predefined data.

```javascript
const userRepository = {
  findById() {
    return {
      id: 1,
      name: "Alice"
    };
  }
};
```

**Spy**

Records information about function calls.

```javascript
let callCount = 0;

const logger = {
  info(message) {
    callCount++;
  }
};

logger.info("Application started");

assert.equal(callCount, 1);
```

**Mock**

Verifies expected interactions, such as whether a dependency was called with particular arguments.

Node.js also provides `mock` support through its built-in test runner.

Example:

```javascript
import test from "node:test";
import assert from "node:assert/strict";

test("calls the notification service", (context) => {
  const sendNotification = context.mock.fn();

  sendNotification("Welcome");

  assert.equal(sendNotification.mock.callCount(), 1);

  assert.deepStrictEqual(
    sendNotification.mock.calls[0].arguments,
    ["Welcome"]
  );
});
```

Mocks and spies are useful when interaction with a dependency is part of the behavior being tested.

# 🗁 Mocking Time and Other Built-in Behavior

Some functions depend on the current time, timers, or other environment behavior.

For example:

```javascript
export function isExpired(expirationTime) {
  return Date.now() > expirationTime;
}
```

This function is harder to test consistently because its output depends on the actual clock.

A better design injects the current time:

```javascript
export function isExpired(expirationTime, now = Date.now()) {
  return now > expirationTime;
}
```

Tests:

```javascript
test("returns true when the expiration time has passed", () => {
  assert.equal(isExpired(1000, 2000), true);
});

test("returns false before expiration", () => {
  assert.equal(isExpired(3000, 2000), false);
});
```

Dependency injection makes time-dependent logic deterministic.

Testing frameworks also provide fake timers for applications that need to test timer behavior.

# 🗁 Testing Without External Dependencies

A unit test should avoid unnecessary communication with external systems.

Consider a function that retrieves data from an API:

```javascript
export async function getProducts() {
  const response = await fetch("/api/products");

  if (!response.ok) {
    throw new Error("Failed to retrieve products");
  }

  return response.json();
}
```

Calling this function directly in a unit test may require a network environment.

Instead, separate the data transformation from the network request.

```javascript
export function normalizeProducts(products) {
  return products.map((product) => ({
    ...product,
    name: product.name.trim()
  }));
}
```

Test the transformation independently:

```javascript
test("normalizes product names", () => {
  const products = [
    { id: 1, name: " Laptop " },
    { id: 2, name: " Mouse " }
  ];

  const result = normalizeProducts(products);

  assert.deepStrictEqual(result, [
    { id: 1, name: "Laptop" },
    { id: 2, name: "Mouse" }
  ]);
});
```

The network interaction can be tested separately using an appropriate integration test or a controlled mock.

# 🗁 Test Coverage

Test coverage measures how much of the source code is executed during testing.

Common metrics include:

* Statement coverage.
* Branch coverage.
* Function coverage.
* Line coverage.

Node.js supports coverage reporting through its test runner.

Run:

```bash
node --experimental-test-coverage --test
```

Coverage reporting can help identify code that has not been exercised.

However, executing a line does not guarantee that its behavior has been verified correctly.

Meaningful assertions are more important than maximizing a coverage percentage.

# 🗁 Testing with Different Frameworks

JavaScript developers can choose from several testing tools.

| Tool | Primary Purpose |
| --- | --- |
| Node.js Test Runner | Built-in testing for Node.js |
| Jest | Testing framework with assertions, mocks, and utilities |
| Vitest | Fast testing framework with modern JavaScript tooling integration |
| Mocha | Flexible JavaScript test framework |
| Jasmine | Behavior-driven testing framework |
| Testing Library | Testing user-facing component behavior |

The choice depends on the application, runtime, ecosystem, and team preferences.

The Node.js test runner is a practical starting point because it does not require an additional testing framework.

# 🗁 Running Unit Tests in CI

Unit tests should run automatically when code changes are pushed or submitted for review.

A simple GitHub Actions workflow:

```yaml
name: Unit Tests

on:
  push:
  pull_request:

jobs:
  test:
    runs-on: ubuntu-latest

    steps:
      - name: Checkout repository
        uses: actions/checkout@v4

      - name: Setup Node.js
        uses: actions/setup-node@v4
        with:
          node-version: 22

      - name: Install dependencies
        run: npm ci

      - name: Run unit tests
        run: npm test
```

This workflow checks out the repository, installs dependencies, and runs the test suite.

For long-lived projects, use a Node.js version that matches the application's supported runtime policy and keep GitHub Actions versions updated according to the repository's maintenance practices.

# 🗁 Best Practices

* Test one behavior at a time.
* Use descriptive test names.
* Follow Arrange, Act, Assert.
* Cover normal cases, edge cases, and failure conditions.
* Keep tests independent and deterministic.
* Prefer pure functions where practical.
* Use dependency injection for external dependencies.
* Avoid unnecessary mocks.
* Use assertions that verify meaningful outcomes.
* Keep setup and teardown logic simple.
* Run tests automatically in CI.
* Treat coverage as a diagnostic metric, not a quality guarantee.
* Update tests whenever application behavior changes.

# 🗁 Common Unit Testing Mistakes

**Testing multiple unrelated behaviors in one test**

Large tests make it harder to identify which behavior failed.

**Writing tests that depend on shared mutable state**

A test should not produce different results depending on which tests ran before it.

**Using real external services unnecessarily**

Network requests and database operations make unit tests slower and less predictable.

**Mocking everything**

Excessive mocking can hide integration problems and make tests verify implementation details instead of behavior.

**Ignoring failure scenarios**

Testing only successful inputs leaves important application behavior unverified.

**Writing assertions that are too weak**

A test that only checks whether a function executed does not necessarily verify its correctness.

**Making tests dependent on internal implementation**

Tests should usually verify observable behavior so that internal refactoring does not break otherwise valid tests.

# 🗁 Practical Exercise

Build a small user validation module.

Create `user.js`:

```javascript
export function validateUser(user) {
  if (!user) {
    throw new Error("User is required");
  }

  if (!user.name || user.name.trim() === "") {
    throw new Error("Name is required");
  }

  if (!Number.isInteger(user.age) || user.age < 18) {
    throw new Error("User must be at least 18");
  }

  return {
    name: user.name.trim(),
    age: user.age
  };
}
```

Create `user.test.js`:

```javascript
import test from "node:test";
import assert from "node:assert/strict";

import { validateUser } from "./user.js";

test("validates a valid user", () => {
  const result = validateUser({
    name: "Alice",
    age: 25
  });

  assert.deepStrictEqual(result, {
    name: "Alice",
    age: 25
  });
});

test("rejects a missing user", () => {
  assert.throws(
    () => validateUser(null),
    {
      message: "User is required"
    }
  );
});

test("rejects an empty name", () => {
  assert.throws(
    () => validateUser({
      name: " ",
      age: 25
    }),
    {
      message: "Name is required"
    }
  );
});

test("rejects users under 18", () => {
  assert.throws(
    () => validateUser({
      name: "Alice",
      age: 16
    }),
    {
      message: "User must be at least 18"
    }
  );
});
```

Run the tests:

```bash
node --test
```

Additional scenarios to implement:

* Missing age.
* Decimal age.
* Negative age.
* Empty string as the name.
* Name with surrounding whitespace.
* Age exactly 18.
* Non-string name.

# 🗁 Summary

Unit testing verifies individual units of code independently to ensure they behave as expected.

Important concepts include:

* Unit test structure.
* Arrange, Act, Assert.
* Test naming.
* Input categories and edge cases.
* Synchronous and asynchronous assertions.
* Exception testing.
* Test grouping.
* Setup and teardown hooks.
* Pure function testing.
* Dependency injection.
* Stubs, mocks, and spies.
* Test coverage.
* Node.js built-in test runner.
* Continuous Integration.

Effective unit testing helps developers detect defects early, refactor confidently, and maintain reliable JavaScript applications.
