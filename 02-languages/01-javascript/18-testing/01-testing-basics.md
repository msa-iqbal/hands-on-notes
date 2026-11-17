# Testing Basics

Software testing is the process of verifying that an application behaves as expected and identifying defects before they affect users.

Testing helps developers ensure that code meets functional requirements, handles unexpected situations, and remains reliable as the application evolves.

In JavaScript development, testing can cover individual functions, integrated modules, user interactions, APIs, and complete application workflows.

Example:

```javascript
function add(a, b) {
  return a + b;
}

console.log(add(2, 3)); // 5
```

The output appears correct, but a developer should verify that the function works for different inputs and edge cases.

```javascript
console.assert(add(2, 3) === 5);
console.assert(add(0, 0) === 0);
console.assert(add(-2, 2) === 0);
```

These checks provide basic verification, but a proper testing strategy uses structured test cases and automated test execution.

# 🗁 Why Testing Matters

Testing provides several important benefits:

* Reliability: verifies that application features behave correctly.
* Bug detection: identifies defects before deployment.
* Regression prevention: detects when new changes break existing functionality.
* Maintainability: makes code safer to modify and refactor.
* Documentation: demonstrates how functions and components are expected to behave.
* Confidence: reduces uncertainty when releasing new features.
* Collaboration: helps teams understand expected behavior and detect integration problems.

Testing is not limited to finding bugs. It also helps define and preserve the intended behavior of software.

# 🗁 What Is a Test Case?

A test case describes a specific scenario used to verify application behavior.

A test case generally includes:

* Test description.
* Input data.
* Preconditions.
* Execution steps.
* Expected result.
* Actual result.
* Pass or fail status.

Example:

| Field | Value |
| --- | --- |
| Test case | Verify addition |
| Input | `2, 3` |
| Expected result | `5` |
| Actual result | `5` |
| Status | Pass |

A test suite is a collection of related test cases.

For example, an addition test suite might include positive numbers, negative numbers, zero, and decimal values.

# 🗁 Manual Testing

Manual testing involves executing test scenarios without relying entirely on automated test scripts.

Example:

A developer tests a login form by:

* Opening the login page.
* Entering valid credentials.
* Submitting the form.
* Verifying successful authentication.
* Entering an incorrect password.
* Verifying the error message.
* Submitting an empty form.
* Verifying validation behavior.

Manual testing is useful for exploratory testing, usability checks, and visual verification.

However, repeating the same test scenarios manually can become time-consuming and inconsistent.

# 🗁 Automated Testing

Automated testing uses software tools and scripts to execute test cases and verify expected results.

Example:

```javascript
function multiply(a, b) {
  return a * b;
}

function testMultiply() {
  const result = multiply(4, 5);
  const expected = 20;

  if (result !== expected) {
    throw new Error(
      `Expected ${expected}, received ${result}`
    );
  }

  console.log("Test passed");
}

testMultiply();
```

Output:

```text
Test passed
```

Automated tests can be executed repeatedly and integrated into development workflows and CI/CD pipelines.

# 🗁 Types of Software Testing

Software testing can be classified according to its purpose and scope.

Common testing types include:

* Unit testing.
* Integration testing.
* System testing.
* End-to-end testing.
* Acceptance testing.
* Regression testing.
* Smoke testing.
* Performance testing.
* Security testing.
* Exploratory testing.

These categories are not mutually exclusive. A test can belong to multiple categories depending on what it verifies and how it is executed.

# 🗁 Unit Testing

Unit testing verifies the behavior of a small, isolated unit of code.

A unit is commonly a function, method, or small module.

Example:

```javascript
function calculateTotal(price, quantity) {
  return price * quantity;
}
```

Possible unit test scenarios:

```javascript
calculateTotal(100, 2); // 200
calculateTotal(50, 3);  // 150
calculateTotal(0, 5);   // 0
```

Unit tests should focus on the behavior of the unit rather than unrelated dependencies.

# 🗁 Integration Testing

Integration testing verifies that multiple modules or components work correctly together.

Example:

Consider an application containing:

* User service.
* Database repository.
* Authentication service.

An integration test might verify that the authentication service retrieves a user through the repository and validates the supplied credentials.

Integration testing helps identify problems involving module boundaries, data exchange, configuration, and external dependencies.

# 🗁 System Testing

System testing verifies the behavior of an integrated application as a complete system.

Example:

An e-commerce system may be tested to verify that:

* Users can browse products.
* Products can be added to a cart.
* Orders can be submitted.
* Payments can be processed.
* Order confirmations are generated.

System testing evaluates the application against its overall requirements.

# 🗁 End-to-End Testing

End-to-end testing verifies complete user workflows across the application.

Example:

A browser-based test might perform the following steps:

```text
Open application
       |
       v
Navigate to login
       |
       v
Enter credentials
       |
       v
Submit login form
       |
       v
Open product page
       |
       v
Add product to cart
       |
       v
Complete checkout
       |
       v
Verify order confirmation
```

End-to-end tests help verify that different application layers work together from the user's perspective.

# 🗁 Acceptance Testing

Acceptance testing verifies whether an application satisfies business requirements and user expectations.

It often focuses on real-world workflows rather than implementation details.

Example:

A requirement states:

"Customers must be able to purchase available products using a valid payment method."

An acceptance test verifies that a customer can complete this workflow and receive an order confirmation.

Acceptance testing may be performed by product owners, business stakeholders, testers, or automated acceptance test suites.

# 🗁 Regression Testing

Regression testing verifies that existing functionality continues to work after code changes.

For example:

A developer modifies the checkout calculation logic.

Regression tests should verify that:

* Product prices are still calculated correctly.
* Discounts still work.
* Taxes are calculated correctly.
* Shipping costs are included.
* Existing payment workflows still function.

Regression testing is particularly important when refactoring code or adding new features.

# 🗁 Smoke Testing

Smoke testing performs a quick set of checks to determine whether the most important parts of an application are working.

Example:

After deploying a web application, a smoke test might verify:

* The application starts successfully.
* The homepage loads.
* Authentication works.
* The main API responds.
* Database connectivity is available.

Smoke tests help identify major failures before performing more extensive testing.

# 🗁 Performance Testing

Performance testing evaluates how an application behaves under different workloads.

Common performance testing categories include:

* Load testing: evaluates behavior under expected traffic.
* Stress testing: evaluates behavior beyond normal operating capacity.
* Spike testing: evaluates sudden increases or decreases in workload.
* Endurance testing: evaluates behavior under sustained workloads.
* Scalability testing: evaluates how the system handles increasing demand.

Example:

A web API might be tested with 1,000 concurrent requests to measure response time, throughput, and error rates.

# 🗁 Security Testing

Security testing identifies vulnerabilities and verifies that application security controls work correctly.

Common security testing areas include:

* Authentication.
* Authorization.
* Input validation.
* Session management.
* Access control.
* Injection vulnerabilities.
* Cross-site scripting.
* Sensitive data exposure.
* Rate limiting.

Security testing helps protect applications, users, and data from unauthorized access and abuse.

# 🗁 Exploratory Testing

Exploratory testing involves investigating an application without following only predefined test cases.

The tester explores different workflows, inputs, and application states to discover unexpected behavior.

Example:

While testing a registration form, a tester might experiment with:

* Very long input values.
* Special characters.
* Unexpected Unicode characters.
* Repeated submissions.
* Browser refreshes during submission.
* Network interruptions.

Exploratory testing is useful for discovering edge cases that predefined test scenarios may miss.

# 🗁 The Testing Pyramid

The testing pyramid is a model for organizing automated tests by scope.

<AsyncImage query="software testing pyramid unit integration end to end testing diagram clean technical illustration" aspectRatio="16:9" width="100%" fit="contain"/>

The traditional testing pyramid contains three main layers:

| Layer | Scope | Typical Characteristics |
| --- | --- | --- |
| Unit tests | Individual functions and modules | Numerous, fast, isolated |
| Integration tests | Connected components and services | Moderate quantity and execution cost |
| End-to-end tests | Complete user workflows | Fewer, slower, broader coverage |

The general recommendation is to maintain many fast unit tests, a meaningful integration test suite, and a smaller set of high-value end-to-end tests.

The exact balance depends on the application architecture and risks.

# 🗁 The AAA Testing Pattern

AAA stands for Arrange, Act, and Assert.

It is a common structure for writing readable tests.

**Arrange:** Prepare the test data and dependencies.

**Act:** Execute the behavior being tested.

**Assert:** Verify the expected result.

Example:

```javascript
function calculateTotal(price, quantity) {
  return price * quantity;
}

// Arrange
const price = 100;
const quantity = 3;

// Act
const result = calculateTotal(price, quantity);

// Assert
if (result !== 300) {
  throw new Error("Expected total to be 300");
}
```

This structure separates test setup, execution, and verification.

# 🗁 Assertions

An assertion checks whether an actual value matches an expected condition.

Example:

```javascript
const actual = 10;
const expected = 10;

console.assert(
  actual === expected,
  "Values should be equal"
);
```

Testing frameworks provide dedicated assertion APIs.

Common assertion operations include:

* Equality checks.
* Strict equality checks.
* Truthiness checks.
* Null and undefined checks.
* Array and object comparisons.
* Exception assertions.
* Promise resolution and rejection assertions.

Example using Node.js's built-in assertion module:

```javascript
import assert from "node:assert/strict";

assert.equal(2 + 3, 5);
assert.deepEqual(
  { name: "Alice" },
  { name: "Alice" }
);
```

The `node:assert/strict` module provides assertion functions that throw errors when conditions are not satisfied.

# 🗁 Test Fixtures

A test fixture is predefined data or setup used during test execution.

Example:

```javascript
const userFixture = {
  id: 1,
  name: "Alice",
  email: "alice@example.com",
  active: true
};
```

The same fixture can be reused across multiple tests.

Fixtures help maintain consistency and reduce duplicated setup code.

# 🗁 Test Isolation

Test isolation means that one test should not unintentionally depend on another test's execution or state.

Example of shared mutable state:

```javascript
let balance = 100;

function deposit(amount) {
  balance += amount;
  return balance;
}
```

Tests using this shared state may produce different results depending on execution order.

A better approach:

```javascript
function createAccount(initialBalance = 0) {
  let balance = initialBalance;

  return {
    deposit(amount) {
      balance += amount;
      return balance;
    },

    getBalance() {
      return balance;
    }
  };
}
```

Each test can create its own account:

```javascript
const account = createAccount(100);

account.deposit(50);

console.log(account.getBalance()); // 150
```

Independent test setup makes tests more predictable.

# 🗁 Test Coverage

Test coverage measures how much of the application code is exercised by tests.

Common coverage metrics include:

* Statement coverage: percentage of executed statements.
* Branch coverage: percentage of executed decision branches.
* Function coverage: percentage of executed functions.
* Line coverage: percentage of executed source lines.

Example:

```javascript
function getDiscount(price, isMember) {
  if (isMember) {
    return price * 0.1;
  }

  return 0;
}
```

Testing only a member:

```javascript
getDiscount(100, true);
```

does not exercise the non-member branch.

A more complete set of tests:

```javascript
getDiscount(100, true);
getDiscount(100, false);
```

Coverage metrics help identify untested code, but high coverage does not guarantee correct behavior.

Meaningful assertions and well-designed test cases remain essential.

# 🗁 Test Doubles

A test double is an object or function used in place of a real dependency during testing.

Common types include:

* Dummy: passed around but not used.
* Stub: returns predefined responses.
* Spy: records how a function is called.
* Mock: verifies expected interactions.
* Fake: provides a simplified working implementation.

Example of a simple stub:

```javascript
function getUser(id, repository) {
  return repository.findById(id);
}

const fakeRepository = {
  findById(id) {
    return {
      id,
      name: "Alice"
    };
  }
};

console.log(getUser(1, fakeRepository));
```

The fake repository replaces the actual data-access implementation.

Test doubles help isolate code from databases, APIs, file systems, and other external dependencies.

# 🗁 Testing JavaScript Code with Node.js

Node.js provides a built-in testing framework through `node:test`.

Create a file named `math.js`:

```javascript
export function add(a, b) {
  return a + b;
}

export function multiply(a, b) {
  return a * b;
}
```

Create `math.test.js`:

```javascript
import test from "node:test";
import assert from "node:assert/strict";

import { add, multiply } from "./math.js";

test("adds two numbers", () => {
  assert.equal(add(2, 3), 5);
});

test("multiplies two numbers", () => {
  assert.equal(multiply(4, 5), 20);
});
```

Configure the project to use ES modules with `"type": "module"` in `package.json`, then run:

```bash
node --test
```

Node.js discovers and executes supported test files.

The built-in test runner is useful for testing Node.js applications without installing an external testing framework.

# 🗁 Continuous Integration and Testing

Continuous Integration (CI) automatically builds and tests code changes as part of a development workflow.

A typical CI workflow:

```text
Developer pushes code
        |
        v
CI pipeline starts
        |
        v
Install dependencies
        |
        v
Run linting
        |
        v
Run automated tests
        |
        v
Generate coverage report
        |
        v
Build application
        |
        v
Report success or failure
```

Automated testing in CI helps detect defects before changes are merged or deployed.

Popular CI platforms include GitHub Actions, GitLab CI/CD, and Jenkins.

# 🗁 Testing Best Practices

* Test observable behavior rather than internal implementation details.
* Write independent and repeatable tests.
* Cover normal cases, edge cases, and failure scenarios.
* Use descriptive test names.
* Keep test setup simple.
* Prefer deterministic tests.
* Avoid unnecessary dependencies between tests.
* Use appropriate test doubles.
* Automate regression testing.
* Focus on meaningful assertions rather than coverage percentages alone.
* Run tests automatically in CI.
* Keep tests maintainable as the application evolves.

# 🗁 Common Testing Mistakes

**Testing only successful scenarios**

Applications must also handle invalid input, missing data, and unexpected conditions.

**Writing tests that depend on execution order**

Independent tests are easier to debug and maintain.

**Over-mocking dependencies**

Excessive mocking can cause tests to pass even when real components do not work together correctly.

**Testing implementation details**

Tests that depend heavily on internal implementation can break during harmless refactoring.

**Ignoring asynchronous behavior**

Promises and asynchronous operations must be awaited or otherwise handled correctly.

**Treating coverage as proof of quality**

High coverage does not guarantee that assertions are meaningful or that important behaviors are tested.

**Creating excessively large end-to-end suites**

End-to-end tests are valuable but often slower and more expensive to maintain than unit and integration tests.

# 🗁 Summary

Software testing verifies that applications behave correctly and remain reliable as they change.

Important concepts include:

* Manual and automated testing.
* Test cases and test suites.
* Unit, integration, and system testing.
* End-to-end and acceptance testing.
* Regression and smoke testing.
* Performance and security testing.
* Exploratory testing.
* Testing pyramid.
* Arrange, Act, Assert.
* Assertions and fixtures.
* Test isolation.
* Test coverage.
* Test doubles.
* Node.js built-in testing.
* Continuous Integration.

A strong testing strategy combines different testing levels to verify individual components, interactions between modules, and complete application workflows.
