# TypeScript Comments

Comments are explanatory notes written inside source code. They help developers understand the purpose, behavior, and structure of code without affecting program execution.

TypeScript supports JavaScript comment syntax. Comments are ignored by the TypeScript compiler during normal compilation and do not become executable JavaScript instructions.

Comments are useful for:

* Explaining complex logic.
* Documenting functions, classes, and interfaces.
* Providing information about parameters and return values.
* Marking unfinished work or potential improvements.
* Generating API documentation with tools such as TypeDoc.

# 🗁 Single-Line Comments

Single-line comments begin with two forward slashes (`//`).

Everything after `//` on that line is treated as a comment.

```typescript
// This is a single-line comment

const name: string = "John"; // Store the user's name

console.log(name);
```

Single-line comments are useful for short explanations and notes.

# 🗁 Multi-Line Comments

Multi-line comments begin with `/*` and end with `*/`.

They can span multiple lines.

```typescript
/*
    This is a multi-line comment.

    It can contain several lines
    of explanatory information.
*/

const firstName: string = "John";
const lastName: string = "Doe";
```

Multi-line comments are useful for explaining complex logic or providing longer contextual information.

# 🗁 Documentation Comments

Documentation comments use the JSDoc-style syntax, beginning with `/**` and ending with `*/`.

They are commonly used to document functions, classes, interfaces, methods, and other declarations.

```typescript
/**
 * Calculates the total price.
 *
 * @param price - The price of a single item.
 * @param quantity - The number of items.
 * @returns The calculated total price.
 */
function calculateTotal(price: number, quantity: number): number {
    return price * quantity;
}
```

Documentation comments make code easier to understand and can provide information to editors, IDEs, and documentation generators.

# 🗁 Common JSDoc Tags

JSDoc supports several tags that describe different parts of a declaration.

| Tag | Description |
| --- | --- |
| `@param` | Documents a function parameter |
| `@returns` | Documents the return value |
| `@type` | Describes the type of a variable or expression |
| `@example` | Provides a usage example |
| `@deprecated` | Marks a declaration as deprecated |
| `@see` | References related documentation |
| `@throws` | Documents an exception that may be thrown |
| `@remarks` | Adds additional explanatory information |
| `@since` | Specifies when a feature was introduced |
| `@defaultValue` | Documents a default value |

Example:

```typescript
/**
 * Checks whether a user is eligible for registration.
 *
 * @param age - The user's age.
 * @returns True if the user is at least 18 years old.
 *
 * @example
 * isEligible(20); // true
 * isEligible(15); // false
 */
function isEligible(age: number): boolean {
    return age >= 18;
}
```

# 🗁 Documenting Variables

Use documentation comments to describe variables when their purpose is not immediately obvious.

```typescript
/**
 * Maximum number of login attempts allowed.
 */
const MAX_LOGIN_ATTEMPTS: number = 5;
```

For simple variables, meaningful names are usually more useful than unnecessary comments.

```typescript
const userAge: number = 25;
const isAuthenticated: boolean = true;
```

# 🗁 Documenting Functions

Document function responsibilities, parameters, return values, and important behavior.

```typescript
/**
 * Calculates the discounted price of a product.
 *
 * @param price - Original product price.
 * @param discount - Discount percentage from 0 to 100.
 * @returns Final price after applying the discount.
 */
function calculateDiscount(
    price: number,
    discount: number
): number {
    return price - (price * discount) / 100;
}
```

# 🗁 Documenting Interfaces and Types

Documentation comments can describe the purpose of interfaces and their properties.

```typescript
/**
 * Represents a registered application user.
 */
interface User {
    /** Unique identifier of the user. */
    id: number;

    /** Full name of the user. */
    name: string;

    /** Email address used for communication. */
    email: string;

    /** Indicates whether the account is active. */
    isActive: boolean;
}
```

The same approach can be used with type aliases.

```typescript
/**
 * Represents the available application account roles.
 */
type UserRole = "admin" | "editor" | "customer";
```

# 🗁 Documenting Classes and Methods

Documentation comments can describe classes, constructors, properties, and methods.

```typescript
/**
 * Represents a bank account.
 */
class BankAccount {
    /**
     * Creates a bank account.
     *
     * @param owner - Account owner's name.
     * @param balance - Initial account balance.
     */
    constructor(
        public owner: string,
        public balance: number
    ) {}

    /**
     * Deposits money into the account.
     *
     * @param amount - Amount to deposit.
     */
    deposit(amount: number): void {
        this.balance += amount;
    }
}
```

# 🗁 TODO and FIXME Comments

Developers often use special comment markers to identify unfinished work or known problems.

**TODO**

Indicates something that needs to be implemented or improved later.

```typescript
// TODO: Add input validation before saving the user.
function saveUser(): void {
    // Implementation
}
```

**FIXME**

Identifies code that has a known issue and needs correction.

```typescript
// FIXME: This calculation does not handle negative values.
function calculateAverage(total: number, count: number): number {
    return total / count;
}
```

Other commonly used markers include:

```typescript
// NOTE: This function is used by the authentication module.

// HACK: Temporary workaround for a third-party API issue.

// REVIEW: Verify this logic before production deployment.
```

These markers are conventions, not TypeScript language features. Some editors and development tools can detect them automatically.

# 🗁 Commenting Out Code

Comments can temporarily disable source code during development.

```typescript
const username: string = "John";

// console.log(username);

console.log("Application started");
```

For multiple lines:

```typescript
/*
const firstName = "John";
const lastName = "Doe";

console.log(firstName, lastName);
*/
```

Avoid keeping large blocks of obsolete code commented out. Version control systems such as Git provide a better way to preserve previous implementations.

# 🗁 Comments and TypeScript Compilation

Comments do not affect the runtime behavior of TypeScript programs.

For example:

```typescript
// This is a comment

const message: string = "Hello TypeScript";

console.log(message);
```

When compiled, the output JavaScript normally contains the executable statements without ordinary comments:

```javascript
const message = "Hello TypeScript";
console.log(message);
```

However, TypeScript provides the `removeComments` compiler option to control whether comments are preserved in emitted JavaScript.

Example configuration:

```json
{
    "compilerOptions": {
        "removeComments": true
    }
}
```

When `removeComments` is `true`, comments are removed from the emitted JavaScript. When it is `false` or omitted, comments are generally preserved.

# 🗁 Documentation Comments in IDEs

Modern editors such as Visual Studio Code can display documentation comments when developers hover over functions, methods, and other documented declarations.

Example:

```typescript
/**
 * Adds two numbers.
 *
 * @param a - First number.
 * @param b - Second number.
 * @returns The sum of both numbers.
 */
function add(a: number, b: number): number {
    return a + b;
}

add(10, 20);
```

Hovering over `add` in a supported editor can display its documentation, including parameter descriptions and the return value.

# 🗁 Generating API Documentation

Documentation comments can be used by documentation generators such as TypeDoc.

TypeDoc analyzes TypeScript declarations and their documentation comments to generate API documentation.

Example:

```typescript
/**
 * Represents a product in the store.
 */
export interface Product {
    /** Unique product identifier. */
    id: number;

    /** Product name. */
    name: string;

    /** Product price. */
    price: number;
}

/**
 * Calculates the total cost of products.
 *
 * @param products - List of products.
 * @returns Combined price of all products.
 */
export function calculateTotal(products: Product[]): number {
    return products.reduce((total, product) => total + product.price, 0);
}
```

Install TypeDoc:

```bash
npm install --save-dev typedoc
```

Generate documentation:

```bash
npx typedoc --entryPointStrategy expand src
```

The generated documentation can be published as part of a project's developer documentation.

# 🗁 Best Practices

* Write comments that explain why something happens, not merely what the code already shows.
* Use meaningful variable and function names to reduce unnecessary comments.
* Use `//` for short explanations.
* Use `/* ... */` for longer explanatory notes.
* Use `/** ... */` for API documentation.
* Document public functions, classes, interfaces, and complex APIs.
* Describe parameters and return values accurately.
* Keep comments updated whenever the implementation changes.
* Remove outdated comments and obsolete commented-out code.
* Use `TODO` and `FIXME` consistently.
* Avoid writing comments that repeat obvious code behavior.
* Use documentation generators when maintaining larger libraries or APIs.

# 🗁 Summary

TypeScript supports three primary comment styles:

| Comment Type | Syntax | Purpose |
| --- | --- | --- |
| Single-line | `//` | Short explanations |
| Multi-line | `/* ... */` | Longer explanations |
| Documentation | `/** ... */` | API and declaration documentation |

Comments improve maintainability, collaboration, and code readability. Documentation comments are especially valuable when developing reusable libraries, APIs, and large TypeScript applications.
