# JavaScript Integration Testing

Integration testing verifies that multiple software components work together correctly.

Unlike unit testing, which focuses on individual functions or modules in isolation, integration testing checks interactions between components and their dependencies.

For example, a web application might contain:

* A service responsible for business logic.
* A repository responsible for database access.
* An API endpoint responsible for handling HTTP requests.
* A database responsible for storing application data.

An integration test verifies that these components cooperate correctly.

# 🗁 Unit Testing vs Integration Testing

| Aspect | Unit Testing | Integration Testing |
| --- | --- | --- |
| Scope | Individual units | Multiple connected components |
| Dependencies | Usually mocked or isolated | Often real or partially real |
| Execution speed | Fast | Usually slower |
| Main purpose | Verify individual behavior | Verify component interactions |
| Failure diagnosis | Usually straightforward | May involve multiple components |

Example:

```javascript
// Unit test
test("calculates the total", () => {
  expect(calculateTotal(100, 10)).toBe(110);
});
```

An integration test might verify that an order service correctly retrieves product prices, calculates the total, and saves the order through a repository.

# 🗁 What Should Be Integration Tested?

Integration tests are especially useful for verifying:

* Service-to-repository interactions.
* Database queries and persistence.
* API endpoint behavior.
* HTTP client integrations.
* Authentication and authorization flows.
* Data serialization and deserialization.
* Event publishing and consumption.
* Communication between application modules.

Focus on important boundaries where components exchange data or depend on one another.

# 🗁 A Simple Integration Testing Example

Consider an application with a user service and a user repository.

```javascript
// userRepository.js

const users = [];

export function saveUser(user) {
  users.push(user);
  return user;
}

export function findUserById(id) {
  return users.find((user) => user.id === id);
}
```

```javascript
// userService.js

import {
  saveUser,
  findUserById,
} from "./userRepository.js";

export function createUser(name, email) {
  const user = {
    id: crypto.randomUUID(),
    name,
    email,
  };

  return saveUser(user);
}

export function getUser(id) {
  return findUserById(id);
}
```

An integration test verifies that the service and repository work together.

```javascript
// userService.test.js

import {
  describe,
  test,
  expect,
} from "vitest";

import {
  createUser,
  getUser,
} from "./userService.js";

describe("User service integration", () => {
  test("creates and retrieves a user", () => {
    const createdUser = createUser(
      "John",
      "john@example.com"
    );

    const retrievedUser = getUser(createdUser.id);

    expect(retrievedUser).toEqual(createdUser);
  });
});
```

This test exercises both the service and repository rather than testing either component independently.

For production applications, shared in-memory state should be reset between tests or replaced with an isolated test database.

# 🗁 Testing API Integrations

API integration tests verify that HTTP endpoints correctly process requests and return expected responses.

The following example uses Node.js, Express, and Vitest with Supertest.

Install the dependencies:

```bash
npm install express
npm install -D vitest supertest
```

Create an API:

```javascript
// app.js

import express from "express";

const app = express();

app.use(express.json());

const users = [
  {
    id: 1,
    name: "John",
  },
];

app.get("/users", (req, res) => {
  res.json(users);
});

app.get("/users/:id", (req, res) => {
  const user = users.find(
    (item) => item.id === Number(req.params.id)
  );

  if (!user) {
    return res.status(404).json({
      message: "User not found",
    });
  }

  res.json(user);
});

export default app;
```

Write integration tests:

```javascript
// app.test.js

import {
  describe,
  test,
  expect,
} from "vitest";

import request from "supertest";

import app from "./app.js";

describe("User API integration", () => {
  test("GET /users returns all users", async () => {
    const response = await request(app)
      .get("/users");

    expect(response.status).toBe(200);

    expect(response.body).toEqual([
      {
        id: 1,
        name: "John",
      },
    ]);
  });

  test("GET /users/:id returns a user", async () => {
    const response = await request(app)
      .get("/users/1");

    expect(response.status).toBe(200);

    expect(response.body).toEqual({
      id: 1,
      name: "John",
    });
  });

  test("GET /users/:id returns 404 for missing users", async () => {
    const response = await request(app)
      .get("/users/999");

    expect(response.status).toBe(404);

    expect(response.body).toEqual({
      message: "User not found",
    });
  });
});
```

Supertest sends HTTP requests directly to the Express application without requiring a separately running server.

# 🗁 Database Integration Testing

Database integration tests verify that application code interacts correctly with a database.

They can cover:

* Database connections.
* Insert and update operations.
* Query behavior.
* Relationships and constraints.
* Transactions and rollbacks.
* Database migrations.
* Data retrieval and persistence.

Example using a repository abstraction:

```javascript
export class UserRepository {
  constructor(database) {
    this.database = database;
  }

  async create(user) {
    return this.database.user.create({
      data: user,
    });
  }

  async findById(id) {
    return this.database.user.findUnique({
      where: { id },
    });
  }
}
```

An integration test can use a real test database:

```javascript
import {
  beforeAll,
  afterAll,
  test,
  expect,
} from "vitest";

import { database } from "./database.js";
import { UserRepository } from "./userRepository.js";

const repository = new UserRepository(database);

beforeAll(async () => {
  await database.$connect();
});

afterAll(async () => {
  await database.$disconnect();
});

test("creates and retrieves a user", async () => {
  const createdUser = await repository.create({
    name: "John",
    email: "john@example.com",
  });

  const retrievedUser = await repository.findById(
    createdUser.id
  );

  expect(retrievedUser).toMatchObject({
    name: "John",
    email: "john@example.com",
  });
});
```

The database client and schema in this example are illustrative; adapt them to the database library used by the application.

# 🗁 Test Isolation and Cleanup

Integration tests often interact with shared resources such as databases, files, caches, or external services.

Without proper isolation, tests can influence one another.

Common isolation techniques include:

* Creating a separate test database.
* Using unique test records.
* Cleaning up inserted data after each test.
* Wrapping operations in database transactions and rolling them back.
* Resetting application state between tests.
* Using containers for reproducible dependencies.

Example:

```javascript
import {
  beforeEach,
  afterEach,
  test,
} from "vitest";

beforeEach(async () => {
  await database.user.deleteMany();
});

afterEach(async () => {
  await database.user.deleteMany();
});

test("creates a user", async () => {
  // Test implementation
});
```

Cleanup should be designed carefully. Deleting all records is appropriate only when the test owns an isolated database.

# 🗁 Mocking in Integration Tests

Integration tests generally use real implementations for the components being integrated.

However, external dependencies may still be mocked when they are:

* Expensive to access.
* Unavailable in test environments.
* Unreliable or nondeterministic.
* Outside the integration boundary being tested.

For example, an order service may use a real database while mocking an external payment provider.

```javascript
import {
  vi,
  test,
  expect,
} from "vitest";

import { processOrder } from "./orderService.js";
import { paymentProvider } from "./paymentProvider.js";

vi.mock("./paymentProvider.js", () => ({
  paymentProvider: {
    charge: vi.fn().mockResolvedValue({
      status: "success",
      transactionId: "txn_123",
    }),
  },
}));

test("processes an order using the payment provider", async () => {
  const result = await processOrder({
    amount: 100,
  });

  expect(paymentProvider.charge).toHaveBeenCalledWith({
    amount: 100,
  });

  expect(result.status).toBe("success");
});
```

This verifies the interaction between the order service and its payment dependency, while replacing the external provider.

Avoid mocking the very components whose integration behavior the test is intended to verify.

# 🗁 Integration Testing Best Practices

* Test important interactions between components.
* Prefer real implementations within the integration boundary.
* Use dedicated test databases and isolated resources.
* Keep test data predictable and independent.
* Clean up resources after test execution.
* Test successful operations and failure scenarios.
* Verify HTTP status codes, response bodies, and side effects.
* Avoid depending on external production services.
* Keep integration tests separate from unit tests when useful.
* Use descriptive test names that explain expected behavior.
* Run integration tests in continuous integration pipelines.

# 🗁 Common Integration Testing Mistakes

| Mistake | Better Approach |
| --- | --- |
| Mocking every dependency | Keep the components under test real |
| Sharing production data | Use isolated test environments |
| Leaving test records behind | Clean up test data |
| Depending on test execution order | Make tests independent |
| Testing only successful scenarios | Include failure and boundary cases |
| Using unstable external APIs | Replace external services with controlled test doubles |
| Testing too many unrelated components | Keep each test focused on a meaningful integration boundary |

# 🗁 When to Use Integration Testing

Use integration testing when correctness depends on multiple components working together.

For example:

* A service must save data through a repository.
* An API endpoint must validate input and persist records.
* An authentication system must verify credentials and generate tokens.
* A frontend data layer must correctly interpret API responses.
* A background worker must consume messages and update application state.

Unit tests and integration tests complement each other. Unit tests provide fast feedback about individual behavior, while integration tests provide confidence in component interactions.

# 🗁 Summary

Integration testing verifies that multiple application components cooperate correctly.

Key concepts:

* Integration tests focus on interactions between components.
* They commonly cover APIs, databases, services, and external boundaries.
* Real dependencies should be preferred within the intended integration boundary.
* Test isolation and cleanup are essential.
* External services can be mocked when appropriate.
* Integration tests complement unit tests rather than replacing them.
