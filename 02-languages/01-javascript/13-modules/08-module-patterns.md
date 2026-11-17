# JavaScript Module Patterns

JavaScript module patterns are architectural techniques used to organize code into reusable, maintainable, and independent units.

Modules help developers separate application responsibilities, control access to internal implementation details, and manage dependencies.

JavaScript supports several module-related patterns, including:

* ES Module Pattern
* CommonJS Pattern
* Module Pattern
* Revealing Module Pattern
* Singleton Module Pattern
* Factory Module Pattern
* Barrel Export Pattern
* Dependency Injection Pattern
* Dynamic Module Loading Pattern

Understanding these patterns helps developers design scalable JavaScript applications.

# 🗁 What Is a Module Pattern?

A module pattern organizes related functionality into a logical unit.

A module can contain:

* Variables.
* Functions.
* Classes.
* Objects.
* Constants.
* Internal implementation details.
* Public APIs.

A well-designed module exposes only the functionality that other parts of an application need.

**Example**

```javascript
// calculator.js

const TAX_RATE = 0.15;

function calculateTax(amount) {
    return amount * TAX_RATE;
}

function calculateTotal(amount) {
    return amount + calculateTax(amount);
}

export {
    calculateTax,
    calculateTotal
};
```

Importing:

```javascript
import { calculateTotal } from "./calculator.js";

console.log(calculateTotal(100));
```

The module keeps `TAX_RATE` private while exposing the required functions.

# 🗁 Why Use Module Patterns?

Module patterns help solve common application architecture problems.

**Encapsulation**

Keep internal implementation details separate from public functionality.

**Reusability**

Reuse functionality across multiple parts of an application.

**Maintainability**

Organize related functionality into focused modules.

**Dependency management**

Make relationships between application components explicit.

**Namespace management**

Avoid unnecessary global variables and naming conflicts.

**Testability**

Separate responsibilities so individual modules can be tested independently.

# 🗁 The Module Pattern Using Closures

Before native ES Modules became widely available, JavaScript developers commonly used Immediately Invoked Function Expressions (IIFEs) and closures to create private module scopes.

This technique is known as the Module Pattern.

```javascript
const calculator = (function () {
    let result = 0;

    function add(value) {
        result += value;
    }

    function subtract(value) {
        result -= value;
    }

    function getResult() {
        return result;
    }

    return {
        add,
        subtract,
        getResult
    };
})();

calculator.add(10);
calculator.add(5);
calculator.subtract(3);

console.log(calculator.getResult());
```

**Output**

```text
12
```

The IIFE executes immediately and returns an object containing the public methods.

The `result` variable remains private because it is accessible only through the closure.

**Important characteristics**

* Creates a private scope.
* Encapsulates internal state.
* Exposes a controlled public API.
* Uses closures to preserve access to private variables.
* Avoids creating unnecessary global variables.

# 🗁 Private and Public Members

The Module Pattern distinguishes between private implementation details and public functionality.

```javascript
const userModule = (function () {
    let users = [];

    function validateUser(name) {
        return typeof name === "string" && name.trim() !== "";
    }

    function addUser(name) {
        if (!validateUser(name)) {
            throw new Error("Invalid username.");
        }

        users.push(name);
    }

    function getUsers() {
        return [...users];
    }

    return {
        addUser,
        getUsers
    };
})();

userModule.addUser("Muhammad");

console.log(userModule.getUsers());
```

**Output**

```text
["Muhammad"]
```

The following members are private:

* `users`
* `validateUser`

The following members are public:

* `addUser`
* `getUsers`

The private implementation cannot be accessed directly through `userModule`.

Returning a copy of the internal array also prevents callers from directly modifying the module's stored array.

# 🗁 Revealing Module Pattern

The Revealing Module Pattern is a variation of the Module Pattern.

Instead of defining public methods directly in the returned object, it defines functions internally and explicitly reveals selected members.

```javascript
const counterModule = (function () {
    let count = 0;

    function increment() {
        count++;
    }

    function decrement() {
        count--;
    }

    function getCount() {
        return count;
    }

    return {
        increment: increment,
        decrement: decrement,
        getCount: getCount
    };
})();
```

Usage:

```javascript
counterModule.increment();
counterModule.increment();

console.log(counterModule.getCount());
```

**Output**

```text
2
```

The returned object explicitly defines the module's public API.

A shorter object property syntax can also be used:

```javascript
return {
    increment,
    decrement,
    getCount
};
```

**Advantages**

* Clear public API.
* Private implementation details.
* Centralized export definition.
* Easy-to-understand encapsulation.

# 🗁 ES Module Pattern

ECMAScript Modules provide a standardized module system built into JavaScript.

They use `export` and `import` to share functionality between files.

`calculator.js`

```javascript
const TAX_RATE = 0.15;

export function calculateTax(amount) {
    return amount * TAX_RATE;
}

export function calculateTotal(amount) {
    return amount + calculateTax(amount);
}
```

`app.js`

```javascript
import {
    calculateTax,
    calculateTotal
} from "./calculator.js";

console.log(calculateTax(100));
console.log(calculateTotal(100));
```

ES Modules provide several important capabilities:

* File-based module organization.
* Named exports.
* Default exports.
* Static dependency analysis.
* Module-level scope.
* Strict mode by default.
* Dynamic imports.
* Live bindings.

For modern JavaScript applications, ES Modules are generally the preferred module system.

# 🗁 CommonJS Module Pattern

CommonJS is commonly used in Node.js applications.

It uses `require()` and `module.exports`.

`math.js`

```javascript
function add(a, b) {
    return a + b;
}

function subtract(a, b) {
    return a - b;
}

module.exports = {
    add,
    subtract
};
```

`app.js`

```javascript
const { add, subtract } = require("./math");

console.log(add(10, 5));
console.log(subtract(10, 5));
```

CommonJS is particularly relevant when maintaining existing Node.js projects and packages.

# 🗁 Singleton Module Pattern

The Singleton Module Pattern ensures that an application uses one shared instance of a particular module or service.

JavaScript module caching makes it possible to implement singleton-like module behavior.

`database.js`

```javascript
class Database {
    constructor() {
        this.connected = false;
    }

    connect() {
        this.connected = true;

        console.log("Database connected.");
    }

    getStatus() {
        return this.connected;
    }
}

const database = new Database();

export default database;
```

`app.js`

```javascript
import database from "./database.js";

database.connect();

console.log(database.getStatus());
```

Another module importing the same resolved module receives the same exported instance.

**Advantages**

* Shared application state.
* Centralized service management.
* Avoids unnecessary repeated initialization.
* Useful for configuration, logging, and connection managers.

**Considerations**

* Shared mutable state can create unexpected dependencies.
* Testing may require resetting or isolating module state.
* Excessive singleton usage can make applications difficult to maintain.

A module that exports a singleton instance should be used deliberately rather than making every service global.

# 🗁 Factory Module Pattern

The Factory Pattern creates and returns objects through a dedicated function.

It is useful when an application needs multiple independent instances of the same type.

```javascript
// userFactory.js

export function createUser(name, email) {
    return {
        name,
        email,

        getProfile() {
            return {
                name: this.name,
                email: this.email
            };
        }
    };
}
```

Usage:

```javascript
import { createUser } from "./userFactory.js";

const user1 = createUser(
    "Muhammad",
    "muhammad@example.com"
);

const user2 = createUser(
    "Alex",
    "alex@example.com"
);

console.log(user1.getProfile());
console.log(user2.getProfile());
```

Each factory call creates a separate object.

**Advantages**

* Encapsulates object creation.
* Supports multiple instances.
* Makes object initialization consistent.
* Allows construction logic to evolve independently.

# 🗁 Service Module Pattern

A service module groups related application operations into a reusable unit.

It is frequently used in frontend and backend applications.

`userService.js`

```javascript
const API_URL = "/api/users";

export async function getUsers() {
    const response = await fetch(API_URL);

    if (!response.ok) {
        throw new Error("Failed to fetch users.");
    }

    return response.json();
}

export async function getUserById(id) {
    const response = await fetch(`${API_URL}/${id}`);

    if (!response.ok) {
        throw new Error("Failed to fetch user.");
    }

    return response.json();
}

export async function createUser(userData) {
    const response = await fetch(API_URL, {
        method: "POST",
        headers: {
            "Content-Type": "application/json"
        },
        body: JSON.stringify(userData)
    });

    if (!response.ok) {
        throw new Error("Failed to create user.");
    }

    return response.json();
}
```

Usage:

```javascript
import {
    getUsers,
    getUserById,
    createUser
} from "./userService.js";

const users = await getUsers();

const user = await getUserById(1);

console.log(users, user);
```

Service modules are useful for keeping API communication separate from UI components and business logic.

# 🗁 Utility Module Pattern

A utility module contains reusable functions that can be shared across different application areas.

`stringUtils.js`

```javascript
export function capitalize(value) {
    if (typeof value !== "string") {
        throw new TypeError("Expected a string.");
    }

    return value.charAt(0).toUpperCase() + value.slice(1);
}

export function reverse(value) {
    if (typeof value !== "string") {
        throw new TypeError("Expected a string.");
    }

    return [...value].reverse().join("");
}

export function isEmpty(value) {
    return typeof value !== "string" || value.trim() === "";
}
```

Usage:

```javascript
import {
    capitalize,
    reverse,
    isEmpty
} from "./stringUtils.js";

console.log(capitalize("javascript"));
console.log(reverse("hello"));
console.log(isEmpty("   "));
```

Utility modules should contain focused, reusable operations rather than unrelated application responsibilities.

# 🗁 Barrel Export Pattern

A barrel file is an entry-point module that re-exports functionality from multiple modules.

It is commonly named `index.js`.

Example project structure:

```text
src/
├── users/
│   ├── User.js
│   ├── userService.js
│   └── index.js
└── app.js
```

`User.js`

```javascript
export default class User {
    constructor(name) {
        this.name = name;
    }
}
```

`userService.js`

```javascript
export function getUsers() {
    return ["Muhammad", "Alex"];
}
```

`index.js`

```javascript
export { default as User } from "./User.js";

export { getUsers } from "./userService.js";
```

`app.js`

```javascript
import {
    User,
    getUsers
} from "./users/index.js";

const user = new User("Muhammad");

console.log(user.name);
console.log(getUsers());
```

The barrel file provides a centralized import location.

**Advantages**

* Simplifies import paths.
* Creates a convenient public API.
* Groups related exports.
* Helps organize larger module directories.

**Considerations**

* Large barrel files can make dependency relationships less obvious.
* Circular dependencies can become harder to detect.
* Some bundler configurations may have difficulty eliminating unused code when modules contain side effects.

Use barrel files where they improve organization without hiding important dependency relationships.

# 🗁 Dependency Injection Pattern

Dependency Injection (DI) means providing a module with the dependencies it needs rather than making it create or locate those dependencies internally.

This improves modularity and testability.

**Without dependency injection**

```javascript
class UserService {
    constructor() {
        this.logger = console;
    }

    createUser(name) {
        this.logger.log(`User created: ${name}`);
    }
}
```

The service directly depends on the global console object.

**With dependency injection**

```javascript
class UserService {
    constructor(logger) {
        this.logger = logger;
    }

    createUser(name) {
        this.logger.log(`User created: ${name}`);
    }
}
```

Usage:

```javascript
const logger = {
    log(message) {
        console.log(message);
    }
};

const userService = new UserService(logger);

userService.createUser("Muhammad");
```

A different logger can be supplied during testing:

```javascript
const testLogger = {
    messages: [],

    log(message) {
        this.messages.push(message);
    }
};

const userService = new UserService(testLogger);

userService.createUser("Alex");

console.log(testLogger.messages);
```

Dependency injection helps reduce tight coupling between modules.

# 🗁 Dynamic Module Loading Pattern

Dynamic module loading allows functionality to be loaded only when required.

It uses the `import()` expression.

```javascript
async function loadReports() {
    const reports = await import("./reports.js");

    reports.initializeReports();
}
```

This pattern is useful for:

* Lazy-loaded routes.
* Optional features.
* Large application components.
* Conditional functionality.
* Code splitting.

A user does not need to download every optional feature during initial application startup.

# 🗁 Module Composition Pattern

Module composition means combining smaller modules to create larger application functionality.

Each module handles a focused responsibility.

Example structure:

```text
src/
├── services/
│   ├── userService.js
│   └── emailService.js
├── utils/
│   └── validator.js
├── controllers/
│   └── userController.js
└── app.js
```

`userService.js`

```javascript
export function createUser(name) {
    return {
        id: crypto.randomUUID(),
        name
    };
}
```

`emailService.js`

```javascript
export function sendWelcomeEmail(user) {
    console.log(`Welcome email sent to ${user.name}`);
}
```

`validator.js`

```javascript
export function isValidName(name) {
    return typeof name === "string" && name.trim().length > 0;
}
```

`userController.js`

```javascript
import { createUser } from "../services/userService.js";
import { sendWelcomeEmail } from "../services/emailService.js";
import { isValidName } from "../utils/validator.js";

export function registerUser(name) {
    if (!isValidName(name)) {
        throw new Error("Invalid username.");
    }

    const user = createUser(name);

    sendWelcomeEmail(user);

    return user;
}
```

`app.js`

```javascript
import { registerUser } from "./controllers/userController.js";

const user = registerUser("Muhammad");

console.log(user);
```

This architecture separates validation, user creation, email operations, and application orchestration.

# 🗁 Module Scope and Encapsulation

Each ES Module has its own scope.

Variables declared inside a module are not automatically available globally.

`config.js`

```javascript
const API_KEY = "example-key";

export function getApiKey() {
    return API_KEY;
}
```

`app.js`

```javascript
import { getApiKey } from "./config.js";

console.log(getApiKey());
```

The importing module cannot directly access `API_KEY` because it is not exported.

This is an important distinction between module scope and global scope.

# 🗁 Circular Dependencies

A circular dependency occurs when two or more modules depend on each other.

Example:

`moduleA.js`

```javascript
import { functionB } from "./moduleB.js";

export function functionA() {
    console.log("Function A");

    functionB();
}
```

`moduleB.js`

```javascript
import { functionA } from "./moduleA.js";

export function functionB() {
    console.log("Function B");
}
```

Both modules depend on each other.

ES Modules support cyclic dependencies, but accessing an imported binding before its initialization can cause a `ReferenceError`.

CommonJS also supports circular dependencies, but a module involved in a cycle may receive another module's partially initialized exports.

**Ways to reduce circular dependencies**

* Separate shared functionality into a third module.
* Reconsider module responsibilities.
* Avoid unnecessary bidirectional dependencies.
* Use dependency injection where appropriate.
* Keep the dependency graph understandable.

# 🗁 Choosing the Right Module Pattern

Different patterns solve different architectural problems.

| Pattern | Main Purpose | Common Use |
| --- | --- | --- |
| Module Pattern | Encapsulation | Legacy JavaScript |
| Revealing Module | Explicit public API | Closure-based modules |
| ES Module | Standardized modules | Modern JavaScript |
| CommonJS | Node.js module organization | Existing Node.js applications |
| Singleton Module | Shared instance | Configuration and logging |
| Factory Module | Object creation | Services and domain objects |
| Service Module | Group related operations | API communication |
| Utility Module | Reusable helpers | Shared functions |
| Barrel Export | Centralized exports | Package and directory entry points |
| Dependency Injection | Loose coupling | Testable application architecture |
| Dynamic Loading | On-demand loading | Lazy-loaded features |
| Module Composition | Combine focused modules | Larger applications |

# 🗁 Best Practices

* Prefer native ES Modules for modern JavaScript applications.
* Keep each module focused on a clear responsibility.
* Expose a small and intentional public API.
* Keep internal implementation details private where possible.
* Avoid excessive shared mutable state.
* Use dependency injection to reduce tight coupling.
* Organize services, utilities, and domain logic into separate modules.
* Use barrel files only when they improve import organization.
* Avoid unnecessary circular dependencies.
* Use dynamic imports for genuinely optional or expensive features.
* Keep module names and directory structures predictable.
* Design modules so they can be tested independently.
* Avoid turning every function into a separate file without a meaningful architectural reason.

# 🗁 Quick Reference

| Concept | Key Idea |
| --- | --- |
| Module Pattern | Uses closures to create private state and public methods |
| Revealing Module | Explicitly returns selected internal functions |
| ES Module | Uses `import` and `export` |
| CommonJS | Uses `require()` and `module.exports` |
| Singleton | Shares one module instance |
| Factory | Creates independent objects |
| Service Module | Groups related application operations |
| Utility Module | Provides reusable helper functions |
| Barrel Export | Re-exports functionality through an entry point |
| Dependency Injection | Supplies dependencies from outside |
| Dynamic Loading | Loads modules on demand |
| Module Composition | Combines focused modules into larger functionality |

# 🗁 Practice Exercises

* Implement a closure-based Module Pattern for a shopping cart.
* Create a Revealing Module with private validation functions.
* Build a utility module containing reusable string functions.
* Create a service module that manages user operations.
* Implement a singleton logger module.
* Build a factory module that creates different user objects.
* Create a barrel file that re-exports several modules.
* Refactor a tightly coupled service using dependency injection.
* Implement lazy loading for a dashboard feature.
* Identify and eliminate a circular dependency in a small project.
* Organize a small application using module composition.
* Compare closure-based modules with native ES Modules.

# 🗁 Key Takeaways

* Module patterns provide architectural approaches for organizing JavaScript code.
* The Module Pattern uses closures to encapsulate private state.
* The Revealing Module Pattern explicitly exposes selected functionality.
* ES Modules provide standardized file-based modularity.
* CommonJS is widely used in the Node.js ecosystem.
* Singleton modules provide shared instances.
* Factory modules create independent objects.
* Service and utility modules separate application responsibilities.
* Barrel files centralize exports.
* Dependency injection reduces coupling and improves testability.
* Dynamic imports support lazy loading and code splitting.
* Good module design improves maintainability, reusability, and scalability.
