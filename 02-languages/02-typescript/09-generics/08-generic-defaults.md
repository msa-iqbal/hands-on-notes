# Generic Defaults

Generic defaults allow you to specify a default type for a generic type parameter.

When a generic type is declared with a default, TypeScript uses that type automatically if the caller does not provide an explicit type argument and cannot infer one from the supplied arguments.

Generic defaults make reusable types, functions, interfaces, and classes easier to use because they reduce the need to repeatedly specify common type arguments.

They are useful for:

* Simplifying generic function calls.
* Defining reusable generic interfaces.
* Creating flexible generic classes.
* Providing default API response types.
* Building reusable collections and repositories.
* Reducing repetitive type annotations.
* Designing developer-friendly libraries.
* Combining defaults with generic constraints.

# 🗁 Basic Syntax

Use the assignment operator (`=`) to specify a default type.

```typescript
type Container<T = string> = {
  value: T;
};
```

Here:

* `T` is a generic type parameter.
* `string` is its default type.
* If no type argument is provided, `T` becomes `string`.

Usage:

```typescript
type StringContainer = Container;

type NumberContainer = Container<number>;
```

Equivalent types:

```typescript
type StringContainer = {
  value: string;
};

type NumberContainer = {
  value: number;
};
```

# 🗁 Generic Defaults with Type Aliases

Generic defaults work with type aliases.

```typescript
type ApiResponse<T = string> = {
  data: T;
  success: boolean;
};
```

Without an explicit type argument:

```typescript
type DefaultResponse = ApiResponse;
```

The default type is used:

```typescript
type DefaultResponse = {
  data: string;
  success: boolean;
};
```

With an explicit type argument:

```typescript
type UserResponse = ApiResponse<{
  id: number;
  name: string;
}>;
```

The supplied type overrides the default.

```typescript
type UserResponse = {
  data: {
    id: number;
    name: string;
  };
  success: boolean;
};
```

# 🗁 Generic Defaults with Multiple Parameters

A generic declaration can contain multiple type parameters, each with its own default.

```typescript
type Result<T = string, E = Error> = {
  data: T;
  error?: E;
};
```

Usage:

```typescript
type DefaultResult = Result;

type NumberResult = Result<number>;

type UserResult = Result<{ id: number; name: string }>;
```

The resulting types are:

```typescript
type DefaultResult = Result<string, Error>;

type NumberResult = Result<number, Error>;

type UserResult = Result<
  { id: number; name: string },
  Error
>;
```

You can also override both parameters:

```typescript
type CustomResult = Result<
  { id: number },
  string
>;
```

# 🗁 Generic Defaults with Functions

Generic functions can define default type parameters.

```typescript
function createValue<T = string>(value?: T): T | undefined {
  return value;
}
```

Usage:

```typescript
const first = createValue();
const second = createValue(100);
const third = createValue("Hello");
```

The inferred types are:

```typescript
// first: string | undefined
// second: number
// third: string
```

When no type argument can be inferred from the arguments, the default is used.

When an argument provides enough information, TypeScript generally infers the type instead of using the default.

You can explicitly specify a type argument:

```typescript
const result = createValue<boolean>(true);
```

# 🗁 Generic Defaults with Interfaces

Generic interfaces can define default type parameters.

```typescript
interface Repository<T = string> {
  items: T[];

  add(item: T): void;
}
```

Using the default:

```typescript
const names: Repository = {
  items: ["Alice", "Bob"],

  add(item) {
    this.items.push(item);
  },
};
```

Using a custom type:

```typescript
interface User {
  id: number;
  name: string;
}

const users: Repository<User> = {
  items: [],

  add(user) {
    this.items.push(user);
  },
};
```

The default makes `Repository` equivalent to `Repository<string>` when no type argument is provided.

# 🗁 Generic Defaults with Classes

Generic classes also support default type parameters.

```typescript
class Storage<T = string> {
  private items: T[] = [];

  add(item: T): void {
    this.items.push(item);
  }

  getAll(): T[] {
    return [...this.items];
  }
}
```

Using the default:

```typescript
const names = new Storage();

names.add("Alice");
names.add("Bob");
```

The class uses `string` as its default type.

Using a custom type:

```typescript
const numbers = new Storage<number>();

numbers.add(10);
numbers.add(20);
```

The explicit type argument overrides the default.

# 🗁 Generic Defaults with Constraints

A generic parameter can have both a constraint and a default.

```typescript
interface Entity {
  id: number;
}

interface Repository<T extends Entity = Entity> {
  items: T[];

  findById(id: number): T | undefined;
}
```

Here:

* `T extends Entity` restricts the allowed types.
* `= Entity` provides the default type.
* Any custom type must satisfy the `Entity` constraint.

Usage:

```typescript
interface User extends Entity {
  name: string;
}

type DefaultRepository = Repository;

type UserRepository = Repository<User>;
```

Invalid type:

```typescript
interface Product {
  title: string;
}

// TypeScript error: Product does not satisfy Entity
// type ProductRepository = Repository<Product>;
```

# 🗁 Generic Defaults with keyof

Generic defaults can be combined with `keyof` constraints.

```typescript
function getProperty<
  T,
  K extends keyof T = keyof T
>(
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

const name = getProperty(user, "name");
const id = getProperty(user, "id");
```

The default for `K` is all keys of `T`.

However, when an argument is provided, TypeScript can infer the more specific key type.

You can also specify only the object type:

```typescript
type User = {
  id: number;
  name: string;
};

type UserKey = keyof User;
```

Generic defaults are especially useful when a type parameter has a sensible fallback.

# 🗁 Generic Defaults with Utility Types

You can combine generic defaults with TypeScript utility types.

```typescript
type PartialObject<T extends object = object> = Partial<T>;
```

Usage:

```typescript
type DefaultObject = PartialObject;

type User = {
  id: number;
  name: string;
};

type PartialUser = PartialObject<User>;
```

Another example using `Record`:

```typescript
type Dictionary<
  K extends PropertyKey = string,
  V = unknown
> = Record<K, V>;
```

Usage:

```typescript
type StringDictionary = Dictionary;

type NumberDictionary = Dictionary<string, number>;

type StatusDictionary = Dictionary<
  "active" | "inactive",
  boolean
>;
```

The defaults provide a convenient starting point while allowing customization.

# 🗁 Generic Defaults with Promise

You can define a generic type that defaults to a common asynchronous result.

```typescript
type AsyncResult<T = void> = Promise<T>;
```

Usage:

```typescript
type DefaultAsyncResult = AsyncResult;

type UserAsyncResult = AsyncResult<{
  id: number;
  name: string;
}>;
```

Equivalent types:

```typescript
type DefaultAsyncResult = Promise<void>;

type UserAsyncResult = Promise<{
  id: number;
  name: string;
}>;
```

This pattern is useful when a generic type has a common default result.

# 🗁 Generic Defaults with React-Style Props

A generic component props type can define a default type for its data.

```typescript
type SelectProps<T = string> = {
  value: T;
  options: T[];
  onChange: (value: T) => void;
};
```

Using the default:

```typescript
const stringSelect: SelectProps = {
  value: "Bangladesh",
  options: ["Bangladesh", "Japan", "Canada"],
  onChange(value) {
    console.log(value);
  },
};
```

Using a custom type:

```typescript
type User = {
  id: number;
  name: string;
};

const userSelect: SelectProps<User> = {
  value: {
    id: 1,
    name: "Alice",
  },

  options: [
    {
      id: 1,
      name: "Alice",
    },
  ],

  onChange(user) {
    console.log(user.name);
  },
};
```

The same generic component contract supports different data types.

# 🗁 Generic Defaults with API Responses

A common practical use case is defining a reusable API response type.

```typescript
interface ApiResponse<T = unknown> {
  success: boolean;
  data: T;
  message?: string;
}
```

For an endpoint returning user information:

```typescript
interface User {
  id: number;
  name: string;
}

type UserResponse = ApiResponse<User>;
```

For an endpoint with no known data structure:

```typescript
type UnknownResponse = ApiResponse;
```

The default `unknown` requires consumers to narrow or validate the data before using it.

This is generally safer than using `any`.

# 🗁 Generic Defaults with Default Parameters

Generic type parameter defaults and JavaScript function parameter defaults are different concepts.

Generic type parameter default:

```typescript
function createValue<T = string>(value?: T): T | undefined {
  return value;
}
```

Function parameter default:

```typescript
function greet(name: string = "Guest"): string {
  return `Hello, ${name}`;
}
```

Comparison:

| Feature | Generic Default | Function Parameter Default |
| --- | --- | --- |
| Syntax | `T = string` | `name = "Guest"` |
| Purpose | Provides a default type | Provides a default runtime value |
| Evaluation | Compile time | Runtime |
| Example | `Array<T = string>` | `greet(name = "Guest")` |

Both can be used in the same function.

```typescript
function createMessage<T = string>(
  value: T = "Hello" as T,
): T {
  return value;
}
```

Here, the generic default controls the type, while the parameter default supplies a runtime value.

# 🗁 Rules for Generic Defaults

TypeScript applies several rules when using generic defaults.

**A default type must satisfy its constraint.**

```typescript
interface Entity {
  id: number;
}

type Repository<T extends Entity = Entity> = {
  items: T[];
};
```

Valid because `Entity` satisfies the constraint.

Invalid:

```typescript
// TypeScript error
// type Repository<T extends Entity = string> = {
//   items: T[];
// };
```

**A required type parameter cannot follow an optional type parameter.**

```typescript
// TypeScript error
// type Example<T = string, U> = {
//   first: T;
//   second: U;
// };
```

Once a type parameter has a default, subsequent type parameters must also have defaults.

Correct:

```typescript
type Example<T = string, U = number> = {
  first: T;
  second: U;
};
```

**A default is used when inference cannot determine a type argument.**

```typescript
function identity<T = string>(value?: T): T | undefined {
  return value;
}

const result = identity();
```

The type of `result` is `string | undefined`.

**Explicit type arguments override defaults.**

```typescript
type Container<T = string> = {
  value: T;
};

type NumberContainer = Container<number>;
```

The resulting type is `Container<number>`.

**Defaults do not prevent type inference.**

```typescript
function wrap<T = string>(value: T): T {
  return value;
}

const result = wrap(100);
```

The inferred type is `number`, not `string`.

# 🗁 Partial Type Argument Specification

TypeScript does not generally allow skipping an earlier generic type argument to provide only a later one.

Consider:

```typescript
type Response<T = string, E = Error> = {
  data: T;
  error?: E;
};
```

You can specify the first argument:

```typescript
type UserResponse = Response<number>;
```

This produces `Response<number, Error>`.

However, you cannot directly skip the first argument and specify only the second:

```typescript
// Not valid syntax for skipping the first type argument
// type CustomErrorResponse = Response<, string>;
```

If you need to customize only the second parameter, you can:

* Supply both type arguments.
* Redesign the generic parameter order.
* Use a separate type alias with a default.
* Use a factory function where appropriate.

Example:

```typescript
type StringDataResponse<E = Error> = Response<string, E>;

type CustomErrorResponse = StringDataResponse<string>;
```

# 🗁 Common Mistakes

**Using a default type that violates the constraint**

```typescript
// TypeScript error
// type Container<T extends number = string> = {
//   value: T;
// };
```

Correct:

```typescript
type Container<T extends number = number> = {
  value: T;
};
```

**Placing required parameters after default parameters**

```typescript
// TypeScript error
// type Pair<T = string, U> = [T, U];
```

Correct:

```typescript
type Pair<T = string, U = number> = [T, U];
```

**Assuming defaults always override inference**

```typescript
function identity<T = string>(value: T): T {
  return value;
}

const result = identity(100);
```

The result type is `number` because TypeScript infers the type from the argument.

**Using any as a default unnecessarily**

```typescript
type ApiResponse<T = any> = {
  data: T;
};
```

Prefer `unknown` when the data type is not known.

```typescript
type ApiResponse<T = unknown> = {
  data: T;
};
```

**Confusing type defaults with runtime defaults**

A generic default does not assign a runtime value.

```typescript
function getValue<T = string>(): T | undefined {
  return undefined;
}
```

The default type is `string`, but the function still returns `undefined`.

# 🗁 Best Practices

* Provide defaults when a generic type has a sensible common type.
* Use `unknown` instead of `any` when the default data type is uncertain.
* Ensure default types satisfy their generic constraints.
* Put required generic parameters before parameters with defaults.
* Avoid unnecessary defaults that make type inference harder to understand.
* Use explicit type arguments when they improve readability.
* Use generic defaults to simplify reusable interfaces, classes, and type aliases.
* Keep generic parameter order intuitive.
* Document defaults when designing public libraries and APIs.
* Remember that inferred types take precedence over generic defaults when inference succeeds.

# 🗁 Summary

* Generic defaults provide fallback types for generic parameters.
* The assignment operator (`=`) defines a default type.
* They work with type aliases, functions, interfaces, and classes.
* Explicit type arguments override defaults.
* Type inference generally takes precedence over defaults.
* A default must satisfy its generic constraint.
* Required type parameters cannot follow parameters with defaults.
* Generic defaults are useful for API responses, repositories, collections, and reusable libraries.
* Generic defaults are compile-time features and do not create runtime values.

# 🗁 Related Topics

* Generic Basics
* Generic Functions
* Generic Interfaces
* Generic Classes
* Generic Constraints
* keyof Operator
* typeof Operator
* Utility Types
* Type Inference
* Conditional Types
* Indexed Access Types
* Function Parameter Defaults
