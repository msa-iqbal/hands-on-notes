# JavaScript Default Export

A default export is a JavaScript module feature that allows a module to expose one primary value for other modules to import.

It is created using the `export default` syntax.

Default exports are commonly used when a module represents one main responsibility, such as a class, function, configuration object, or service.

Unlike named exports, default exports do not require the importing module to use the original declaration name.

# 🗁 What Is a Default Export?

A default export makes one primary value available from a module.

**Example**

`greeting.js`

```javascript
export default function greet(name) {
    return `Hello, ${name}!`;
}
```

`app.js`

```javascript
import greet from "./greeting.js";

console.log(greet("Muhammad"));
```

**Output**

```text
Hello, Muhammad!
```

The `greet` function is the default export of `greeting.js`.

# 🗁 Default Export Syntax

The general syntax is:

```javascript
export default expression;
```

A function or class declaration can also be exported directly:

```javascript
export default function functionName() {
    // Function implementation
}
```

```javascript
export default class ClassName {
    // Class implementation
}
```

A default export can represent:

* A function.
* A class.
* An object.
* An array.
* A primitive value.
* An expression.
* An existing variable.

# 🗁 Default Function Export

Functions are commonly exported as the primary functionality of a module.

**Named function declaration**

```javascript
// greeting.js

export default function greet(name) {
    return `Hello, ${name}!`;
}
```

Importing:

```javascript
import greet from "./greeting.js";

console.log(greet("Alex"));
```

**Anonymous function declaration**

```javascript
// greeting.js

export default function (name) {
    return `Hello, ${name}!`;
}
```

Importing:

```javascript
import greet from "./greeting.js";

console.log(greet("Alex"));
```

The importing module chooses the local name `greet`.

# 🗁 Default Arrow Function Export

An arrow function can be exported as the default value.

```javascript
// calculator.js

const calculateTotal = (price, quantity) => {
    return price * quantity;
};

export default calculateTotal;
```

Importing:

```javascript
import calculateTotal from "./calculator.js";

console.log(calculateTotal(100, 3));
```

**Output**

```text
300
```

An arrow function expression can also be exported directly:

```javascript
export default (price, quantity) => {
    return price * quantity;
};
```

# 🗁 Default Class Export

A class can be exported as the default value.

```javascript
// User.js

export default class User {
    constructor(name, email) {
        this.name = name;
        this.email = email;
    }

    getProfile() {
        return {
            name: this.name,
            email: this.email
        };
    }
}
```

Importing:

```javascript
import User from "./User.js";

const user = new User(
    "Muhammad",
    "user@example.com"
);

console.log(user.getProfile());
```

A default class export is useful when a module primarily represents one class.

# 🗁 Default Anonymous Class Export

A class does not need a declared name when exported as the default value.

```javascript
// User.js

export default class {
    constructor(name) {
        this.name = name;
    }

    greet() {
        return `Hello, ${this.name}!`;
    }
}
```

Importing:

```javascript
import User from "./User.js";

const user = new User("Alex");

console.log(user.greet());
```

The importing module supplies the local identifier `User`.

# 🗁 Default Object Export

An object can be exported as the default value.

```javascript
// config.js

const config = {
    appName: "My Application",
    version: "1.0.0",
    environment: "development",
    debug: true
};

export default config;
```

Importing:

```javascript
import config from "./config.js";

console.log(config.appName);
console.log(config.version);
console.log(config.environment);
```

This pattern is useful for application configuration and grouped settings.

An object can also be exported directly:

```javascript
export default {
    appName: "My Application",
    version: "1.0.0",
    debug: true
};
```

# 🗁 Default Array Export

An array can also be the default export.

```javascript
// roles.js

export default [
    "Admin",
    "Editor",
    "Developer",
    "Guest"
];
```

Importing:

```javascript
import roles from "./roles.js";

console.log(roles);
```

# 🗁 Default Primitive Value Export

Primitive values can be exported as the default value.

```javascript
// appName.js

export default "My Application";
```

Importing:

```javascript
import appName from "./appName.js";

console.log(appName);
```

Other primitive values can also be exported:

```javascript
export default 100;
```

```javascript
export default true;
```

```javascript
export default null;
```

# 🗁 Default Expression Export

A default export can contain an expression.

```javascript
const basePrice = 100;
const taxRate = 0.15;

export default basePrice + basePrice * taxRate;
```

Importing:

```javascript
import totalPrice from "./price.js";

console.log(totalPrice);
```

**Output**

```text
115
```

The expression is evaluated when the module is evaluated.

# 🗁 Default Export with an Existing Declaration

An existing declaration can be exported using `export default` followed by its identifier.

```javascript
// greeting.js

function greet(name) {
    return `Hello, ${name}!`;
}

export default greet;
```

This is useful when a function or class is defined separately from its export.

# 🗁 Default Export Using an Export List

A declaration can be exported as the default value using an export list.

```javascript
// greeting.js

function greet(name) {
    return `Hello, ${name}!`;
}

export {
    greet as default
};
```

This is equivalent to:

```javascript
export default greet;
```

It can be useful when organizing multiple exports at the end of a module.

# 🗁 Importing a Default Export

A default export is imported without curly braces.

**Default export**

```javascript
// math.js

export default function add(a, b) {
    return a + b;
}
```

**Default import**

```javascript
import add from "./math.js";

console.log(add(10, 20));
```

The local identifier does not need to match the original declaration name.

```javascript
import sum from "./math.js";
```

Both imports reference the same default export.

# 🗁 Renaming Default Imports

A default import can use any valid local identifier.

```javascript
// user.js

export default class User {
    constructor(name) {
        this.name = name;
    }
}
```

Different importing modules can choose different names:

```javascript
import User from "./user.js";
```

```javascript
import ApplicationUser from "./user.js";
```

```javascript
import Account from "./user.js";
```

These identifiers all refer to the same default export.

The module's original implementation does not change.

# 🗁 Combining Default and Named Exports

A module can contain one default export and multiple named exports.

```javascript
// user.js

export const ROLE = "Developer";

export const STATUS = "Active";

export function validateUser(user) {
    return Boolean(user?.name);
}

export default class User {
    constructor(name) {
        this.name = name;
    }
}
```

Importing both types:

```javascript
import User, {
    ROLE,
    STATUS,
    validateUser
} from "./user.js";

const user = new User("Muhammad");

console.log(ROLE);
console.log(STATUS);
console.log(validateUser(user));
```

The default import appears before the named imports.

# 🗁 Re-Exporting a Default Export

A module can re-export the default export of another module.

**Example structure**

```text
project/
├── app.js
├── User.js
└── index.js
```

`User.js`

```javascript
export default class User {
    constructor(name) {
        this.name = name;
    }
}
```

`index.js`

```javascript
export { default } from "./User.js";
```

`app.js`

```javascript
import User from "./index.js";

const user = new User("Muhammad");

console.log(user.name);
```

This allows a centralized module to expose default exports from other modules.

# 🗁 Re-Exporting a Default Export as a Named Export

A default export can be exposed under a named export.

```javascript
// index.js

export { default as User } from "./User.js";
```

Importing:

```javascript
import { User } from "./index.js";

const user = new User("Alex");
```

This is particularly useful when creating a package entry point that exposes multiple classes and utilities.

# 🗁 Default Export vs Named Export

Both export types make module functionality available, but they differ in syntax and usage.

| Feature | Default Export | Named Export |
| --- | --- | --- |
| Number per module | One | Multiple |
| Export syntax | `export default` | `export` |
| Import syntax | `import User from "./user.js"` | `import { User } from "./user.js"` |
| Curly braces | Not required | Required |
| Import naming | Can choose any local name | Must match or use an alias |
| Typical use | Primary module functionality | Multiple related declarations |

**Default export example**

```javascript
export default function greet() {}
```

```javascript
import greet from "./greeting.js";
```

**Named export example**

```javascript
export function greet() {}
```

```javascript
import { greet } from "./greeting.js";
```

# 🗁 Default Export Rules

Important rules to remember:

* A module can have only one default export.
* A module can have multiple named exports.
* Default imports do not require curly braces.
* The importing module can choose any local identifier for a default export.
* A default export can be a function, class, object, array, primitive, or expression.
* Default exports can coexist with named exports.
* Default exports can be re-exported from another module.
* A default export can be exposed as a named export using `as`.

**Invalid: multiple default exports**

```javascript
export default function greet() {}

export default class User {}
```

**Valid: one default and multiple named exports**

```javascript
export default class User {}

export function validateUser() {}

export const ROLE = "Developer";
```

# 🗁 Common Mistakes

**Using curly braces when importing a default export**

Incorrect:

```javascript
import { User } from "./user.js";
```

Correct:

```javascript
import User from "./user.js";
```

**Using a default import for a named export**

Incorrect:

```javascript
// math.js
export function add(a, b) {
    return a + b;
}
```

```javascript
import add from "./math.js";
```

Correct:

```javascript
import { add } from "./math.js";
```

**Declaring multiple default exports**

Incorrect:

```javascript
export default function greet() {}

export default function welcome() {}
```

Use one default export and named exports for additional functionality.

**Confusing default export aliases with named exports**

```javascript
export { greet as default };
```

This exports `greet` as the default export.

It does not create a named export called `default` that must be imported with curly braces.

**Assuming the local import name changes the original export**

```javascript
import Greeting from "./greeting.js";
```

`Greeting` is only a local identifier. The original module remains unchanged.

# 🗁 Best Practices

* Use default exports when a module has one primary responsibility.
* Use descriptive names for default-imported values.
* Prefer named exports when a module provides several equally important utilities.
* Avoid creating unnecessary default exports just to simplify import syntax.
* Keep module responsibilities clear.
* Combine default and named exports only when the module's public interface benefits from both.
* Use centralized entry points to organize exports when appropriate.
* Avoid exporting unnecessary internal implementation details.
* Maintain consistent export conventions across a project.

# 🗁 Quick Reference

| Operation | Syntax |
| --- | --- |
| Default function | `export default function greet() {}` |
| Default class | `export default class User {}` |
| Default object | `export default { name: "App" }` |
| Default value | `export default "JavaScript"` |
| Existing declaration | `export default greet` |
| Anonymous function | `export default function () {}` |
| Anonymous class | `export default class {}` |
| Default import | `import User from "./user.js"` |
| Re-export default | `export { default } from "./user.js"` |
| Default as named | `export { default as User } from "./user.js"` |
| Named export as default | `export { greet as default }` |

# 🗁 Practice Exercises

* Create a module with a default-exported greeting function.
* Create a module with a default-exported class.
* Export an anonymous function as the default value.
* Export an object containing application configuration.
* Export an array containing user roles.
* Export a primitive value as the default export.
* Import the same default export using different local identifiers.
* Combine one default export with multiple named exports.
* Re-export a default class through an `index.js` file.
* Re-export a default export under a named identifier.
* Build a small application using default exports for services and classes.
* Compare default exports with named exports in a practical project.

# 🗁 Key Takeaways

* Default exports expose one primary value from a JavaScript module.
* The `export default` syntax creates a default export.
* Functions, classes, objects, arrays, primitive values, and expressions can be default-exported.
* A module can have only one default export.
* Default imports do not require curly braces.
* Importing modules can choose their own local identifier.
* Default exports can coexist with named exports.
* Default exports can be re-exported directly or under named identifiers.
* Default exports are useful when a module has one clearly defined primary responsibility.
