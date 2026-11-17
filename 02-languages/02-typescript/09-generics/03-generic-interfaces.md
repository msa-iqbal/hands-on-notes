# Generic Interfaces

Generic interfaces are TypeScript interfaces that accept type parameters, allowing the same interface to describe different data types while maintaining type safety.

Instead of creating separate interfaces for strings, numbers, objects, or other structures, you can define one reusable interface and provide the required type when using it.

Generic interfaces are useful for:

* Defining reusable data structures.
* Creating type-safe API response contracts.
* Describing repositories and service layers.
* Defining collections and containers.
* Modeling key-value relationships.
* Creating reusable callback contracts.
* Building flexible application architectures.

# 🗁 Basic Syntax

A generic interface declares one or more type parameters inside angle brackets.

```typescript
interface InterfaceName<T> {
  property: T;
}
```

Example:

```typescript
interface Box<T> {
  value: T;
}
```

Here:

* `Box` is the interface name.
* `T` is a generic type parameter.
* `value: T` means the property uses the supplied type.

The interface does not specify a concrete type until it is used.

# 🗁 Using Generic Interfaces

Provide a type argument when declaring a variable with a generic interface.

```typescript
interface Box<T> {
  value: T;
}

const stringBox: Box<string> = {
  value: "Hello",
};

const numberBox: Box<number> = {
  value: 100,
};

const booleanBox: Box<boolean> = {
  value: true,
};
```

Each variable uses the same interface with a different type argument.

Equivalent expanded types:

```typescript
type StringBox = {
  value: string;
};

type NumberBox = {
  value: number;
};

type BooleanBox = {
  value: boolean;
};
```

# 🗁 Generic Interfaces with Multiple Properties

A generic interface can use the same type parameter across multiple properties.

```typescript
interface User<T> {
  id: T;
  referenceId: T;
  name: string;
}
```

Usage:

```typescript
const user: User<number> = {
  id: 101,
  referenceId: 5001,
  name: "Alice",
};
```

Both `id` and `referenceId` must use the same type.

Another example:

```typescript
const employee: User<string> = {
  id: "EMP-101",
  referenceId: "REF-5001",
  name: "Bob",
};
```

The interface supports both numeric and string identifiers.

# 🗁 Generic Interfaces with Multiple Type Parameters

An interface can declare multiple independent type parameters.

```typescript
interface Pair<T, U> {
  first: T;
  second: U;
}
```

Usage:

```typescript
const user: Pair<string, number> = {
  first: "Alice",
  second: 25,
};

const result: Pair<boolean, string> = {
  first: true,
  second: "Success",
};
```

The first and second properties can have different types.

Another example:

```typescript
interface KeyValue<K, V> {
  key: K;
  value: V;
}
```

Usage:

```typescript
const entry: KeyValue<string, number> = {
  key: "age",
  value: 25,
};
```

This pattern is useful for dictionaries, maps, caches, and configuration structures.

# 🗁 Generic Interfaces with Optional Properties

Generic interfaces can define optional properties.

```typescript
interface ApiResult<T> {
  data?: T;
  message?: string;
  success: boolean;
}
```

Usage:

```typescript
const successResult: ApiResult<string> = {
  success: true,
  data: "Operation completed",
};

const errorResult: ApiResult<string> = {
  success: false,
  message: "Something went wrong",
};
```

The `data` property uses the supplied generic type, while the other properties remain fixed.

# 🗁 Generic Interfaces with Readonly Properties

Generic interfaces can define readonly properties.

```typescript
interface RecordItem<T> {
  readonly id: number;
  readonly value: T;
}
```

Usage:

```typescript
const record: RecordItem<string> = {
  id: 1,
  value: "Hello",
};
```

Attempting to modify a readonly property produces a TypeScript error:

```typescript
// TypeScript error
// record.id = 2;

// TypeScript error
// record.value = "Updated";
```

Readonly behavior applies regardless of the supplied generic type.

# 🗁 Generic Interfaces with Methods

An interface can define methods that use its generic type parameter.

```typescript
interface Repository<T> {
  getAll(): T[];
  getById(id: number): T | undefined;
  save(item: T): void;
  delete(id: number): boolean;
}
```

The interface describes a repository that works with a particular data type.

Define a user model:

```typescript
interface User {
  id: number;
  name: string;
}
```

Implement the repository:

```typescript
class UserRepository implements Repository<User> {
  private users: User[] = [];

  getAll(): User[] {
    return this.users;
  }

  getById(id: number): User | undefined {
    return this.users.find((user) => user.id === id);
  }

  save(item: User): void {
    this.users.push(item);
  }

  delete(id: number): boolean {
    const index = this.users.findIndex(
      (user) => user.id === id,
    );

    if (index === -1) {
      return false;
    }

    this.users.splice(index, 1);
    return true;
  }
}
```

The same repository contract can be implemented for products, employees, or other models.

# 🗁 Generic Interfaces with Function Properties

An interface can describe a function using a generic type parameter.

```typescript
interface Transformer<T> {
  transform(value: T): T;
}
```

Implementation:

```typescript
const stringTransformer: Transformer<string> = {
  transform(value) {
    return value.toUpperCase();
  },
};

const numberTransformer: Transformer<number> = {
  transform(value) {
    return value * 2;
  },
};
```

The function parameter and return type are both determined by the supplied type argument.

You can also define a generic function signature directly inside an interface:

```typescript
interface Identity {
  <T>(value: T): T;
}
```

Implementation:

```typescript
const identity: Identity = (value) => {
  return value;
};

const name = identity("Alice");
const age = identity(25);
```

The difference is important:

* `Transformer<T>` specializes the interface for one type.
* `Identity` describes a generic function that can choose a type argument for each call.

# 🗁 Generic Interfaces with Callbacks

Generic interfaces can define callback contracts.

```typescript
interface EventHandler<T> {
  handle(event: T): void;
}
```

Define an event:

```typescript
interface UserEvent {
  userId: number;
  action: string;
}
```

Implement the handler:

```typescript
const userEventHandler: EventHandler<UserEvent> = {
  handle(event) {
    console.log(event.userId);
    console.log(event.action);
  },
};
```

The callback receives the correct event type automatically.

This pattern is useful for event systems, message handlers, and application services.

# 🗁 Generic Interfaces with Arrays

Generic interfaces can describe collections of values.

```typescript
interface Collection<T> {
  items: T[];
  add(item: T): void;
  getAll(): T[];
}
```

Implementation:

```typescript
class DataCollection<T> implements Collection<T> {
  items: T[] = [];

  add(item: T): void {
    this.items.push(item);
  }

  getAll(): T[] {
    return this.items;
  }
}
```

Usage:

```typescript
const names = new DataCollection<string>();

names.add("Alice");
names.add("Bob");

const numbers = new DataCollection<number>();

numbers.add(10);
numbers.add(20);
```

Each collection preserves its own element type.

# 🗁 Generic Interfaces with Constraints

Generic interfaces can restrict their type parameters using `extends`.

```typescript
interface Entity {
  id: number;
}

interface Repository<T extends Entity> {
  findById(id: number): T | undefined;
  save(item: T): void;
}
```

Any type used with `Repository<T>` must contain a compatible `id` property.

Valid model:

```typescript
interface User {
  id: number;
  name: string;
}

type UserRepository = Repository<User>;
```

Invalid model:

```typescript
interface Product {
  name: string;
}

// TypeScript error:
// Product does not satisfy the Entity constraint.
type ProductRepository = Repository<Product>;
```

Generic constraints are useful when an interface requires certain properties or capabilities from its type arguments.

# 🗁 Generic Interfaces with Default Type Parameters

A generic interface can define a default type argument.

```typescript
interface ApiResponse<T = string> {
  success: boolean;
  data: T;
}
```

Using the default type:

```typescript
const response: ApiResponse = {
  success: true,
  data: "Operation completed",
};
```

Using an explicit type:

```typescript
const userResponse: ApiResponse<User> = {
  success: true,
  data: {
    id: 1,
    name: "Alice",
  },
};
```

The default is used when no type argument is supplied.

# 🗁 Generic Interfaces with API Responses

A common real-world use case is defining reusable API response structures.

```typescript
interface ApiResponse<T> {
  success: boolean;
  data: T;
  message: string;
}
```

Define application models:

```typescript
interface User {
  id: number;
  name: string;
  email: string;
}

interface Product {
  id: number;
  title: string;
  price: number;
}
```

Create response types:

```typescript
type UserResponse = ApiResponse<User>;
type ProductResponse = ApiResponse<Product>;

type UserListResponse = ApiResponse<User[]>;
type ProductListResponse = ApiResponse<Product[]>;
```

Example response:

```typescript
const response: UserResponse = {
  success: true,
  data: {
    id: 1,
    name: "Alice",
    email: "alice@example.com",
  },
  message: "User loaded successfully",
};
```

The response structure is reusable, while the `data` property remains specific to each model.

# 🗁 Generic Interfaces with Promises

Interfaces can describe asynchronous operations using `Promise<T>`.

```typescript
interface DataService<T> {
  getById(id: number): Promise<T | undefined>;
  getAll(): Promise<T[]>;
  create(data: T): Promise<T>;
}
```

Implement a service contract for users:

```typescript
interface User {
  id: number;
  name: string;
}

class UserService implements DataService<User> {
  async getById(id: number): Promise<User | undefined> {
    return {
      id,
      name: "Alice",
    };
  }

  async getAll(): Promise<User[]> {
    return [
      {
        id: 1,
        name: "Alice",
      },
    ];
  }

  async create(data: User): Promise<User> {
    return data;
  }
}
```

The generic interface ensures that all asynchronous operations use the same data model consistently.

# 🗁 Extending Generic Interfaces

A generic interface can extend another generic interface.

```typescript
interface Identifiable<T> {
  id: T;
}

interface Timestamped {
  createdAt: Date;
}

interface User<T> extends Identifiable<T>, Timestamped {
  name: string;
}
```

Usage:

```typescript
const user: User<number> = {
  id: 1,
  name: "Alice",
  createdAt: new Date(),
};
```

The resulting interface combines:

* A generic identifier.
* A creation timestamp.
* A fixed string name.

You can also extend a generic interface with a specific type argument.

```typescript
interface StringIdentifiable extends Identifiable<string> {
  name: string;
}
```

Now the identifier must be a string.

# 🗁 Generic Interfaces vs Generic Type Aliases

Both interfaces and type aliases can describe generic structures.

Using an interface:

```typescript
interface Container<T> {
  value: T;
}
```

Using a type alias:

```typescript
type Container<T> = {
  value: T;
};
```

Both can be used similarly:

```typescript
const first: Container<string> = {
  value: "Hello",
};

const second: Container<number> = {
  value: 100,
};
```

Comparison:

| Feature | Generic Interface | Generic Type Alias |
| --- | --- | --- |
| Generic parameters | Yes | Yes |
| Object structure | Yes | Yes |
| Method signatures | Yes | Yes |
| Extending | `extends` | Intersections and other type expressions |
| Declaration merging | Supported | Not supported |
| Union and intersection types | Can participate | Can directly define them |
| Primitive and tuple aliases | Not directly | Yes |

Use interfaces for object contracts that may be extended or declaration-merged. Use type aliases when defining unions, intersections, tuples, or more complex type expressions.

# 🗁 Generic Interfaces vs Generic Functions

Both can use type parameters, but they serve different purposes.

Generic interface:

```typescript
interface Box<T> {
  value: T;
}
```

Generic function:

```typescript
function identity<T>(value: T): T {
  return value;
}
```

Comparison:

| Feature | Generic Interface | Generic Function |
| --- | --- | --- |
| Primary purpose | Describe a reusable contract | Implement reusable behavior |
| Type parameters | Define the structure's types | Define the function's types |
| Used with | Objects, classes, contracts | Function calls |
| Example | `Box<number>` | `identity<number>(10)` |

A generic interface can also describe a generic function, so these features can work together.

# 🗁 Common Mistakes

**Forgetting to provide a required type argument**

```typescript
interface Box<T> {
  value: T;
}

// Depending on the declaration context, TypeScript may require T.
const box: Box<string> = {
  value: "Hello",
};
```

Provide a type argument or define a suitable default type parameter.

**Using the same generic parameter unnecessarily**

```typescript
interface Pair<T> {
  first: T;
  second: T;
}
```

This requires both properties to have the same type.

If they should support independent types, use two parameters:

```typescript
interface Pair<T, U> {
  first: T;
  second: U;
}
```

**Using any instead of a generic parameter**

```typescript
interface Box {
  value: any;
}
```

This removes type safety.

Prefer:

```typescript
interface Box<T> {
  value: T;
}
```

**Assuming a generic interface validates runtime data**

A generic interface only describes compile-time types. It does not validate API responses or other external data at runtime.

**Adding unnecessary generic parameters**

Avoid declaring type parameters that do not affect the interface's properties or methods. Unnecessary parameters make contracts harder to understand.

# 🗁 Best Practices

* Use generic interfaces for reusable object contracts.
* Choose descriptive type parameter names when the structure is complex.
* Use multiple parameters when properties represent independent types.
* Use constraints when the interface requires specific properties.
* Use default type parameters when a meaningful default exists.
* Prefer `readonly` properties when consumers should not modify data.
* Use generic interfaces for repositories, services, API responses, and collections.
* Extend existing interfaces to reuse common contracts.
* Avoid `any` when a generic parameter can preserve type information.
* Keep interface contracts focused and predictable.

# 🗁 Summary

* Generic interfaces describe reusable structures using type parameters.
* They can contain properties, methods, callbacks, and function signatures.
* They support multiple generic parameters.
* They can define optional and readonly properties.
* They support constraints and default type arguments.
* They can extend other generic and non-generic interfaces.
* They are useful for API responses, repositories, services, and collections.
* They preserve type safety while reducing duplicated interface definitions.

# 🗁 Related Topics

* Generic Basics
* Generic Functions
* Generic Classes
* Generic Constraints
* Generic Defaults
* Type Aliases
* Interface Extension
* Interface Merging
* Function Types
* keyof Operator
* Indexed Access Types
* Promise
* Utility Types
