# JavaScript Test Frameworks

A test framework is a software tool that provides a structured environment for writing, organizing, executing, and reporting automated tests.

In JavaScript development, test frameworks help developers verify that application code behaves as expected.

A test framework typically provides:

* Test definitions.
* Test suites and grouping.
* Assertions.
* Setup and teardown hooks.
* Test execution.
* Reporting.
* Mocking and spying utilities.
* Asynchronous test support.

For example:

```javascript
describe("Calculator", () => {
  test("adds two numbers", () => {
    expect(2 + 3).toBe(5);
  });
});
```

Here:

* `describe()` groups related tests.
* `test()` defines an individual test.
* `expect()` creates an assertion.
* `toBe()` checks the expected result.

# 🗁 Popular JavaScript Testing Frameworks

Several testing frameworks are widely used in the JavaScript ecosystem.

| Framework | Primary Use | Key Characteristics |
| --- | --- | --- |
| Jest | General JavaScript testing | All-in-one testing toolkit |
| Vitest | Modern JavaScript and TypeScript projects | Fast execution and Vite integration |
| Mocha | Flexible JavaScript testing | Extensible test runner |
| Jasmine | Behavior-driven testing | Built-in assertions and spies |
| Node.js Test Runner | Native Node.js testing | Built into Node.js |

# 🗁 Jest

<badge color="success">Popular</badge>

<Entity value="Jest" category="software" disambig="JavaScript testing framework"/> is a JavaScript testing framework developed by Meta.

It provides an integrated testing environment with:

* Test runners.
* Assertions.
* Mock functions.
* Snapshot testing.
* Code coverage.
* Asynchronous testing.
* Setup and teardown hooks.

Install Jest:

```bash
npm install --save-dev jest
```

For a project using ES modules, configure Jest according to its ESM documentation and your Node.js version. The following basic example uses CommonJS.

Create a module:

```javascript
// calculator.js

function add(a, b) {
  return a + b;
}

function subtract(a, b) {
  return a - b;
}

module.exports = {
  add,
  subtract,
};
```

Create a test:

```javascript
// calculator.test.js

const {
  add,
  subtract,
} = require("./calculator");

describe("Calculator", () => {
  test("adds two numbers", () => {
    expect(add(2, 3)).toBe(5);
  });

  test("subtracts two numbers", () => {
    expect(subtract(5, 3)).toBe(2);
  });
});
```

Configure the test script:

```json
{
  "scripts": {
    "test": "jest"
  }
}
```

Run tests:

```bash
npm test
```

Common Jest matchers:

```javascript
expect(value).toBe(10);
expect(value).toEqual({ name: "John" });
expect(value).toBeTruthy();
expect(value).toBeFalsy();
expect(value).toBeNull();
expect(value).toBeDefined();
expect(array).toContain("JavaScript");
expect(value).toBeGreaterThan(5);
expect(callback).toHaveBeenCalled();
```

Jest is particularly useful when a project needs an established testing ecosystem with built-in mocking and coverage support.

Documentation: <https://jestjs.io/>

# 🗁 Vitest

<badge color="success">Modern</badge>

<Entity value="Vitest" category="software" disambig="JavaScript and TypeScript testing framework"/> is a modern testing framework designed to integrate closely with Vite.

It provides:

* Fast test execution.
* Jest-compatible APIs.
* Native ES module support.
* TypeScript support.
* Mocking and spying.
* Code coverage.
* Watch mode.
* Browser testing capabilities.

Install Vitest:

```bash
npm install --save-dev vitest
```

Create a module:

```javascript
// calculator.js

export function add(a, b) {
  return a + b;
}

export function multiply(a, b) {
  return a * b;
}
```

Create a test:

```javascript
// calculator.test.js

import {
  describe,
  test,
  expect,
} from "vitest";

import {
  add,
  multiply,
} from "./calculator.js";

describe("Calculator", () => {
  test("adds two numbers", () => {
    expect(add(2, 3)).toBe(5);
  });

  test("multiplies two numbers", () => {
    expect(multiply(2, 3)).toBe(6);
  });
});
```

Configure `package.json`:

```json
{
  "scripts": {
    "test": "vitest",
    "test:run": "vitest run"
  }
}
```

Run tests:

```bash
npm test
```

Run tests once without watch mode:

```bash
npm run test:run
```

Vitest also supports configuration:

```javascript
// vitest.config.js

import { defineConfig } from "vitest/config";

export default defineConfig({
  test: {
    environment: "node",
    coverage: {
      provider: "v8",
      reporter: ["text", "html"],
    },
  },
});
```

Install the coverage provider if needed:

```bash
npm install --save-dev @vitest/coverage-v8
```

Vitest is an excellent choice for modern Vite-based applications and many standalone JavaScript or TypeScript projects.

Documentation: <https://vitest.dev/>

# 🗁 Mocha

<Entity value="Mocha" category="software" disambig="JavaScript test framework"/> is a flexible JavaScript test framework that runs tests in Node.js and browsers.

Unlike Jest and Vitest, Mocha traditionally focuses on test execution rather than providing every testing utility itself.

It is commonly combined with:

* Chai for assertions.
* Sinon for mocks, stubs, and spies.
* NYC for code coverage.

Install Mocha and Chai:

```bash
npm install --save-dev mocha chai
```

Create a module:

```javascript
// calculator.js

export function add(a, b) {
  return a + b;
}
```

Create a test:

```javascript
// calculator.test.js

import { describe, it } from "mocha";
import { expect } from "chai";

import { add } from "./calculator.js";

describe("Calculator", () => {
  it("adds two numbers", () => {
    expect(add(2, 3)).to.equal(5);
  });
});
```

Configure the test script:

```json
{
  "scripts": {
    "test": "mocha"
  }
}
```

Run:

```bash
npm test
```

Mocha is useful when developers want control over their testing stack and prefer selecting separate tools for assertions, mocking, and coverage.

Documentation: <https://mochajs.org/>

# 🗁 Jasmine

<Entity value="Jasmine" category="software" disambig="JavaScript behavior-driven development testing framework"/> is a behavior-driven development (BDD) testing framework for JavaScript.

It includes:

* Test suites.
* Specifications.
* Matchers.
* Spies.
* Setup and teardown functions.
* Asynchronous testing support.

Install Jasmine:

```bash
npm install --save-dev jasmine
```

Initialize its configuration:

```bash
npx jasmine init
```

Create a module:

```javascript
// calculator.js

export function add(a, b) {
  return a + b;
}
```

Create a specification:

```javascript
// spec/calculator.spec.js

import { add } from "../calculator.js";

describe("Calculator", () => {
  it("adds two numbers", () => {
    expect(add(2, 3)).toBe(5);
  });
});
```

Run:

```bash
npx jasmine
```

Jasmine is useful for projects that prefer a BDD-style testing approach with built-in testing utilities.

Documentation: <https://jasmine.github.io/>

# 🗁 Node.js Built-in Test Runner

Node.js provides a native test runner through the `node:test` module.

It allows developers to write automated tests without installing an external testing framework.

The built-in test runner supports:

* Test definitions.
* Nested test suites.
* Assertions through `node:assert`.
* Hooks.
* Mocking utilities.
* Synchronous and asynchronous tests.
* Test execution and reporting.

Create a module:

```javascript
// calculator.js

export function add(a, b) {
  return a + b;
}
```

Create a test:

```javascript
// calculator.test.js

import test from "node:test";
import assert from "node:assert/strict";

import { add } from "./calculator.js";

test("Calculator", async (t) => {
  await t.test("adds two numbers", () => {
    assert.equal(add(2, 3), 5);
  });
});
```

Run tests:

```bash
node --test
```

Alternatively, define a package script:

```json
{
  "scripts": {
    "test": "node --test"
  }
}
```

Run:

```bash
npm test
```

The built-in test runner is particularly useful for Node.js applications that need automated testing without additional framework dependencies.

Documentation: <https://nodejs.org/api/test.html>

# 🗁 Comparison of Testing Frameworks

| Feature | Jest | Vitest | Mocha | Jasmine | Node.js Test Runner |
| --- | --- | --- | --- | --- | --- |
| Test runner | Built-in | Built-in | Built-in | Built-in | Built-in |
| Assertions | Built-in | Built-in | External or custom | Built-in | `node:assert` |
| Mocking | Built-in | Built-in | External or custom | Built-in spies | Built-in utilities |
| Coverage | Built-in integration | Provider-based | External tools | External tools | Supported through Node.js tooling |
| ES modules | Supported | Native support | Supported | Supported | Native support |
| TypeScript | Requires suitable tooling | Supported | Requires suitable tooling | Requires suitable tooling | Requires suitable tooling |
| Vite integration | Possible | Excellent | Possible | Possible | Not specific to Vite |

# 🗁 Choosing the Right Framework

The best framework depends on project requirements and the existing development environment.

| Project Requirement | Suggested Framework |
| --- | --- |
| Modern Vite application | Vitest |
| Established Jest-based project | Jest |
| Flexible testing architecture | Mocha |
| BDD-oriented testing | Jasmine |
| Minimal Node.js testing dependencies | Node.js Test Runner |

Consider the following factors:

* **Ecosystem:** Check compatibility with the libraries and tools already used by the project.
* **Execution speed:** Consider test startup time, parallel execution, and overall test duration.
* **Developer experience:** Evaluate debugging, watch mode, reporting, and IDE integration.
* **Mocking:** Determine whether the framework provides the mocking capabilities your application needs.
* **Coverage:** Check whether coverage reporting integrates with your CI pipeline.
* **Maintenance:** Prefer actively maintained tools with reliable documentation.

# 🗁 Running Tests in Development and CI

Testing frameworks can execute tests in different modes.

**Watch mode**

Automatically reruns relevant tests when files change.

```bash
npx vitest
```

**Single execution**

Runs the tests once and exits.

```bash
npx vitest run
```

**Code coverage**

Generates a coverage report.

```bash
npx vitest run --coverage
```

**Continuous integration**

Run tests as part of an automated pipeline:

```bash
npm ci
npm test
```

For CI environments, use a non-watch test command so the process exits after completing the test suite.

# 🗁 Recommended Testing Stack

For a modern JavaScript or TypeScript application using Vite, a practical testing stack is:

| Tool | Responsibility |
| --- | --- |
| Vitest | Test execution and assertions |
| Testing Library | Component and user-interaction testing |
| Supertest | HTTP API integration testing |
| MSW | HTTP request mocking |
| V8 Coverage | Code coverage collection |
| Playwright | End-to-end browser testing |

These tools solve different testing problems and can be combined when appropriate.

# 🗁 Summary

JavaScript testing frameworks provide tools for writing and executing automated tests.

Key concepts:

* Jest provides an established all-in-one testing ecosystem.
* Vitest offers modern testing with strong Vite integration.
* Mocha provides a flexible and extensible testing architecture.
* Jasmine provides a BDD-oriented testing environment.
* Node.js includes a native test runner.
* Framework selection should depend on project requirements, ecosystem compatibility, and developer workflow.
