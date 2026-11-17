# TypeScript vs JavaScript

JavaScript and TypeScript are closely related programming languages used for web development, server-side applications, desktop applications, and more.

JavaScript is a dynamically typed programming language that executes directly in JavaScript runtimes such as browsers and Node.js.

TypeScript is a strongly typed programming language built on top of JavaScript. It adds a static type system and additional development features while preserving JavaScript's core runtime behavior.

TypeScript code is generally transformed into JavaScript before execution.

# 🗁 What Is JavaScript?

JavaScript is a high-level, dynamically typed programming language standardized as ECMAScript.

It is widely used for:

* Frontend web development.
* Backend development with Node.js.
* Browser-based applications.
* Desktop applications.
* Mobile applications.
* Serverless applications.

Example:

```javascript
let username = "John";
let age = 25;

console.log(username);
console.log(age);
```

JavaScript determines the types of these values at runtime.

A variable can also hold a different type of value later:

```javascript
let value = 100;

value = "Hello";

console.log(value);
```

This is valid JavaScript because variables are not restricted to a single type.

# 🗁 What Is TypeScript?

TypeScript is an open-source programming language developed by Microsoft.

It extends JavaScript with features such as:

* Static type checking.
* Type annotations.
* Interfaces.
* Type aliases.
* Generics.
* Enums.
* Advanced type manipulation.
* Improved editor tooling.

Example:

```typescript
let username: string = "John";
let age: number = 25;

console.log(username);
console.log(age);
```

The type annotations describe the expected types of the variables.

Assigning an incompatible value produces a compile-time type error:

```typescript
let age: number = 25;

age = "twenty-five";
```

TypeScript reports that a string cannot be assigned to a number variable.

# 🗁 Major Differences

| Feature | JavaScript | TypeScript |
| --- | --- | --- |
| Typing system | Dynamic | Static type checking |
| Type annotations | Not supported | Supported |
| Compilation | No TypeScript compilation required | Usually transformed into JavaScript |
| Execution | Directly executed by JavaScript runtimes | Executes after transformation into JavaScript, or through compatible development tools |
| Type checking | Primarily runtime behavior | Compile-time and runtime behavior |
| Interfaces | Not supported as a language construct | Supported |
| Type aliases | Not supported as a language construct | Supported |
| Generics | No native static generics | Supported |
| Editor support | Good | Enhanced through type information |
| Learning curve | Generally easier initially | Requires understanding of its type system |
| File extension | `.js` | `.ts` |
| JSX support | `.jsx` | `.tsx` |

# 🗁 Static Typing vs Dynamic Typing

One of the most important differences is how the two languages handle types.

**JavaScript: Dynamic Typing**

JavaScript determines a value's type at runtime.

```javascript
let data = 100;

data = "Hello";

data = true;

console.log(data);
```

The variable can hold values of different types.

**TypeScript: Static Type Checking**

TypeScript checks type compatibility during development or compilation.

```typescript
let data: number = 100;

data = "Hello";
```

The compiler reports an error because `data` is declared as a number.

However, TypeScript also supports flexible types through unions:

```typescript
let data: number | string;

data = 100;
data = "Hello";
```

Both assignments are valid because the variable explicitly allows either type.

# 🗁 TypeScript Is a Superset of JavaScript

TypeScript builds upon JavaScript and supports existing JavaScript syntax.

Most valid JavaScript code can be used in TypeScript, although some JavaScript patterns may require type annotations, declarations, or configuration adjustments to pass strict type checking.

JavaScript:

```javascript
function greet(name) {
    return "Hello, " + name;
}
```

Equivalent TypeScript:

```typescript
function greet(name: string): string {
    return "Hello, " + name;
}
```

The TypeScript version adds parameter and return types without changing the function's fundamental behavior.

# 🗁 Compilation and Execution

JavaScript can execute directly in a compatible runtime.

```javascript
console.log("Hello, JavaScript!");
```

TypeScript generally requires transformation into JavaScript before deployment.

```typescript
const message: string = "Hello, TypeScript!";

console.log(message);
```

Compile it:

```bash
npx tsc index.ts
```

Run the generated JavaScript:

```bash
node index.js
```

The TypeScript compiler performs type checking and removes TypeScript-specific syntax during JavaScript emission.

The resulting JavaScript is what the runtime executes.

# 🗁 TypeScript Types Do Not Exist at Runtime

TypeScript's static type system is primarily a development-time feature.

Consider:

```typescript
interface User {
    id: number;
    name: string;
}

const user: User = {
    id: 1,
    name: "John"
};

console.log(user);
```

After compilation, the interface declaration is removed:

```javascript
const user = {
    id: 1,
    name: "John"
};

console.log(user);
```

The JavaScript runtime does not know that the object was declared as a `User`.

This distinction is important when working with external data, such as API responses, user input, and database records.

TypeScript annotations alone do not validate external data at runtime. Runtime validation requires actual JavaScript logic or a validation library.

# 🗁 Error Detection

TypeScript can detect many programming mistakes before the application executes.

JavaScript example:

```javascript
function calculateTotal(price, quantity) {
    return price * quantity;
}

console.log(calculateTotal(100, "5"));
```

JavaScript implicitly converts the string `"5"` into a number during multiplication.

TypeScript example:

```typescript
function calculateTotal(
    price: number,
    quantity: number
): number {
    return price * quantity;
}

console.log(calculateTotal(100, "5"));
```

TypeScript reports an error because the second argument must be a number.

Correct usage:

```typescript
console.log(calculateTotal(100, 5));
```

Static type checking helps identify incompatible operations and incorrect function arguments before runtime.

# 🗁 Interfaces and Type Aliases

JavaScript does not provide native interfaces or type aliases for static type checking.

TypeScript provides both.

**Interface example:**

```typescript
interface Product {
    id: number;
    name: string;
    price: number;
}

const product: Product = {
    id: 1,
    name: "Laptop",
    price: 1200
};
```

**Type alias example:**

```typescript
type Status = "pending" | "approved" | "rejected";

let orderStatus: Status = "pending";
```

These features help developers define reusable and maintainable data structures.

# 🗁 Object-Oriented Programming

Both JavaScript and TypeScript support classes and inheritance.

JavaScript:

```javascript
class Person {
    constructor(name) {
        this.name = name;
    }

    greet() {
        console.log(`Hello, ${this.name}`);
    }
}

const person = new Person("John");

person.greet();
```

TypeScript:

```typescript
class Person {
    constructor(private name: string) {}

    greet(): void {
        console.log(`Hello, ${this.name}`);
    }
}

const person = new Person("John");

person.greet();
```

TypeScript adds access modifiers and type annotations.

The `private` modifier in this example provides TypeScript-level access control. It is removed during compilation, so it is not equivalent to JavaScript's runtime-enforced `#privateField` syntax.

# 🗁 When to Use JavaScript

JavaScript is a suitable choice when:

* Building small scripts or simple applications.
* Working with existing JavaScript projects.
* Creating quick prototypes.
* Learning programming fundamentals.
* Using libraries or tools that do not require TypeScript.
* Avoiding an additional compilation and type-checking workflow.

JavaScript remains a powerful choice for applications of every size.

# 🗁 When to Use TypeScript

TypeScript is particularly useful when:

* Building large applications.
* Working with complex data structures.
* Developing reusable libraries.
* Maintaining long-term projects.
* Collaborating with multiple developers.
* Building frontend applications with React, Angular, or Vue.
* Developing backend applications with Node.js.
* Working with APIs and database models.
* Refactoring code across large codebases.

Its static type system helps make code easier to understand, maintain, and refactor.

# 🗁 Advantages of JavaScript

* Simple to start using.
* Runs directly in modern JavaScript environments.
* No TypeScript compilation step is required.
* Extensive ecosystem and community.
* Flexible dynamic typing.
* Supported by virtually every modern web browser.

# 🗁 Disadvantages of JavaScript

* Many type-related errors are discovered only at runtime.
* Large applications can become harder to maintain.
* Refactoring complex code can be risky.
* Function contracts and object structures are not enforced by a built-in static type system.
* Editor tooling has less type information unless additional declarations or analysis are available.

# 🗁 Advantages of TypeScript

* Detects many type-related mistakes before runtime.
* Provides better autocomplete and editor assistance.
* Supports interfaces, type aliases, and generics.
* Improves maintainability in large codebases.
* Makes refactoring safer.
* Provides clearer function and data contracts.
* Supports gradual adoption in existing JavaScript projects.

# 🗁 Disadvantages of TypeScript

* Requires learning its type system.
* Usually introduces a compilation or transformation step.
* Type definitions may be needed for JavaScript libraries.
* Complex type-level programming can become difficult to understand.
* Incorrect or excessive type annotations can make code unnecessarily complicated.
* Static types do not automatically validate runtime data.

# 🗁 Can JavaScript and TypeScript Be Used Together?

Yes. JavaScript and TypeScript can coexist in the same project.

For example:

```text
my-project/
│
├── src/
│   ├── index.ts
│   ├── app.ts
│   ├── helpers.js
│   └── legacy.js
│
├── package.json
└── tsconfig.json
```

A TypeScript project can gradually adopt TypeScript while retaining existing JavaScript files.

The `allowJs` compiler option allows JavaScript files to be included in a TypeScript project.

Example `tsconfig.json`:

```json
{
    "compilerOptions": {
        "allowJs": true,
        "checkJs": true,
        "strict": true,
        "outDir": "./dist"
    },
    "include": [
        "src/**/*"
    ]
}
```

With `allowJs`, JavaScript files can participate in the project. With `checkJs`, TypeScript can report errors in JavaScript files as well.

# 🗁 TypeScript and JavaScript in Modern Web Development

Both languages are widely used in modern web development.

Common technologies include:

| Technology | JavaScript | TypeScript |
| --- | --- | --- |
| React | Supported | Supported through TSX |
| Angular | Uses TypeScript as its primary development language | Primary language |
| Vue | Supported | Supported |
| Node.js | Supported | Supported through compilation or compatible tools |
| Express.js | Supported | Supported |
| Next.js | Supported | Supported |
| NestJS | Supported | TypeScript-focused |
| Vite | Supported | Supported |

Choosing between them depends on project requirements, team experience, complexity, and long-term maintenance needs.

# 🗁 Summary

JavaScript and TypeScript share the same fundamental runtime ecosystem, but they provide different development experiences.

| Area | JavaScript | TypeScript |
| --- | --- | --- |
| Core language | JavaScript | JavaScript plus a static type system |
| Type checking | Dynamic runtime behavior | Static checking |
| Type safety | Not enforced by a built-in static type system | Compile-time type checking |
| Interfaces | No native static interfaces | Supported |
| Generics | No native static generics | Supported |
| Runtime | JavaScript runtime | JavaScript runtime after transformation |
| Best suited for | Projects of any size | Especially useful for large and maintainable projects |

**Key takeaway:** TypeScript does not replace JavaScript's runtime. It enhances the development experience by adding static type checking and additional language features. Learning JavaScript fundamentals first makes understanding TypeScript much easier.
