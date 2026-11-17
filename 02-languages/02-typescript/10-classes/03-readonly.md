# Readonly Properties

The `readonly` modifier in TypeScript prevents a class property from being reassigned after its initialization.

It is commonly used to protect values that should remain unchanged throughout an object's lifetime.

Readonly properties help enforce immutability at the type-checking level and make class behavior more predictable.

# 🗁 What Is readonly?

The `readonly` modifier marks a class property as read-only.

```typescript
class User {
  readonly id: number;
  name: string;

  constructor(id: number, name: string) {
    this.id = id;
    this.name = name;
  }
}
```

Usage:

```typescript
const user = new User(101, "Alice");

console.log(user.id);

user.name = "Bob"; // Allowed

// Error: Cannot assign to 'id' because it is a read-only property
// user.id = 202;
```

The `id` property can be initialized inside the constructor but cannot be reassigned afterward.

The `name` property remains mutable because it is not marked as readonly.

# 🗁 Readonly Property Initialization

A readonly property can be initialized in two primary ways.

**Initialize during declaration**

```typescript
class Configuration {
  readonly applicationName: string = "My Application";
  readonly version: string = "1.0.0";
}
```

**Initialize inside the constructor**

```typescript
class User {
  readonly id: number;
  readonly username: string;

  constructor(id: number, username: string) {
    this.id = id;
    this.username = username;
  }
}
```

A readonly property without an initializer must be initialized in the constructor when strict property initialization is enabled.

It cannot be assigned later through an ordinary instance method.

```typescript
class User {
  readonly id: number;

  constructor(id: number) {
    this.id = id;
  }

  updateId(newId: number): void {
    // Error: Cannot assign to readonly property
    // this.id = newId;
  }
}
```

# 🗁 Readonly with Type Annotations

Readonly properties can use any valid TypeScript type.

```typescript
class Product {
  readonly id: number;
  readonly title: string;
  readonly price: number;
  readonly available: boolean;

  constructor(
    id: number,
    title: string,
    price: number,
    available: boolean,
  ) {
    this.id = id;
    this.title = title;
    this.price = price;
    this.available = available;
  }
}
```

Readonly does not change the property's type. It only restricts reassignment.

# 🗁 Readonly Arrays as Class Properties

A readonly property prevents reassignment of the property itself, but it does not automatically make an object or array immutable.

Consider:

```typescript
class Team {
  readonly members: string[];

  constructor(members: string[]) {
    this.members = members;
  }
}
```

The following assignment is not allowed:

```typescript
const team = new Team(["Alice", "Bob"]);

// Error: Cannot assign to readonly property
// team.members = ["Charlie"];
```

However, array contents can still be modified:

```typescript
team.members.push("Charlie");

console.log(team.members);
```

Output:

```text
["Alice", "Bob", "Charlie"]
```

To prevent array modification through the property, use `readonly` array types.

```typescript
class Team {
  readonly members: readonly string[];

  constructor(members: readonly string[]) {
    this.members = members;
  }
}
```

Now these operations are rejected:

```typescript
const team = new Team(["Alice", "Bob"]);

// Error: Property 'push' does not exist
// team.members.push("Charlie");

// Error: Index signature is readonly
// team.members[0] = "Charlie";
```

A readonly array prevents modification through that reference. It does not guarantee that another reference cannot modify the same underlying array.

# 🗁 Readonly with Object Properties

Readonly properties containing objects also allow mutation of the object's contents unless those contents are separately restricted.

```typescript
class User {
  readonly profile: {
    name: string;
    age: number;
  };

  constructor(name: string, age: number) {
    this.profile = { name, age };
  }
}
```

Reassigning the object is not allowed:

```typescript
const user = new User("Alice", 25);

// Error: Cannot assign to readonly property
// user.profile = { name: "Bob", age: 30 };
```

But its nested properties remain mutable:

```typescript
user.profile.name = "Bob";
user.profile.age = 30;
```

To make the nested properties readonly, declare them as readonly too.

```typescript
class User {
  readonly profile: {
    readonly name: string;
    readonly age: number;
  };

  constructor(name: string, age: number) {
    this.profile = { name, age };
  }
}
```

Now neither the profile reference nor its declared properties can be reassigned.

# 🗁 Readonly with Access Modifiers

Readonly can be combined with access modifiers.

```typescript
class User {
  public readonly id: number;
  protected readonly role: string;
  private readonly createdAt: Date;

  constructor(id: number, role: string) {
    this.id = id;
    this.role = role;
    this.createdAt = new Date();
  }

  getCreationDate(): Date {
    return this.createdAt;
  }
}
```

The modifiers control accessibility, while `readonly` controls reassignment.

| Declaration | Meaning |
| --- | --- |
| `public readonly` | Publicly accessible but not reassignable |
| `protected readonly` | Accessible in the class hierarchy but not reassignable |
| `private readonly` | Accessible only inside the declaring class and not reassignable |

# 🗁 Readonly with Inheritance

Readonly properties declared in a parent class cannot be reassigned by subclasses.

```typescript
class User {
  readonly id: number;

  constructor(id: number) {
    this.id = id;
  }
}

class Admin extends User {
  constructor(id: number) {
    super(id);

    // Error: Cannot assign to readonly property
    // this.id = 200;
  }
}
```

The parent constructor initializes the property.

The subclass can read the property but cannot reassign it.

# 🗁 Readonly vs const

Both `readonly` and `const` prevent reassignment, but they apply to different declarations.

| Feature | const | readonly |
| --- | --- | --- |
| Applies to | Variables | Properties and type members |
| Prevents variable reassignment | Yes | Not applicable |
| Prevents property reassignment | Not directly | Yes |
| Can initialize in constructor | Not applicable | Yes |
| Can be used with interfaces | No | Yes |

Example:

```typescript
const userId = 101;

// Error: Cannot reassign a constant
// userId = 202;

class User {
  readonly id = 101;
}
```

Use `const` for variables that should not be reassigned.

Use `readonly` for properties that should not be reassigned.

# 🗁 Readonly in Interfaces

The `readonly` modifier can also be used in interfaces.

```typescript
interface User {
  readonly id: number;
  readonly email: string;
  name: string;
}
```

An object implementing this structure cannot reassign its readonly properties through that type.

```typescript
const user: User = {
  id: 101,
  email: "alice@example.com",
  name: "Alice",
};

user.name = "Bob";

// Error: Cannot assign to readonly property
// user.id = 202;
```

This is useful when defining data contracts that should not allow certain values to be changed.

# 🗁 Readonly in Type Aliases

Readonly properties can also be declared in object type aliases.

```typescript
type Product = {
  readonly id: number;
  readonly sku: string;
  title: string;
  price: number;
};
```

Usage:

```typescript
const product: Product = {
  id: 101,
  sku: "KB-001",
  title: "Keyboard",
  price: 75,
};

product.price = 80;

// Error: Cannot assign to readonly property
// product.id = 202;
```

# 🗁 Readonly with Type Assertions

Type assertions can change the type through which a value is viewed.

For example:

```typescript
interface User {
  readonly id: number;
  name: string;
}

const user: User = {
  id: 101,
  name: "Alice",
};

const mutableUser = user as {
  id: number;
  name: string;
};

mutableUser.id = 202;
```

This compiles because the assertion describes a mutable view of the object.

However, it bypasses the original type-level restriction. It does not make the object immutable or provide runtime protection.

Avoid using assertions simply to bypass readonly constraints.

# 🗁 Readonly with Object.freeze()

TypeScript's `readonly` modifier provides compile-time restrictions.

JavaScript's `Object.freeze()` provides runtime protection against changing an object's own properties.

```typescript
const configuration = Object.freeze({
  applicationName: "My Application",
  version: "1.0.0",
});

// Error: Cannot assign to readonly property
// configuration.version = "2.0.0";
```

`Object.freeze()` is shallow.

Nested objects are not automatically frozen.

```typescript
const settings = Object.freeze({
  theme: {
    color: "blue",
  },
});

settings.theme.color = "red"; // Allowed
```

For nested runtime immutability, every relevant nested object must also be frozen or handled by an appropriate deep-freezing strategy.

# 🗁 Practical Example: Product Catalog

```typescript
class Product {
  readonly id: number;
  readonly sku: string;

  title: string;
  price: number;
  stock: number;

  constructor(
    id: number,
    sku: string,
    title: string,
    price: number,
    stock: number,
  ) {
    this.id = id;
    this.sku = sku;
    this.title = title;
    this.price = price;
    this.stock = stock;
  }

  updatePrice(price: number): void {
    if (price < 0) {
      throw new Error("Price cannot be negative");
    }

    this.price = price;
  }

  updateStock(quantity: number): void {
    if (quantity < 0) {
      throw new Error("Stock cannot be negative");
    }

    this.stock = quantity;
  }

  getDetails(): string {
    return `${this.title} - $${this.price}`;
  }
}
```

Usage:

```typescript
const product = new Product(
  101,
  "KB-001",
  "Mechanical Keyboard",
  75,
  20,
);

product.updatePrice(80);
product.updateStock(15);

console.log(product.getDetails());

console.log(product.stock);

// Error: Product ID cannot be reassigned
// product.id = 202;

// Error: SKU cannot be reassigned
// product.sku = "KB-002";
```

The product ID and SKU remain stable, while the title, price, and stock can be updated.

# 🗁 Common Mistakes

**Assuming readonly makes an entire object immutable**

Readonly restricts reassignment of the declared property. Nested objects and arrays can still be mutable.

**Using readonly when runtime immutability is required**

Readonly is primarily a compile-time restriction. Use runtime mechanisms such as `Object.freeze()` when necessary.

**Confusing readonly with const**

`const` applies to variable bindings, while `readonly` applies to properties and type members.

**Expecting readonly to prevent constructor initialization**

A readonly property can be initialized during declaration or assigned in the constructor.

**Assuming readonly prevents mutation through every reference**

Another mutable reference can still modify the underlying object or array.

**Using type assertions to bypass readonly restrictions**

Assertions can weaken type safety without providing any additional runtime guarantees.

# 🗁 Best Practices

* Use readonly for identifiers and values that should remain stable.
* Initialize readonly properties during declaration or in the constructor.
* Combine readonly with access modifiers when appropriate.
* Use readonly array types when array contents should not be modified through a reference.
* Use readonly object properties for immutable data contracts.
* Understand the difference between shallow and deep immutability.
* Use `Object.freeze()` when runtime protection of object properties is required.
* Avoid unnecessary type assertions that bypass readonly constraints.
* Prefer immutable data patterns when predictable state changes are important.

# 🗁 Summary

* `readonly` prevents reassignment of class properties after initialization.
* Readonly properties can be initialized during declaration or in the constructor.
* Readonly can be combined with `public`, `protected`, and `private`.
* Readonly does not automatically make nested objects or arrays immutable.
* Readonly arrays prevent mutation through the readonly array reference.
* Interfaces and type aliases also support readonly properties.
* `const` applies to variables, while `readonly` applies to properties and type members.
* TypeScript readonly restrictions are compile-time checks.
* `Object.freeze()` provides shallow runtime protection.

# 🗁 Related Topics

* Class Basics
* Access Modifiers
* Constructors
* Inheritance
* Abstract Classes
* Implements
* Static Members
* Parameter Properties
* Readonly Arrays
* Utility Types
* Immutability
* Object.freeze()
