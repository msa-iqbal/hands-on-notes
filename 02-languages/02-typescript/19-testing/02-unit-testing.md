# Unit Testing

Unit testing is a software testing technique that verifies individual units of code independently.

A unit is usually a small, isolated piece of functionality, such as:

* A function.
* A class method.
* A class.
* A utility module.
* A business logic component.

In TypeScript, unit testing helps verify that individual units produce expected results for different inputs and conditions.

# 🗁 What Is Unit Testing?

Unit testing focuses on testing the smallest practical pieces of application functionality.

Consider this function:

```typescript
export function calculateArea(
  width: number,
  height: number
): number {
  return width * height;
}
```

A unit test verifies that the function calculates the correct area.

```typescript
import { describe, expect, it } from "vitest";
import { calculateArea } from "./calculateArea";

describe("calculateArea", () => {
  it("calculates the area correctly", () => {
    expect(calculateArea(10, 5)).toBe(50);
  });
});
```

The test verifies one specific unit without requiring the entire application to run.

# 🗁 Why Unit Testing Is Important

Unit testing provides several advantages:

* Detects bugs early in development.
* Verifies individual functions and methods.
* Makes refactoring safer.
* Documents expected behavior.
* Reduces debugging time.
* Helps identify edge cases.
* Prevents regressions.
* Encourages modular and maintainable code.

# 🗁 Characteristics of Good Unit Tests

Good unit tests should have the following characteristics.

**Isolated**

A unit test should focus on one unit of functionality without depending unnecessarily on unrelated components.

**Repeatable**

Running the same test multiple times under the same conditions should produce the same result.

**Deterministic**

A test should not produce unpredictable results because of random values, external services, or timing.

**Fast**

Unit tests should execute quickly so developers can run them frequently.

**Independent**

One test should not depend on another test's execution order or state.

**Readable**

Test names and assertions should clearly communicate the expected behavior.

# 🗁 Setting Up Vitest

Vitest is a testing framework that integrates naturally with modern TypeScript and Vite projects.

Install the required dependencies:

```bash
npm install --save-dev vitest typescript
```

For a basic TypeScript project, create the following structure:

```text
my-app/
├── src/
│   └── calculator.ts
├── tests/
│   └── calculator.test.ts
├── package.json
└── tsconfig.json
```

Add a test script to `package.json`:

```json
{
  "scripts": {
    "test": "vitest",
    "test:run": "vitest run"
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

# 🗁 Writing Your First Unit Test

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

Create its test file:

**tests/calculator.test.ts**

```typescript
import { describe, expect, it } from "vitest";

import {
  add,
  subtract,
  multiply
} from "../src/calculator";

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

Vitest discovers the test file, executes the test cases, and reports their results.

# 🗁 Testing Functions

Functions are among the most common units tested in TypeScript applications.

Example:

```typescript
export function isAdult(age: number): boolean {
  return age >= 18;
}
```

Tests:

```typescript
import { describe, expect, it } from "vitest";
import { isAdult } from "../src/isAdult";

describe("isAdult", () => {
  it("returns true for an adult", () => {
    expect(isAdult(25)).toBe(true);
  });

  it("returns true at the age boundary", () => {
    expect(isAdult(18)).toBe(true);
  });

  it("returns false for a minor", () => {
    expect(isAdult(16)).toBe(false);
  });
});
```

Test different inputs to verify the function's behavior across its expected input range.

# 🗁 Testing Class Methods

Classes can be tested by creating instances and verifying their public behavior.

Example:

**src/BankAccount.ts**

```typescript
export class BankAccount {
  private balance: number;

  constructor(initialBalance: number = 0) {
    this.balance = initialBalance;
  }

  deposit(amount: number): void {
    if (amount <= 0) {
      throw new Error("Deposit amount must be positive");
    }

    this.balance += amount;
  }

  withdraw(amount: number): void {
    if (amount <= 0) {
      throw new Error("Withdrawal amount must be positive");
    }

    if (amount > this.balance) {
      throw new Error("Insufficient balance");
    }

    this.balance -= amount;
  }

  getBalance(): number {
    return this.balance;
  }
}
```

Test file:

**tests/BankAccount.test.ts**

```typescript
import { describe, expect, it } from "vitest";
import { BankAccount } from "../src/BankAccount";

describe("BankAccount", () => {
  it("initializes with the provided balance", () => {
    const account = new BankAccount(100);

    expect(account.getBalance()).toBe(100);
  });

  it("deposits money", () => {
    const account = new BankAccount(100);

    account.deposit(50);

    expect(account.getBalance()).toBe(150);
  });

  it("withdraws money", () => {
    const account = new BankAccount(100);

    account.withdraw(30);

    expect(account.getBalance()).toBe(70);
  });

  it("rejects negative deposits", () => {
    const account = new BankAccount(100);

    expect(() => account.deposit(-10)).toThrow(
      "Deposit amount must be positive"
    );
  });

  it("rejects withdrawals exceeding the balance", () => {
    const account = new BankAccount(100);

    expect(() => account.withdraw(150)).toThrow(
      "Insufficient balance"
    );
  });
});
```

These tests verify the class's public behavior without directly accessing its private state.

# 🗁 Testing Different Input Scenarios

A good unit test suite covers more than one ordinary input.

Consider:

```typescript
export function calculateDiscount(
  price: number,
  percentage: number
): number {
  if (price < 0) {
    throw new Error("Price cannot be negative");
  }

  if (percentage < 0 || percentage > 100) {
    throw new Error("Invalid discount percentage");
  }

  return price - (price * percentage) / 100;
}
```

Important scenarios include:

| Scenario | Input | Expected Result |
| --- | --- | --- |
| Normal discount | `(100, 10)` | `90` |
| No discount | `(100, 0)` | `100` |
| Full discount | `(100, 100)` | `0` |
| Zero price | `(0, 10)` | `0` |
| Negative price | `(-100, 10)` | Error |
| Negative discount | `(100, -10)` | Error |
| Excessive discount | `(100, 110)` | Error |

Tests:

```typescript
import { describe, expect, it } from "vitest";
import { calculateDiscount } from "../src/calculateDiscount";

describe("calculateDiscount", () => {
  it("calculates a normal discount", () => {
    expect(calculateDiscount(100, 10)).toBe(90);
  });

  it("handles zero discount", () => {
    expect(calculateDiscount(100, 0)).toBe(100);
  });

  it("handles full discount", () => {
    expect(calculateDiscount(100, 100)).toBe(0);
  });

  it("rejects negative prices", () => {
    expect(() => calculateDiscount(-100, 10)).toThrow();
  });

  it("rejects invalid discount percentages", () => {
    expect(() => calculateDiscount(100, 110)).toThrow();
  });
});
```

# 🗁 Testing Exceptions

Functions sometimes throw errors when they receive invalid input or encounter unexpected conditions.

Vitest provides `toThrow()` for testing synchronous exceptions.

Example:

```typescript
function divide(a: number, b: number): number {
  if (b === 0) {
    throw new Error("Division by zero");
  }

  return a / b;
}
```

Test:

```typescript
import { expect, it } from "vitest";

it("throws an error when dividing by zero", () => {
  expect(() => divide(10, 0)).toThrow(
    "Division by zero"
  );
});
```

The function must be wrapped in a callback so the testing framework can capture the exception.

# 🗁 Testing Asynchronous Functions

Asynchronous functions return promises and require asynchronous assertions.

Example:

```typescript
export async function fetchUserName(): Promise<string> {
  return "Alice";
}
```

Test:

```typescript
import { expect, it } from "vitest";
import { fetchUserName } from "../src/fetchUserName";

it("returns the user name", async () => {
  await expect(fetchUserName()).resolves.toBe("Alice");
});
```

Testing rejected promises:

```typescript
export async function fetchUser(): Promise<never> {
  throw new Error("User not found");
}
```

```typescript
import { expect, it } from "vitest";
import { fetchUser } from "../src/fetchUser";

it("rejects when the user is unavailable", async () => {
  await expect(fetchUser()).rejects.toThrow(
    "User not found"
  );
});
```

Always await asynchronous assertions to ensure the test runner correctly waits for their completion.

# 🗁 Testing with Parameterized Tests

Parameterized tests execute the same test logic with multiple input values.

Vitest provides `it.each()` and `test.each()` for this purpose.

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

Parameterized tests reduce duplication when verifying the same behavior across many inputs.

# 🗁 Testing Object Values

Objects can be tested using deep equality assertions.

Example:

```typescript
interface User {
  id: number;
  name: string;
  active: boolean;
}

export function createUser(
  id: number,
  name: string
): User {
  return {
    id,
    name,
    active: true
  };
}
```

Test:

```typescript
import { expect, it } from "vitest";
import { createUser } from "../src/createUser";

it("creates a user with the expected properties", () => {
  expect(createUser(1, "Alice")).toEqual({
    id: 1,
    name: "Alice",
    active: true
  });
});
```

Use `toEqual()` for deep structural comparisons and `toStrictEqual()` when stricter comparison semantics are required.

# 🗁 Testing Arrays and Collections

Array-related functions should be tested for normal, empty, and boundary cases.

Example:

```typescript
export function getActiveUsers(
  users: { name: string; active: boolean }[]
): string[] {
  return users
    .filter(user => user.active)
    .map(user => user.name);
}
```

Tests:

```typescript
import { expect, it } from "vitest";
import { getActiveUsers } from "../src/getActiveUsers";

it("returns names of active users", () => {
  const users = [
    { name: "Alice", active: true },
    { name: "Bob", active: false },
    { name: "Charlie", active: true }
  ];

  expect(getActiveUsers(users)).toEqual([
    "Alice",
    "Charlie"
  ]);
});

it("returns an empty array when there are no users", () => {
  expect(getActiveUsers([])).toEqual([]);
});
```

# 🗁 Test Isolation

Test isolation means each test executes independently without relying on state left behind by other tests.

Consider a shared mutable object:

```typescript
const users: string[] = [];

function addUser(name: string): void {
  users.push(name);
}
```

If tests share this array, one test may affect another.

Vitest provides lifecycle hooks such as:

* `beforeEach()`
* `afterEach()`
* `beforeAll()`
* `afterAll()`

Example:

```typescript
import {
  afterEach,
  beforeEach,
  describe,
  expect,
  it
} from "vitest";

const users: string[] = [];

function addUser(name: string): void {
  users.push(name);
}

describe("User collection", () => {
  beforeEach(() => {
    users.length = 0;
  });

  afterEach(() => {
    users.length = 0;
  });

  it("adds a user", () => {
    addUser("Alice");

    expect(users).toEqual(["Alice"]);
  });

  it("starts with an empty collection", () => {
    expect(users).toEqual([]);
  });
});
```

Each test starts with a predictable state.

# 🗁 Testing Pure Functions

A pure function produces the same output for the same input and does not cause observable side effects.

Example:

```typescript
function square(number: number): number {
  return number * number;
}
```

Tests:

```typescript
import { expect, it } from "vitest";

it("returns the square of a number", () => {
  expect(square(4)).toBe(16);
  expect(square(5)).toBe(25);
});
```

Pure functions are generally easy to unit test because they do not depend on external state.

# 🗁 Testing Functions with Dependencies

Some functions depend on external services, databases, APIs, or other modules.

For example:

```typescript
interface UserRepository {
  findById(id: number): Promise<string | null>;
}

export class UserService {
  constructor(
    private readonly repository: UserRepository
  ) {}

  async getUserName(id: number): Promise<string> {
    const user = await this.repository.findById(id);

    if (!user) {
      throw new Error("User not found");
    }

    return user;
  }
}
```

A unit test can provide a simple fake repository:

```typescript
import { describe, expect, it } from "vitest";
import { UserService } from "../src/UserService";

describe("UserService", () => {
  it("returns the user name", async () => {
    const fakeRepository = {
      findById: async () => "Alice"
    };

    const service = new UserService(fakeRepository);

    await expect(service.getUserName(1)).resolves.toBe(
      "Alice"
    );
  });

  it("throws when the user does not exist", async () => {
    const fakeRepository = {
      findById: async () => null
    };

    const service = new UserService(fakeRepository);

    await expect(service.getUserName(1)).rejects.toThrow(
      "User not found"
    );
  });
});
```

This approach isolates the service from the actual database or external infrastructure.

# 🗁 Unit Testing vs Integration Testing

| Unit Testing | Integration Testing |
| --- | --- |
| Tests individual units | Tests interactions between units |
| Usually isolates dependencies | Uses multiple real or controlled components |
| Executes quickly | May execute more slowly |
| Focuses on individual behavior | Focuses on component interactions |
| Often uses fakes or mocks | May use real databases or services |
| Easier to pinpoint failures | Can expose integration problems |

Both testing approaches are important and complement each other.

# 🗁 Running Unit Tests in Watch Mode

Watch mode automatically reruns relevant tests when source or test files change.

Run:

```bash
npx vitest
```

Run tests once:

```bash
npx vitest run
```

Run a specific test file:

```bash
npx vitest run tests/calculator.test.ts
```

Run tests matching a name:

```bash
npx vitest run -t "calculates the area"
```

These commands help developers execute focused tests during development.

# 🗁 Best Practices

* Test one logical behavior per test case.
* Use descriptive test names.
* Follow Arrange, Act, Assert.
* Test normal inputs and edge cases.
* Verify errors and invalid inputs.
* Keep tests independent.
* Avoid testing private implementation details.
* Prefer dependency injection for external dependencies.
* Use fakes or mocks when isolation is necessary.
* Avoid unnecessary mocking of simple business logic.
* Keep test execution fast.
* Run unit tests automatically in CI/CD pipelines.

# 🗁 Summary

Unit testing verifies individual functions, methods, classes, and modules independently.

Important concepts include:

* Testing functions and class methods.
* Testing synchronous and asynchronous behavior.
* Testing exceptions.
* Parameterized tests.
* Testing objects and collections.
* Test isolation and lifecycle hooks.
* Pure functions and dependency isolation.
* Running tests in watch mode.

A strong unit test suite helps maintain correctness, simplifies refactoring, and provides confidence when changing application code.
