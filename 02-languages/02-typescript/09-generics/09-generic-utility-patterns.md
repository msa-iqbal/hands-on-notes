# Generic Utility Patterns

Generic utility patterns are reusable TypeScript techniques that combine generics with other type-system features to solve common programming problems.

They allow developers to create flexible, type-safe abstractions without repeatedly writing the same type definitions.

Instead of building separate functions or interfaces for every data type, generic utility patterns allow one implementation to work with many different types while preserving their relationships.

They are commonly used in:

* API clients and response handling.
* Repository and data-access layers.
* State management.
* Form handling.
* Object manipulation.
* Event systems.
* Reusable collections.
* Validation libraries.
* Service and application architecture.
* Framework and library development.

# 🗁 Generic Identity Pattern

The identity pattern accepts a value and returns the same value without changing its type.

```typescript
function identity<T>(value: T): T {
  return value;
}
```

Usage:

```typescript
const name = identity("Alice");
const age = identity(25);
const active = identity(true);
```

Inferred types:

```typescript
// name: string
// age: number
// active: boolean
```

The same function works with different types while preserving the original type.

This pattern is useful when building generic wrappers, transformations, and utility functions.

# 🗁 Generic Container Pattern

A generic container stores a value without knowing its concrete type in advance.

```typescript
interface Container<T> {
  value: T;
}
```

Usage:

```typescript
const stringContainer: Container<string> = {
  value: "Hello",
};

const numberContainer: Container<number> = {
  value: 100,
};
```

You can also create a reusable class.

```typescript
class Storage<T> {
  constructor(private value: T) {}

  getValue(): T {
    return this.value;
  }

  setValue(value: T): void {
    this.value = value;
  }
}
```

Usage:

```typescript
const nameStorage = new Storage("Alice");

nameStorage.setValue("Bob");

const name = nameStorage.getValue();
```

The generic parameter ensures that the stored value and returned value have consistent types.

# 🗁 Generic Repository Pattern

The repository pattern provides a consistent interface for accessing and managing data.

A generic repository can work with different entity types.

```typescript
interface Entity {
  id: number;
}

interface Repository<T extends Entity> {
  create(item: T): void;
  findById(id: number): T | undefined;
  findAll(): T[];
  delete(id: number): boolean;
}
```

Implementation:

```typescript
class InMemoryRepository<T extends Entity>
  implements Repository<T> {
  private items: T[] = [];

  create(item: T): void {
    this.items.push(item);
  }

  findById(id: number): T | undefined {
    return this.items.find((item) => item.id === id);
  }

  findAll(): T[] {
    return [...this.items];
  }

  delete(id: number): boolean {
    const index = this.items.findIndex(
      (item) => item.id === id,
    );

    if (index === -1) {
      return false;
    }

    this.items.splice(index, 1);

    return true;
  }
}
```

Define an entity:

```typescript
interface User extends Entity {
  name: string;
  email: string;
}

const users = new InMemoryRepository<User>();

users.create({
  id: 1,
  name: "Alice",
  email: "alice@example.com",
});

const user = users.findById(1);
```

The same repository implementation can support products, orders, customers, and other entities.

# 🗁 Generic API Response Pattern

API responses often follow a common structure even when their data differs.

A generic response type avoids creating separate response interfaces for every endpoint.

```typescript
interface ApiResponse<T> {
  success: boolean;
  data: T;
  message?: string;
}
```

Define a user type:

```typescript
interface User {
  id: number;
  name: string;
  email: string;
}
```

Use the generic response:

```typescript
type UserResponse = ApiResponse<User>;
```

For a collection:

```typescript
type UsersResponse = ApiResponse<User[]>;
```

For a product:

```typescript
interface Product {
  id: number;
  title: string;
  price: number;
}

type ProductResponse = ApiResponse<Product>;
```

You can also provide a default type.

```typescript
interface ApiResponse<T = unknown> {
  success: boolean;
  data: T;
  message?: string;
}
```

Using `unknown` is useful when the response structure has not yet been established.

# 🗁 Generic Result Pattern

A result type represents either a successful operation or a failed operation.

A discriminated union is a natural way to model this.

```typescript
type Result<T, E = Error> =
  | {
      success: true;
      data: T;
    }
  | {
      success: false;
      error: E;
    };
```

Usage:

```typescript
function getUser(id: number): Result<User> {
  if (id <= 0) {
    return {
      success: false,
      error: new Error("Invalid user ID"),
    };
  }

  return {
    success: true,
    data: {
      id,
      name: "Alice",
      email: "alice@example.com",
    },
  };
}
```

Handle the result:

```typescript
const result = getUser(1);

if (result.success) {
  console.log(result.data.name);
} else {
  console.error(result.error.message);
}
```

The `success` property allows TypeScript to narrow the union automatically.

# 🗁 Generic Pagination Pattern

Pagination is a common requirement in APIs and data-driven applications.

A generic pagination type can represent paginated data of any kind.

```typescript
interface PaginatedResponse<T> {
  data: T[];
  page: number;
  limit: number;
  total: number;
  totalPages: number;
}
```

Usage:

```typescript
const usersPage: PaginatedResponse<User> = {
  data: [
    {
      id: 1,
      name: "Alice",
      email: "alice@example.com",
    },
  ],
  page: 1,
  limit: 10,
  total: 50,
  totalPages: 5,
};
```

The same structure can represent other resources.

```typescript
const productsPage: PaginatedResponse<Product> = {
  data: [
    {
      id: 101,
      title: "Keyboard",
      price: 75,
    },
  ],
  page: 1,
  limit: 10,
  total: 20,
  totalPages: 2,
};
```

# 🗁 Generic Collection Pattern

A collection abstraction provides reusable operations for a group of values.

```typescript
class Collection<T> {
  constructor(private items: T[] = []) {}

  add(item: T): void {
    this.items.push(item);
  }

  remove(item: T): void {
    this.items = this.items.filter(
      (current) => current !== item,
    );
  }

  getAll(): readonly T[] {
    return this.items;
  }

  find(
    predicate: (item: T) => boolean,
  ): T | undefined {
    return this.items.find(predicate);
  }

  count(): number {
    return this.items.length;
  }
}
```

Usage:

```typescript
const names = new Collection<string>();

names.add("Alice");
names.add("Bob");

const result = names.find(
  (name) => name === "Alice",
);
```

A different collection can store objects:

```typescript
const users = new Collection<User>();

users.add({
  id: 1,
  name: "Alice",
  email: "alice@example.com",
});
```

The collection operations remain reusable across types.

# 🗁 Generic Key-Value Pattern

A generic key-value structure can represent dictionaries, caches, and lookup tables.

```typescript
class Dictionary<K extends PropertyKey, V> {
  private items = new Map<K, V>();

  set(key: K, value: V): void {
    this.items.set(key, value);
  }

  get(key: K): V | undefined {
    return this.items.get(key);
  }

  has(key: K): boolean {
    return this.items.has(key);
  }

  delete(key: K): boolean {
    return this.items.delete(key);
  }
}
```

Usage:

```typescript
const userDictionary = new Dictionary<number, User>();

userDictionary.set(1, {
  id: 1,
  name: "Alice",
  email: "alice@example.com",
});

const user = userDictionary.get(1);
```

Here:

* `K` represents the key type.
* `V` represents the value type.
* `PropertyKey` restricts keys to `string | number | symbol`.

The `Map` implementation preserves the key and value types.

# 🗁 Generic Event Handler Pattern

Generic event handlers can preserve event payload types.

```typescript
type EventHandler<T> = (payload: T) => void;
```

Define event payloads:

```typescript
interface UserCreatedEvent {
  userId: number;
  name: string;
}

interface OrderCreatedEvent {
  orderId: number;
  total: number;
}
```

Create handlers:

```typescript
const userCreatedHandler: EventHandler<UserCreatedEvent> = (
  event,
) => {
  console.log(event.userId);
  console.log(event.name);
};

const orderCreatedHandler: EventHandler<OrderCreatedEvent> = (
  event,
) => {
  console.log(event.orderId);
  console.log(event.total);
};
```

Each handler receives the correct payload type.

# 🗁 Generic Event Emitter Pattern

An event emitter can use a generic event map to associate event names with their payload types.

```typescript
type EventMap = {
  [eventName: string]: unknown;
};

class EventEmitter<Events extends EventMap> {
  private handlers = new Map<
    keyof Events,
    Array<(payload: never) => void>
  >();

  on<K extends keyof Events>(
    event: K,
    handler: (payload: Events[K]) => void,
  ): void {
    const handlers = this.handlers.get(event) ?? [];

    handlers.push(
      handler as (payload: never) => void,
    );

    this.handlers.set(event, handlers);
  }

  emit<K extends keyof Events>(
    event: K,
    payload: Events[K],
  ): void {
    const handlers = this.handlers.get(event) ?? [];

    for (const handler of handlers) {
      handler(payload as never);
    }
  }
}
```

Define the event map:

```typescript
interface AppEvents {
  login: {
    userId: number;
  };

  message: {
    text: string;
    sender: string;
  };
}
```

Usage:

```typescript
const emitter = new EventEmitter<AppEvents>();

emitter.on("login", (event) => {
  console.log(event.userId);
});

emitter.on("message", (event) => {
  console.log(event.text);
  console.log(event.sender);
});

emitter.emit("login", {
  userId: 1,
});

emitter.emit("message", {
  text: "Hello",
  sender: "Alice",
});
```

Invalid event names and incompatible payloads are rejected by TypeScript.

The internal assertions bridge the generic event map to the storage representation. In production libraries, event handler storage should be encapsulated carefully and tested for runtime behavior.

# 🗁 Generic Object Property Pattern

You can create reusable functions for reading object properties.

```typescript
function getProperty<T, K extends keyof T>(
  object: T,
  key: K,
): T[K] {
  return object[key];
}
```

Usage:

```typescript
const user = {
  id: 1,
  name: "Alice",
  active: true,
};

const id = getProperty(user, "id");
const name = getProperty(user, "name");
const active = getProperty(user, "active");
```

The return type depends on the selected key.

You can also create a generic setter.

```typescript
function setProperty<T, K extends keyof T>(
  object: T,
  key: K,
  value: T[K],
): void {
  object[key] = value;
}
```

Usage:

```typescript
setProperty(user, "name", "Bob");
setProperty(user, "active", false);
```

The setter prevents assigning a value that does not match the selected property's type.

# 🗁 Generic Object Transformation Pattern

Mapped types and generics allow you to transform object properties.

For example, create a type that makes every property optional.

```typescript
type Optional<T> = {
  [K in keyof T]?: T[K];
};
```

Usage:

```typescript
interface User {
  id: number;
  name: string;
  email: string;
}

type OptionalUser = Optional<User>;
```

Result:

```typescript
type OptionalUser = {
  id?: number;
  name?: string;
  email?: string;
};
```

Create a readonly transformation:

```typescript
type Immutable<T> = {
  readonly [K in keyof T]: T[K];
};
```

These patterns are the foundation of TypeScript's mapped utility types.

# 🗁 Generic Filter Pattern

A generic filter function can work with any array element type.

```typescript
function filterItems<T>(
  items: readonly T[],
  predicate: (item: T) => boolean,
): T[] {
  return items.filter(predicate);
}
```

Usage with numbers:

```typescript
const numbers = [1, 2, 3, 4, 5];

const evenNumbers = filterItems(
  numbers,
  (number) => number % 2 === 0,
);
```

Usage with objects:

```typescript
const activeUsers = filterItems(
  users,
  (user) => user.active,
);
```

The callback receives the correct element type automatically.

# 🗁 Generic Sorting Pattern

You can create a reusable sorting function using a selector.

```typescript
function sortBy<T, K extends keyof T>(
  items: readonly T[],
  key: K,
): T[] {
  return [...items].sort((a, b) => {
    const first = a[key];
    const second = b[key];

    if (first === second) {
      return 0;
    }

    if (first === undefined) {
      return -1;
    }

    if (second === undefined) {
      return 1;
    }

    return first < second ? -1 : 1;
  });
}
```

Usage:

```typescript
const products = [
  {
    id: 1,
    title: "Keyboard",
    price: 75,
  },
  {
    id: 2,
    title: "Mouse",
    price: 25,
  },
];

const sortedProducts = sortBy(products, "price");
```

The key must exist on the object type.

This implementation is suitable for comparable property types such as strings and numbers. For more complex sorting requirements, a comparator function provides greater flexibility.

# 🗁 Generic Factory Pattern

A factory function creates objects or instances while preserving their types.

```typescript
function createObject<T extends object>(value: T): T {
  return { ...value };
}
```

Usage:

```typescript
const user = createObject({
  id: 1,
  name: "Alice",
});
```

The returned object retains its inferred property types.

A constructor-based factory can support classes with different constructor arguments.

```typescript
function createInstance<
  Args extends unknown[],
  Instance
>(
  Constructor: new (...args: Args) => Instance,
  ...args: Args
): Instance {
  return new Constructor(...args);
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

The factory preserves both constructor argument types and the resulting instance type.

# 🗁 Generic Cache Pattern

A cache stores values associated with keys.

```typescript
class Cache<K, V> {
  private items = new Map<K, V>();

  set(key: K, value: V): void {
    this.items.set(key, value);
  }

  get(key: K): V | undefined {
    return this.items.get(key);
  }

  clear(): void {
    this.items.clear();
  }
}
```

Usage:

```typescript
const userCache = new Cache<number, User>();

userCache.set(1, {
  id: 1,
  name: "Alice",
  email: "alice@example.com",
});

const cachedUser = userCache.get(1);
```

The cache can also store different types:

```typescript
const tokenCache = new Cache<string, string>();

tokenCache.set("accessToken", "abc123");
```

The generic parameters ensure consistent key-value relationships.

# 🗁 Generic Form State Pattern

Form handling often requires storing values of different types while preserving their structure.

```typescript
type FormState<T> = {
  values: T;
  errors: Partial<Record<keyof T, string>>;
  touched: Partial<Record<keyof T, boolean>>;
};
```

Define a form model:

```typescript
interface RegistrationForm {
  name: string;
  email: string;
  age: number;
}
```

Create its state:

```typescript
const formState: FormState<RegistrationForm> = {
  values: {
    name: "Alice",
    email: "alice@example.com",
    age: 25,
  },

  errors: {
    email: "Invalid email address",
  },

  touched: {
    name: true,
    email: true,
  },
};
```

The `keyof` operator ensures that error and touched keys correspond to actual form fields.

# 🗁 Generic Promise Pattern

A generic asynchronous function can preserve the type of its resolved value.

```typescript
async function fetchValue<T>(
  value: T,
): Promise<T> {
  return value;
}
```

Usage:

```typescript
const userPromise = fetchValue({
  id: 1,
  name: "Alice",
});

const numberPromise = fetchValue(100);
```

Inferred types:

```typescript
// userPromise: Promise<{ id: number; name: string }>
// numberPromise: Promise<number>
```

You can also use the `Awaited` utility type to extract a promise's resolved value.

```typescript
type UserResult = Awaited<typeof userPromise>;
```

# 🗁 Generic Builder Pattern

A builder allows objects to be constructed incrementally.

```typescript
class QueryBuilder<T extends object> {
  private filters: Partial<T> = {};

  where<K extends keyof T>(
    key: K,
    value: T[K],
  ): this {
    this.filters[key] = value;
    return this;
  }

  build(): Partial<T> {
    return { ...this.filters };
  }
}
```

Usage:

```typescript
interface User {
  id: number;
  name: string;
  active: boolean;
}

const query = new QueryBuilder<User>()
  .where("id", 1)
  .where("active", true)
  .build();
```

The builder restricts property names and values according to the supplied model.

# 🗁 Generic Constraints and Defaults Together

Generic utility patterns often combine constraints and default type parameters.

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

  findById(id: number): T | undefined {
    return this.items.find(
      (item) => item.id === id,
    );
  }
}
```

Usage with the default:

```typescript
const repository = new Repository();
```

Usage with a specific model:

```typescript
interface User extends Entity {
  name: string;
}

const userRepository = new Repository<User>();
```

This pattern provides both flexibility and sensible defaults.

# 🗁 Generic Utility Patterns in Real Applications

Generic patterns become particularly valuable when building application architecture.

| Application Area | Useful Generic Pattern |
| --- | --- |
| API integration | API Response, Result |
| Database access | Repository |
| State management | Container, Store |
| Caching | Cache, Key-Value |
| Event systems | Event Handler, Event Emitter |
| Forms | Form State |
| Collections | Collection, Filter |
| Object manipulation | Property Access, Mapped Types |
| Pagination | Paginated Response |
| Object creation | Factory, Builder |
| Asynchronous operations | Promise Wrapper |

The goal is not to make every function generic. Generics should be introduced when they improve reuse, type safety, or maintainability.

# 🗁 Common Mistakes

**Using any instead of a generic parameter**

```typescript
function identity(value: any): any {
  return value;
}
```

This loses the relationship between the input and output types.

Better:

```typescript
function identity<T>(value: T): T {
  return value;
}
```

**Using overly complicated generics**

A generic abstraction should solve a real problem.

Avoid introducing multiple type parameters when a simple type is sufficient.

**Using type assertions to bypass generic constraints**

```typescript
function getProperty<T>(
  object: T,
  key: string,
) {
  return (object as any)[key];
}
```

Better:

```typescript
function getProperty<T, K extends keyof T>(
  object: T,
  key: K,
): T[K] {
  return object[key];
}
```

**Ignoring runtime behavior**

Generics provide compile-time type checking. They do not validate external API data, enforce database constraints, or guarantee runtime correctness.

**Returning mutable internal collections**

Returning an internal array directly can allow callers to modify internal state.

Prefer a copy or readonly view when encapsulation matters.

```typescript
getAll(): readonly T[] {
  return [...this.items];
}
```

**Creating abstractions without practical value**

Not every repeated function needs a generic class or framework. Prefer the simplest abstraction that preserves the required type relationships.

# 🗁 Best Practices

* Use generics when behavior is independent of a specific data type.
* Preserve relationships between input, output, and callback types.
* Use `extends` when a generic operation requires specific capabilities.
* Combine `keyof` and indexed access types for safe object operations.
* Use discriminated unions for success and failure results.
* Prefer `unknown` over `any` for uncertain data.
* Use generic defaults when there is a sensible common type.
* Keep generic abstractions focused and reusable.
* Avoid unnecessary type parameters and complex assertions.
* Separate compile-time guarantees from runtime validation.
* Test generic utilities with different types and edge cases.
* Design public generic APIs with readable type signatures.

# 🗁 Summary

* Generic utility patterns combine generics with TypeScript's type-system features.
* They make reusable functions, classes, and interfaces possible.
* Common patterns include repositories, containers, caches, collections, factories, and builders.
* `keyof` and indexed access types provide safe object manipulation.
* Discriminated unions provide type-safe result handling.
* Generic constraints enforce required object structures.
* Generic defaults reduce repetitive type arguments.
* Generic abstractions improve maintainability when applied to genuine reuse requirements.
* Good generic design preserves type relationships without introducing unnecessary complexity.

# 🗁 Related Topics

* Generic Basics
* Generic Functions
* Generic Interfaces
* Generic Classes
* Generic Constraints
* Generic Defaults
* keyof Operator
* typeof Operator
* Indexed Access Types
* Mapped Types
* Conditional Types
* Utility Types
* Type Inference
* Structural Typing
* Discriminated Unions
* Repository Pattern
* Factory Pattern
* Builder Pattern
