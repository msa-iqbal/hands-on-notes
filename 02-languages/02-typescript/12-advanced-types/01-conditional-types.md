# Conditional Types

Conditional types are a powerful TypeScript feature that allows types to be selected based on a condition.

They work similarly to conditional expressions in JavaScript, but operate at the type level rather than at runtime.

A conditional type checks whether one type is assignable to another and produces one of two possible types depending on the result.

Conditional types are especially useful for building reusable utility types, implementing advanced generic types, and creating flexible type transformations.

# 🗁 Syntax

```typescript
type Result = SomeType extends AnotherType
  ? TrueType
  : FalseType;
```

* `SomeType`: The type being checked.
* `extends`: Checks whether the first type is assignable to the second type.
* `TrueType`: The resulting type when the condition is true.
* `FalseType`: The resulting type when the condition is false.

# 🗁 Basic Example

```typescript
type IsString<T> = T extends string ? true : false;

type A = IsString<string>;
type B = IsString<number>;

// A: true
// B: false
```

TypeScript evaluates the condition for each supplied type.

* `string` extends `string`, so the result is `true`.
* `number` does not extend `string`, so the result is `false`.

# 🗁 Conditional Types with Generics

Conditional types become especially useful when combined with generics.

```typescript
type CheckType<T> = T extends number
  ? "Numeric"
  : "Non-numeric";

type A = CheckType<number>;
type B = CheckType<string>;
type C = CheckType<boolean>;

// A: "Numeric"
// B: "Non-numeric"
// C: "Non-numeric"
```

The result changes depending on the type supplied to `T`.

# 🗁 Checking Object Types

You can use conditional types to determine whether a type satisfies a particular object structure.

```typescript
type HasName<T> = T extends { name: string }
  ? true
  : false;

type User = {
  id: number;
  name: string;
};

type Product = {
  id: number;
  price: number;
};

type A = HasName<User>;
type B = HasName<Product>;

// A: true
// B: false
```

Because TypeScript uses structural typing, a type can satisfy the condition even if it contains additional properties.

# 🗁 Conditional Types with Union Types

Conditional types can evaluate union types.

```typescript
type Check<T> = T extends string
  ? "String"
  : "Other";

type Result = Check<string | number>;
```

When the checked type is a naked type parameter, TypeScript distributes the conditional over each union member.

The evaluation is:

```typescript
type Result =
  | (string extends string ? "String" : "Other")
  | (number extends string ? "String" : "Other");

// Result:
// "String" | "Other"
```

This behavior is called a distributive conditional type.

# 🗁 Non-Distributive Conditional Types

You can prevent distributive behavior by wrapping the checked type in a tuple.

```typescript
type Check<T> = [T] extends [string]
  ? "String"
  : "Other";

type Result = Check<string | number>;

// Result:
// "Other"
```

Instead of checking each union member independently, TypeScript checks the entire union as one type.

Because `string | number` is not assignable to `string`, the result is `"Other"`.

# 🗁 Nested Conditional Types

You can combine multiple conditions to create more advanced type transformations.

```typescript
type DataType<T> =
  T extends string
    ? "Text"
    : T extends number
      ? "Numeric"
      : T extends boolean
        ? "Boolean"
        : "Unknown";

type A = DataType<string>;
type B = DataType<number>;
type C = DataType<boolean>;
type D = DataType<object>;

// A: "Text"
// B: "Numeric"
// C: "Boolean"
// D: "Unknown"
```

Nested conditional types are useful when several possible type conditions must be evaluated.

# 🗁 Conditional Types with Never

The `never` type represents a type with no possible values.

It is commonly used in conditional types to remove unwanted union members.

```typescript
type RemoveString<T> = T extends string
  ? never
  : T;

type Result = RemoveString<string | number | boolean>;

// Result:
// number | boolean
```

The string member becomes `never` and disappears from the resulting union.

This is the fundamental idea behind the built-in `Exclude` utility type.

```typescript
type MyExclude<T, U> = T extends U ? never : T;

type Result = MyExclude<
  string | number | boolean,
  string
>;

// Result:
// number | boolean
```

# 🗁 Conditional Types with Infer

The `infer` keyword allows TypeScript to infer and capture a type inside a conditional type.

For example, you can extract the return type of a function.

```typescript
type MyReturnType<T> =
  T extends (...args: any[]) => infer R
    ? R
    : never;

type Calculate = (a: number, b: number) => number;

type Result = MyReturnType<Calculate>;

// Result:
// number
```

Here, `infer R` captures the function's return type.

The built-in `ReturnType` utility uses this technique.

# 🗁 Conditional Types with Arrays

You can use conditional types to inspect array types.

```typescript
type ElementType<T> =
  T extends (infer U)[]
    ? U
    : T;

type A = ElementType<string[]>;
type B = ElementType<number[]>;
type C = ElementType<boolean>;

// A: string
// B: number
// C: boolean
```

The conditional type extracts the element type when the supplied type is an array.

# 🗁 Conditional Types with Tuples

Conditional types can also inspect tuple structures.

```typescript
type FirstElement<T> =
  T extends [infer First, ...unknown[]]
    ? First
    : never;

type A = FirstElement<[string, number, boolean]>;
type B = FirstElement<[number, string]>;

// A: string
// B: number
```

The `infer First` expression captures the first tuple element's type.

# 🗁 Conditional Types with Functions

You can check whether a type represents a function.

```typescript
type IsFunction<T> = T extends (...args: any[]) => any
  ? true
  : false;

type A = IsFunction<() => void>;
type B = IsFunction<string>;
type C = IsFunction<(value: number) => string>;

// A: true
// B: false
// C: true
```

This pattern is useful when creating generic APIs that behave differently depending on whether a supplied type is callable.

# 🗁 Conditional Types with Constraints

Conditional types can work alongside generic constraints.

```typescript
type GetId<T extends { id: unknown }> =
  T extends { id: number }
    ? number
    : never;

type User = {
  id: number;
  name: string;
};

type Product = {
  id: string;
  price: number;
};

type A = GetId<User>;
type B = GetId<Product>;

// A: number
// B: never
```

The generic constraint ensures that every supplied type has an `id` property. The conditional then checks whether that property is compatible with `number`.

# 🗁 Practical Example: Extracting Promise Values

You can build a utility type that extracts the resolved value of a promise.

```typescript
type MyAwaited<T> =
  T extends Promise<infer U>
    ? MyAwaited<U>
    : T;

type A = MyAwaited<Promise<string>>;
type B = MyAwaited<Promise<Promise<number>>>;

// A: string
// B: number
```

This demonstrates how conditional types, `infer`, and recursion can work together.

The built-in `Awaited` utility provides more comprehensive handling of promise-like types.

# 🗁 Practical Example: Type-Safe API Responses

Suppose an application has different API response structures depending on whether an operation succeeds or fails.

```typescript
type ApiResponse<T> =
  T extends string
    ? {
        status: "success";
        message: T;
      }
    : {
        status: "data";
        data: T;
      };

type MessageResponse = ApiResponse<string>;

type UserResponse = ApiResponse<{
  id: number;
  name: string;
}>;
```

The resulting types are:

```typescript
// MessageResponse:
{
  status: "success";
  message: string;
}

// UserResponse:
{
  status: "data";
  data: {
    id: number;
    name: string;
  };
}
```

A single generic type can therefore produce different structures based on its input type.

# 🗁 Important Behavior

**Conditional types perform assignability checks, not runtime comparisons.**

```typescript
type Result = number extends string
  ? "Yes"
  : "No";

// Result:
// "No"
```

No JavaScript code is executed.

**Distributive behavior applies to naked type parameters.**

```typescript
type Check<T> = T extends string ? true : false;

type Result = Check<string | number>;

// Result:
// true | false
```

Wrapping the type parameter in a tuple prevents distribution.

```typescript
type Check<T> = [T] extends [string] ? true : false;

type Result = Check<string | number>;

// Result:
// false
```

**Conditional types can defer evaluation.**

When a condition depends on an unresolved generic type parameter, TypeScript may defer deciding the result until the type parameter is known.

```typescript
type Result<T> = T extends string
  ? string[]
  : number[];
```

The final type depends on the type supplied to `T`.

**The `never` type has special distributive behavior.**

```typescript
type Check<T> = T extends string ? true : false;

type Result = Check<never>;

// Result:
// never
```

Because distributive conditional types evaluate union members individually and `never` represents an empty union, the result is `never`.

# 🗁 Best Practices

* Use conditional types when a resulting type depends on another type.
* Combine conditional types with generics for reusable type transformations.
* Use `infer` to extract nested type information.
* Understand distributive behavior when working with union types.
* Wrap type parameters in tuples when distribution is not desired.
* Use `never` to filter unwanted union members.
* Keep deeply nested conditional types readable by extracting smaller helper types.
* Prefer built-in utility types when they already solve the problem.

# 🗁 Summary

* Conditional types select a type based on an assignability condition.
* They use the `extends`, `?`, and `:` operators.
* They work with generics, unions, arrays, tuples, objects, and functions.
* Distributive conditional types evaluate union members individually.
* Tuple wrapping prevents distributive behavior.
* The `infer` keyword extracts types inside conditional expressions.
* The `never` type is useful for filtering union members.
* Conditional types are a foundation of advanced TypeScript utility types.
