# Declaration Merging

Declaration merging is a TypeScript feature that allows multiple declarations with the same name to be combined into a single definition.

Instead of treating declarations as conflicting duplicates, TypeScript can merge compatible declarations and make their members available through one resulting type.

Declaration merging commonly applies to:

* Interfaces
* Namespaces
* Classes and namespaces
* Functions and namespaces
* Enums and namespaces

It is particularly useful when extending existing APIs, organizing large type definitions, and augmenting third-party libraries.

# 🗁 Interface Declaration Merging

Interfaces with the same name in the same scope are automatically merged.

```typescript
interface User {
  id: number;
}

interface User {
  name: string;
}

interface User {
  email: string;
}
```

TypeScript combines all three declarations into one interface.

```typescript
const user: User = {
  id: 1,
  name: "Alice",
  email: "alice@example.com",
};
```

The resulting `User` interface contains all three properties.

# 🗁 Merging Interface Members

When interfaces are merged, their members are combined.

```typescript
interface Product {
  id: number;
  name: string;
}

interface Product {
  price: number;
  category: string;
}
```

The resulting structure is equivalent to:

```typescript
interface Product {
  id: number;
  name: string;
  price: number;
  category: string;
}
```

However, duplicate non-function members must have compatible declarations.

```typescript
interface User {
  id: number;
}

interface User {
  // Error: subsequent property declarations must have the same type
  id: string;
}
```

The same property cannot be declared with conflicting types.

# 🗁 Function Overload Merging

Interfaces can contain function overloads, and their overload signatures are combined during declaration merging.

```typescript
interface Formatter {
  format(value: string): string;
}

interface Formatter {
  format(value: number): string;
}
```

Both signatures are available:

```typescript
const formatter: Formatter = {
  format(value: string | number): string {
    return String(value);
  },
};

formatter.format("Hello");
formatter.format(100);
```

The implementation must be compatible with the overload signatures.

# 🗁 Namespace Declaration Merging

Namespaces with the same name can also merge.

```typescript
namespace App {
  export const version = "1.0.0";
}

namespace App {
  export function start(): void {
    console.log("Application started");
  }
}
```

Both declarations contribute to the same namespace.

```typescript
console.log(App.version);

App.start();
```

Only exported members are accessible outside the namespace.

```typescript
namespace App {
  const internalValue = 100;

  export const publicValue = 200;
}

console.log(App.publicValue);

// Error: internalValue is not exported
console.log(App.internalValue);
```

# 🗁 Class and Namespace Merging

A namespace can merge with a class to add related static members.

The namespace declaration must appear after the class declaration.

```typescript
class User {
  constructor(public name: string) {}

  greet(): void {
    console.log(`Hello, ${this.name}`);
  }
}

namespace User {
  export const role = "Member";

  export function create(name: string): User {
    return new User(name);
  }
}
```

Usage:

```typescript
const user = User.create("Alice");

user.greet();

console.log(User.role);
```

The class provides instance behavior, while the namespace provides additional static functionality.

A namespace cannot merge with a class by adding new instance properties to its instances.

# 🗁 Function and Namespace Merging

A namespace can also merge with a function to expose related properties and utilities.

```typescript
function createLogger(message: string): void {
  console.log(`[LOG]: ${message}`);
}

namespace createLogger {
  export const version = "1.0.0";

  export function warn(message: string): void {
    console.warn(`[WARNING]: ${message}`);
  }
}
```

Usage:

```typescript
createLogger("Application started");

createLogger.warn("Something needs attention");

console.log(createLogger.version);
```

This pattern is useful when a function needs associated utility methods or metadata.

# 🗁 Enum and Namespace Merging

Enums can merge with namespaces to provide additional functionality.

```typescript
enum Status {
  Pending,
  Approved,
  Rejected,
}

namespace Status {
  export function isFinal(status: Status): boolean {
    return status === Status.Approved ||
      status === Status.Rejected;
  }
}
```

Usage:

```typescript
console.log(Status.isFinal(Status.Approved)); // true
console.log(Status.isFinal(Status.Pending)); // false
```

This allows utility functions to be grouped with their related enum.

# 🗁 Namespace and Namespace Merging

Multiple namespaces with the same name can contribute different exported members.

```typescript
namespace Database {
  export function connect(): void {
    console.log("Connecting to database");
  }
}

namespace Database {
  export function disconnect(): void {
    console.log("Disconnecting from database");
  }
}
```

Usage:

```typescript
Database.connect();
Database.disconnect();
```

The resulting namespace exposes both functions.

# 🗁 Declaration Merging Restrictions

Not every declaration combination is allowed.

| Declaration A | Declaration B | Can merge? |
| --- | --- | --- |
| Interface | Interface | Yes |
| Namespace | Namespace | Yes |
| Class | Namespace | Yes, namespace after class |
| Function | Namespace | Yes, namespace after function |
| Enum | Namespace | Yes, namespace after enum |
| Class | Class | No |
| Class | Interface | No direct declaration merge |
| Type alias | Type alias | No |
| Type alias | Interface | No |

A class can implement an interface, but that is different from declaration merging.

```typescript
interface User {
  id: number;
}

class UserModel implements User {
  constructor(public id: number) {}
}
```

The class implements the interface contract without merging with it.

# 🗁 Declaration Merging vs Type Intersection

Declaration merging and intersection types can produce similar-looking results, but they are different mechanisms.

**Declaration merging:**

```typescript
interface User {
  id: number;
}

interface User {
  name: string;
}
```

Both declarations contribute to the same interface.

**Intersection type:**

```typescript
type UserIdentity = {
  id: number;
};

type UserProfile = {
  name: string;
};

type User = UserIdentity & UserProfile;
```

The intersection combines existing types into a new type.

An intersection can combine type aliases, interfaces, and other compatible types without relying on repeated declarations with the same name.

# 🗁 Declaration Merging and Module Scope

Declaration merging works within the relevant declaration scope.

An interface declared in a module is not automatically available globally.

```typescript
export interface User {
  id: number;
}
```

To use it elsewhere, import it:

```typescript
import type { User } from "./user";

const user: User = {
  id: 1,
};
```

When two declarations need to contribute to the same exported interface, they must participate in the same module declaration context or use an appropriate module augmentation mechanism.

# 🗁 Practical Example: Extending Application Types

Declaration merging can be useful when different parts of an application contribute properties to a shared configuration interface.

```typescript
interface AppConfig {
  appName: string;
}

interface AppConfig {
  environment: "development" | "production";
}

interface AppConfig {
  debug: boolean;
}
```

Usage:

```typescript
const config: AppConfig = {
  appName: "My Application",
  environment: "development",
  debug: true,
};
```

Each declaration adds a responsibility to the same configuration contract.

In larger applications, carefully organize these declarations to avoid unexpected coupling.

# 🗁 Best Practices

* Use declaration merging when multiple declarations genuinely contribute to the same conceptual type.
* Keep merged interfaces logically related.
* Avoid scattering declarations across unrelated files without a clear reason.
* Ensure duplicate property declarations have compatible types.
* Use namespaces for related static utilities when working with namespace-based APIs.
* Prefer ordinary composition and intersection types when combining independently named types.
* Understand the difference between declaration merging and module augmentation.
* Avoid unnecessary merging that makes a type difficult to locate and maintain.

# 🗁 Key Takeaways

* Declaration merging combines multiple declarations with the same name.
* Interfaces and namespaces support declaration merging.
* Classes, functions, and enums can merge with namespaces under specific rules.
* Duplicate non-function interface members must have compatible types.
* Function overload signatures can be combined.
* Classes cannot directly merge with other classes or interfaces.
* Type aliases do not support declaration merging.
* Declaration merging is useful for extending existing APIs and organizing related type definitions.
