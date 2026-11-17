# Generic Constraints

Generic constraints allow you to restrict the types that can be supplied to a generic type parameter.

By default, a generic type parameter can represent almost any type. However, some operations require specific properties or capabilities.

For example, a function that reads an object's `length` property cannot safely accept every possible type.

Generic constraints solve this problem by defining requirements that a type must satisfy.

They are useful for:

* Restricting generic function arguments.
* Requiring specific object properties.
* Accessing properties safely inside generic functions.
* Building reusable repositories and services.
* Working with object keys and values.
* Enforcing structural contracts.
* Creating type-safe utility functions.
* Improving generic API design.

# 🗁 Basic Syntax

Use the `extends` keyword to define a generic constraint.

```typescript
function functionName<T extends Constraint>(
  value: T,
): T {
  return value;
}
```

Here:

* `T` is the generic type parameter.
* `extends` introduces the constraint.
* `Constraint` defines the required type or structure.
* The supplied type must satisfy the constraint.

Example:

```typescript
function identity<T extends string>(value: T): T {
  return value;
}
```

Usage:

```typescript
identity("Hello");

// TypeScript error
// identity(100);
```

Only types assignable to `string` are accepted.

# 🗁 The Problem Without Constraints

Consider a generic function that reads the `length` property.

```typescript
function getLength<T>(value: T): number {
  return value.length;
}
```

This produces a TypeScript error because `T` could represent a number, boolean, object, or any other type.

TypeScript cannot guarantee that every possible value has a `length` property.

A generic constraint provides that guarantee.

```typescript
function getLength<T extends { length: number }>(
  value: T,
): number {
  return value.length;
}
```

Usage:

```typescript
getLength("Hello");
getLength([10, 20, 30]);

getLength({
  length: 5,
});
```

Invalid usage:

```typescript
// TypeScript error
// getLength(100);

// TypeScript error
// getLength(true);
```

The function now accepts only types containing a compatible numeric `length` property.

# 🗁 Constraints with Primitive Types

A generic parameter can be constrained to a primitive type.

```typescript
function printValue<T extends string>(value: T): void {
  console.log(value);
}
```

Usage:

```typescript
printValue("Hello");
printValue("TypeScript");

// TypeScript error
// printValue(100);
```

Other examples:

```typescript
function getNumber<T extends number>(value: T): T {
  return value;
}

function getBoolean<T extends boolean>(value: T): T {
  return value;
}
```

The constraint determines which types are accepted.

# 🗁 Constraints with Object Types

You can require an object to contain specific properties.

```typescript
interface User {
  id: number;
  name: string;
}

function displayUser<T extends User>(user: T): void {
  console.log(user.id);
  console.log(user.name);
}
```

Usage:

```typescript
displayUser({
  id: 1,
  name: "Alice",
});

displayUser({
  id: 2,
  name: "Bob",
  email: "bob@example.com",
});
```

The second object is also accepted because it contains all required properties of `User`.

The additional `email` property is allowed because the generic type can preserve the more specific inferred object type.

Invalid usage:

```typescript
// TypeScript error: missing name
// displayUser({
//   id: 1,
// });
```

# 🗁 Constraints with Interfaces

Interfaces are a convenient way to define reusable generic constraints.

```typescript
interface Identifiable {
  id: number;
}

function getId<T extends Identifiable>(item: T): number {
  return item.id;
}
```

Usage:

```typescript
const user = {
  id: 1,
  name: "Alice",
};

const product = {
  id: 101,
  title: "Keyboard",
};

getId(user);
getId(product);
```

Both objects satisfy the `Identifiable` interface.

This pattern is useful when multiple models share common properties.

# 🗁 Constraints with Type Aliases

Type aliases can also define generic constraints.

```typescript
type HasName = {
  name: string;
};

function getName<T extends HasName>(value: T): string {
  return value.name;
}
```

Usage:

```typescript
getName({
  name: "Alice",
});

getName({
  name: "Bob",
  age: 25,
});
```

The function accepts any compatible object containing a string `name` property.

# 🗁 Constraints with keyof

The `keyof` operator can restrict a generic type parameter to the keys of another type.

```typescript
function getProperty<T, K extends keyof T>(
  object: T,
  key: K,
): T[K] {
  return object[key];
}
```

Example:

```typescript
const user = {
  name: "Alice",
  age: 25,
  active: true,
};

const name = getProperty(user, "name");
const age = getProperty(user, "age");
const active = getProperty(user, "active");
```

Inferred types:

```typescript
// name: string
// age: number
// active: boolean
```

Invalid property:

```typescript
// TypeScript error
// getProperty(user, "email");
```

The constraint `K extends keyof T` ensures that the supplied key belongs to the object type.

# 🗁 Constraints with Multiple Type Parameters

A generic function can use one type parameter to constrain another.

```typescript
function getValue<T, K extends keyof T>(
  object: T,
  key: K,
): T[K] {
  return object[key];
}
```

Here:

* `T` represents the object type.
* `K` represents a valid key of `T`.
* `T[K]` represents the selected property's type.

Another example:

```typescript
function createPair<T, U extends T>(
  first: T,
  second: U,
): [T, U] {
  return [first, second];
}
```

Usage:

```typescript
const result = createPair<number, 10>(5, 10);
```

The second type argument must be assignable to the first type argument.

# 🗁 Constraints with Union Types

A generic parameter can be constrained to a union of allowed types.

```typescript
function formatValue<T extends string | number>(
  value: T,
): string {
  return String(value);
}
```

Usage:

```typescript
formatValue("Hello");
formatValue(100);

// TypeScript error
// formatValue(true);
```

The function accepts only strings and numbers.

You can also use union types with other generic structures.

```typescript
type Identifier = string | number;

function findById<T extends Identifier>(id: T): T {
  return id;
}
```

# 🗁 Constraints with Arrays

You can restrict a generic type to an array of a particular element type.

```typescript
function firstElement<T extends unknown[]>(
  items: T,
): T[number] | undefined {
  return items[0];
}
```

Usage:

```typescript
const firstNumber = firstElement([10, 20, 30]);
const firstName = firstElement(["Alice", "Bob"]);
```

The indexed access type `T[number]` represents the type of the array elements.

For functions that only need an array of elements of type `T`, a simpler signature is often preferable:

```typescript
function getFirst<T>(
  items: readonly T[],
): T | undefined {
  return items[0];
}
```

This accepts both mutable arrays and readonly arrays.

# 🗁 Constraints with Constructors

You can constrain a generic parameter to a constructor signature.

```typescript
type Constructor<T> = new (...args: any[]) => T;
```

Create a generic function:

```typescript
function createInstance<T>(
  ConstructorClass: Constructor<T>,
): T {
  return new ConstructorClass();
}
```

Usage:

```typescript
class User {
  name = "Alice";
}

const user = createInstance(User);
```

The function accepts classes with compatible constructors and returns an instance of the corresponding type.

A more flexible version can preserve constructor arguments:

```typescript
type Constructor<T, Args extends unknown[] = unknown[]> =
  new (...args: Args) => T;

function createInstance<T, Args extends unknown[]>(
  ConstructorClass: new (...args: Args) => T,
  ...args: Args
): T {
  return new ConstructorClass(...args);
}
```

Usage:

```typescript
class Product {
  constructor(
    public title: string,
    public price: number,
  ) {}
}

const product = createInstance(
  Product,
  "Keyboard",
  75,
);
```

Both the constructor arguments and resulting instance type are preserved.

# 🗁 Constraints with Generic Interfaces

Generic interfaces can define constraints for their type parameters.

```typescript
interface Entity {
  id: number;
}

interface Repository<T extends Entity> {
  add(item: T): void;
  findById(id: number): T | undefined;
}
```

A model must satisfy the `Entity` contract before it can be used with the repository.

```typescript
interface User {
  id: number;
  name: string;
}

type UserRepository = Repository<User>;
```

This guarantees that every repository entity has an accessible numeric identifier.

# 🗁 Constraints with Generic Classes

Generic classes can also use constraints.

```typescript
interface Identifiable {
  id: number;
}

class Repository<T extends Identifiable> {
  private items: T[] = [];

  add(item: T): void {
    this.items.push(item);
  }

  findById(id: number): T | undefined {
    return this.items.find((item) => item.id === id);
  }
}
```

Usage:

```typescript
interface User {
  id: number;
  name: string;
}

const users = new Repository<User>();

users.add({
  id: 1,
  name: "Alice",
});
```

The class can safely access `item.id` because the constraint guarantees its existence.

# 🗁 Constraints with Default Type Parameters

A generic type parameter can have both a constraint and a default.

```typescript
interface Entity {
  id: number;
}

class Repository<T extends Entity = Entity> {
  private items: T[] = [];

  add(item: T): void {
    this.items.push(item);
  }

  getAll(): T[] {
    return [...this.items];
  }
}
```

Using the default type:

```typescript
const repository = new Repository();
```

Using a specific type:

```typescript
interface User extends Entity {
  name: string;
}

const userRepository = new Repository<User>();
```

The default type must satisfy the constraint.

# 🗁 Constraints with Nested Properties

Constraints can require nested object structures.

```typescript
interface HasAddress {
  address: {
    city: string;
    country: string;
  };
}

function getCity<T extends HasAddress>(value: T): string {
  return value.address.city;
}
```

Usage:

```typescript
const user = {
  name: "Alice",
  address: {
    city: "Dhaka",
    country: "Bangladesh",
  },
};

getCity(user);
```

Invalid structure:

```typescript
// TypeScript error: missing country
// getCity({
//   address: {
//     city: "Dhaka",
//   },
// });
```

The constraint guarantees that the nested properties are available.

# 🗁 Constraints with Function Types

A generic parameter can be constrained to a function signature.

```typescript
function execute<T extends (...args: never[]) => unknown>(
  callback: T,
): ReturnType<T> {
  return callback();
}
```

A simpler and more flexible implementation can use an inferred function signature directly:

```typescript
function execute<Args extends unknown[], Result>(
  callback: (...args: Args) => Result,
  ...args: Args
): Result {
  return callback(...args);
}
```

Usage:

```typescript
const result = execute(
  (name: string, age: number) => {
    return `${name} is ${age} years old`;
  },
  "Alice",
  25,
);
```

The function preserves both the argument types and the return type.

# 🗁 Constraints vs Type Assertions

Generic constraints and type assertions are different mechanisms.

Generic constraint:

```typescript
function getLength<T extends { length: number }>(
  value: T,
): number {
  return value.length;
}
```

Type assertion:

```typescript
function getLength(value: unknown): number {
  return (value as { length: number }).length;
}
```

Comparison:

| Feature | Generic Constraint | Type Assertion |
| --- | --- | --- |
| Purpose | Restricts accepted types | Tells TypeScript to treat a value as a type |
| Type checking | Enforces the required structure | Does not prove the structure exists |
| Runtime validation | No | No |
| Preserves generic type | Yes | Not necessarily |
| Suitable for reusable APIs | Yes | Only when an assertion is justified |

Prefer constraints when designing reusable generic APIs because they enforce requirements at compile time.

# 🗁 Constraints vs Union Types

Both can restrict accepted types, but they express different relationships.

Union constraint:

```typescript
function process<T extends string | number>(
  value: T,
): T {
  return value;
}
```

Structural constraint:

```typescript
function process<T extends { id: number }>(
  value: T,
): T {
  return value;
}
```

The first allows only types assignable to `string` or `number`.

The second allows any type containing a compatible numeric `id` property.

Use union constraints for a fixed set of allowed types and structural constraints for shared capabilities.

# 🗁 Common Mistakes

**Forgetting the extends keyword**

```typescript
function getLength<T>(value: T): number {
  // TypeScript error
  return value.length;
}
```

Correct:

```typescript
function getLength<T extends { length: number }>(
  value: T,
): number {
  return value.length;
}
```

**Using an overly restrictive constraint**

```typescript
function getId<T extends { id: number }>(
  value: T,
): number {
  return value.id;
}
```

This rejects objects whose identifiers are strings.

If both identifier types are valid, use a union:

```typescript
function getId<T extends { id: string | number }>(
  value: T,
): string | number {
  return value.id;
}
```

**Confusing extends with inheritance**

In a generic declaration, `extends` establishes an assignability constraint. It does not mean the supplied type must inherit from a class.

**Assuming constraints modify the original type**

A constraint only limits which types can be supplied. It does not automatically add missing properties to the actual type.

**Using any unnecessarily**

```typescript
function getProperty<T extends any>(
  value: T,
): T {
  return value;
}
```

This constraint provides no meaningful restriction. Remove it:

```typescript
function getProperty<T>(value: T): T {
  return value;
}
```

# 🗁 Best Practices

* Use `extends` to define the minimum structure a generic operation requires.
* Prefer interfaces or type aliases for reusable object constraints.
* Use `keyof` when a parameter must be a valid property key.
* Use structural constraints instead of unnecessary class inheritance.
* Keep constraints as broad as the function's actual requirements allow.
* Use multiple generic parameters when different types have separate responsibilities.
* Preserve the original generic type whenever possible.
* Avoid meaningless constraints such as `T extends any`.
* Do not confuse compile-time constraints with runtime validation.
* Use constraints to make generic functions and classes predictable.

# 🗁 Summary

* Generic constraints restrict the types accepted by generic parameters.
* The `extends` keyword defines a constraint.
* Constraints can use primitive types, unions, interfaces, type aliases, and object structures.
* `keyof` can restrict a parameter to valid object keys.
* Multiple type parameters can have relationships through constraints.
* Generic functions, interfaces, and classes all support constraints.
* Constructor and function signatures can also be used as constraints.
* Constraints provide compile-time guarantees without performing runtime validation.
* Well-designed constraints improve type safety and generic API usability.

# 🗁 Related Topics

* Generic Basics
* Generic Functions
* Generic Interfaces
* Generic Classes
* Generic Defaults
* keyof Operator
* typeof Operator
* Indexed Access Types
* Type Parameters
* Type Assertions
* Structural Typing
* Function Types
* Constructor Types
* Utility Types
