# Custom Errors

Custom errors are application-specific error types created by developers to represent particular failure conditions.

JavaScript provides built-in error types such as `Error`, `TypeError`, and `RangeError`. However, real-world applications often require more meaningful error categories.

For example, an application might need to distinguish between:

* Authentication failures.
* Authorization failures.
* Resource not found errors.
* Validation failures.
* Payment processing failures.
* Database connection failures.

Custom errors make these situations easier to identify, handle, log, and debug.

# 🗁 Creating a Custom Error

The simplest way to create a custom error is to extend the built-in `Error` class.

```javascript
class ValidationError extends Error {
  constructor(message) {
    super(message);

    this.name = "ValidationError";
  }
}

throw new ValidationError("Invalid email address");
```

The `extends Error` declaration establishes inheritance from JavaScript's built-in `Error` class.

The `super(message)` call initializes the parent `Error` object with the supplied message.

The `name` property identifies the custom error type.

# 🗁 Handling a Custom Error

Custom errors can be handled using ordinary `try...catch` statements.

```javascript
class ValidationError extends Error {
  constructor(message) {
    super(message);

    this.name = "ValidationError";
  }
}

try {
  throw new ValidationError("Email is required");
} catch (error) {
  console.log(error.name);
  console.log(error.message);
}
```

Output:

```text
ValidationError
Email is required
```

# 🗁 Using the Instanceof Operator

The `instanceof` operator checks whether an error object belongs to a particular class or inherits from it.

```javascript
class ValidationError extends Error {
  constructor(message) {
    super(message);

    this.name = "ValidationError";
  }
}

try {
  throw new ValidationError("Invalid username");
} catch (error) {
  if (error instanceof ValidationError) {
    console.log("Validation failure:", error.message);
  } else {
    console.log("Unexpected error:", error.message);
  }
}
```

This approach is useful when an application needs different handling strategies for different error types.

# 🗁 Creating Multiple Custom Error Types

An application can define multiple custom error classes for different failure conditions.

```javascript
class ValidationError extends Error {
  constructor(message) {
    super(message);
    this.name = "ValidationError";
  }
}

class AuthenticationError extends Error {
  constructor(message) {
    super(message);
    this.name = "AuthenticationError";
  }
}

class NotFoundError extends Error {
  constructor(message) {
    super(message);
    this.name = "NotFoundError";
  }
}
```

Each class represents a distinct category of application failure.

Example:

```javascript
function authenticateUser(username, password) {
  if (!username || !password) {
    throw new ValidationError("Username and password are required");
  }

  if (username !== "admin" || password !== "secret") {
    throw new AuthenticationError("Invalid credentials");
  }

  return {
    username,
    authenticated: true
  };
}

try {
  const user = authenticateUser("john", "123");

  console.log(user);
} catch (error) {
  if (error instanceof ValidationError) {
    console.log("Validation:", error.message);
  } else if (error instanceof AuthenticationError) {
    console.log("Authentication:", error.message);
  } else {
    console.log("Unexpected error:", error.message);
  }
}
```

# 🗁 Adding Custom Properties

Custom errors can contain additional properties beyond the standard `Error` properties.

For example, an HTTP-related error might include a status code and an application error code.

```javascript
class HttpError extends Error {
  constructor(message, statusCode, code) {
    super(message);

    this.name = "HttpError";
    this.statusCode = statusCode;
    this.code = code;
  }
}

try {
  throw new HttpError(
    "User not found",
    404,
    "USER_NOT_FOUND"
  );
} catch (error) {
  console.log(error.name);
  console.log(error.message);
  console.log(error.statusCode);
  console.log(error.code);
}
```

Output:

```text
HttpError
User not found
404
USER_NOT_FOUND
```

Additional properties allow application code to make decisions without relying on error-message text.

# 🗁 Custom Error with Error Cause

Sometimes a custom error represents a higher-level failure caused by another error.

The `cause` option allows the original error to be preserved.

```javascript
class DatabaseError extends Error {
  constructor(message, options) {
    super(message, options);

    this.name = "DatabaseError";
  }
}

try {
  try {
    throw new Error("Connection timeout");
  } catch (error) {
    throw new DatabaseError("Unable to retrieve users", {
      cause: error
    });
  }
} catch (error) {
  console.log(error.name);
  console.log(error.message);
  console.log(error.cause.message);
}
```

Output:

```text
DatabaseError
Unable to retrieve users
Connection timeout
```

The `cause` property preserves the underlying error, making debugging easier.

# 🗁 Building a Custom Error Hierarchy

Larger applications can organize custom errors into a hierarchy.

A common approach is to create a base application error and derive more specific errors from it.

```javascript
class AppError extends Error {
  constructor(message, options = {}) {
    super(message, options);

    this.name = new.target.name;
    this.code = options.code ?? "APPLICATION_ERROR";
    this.statusCode = options.statusCode ?? 500;
  }
}

class ValidationError extends AppError {
  constructor(message) {
    super(message, {
      code: "VALIDATION_ERROR",
      statusCode: 400
    });
  }
}

class AuthenticationError extends AppError {
  constructor(message) {
    super(message, {
      code: "AUTHENTICATION_ERROR",
      statusCode: 401
    });
  }
}

class NotFoundError extends AppError {
  constructor(message) {
    super(message, {
      code: "NOT_FOUND",
      statusCode: 404
    });
  }
}
```

Usage:

```javascript
try {
  throw new NotFoundError("User does not exist");
} catch (error) {
  if (error instanceof AppError) {
    console.log({
      name: error.name,
      message: error.message,
      code: error.code,
      statusCode: error.statusCode
    });
  }
}
```

Output:

```javascript
{
  name: "NotFoundError",
  message: "User does not exist",
  code: "NOT_FOUND",
  statusCode: 404
}
```

The `new.target.name` expression identifies the constructor used to create the instance, including when the instance belongs to a subclass.

# 🗁 Custom Errors in an API

Custom errors are especially useful in backend applications because they allow services and controllers to communicate failures consistently.

Example application error classes:

```javascript
class AppError extends Error {
  constructor(message, statusCode, code) {
    super(message);

    this.name = new.target.name;
    this.statusCode = statusCode;
    this.code = code;
  }
}

class NotFoundError extends AppError {
  constructor(message) {
    super(message, 404, "NOT_FOUND");
  }
}

class ValidationError extends AppError {
  constructor(message) {
    super(message, 400, "VALIDATION_ERROR");
  }
}
```

A service function:

```javascript
const users = [
  {
    id: 1,
    name: "John"
  },
  {
    id: 2,
    name: "Sarah"
  }
];

function findUserById(id) {
  if (!Number.isInteger(id) || id <= 0) {
    throw new ValidationError("Invalid user ID");
  }

  const user = users.find(user => user.id === id);

  if (!user) {
    throw new NotFoundError("User not found");
  }

  return user;
}
```

A centralized error handler can translate these errors into HTTP responses.

```javascript
function handleError(error) {
  if (error instanceof AppError) {
    return {
      status: error.statusCode,
      body: {
        success: false,
        code: error.code,
        message: error.message
      }
    };
  }

  return {
    status: 500,
    body: {
      success: false,
      code: "INTERNAL_SERVER_ERROR",
      message: "An unexpected error occurred"
    }
  };
}
```

Usage:

```javascript
try {
  const user = findUserById(10);

  console.log(user);
} catch (error) {
  const response = handleError(error);

  console.log(response);
}
```

This separates business logic from error-response formatting.

# 🗁 Custom Errors in Node.js

In Node.js applications, custom errors can be organized into dedicated modules.

Example project structure:

```text
src/
├── errors/
│   ├── app-error.js
│   ├── validation-error.js
│   ├── authentication-error.js
│   └── not-found-error.js
├── services/
│   └── user-service.js
└── index.js
```

Example `app-error.js`:

```javascript
export class AppError extends Error {
  constructor(message, options = {}) {
    super(message, options);

    this.name = new.target.name;
    this.statusCode = options.statusCode ?? 500;
    this.code = options.code ?? "APPLICATION_ERROR";
  }
}
```

Example `validation-error.js`:

```javascript
import { AppError } from "./app-error.js";

export class ValidationError extends AppError {
  constructor(message) {
    super(message, {
      statusCode: 400,
      code: "VALIDATION_ERROR"
    });
  }
}
```

Example `not-found-error.js`:

```javascript
import { AppError } from "./app-error.js";

export class NotFoundError extends AppError {
  constructor(message) {
    super(message, {
      statusCode: 404,
      code: "NOT_FOUND"
    });
  }
}
```

This modular structure improves maintainability as the application grows.

# 🗁 Custom Errors vs Built-in Errors

| Feature | Built-in Errors | Custom Errors |
| --- | --- | --- |
| Origin | Provided by JavaScript | Defined by developers |
| Purpose | Represent general language and runtime failures | Represent application-specific failures |
| Examples | `TypeError`, `RangeError`, `SyntaxError` | `ValidationError`, `NotFoundError` |
| Properties | Standard error properties | Standard and application-specific properties |
| Inheritance | Built into JavaScript | Usually extends `Error` or another custom error |

# 🗁 Common Mistakes

**Forgetting to call `super()`**

```javascript
class CustomError extends Error {
  constructor(message) {
    this.name = "CustomError";
  }
}
```

A derived class constructor must call `super()` before accessing `this`.

Correct:

```javascript
class CustomError extends Error {
  constructor(message) {
    super(message);

    this.name = "CustomError";
  }
}
```

**Not setting a meaningful error name**

```javascript
class ValidationError extends Error {
  constructor(message) {
    super(message);
  }
}
```

The instance's default name is `Error`.

Prefer:

```javascript
class ValidationError extends Error {
  constructor(message) {
    super(message);

    this.name = "ValidationError";
  }
}
```

**Checking error messages instead of error types**

```javascript
if (error.message === "User not found") {
  // Handle error
}
```

Message text can change and is not a reliable error identifier.

Prefer:

```javascript
if (error instanceof NotFoundError) {
  // Handle error
}
```

**Creating unnecessary custom error classes**

Not every failure requires a separate class. Use custom types when they provide meaningful classification or handling behavior.

**Discarding the original cause**

When wrapping an existing error, preserve it with the `cause` option.

# 🗁 Best Practices

* Extend the built-in `Error` class when creating custom error types.
* Always call `super(message)` in derived error constructors.
* Use descriptive class names.
* Set application-specific properties such as `code` and `statusCode` when useful.
* Use `instanceof` for reliable error classification within the same JavaScript realm.
* Preserve underlying errors through `cause`.
* Create a shared base error class for larger applications.
* Keep custom errors focused on meaningful application failure categories.
* Centralize error-response formatting in backend applications.
* Avoid exposing stack traces or internal implementation details to clients.

# 🗁 Key Takeaways

* Custom errors represent application-specific failure conditions.
* Extend `Error` to create custom error classes.
* Use `super(message)` to initialize the parent error.
* Use `instanceof` to identify custom error types.
* Custom errors can contain additional properties such as error codes and HTTP status codes.
* The `cause` option preserves the original error when wrapping another exception.
* Error hierarchies help organize complex application error handling.
* Centralized error handling separates application logic from response formatting.
