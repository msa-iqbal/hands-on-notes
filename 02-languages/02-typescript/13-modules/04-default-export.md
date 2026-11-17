# Default Export

Default exports allow a TypeScript module to expose one primary declaration that other modules can import without using curly braces.

They are useful when a module has one main responsibility, such as exporting a class, function, component, configuration object, or service.

Unlike named exports, a module can have only one default export.

# 🗁 What Is a Default Export?

A default export is a declaration marked with the `export default` keyword.

```typescript
export default function greet(name: string): string {
  return `Hello, ${name}`;
}
```

Another module can import it without specifying the original declaration name:

```typescript
import greet from "./greet";

console.log(greet("Alex"));
```

The imported name can be different from the name used in the exporting module.

# 🗁 Default Exporting a Function

Functions can be exported as the default declaration.

```typescript
export default function calculateArea(
  width: number,
  height: number
): number {
  return width * height;
}
```

Importing:

```typescript
import calculateArea from "./geometry";

const area = calculateArea(10, 5);

console.log(area);
```

You can also use a function expression:

```typescript
const calculateArea = (
  width: number,
  height: number
): number => {
  return width * height;
};

export default calculateArea;
```

Both approaches export the function as the module's default declaration.

# 🗁 Default Exporting a Class

A class can be exported as the default declaration.

```typescript
export default class User {
  constructor(
    public id: number,
    public name: string
  ) {}

  getDetails(): string {
    return `${this.id}: ${this.name}`;
  }
}
```

Importing:

```typescript
import User from "./user";

const user = new User(101, "Alex");

console.log(user.getDetails());
```

This pattern is common when a file represents one primary class.

# 🗁 Default Exporting an Object

You can export an object as the default declaration.

```typescript
const appConfig = {
  appName: "My Application",
  version: "1.0.0",
  environment: "development",
};

export default appConfig;
```

Importing:

```typescript
import appConfig from "./app-config";

console.log(appConfig.appName);

console.log(appConfig.version);
```

This is useful for configuration modules and objects containing related settings.

# 🗁 Default Exporting an Array

Arrays can also be exported as default values.

```typescript
const supportedLanguages = [
  "English",
  "Bangla",
  "Spanish",
];

export default supportedLanguages;
```

Importing:

```typescript
import supportedLanguages from "./languages";

console.log(supportedLanguages);
```

# 🗁 Default Exporting a Constant

A constant can be exported directly.

```typescript
const API_URL = "https://api.example.com";

export default API_URL;
```

Importing:

```typescript
import API_URL from "./config";

console.log(API_URL);
```

You can also write:

```typescript
export default "https://api.example.com";
```

However, directly exporting a value makes it harder to reference that declaration by its original name within the same module.

# 🗁 Default Exporting an Anonymous Function

A default-exported function does not require a name.

```typescript
export default function (
  firstName: string,
  lastName: string
): string {
  return `${firstName} ${lastName}`;
}
```

Importing:

```typescript
import getFullName from "./full-name";

console.log(getFullName("Alex", "Morgan"));
```

The importing module chooses the local name.

# 🗁 Default Exporting an Anonymous Class

You can also export an anonymous class.

```typescript
export default class {
  constructor(public message: string) {}

  display(): void {
    console.log(this.message);
  }
}
```

Importing:

```typescript
import Message from "./message";

const message = new Message("Hello TypeScript");

message.display();
```

Named classes are generally easier to identify in debugging tools and stack traces, so prefer named classes when practical.

# 🗁 Renaming Default Imports

Default imports do not need to use the declaration's original name.

`user.ts`

```typescript
export default class User {
  constructor(public name: string) {}
}
```

`app.ts`

```typescript
import UserModel from "./user";

const user = new UserModel("Alex");
```

The local name `UserModel` is independent of the original declaration name.

This differs from named imports, where the original exported name must be used unless an alias is specified.

# 🗁 Combining Default and Named Exports

A module can have one default export and multiple named exports.

```typescript
export const API_VERSION = "v1";

export function connect(): void {
  console.log("Connected");
}

export default class ApiClient {
  request(): void {
    console.log("Sending request");
  }
}
```

Importing:

```typescript
import ApiClient, {
  API_VERSION,
  connect,
} from "./api-client";

const client = new ApiClient();

client.request();

connect();

console.log(API_VERSION);
```

The default import appears separately from named imports.

# 🗁 Default Import with an Alias

A default import is already a local binding, so it does not use the `as` keyword.

```typescript
import ApiClient from "./api-client";
```

To rename it, simply choose another local identifier:

```typescript
import Client from "./api-client";
```

For named imports, use `as`:

```typescript
import { API_VERSION as VERSION } from "./api-client";
```

# 🗁 Re-Exporting a Default Export

You can re-export another module's default declaration.

`calculator.ts`

```typescript
export default class Calculator {
  add(a: number, b: number): number {
    return a + b;
  }
}
```

`index.ts`

```typescript
export { default } from "./calculator";
```

Another module can import it through the index file:

```typescript
import Calculator from "./index";

const calculator = new Calculator();

console.log(calculator.add(10, 20));
```

This is useful when creating barrel files.

# 🗁 Re-Exporting a Default Export with a Name

You can expose another module's default export as a named export.

`calculator.ts`

```typescript
export default class Calculator {
  add(a: number, b: number): number {
    return a + b;
  }
}
```

`index.ts`

```typescript
export { default as Calculator } from "./calculator";
```

Importing:

```typescript
import { Calculator } from "./index";

const calculator = new Calculator();
```

This is useful when a package wants to provide a consistent named-export interface.

# 🗁 Importing Default and Named Exports Separately

You can import default and named declarations in separate statements.

```typescript
import ApiClient from "./api-client";

import {
  API_VERSION,
  connect,
} from "./api-client";
```

You can also combine them into one statement:

```typescript
import ApiClient, {
  API_VERSION,
  connect,
} from "./api-client";
```

Both approaches are valid.

# 🗁 Default Exports in React Components

Default exports are commonly used for React components.

```tsx
import React from "react";

export default function UserCard() {
  return (
    <div>
      <h1>User Profile</h1>
      <p>Welcome to the application.</p>
    </div>
  );
}
```

Importing:

```tsx
import UserCard from "./UserCard";

function App() {
  return <UserCard />;
}

export default App;
```

A component file often has one primary component, making default exports convenient.

# 🗁 Default Exports in Service Modules

A service module can expose one primary service class.

`user.service.ts`

```typescript
export default class UserService {
  getUsers(): string[] {
    return ["Alex", "Sam", "John"];
  }

  getUserById(id: number): string {
    return `User ${id}`;
  }
}
```

Importing:

```typescript
import UserService from "./user.service";

const userService = new UserService();

console.log(userService.getUsers());

console.log(userService.getUserById(101));
```

This approach can be useful when a module is centered around one service.

# 🗁 Default Exports and TypeScript Types

A default export can represent a type declaration.

For example, a type alias:

```typescript
type User = {
  id: number;
  name: string;
};

export default User;
```

Importing it as a type:

```typescript
import type User from "./user";

const user: User = {
  id: 101,
  name: "Alex",
};
```

A default-exported interface is also possible:

```typescript
export default interface User {
  id: number;
  name: string;
}
```

Importing:

```typescript
import type User from "./user";
```

These declarations are used only by the TypeScript type system and do not create runtime JavaScript values.

# 🗁 Default Export vs Named Export

| Feature | Default Export | Named Export |
| --- | --- | --- |
| Syntax | `export default` | `export` |
| Number per module | One | Multiple |
| Import syntax | Without curly braces | With curly braces |
| Import naming | Any local name | Original name or alias |
| Re-export syntax | `export { default }` | `export { name }` |
| Common use | Primary module declaration | Multiple related declarations |

Example:

```typescript
// Default export
export default function greet(): void {}

// Named exports
export function login(): void {}

export function logout(): void {}
```

Importing:

```typescript
import greet, {
  login,
  logout,
} from "./auth";
```

# 🗁 Default Export vs Export Default Expression

These two forms have different purposes.

**Exporting a named declaration as default:**

```typescript
function greet(): string {
  return "Hello";
}

export default greet;
```

The function is declared locally and then exported as the default value.

**Declaring a default-exported function directly:**

```typescript
export default function greet(): string {
  return "Hello";
}
```

Both export the same function as the module's default declaration.

# 🗁 Common Default Export Errors

**Using curly braces for a default import**

Incorrect:

```typescript
import { User } from "./user";
```

This expects a named export called `User`.

Correct:

```typescript
import User from "./user";
```

**Using a default import for a named export**

If a module contains:

```typescript
export class User {}
```

This is incorrect:

```typescript
import User from "./user";
```

Correct:

```typescript
import { User } from "./user";
```

**Declaring multiple default exports**

Incorrect:

```typescript
export default function add(): void {}

export default function subtract(): void {}
```

A module can have only one default export.

Correct:

```typescript
export default function add(): void {}

export function subtract(): void {}
```

**Importing a nonexistent default export**

If a module contains only named exports:

```typescript
export const API_URL = "https://api.example.com";
```

This import is incorrect:

```typescript
import API_URL from "./config";
```

Correct:

```typescript
import { API_URL } from "./config";
```

# 🗁 Best Practices

* Use default exports when a module has one clear primary declaration.
* Prefer named exports when a module exposes several related utilities.
* Keep export conventions consistent throughout a project.
* Avoid using default exports merely to make import names flexible.
* Prefer meaningful declaration names for better readability and debugging.
* Use `import type` when importing declarations exclusively for type checking.
* Use explicit re-exports when designing public module interfaces.
* Avoid exporting unnecessary implementation details.
* Do not mix default and named exports without a clear reason.

# 🗁 Summary

Default exports allow a module to expose one primary declaration.

Key concepts:

* `export default` creates a default export.
* A module can have only one default export.
* Functions, classes, objects, arrays, constants, and type declarations can be default-exported.
* Default imports do not require curly braces.
* The importing module can choose its own local identifier.
* Default and named exports can coexist.
* `export { default }` re-exports a default declaration.
* `export { default as Name }` exposes a default declaration under a named export.
* `import type` can be used when importing a default-exported type.

Understanding default exports helps you organize TypeScript modules and design clean, reusable module interfaces.
