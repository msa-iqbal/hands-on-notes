# Interfaces

An interface in TypeScript defines a contract that describes the structure of an object.

It specifies which properties and methods an object must provide and the types those members must follow.

Interfaces are commonly used to:

* Define object structures.
* Describe API request and response data.
* Establish contracts between application components.
* Define class implementations.
* Create reusable and extensible data models.
* Improve code consistency and maintainability.

# 🗁 Basic Syntax

Use the `interface` keyword to declare an interface.

```typescript
interface InterfaceName {
  propertyName: Type;
}
```

Example:

```typescript
interface User {
  id: number;
  name: string;
  email: string;
}
```

This interface requires an object to contain three properties:

* `id` must be a number.
* `name` must be a string.
* `email` must be a string.

Create an object that implements the structure:

```typescript
const user: User = {
  id: 1,
  name: "Alice",
  email: "alice@example.com",
};
```

TypeScript checks whether the object satisfies the interface.

# 🗁 Interface Properties

An interface can define multiple properties with different types.

```typescript
interface Product {
  id: number;
  name: string;
  price: number;
  available: boolean;
  tags: string[];
}
```

Example:

```typescript
const product: Product = {
  id: 101,
  name: "Keyboard",
  price: 75,
  available: true,
  tags: ["computer", "accessory"],
};
```

Properties are required by default unless explicitly declared optional.

# 🗁 Optional Properties

Use the `?` modifier to make an interface property optional.

```typescript
interface User {
  id: number;
  name: string;
  email?: string;
}
```

Both objects are valid:

```typescript
const user1: User = {
  id: 1,
  name: "Alice",
};

const user2: User = {
  id: 2,
  name: "Bob",
  email: "bob@example.com",
};
```

An optional property may be omitted.

When accessing it, TypeScript includes `undefined` in its type.

```typescript
const email = user1.email;

// string | undefined
```

You should check whether the value exists before using it.

```typescript
if (user1.email !== undefined) {
  console.log(user1.email.toLowerCase());
}
```

# 🗁 Readonly Properties

Use the `readonly` modifier to prevent a property from being reassigned after object creation.

```typescript
interface User {
  readonly id: number;
  name: string;
}

const user: User = {
  id: 1,
  name: "Alice",
};

user.name = "Bob"; // Valid

// Type error:
user.id = 2;
```

Readonly is a compile-time restriction. It does not automatically freeze the object at runtime.

# 🗁 Method Signatures

Interfaces can define methods that an object must implement.

```typescript
interface User {
  id: number;
  name: string;

  greet(): string;
}
```

Example:

```typescript
const user: User = {
  id: 1,
  name: "Alice",

  greet() {
    return `Hello, ${this.name}!`;
  },
};

console.log(user.greet());
```

The method must follow the declared parameter and return types.

# 🗁 Function Properties

An interface can also define properties whose values are functions.

```typescript
interface Calculator {
  add: (a: number, b: number) => number;
  subtract: (a: number, b: number) => number;
}
```

Example:

```typescript
const calculator: Calculator = {
  add: (a, b) => a + b,
  subtract: (a, b) => a - b,
};

console.log(calculator.add(10, 5));
```

Method signatures and function-valued properties describe callable members using different syntax. They can also differ in some type-system behaviors, such as parameter variance.

# 🗁 Nested Object Structures

Interfaces can describe nested objects.

```typescript
interface Address {
  city: string;
  country: string;
  postalCode: string;
}

interface User {
  id: number;
  name: string;
  address: Address;
}
```

Example:

```typescript
const user: User = {
  id: 1,
  name: "Alice",
  address: {
    city: "Dhaka",
    country: "Bangladesh",
    postalCode: "1000",
  },
};
```

Separating nested structures into interfaces makes complex models easier to reuse and maintain.

You can also define nested structures directly:

```typescript
interface User {
  id: number;
  name: string;

  address: {
    city: string;
    country: string;
  };
}
```

# 🗁 Array Types with Interfaces

Interfaces can describe arrays of objects indirectly through their properties or directly through an index signature.

For an array of users, you can use the interface as the element type:

```typescript
interface User {
  id: number;
  name: string;
}

const users: User[] = [
  {
    id: 1,
    name: "Alice",
  },
  {
    id: 2,
    name: "Bob",
  },
];
```

You can also use the generic array syntax:

```typescript
const users: Array<User> = [
  {
    id: 1,
    name: "Alice",
  },
];
```

Both forms describe an array containing `User` objects.

# 🗁 Index Signatures

An index signature allows an interface to describe objects with dynamic property names.

```typescript
interface Scores {
  [subject: string]: number;
}
```

Example:

```typescript
const scores: Scores = {
  mathematics: 95,
  physics: 88,
  chemistry: 91,
};
```

The index signature requires values accessed through string keys to be numbers.

You can combine an index signature with explicitly declared properties, provided their types are compatible.

```typescript
interface User {
  id: number;
  name: string;
  [key: string]: string | number;
}
```

Example:

```typescript
const user: User = {
  id: 1,
  name: "Alice",
  age: 25,
};
```

# 🗁 Extending Interfaces

An interface can extend another interface using the `extends` keyword.

This allows one interface to inherit the properties and methods of another.

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

Example:

```typescript
const employee: Employee = {
  name: "Alice",
  age: 25,
  employeeId: 1001,
  department: "Engineering",
};
```

The `Employee` interface includes all properties from `Person` along with its own properties.

An interface can extend multiple interfaces:

```typescript
interface Identity {
  id: number;
}

interface Contact {
  email: string;
}

interface User extends Identity, Contact {
  name: string;
}
```

The resulting interface requires properties from all three definitions.

# 🗁 Implementing Interfaces in Classes

A class can implement an interface using the `implements` keyword.

The class must satisfy the interface's required structure.

```typescript
interface User {
  id: number;
  name: string;

  greet(): string;
}

class Admin implements User {
  constructor(
    public id: number,
    public name: string,
  ) {}

  greet(): string {
    return `Hello, ${this.name}!`;
  }
}
```

Create an instance:

```typescript
const admin = new Admin(1, "Alice");

console.log(admin.greet());
```

The `implements` keyword checks that the class conforms to the interface. It does not automatically generate implementations or inherit code from the interface.

# 🗁 Interfaces for Function Types

An interface can describe a callable function using a call signature.

```typescript
interface Add {
  (a: number, b: number): number;
}
```

Example:

```typescript
const add: Add = (a, b) => {
  return a + b;
};

console.log(add(10, 20));
```

This is useful when a function needs to be represented by a reusable interface type.

# 🗁 Interfaces with Construct Signatures

An interface can describe a constructor signature using the `new` keyword.

```typescript
interface UserConstructor {
  new (id: number, name: string): User;
}

interface User {
  id: number;
  name: string;
}
```

A class can satisfy this constructor shape:

```typescript
class Admin {
  constructor(
    public id: number,
    public name: string,
  ) {}
}

const UserClass: UserConstructor = Admin;

const user = new UserClass(1, "Alice");
```

Construct signatures are useful when working with class constructors, factories, and dependency injection.

# 🗁 Generic Interfaces

Interfaces can declare generic type parameters.

```typescript
interface Response<T> {
  data: T;
  success: boolean;
}
```

The generic parameter allows the interface to work with different data types.

Example:

```typescript
interface User {
  id: number;
  name: string;
}

const userResponse: Response<User> = {
  data: {
    id: 1,
    name: "Alice",
  },
  success: true,
};

const numberResponse: Response<number> = {
  data: 100,
  success: true,
};
```

Generic interfaces are especially useful for reusable API response structures and data containers.

# 🗁 Interfaces with Union Types

An interface can contain properties that accept union types.

```typescript
interface User {
  id: number;
  name: string;
  status: "active" | "inactive";
  age: number | string;
}
```

Example:

```typescript
const user: User = {
  id: 1,
  name: "Alice",
  status: "active",
  age: 25,
};
```

Union types allow properties to accept one of several permitted types.

# 🗁 Interfaces and Intersection Types

Interfaces can be combined using intersection types.

```typescript
interface Identity {
  id: number;
}

interface Contact {
  email: string;
}

type User = Identity & Contact;
```

Example:

```typescript
const user: User = {
  id: 1,
  email: "alice@example.com",
};
```

An intersection type combines the requirements of both interfaces.

# 🗁 Declaration Merging

One important feature of interfaces is declaration merging.

If multiple interfaces with the same name are declared in the same scope, TypeScript merges their members.

```typescript
interface User {
  id: number;
}

interface User {
  name: string;
}
```

TypeScript treats them as one interface:

```typescript
const user: User = {
  id: 1,
  name: "Alice",
};
```

Declaration merging is useful for extending existing type definitions, especially when working with third-party libraries and global declarations.

If duplicate declarations define the same property, their types must be compatible. Incompatible declarations produce an error.

# 🗁 Interface Naming Conventions

TypeScript does not require a special naming convention for interfaces.

A common convention is to use PascalCase.

```typescript
interface User {}
interface Product {}
interface ApiResponse {}
interface DatabaseConnection {}
```

Avoid adding an `I` prefix automatically unless your project's established style requires it.

# 🗁 Interface vs Type Alias

Interfaces and type aliases can both describe object structures.

Interface:

```typescript
interface User {
  id: number;
  name: string;
}
```

Type alias:

```typescript
type User = {
  id: number;
  name: string;
};
```

Comparison:

| Feature | Interface | Type Alias |
| --- | --- | --- |
| Describe object structures | Yes | Yes |
| Optional properties | Yes | Yes |
| Readonly properties | Yes | Yes |
| Method signatures | Yes | Yes |
| Generic types | Yes | Yes |
| Extend or combine structures | With `extends` | With intersections |
| Union types | No direct declaration | Yes |
| Tuple types | No direct tuple declaration | Yes |
| Declaration merging | Yes | No |
| Primitive aliases | No | Yes |
| Conditional types | No direct declaration | Yes |

Both are important parts of TypeScript's type system.

A practical approach is to use interfaces for extensible object contracts and type aliases for unions, tuples, and more complex type expressions.

# 🗁 Practical Example: User Management

Interfaces are useful for modeling entities in application architecture.

```typescript
interface Address {
  city: string;
  country: string;
}

interface User {
  readonly id: number;
  name: string;
  email: string;
  address: Address;
  roles: string[];
  active: boolean;
}

interface CreateUserInput {
  name: string;
  email: string;
  address: Address;
}

function createUser(
  input: CreateUserInput,
): User {
  return {
    id: Date.now(),
    name: input.name,
    email: input.email,
    address: input.address,
    roles: ["user"],
    active: true,
  };
}

const user = createUser({
  name: "Alice",
  email: "alice@example.com",
  address: {
    city: "Dhaka",
    country: "Bangladesh",
  },
});
```

This example separates the input contract from the complete user model.

# 🗁 Best Practices

* Use PascalCase for interface names.
* Give properties meaningful and consistent names.
* Prefer reusable interfaces for shared object structures.
* Use optional properties only when values may genuinely be absent.
* Use `readonly` for properties that should not be reassigned.
* Use interface extension to build related object contracts.
* Use generics when an interface should work with multiple data types.
* Use declaration merging carefully to avoid unexpected type changes.
* Prefer specific property types over `any`.
* Keep interfaces focused on a clear responsibility.
* Use runtime validation when accepting external data because interfaces only provide compile-time checks.

# 🗁 Summary

* Interfaces define contracts for object structures.
* They are declared using the `interface` keyword.
* Properties can be required, optional, or readonly.
* Interfaces support methods, function properties, and index signatures.
* Interfaces can describe nested objects and generic structures.
* Interfaces can extend one or multiple interfaces.
* Classes can implement interfaces using `implements`.
* Interfaces support call and construct signatures.
* Declaration merging allows multiple declarations with the same name to combine.
* Interfaces and type aliases both describe object structures but have different capabilities.

# 🗁 Related Topics

* Object Types
* Type Aliases
* Interface Extension
* Interface Merging
* Optional Properties
* Readonly Properties
* Index Signatures
* Function Interfaces
* Generic Interfaces
* Classes and Implements
* Structural Typing
