# Generic Functions

Generic functions are functions that use type parameters to work with different data types while preserving type safety.

Instead of defining separate functions for strings, numbers, objects, or arrays, you can create one generic function that adapts to the supplied types.

Generic functions are useful for:

* Creating reusable utility functions.
* Preserving input and output type relationships.
* Processing different data structures.
* Building type-safe array operations.
* Working with objects and properties.
* Creating reusable application services.
* Implementing data transformation functions.
* Avoiding unnecessary use of `any`.

# 🗁 Basic Syntax

A generic function declares one or more type parameters inside angle brackets.

```typescript
function functionName<T>(parameter: T): T {
  return parameter;
}
```

Example:

```typescript
function identity<T>(value: T): T {
  return value;
}
```

Here:

* `T` is a generic type parameter.
* `value: T` means the parameter uses type `T`.
* `: T` means the function returns the same type.

Usage:

```typescript
const name = identity<string>("Alice");
const age = identity<number>(25);
const active = identity<boolean>(true);
```

The function works with different types without duplicating its implementation.

# 🗁 Explicit Type Arguments

You can explicitly provide the type argument when calling a generic function.

```typescript
function getValue<T>(value: T): T {
  return value;
}

const username = getValue<string>("Alice");
const userAge = getValue<number>(30);
```

TypeScript checks that the supplied argument matches the specified type.

Invalid usage:

```typescript
// TypeScript error
const age = getValue<number>("30");
```

The function expects a number because `T` was explicitly specified as `number`.

Explicit type arguments are useful when inference is insufficient or when you want to make the intended type clear.

# 🗁 Type Argument Inference

TypeScript can automatically infer generic type arguments from function arguments.

```typescript
function identity<T>(value: T): T {
  return value;
}

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

You do not need to write `<string>`, `<number>`, or `<boolean>` in these calls.

Prefer inference when it produces the intended type.

# 🗁 Generic Functions with Multiple Parameters

A generic function can accept multiple parameters using the same type parameter.

```typescript
function compare<T>(first: T, second: T): boolean {
  return first === second;
}
```

Usage:

```typescript
compare<number>(10, 20);
compare<string>("Alice", "Bob");
compare<boolean>(true, false);
```

Both arguments must satisfy the same inferred or explicitly supplied type.

For example:

```typescript
// TypeScript error
compare(10, "20");
```

You can also use separate type parameters when the arguments are allowed to have different types.

```typescript
function pair<T, U>(first: T, second: U): [T, U] {
  return [first, second];
}

const result = pair("Alice", 25);
```

Inferred type:

```typescript
[string, number]
```

# 🗁 Generic Functions with Return Types

A generic function can use its type parameter as its return type.

```typescript
function createValue<T>(value: T): T {
  return value;
}
```

Usage:

```typescript
const message = createValue("Hello");
const count = createValue(100);
```

The return type depends on the argument.

You can also return a generic type inside another structure.

```typescript
function wrapValue<T>(value: T): { data: T } {
  return {
    data: value,
  };
}
```

Usage:

```typescript
const result = wrapValue("Hello");
```

Inferred return type:

```typescript
{
  data: string;
}
```

The generic type parameter is preserved inside the returned object.

# 🗁 Generic Functions with Arrays

Generics are especially useful for array utility functions.

**Get the first element**

```typescript
function getFirst<T>(items: T[]): T | undefined {
  return items[0];
}

const firstNumber = getFirst([10, 20, 30]);
const firstName = getFirst(["Alice", "Bob"]);
```

Inferred return types:

```typescript
// firstNumber: number | undefined
// firstName: string | undefined
```

The `undefined` type accounts for an empty array.

**Reverse an array**

```typescript
function reverseArray<T>(items: T[]): T[] {
  return [...items].reverse();
}

const numbers = reverseArray([1, 2, 3]);
const names = reverseArray(["Alice", "Bob"]);
```

The function preserves the element type while returning a new array.

**Get the last element**

```typescript
function getLast<T>(items: T[]): T | undefined {
  return items.at(-1);
}
```

Usage:

```typescript
const lastNumber = getLast([10, 20, 30]);
const lastName = getLast(["Alice", "Bob"]);
```

# 🗁 Generic Functions with Readonly Arrays

A function that only reads an array should often accept a readonly array.

```typescript
function getFirst<T>(items: readonly T[]): T | undefined {
  return items[0];
}
```

It accepts both mutable and readonly arrays.

```typescript
const mutableNumbers = [10, 20, 30] as number[];
const readonlyNumbers = [10, 20, 30] as const;

getFirst(mutableNumbers);
getFirst(readonlyNumbers);
```

This function does not modify the supplied array, so accepting readonly input makes its contract more flexible.

# 🗁 Generic Functions with Objects

Generic functions can preserve the structure of objects.

```typescript
function identityObject<T extends object>(value: T): T {
  return value;
}
```

Usage:

```typescript
const user = identityObject({
  id: 1,
  name: "Alice",
  active: true,
});
```

The returned type retains the original object properties.

```typescript
// {
//   id: number;
//   name: string;
//   active: boolean;
// }
```

The `object` constraint excludes primitive values such as strings, numbers, and booleans. It still permits arrays and functions because they are objects in TypeScript's type system.

# 🗁 Generic Functions with keyof

The `keyof` operator can be combined with generics to create type-safe object property access.

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

Invalid property access:

```typescript
// TypeScript error
getProperty(user, "email");
```

The constraint `K extends keyof T` ensures that the key must exist on the object.

The return type `T[K]` represents the type of the selected property.

# 🗁 Generic Functions with Callbacks

A generic function can accept a callback that operates on the same type parameter.

```typescript
function processValue<T>(
  value: T,
  callback: (item: T) => void,
): void {
  callback(value);
}
```

Usage:

```typescript
processValue("Hello", (value) => {
  console.log(value.toUpperCase());
});

processValue(100, (value) => {
  console.log(value.toFixed(2));
});
```

TypeScript infers the callback parameter type from the generic function argument.

This pattern is useful for reusable processing functions, event handlers, and collection operations.

# 🗁 Generic Functions with Promises

Generics can represent the type of a value returned by an asynchronous operation.

```typescript
async function fetchData<T>(
  url: string,
): Promise<T> {
  const response = await fetch(url);

  if (!response.ok) {
    throw new Error(`Request failed: ${response.status}`);
  }

  return (await response.json()) as T;
}
```

Usage:

```typescript
interface User {
  id: number;
  name: string;
}

const user = await fetchData<User>("/api/users/1");
```

The variable `user` has type `User`.

The generic parameter describes the expected response type at compile time. It does not validate the actual JSON received from the server. Runtime validation requires an additional validation mechanism.

# 🗁 Generic Functions with Multiple Type Parameters

Use multiple type parameters when a function needs to preserve relationships between different values.

Example:

```typescript
function createEntry<K, V>(
  key: K,
  value: V,
): { key: K; value: V } {
  return {
    key,
    value,
  };
}
```

Usage:

```typescript
const entry = createEntry("id", 101);
```

Inferred type:

```typescript
{
  key: string;
  value: number;
}
```

Another example:

```typescript
function mergeObjects<T, U>(
  first: T,
  second: U,
): T & U {
  return {
    ...first,
    ...second,
  };
}
```

Usage:

```typescript
const user = mergeObjects(
  {
    name: "Alice",
  },
  {
    age: 25,
  },
);
```

The resulting type combines both object types.

```typescript
{
  name: string;
  age: number;
}
```

# 🗁 Generic Functions with Constraints

Sometimes a generic function requires a specific property or capability.

Use `extends` to constrain the type parameter.

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
getLength({ length: 5 });
```

Invalid usage:

```typescript
// TypeScript error
getLength(100);
```

The function accepts any type that has a compatible numeric `length` property.

This approach preserves the original input type while restricting which types are accepted.

# 🗁 Generic Functions with Default Type Parameters

A generic function can define a default type argument.

```typescript
function createValue<T = string>(value?: T): T | undefined {
  return value;
}
```

Usage:

```typescript
const first = createValue();
const second = createValue<number>(100);
const third = createValue("Hello");
```

The default type argument is used when TypeScript cannot infer `T` from an argument and no explicit type argument is supplied.

The default does not force every call to use that type when inference can determine another type.

# 🗁 Generic Arrow Functions

Arrow functions can also use generic type parameters.

```typescript
const identity = <T>(value: T): T => {
  return value;
};
```

Usage:

```typescript
const name = identity("Alice");
const age = identity(25);
```

In a `.tsx` file, the syntax `<T>` can be interpreted as the beginning of a JSX element.

Use a trailing comma to make the generic parameter declaration unambiguous:

```tsx
const identity = <T,>(value: T): T => {
  return value;
};
```

Another option is to use an explicit constraint:

```tsx
const identity = <T extends unknown>(value: T): T => {
  return value;
};
```

Both forms are useful for generic arrow functions in TSX files.

# 🗁 Generic Functions with Function Types

You can describe generic functions using function type syntax.

```typescript
type Identity = <T>(value: T) => T;
```

Implement a function using that type:

```typescript
const identity: Identity = (value) => {
  return value;
};
```

You can also define a reusable generic function type alias:

```typescript
type Transformer<T> = (value: T) => T;
```

Usage:

```typescript
const uppercase: Transformer<string> = (value) => {
  return value.toUpperCase();
};

const double: Transformer<number> = (value) => {
  return value * 2;
};
```

The first example describes a generic function that chooses its type argument for each call. The second describes a function type that has already been specialized with a particular type.

# 🗁 Generic Function Overloads

Generic functions can be combined with overload signatures when different input patterns require different return types.

```typescript
function getValue<T>(value: T): T;
function getValue<T>(value: T[]): T | undefined;

function getValue<T>(value: T | T[]): T | undefined {
  if (Array.isArray(value)) {
    return value[0];
  }

  return value;
}
```

Usage:

```typescript
const name = getValue("Alice");
const firstNumber = getValue([10, 20, 30]);
```

The overload signatures describe the public calling patterns, while the implementation handles the actual behavior.

Overloads should be used when they provide a clearer and more precise API than a single generic signature.

# 🗁 Generic Functions vs Non-Generic Functions

A non-generic function usually works with a specific type or a broader type.

```typescript
function getString(value: string): string {
  return value;
}
```

A generic function can preserve a relationship between different types.

```typescript
function identity<T>(value: T): T {
  return value;
}
```

Comparison:

| Feature | Non-Generic Function | Generic Function |
| --- | --- | --- |
| Type flexibility | Usually limited | Flexible |
| Type parameter | Not required | Required |
| Input/output relationship | Defined by signature | Can be preserved through `T` |
| Reusability | Depends on declared types | Can support many types |
| Type safety | Yes | Yes |

Use a non-generic function when its input and output types are fixed. Use a generic function when the same operation should work with multiple types while retaining their relationships.

# 🗁 Common Mistakes

**Using any instead of a generic parameter**

```typescript
function identity(value: any): any {
  return value;
}
```

This loses the relationship between the input and output.

Prefer:

```typescript
function identity<T>(value: T): T {
  return value;
}
```

**Declaring unnecessary type parameters**

```typescript
function printValue<T>(value: string): void {
  console.log(value);
}
```

The type parameter is not used meaningfully. Remove it.

**Assuming every generic needs an explicit type argument**

```typescript
const result = identity<string>("Hello");
```

When inference is sufficient, this is simpler:

```typescript
const result = identity("Hello");
```

**Using a generic without preserving a relationship**

```typescript
function getLength<T>(value: T): number {
  return 10;
}
```

The generic parameter does not contribute to the function's type behavior. A non-generic function may be more appropriate.

**Assuming a generic validates runtime data**

```typescript
const user = JSON.parse(json) as User;
```

A type assertion does not validate the JSON. Generic return types and assertions also cannot guarantee that external data matches a declared interface.

Use runtime validation when data comes from untrusted sources.

# 🗁 Best Practices

* Use generics when a function must support multiple types while preserving type information.
* Prefer automatic type inference when possible.
* Use explicit type arguments when they improve clarity or resolve ambiguity.
* Use multiple type parameters only when the function needs multiple independent type relationships.
* Use `extends` to express required structural capabilities.
* Prefer `readonly` array parameters when the function does not mutate its input.
* Keep generic function signatures understandable.
* Avoid unnecessary `any` and unused type parameters.
* Use runtime validation for external data.
* Prefer simple concrete types when a generic abstraction adds no real value.

# 🗁 Summary

* Generic functions use type parameters to support multiple data types.
* Type parameters are declared inside angle brackets.
* TypeScript can infer generic type arguments from function arguments.
* A function can use multiple type parameters.
* Generics work with arrays, objects, callbacks, promises, and arrow functions.
* Generic constraints restrict accepted types.
* Generic defaults provide fallback type arguments.
* Generic function types can describe reusable function contracts.
* Generics preserve type relationships without sacrificing type safety.

# 🗁 Related Topics

* Generic Basics
* Generic Interfaces
* Generic Classes
* Generic Constraints
* Generic Defaults
* Generic Utility Patterns
* Function Types
* Function Overloads
* Type Inference
* keyof Operator
* Indexed Access Types
* Conditional Types
* Promise
* Readonly Arrays
