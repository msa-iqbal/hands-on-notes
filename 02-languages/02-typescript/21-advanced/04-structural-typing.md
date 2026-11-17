# Structural Typing

Structural typing is a type compatibility system in which TypeScript determines whether two types are compatible based on their members rather than their declared names or inheritance relationships.

If an object has all the required properties with compatible types, it can be assigned to another type that expects those properties.

TypeScript primarily uses structural typing instead of nominal typing.

```typescript
type User = {
  id: number;
  name: string;
};

type Employee = {
  id: number;
  name: string;
};

const employee: Employee = {
  id: 1,
  name: "Alice",
};

const user: User = employee; // Valid
```

Although `User` and `Employee` are separate type aliases, their structures are compatible.

# 🗁 Structural Typing vs Nominal Typing

Structural typing checks the shape of a value.

Nominal typing checks whether types have the same declared identity or an explicitly established relationship.

**Structural typing:**

```typescript
type UserId = {
  id: number;
};

type ProductId = {
  id: number;
};

const userId: UserId = { id: 10 };
const productId: ProductId = userId; // Valid
```

Both types have the same structure, so TypeScript considers them compatible.

**Nominal typing concept:**

In a nominal type system, `UserId` and `ProductId` would be incompatible simply because they are different declared types, even though their structures are identical.

TypeScript does not enforce nominal typing by default, but branded types can approximate it.

# 🗁 Compatibility Based on Members

For object types, TypeScript generally checks whether the source contains the required members of the target type.

```typescript
type Person = {
  name: string;
  age: number;
};

const employee = {
  name: "Alice",
  age: 30,
  department: "Engineering",
};

const person: Person = employee; // Valid
```

The source object contains every required property of `Person`.

Its additional `department` property does not prevent assignment.

# 🗁 Extra Properties and Excess Property Checking

Structural compatibility allows a source variable to contain additional properties.

However, TypeScript performs excess property checking on fresh object literals assigned directly to certain object types.

```typescript
type Person = {
  name: string;
  age: number;
};

// Valid: additional property exists on a variable
const employee = {
  name: "Alice",
  age: 30,
  department: "Engineering",
};

const person: Person = employee;
```

Direct object literal assignment behaves differently:

```typescript
type Person = {
  name: string;
  age: number;
};

// Error: department is not declared in Person
const person: Person = {
  name: "Alice",
  age: 30,
  department: "Engineering",
};
```

Excess property checking is an additional validation mechanism, not a replacement for structural typing.

# 🗁 Function Compatibility

Function types are also checked structurally.

A function is generally compatible with another function type when its parameters and return type satisfy the target function signature.

```typescript
type Formatter = (value: string) => string;

const formatName = (value: string): string => {
  return value.trim().toUpperCase();
};

const formatter: Formatter = formatName;
```

The function satisfies the expected signature.

# 🗁 Return Type Compatibility

A function returning a more specific compatible type can often be assigned to a function type expecting a broader return type.

```typescript
type Animal = {
  name: string;
};

type Dog = {
  name: string;
  bark(): void;
};

const createDog = (): Dog => ({
  name: "Buddy",
  bark() {
    console.log("Woof!");
  },
});

const createAnimal: () => Animal = createDog;
```

This is valid because every `Dog` contains the members required by `Animal`.

# 🗁 Class Compatibility

TypeScript also applies structural typing to class instances.

Two classes can be compatible even if neither extends the other.

```typescript
class User {
  constructor(
    public id: number,
    public name: string,
  ) {}
}

class Employee {
  constructor(
    public id: number,
    public name: string,
  ) {}
}

const employee = new Employee(1, "Alice");

const user: User = employee; // Valid
```

Both instances expose the same public structure.

# 🗁 Private and Protected Members

Private and protected class members introduce an important exception to ordinary structural compatibility.

When a target class contains a private or protected member, the corresponding member in the source must originate from the same class declaration.

```typescript
class User {
  private id: number;

  constructor(id: number) {
    this.id = id;
  }
}

class Employee {
  private id: number;

  constructor(id: number) {
    this.id = id;
  }
}

const employee = new Employee(1);

// Error: private members originate from different declarations
const user: User = employee;
```

Although both classes declare a private `id` property, those properties have different origins.

If the classes share a common base class, their inherited private or protected members can satisfy this compatibility requirement.

```typescript
class Entity {
  protected id: number;

  constructor(id: number) {
    this.id = id;
  }
}

class User extends Entity {
  constructor(id: number) {
    super(id);
  }
}

class Employee extends Entity {
  constructor(id: number) {
    super(id);
  }
}

const employee = new Employee(1);

const user: User = employee; // Valid
```

# 🗁 Branded Types and Nominal-Like Behavior

Structural typing can sometimes allow values that should represent different domain concepts to be mixed accidentally.

For example:

```typescript
type UserId = number;
type ProductId = number;

const userId: UserId = 100;
const productId: ProductId = userId; // Valid
```

Both aliases represent `number`, so they are interchangeable.

Branded types introduce a unique marker property to distinguish otherwise identical primitive types.

```typescript
declare const userIdBrand: unique symbol;
declare const productIdBrand: unique symbol;

type UserId = number & {
  readonly [userIdBrand]: "UserId";
};

type ProductId = number & {
  readonly [productIdBrand]: "ProductId";
};
```

Now the types are incompatible:

```typescript
declare const userId: UserId;
declare const productId: ProductId;

const anotherUserId: UserId = userId; // Valid

const anotherProductId: ProductId = productId; // Valid

// Error: UserId is not assignable to ProductId
const invalidProductId: ProductId = userId;
```

Branded types are useful for identifiers, validated values, domain-specific primitives, and preventing accidental mixing of semantically different values.

# 🗁 Structural Typing with Generic Types

Generic types are also compared based on their resulting structures.

```typescript
interface Box<T> {
  value: T;
}

const numberBox: Box<number> = {
  value: 100,
};

const anotherNumberBox: Box<number> = numberBox; // Valid
```

Different type arguments can produce incompatible structures:

```typescript
const stringBox: Box<string> = {
  value: "Hello",
};

// Error: string is not assignable to number
const invalidBox: Box<number> = stringBox;
```

The compatibility depends on the members and how their generic types are instantiated.

# 🗁 Practical Example: Service Contracts

Structural typing makes it possible to write functions that depend on object capabilities rather than concrete classes.

```typescript
type Logger = {
  log(message: string): void;
};

class ConsoleLogger {
  log(message: string): void {
    console.log(message);
  }
}

class FileLogger {
  log(message: string): void {
    console.log(`Writing to file: ${message}`);
  }
}

function processRequest(logger: Logger): void {
  logger.log("Request processed");
}

processRequest(new ConsoleLogger());
processRequest(new FileLogger());
```

Both classes satisfy the `Logger` contract without explicitly implementing an interface.

This supports flexible architecture, dependency inversion, and easier testing.

# 🗁 Best Practices

* Design object types around the properties and behaviors consumers actually need.
* Prefer structural contracts when multiple implementations should satisfy the same requirements.
* Use interfaces or type aliases to describe reusable object shapes.
* Use branded types when structurally identical values must remain distinct.
* Understand excess property checking before interpreting object assignment errors.
* Remember that private and protected class members affect compatibility.
* Avoid relying on class names alone to establish type identity.
* Use narrow contracts to reduce unnecessary coupling between components.

# 🗁 Key Takeaways

* TypeScript primarily uses structural typing.
* Compatibility depends on members rather than declared type names.
* A source object can contain additional properties and still satisfy a target type.
* Fresh object literals receive excess property checking.
* Function signatures and class instances are also checked structurally.
* Private and protected members introduce origin-based compatibility requirements.
* Branded types can provide nominal-like distinctions.
* Structural typing encourages reusable contracts and loosely coupled software architecture.
