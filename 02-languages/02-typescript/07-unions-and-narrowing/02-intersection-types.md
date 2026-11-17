# Intersection Types

Intersection types are a TypeScript feature that combines multiple types into a single type.

An intersection type is created using the ampersand (`&`) operator.

It requires a value to satisfy all the combined type requirements.

Intersection types are useful when:

* Combining properties from multiple object types.
* Extending existing type definitions.
* Building reusable application models.
* Composing capabilities from different interfaces.
* Creating complex types from smaller types.

# 🗁 Basic Syntax

Use the ampersand (`&`) operator to combine types.

```typescript
type CombinedType = TypeA & TypeB;
```

Example:

```typescript
type Person = {
  name: string;
};

type Employee = {
  employeeId: number;
};

type EmployeePerson = Person & Employee;
```

A value of `EmployeePerson` must contain the properties required by both `Person` and `Employee`.

```typescript
const employee: EmployeePerson = {
  name: "Alice",
  employeeId: 1001,
};
```

# 🗁 Intersection Types with Object Types

Intersection types are commonly used to combine object types.

```typescript
type User = {
  id: number;
  name: string;
};

type Contact = {
  email: string;
  phone: string;
};

type UserContact = User & Contact;
```

Example:

```typescript
const user: UserContact = {
  id: 1,
  name: "Alice",
  email: "alice@example.com",
  phone: "1234567890",
};
```

The resulting type contains all properties from both types.

# 🗁 Intersection Types with Interfaces

You can combine multiple interfaces using intersection types.

```typescript
interface Person {
  name: string;
  age: number;
}

interface Employee {
  employeeId: number;
  department: string;
}

type EmployeeDetails = Person & Employee;
```

Example:

```typescript
const employee: EmployeeDetails = {
  name: "Alice",
  age: 28,
  employeeId: 1001,
  department: "Engineering",
};
```

This approach allows you to compose existing interface definitions without duplicating their properties.

# 🗁 Intersection Types with Multiple Types

An intersection can combine more than two types.

```typescript
type Person = {
  name: string;
};

type Employee = {
  employeeId: number;
};

type Contact = {
  email: string;
};

type CompleteEmployee = Person & Employee & Contact;
```

Example:

```typescript
const employee: CompleteEmployee = {
  name: "Alice",
  employeeId: 1001,
  email: "alice@example.com",
};
```

Every combined type contributes its requirements.

# 🗁 Intersection Types with Type Aliases

Type aliases make intersection types reusable and easier to maintain.

```typescript
type HasId = {
  id: number;
};

type HasTimestamp = {
  createdAt: Date;
};

type Entity = HasId & HasTimestamp;
```

Use the resulting type in multiple declarations:

```typescript
const user: Entity = {
  id: 1,
  createdAt: new Date(),
};

const product: Entity = {
  id: 2,
  createdAt: new Date(),
};
```

This is useful when several application models share common properties.

# 🗁 Intersection Types with Functions

Intersection types can combine multiple function signatures.

```typescript
type StringHandler = (value: string) => void;

type NumberHandler = (value: number) => void;

type CombinedHandler = StringHandler & NumberHandler;
```

A function assigned to `CombinedHandler` must support both call signatures.

```typescript
const handleValue: CombinedHandler = (
  value: string | number,
): void => {
  console.log(value);
};

handleValue("Hello");
handleValue(100);
```

The function supports both string and number arguments.

# 🗁 Intersection Types with Generic Types

Intersection types can be used with generics to extend existing types.

```typescript
type WithId<T> = T & {
  id: number;
};
```

Example:

```typescript
type User = {
  name: string;
  email: string;
};

type UserWithId = WithId<User>;
```

Create an object:

```typescript
const user: UserWithId = {
  id: 1,
  name: "Alice",
  email: "alice@example.com",
};
```

The generic type preserves the original properties and adds an `id` property.

# 🗁 Intersection Types with Type Extension

Intersection types provide another way to compose existing types.

Using an intersection:

```typescript
type Person = {
  name: string;
};

type Employee = Person & {
  employeeId: number;
};
```

Using interface extension:

```typescript
interface Person {
  name: string;
}

interface Employee extends Person {
  employeeId: number;
}
```

Both approaches can combine object properties.

However, they are not identical:

| Feature | Intersection Types | Interface Extension |
| --- | --- | --- |
| Syntax | `&` | `extends` |
| Works with type aliases | Yes | No, only object-like types and interfaces |
| Combines multiple types | Yes | Supports multiple interface bases |
| Conflicting properties | May produce impossible types | Usually reports an extension error |
| Produces a new combined type | Yes | Declares an extended interface |

Choose the approach that best represents the intended type relationship.

# 🗁 Intersection Types with Conflicting Properties

When two types contain the same property with incompatible types, an intersection can produce an impossible property type.

Example:

```typescript
type StringValue = {
  value: string;
};

type NumberValue = {
  value: number;
};

type CombinedValue = StringValue & NumberValue;
```

The resulting `value` property must satisfy both `string` and `number`.

```typescript
const result: CombinedValue = {
  // Error:
  // No value can satisfy both string and number.
  value: "Hello",
};
```

The effective type of `value` is:

```typescript
string & number
```

This reduces to `never` because a value cannot simultaneously be a string and a number.

A compatible example:

```typescript
type First = {
  id: number;
  name: string;
};

type Second = {
  id: number;
  email: string;
};

type Combined = First & Second;
```

Both types define `id` as a number, so the intersection is valid.

# 🗁 Intersection Types with Optional Properties

Intersection types can combine required and optional properties.

```typescript
type Person = {
  name: string;
};

type Contact = {
  email?: string;
};

type User = Person & Contact;
```

Example:

```typescript
const user: User = {
  name: "Alice",
};

const anotherUser: User = {
  name: "Bob",
  email: "bob@example.com",
};
```

The `name` property is required, while `email` is optional.

# 🗁 Intersection Types with Readonly Properties

Intersection types preserve readonly restrictions from their constituent types.

```typescript
type Identity = {
  readonly id: number;
};

type Profile = {
  name: string;
};

type User = Identity & Profile;
```

Example:

```typescript
const user: User = {
  id: 1,
  name: "Alice",
};

// Error:
// user.id = 2;
```

The `id` property remains readonly in the resulting type.

# 🗁 Intersection Types with Union Types

You can combine intersection and union types to create more expressive type definitions.

```typescript
type Admin = {
  role: "admin";
  permissions: string[];
};

type Customer = {
  role: "customer";
  shippingAddress: string;
};

type User = {
  id: number;
  name: string;
};

type Account = User & (Admin | Customer);
```

An `Account` must contain the properties of `User` and must also satisfy either `Admin` or `Customer`.

Example:

```typescript
const admin: Account = {
  id: 1,
  name: "Alice",
  role: "admin",
  permissions: ["create", "delete"],
};

const customer: Account = {
  id: 2,
  name: "Bob",
  role: "customer",
  shippingAddress: "Dhaka",
};
```

This pattern is useful when several variants share common properties.

# 🗁 Practical Example: User Profile Composition

Suppose an application has several reusable user-related models.

```typescript
type BasicInfo = {
  id: number;
  name: string;
};

type ContactInfo = {
  email: string;
  phone: string;
};

type AddressInfo = {
  city: string;
  country: string;
};

type UserProfile = BasicInfo & ContactInfo & AddressInfo;
```

Create a complete user profile:

```typescript
const user: UserProfile = {
  id: 1,
  name: "Alice",
  email: "alice@example.com",
  phone: "1234567890",
  city: "Dhaka",
  country: "Bangladesh",
};
```

Each type represents one responsibility, and the intersection combines them into a complete model.

# 🗁 Practical Example: API Entity

A backend application may use common metadata across different entities.

```typescript
type BaseEntity = {
  id: number;
  createdAt: Date;
  updatedAt: Date;
};

type User = {
  name: string;
  email: string;
};

type Product = {
  title: string;
  price: number;
};

type UserEntity = BaseEntity & User;

type ProductEntity = BaseEntity & Product;
```

Create entities:

```typescript
const user: UserEntity = {
  id: 1,
  createdAt: new Date(),
  updatedAt: new Date(),
  name: "Alice",
  email: "alice@example.com",
};

const product: ProductEntity = {
  id: 101,
  createdAt: new Date(),
  updatedAt: new Date(),
  title: "Mechanical Keyboard",
  price: 75,
};
```

This reduces duplication and keeps shared metadata consistent.

# 🗁 Practical Example: Adding Capabilities

Intersection types can compose different capabilities into a single object.

```typescript
type CanRead = {
  read(): void;
};

type CanWrite = {
  write(data: string): void;
};

type CanDelete = {
  delete(id: number): void;
};

type FileManager = CanRead & CanWrite & CanDelete;
```

Implement the combined type:

```typescript
const fileManager: FileManager = {
  read() {
    console.log("Reading file...");
  },

  write(data) {
    console.log(`Writing: ${data}`);
  },

  delete(id) {
    console.log(`Deleting file: ${id}`);
  },
};
```

The resulting object must implement all three capabilities.

# 🗁 Intersection Types vs Union Types

Intersection and union types combine types in different ways.

**Intersection (`&`)**

A value must satisfy all combined type requirements.

```typescript
type Person = {
  name: string;
};

type Employee = {
  employeeId: number;
};

type EmployeePerson = Person & Employee;

const employee: EmployeePerson = {
  name: "Alice",
  employeeId: 1001,
};
```

**Union (`|`)**

A value must satisfy at least one member of the union.

```typescript
type StringOrNumber = string | number;

let value: StringOrNumber;

value = "Hello";
value = 100;
```

| Feature | Intersection (`&`) | Union (`\|`) |
| --- | --- | --- |
| Meaning | All combined requirements | One of the alternatives |
| Main purpose | Compose types | Represent alternatives |
| Object properties | Combines requirements | Only common properties are directly accessible |
| Typical use | Shared capabilities | Different object variants |

# 🗁 Common Mistakes

**Confusing intersection with union**

```typescript
type Value = string & number;
```

This does not mean a value can be either a string or a number.

Use a union for alternatives:

```typescript
type Value = string | number;
```

**Combining incompatible properties**

```typescript
type First = {
  id: string;
};

type Second = {
  id: number;
};

type Combined = First & Second;
```

The resulting `id` property becomes impossible to satisfy.

Use compatible property types or redesign the type definitions.

**Assuming an intersection makes properties optional**

```typescript
type User = {
  name: string;
};

type Contact = {
  email: string;
};

type UserContact = User & Contact;
```

Both properties are required.

If a property should be optional, explicitly declare it as optional.

```typescript
type Contact = {
  email?: string;
};
```

**Overusing complex intersections**

```typescript
type Complex =
  TypeA &
  TypeB &
  TypeC &
  TypeD &
  TypeE;
```

Excessively complicated compositions can make types difficult to understand.

Create meaningful intermediate aliases when necessary.

# 🗁 Best Practices

* Use intersection types to compose related object structures.
* Keep individual types small and focused.
* Use descriptive type aliases for complex intersections.
* Reuse common properties through shared types.
* Ensure overlapping properties have compatible types.
* Avoid intersections that produce `never` unintentionally.
* Use interfaces with `extends` when defining clear object hierarchies.
* Use intersections when composing capabilities or combining existing type aliases.
* Combine unions and intersections carefully to model complex application data.
* Keep type definitions readable and maintainable.

# 🗁 Summary

* Intersection types combine multiple types using the `&` operator.
* A value must satisfy all combined type requirements.
* They are commonly used with object types, interfaces, and type aliases.
* Multiple types can be combined into one type.
* Generic intersection patterns allow reusable type composition.
* Conflicting property types can produce `never`.
* Intersection types preserve optional and readonly property restrictions.
* Unions and intersections can be combined for complex models.
* Intersection types are useful for reusable entities, API models, and capability composition.

# 🗁 Related Topics

* Union Types
* Type Narrowing
* Type Aliases
* Object Types
* Interfaces
* Interface Extension
* Generic Types
* Conditional Types
* Mapped Types
* Discriminated Unions
