# Generic Classes

Generic classes are TypeScript classes that use type parameters to work with different data types while maintaining type safety.

Instead of creating separate classes for strings, numbers, users, products, or other data structures, you can define one reusable class that accepts a type argument.

Generic classes are useful for:

* Building reusable data containers.
* Implementing stacks, queues, and collections.
* Creating type-safe repositories.
* Managing application state.
* Building reusable storage mechanisms.
* Implementing caching systems.
* Creating data processing utilities.
* Reducing duplicated class implementations.

# 🗁 Basic Syntax

A generic class declares its type parameters after the class name.

```typescript
class ClassName<T> {
  // Class implementation
}
```

Example:

```typescript
class Box<T> {
  value: T;

  constructor(value: T) {
    this.value = value;
  }

  getValue(): T {
    return this.value;
  }
}
```

Here:

* `Box` is the class name.
* `T` is a generic type parameter.
* `value: T` stores a value of type `T`.
* `constructor(value: T)` initializes the value.
* `getValue(): T` returns the same type.

The class can be instantiated with different types.

# 🗁 Creating Generic Class Instances

Provide a type argument when creating an instance.

```typescript
const stringBox = new Box<string>("Hello");
const numberBox = new Box<number>(100);
const booleanBox = new Box<boolean>(true);
```

Each instance has its own specialized type.

```typescript
stringBox.getValue(); // string
numberBox.getValue(); // number
booleanBox.getValue(); // boolean
```

TypeScript also supports type argument inference in many constructor calls.

```typescript
const nameBox = new Box("Alice");
const ageBox = new Box(25);
```

Inferred types:

```typescript
// Box<string>
// Box<number>
```

Prefer inference when it produces the intended type.

# 🗁 Generic Class Properties

A generic class can use its type parameter for multiple properties.

```typescript
class User<T> {
  id: T;
  referenceId: T;
  name: string;

  constructor(id: T, referenceId: T, name: string) {
    this.id = id;
    this.referenceId = referenceId;
    this.name = name;
  }
}
```

Usage:

```typescript
const user = new User<number>(1, 1001, "Alice");

const employee = new User<string>(
  "EMP-101",
  "REF-1001",
  "Bob",
);
```

Both identifiers use the same type parameter.

# 🗁 Generic Class Methods

Methods inside a generic class can use the class-level type parameter.

```typescript
class Storage<T> {
  private value: T;

  constructor(value: T) {
    this.value = value;
  }

  setValue(value: T): void {
    this.value = value;
  }

  getValue(): T {
    return this.value;
  }
}
```

Usage:

```typescript
const storage = new Storage<string>("Initial");

storage.setValue("Updated");

console.log(storage.getValue());
```

The class consistently uses `string` for its stored value.

# 🗁 Generic Classes with Multiple Type Parameters

A class can declare multiple generic parameters.

```typescript
class Pair<T, U> {
  first: T;
  second: U;

  constructor(first: T, second: U) {
    this.first = first;
    this.second = second;
  }

  getPair(): [T, U] {
    return [this.first, this.second];
  }
}
```

Usage:

```typescript
const user = new Pair<string, number>("Alice", 25);

const result = new Pair<boolean, string>(
  true,
  "Success",
);
```

Inferred instance types:

```typescript
// Pair<string, number>
// Pair<boolean, string>
```

Multiple type parameters are useful when a class manages different kinds of data.

# 🗁 Generic Classes with Arrays

Generic classes are commonly used to build reusable collections.

Example:

```typescript
class Collection<T> {
  private items: T[] = [];

  add(item: T): void {
    this.items.push(item);
  }

  getAll(): T[] {
    return [...this.items];
  }

  getCount(): number {
    return this.items.length;
  }

  clear(): void {
    this.items = [];
  }
}
```

Usage:

```typescript
const names = new Collection<string>();

names.add("Alice");
names.add("Bob");

const numbers = new Collection<number>();

numbers.add(10);
numbers.add(20);
numbers.add(30);
```

Each collection maintains its own element type.

Invalid usage:

```typescript
// TypeScript error
// names.add(100);

// TypeScript error
// numbers.add("Hello");
```

# 🗁 Generic Stack Implementation

A stack is a data structure that follows the Last In, First Out (LIFO) principle.

The last element inserted is the first element removed.

A generic stack can work with different data types.

```typescript
class Stack<T> {
  private items: T[] = [];

  push(item: T): void {
    this.items.push(item);
  }

  pop(): T | undefined {
    return this.items.pop();
  }

  peek(): T | undefined {
    return this.items[this.items.length - 1];
  }

  isEmpty(): boolean {
    return this.items.length === 0;
  }

  size(): number {
    return this.items.length;
  }
}
```

Usage:

```typescript
const numberStack = new Stack<number>();

numberStack.push(10);
numberStack.push(20);
numberStack.push(30);

console.log(numberStack.pop());  // 30
console.log(numberStack.peek()); // 20
console.log(numberStack.size()); // 2
```

String stack:

```typescript
const nameStack = new Stack<string>();

nameStack.push("Alice");
nameStack.push("Bob");

console.log(nameStack.pop()); // Bob
```

The same implementation supports multiple types.

# 🗁 Generic Queue Implementation

A queue follows the First In, First Out (FIFO) principle.

The first element inserted is the first element removed.

```typescript
class Queue<T> {
  private items: T[] = [];

  enqueue(item: T): void {
    this.items.push(item);
  }

  dequeue(): T | undefined {
    return this.items.shift();
  }

  peek(): T | undefined {
    return this.items[0];
  }

  isEmpty(): boolean {
    return this.items.length === 0;
  }

  size(): number {
    return this.items.length;
  }
}
```

Usage:

```typescript
const queue = new Queue<string>();

queue.enqueue("First");
queue.enqueue("Second");
queue.enqueue("Third");

console.log(queue.dequeue()); // First
console.log(queue.peek());    // Second
```

This implementation demonstrates generic typing. For high-performance queues with large datasets, consider a data structure that avoids repeatedly shifting array elements.

# 🗁 Generic Classes with Interfaces

A generic class can implement a generic interface.

```typescript
interface Repository<T> {
  add(item: T): void;
  getAll(): T[];
  findById(id: number): T | undefined;
}
```

Define a model:

```typescript
interface User {
  id: number;
  name: string;
}
```

Implement the interface:

```typescript
class UserRepository implements Repository<User> {
  private users: User[] = [];

  add(item: User): void {
    this.users.push(item);
  }

  getAll(): User[] {
    return [...this.users];
  }

  findById(id: number): User | undefined {
    return this.users.find((user) => user.id === id);
  }
}
```

Usage:

```typescript
const repository = new UserRepository();

repository.add({
  id: 1,
  name: "Alice",
});

console.log(repository.getAll());
console.log(repository.findById(1));
```

The interface defines the contract, while the class provides its implementation.

# 🗁 Generic Classes with Constraints

Sometimes a generic class requires every supplied type to contain specific properties.

Use `extends` to define a constraint.

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

Define a model:

```typescript
interface User {
  id: number;
  name: string;
}

interface Product {
  id: number;
  title: string;
}
```

Create instances:

```typescript
const users = new Repository<User>();
const products = new Repository<Product>();
```

Both models satisfy the `Identifiable` constraint.

Invalid model:

```typescript
interface Category {
  title: string;
}

// TypeScript error:
// Category does not contain the required id property.
const categories = new Repository<Category>();
```

Constraints allow generic classes to access properties that are guaranteed to exist.

# 🗁 Generic Classes with Inheritance

A generic class can extend another generic class.

```typescript
class Storage<T> {
  protected value: T;

  constructor(value: T) {
    this.value = value;
  }

  getValue(): T {
    return this.value;
  }
}

class SecureStorage<T> extends Storage<T> {
  getSecureValue(): T {
    return this.value;
  }
}
```

Usage:

```typescript
const storage = new SecureStorage<string>("Secret");

console.log(storage.getValue());
console.log(storage.getSecureValue());
```

The subclass inherits the generic type parameter from the parent class.

You can also specialize the parent class with a concrete type.

```typescript
class StringStorage extends Storage<string> {
  getUppercase(): string {
    return this.value.toUpperCase();
  }
}
```

Now `StringStorage` always works with strings.

# 🗁 Generic Classes with Static Members

Static members belong to the class itself rather than individual instances.

A class-level generic type parameter cannot be used directly by static members because static members are shared across all instances of the class.

Invalid example:

```typescript
class Box<T> {
  value: T;

  constructor(value: T) {
    this.value = value;
  }

  // TypeScript error:
  // static defaultValue: T;
}
```

A static method can declare its own generic type parameter.

```typescript
class Box<T> {
  value: T;

  constructor(value: T) {
    this.value = value;
  }

  static create<U>(value: U): Box<U> {
    return new Box<U>(value);
  }
}
```

Usage:

```typescript
const stringBox = Box.create("Hello");
const numberBox = Box.create(100);
```

The static method uses its own type parameter `U`, independent of the instance-level `T`.

# 🗁 Generic Classes with Readonly Properties

Generic classes can define readonly properties.

```typescript
class ImmutableBox<T> {
  constructor(
    public readonly value: T,
  ) {}

  getValue(): T {
    return this.value;
  }
}
```

Usage:

```typescript
const box = new ImmutableBox<string>("Hello");

console.log(box.value);
```

Attempting to reassign the property produces a TypeScript error:

```typescript
// TypeScript error
// box.value = "Updated";
```

Readonly properties are useful when a value should be assigned during initialization and remain unchanged through that property afterward.

# 🗁 Generic Classes with Default Type Parameters

A generic class can provide a default type parameter.

```typescript
class Container<T = string> {
  value: T;

  constructor(value: T) {
    this.value = value;
  }

  getValue(): T {
    return this.value;
  }
}
```

Using the default type:

```typescript
const name = new Container("Alice");
```

Using an explicit type:

```typescript
const age = new Container<number>(25);
```

The default type is used when TypeScript cannot infer a type argument and none is explicitly provided.

# 🗁 Generic Classes with Multiple Generic Parameters and Defaults

You can combine multiple type parameters with default values.

```typescript
class KeyValueStore<K, V = string> {
  private items = new Map<K, V>();

  set(key: K, value: V): void {
    this.items.set(key, value);
  }

  get(key: K): V | undefined {
    return this.items.get(key);
  }
}
```

Usage:

```typescript
const userStore = new KeyValueStore<number, string>();

userStore.set(1, "Alice");

const statusStore = new KeyValueStore<string>();

statusStore.set("status", "active");
```

The second type parameter defaults to `string` when omitted.

# 🗁 Generic Classes with Promise-Based Methods

A generic class can manage asynchronous operations.

```typescript
class AsyncStorage<T> {
  constructor(private value: T) {}

  async getValue(): Promise<T> {
    return this.value;
  }

  async setValue(value: T): Promise<void> {
    this.value = value;
  }
}
```

Usage:

```typescript
const storage = new AsyncStorage<number>(100);

const value = await storage.getValue();

await storage.setValue(200);
```

The asynchronous methods preserve the generic type throughout the class.

# 🗁 Generic Class Type Inference

TypeScript can infer a generic class's type argument from constructor arguments.

```typescript
class DataHolder<T> {
  constructor(public value: T) {}

  getValue(): T {
    return this.value;
  }
}
```

Inference:

```typescript
const first = new DataHolder("Hello");
const second = new DataHolder(100);
```

Inferred instance types:

```typescript
// DataHolder<string>
// DataHolder<number>
```

Explicit type arguments are still useful when the intended type is broader than the constructor argument.

```typescript
const values = new DataHolder<number | string>(100);

values.value = "Hello";
```

Here, the explicit union type allows both numbers and strings.

# 🗁 Generic Classes vs Generic Interfaces

Both can describe reusable structures, but they serve different purposes.

Generic interface:

```typescript
interface Box<T> {
  value: T;
  getValue(): T;
}
```

Generic class:

```typescript
class DataBox<T> {
  constructor(public value: T) {}

  getValue(): T {
    return this.value;
  }
}
```

Comparison:

| Feature | Generic Interface | Generic Class |
| --- | --- | --- |
| Defines a contract | Yes | Implements behavior |
| Contains runtime implementation | No | Yes |
| Has constructors | No | Yes |
| Supports generic parameters | Yes | Yes |
| Can implement or extend | Yes, through interface inheritance | Yes |
| Creates runtime instances | No | Yes |

Use interfaces to define expected structure and classes when you need actual behavior and instance creation.

# 🗁 Generic Classes vs Generic Functions

Generic functions apply type parameters to individual function calls.

Generic classes apply type parameters to class instances.

Generic function:

```typescript
function identity<T>(value: T): T {
  return value;
}

const name = identity("Alice");
const age = identity(25);
```

Generic class:

```typescript
class Storage<T> {
  constructor(public value: T) {}
}

const nameStorage = new Storage("Alice");
const ageStorage = new Storage(25);
```

Each generic class instance maintains its own selected type.

# 🗁 Common Mistakes

**Using any instead of a generic parameter**

```typescript
class Storage {
  value: any;

  constructor(value: any) {
    this.value = value;
  }
}
```

This removes useful type information.

Prefer:

```typescript
class Storage<T> {
  constructor(public value: T) {}
}
```

**Trying to use a class type parameter in a static property**

```typescript
class Box<T> {
  // TypeScript error
  // static value: T;
}
```

Static members cannot access the class's instance-level generic parameter. Give a static method its own generic parameter when needed.

**Using inconsistent types**

```typescript
class Box<T> {
  constructor(public value: T) {}
}

const box = new Box<number>(100);

// TypeScript error
// box.value = "Hello";
```

The selected type argument must remain consistent.

**Assuming generic classes validate runtime data**

Generic parameters are erased during compilation. They do not automatically validate external data at runtime.

**Creating unnecessary generic parameters**

Do not introduce type parameters when a concrete type provides a clearer and simpler class design.

# 🗁 Best Practices

* Use generic classes for reusable data structures and services.
* Preserve type information across properties and methods.
* Use constraints when the class requires specific properties.
* Prefer interfaces to define contracts that classes implement.
* Use separate type parameters for independent data types.
* Use default type parameters when a sensible default exists.
* Give static methods their own generic parameters when needed.
* Use readonly properties when values should not be reassigned.
* Prefer type inference when it produces the intended type.
* Keep generic class implementations simple and maintainable.

# 🗁 Summary

* Generic classes use type parameters to support multiple data types.
* Type parameters are declared after the class name.
* Properties, constructors, and instance methods can use class-level type parameters.
* A class can have multiple generic parameters.
* Generic classes can implement generic interfaces.
* They support constraints, inheritance, and default type parameters.
* Static members cannot directly use instance-level generic parameters.
* Generic classes are useful for collections, stacks, queues, repositories, and storage systems.
* They provide reusable implementations without sacrificing type safety.

# 🗁 Related Topics

* Generic Basics
* Generic Functions
* Generic Interfaces
* Generic Constraints
* Generic Defaults
* Class Basics
* Class Inheritance
* Access Modifiers
* Static Members
* Readonly Properties
* Interfaces
* Type Parameters
* Promise
* Data Structures
