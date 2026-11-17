# Type-Level Programming

Type-level programming is the practice of using TypeScript's type system to perform computations, transformations, and logical operations at compile time.

Instead of manipulating runtime values with JavaScript, type-level programming manipulates types to produce new types.

It allows developers to build reusable type utilities, enforce complex constraints, and create strongly typed APIs.

Common applications include:

* Transforming object properties.
* Extracting information from complex types.
* Filtering and manipulating union types.
* Building reusable generic utilities.
* Validating relationships between types.
* Creating type-safe APIs and libraries.
* Modeling complex application states.

# 🗁 Understanding Type-Level Programming

Consider a normal JavaScript function:

```typescript
function add(a: number, b: number): number {
  return a + b;
}

const result = add(10, 20);
```

This function operates on runtime values.

Type-level programming operates on types instead:

```typescript
type AddType<A, B> =
  A extends number
    ? B extends number
      ? number
      : never
    : never;

type Result = AddType<number, number>;
```

The resulting type is:

```typescript
type Result = number;
```

This example performs a type-level check rather than arithmetic addition.

TypeScript's type system can inspect, transform, and combine types, but it does not provide general-purpose compile-time arithmetic evaluation.

# 🗁 Type-Level Programming vs Runtime Programming

| Feature | Runtime Programming | Type-Level Programming |
| --- | --- | --- |
| Operates on | Values | Types |
| Execution | Runtime | Compile time |
| Main language | JavaScript | TypeScript type system |
| Output | Runtime values | New types |
| Error detection | Runtime or compile time | Compile time |
| Examples | Functions and loops | Conditional and mapped types |

Example:

```typescript
// Runtime programming

function getLength(value: string): number {
  return value.length;
}
```

```typescript
// Type-level programming

type LengthType<T> =
  T extends string
    ? number
    : never;

type Result = LengthType<string>;
```

The first calculates the actual length of a string.

The second determines the resulting type based on the input type.

# 🗁 Generic Types as Type-Level Parameters

Generic types are the foundation of reusable type-level programming.

They allow types to accept other types as parameters.

```typescript
type Container<T> = {
  value: T;
};
```

Usage:

```typescript
type StringContainer = Container<string>;

type NumberContainer = Container<number>;
```

Results:

```typescript
type StringContainer = {
  value: string;
};

type NumberContainer = {
  value: number;
};
```

The generic parameter `T` makes the type reusable for different input types.

# 🗁 Conditional Types as Type-Level Logic

Conditional types allow you to express type-level decisions.

```typescript
type IsString<T> =
  T extends string
    ? true
    : false;
```

Usage:

```typescript
type A = IsString<string>;

type B = IsString<number>;
```

Results:

```typescript
type A = true;

type B = false;
```

Conditional types work similarly to conditional expressions in JavaScript, but they operate on types.

# 🗁 Combining Conditional Types

You can combine multiple conditions to classify types.

```typescript
type TypeCategory<T> =
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
type A = TypeCategory<string>;

type B = TypeCategory<number>;

type C = TypeCategory<boolean>;

type D = TypeCategory<Date>;
```

Results:

```typescript
type A = "string";

type B = "number";

type C = "boolean";

type D = "other";
```

This is useful when creating utilities that behave differently depending on their input types.

# 🗁 Mapped Types as Type-Level Transformation

Mapped types allow you to iterate over the keys of an object type and transform its properties.

```typescript
type User = {
  id: number;
  name: string;
  email: string;
};
```

Create a type that makes every property optional:

```typescript
type MyPartial<T> = {
  [K in keyof T]?: T[K];
};
```

Usage:

```typescript
type OptionalUser = MyPartial<User>;
```

Result:

```typescript
type OptionalUser = {
  id?: number;
  name?: string;
  email?: string;
};
```

The mapped type iterates over every property key and creates an optional version.

# 🗁 Keyof as Type-Level Key Extraction

The `keyof` operator extracts the property keys of an object type.

```typescript
type User = {
  id: number;
  name: string;
  email: string;
};

type UserKeys = keyof User;
```

Result:

```typescript
type UserKeys = "id" | "name" | "email";
```

You can use these keys to create generic constraints.

```typescript
function getProperty<T, K extends keyof T>(
  object: T,
  key: K
): T[K] {
  return object[key];
}
```

Usage:

```typescript
const user = {
  id: 101,
  name: "Alex",
  email: "alex@example.com",
};

const name = getProperty(user, "name");
```

TypeScript infers the return type as `string`.

This technique provides type-safe property access.

# 🗁 Indexed Access Types

Indexed access types extract the type of a specific property.

```typescript
type User = {
  id: number;
  name: string;
  active: boolean;
};

type UserId = User["id"];

type UserName = User["name"];
```

Results:

```typescript
type UserId = number;

type UserName = string;
```

You can also extract multiple property types:

```typescript
type UserValues = User["id" | "name"];
```

Result:

```typescript
type UserValues = number | string;
```

Indexed access types are useful when building generic utilities that depend on existing object properties.

# 🗁 Infer as Type-Level Extraction

The `infer` keyword allows TypeScript to capture a type from another type.

```typescript
type ReturnTypeOf<T> =
  T extends (...args: any[]) => infer R
    ? R
    : never;
```

Usage:

```typescript
function getUser() {
  return {
    id: 101,
    name: "Alex",
  };
}

type User = ReturnTypeOf<typeof getUser>;
```

Result:

```typescript
type User = {
  id: number;
  name: string;
};
```

Here, `infer R` captures the function's return type.

# 🗁 Template Literal Types

Template literal types allow you to construct new string literal types from existing types.

```typescript
type Greeting = `Hello ${string}`;
```

Valid values:

```typescript
const a: Greeting = "Hello Alex";

const b: Greeting = "Hello World";
```

Invalid value:

```typescript
const c: Greeting = "Hi Alex";
```

You can also combine unions:

```typescript
type Color = "red" | "blue";

type Size = "small" | "large";

type Variant = `${Color}-${Size}`;
```

Result:

```typescript
type Variant =
  | "red-small"
  | "red-large"
  | "blue-small"
  | "blue-large";
```

Template literal types are useful for event names, route patterns, property naming conventions, and API contracts.

# 🗁 Recursive Type-Level Programming

Type-level recursion allows a type to refer to itself while transforming another type.

Consider a nested array:

```typescript
type DeepArray<T> =
  T extends (infer U)[]
    ? DeepArray<U>
    : T;
```

Usage:

```typescript
type Result = DeepArray<number[][][]>;
```

Result:

```typescript
type Result = number;
```

The conditional type repeatedly extracts the inner array element until it reaches a non-array type.

Recursive types are useful for:

* Deep object transformations.
* Nested array processing.
* JSON type definitions.
* Recursive data structures.
* Nested property path utilities.

# 🗁 Building a Deep Readonly Utility

TypeScript provides the built-in `Readonly<T>` utility, which makes properties readonly at the top level.

You can create a recursive utility for nested objects:

```typescript
type DeepReadonly<T> =
  T extends (...args: any[]) => any
    ? T
    : T extends object
      ? {
          readonly [K in keyof T]: DeepReadonly<T[K]>;
        }
      : T;
```

Usage:

```typescript
type User = {
  id: number;
  profile: {
    name: string;
    address: {
      city: string;
    };
  };
};

type ReadonlyUser = DeepReadonly<User>;
```

Result:

```typescript
type ReadonlyUser = {
  readonly id: number;
  readonly profile: {
    readonly name: string;
    readonly address: {
      readonly city: string;
    };
  };
};
```

Nested properties are also readonly.

This implementation preserves function types and recursively transforms object properties. Production utilities may need additional handling for arrays, tuples, maps, sets, and special object types.

# 🗁 Building a Deep Partial Utility

A deep partial utility makes all nested properties optional.

```typescript
type DeepPartial<T> =
  T extends (...args: any[]) => any
    ? T
    : T extends object
      ? {
          [K in keyof T]?: DeepPartial<T[K]>;
        }
      : T;
```

Usage:

```typescript
type User = {
  id: number;
  profile: {
    name: string;
    address: {
      city: string;
      country: string;
    };
  };
};

type PartialUser = DeepPartial<User>;
```

Result:

```typescript
type PartialUser = {
  id?: number;
  profile?: {
    name?: string;
    address?: {
      city?: string;
      country?: string;
    };
  };
};
```

This is useful for deeply nested configuration objects and partial update operations.

# 🗁 Distributive Conditional Types

Distributive conditional types allow conditional logic to operate independently on every member of a union.

```typescript
type ToArray<T> =
  T extends unknown
    ? T[]
    : never;
```

Usage:

```typescript
type Result = ToArray<string | number>;
```

Result:

```typescript
type Result = string[] | number[];
```

This behavior is useful for filtering and transforming union types.

You can prevent distribution by wrapping the generic parameter:

```typescript
type ToArray<T> =
  [T] extends [unknown]
    ? T[]
    : never;
```

Now the complete union is wrapped into one array type:

```typescript
type Result = ToArray<string | number>;

// (string | number)[]
```

# 🗁 Type-Level String Manipulation

TypeScript provides built-in string manipulation utilities.

```typescript
type UppercaseText = Uppercase<"hello">;

type LowercaseText = Lowercase<"HELLO">;

type CapitalizedText = Capitalize<"hello">;

type UncapitalizedText = Uncapitalize<"Hello">;
```

Results:

```typescript
type UppercaseText = "HELLO";

type LowercaseText = "hello";

type CapitalizedText = "Hello";

type UncapitalizedText = "hello";
```

These utilities can also be used with template literal types.

```typescript
type EventName<T extends string> = `on${Capitalize<T>}`;

type ClickEvent = EventName<"click">;

type SubmitEvent = EventName<"submit">;
```

Results:

```typescript
type ClickEvent = "onClick";

type SubmitEvent = "onSubmit";
```

# 🗁 Building a Type-Safe Event System

Template literal types, mapped types, and generics can work together to create strongly typed event names.

```typescript
type Events = {
  click: {
    x: number;
    y: number;
  };

  submit: {
    formId: string;
  };

  change: {
    value: string;
  };
};

type EventHandlers<T> = {
  [K in keyof T as `on${Capitalize<string & K>}`]:
    (payload: T[K]) => void;
};
```

Usage:

```typescript
type Handlers = EventHandlers<Events>;
```

Result:

```typescript
type Handlers = {
  onClick: (payload: {
    x: number;
    y: number;
  }) => void;

  onSubmit: (payload: {
    formId: string;
  }) => void;

  onChange: (payload: {
    value: string;
  }) => void;
};
```

The mapped type transforms every event key into a handler property while preserving its corresponding payload type.

# 🗁 Type-Level Utility Composition

Complex type-level operations are easier to understand when smaller utilities are combined.

Consider a type that extracts the return type of a function and makes its object properties optional.

```typescript
type MyReturnType<T> =
  T extends (...args: any[]) => infer R
    ? R
    : never;

type MyPartial<T> = {
  [K in keyof T]?: T[K];
};

type OptionalReturn<T> =
  MyPartial<MyReturnType<T>>;
```

Usage:

```typescript
function getUser() {
  return {
    id: 101,
    name: "Alex",
    email: "alex@example.com",
  };
}

type Result = OptionalReturn<typeof getUser>;
```

Result:

```typescript
type Result = {
  id?: number;
  name?: string;
  email?: string;
};
```

This demonstrates how multiple type-level utilities can be composed to create more advanced transformations.

# 🗁 Type-Level Programming with `satisfies`

The `satisfies` operator validates that an expression conforms to a type without replacing the expression's inferred type with that target type.

```typescript
type RouteConfig = Record<
  string,
  {
    path: string;
    requiresAuth: boolean;
  }
>;

const routes = {
  home: {
    path: "/",
    requiresAuth: false,
  },
  dashboard: {
    path: "/dashboard",
    requiresAuth: true,
  },
} satisfies RouteConfig;
```

You can still access the specific inferred properties:

```typescript
const homePath = routes.home.path;
```

The `satisfies` operator is not itself a type-level transformation, but it is useful when defining values that must conform to complex type-level contracts.

# 🗁 Type-Level Arithmetic

TypeScript does not support general-purpose arithmetic on numeric literal types in the same way JavaScript supports runtime arithmetic.

However, recursive tuple types can model certain arithmetic operations.

For example, you can represent a natural number using a tuple's length.

```typescript
type BuildTuple<
  Length extends number,
  Result extends unknown[] = []
> =
  Result["length"] extends Length
    ? Result
    : BuildTuple<Length, [...Result, unknown]>;
```

Usage:

```typescript
type Three = BuildTuple<3>;

type Five = BuildTuple<5>;
```

Results:

```typescript
type Three = [unknown, unknown, unknown];

type Five = [
  unknown,
  unknown,
  unknown,
  unknown,
  unknown
];
```

You can use tuple lengths to model addition:

```typescript
type Add<
  A extends number,
  B extends number
> = [
  ...BuildTuple<A>,
  ...BuildTuple<B>
]["length"];
```

Usage:

```typescript
type Result = Add<2, 3>;
```

Result:

```typescript
type Result = 5;
```

This technique is suitable for small natural-number calculations within the type system, not arbitrary numeric computation.

It also has practical limitations because recursive type evaluation can become expensive for large numbers.

# 🗁 Type-Level Constraints

Generic constraints allow you to restrict which types can be passed to a utility.

```typescript
type GetLength<T extends { length: number }> = T["length"];
```

Usage:

```typescript
type StringLength = GetLength<string[]>;

type TupleLength = GetLength<[string, number]>;
```

Results:

```typescript
type StringLength = number;

type TupleLength = 2;
```

The constraint ensures that the supplied type contains a `length` property compatible with `number`.

# 🗁 Type-Level Validation

Conditional types can validate whether a type satisfies a particular structure.

```typescript
type HasId<T> =
  T extends { id: number }
    ? true
    : false;
```

Usage:

```typescript
type A = HasId<{
  id: number;
  name: string;
}>;

type B = HasId<{
  name: string;
}>;
```

Results:

```typescript
type A = true;

type B = false;
```

You can use this pattern to build constraints for domain models, API responses, and configuration types.

# 🗁 Common Type-Level Programming Patterns

| Pattern | Main TypeScript Feature | Purpose |
| --- | --- | --- |
| Property transformation | Mapped types | Modify object properties |
| Type filtering | Conditional types | Remove unwanted union members |
| Type extraction | `infer` | Capture nested types |
| Key extraction | `keyof` | Obtain object property names |
| Property lookup | Indexed access | Extract property types |
| String construction | Template literal types | Build string literal types |
| Recursive transformation | Recursive conditional types | Process nested structures |
| Type classification | Conditional types | Determine type categories |
| Type composition | Generic utilities | Combine transformations |
| Domain distinction | Branded types | Prevent mixing semantic values |

# 🗁 Limitations and Performance Considerations

Type-level programming is powerful, but excessive complexity can make a TypeScript project difficult to maintain.

Important considerations:

* Deep recursive types can increase compiler workload.
* Complex conditional types can produce difficult error messages.
* Large unions can increase type-checking complexity.
* Recursive transformations may encounter compiler instantiation limits.
* Type assertions can bypass carefully designed constraints.
* Type-level computations do not replace runtime validation.
* Highly generic utilities can become difficult for other developers to understand.

Prefer straightforward types when a complex transformation does not provide meaningful value.

# 🗁 Best Practices

* Start with small, reusable generic utilities.
* Use meaningful names for generic parameters.
* Separate complex transformations into smaller types.
* Prefer built-in utility types when they already solve the problem.
* Use conditional types for type-level decisions.
* Use mapped types for object transformations.
* Use `infer` when extracting nested types.
* Use recursive types only when nested transformations are necessary.
* Test utilities against edge cases such as `never`, `unknown`, unions, and optional properties.
* Monitor compiler performance in large projects.
* Document complex type utilities with examples.
* Keep runtime validation separate from compile-time type checking.

# 🗁 Summary

Type-level programming uses TypeScript's type system to perform computations and transformations on types at compile time.

Key concepts:

* Generics provide reusable type parameters.
* Conditional types provide type-level decision-making.
* Mapped types transform object properties.
* `keyof` extracts property keys.
* Indexed access extracts property types.
* `infer` captures nested types.
* Template literal types construct string types.
* Recursive types process nested structures.
* Distributive conditional types transform union members independently.
* Branded types distinguish semantically different values.
* Tuple types can model limited arithmetic operations.

Type-level programming is an important foundation for building advanced TypeScript libraries, reusable utilities, type-safe APIs, and large-scale applications.
