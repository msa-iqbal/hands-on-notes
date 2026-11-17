# Recursive Types

Recursive types in TypeScript are types that reference themselves directly or indirectly.

They allow you to describe data structures with repeating or nested relationships, such as:

* Tree structures.
* Linked lists.
* Nested objects.
* JSON data.
* Hierarchical categories.
* Nested comments.
* File systems.
* Recursive API responses.

Recursive types are particularly useful when the depth or number of nested elements is not known in advance.

# 🗁 Basic Syntax

A recursive type references itself within its own definition.

```typescript
type RecursiveType = {
  value: string;
  next?: RecursiveType;
};
```

The `RecursiveType` definition refers to itself through the optional `next` property.

Example:

```typescript
const first: RecursiveType = {
  value: "First",
  next: {
    value: "Second",
    next: {
      value: "Third"
    }
  }
};
```

The structure can continue to any depth allowed by the runtime data.

# 🗁 Recursive Object Types

You can create nested object types by referencing the same type inside its properties.

```typescript
type Category = {
  id: number;
  name: string;
  children?: Category[];
};
```

Usage:

```typescript
const categories: Category[] = [
  {
    id: 1,
    name: "Programming",
    children: [
      {
        id: 2,
        name: "JavaScript",
        children: [
          {
            id: 3,
            name: "TypeScript"
          }
        ]
      }
    ]
  }
];
```

Each category can contain additional categories.

This structure is useful for navigation menus, product categories, organizational hierarchies, and nested data.

# 🗁 Recursive Array Types

You can define arrays that contain values of their own recursive type.

```typescript
type NestedNumber = number | NestedNumber[];
```

Usage:

```typescript
const numbers: NestedNumber = [
  1,
  2,
  [
    3,
    4,
    [
      5,
      6
    ]
  ]
];
```

The type allows either a number or another array of `NestedNumber` values.

You can also create recursive string arrays:

```typescript
type NestedString = string | NestedString[];

const values: NestedString = [
  "JavaScript",
  [
    "TypeScript",
    [
      "React",
      "Node.js"
    ]
  ]
];
```

# 🗁 Recursive JSON Types

JSON supports nested objects and arrays, making it a natural use case for recursive types.

A JSON value can be a string, number, boolean, `null`, array, or object containing other JSON values.

```typescript
type JSONValue =
  | string
  | number
  | boolean
  | null
  | JSONValue[]
  | {
      [key: string]: JSONValue;
    };
```

Usage:

```typescript
const data: JSONValue = {
  name: "John",
  age: 25,
  active: true,
  address: {
    city: "Dhaka",
    country: "Bangladesh"
  },
  skills: [
    "JavaScript",
    "TypeScript",
    {
      name: "React",
      experience: 3
    }
  ]
};
```

This type supports arbitrarily nested JSON-compatible structures.

# 🗁 Recursive Tree Types

Tree structures consist of nodes that can contain additional child nodes.

```typescript
type TreeNode = {
  value: string;
  children: TreeNode[];
};
```

Example:

```typescript
const tree: TreeNode = {
  value: "Root",
  children: [
    {
      value: "Frontend",
      children: [
        {
          value: "React",
          children: []
        },
        {
          value: "Vue",
          children: []
        }
      ]
    },
    {
      value: "Backend",
      children: [
        {
          value: "Node.js",
          children: []
        }
      ]
    }
  ]
};
```

Each node has the same structure, allowing the tree to grow to any depth.

# 🗁 Recursive Linked Lists

A linked list is a data structure in which each node contains a value and a reference to the next node.

```typescript
type LinkedList<T> = {
  value: T;
  next: LinkedList<T> | null;
};
```

Usage:

```typescript
const numbers: LinkedList<number> = {
  value: 10,
  next: {
    value: 20,
    next: {
      value: 30,
      next: null
    }
  }
};
```

The generic type parameter `T` allows the linked list to store different value types.

```typescript
type StringList = LinkedList<string>;

type NumberList = LinkedList<number>;
```

# 🗁 Recursive Types with Generics

Recursive types can be combined with generics to create reusable structures.

```typescript
type TreeNode<T> = {
  value: T;
  children: TreeNode<T>[];
};
```

Usage:

```typescript
const numberTree: TreeNode<number> = {
  value: 1,
  children: [
    {
      value: 2,
      children: []
    },
    {
      value: 3,
      children: []
    }
  ]
};
```

The same type can represent different kinds of tree data.

```typescript
type StringTree = TreeNode<string>;

type BooleanTree = TreeNode<boolean>;
```

# 🗁 Recursive Types with Union Types

Recursive types frequently use union types to represent multiple possible values.

```typescript
type Expression =
  | number
  | {
      operator: "+" | "-" | "*" | "/";
      left: Expression;
      right: Expression;
    };
```

Usage:

```typescript
const expression: Expression = {
  operator: "+",
  left: 10,
  right: {
    operator: "*",
    left: 5,
    right: 2
  }
};
```

This represents a mathematical expression as a recursive tree.

The `Expression` type can represent either a number or another expression containing two child expressions.

# 🗁 Recursive Conditional Types

TypeScript supports recursive conditional types, allowing a type to repeatedly transform itself.

Example:

```typescript
type DeepArrayElement<T> =
  T extends readonly (infer U)[]
    ? DeepArrayElement<U>
    : T;
```

Usage:

```typescript
type Result = DeepArrayElement<
  number[][][]
>;
```

Result:

```typescript
type Result = number;
```

The conditional type repeatedly extracts array element types until it reaches a non-array type.

This pattern is useful for deeply nested arrays and similar structures.

# 🗁 Recursive Promise Types

You can create a recursive utility type to unwrap nested promises.

```typescript
type UnwrapPromise<T> =
  T extends Promise<infer U>
    ? UnwrapPromise<U>
    : T;
```

Usage:

```typescript
type Result = UnwrapPromise<
  Promise<Promise<Promise<string>>>
>;
```

Result:

```typescript
type Result = string;
```

The type recursively unwraps each promise layer.

For normal promise unwrapping, prefer the built-in `Awaited<T>` utility type.

# 🗁 Deep Readonly Types

You can create a recursive utility type that makes every nested object property readonly.

```typescript
type DeepReadonly<T> =
  T extends (...args: any[]) => any
    ? T
    : T extends readonly unknown[]
      ? {
          readonly [K in keyof T]: DeepReadonly<T[K]>
        }
      : T extends object
        ? {
            readonly [K in keyof T]: DeepReadonly<T[K]>
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

The function branch preserves function types, while the array branch recursively transforms array or tuple elements.

This is a simplified utility type. Production implementations may need additional handling for special objects such as `Date`, `Map`, `Set`, and class instances.

# 🗁 Deep Partial Types

The built-in `Partial<T>` makes only the top-level properties optional.

A recursive version can make nested properties optional as well.

```typescript
type DeepPartial<T> =
  T extends (...args: any[]) => any
    ? T
    : T extends readonly unknown[]
      ? {
          [K in keyof T]?: DeepPartial<T[K]>
        }
      : T extends object
        ? {
            [K in keyof T]?: DeepPartial<T[K]>
          }
        : T;
```

Usage:

```typescript
type User = {
  id: number;
  profile: {
    name: string;
    email: string;
  };
};

type PartialUser = DeepPartial<User>;
```

Example:

```typescript
const user: PartialUser = {
  profile: {
    name: "John"
  }
};
```

Both the top-level and nested properties are optional.

This simplified implementation treats arrays and tuples as mapped object structures. For application-specific utilities, define the desired array and tuple behavior explicitly.

# 🗁 Recursive Type for Nested Object Paths

You can use recursive conditional types to generate paths to nested object properties.

```typescript
type PropertyPath<T> = {
  [K in keyof T & string]:
    T[K] extends object
      ? K | `${K}.${PropertyPath<T[K]>}`
      : K
}[keyof T & string];
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

type UserPaths = PropertyPath<User>;
```

Result:

```typescript
type UserPaths =
  | "id"
  | "profile"
  | "profile.name"
  | "profile.address"
  | "profile.address.city"
  | "profile.address.country";
```

This pattern is useful for:

* Form field paths.
* Nested configuration access.
* Object validation.
* Type-safe property selectors.

The example assumes string-keyed object properties and is not designed to handle every special object type or recursive structure.

# 🗁 Recursive Type for File Systems

A file system contains files and directories. A directory can contain additional files and directories.

```typescript
type File = {
  type: "file";
  name: string;
  size: number;
};

type Directory = {
  type: "directory";
  name: string;
  children: FileSystemEntry[];
};

type FileSystemEntry = File | Directory;
```

Usage:

```typescript
const root: Directory = {
  type: "directory",
  name: "project",
  children: [
    {
      type: "file",
      name: "package.json",
      size: 1024
    },
    {
      type: "directory",
      name: "src",
      children: [
        {
          type: "file",
          name: "index.ts",
          size: 2048
        }
      ]
    }
  ]
};
```

This example uses a recursive discriminated union to model a realistic file system structure.

# 🗁 Recursive Type for Nested Comments

Comment systems often support replies to existing comments.

```typescript
type Comment = {
  id: number;
  author: string;
  message: string;
  replies: Comment[];
};
```

Usage:

```typescript
const comments: Comment[] = [
  {
    id: 1,
    author: "John",
    message: "Great article!",
    replies: [
      {
        id: 2,
        author: "Alex",
        message: "I agree!",
        replies: [
          {
            id: 3,
            author: "Sam",
            message: "Thanks for sharing.",
            replies: []
          }
        ]
      }
    ]
  }
];
```

This structure supports threaded discussions with nested replies.

# 🗁 Recursive Types and Type-Level Recursion

Recursive types are not limited to describing runtime data structures. They can also perform compile-time type transformations.

Consider a type that reverses a tuple:

```typescript
type Reverse<T extends unknown[]> =
  T extends [infer First, ...infer Rest]
    ? [...Reverse<Rest>, First]
    : [];
```

Usage:

```typescript
type Original = [string, number, boolean];

type Reversed = Reverse<Original>;
```

Result:

```typescript
type Reversed = [boolean, number, string];
```

The type repeatedly extracts the first element and recursively processes the remaining tuple.

A readonly-compatible version can be written as:

```typescript
type ReverseReadonly<T extends readonly unknown[]> =
  T extends readonly [infer First, ...infer Rest]
    ? [...ReverseReadonly<Rest>, First]
    : [];
```

# 🗁 Recursive Type Instantiation Limits

Recursive types can create increasingly complex type computations.

TypeScript has compiler safeguards that prevent excessive type instantiation and infinite type expansion.

For example, an incorrectly designed recursive type may repeatedly expand without reaching a terminating condition.

```typescript
type Infinite<T> = Infinite<T>;
```

This type cannot produce a useful resolved result and causes a compiler error.

A valid recursive conditional type should have a terminating condition.

```typescript
type Flatten<T> =
  T extends readonly (infer U)[]
    ? Flatten<U>
    : T;
```

For finite nested arrays, recursion eventually reaches a non-array type.

Important considerations:

* Keep recursive transformations as simple as possible.
* Ensure recursive branches can eventually terminate.
* Avoid unnecessary nested conditional types.
* Be careful when processing very large unions or deeply nested structures.
* Test complex utility types against realistic input types.
* Consider compiler performance when designing reusable type-level utilities.

# 🗁 Common Mistakes

**Forgetting the recursive base case**

A recursive conditional type should have a branch that stops recursion.

```typescript
type Flatten<T> =
  T extends readonly (infer U)[]
    ? Flatten<U>
    : T;
```

The non-array branch provides the termination condition.

**Using an incorrect recursive property type**

```typescript
type Category = {
  name: string;
  children: Category[];
};
```

Every category must provide a `children` array, even when it is empty.

If children are optional, define them accordingly:

```typescript
type Category = {
  name: string;
  children?: Category[];
};
```

**Confusing recursive types with recursive functions**

Recursive types describe relationships between types. They do not automatically create recursive runtime behavior.

```typescript
type TreeNode = {
  value: number;
  children: TreeNode[];
};
```

A separate function is needed to traverse the tree at runtime.

**Ignoring special object types**

A generic recursive transformation using `T extends object` may unintentionally transform classes, dates, maps, sets, and other built-in objects.

Handle special cases explicitly when building production utility types.

# 🗁 Best Practices

* Use recursive types for naturally nested data structures.
* Use generics to make recursive structures reusable.
* Use discriminated unions when recursive structures have multiple node variants.
* Include clear termination conditions in recursive conditional types.
* Use built-in utility types when they already solve the problem.
* Handle readonly arrays and tuples when designing reusable recursive utilities.
* Consider special object types when implementing `DeepReadonly` or `DeepPartial`.
* Avoid unnecessary recursion in complex type-level transformations.
* Test recursive utilities with empty, nested, readonly, and union-based inputs.

# 🗁 Summary

Recursive types allow TypeScript types to reference themselves, making it possible to model nested and hierarchical structures.

Key concepts:

* Recursive object types describe nested objects.
* Recursive arrays represent arbitrarily nested collections.
* Recursive unions describe structures with multiple possible node types.
* Generic recursive types create reusable data structures.
* Recursive conditional types perform repeated compile-time transformations.
* Recursive mapped types support utilities such as `DeepReadonly` and `DeepPartial`.
* Recursive tuple types can perform transformations such as reversing tuples.
* TypeScript compiler safeguards limit excessive recursive type instantiation.

Recursive types are essential when working with complex application data, hierarchical structures, and advanced type-level programming.
