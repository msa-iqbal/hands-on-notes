# Test Coverage in TypeScript

Test coverage is a measurement of how much of an application's source code is executed while running automated tests.

It helps developers identify parts of the codebase that are not exercised by existing tests.

Test coverage is commonly measured using four metrics:

* Statement coverage.
* Branch coverage.
* Function coverage.
* Line coverage.

Coverage is useful for identifying testing gaps, but high coverage does not automatically mean that an application is thoroughly tested.

# 🗁 Why Test Coverage Matters

Test coverage helps developers:

* Identify untested code.
* Find missing test cases.
* Evaluate testing completeness.
* Detect untested conditional paths.
* Improve regression testing.
* Monitor testing quality over time.

For example, a function may have tests for successful execution but no tests for invalid input or error handling.

Coverage reports can help reveal these gaps.

# 🗁 Types of Test Coverage

**Statement Coverage**

Measures the percentage of executable statements that have been executed by tests.

```typescript
function getDiscount(price: number) {
  const discount = price * 0.1;
  const finalPrice = price - discount;

  return finalPrice;
}
```

A test that calls `getDiscount()` executes all three statements.

**Branch Coverage**

Measures whether each possible branch of a conditional expression has been executed.

```typescript
function getAccessLevel(isAdmin: boolean) {
  if (isAdmin) {
    return "admin";
  }

  return "user";
}
```

Two test cases are needed to cover both branches:

```typescript
expect(getAccessLevel(true)).toBe("admin");
expect(getAccessLevel(false)).toBe("user");
```

**Function Coverage**

Measures the percentage of declared functions that have been called during testing.

```typescript
function add(a: number, b: number) {
  return a + b;
}

function subtract(a: number, b: number) {
  return a - b;
}
```

If tests call only `add()`, function coverage is incomplete.

**Line Coverage**

Measures the percentage of executable source lines that have been executed.

Line coverage is related to statement coverage, but they are not identical. Multiple statements can exist on one line, and a single statement can span multiple lines.

# 🗁 Coverage Metrics Comparison

| Metric | Measures |
| --- | --- |
| Statement | Executed statements |
| Branch | Executed conditional paths |
| Function | Called functions |
| Line | Executed executable lines |

# 🗁 Generating Coverage with Vitest

Vitest supports code coverage through dedicated coverage providers.

Install the V8 coverage provider:

```bash
npm install -D @vitest/coverage-v8
```

Alternatively, use the Istanbul provider:

```bash
npm install -D @vitest/coverage-istanbul
```

Use a provider compatible with your installed Vitest version.

# 🗁 Configuring Coverage

Configure coverage in `vitest.config.ts`:

```typescript
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
        "src/**/*.ts",
      ],
      exclude: [
        "src/**/*.test.ts",
        "src/**/*.spec.ts",
      ],
    },
  },
});
```

Important options:

| Option | Purpose |
| --- | --- |
| `provider` | Selects the coverage engine |
| `reporter` | Defines report formats |
| `include` | Specifies files to include |
| `exclude` | Excludes files from coverage |
| `thresholds` | Defines minimum coverage requirements |

# 🗁 Running Coverage Reports

Add a coverage script to `package.json`:

```json
{
  "scripts": {
    "test": "vitest run",
    "test:coverage": "vitest run --coverage"
  }
}
```

Run the coverage command:

```bash
npm run test:coverage
```

Vitest executes the tests and generates coverage reports.

# 🗁 Understanding Coverage Reports

A text report may look like this:

```text
File                 % Stmts  % Branch  % Funcs  % Lines
--------------------------------------------------------
src/calculator.ts      100       100       100      100
src/user-service.ts     80        50        75       80
--------------------------------------------------------
All files               90        75        87.5     90
```

The metrics indicate how much of the source code was exercised.

For example:

* `100%` statement coverage means all measured statements executed.
* `50%` branch coverage means only half of the measured branches executed.
* `75%` function coverage means three-quarters of the measured functions executed.

Coverage percentages are measurements, not guarantees of correctness.

# 🗁 HTML Coverage Reports

The HTML reporter generates a browsable report.

With the configuration above, run:

```bash
npm run test:coverage
```

Open:

```text
coverage/index.html
```

The HTML report typically provides:

* Overall coverage summaries.
* Per-file coverage statistics.
* Source code highlighting.
* Uncovered line indicators.
* Branch coverage details.

This makes it easier to locate untested code.

# 🗁 Setting Coverage Thresholds

Coverage thresholds enforce minimum coverage requirements.

Example:

```typescript
import { defineConfig } from "vitest/config";

export default defineConfig({
  test: {
    coverage: {
      provider: "v8",
      reporter: ["text", "html"],
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

With these thresholds, the coverage check fails if the measured coverage falls below the configured minimum.

Thresholds help maintain a consistent testing baseline as a project grows.

# 🗁 Excluding Files from Coverage

Some files do not need direct coverage measurement.

Examples include:

* Generated files.
* Configuration files.
* Type declaration files.
* Test files.
* Build output.

You can exclude them through the coverage configuration:

```typescript
coverage: {
  exclude: [
    "src/**/*.test.ts",
    "src/**/*.spec.ts",
    "src/**/*.d.ts",
    "src/generated/**",
    "src/**/*.config.ts",
  ],
}
```

Keep exclusions intentional. Excluding too many files can make coverage percentages misleading.

# 🗁 Testing Uncovered Branches

Consider a function with multiple execution paths:

```typescript
function calculatePrice(
  price: number,
  discount: number
): number {
  if (price < 0) {
    throw new Error("Invalid price");
  }

  if (discount < 0 || discount > 100) {
    throw new Error("Invalid discount");
  }

  return price - (price * discount) / 100;
}
```

A comprehensive test suite should cover:

* A valid price and discount.
* A negative price.
* A negative discount.
* A discount greater than 100.
* Boundary values such as zero and 100.

Example:

```typescript
import { describe, it, expect } from "vitest";

import { calculatePrice } from "./calculator";

describe("calculatePrice", () => {
  it("calculates discounted price", () => {
    expect(calculatePrice(100, 20)).toBe(80);
  });

  it("rejects negative prices", () => {
    expect(() => calculatePrice(-100, 20))
      .toThrow("Invalid price");
  });

  it("rejects negative discounts", () => {
    expect(() => calculatePrice(100, -10))
      .toThrow("Invalid discount");
  });

  it("rejects discounts above 100", () => {
    expect(() => calculatePrice(100, 120))
      .toThrow("Invalid discount");
  });

  it("supports a zero discount", () => {
    expect(calculatePrice(100, 0)).toBe(100);
  });

  it("supports a full discount", () => {
    expect(calculatePrice(100, 100)).toBe(0);
  });
});
```

The purpose is to exercise meaningful execution paths, not simply increase a percentage.

# 🗁 Coverage in CI/CD Pipelines

Coverage checks can run automatically in continuous integration pipelines.

A typical workflow is:

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
Run type checking
        |
        v
Run automated tests
        |
        v
Generate coverage report
        |
        v
Check coverage thresholds
        |
        v
Build and deploy
```

A pipeline can reject changes when tests fail or coverage falls below an agreed threshold.

# 🗁 Coverage Best Practices

* Prioritize meaningful test cases over percentage targets.
* Test success paths, failure paths, and boundary conditions.
* Pay attention to branch coverage.
* Cover critical business logic thoroughly.
* Include coverage checks in CI pipelines.
* Review uncovered code regularly.
* Keep coverage exclusions minimal and justified.
* Establish realistic thresholds for the project.
* Avoid writing meaningless tests simply to increase coverage.

# 🗁 Common Mistakes

**Treating 100% coverage as perfect testing**

Code can execute without its behavior being properly verified.

**Ignoring branch coverage**

A function can have high line coverage while important conditional paths remain untested.

**Testing only happy paths**

Error handling, invalid input, and boundary conditions also need attention.

**Excluding too many files**

Excessive exclusions can inflate reported coverage.

**Using coverage as the only quality metric**

Coverage should complement code review, static analysis, type checking, and meaningful assertions.

# 🗁 Key Takeaways

* Test coverage measures which parts of source code execute during testing.
* Statement, branch, function, and line coverage measure different aspects.
* Vitest supports coverage through V8 and Istanbul providers.
* HTML reports help identify uncovered code.
* Coverage thresholds enforce minimum testing requirements.
* Branch coverage is particularly useful for identifying missing conditional test cases.
* High coverage does not guarantee correct application behavior.
* Meaningful assertions and representative test cases matter more than percentages.
