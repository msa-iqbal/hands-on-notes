# Distributive Conditional Types

Distributive conditional types are a feature of TypeScript that allows conditional types to be applied individually to each member of a union type.

When a conditional type operates on a naked generic type parameter, TypeScript distributes the condition across the members of the union and combines the results into a new union.

This behavior is particularly useful for:

* Filtering union types.
* Transforming individual union members.
* Extracting specific types.
* Creating reusable utility types.
* Building advanced generic type transformations.

# 🗁 Basic Syntax

A distributive conditional type uses a generic type parameter directly on the left side of the `extends` keyword.

```typescript
type MyType<T> =
  T extends SomeType
    ? TrueType
    : FalseType;
```

When `T` is a union, TypeScript evaluates the condition separately for each union member.

# 🗁 Basic Example

Consider a union of different primitive types:

```typescript
type CheckType<T> =
  T extends string
    ? "String"
    : "Other";

type Result = CheckType<string | number | boolean>;
```

TypeScript distributes the conditional type:

```typescript
type Result =
  | CheckType<string>
  | CheckType<number>
  | CheckType<boolean>;
```

Each member is evaluated independently:

```typescript
type Result =
  | "String"
  | "Other"
  | "Other";
```

Duplicate union members are automatically eliminated.

Final result:

```typescript
type Result = "String" | "Other";
```

# 🗁 How Distribution Works

Consider the following conditional type:

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

Because `T` is a naked type parameter, TypeScript distributes the operation:

```typescript
type Result =
  | ToArray<string>
  | ToArray<number>;
```

Final result:

```typescript
type Result = string[] | number[];
```

This is different from creating an array containing a union type:

```typescript
type Result = (string | number)[];
```

The difference is:

```typescript
type Distributed = string[] | number[];

type NonDistributed = (string | number)[];
```

`Distributed` allows either an array of strings or an array of numbers.

`NonDistributed` allows one array containing both strings and numbers.

# 🗁 Naked Type Parameters

Distribution occurs when the checked type is a naked generic type parameter.

A naked type parameter appears directly before `extends`, without being wrapped in another type expression.

Distributive example:

```typescript
type Distribute<T> =
  T extends string
    ? T[]
    : never;

type Result = Distribute<string | number>;
```

Result:

```typescript
type Result = string[];
```

Non-distributive example:

```typescript
type NonDistribute<T> =
  [T] extends [string]
    ? T[]
    : never;

type Result = NonDistribute<string | number>;
```

Result:

```typescript
type Result = never;
```

Here, `[T]` is checked as a complete type rather than distributing over its individual union members.

# 🗁 Preventing Distribution

You can prevent distribution by wrapping the checked type parameter in a tuple.

```typescript
type IsString<T> =
  [T] extends [string]
    ? true
    : false;
```

Usage:

```typescript
type A = IsString<string>;

type B = IsString<number>;

type C = IsString<string | number>;
```

Result:

```typescript
type A = true;

type B = false;

type C = false;
```

The complete union `string | number` must extend `string` for the condition to succeed.

This is useful when you want to test a union as a whole.

# 🗁 Filtering Union Members

One of the most common applications of distributive conditional types is filtering union members.

```typescript
type FilterStrings<T> =
  T extends string
    ? T
    : never;
```

Usage:

```typescript
type Mixed =
  | string
  | number
  | boolean
  | null;

type StringValues = FilterStrings<Mixed>;
```

Result:

```typescript
type StringValues = string;
```

Explanation:

* `string` matches the condition and remains.
* `number` becomes `never`.
* `boolean` becomes `never`.
* `null` becomes `never`.

Because `never` is removed from unions, only matching members remain.

# 🗁 Excluding Union Members

You can use distributive conditional types to remove specific members.

```typescript
type MyExclude<T, U> =
  T extends U
    ? never
    : T;
```

Usage:

```typescript
type Status =
  | "pending"
  | "success"
  | "failed";

type ActiveStatus = MyExclude<Status, "failed">;
```

Result:

```typescript
type ActiveStatus =
  | "pending"
  | "success";
```

This is the underlying pattern used by TypeScript's built-in `Exclude<T, U>` utility type.

# 🗁 Extracting Union Members

You can also keep only the union members that match a specified type.

```typescript
type MyExtract<T, U> =
  T extends U
    ? T
    : never;
```

Usage:

```typescript
type Values =
  | string
  | number
  | boolean
  | null;

type PrimitiveNumbers = MyExtract<Values, number | boolean>;
```

Result:

```typescript
type PrimitiveNumbers = number | boolean;
```

This follows the same pattern as the built-in `Extract<T, U>` utility type.

# 🗁 Filtering Object Union Members

Distributive conditional types can filter complex object unions.

```typescript
type User =
  | {
      type: "admin";
      permissions: string[];
    }
  | {
      type: "customer";
      loyaltyPoints: number;
    }
  | {
      type: "guest";
      temporary: boolean;
    };
```

Create a utility type that extracts a particular object variant:

```typescript
type ExtractByType<T, K> =
  T extends { type: K }
    ? T
    : never;
```

Usage:

```typescript
type Admin = ExtractByType<User, "admin">;
```

Result:

```typescript
type Admin = {
  type: "admin";
  permissions: string[];
};
```

The condition is evaluated separately for each member of the `User` union.

# 🗁 Extracting Function Types

You can use distribution to extract specific function signatures from a union.

```typescript
type Functions =
  | (() => string)
  | ((id: number) => number)
  | ((active: boolean) => boolean);

type StringFunctions<T> =
  T extends (...args: any[]) => string
    ? T
    : never;
```

Usage:

```typescript
type Result = StringFunctions<Functions>;
```

Result:

```typescript
type Result = () => string;
```

Only the function returning `string` remains.

# 🗁 Distribution with `never`

The `never` type represents values that cannot occur.

In distributive conditional types, `never` is particularly useful for filtering union members.

```typescript
type RemoveNumbers<T> =
  T extends number
    ? never
    : T;

type Result = RemoveNumbers<string | number | boolean>;
```

Result:

```typescript
type Result = string | boolean;
```

The `number` member becomes `never` and disappears from the resulting union.

An important detail is that a distributive conditional type applied directly to `never` evaluates to `never`.

```typescript
type IsString<T> =
  T extends string
    ? true
    : false;

type Result = IsString<never>;
```

Result:

```typescript
type Result = never;
```

This happens because `never` behaves like an empty union for distributive conditional types.

If you need to check whether a type is exactly `never`, prevent distribution:

```typescript
type IsNever<T> =
  [T] extends [never]
    ? true
    : false;

type Result = IsNever<never>;
```

Result:

```typescript
type Result = true;
```

# 🗁 Distribution with `unknown`

The `unknown` type represents a value whose specific type is not yet known.

You can use distributive conditional types to inspect union members even when the union includes `unknown`.

```typescript
type Check<T> =
  T extends string
    ? "String"
    : "Other";

type Result = Check<string | number | unknown>;
```

Result:

```typescript
type Result = "Other";
```

The union simplifies to `unknown` because `unknown` absorbs other types in a union.

Therefore, the conditional type effectively evaluates `unknown`.

# 🗁 Nested Distributive Conditional Types

You can combine multiple conditional types to perform more complex transformations.

```typescript
type TypeCategory<T> =
  T extends string
    ? "Text"
    : T extends number
      ? "Numeric"
      : T extends boolean
        ? "Boolean"
        : "Other";
```

Usage:

```typescript
type Values =
  | string
  | number
  | boolean
  | null;

type Categories = TypeCategory<Values>;
```

Result:

```typescript
type Categories =
  | "Text"
  | "Numeric"
  | "Boolean"
  | "Other";
```

Each union member is evaluated independently through the nested conditions.

# 🗁 Combining Distribution with `infer`

You can combine distributive conditional types with `infer` to extract information from every member of a union.

```typescript
type UnwrapArray<T> =
  T extends (infer U)[]
    ? U
    : T;
```

Usage:

```typescript
type Values =
  | string[]
  | number[]
  | boolean;

type Result = UnwrapArray<Values>;
```

Result:

```typescript
type Result = string | number | boolean;
```

TypeScript processes each union member:

* `string[]` becomes `string`.
* `number[]` becomes `number`.
* `boolean` remains `boolean`.

# 🗁 Distribution with Mapped Types

You can combine distributive conditional types with mapped types to transform each member of a union.

```typescript
type Nullable<T> =
  T extends object
    ? {
        [K in keyof T]: T[K] | null;
      }
    : T;
```

Usage:

```typescript
type User =
  | {
      type: "admin";
      permissions: string[];
    }
  | {
      type: "customer";
      name: string;
    };

type NullableUser = Nullable<User>;
```

Result:

```typescript
type NullableUser =
  | {
      type: "admin" | null;
      permissions: string[] | null;
    }
  | {
      type: "customer" | null;
      name: string | null;
    };
```

Each object variant is transformed independently.

Notice that the discriminant property also becomes nullable. In real applications, you may want to preserve discriminant properties instead of transforming every property.

# 🗁 Practical Example: API Response Filtering

Suppose an API can return several response types.

```typescript
type ApiResponse =
  | {
      status: "success";
      data: {
        id: number;
        name: string;
      };
    }
  | {
      status: "error";
      message: string;
    }
  | {
      status: "loading";
    };
```

You can extract only successful responses:

```typescript
type SuccessResponse<T> =
  T extends { status: "success" }
    ? T
    : never;

type SuccessfulApiResponse =
  SuccessResponse<ApiResponse>;
```

Result:

```typescript
type SuccessfulApiResponse = {
  status: "success";
  data: {
    id: number;
    name: string;
  };
};
```

This is useful when building strongly typed API response handlers.

# 🗁 Practical Example: Extracting Promise Types from a Union

Suppose you have several possible asynchronous result types.

```typescript
type Results =
  | Promise<string>
  | Promise<number>
  | boolean;
```

You can extract the resolved promise values:

```typescript
type ResolvePromises<T> =
  T extends Promise<infer U>
    ? U
    : T;

type ResolvedResults = ResolvePromises<Results>;
```

Result:

```typescript
type ResolvedResults =
  | string
  | number
  | boolean;
```

Each union member is independently inspected and transformed.

# 🗁 Practical Example: Creating a Type Filter

You can build a reusable utility that selects only object types from a union.

```typescript
type ObjectTypes<T> =
  T extends object
    ? T
    : never;
```

Usage:

```typescript
type Mixed =
  | string
  | number
  | {
      id: number;
    }
  | {
      name: string;
    };

type Objects = ObjectTypes<Mixed>;
```

Result:

```typescript
type Objects =
  | {
      id: number;
    }
  | {
      name: string;
    };
```

This can be useful when processing unions that contain both primitive and object types.

# 🗁 Practical Example: Creating a Non-Nullable Union

You can remove `null` and `undefined` from a union using a distributive conditional type.

```typescript
type MyNonNullable<T> =
  T extends null | undefined
    ? never
    : T;
```

Usage:

```typescript
type UserInput =
  | string
  | number
  | null
  | undefined;

type ValidInput = MyNonNullable<UserInput>;
```

Result:

```typescript
type ValidInput = string | number;
```

TypeScript provides the built-in `NonNullable<T>` utility type for this operation.

# 🗁 Distributive vs Non-Distributive Conditional Types

Understanding the difference is essential when building advanced utility types.

| Feature | Distributive | Non-distributive |
| --- | --- | --- |
| Checked type | Naked generic parameter | Wrapped generic parameter |
| Union behavior | Processes each member | Processes the entire union |
| Syntax | `T extends U` | `[T] extends [U]` |
| Typical use | Filtering and transforming unions | Checking a union as a whole |

Example:

```typescript
type Distributed<T> =
  T extends string
    ? true
    : false;

type NonDistributed<T> =
  [T] extends [string]
    ? true
    : false;
```

Usage:

```typescript
type A = Distributed<string | number>;

type B = NonDistributed<string | number>;
```

Result:

```typescript
type A = true | false;

type B = false;
```

The first type distributes across the union members. The second checks the complete union.

# 🗁 Common Mistakes

**Assuming every conditional type distributes**

```typescript
type Check<T> =
  [T] extends [string]
    ? true
    : false;
```

This does not distribute because `T` is wrapped in a tuple.

**Forgetting that `never` disappears from unions**

```typescript
type Filter<T> =
  T extends string
    ? T
    : never;
```

The `never` results are eliminated from the final union.

**Confusing an array of a union with a union of arrays**

```typescript
type A = (string | number)[];

type B = string[] | number[];
```

These types represent different structures.

**Expecting object unions to be transformed as a single object**

When a conditional type distributes over a union, each member is processed independently. This behavior can preserve separate object variants rather than merging them.

# 🗁 Best Practices

* Use distributive conditional types when processing union members independently.
* Use `never` to remove unwanted union members.
* Use tuple wrapping to prevent distribution when checking an entire union.
* Combine distribution with `infer` for extracting nested types.
* Prefer built-in utility types such as `Exclude`, `Extract`, and `NonNullable` when appropriate.
* Pay attention to how `never` and `unknown` behave in union types.
* Test complex conditional types with single types, unions, `never`, and `unknown`.
* Keep complex transformations readable by separating them into smaller utility types.

# 🗁 Summary

Distributive conditional types allow TypeScript to evaluate conditional types independently for every member of a union.

Key concepts:

* Distribution occurs when a naked generic type parameter is checked with `extends`.
* Each union member is processed independently.
* `never` can remove unwanted members from a resulting union.
* `infer` can extract information from individual union members.
* Tuple wrapping such as `[T] extends [U]` prevents distribution.
* `Exclude`, `Extract`, and `NonNullable` use related conditional type patterns.
* Distribution is useful for filtering, transforming, and extracting union members.

Distributive conditional types are a fundamental concept in advanced TypeScript type-level programming.
