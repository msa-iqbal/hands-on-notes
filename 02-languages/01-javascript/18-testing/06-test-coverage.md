# JavaScript Test Coverage

Test coverage is a software testing metric that measures how much of an application's source code is executed while automated tests run.

It helps developers identify code that has not been exercised by their test suites.

For example, consider this function:

```javascript
function calculateDiscount(price, isPremium) {
  if (isPremium) {
    return price * 0.8;
  }

  return price;
}
```

If a test only executes the premium-user branch, the other branch remains untested.

```javascript
test("applies premium discount", () => {
  expect(calculateDiscount(100, true)).toBe(80);
});
```

Coverage tools can identify that the `isPremium === false` path was never executed.

# 🗁 Why Test Coverage Matters

Test coverage helps developers:

* Identify untested source code.
* Discover missing test scenarios.
* Find unexecuted conditional branches.
* Improve test-suite completeness.
* Monitor testing progress.
* Detect regressions in critical application areas.
* Establish measurable quality goals.

However, high coverage does not automatically mean that software is correct.

Coverage measures which code executes, not whether the tests verify the correct behavior.

# 🗁 Types of Test Coverage

Several coverage metrics help evaluate different aspects of a test suite.

# 🗁 Statement Coverage

Statement coverage measures the percentage of executable statements executed during testing.

Formula:

```text
Statement Coverage =
  (Executed Statements / Total Executable Statements) × 100
```

Example:

```javascript
function calculateTotal(price, quantity) {
  const subtotal = price * quantity;
  const tax = subtotal * 0.1;
  return subtotal + tax;
}
```

If all three executable statements run during testing:

```text
Statement Coverage = (3 / 3) × 100
                  = 100%
```

# 🗁 Branch Coverage

Branch coverage measures whether each possible outcome of a conditional decision has been executed.

Consider:

```javascript
function getAccess(isAuthenticated) {
  if (isAuthenticated) {
    return "Granted";
  }

  return "Denied";
}
```

There are two branches:

* `isAuthenticated === true`
* `isAuthenticated === false`

Testing only the successful branch does not provide complete branch coverage.

```javascript
test("grants authenticated users access", () => {
  expect(getAccess(true)).toBe("Granted");
});

test("denies unauthenticated users access", () => {
  expect(getAccess(false)).toBe("Denied");
});
```

Formula:

```text
Branch Coverage =
  (Executed Branches / Total Branches) × 100
```

# 🗁 Function Coverage

Function coverage measures the percentage of functions that are called during testing.

```javascript
function add(a, b) {
  return a + b;
}

function subtract(a, b) {
  return a - b;
}

function multiply(a, b) {
  return a * b;
}
```

If tests call `add()` and `subtract()` but never call `multiply()`:

```text
Function Coverage = (2 / 3) × 100
                  = 66.67%
```

# 🗁 Line Coverage

Line coverage measures the percentage of executable source-code lines executed during testing.

It is similar to statement coverage, but the two metrics can differ when multiple statements appear on the same line or when source mapping is involved.

Example:

```javascript
function greet(name) {
  const message = `Hello, ${name}`;
  console.log(message);
  return message;
}
```

If all executable lines run, the function has complete line coverage.

# 🗁 Condition Coverage

Condition coverage measures whether each individual Boolean condition has evaluated to both `true` and `false`.

Consider:

```javascript
function canAccess(user) {
  if (user.isActive && user.isAdmin) {
    return true;
  }

  return false;
}
```

The conditions are:

* `user.isActive`
* `user.isAdmin`

A useful test set should make each condition evaluate to both Boolean outcomes.

```javascript
test("allows an active administrator", () => {
  expect(
    canAccess({
      isActive: true,
      isAdmin: true,
    })
  ).toBe(true);
});

test("denies an inactive administrator", () => {
  expect(
    canAccess({
      isActive: false,
      isAdmin: true,
    })
  ).toBe(false);
});

test("denies an active non-administrator", () => {
  expect(
    canAccess({
      isActive: true,
      isAdmin: false,
    })
  ).toBe(false);
});
```

These tests exercise both outcomes of each condition.

Condition coverage is different from branch coverage. Branch coverage checks decision outcomes, while condition coverage checks the individual Boolean expressions.

# 🗁 Coverage Metrics Comparison

| Coverage Type | Measures |
| --- | --- |
| Statement | Executed statements |
| Line | Executed source lines |
| Branch | Executed decision outcomes |
| Function | Functions called |
| Condition | Individual Boolean condition outcomes |

No single coverage metric provides a complete picture of software quality.

# 🗁 Measuring Coverage with Vitest

Vitest supports coverage reporting through coverage providers.

The two commonly available providers are:

* `v8`: Uses V8's native coverage information.
* `istanbul`: Uses Istanbul instrumentation.

Install the V8 coverage provider:

```bash
npm install --save-dev @vitest/coverage-v8
```

Alternatively, install Istanbul:

```bash
npm install --save-dev @vitest/coverage-istanbul
```

Configure Vitest:

```javascript
// vitest.config.js

import { defineConfig } from "vitest/config";

export default defineConfig({
  test: {
    coverage: {
      provider: "v8",
      reporter: [
        "text",
        "html",
        "lcov",
      ],
      include: [
        "src/**/*.js",
      ],
      exclude: [
        "src/**/*.test.js",
        "src/**/*.spec.js",
      ],
    },
  },
});
```

Run tests with coverage:

```bash
npx vitest run --coverage
```

Vitest generates coverage information based on the configured provider and reporters.

# 🗁 Understanding Coverage Reports

A typical coverage report looks like this:

```text
File                 | % Stmts | % Branch | % Funcs | % Lines
---------------------|---------|----------|---------|-------
calculator.js        | 100.00  | 100.00   | 100.00  | 100.00
userService.js       | 85.00   | 75.00    | 90.00   | 85.00
paymentService.js    | 60.00   | 50.00    | 70.00   | 60.00
```

The columns represent:

* `% Stmts`: Statement coverage.
* `% Branch`: Branch coverage.
* `% Funcs`: Function coverage.
* `% Lines`: Line coverage.

The report helps identify modules that need additional test scenarios.

An HTML report can provide more detailed information about uncovered lines and branches.

# 🗁 Measuring Coverage with Jest

Jest also provides built-in coverage reporting.

Run:

```bash
npx jest --coverage
```

Configure coverage in `package.json`:

```json
{
  "jest": {
    "collectCoverage": true,
    "collectCoverageFrom": [
      "src/**/*.js",
      "!src/**/*.test.js",
      "!src/**/*.spec.js"
    ],
    "coverageDirectory": "coverage",
    "coverageReporters": [
      "text",
      "html",
      "lcov"
    ]
  }
}
```

Run:

```bash
npm test
```

Jest generates coverage reports using its configured coverage provider.

# 🗁 Setting Coverage Thresholds

Coverage thresholds establish minimum acceptable coverage levels.

They help prevent coverage from decreasing unnoticed.

Vitest example:

```javascript
// vitest.config.js

import { defineConfig } from "vitest/config";

export default defineConfig({
  test: {
    coverage: {
      provider: "v8",
      thresholds: {
        statements: 80,
        branches: 75,
        functions: 80,
        lines: 80,
      },
    },
  },
});
```

Jest example:

```javascript
// jest.config.js

export default {
  coverageThreshold: {
    global: {
      statements: 80,
      branches: 75,
      functions: 80,
      lines: 80,
    },
  },
};
```

If coverage falls below the configured thresholds, the test command can fail.

Thresholds should reflect the project's actual quality requirements rather than arbitrary targets.

# 🗁 Excluding Files from Coverage

Some files do not need to be included in coverage calculations.

Examples include:

* Generated files.
* Build output.
* Configuration files.
* Test files.
* Type declaration files.
* Third-party code.

Vitest example:

```javascript
coverage: {
  exclude: [
    "node_modules/**",
    "dist/**",
    "**/*.test.js",
    "**/*.spec.js",
    "**/*.d.ts",
  ],
}
```

Exclusions should be deliberate. Excluding too much code can make coverage reports misleading.

# 🗁 Coverage in Continuous Integration

Coverage reporting is particularly useful in continuous integration pipelines.

A typical CI workflow:

```bash
npm ci
npm run test:coverage
```

Example package script:

```json
{
  "scripts": {
    "test": "vitest",
    "test:run": "vitest run",
    "test:coverage": "vitest run --coverage"
  }
}
```

A CI pipeline can use coverage thresholds to prevent a build from passing when required coverage levels are not met.

Coverage reports can also be uploaded to external reporting services for historical tracking and team visibility.

# 🗁 Code Coverage vs Test Quality

High code coverage does not guarantee effective tests.

Consider:

```javascript
function divide(a, b) {
  return a / b;
}
```

A test that only checks whether the function executes can increase coverage without verifying its correctness.

```javascript
test("executes divide", () => {
  divide(10, 2);
});
```

A meaningful test verifies the expected result:

```javascript
test("divides two numbers correctly", () => {
  expect(divide(10, 2)).toBe(5);
});
```

Additional tests should verify important edge cases:

```javascript
test("handles division by zero", () => {
  expect(divide(10, 0)).toBe(Infinity);
});
```

The correct behavior for division by zero depends on the application's requirements. The test should encode the intended contract.

# 🗁 Coverage vs Mutation Testing

Coverage and mutation testing measure different aspects of test effectiveness.

**Code coverage** measures which parts of the source code execute.

**Mutation testing** evaluates whether tests detect intentionally introduced defects.

For example, a mutation testing tool might change:

```javascript
return a + b;
```

Into:

```javascript
return a - b;
```

If the existing tests still pass, they may not adequately verify the function's behavior.

Popular JavaScript mutation testing tools include:

* StrykerJS.

Documentation: <https://stryker-mutator.io/>

# 🗁 Test Coverage Best Practices

* Prioritize meaningful tests over maximum coverage percentages.
* Measure statement, branch, function, and line coverage.
* Focus on critical business logic.
* Test error handling and edge cases.
* Configure reasonable coverage thresholds.
* Review uncovered code regularly.
* Include coverage checks in CI pipelines.
* Avoid excluding files simply to improve percentages.
* Use mutation testing to evaluate test effectiveness.
* Maintain clear and deterministic test cases.

# 🗁 Common Test Coverage Mistakes

| Mistake | Better Approach |
| --- | --- |
| Chasing 100% coverage blindly | Prioritize meaningful behavioral verification |
| Testing only successful paths | Include failure and boundary scenarios |
| Ignoring branch coverage | Verify conditional outcomes |
| Excluding too many files | Keep exclusions transparent and justified |
| Treating coverage as a quality guarantee | Evaluate assertions and test effectiveness |
| Setting arbitrary thresholds | Align thresholds with project risk and requirements |

# 🗁 Summary

Test coverage measures how much source code is executed by automated tests.

Key concepts:

* Statement coverage measures executed statements.
* Line coverage measures executed source lines.
* Branch coverage measures decision outcomes.
* Function coverage measures called functions.
* Condition coverage measures Boolean condition outcomes.
* Vitest and Jest provide coverage reporting.
* Coverage thresholds help enforce testing standards.
* High coverage does not guarantee high-quality tests.
* Mutation testing can reveal weaknesses in test assertions.

The goal of test coverage is not simply to execute every line of code. It is to identify untested behavior and improve confidence in application correctness.
