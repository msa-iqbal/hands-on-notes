# Interface Extension

Interface extension is a TypeScript feature that allows one interface to inherit the properties and methods of another interface.

It uses the `extends` keyword to create a new interface based on an existing one.

Interface extension helps you:

* Reuse existing interface definitions.
* Avoid duplicating common properties.
* Build hierarchical data structures.
* Organize related application models.
* Create specialized interfaces from general contracts.
* Maintain consistent object structures across an application.

# 🗁 Basic Syntax

Use the `extends` keyword to extend an existing interface.

```typescript
interface BaseInterface {
  property: Type;
}

interface ExtendedInterface extends BaseInterface {
  additionalProperty: Type;
}
```

The extended interface automatically includes all members of the base interface.

Example:

```typescript
interface Person {
  name: string;
  age: number;
}

interface Employee extends Person {
  employeeId: number;
  department: string;
}
```

The `Employee` interface contains all properties from `Person`, along with its own properties.

Create an object:

```typescript
const employee: Employee = {
  name: "Alice",
  age: 25,
  employeeId: 1001,
  department: "Engineering",
};
```

# 🗁 Extending a Single Interface

An interface can extend one parent interface.

```typescript
interface User {
  id: number;
  name: string;
}

interface Admin extends User {
  permissions: string[];
}
```

Example:

```typescript
const admin: Admin = {
  id: 1,
  name: "Alice",
  permissions: ["create", "update", "delete"],
};
```

The `Admin` interface inherits `id` and `name` from `User`.

# 🗁 Extending Multiple Interfaces

TypeScript allows an interface to extend multiple interfaces.

Separate the parent interfaces using commas.

```typescript
interface Identity {
  id: number;
  name: string;
}

interface Contact {
  email: string;
  phone: string;
}

interface User extends Identity, Contact {
  active: boolean;
}
```

Example:

```typescript
const user: User = {
  id: 1,
  name: "Alice",
  email: "alice@example.com",
  phone: "1234567890",
  active: true,
};
```

The resulting interface includes members from all parent interfaces and its own declaration.

# 🗁 Multi-Level Interface Extension

An interface can extend another interface that already extends a different interface.

```typescript
interface Person {
  name: string;
}

interface Employee extends Person {
  employeeId: number;
}

interface Manager extends Employee {
  teamSize: number;
}
```

Example:

```typescript
const manager: Manager = {
  name: "Alice",
  employeeId: 1001,
  teamSize: 10,
};
```

The `Manager` interface inherits members through the entire extension chain.

# 🗁 Extending Interfaces with Optional Properties

Optional properties are inherited just like required properties.

```typescript
interface User {
  id: number;
  name: string;
  email?: string;
}

interface Admin extends User {
  permissions: string[];
}
```

Example:

```typescript
const admin: Admin = {
  id: 1,
  name: "Alice",
  permissions: ["manage-users"],
};
```

The `email` property is optional because it is optional in the parent interface.

# 🗁 Extending Interfaces with Readonly Properties

Readonly properties are also inherited.

```typescript
interface User {
  readonly id: number;
  name: string;
}

interface Admin extends User {
  permissions: string[];
}
```

Example:

```typescript
const admin: Admin = {
  id: 1,
  name: "Alice",
  permissions: ["manage-users"],
};

// Valid:
admin.name = "Bob";

// Type error:
admin.id = 2;
```

The extended interface preserves the readonly restriction.

# 🗁 Overriding Properties During Extension

An extending interface can redeclare an inherited property when the new property type is compatible with the original property type.

For example, a narrower property type is allowed:

```typescript
interface User {
  id: number;
  status: string;
}

interface Admin extends User {
  status: "active" | "inactive";
}
```

The `Admin` interface restricts `status` to two permitted string values.

Example:

```typescript
const admin: Admin = {
  id: 1,
  status: "active",
};
```

An incompatible property type causes an error.

```typescript
interface User {
  id: number;
}

// Error: string is not compatible with number.
interface Admin extends User {
  id: string;
}
```

For ordinary mutable properties, TypeScript requires the extended property type to be compatible with the inherited property type.

# 🗁 Extending Interfaces with Different Property Names

An extending interface does not need to modify inherited property names.

It can simply introduce additional properties.

```typescript
interface Product {
  id: number;
  name: string;
}

interface DigitalProduct extends Product {
  downloadUrl: string;
  fileSize: number;
}
```

Example:

```typescript
const product: DigitalProduct = {
  id: 101,
  name: "TypeScript Course",
  downloadUrl: "/downloads/typescript-course.zip",
  fileSize: 1024,
};
```

This approach is useful for creating specialized models.

# 🗁 Interface Extension vs Intersection Types

Interface extension and intersection types can both combine object structures.

Interface extension:

```typescript
interface Identity {
  id: number;
  name: string;
}

interface Contact extends Identity {
  email: string;
}
```

Intersection type:

```typescript
type Identity = {
  id: number;
  name: string;
};

type Contact = Identity & {
  email: string;
};
```

Both produce a structure containing `id`, `name`, and `email`.

However, their behavior differs when properties conflict.

Consider two incompatible property definitions:

```typescript
interface A {
  value: string;
}

interface B {
  value: number;
}
```

Using interface extension:

```typescript
// Error: the inherited property types are incompatible.
interface C extends A, B {}
```

Using an intersection:

```typescript
type C = A & B;
```

The resulting `value` property has an intersection type of `string & number`, which is effectively impossible to satisfy.

| Feature | Interface Extension | Intersection |
| --- | --- | --- |
| Syntax | `extends` | `&` |
| Reuses existing structures | Yes | Yes |
| Combines multiple types | Yes | Yes |
| Conflicting properties | Generally rejected | Types are intersected |
| Declaration merging | Supported by interfaces | Not supported by type aliases |
| Common use | Extensible object contracts | Combining arbitrary types |

# 🗁 Extending Generic Interfaces

An interface can extend a generic interface by supplying a concrete type argument.

```typescript
interface Response<T> {
  data: T;
  success: boolean;
}

interface User {
  id: number;
  name: string;
}

interface UserResponse extends Response<User> {
  message: string;
}
```

Example:

```typescript
const response: UserResponse = {
  data: {
    id: 1,
    name: "Alice",
  },
  success: true,
  message: "User retrieved successfully",
};
```

The `UserResponse` interface inherits the generic response structure with `T` resolved to `User`.

# 🗁 Extending Generic Interfaces with Type Parameters

An extending interface can also declare its own generic parameter.

```typescript
interface Response<T> {
  data: T;
  success: boolean;
}

interface PaginatedResponse<T> extends Response<T[]> {
  page: number;
  totalPages: number;
}
```

Example:

```typescript
interface User {
  id: number;
  name: string;
}

const response: PaginatedResponse<User> = {
  data: [
    {
      id: 1,
      name: "Alice",
    },
    {
      id: 2,
      name: "Bob",
    },
  ],
  success: true,
  page: 1,
  totalPages: 5,
};
```

The inherited `data` property becomes an array of `User` objects.

# 🗁 Extending Interfaces with Function Members

Interfaces can inherit method signatures and function-valued properties.

```typescript
interface Logger {
  log(message: string): void;
}

interface AdvancedLogger extends Logger {
  error(message: string): void;
  warn(message: string): void;
}
```

Example:

```typescript
const logger: AdvancedLogger = {
  log(message) {
    console.log(message);
  },

  error(message) {
    console.error(message);
  },

  warn(message) {
    console.warn(message);
  },
};
```

The extended interface requires all methods from the parent and its own declaration.

# 🗁 Extending Interfaces in Classes

A class can implement an extended interface.

```typescript
interface Person {
  name: string;
}

interface Employee extends Person {
  employeeId: number;
}

class Developer implements Employee {
  constructor(
    public name: string,
    public employeeId: number,
  ) {}
}
```

Example:

```typescript
const developer = new Developer("Alice", 1001);

console.log(developer.name);
console.log(developer.employeeId);
```

The class must satisfy the complete structure of the extended interface.

# 🗁 Practical Example: Application User Roles

Interface extension is useful when an application has multiple user roles that share common properties.

```typescript
interface BaseUser {
  readonly id: number;
  name: string;
  email: string;
  active: boolean;
}

interface Admin extends BaseUser {
  permissions: string[];
}

interface Customer extends BaseUser {
  shippingAddress: string;
}

interface Developer extends BaseUser {
  programmingLanguages: string[];
}
```

Create different user objects:

```typescript
const admin: Admin = {
  id: 1,
  name: "Alice",
  email: "alice@example.com",
  active: true,
  permissions: ["manage-users", "manage-products"],
};

const customer: Customer = {
  id: 2,
  name: "Bob",
  email: "bob@example.com",
  active: true,
  shippingAddress: "Dhaka, Bangladesh",
};

const developer: Developer = {
  id: 3,
  name: "Charlie",
  email: "charlie@example.com",
  active: true,
  programmingLanguages: ["TypeScript", "Python"],
};
```

All three interfaces share the same base properties but add role-specific members.

# 🗁 Common Mistakes

**Forgetting inherited properties**

```typescript
interface User {
  id: number;
  name: string;
}

interface Admin extends User {
  permissions: string[];
}

// Error: id and name are missing.
const admin: Admin = {
  permissions: ["manage-users"],
};
```

Include all required inherited properties:

```typescript
const admin: Admin = {
  id: 1,
  name: "Alice",
  permissions: ["manage-users"],
};
```

**Using incompatible property types**

```typescript
interface User {
  id: number;
}

// Error:
interface Admin extends User {
  id: string;
}
```

The extended property must remain compatible with the inherited property.

**Confusing extension with implementation**

```typescript
interface User {
  id: number;
  name: string;
}

interface Admin extends User {
  permissions: string[];
}
```

`extends` establishes an interface relationship. It does not create an object or implement any runtime behavior.

**Extending unrelated interfaces with conflicting members**

```typescript
interface A {
  value: string;
}

interface B {
  value: number;
}

// Error:
interface C extends A, B {}
```

Resolve the conflicting definitions before extending multiple interfaces.

# 🗁 Best Practices

* Use a base interface for properties shared by related models.
* Use descriptive names for base and specialized interfaces.
* Extend interfaces when the new structure is a natural specialization of an existing contract.
* Keep inheritance hierarchies relatively shallow.
* Avoid incompatible property redeclarations.
* Use multiple interface extension when combining independent contracts.
* Use intersection types when combining arbitrary types is more appropriate.
* Use generic interfaces to create reusable extension patterns.
* Keep interfaces focused on a clear responsibility.
* Prefer composition over deep inheritance when structures become difficult to maintain.

# 🗁 Summary

* Interface extension uses the `extends` keyword.
* An interface can extend one or multiple interfaces.
* Extended interfaces inherit all parent members.
* Extension supports optional and readonly properties.
* An extending interface can introduce additional members.
* Inherited property types must remain compatible.
* Generic interfaces can be extended with concrete or generic type arguments.
* Classes can implement extended interfaces.
* Interface extension is useful for building reusable and specialized object contracts.

# 🗁 Related Topics

* Object Types
* Type Aliases
* Interfaces
* Interface Merging
* Optional Properties
* Readonly Properties
* Generic Interfaces
* Intersection Types
* Classes and Implements
* Structural Typing
