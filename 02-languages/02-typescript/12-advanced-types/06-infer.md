# Infer Keyword

The `infer` keyword in TypeScript allows you to declare a type variable inside a conditional type and let TypeScript infer its type from an existing structure.

It is primarily used with conditional types to extract information from complex types without manually specifying every detail.

The `infer` keyword is especially useful for:

* Extracting function return types.
* Extracting function parameter types.
* Extracting array and tuple element types.
* Extracting promise result types.
* Working with nested generic structures.
* Building custom utility types.

# 🗁 Basic Syntax

The `infer` keyword is used inside the `extends` clause of a conditional type.

```typescript
type SomeType<T> =
  T extends SomeStructure<infer U>
    ? U
    : never;
```

Where:

* `T` is the type being examined.
* `extends` checks whether `T` matches a particular structure.
* `infer U` asks TypeScript to infer a type from that structure.
* `U` represents the inferred type.
* `?` defines the result when the condition matches.
* `:` defines the result when the condition does not match.

The inferred type variable is available only within the corresponding conditional type branch.

# 🗁 Basic Example

Consider a generic array type:

```typescript
type ArrayElement<T> =
  T extends (infer U)[]
    ? U
    : never;
```

Usage:

```typescript
type Numbers = ArrayElement<number[]>;

type Names = ArrayElement<string[]>;
```

Result:

```typescript
type Numbers = number;

type Names = string;
```

TypeScript examines the array structure and infers the element type.

# 🗁 Extracting Array Element Types

You can use `infer` to extract the element type from an array.

```typescript
type ElementType<T> =
  T extends (infer Item)[]
    ? Item
    : never;
```

Usage:

```typescript
type NumberElement = ElementType<number[]>;

type StringElement = ElementType<string[]>;

type User = {
  id: number;
  name: string;
};

type UserElement = ElementType<User[]>;
```

Result:

```typescript
type NumberElement = number;

type StringElement = string;

type UserElement = {
  id: number;
  name: string;
};
```

You can also use the `readonly` array structure:

```typescript
type ReadonlyElement<T> =
  T extends readonly (infer Item)[]
    ? Item
    : never;
```

Usage:

```typescript
type Roles = ReadonlyElement<
  readonly ["admin", "editor", "user"]
>;
```

Result:

```typescript
type Roles = "admin" | "editor" | "user";
```

# 🗁 Extracting Function Return Types

One of the most common uses of `infer` is extracting a function's return type.

```typescript
type MyReturnType<T> =
  T extends (...args: any[]) => infer R
    ? R
    : never;
```

Usage:

```typescript
function getUser() {
  return {
    id: 1,
    name: "John"
  };
}

type User = MyReturnType<typeof getUser>;
```

Result:

```typescript
type User = {
  id: number;
  name: string;
};
```

Explanation:

* `T` represents the function type.
* `(...args: any[]) => infer R` checks whether `T` is a function.
* `infer R` extracts its return type.
* If `T` is not a function, the result is `never`.

TypeScript provides a built-in utility type called `ReturnType<T>` for this purpose.

```typescript
type User = ReturnType<typeof getUser>;
```

# 🗁 Extracting Function Parameter Types

You can extract all function parameter types using `infer`.

```typescript
type MyParameters<T> =
  T extends (...args: infer P) => any
    ? P
    : never;
```

Usage:

```typescript
function createUser(
  name: string,
  age: number,
  active: boolean
) {
  return {
    name,
    age,
    active
  };
}

type CreateUserParameters =
  MyParameters<typeof createUser>;
```

Result:

```typescript
type CreateUserParameters = [
  name: string,
  age: number,
  active: boolean
];
```

The inferred parameter type is represented as a tuple.

TypeScript provides the built-in `Parameters<T>` utility type:

```typescript
type CreateUserParameters =
  Parameters<typeof createUser>;
```

# 🗁 Extracting the First Function Parameter

You can combine `infer` with tuple patterns to extract a specific parameter.

```typescript
type FirstParameter<T> =
  T extends (first: infer P, ...args: any[]) => any
    ? P
    : never;
```

Usage:

```typescript
function createProduct(
  title: string,
  price: number,
  available: boolean
) {
  return {
    title,
    price,
    available
  };
}

type FirstArg = FirstParameter<typeof createProduct>;
```

Result:

```typescript
type FirstArg = string;
```

You can extract the second parameter in a similar way:

```typescript
type SecondParameter<T> =
  T extends (
    first: any,
    second: infer P,
    ...args: any[]
  ) => any
    ? P
    : never;
```

Result:

```typescript
type SecondArg = SecondParameter<typeof createProduct>;
// number
```

# 🗁 Extracting Promise Result Types

You can use `infer` to extract the resolved type of a `Promise`.

```typescript
type PromiseResult<T> =
  T extends Promise<infer U>
    ? U
    : T;
```

Usage:

```typescript
type NumberResult = PromiseResult<Promise<number>>;

type UserResult = PromiseResult<
  Promise<{ id: number; name: string }>
>;
```

Result:

```typescript
type NumberResult = number;

type UserResult = {
  id: number;
  name: string;
};
```

The built-in `Awaited<T>` utility type provides more comprehensive handling of promise-like types.

```typescript
type UserResult = Awaited<
  Promise<{ id: number; name: string }>
>;
```

# 🗁 Extracting Nested Promise Types

You can use `infer` to extract values from nested promise structures.

```typescript
type DeepPromiseResult<T> =
  T extends Promise<infer U>
    ? DeepPromiseResult<U>
    : T;
```

Usage:

```typescript
type Result = DeepPromiseResult<
  Promise<Promise<Promise<number>>>
>;
```

Result:

```typescript
type Result = number;
```

The conditional type recursively unwraps promises until it reaches a non-promise type.

# 🗁 Extracting Tuple Elements

You can use `infer` to extract tuple elements individually.

```typescript
type FirstElement<T> =
  T extends [infer First, ...unknown[]]
    ? First
    : never;
```

Usage:

```typescript
type User = [number, string, boolean];

type UserId = FirstElement<User>;
```

Result:

```typescript
type UserId = number;
```

You can extract the first and second elements together:

```typescript
type FirstTwo<T> =
  T extends [
    infer First,
    infer Second,
    ...unknown[]
  ]
    ? [First, Second]
    : never;
```

Usage:

```typescript
type User = [number, string, boolean];

type UserDetails = FirstTwo<User>;
```

Result:

```typescript
type UserDetails = [number, string];
```

For readonly tuples, use `readonly` in the matching pattern:

```typescript
type FirstReadonlyElement<T> =
  T extends readonly [infer First, ...unknown[]]
    ? First
    : never;
```

# 🗁 Extracting the Last Tuple Element

You can use tuple rest inference to extract the last element.

```typescript
type LastElement<T> =
  T extends [...unknown[], infer Last]
    ? Last
    : never;
```

Usage:

```typescript
type User = [number, string, boolean];

type UserStatus = LastElement<User>;
```

Result:

```typescript
type UserStatus = boolean;
```

For readonly tuples:

```typescript
type LastReadonlyElement<T> =
  T extends readonly [...unknown[], infer Last]
    ? Last
    : never;
```

# 🗁 Extracting Object Property Types

You can use `infer` to extract property types from object structures.

```typescript
type ExtractId<T> =
  T extends { id: infer Id }
    ? Id
    : never;
```

Usage:

```typescript
type User = {
  id: number;
  name: string;
};

type UserId = ExtractId<User>;
```

Result:

```typescript
type UserId = number;
```

You can extract multiple properties:

```typescript
type ExtractUser<T> =
  T extends {
    id: infer Id;
    name: infer Name;
  }
    ? {
        id: Id;
        name: Name;
      }
    : never;
```

Usage:

```typescript
type User = {
  id: number;
  name: string;
  email: string;
};

type UserSummary = ExtractUser<User>;
```

Result:

```typescript
type UserSummary = {
  id: number;
  name: string;
};
```

The original object can contain additional properties. The conditional type only requires the specified structure.

# 🗁 Extracting Constructor Instance Types

You can use `infer` to extract the instance type produced by a constructor.

```typescript
type MyInstanceType<T> =
  T extends new (...args: any[]) => infer Instance
    ? Instance
    : never;
```

Usage:

```typescript
class User {
  constructor(
    public id: number,
    public name: string
  ) {}
}

type UserInstance = MyInstanceType<typeof User>;
```

Result:

```typescript
type UserInstance = User;
```

TypeScript provides the built-in `InstanceType<T>` utility type:

```typescript
type UserInstance = InstanceType<typeof User>;
```

# 🗁 Extracting Constructor Parameters

You can also extract the parameters accepted by a class constructor.

```typescript
type MyConstructorParameters<T> =
  T extends new (...args: infer P) => any
    ? P
    : never;
```

Usage:

```typescript
class Product {
  constructor(
    public title: string,
    public price: number
  ) {}
}

type ProductParameters =
  MyConstructorParameters<typeof Product>;
```

Result:

```typescript
type ProductParameters = [
  title: string,
  price: number
];
```

TypeScript provides the built-in `ConstructorParameters<T>` utility type.

# 🗁 Inferring from Multiple Positions

TypeScript can infer a type from multiple positions within the same structure.

Consider the following example:

```typescript
type ElementType<T> =
  T extends {
    first: infer U;
    second: infer U;
  }
    ? U
    : never;
```

Usage:

```typescript
type Result = ElementType<{
  first: string;
  second: number;
}>;
```

Result:

```typescript
type Result = string | number;
```

When the same inferred variable appears in multiple covariant positions, TypeScript can combine the inferred candidates into a union.

# 🗁 Inferring from Function Parameters

Inference from function parameter positions can behave differently.

```typescript
type InferParameter<T> =
  T extends {
    first: (value: infer U) => void;
    second: (value: infer U) => void;
  }
    ? U
    : never;
```

Usage:

```typescript
type Result = InferParameter<{
  first: (value: string) => void;
  second: (value: number) => void;
}>;
```

Result:

```typescript
type Result = never;
```

In this example, the candidates come from contravariant positions. TypeScript combines compatible candidates using an intersection, and `string & number` reduces to `never`.

The distinction between covariant and contravariant inference becomes important in advanced type-level programming.

# 🗁 Multiple Conditional Inference Branches

You can use multiple conditional branches to extract different types.

```typescript
type ExtractValue<T> =
  T extends string
    ? "string"
    : T extends number
      ? "number"
      : T extends boolean
        ? "boolean"
        : "other";
```

Usage:

```typescript
type A = ExtractValue<string>;
type B = ExtractValue<number>;
type C = ExtractValue<boolean>;
type D = ExtractValue<object>;
```

Result:

```typescript
type A = "string";
type B = "number";
type C = "boolean";
type D = "other";
```

This example uses conditional types rather than `infer`, but the same branching structure can be combined with inference to inspect more complex types.

# 🗁 Practical Example: Extracting API Data

Suppose an API function returns a promise containing a response object.

```typescript
async function getUser() {
  return {
    success: true,
    data: {
      id: 1,
      name: "John",
      email: "john@example.com"
    }
  };
}
```

You can extract the response type:

```typescript
type ApiResponse = Awaited<ReturnType<typeof getUser>>;

type User = ApiResponse["data"];
```

Result:

```typescript
type ApiResponse = {
  success: boolean;
  data: {
    id: number;
    name: string;
    email: string;
  };
};

type User = {
  id: number;
  name: string;
  email: string;
};
```

This approach keeps API response types synchronized with the actual function return type.

# 🗁 Practical Example: Extracting Event Handler Arguments

You can extract the event type accepted by a handler.

```typescript
type HandlerEvent<T> =
  T extends (event: infer Event) => void
    ? Event
    : never;
```

Usage:

```typescript
type ClickHandler = (
  event: {
    target: string;
    timestamp: number;
  }
) => void;

type ClickEvent = HandlerEvent<ClickHandler>;
```

Result:

```typescript
type ClickEvent = {
  target: string;
  timestamp: number;
};
```

This pattern is useful when building reusable event systems and callback abstractions.

# 🗁 Practical Example: Extracting Function Arguments and Return Type

You can create a reusable function description type.

```typescript
type FunctionDetails<T> =
  T extends (...args: infer Args) => infer Result
    ? {
        arguments: Args;
        result: Result;
      }
    : never;
```

Usage:

```typescript
function createProduct(
  title: string,
  price: number
) {
  return {
    id: 1,
    title,
    price
  };
}

type ProductDetails =
  FunctionDetails<typeof createProduct>;
```

Result:

```typescript
type ProductDetails = {
  arguments: [title: string, price: number];
  result: {
    id: number;
    title: string;
    price: number;
  };
};
```

# 🗁 Common Mistakes

**Using `infer` outside a conditional type**

```typescript
// Invalid:
type Result<infer T> = T;
```

`infer` must appear within the matching pattern of a conditional type.

**Trying to use an inferred variable outside its branch**

```typescript
type Extract<T> =
  T extends Promise<infer U>
    ? U
    : never;

// Invalid:
// type Result = U;
```

The inferred variable `U` is available only within the conditional type branch where it is declared.

**Using a mutable tuple pattern for readonly tuples**

```typescript
type First<T> =
  T extends [infer U, ...unknown[]]
    ? U
    : never;

type Result = First<readonly [string, number]>;
```

The pattern does not match because a readonly tuple cannot extend a mutable tuple type.

Use a readonly pattern instead:

```typescript
type First<T> =
  T extends readonly [infer U, ...unknown[]]
    ? U
    : never;
```

**Confusing `infer` with runtime inference**

```typescript
type Element<T> =
  T extends (infer U)[]
    ? U
    : never;
```

This operation happens entirely in the TypeScript type system. It does not inspect or transform runtime values.

# 🗁 Best Practices

* Use `infer` when extracting types from complex generic structures.
* Combine `infer` with conditional types for reusable type transformations.
* Prefer built-in utility types such as `ReturnType`, `Parameters`, `Awaited`, and `InstanceType` when they meet your requirements.
* Use descriptive inferred variable names when the transformation is complex.
* Use readonly tuple patterns when supporting readonly arrays and tuples.
* Keep recursive inference patterns understandable and avoid unnecessary complexity.
* Remember that inferred variables are scoped to their conditional type branches.
* Understand variance when inferring the same variable from multiple positions.

# 🗁 Summary

The `infer` keyword allows TypeScript to extract and infer types from existing structures inside conditional types.

Key concepts:

* `infer U` declares a type variable whose value TypeScript determines.
* `infer` must be used inside a conditional type's `extends` pattern.
* Array patterns can extract element types.
* Function patterns can extract parameters and return types.
* Promise patterns can extract resolved value types.
* Tuple patterns can extract individual elements.
* Constructor patterns can extract instance and constructor parameter types.
* Multiple inference positions can produce unions or intersections depending on variance.
* Built-in utility types use similar inference techniques.

The `infer` keyword is one of the most powerful tools for building advanced, reusable TypeScript utility types.
