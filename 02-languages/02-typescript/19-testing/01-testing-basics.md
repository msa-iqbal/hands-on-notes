# Testing Basics

Testing is the process of verifying that software behaves as expected under different conditions.

In TypeScript development, testing helps ensure that functions, classes, modules, and application components produce correct results and handle unexpected situations properly.

TypeScript provides static type checking, but type safety alone does not guarantee runtime correctness.

Testing verifies actual application behavior.

# 🗁 What Is Software Testing?

Software testing involves executing or analyzing software to identify defects and verify expected behavior.

Consider this TypeScript function:

```typescript
function add(a: number, b: number): number {
  return a + b;
}
```

Its types describe the expected inputs and output.

However, tests are needed to verify its behavior:

```typescript
console.log(add(10, 5)); // 15
console.log(add(-2, 2)); // 0
console.log(add(0, 0));  // 0
```

Testing makes these expectations repeatable and automatically verifiable.

# 🗁 Why Testing Is Important

Testing provides several benefits:

* Detects defects before deployment.
* Verifies expected application behavior.
* Prevents existing functionality from breaking.
* Improves code reliability.
* Makes refactoring safer.
* Documents expected behavior through executable examples.
* Helps identify edge cases.
* Supports continuous integration and delivery.

# 🗁 Type Checking vs Testing

Type checking and testing solve different problems.

| Type Checking | Testing |
| --- | --- |
| Checks type correctness | Checks application behavior |
| Usually performed at compile time | Can be performed at compile time or runtime |
| Detects type-related errors | Detects logical and behavioral errors |
| Analyzes type compatibility | Executes or analyzes test cases |
| Cannot guarantee correct output | Verifies expected output |

Example:

```typescript
function divide(a: number, b: number): number {
  return a / b;
}
```

TypeScript accepts this function because its types are valid.

However:

```typescript
console.log(divide(10, 0)); // Infinity
```

The function has a possible logical problem that ordinary type checking does not detect.

A test can verify the expected behavior for division by zero.

# 🗁 Types of Software Testing

Software testing can be organized into several levels and categories.

**Unit Testing**

Tests individual functions, methods, or small isolated components.

**Integration Testing**

Tests whether multiple modules or services work correctly together.

**System Testing**

Tests the complete application as an integrated system.

**End-to-End Testing**

Tests complete user workflows from the application's entry point to the expected result.

**Regression Testing**

Verifies that previously working functionality continues to work after changes.

**Performance Testing**

Evaluates application performance under different workloads.

# 🗁 Unit Testing

Unit testing verifies the behavior of small, isolated pieces of software.

Example function:

```typescript
export function calculateTotal(
  price: number,
  quantity: number
): number {
  return price * quantity;
}
```

Expected behavior:

* `calculateTotal(100, 2)` should return `200`.
* `calculateTotal(50, 3)` should return `150`.
* `calculateTotal(0, 5)` should return `0`.

A unit test checks these expectations automatically.

Example using Vitest:

```typescript
import { describe, expect, it } from "vitest";
import { calculateTotal } from "./calculateTotal";

describe("calculateTotal", () => {
  it("multiplies price by quantity", () => {
    expect(calculateTotal(100, 2)).toBe(200);
  });

  it("returns zero when price is zero", () => {
    expect(calculateTotal(0, 5)).toBe(0);
  });
});
```

# 🗁 Integration Testing

Integration testing verifies that multiple units work together correctly.

For example, an order service may depend on:

* Product service.
* Inventory service.
* Payment service.
* Database layer.

A test can verify that these components interact correctly when processing an order.

Example:

```typescript
interface Product {
  id: number;
  price: number;
}

interface Order {
  productId: number;
  quantity: number;
}

function calculateOrderTotal(
  order: Order,
  product: Product
): number {
  if (order.productId !== product.id) {
    throw new Error("Product mismatch");
  }

  return product.price * order.quantity;
}
```

An integration test could verify that order data and product data are correctly combined.

# 🗁 System Testing

System testing evaluates the complete application after its components have been integrated.

For example, an e-commerce application may be tested to verify:

* User registration.
* Authentication.
* Product browsing.
* Cart management.
* Checkout.
* Payment processing.
* Order history.

The objective is to verify that the application satisfies its overall functional requirements.

# 🗁 End-to-End Testing

End-to-end testing verifies complete application workflows from the user's perspective.

For example, an e-commerce checkout workflow might involve:

```text
Open Application
       |
       v
Login
       |
       v
Browse Products
       |
       v
Add Product to Cart
       |
       v
Checkout
       |
       v
Complete Payment
       |
       v
Verify Order Confirmation
```

End-to-end tests commonly use browser automation tools such as Playwright and Cypress.

# 🗁 Regression Testing

Regression testing ensures that changes do not break existing functionality.

Suppose a developer modifies a discount calculation function.

Previously working scenarios should continue to produce correct results.

Example:

```typescript
export function applyDiscount(
  price: number,
  discountPercentage: number
): number {
  return price - (price * discountPercentage) / 100;
}
```

Tests:

```typescript
import { describe, expect, it } from "vitest";
import { applyDiscount } from "./discount";

describe("applyDiscount", () => {
  it("applies a 10 percent discount", () => {
    expect(applyDiscount(100, 10)).toBe(90);
  });

  it("returns the original price for zero discount", () => {
    expect(applyDiscount(100, 0)).toBe(100);
  });
});
```

Whenever the implementation changes, these tests can be executed again to verify existing behavior.

# 🗁 Test Case

A test case describes a specific scenario that should be verified.

A test case commonly contains:

| Element | Description |
| --- | --- |
| Test name | Identifies the scenario |
| Input | Data supplied to the functionality |
| Expected result | Correct expected behavior |
| Actual result | Behavior observed during execution |
| Status | Passed or failed |

Example:

| Test Case | Input | Expected Result |
| --- | --- | --- |
| Positive addition | `add(5, 3)` | `8` |
| Negative addition | `add(-5, 3)` | `-2` |
| Zero addition | `add(0, 0)` | `0` |

# 🗁 Test Suite

A test suite is a collection of related test cases.

For example, a test suite for an authentication service might contain tests for:

* Valid credentials.
* Invalid passwords.
* Missing email addresses.
* Expired sessions.
* Account lockouts.

In Vitest:

```typescript
import { describe, expect, it } from "vitest";

describe("Authentication", () => {
  it("accepts valid credentials", () => {
    expect(true).toBe(true);
  });

  it("rejects invalid credentials", () => {
    expect(false).toBe(false);
  });
});
```

`describe()` groups related tests, while `it()` defines an individual test case.

# 🗁 Arrange, Act, Assert Pattern

Arrange, Act, Assert (AAA) is a common structure for writing readable tests.

**Arrange**

Prepare the test data and required dependencies.

**Act**

Execute the functionality being tested.

**Assert**

Verify that the actual result matches the expected result.

Example:

```typescript
import { expect, it } from "vitest";

function multiply(a: number, b: number): number {
  return a * b;
}

it("multiplies two numbers", () => {
  // Arrange
  const firstNumber = 5;
  const secondNumber = 4;

  // Act
  const result = multiply(firstNumber, secondNumber);

  // Assert
  expect(result).toBe(20);
});
```

The AAA pattern makes test cases easier to understand and maintain.

# 🗁 Test Assertions

Assertions verify whether actual results satisfy expected conditions.

Common assertion examples using Vitest:

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

Common assertion methods:

| Assertion | Purpose |
| --- | --- |
| `toBe()` | Strict equality |
| `toEqual()` | Deep equality comparison |
| `toStrictEqual()` | Strict deep equality |
| `toBeTruthy()` | Checks truthiness |
| `toBeFalsy()` | Checks falsiness |
| `toBeNull()` | Checks for `null` |
| `toBeUndefined()` | Checks for `undefined` |
| `toContain()` | Checks collection membership |
| `toThrow()` | Checks whether a function throws |
| `toBeGreaterThan()` | Checks greater-than comparison |
| `toBeLessThan()` | Checks less-than comparison |

# 🗁 Positive and Negative Testing

Positive testing verifies that software behaves correctly with valid inputs.

Negative testing verifies how software behaves with invalid or unexpected inputs.

Example:

```typescript
function getUsername(username: string): string {
  if (username.trim().length === 0) {
    throw new Error("Username is required");
  }

  return username;
}
```

Positive test:

```typescript
expect(getUsername("alice")).toBe("alice");
```

Negative test:

```typescript
expect(() => getUsername("")).toThrow(
  "Username is required"
);
```

Both types of tests are important for reliable software.

# 🗁 Edge Case Testing

Edge cases are unusual or boundary conditions that may expose defects.

For a quantity validation function, useful cases include:

* Zero.
* Negative numbers.
* Minimum allowed value.
* Maximum allowed value.
* Values immediately outside the allowed range.
* Very large numbers.
* Unexpected input values at runtime.

Example:

```typescript
function isValidQuantity(quantity: number): boolean {
  return Number.isInteger(quantity) &&
    quantity >= 1 &&
    quantity <= 100;
}
```

Tests:

```typescript
expect(isValidQuantity(1)).toBe(true);
expect(isValidQuantity(100)).toBe(true);
expect(isValidQuantity(0)).toBe(false);
expect(isValidQuantity(101)).toBe(false);
expect(isValidQuantity(1.5)).toBe(false);
```

Boundary testing helps identify defects that ordinary test cases may miss.

# 🗁 Test-Driven Development

Test-Driven Development (TDD) is a development approach in which tests are written before the implementation.

It follows the Red-Green-Refactor cycle.

**Red**

Write a test that fails because the functionality does not exist yet.

**Green**

Implement the minimum functionality required to make the test pass.

**Refactor**

Improve the implementation while keeping all tests passing.

Workflow:

```text
Write a Failing Test
        |
        v
Implement Minimum Code
        |
        v
Make the Test Pass
        |
        v
Refactor Implementation
        |
        v
Run Tests Again
```

TDD encourages small development steps and clearly defined expected behavior.

# 🗁 Behavior-Driven Development

Behavior-Driven Development (BDD) focuses on describing software behavior in terms that developers, testers, and business stakeholders can understand.

A common format is:

* Given: Initial context.
* When: An action occurs.
* Then: An expected outcome.

Example:

```text
Feature: User Authentication

Scenario: Successful login

Given a registered user exists
When the user provides valid credentials
Then the user should be authenticated
```

BDD helps connect technical implementation with business requirements.

# 🗁 Code Coverage

Code coverage measures how much of the source code is executed by a test suite.

Common coverage metrics include:

| Metric | Description |
| --- | --- |
| Statement Coverage | Percentage of executed statements |
| Branch Coverage | Percentage of executed conditional branches |
| Function Coverage | Percentage of executed functions |
| Line Coverage | Percentage of executed source lines |

Example:

```typescript
function getDiscount(price: number): number {
  if (price > 100) {
    return price * 0.1;
  }

  return 0;
}
```

Testing only a price of `150` executes the first branch but not the second.

A test suite with both `150` and `50` exercises both branches.

High coverage does not automatically mean high-quality tests. Assertions must still verify meaningful behavior.

# 🗁 Testing Tools for TypeScript

Several tools are commonly used in TypeScript projects.

| Tool | Primary Purpose |
| --- | --- |
| Vitest | Unit and integration testing |
| Jest | JavaScript and TypeScript testing |
| Mocha | JavaScript testing framework |
| Playwright | Browser automation and end-to-end testing |
| Cypress | Browser-based application testing |
| Testing Library | Testing user-facing UI behavior |
| Supertest | HTTP endpoint testing |
| MSW | API request mocking |
| Istanbul / c8 | Code coverage |

The appropriate tool depends on the application architecture and testing requirements.

# 🗁 Best Practices

* Write tests for important business logic.
* Keep test cases small and focused.
* Use descriptive test names.
* Follow Arrange, Act, Assert where appropriate.
* Test positive, negative, and boundary scenarios.
* Prefer testing observable behavior over implementation details.
* Avoid unnecessary dependencies between tests.
* Keep tests deterministic and repeatable.
* Use mocks only when isolation or control is necessary.
* Run tests automatically in CI/CD pipelines.
* Treat coverage as a measurement, not a quality guarantee.
* Keep test code readable and maintainable.

# 🗁 Summary

Testing verifies that software behaves as expected and continues working as it evolves.

Important concepts include:

* Unit, integration, system, and end-to-end testing.
* Regression testing.
* Test cases and test suites.
* Assertions.
* Arrange, Act, Assert.
* Positive, negative, and edge case testing.
* Test-Driven Development.
* Behavior-Driven Development.
* Code coverage.

TypeScript type checking catches many programming errors, while testing verifies actual runtime behavior. Both are essential parts of a reliable development workflow.
