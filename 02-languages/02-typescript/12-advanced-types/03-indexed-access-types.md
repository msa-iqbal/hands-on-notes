# Indexed Access Types

Indexed access types allow you to retrieve the type of a specific property from another type using its key.

They use square bracket notation (`T[K]`) and are particularly useful when you want to reuse property types without defining them again.

Indexed access types help create reusable, maintainable, and strongly typed applications.

# 🗁 Basic Syntax

The basic syntax is:

```typescript
type PropertyType = Type["property"];
```

Consider the following example:

```typescript
type User = {
  id: number;
  name: string;
  email: string;
};

type UserId = User["id"];
type UserName = User["name"];
type UserEmail = User["email"];
```

The resulting types are:

```typescript
type UserId = number;
type UserName = string;
type UserEmail = string;
```

TypeScript looks up the specified property and returns its corresponding type.

# 🗁 Accessing Multiple Properties

You can access multiple property types by providing a union of property keys.

```typescript
type User = {
  id: number;
  name: string;
  age: number;
  email: string;
};

type UserDetails = User["name" | "age"];
```

Result:

```typescript
type UserDetails = string | number;
```

The result is a union of the selected properties' value types.

You can also use `keyof` to retrieve all property value types:

```typescript
type User = {
  id: number;
  name: string;
  active: boolean;
};

type UserValues = User[keyof User];
```

Result:

```typescript
type UserValues = number | string | boolean;
```

# 🗁 Using `keyof` with Indexed Access Types

The `keyof` operator produces a union of an object's property keys.

You can combine it with indexed access types to retrieve all property value types.

```typescript
type Product = {
  id: number;
  title: string;
  price: number;
  available: boolean;
};

type ProductKeys = keyof Product;

type ProductValues = Product[ProductKeys];
```

Equivalent shorter form:

```typescript
type ProductValues = Product[keyof Product];
```

Result:

```typescript
type ProductValues = number | string | boolean;
```

This pattern is useful when working with generic object types.

# 🗁 Indexed Access with Arrays

Indexed access types can retrieve the element type of an array.

```typescript
type Users = {
  id: number;
  name: string;
}[];

type User = Users[number];
```

Result:

```typescript
type User = {
  id: number;
  name: string;
};
```

The `number` index accesses the array's element type.

You can also use the `typeof` operator with an existing array value:

```typescript
const users = [
  { id: 1, name: "John" },
  { id: 2, name: "Alex" }
];

type User = typeof users[number];
```

Result:

```typescript
type User = {
  id: number;
  name: string;
};
```

This is especially useful when deriving types from existing data.

# 🗁 Indexed Access with Tuples

Tuples support indexed access types as well.

```typescript
type User = [number, string, boolean];

type UserId = User[0];
type UserName = User[1];
type UserStatus = User[2];
```

Result:

```typescript
type UserId = number;
type UserName = string;
type UserStatus = boolean;
```

You can retrieve the union of all tuple element types using `number`:

```typescript
type UserValues = User[number];
```

Result:

```typescript
type UserValues = number | string | boolean;
```

# 🗁 Accessing Nested Property Types

Indexed access types can access deeply nested properties by chaining property keys.

```typescript
type User = {
  id: number;
  profile: {
    name: string;
    email: string;
    address: {
      city: string;
      country: string;
    };
  };
};

type Profile = User["profile"];

type UserName = User["profile"]["name"];

type UserCity = User["profile"]["address"]["city"];
```

Result:

```typescript
type Profile = {
  name: string;
  email: string;
  address: {
    city: string;
    country: string;
  };
};

type UserName = string;

type UserCity = string;
```

This approach helps reuse nested property types without duplicating their definitions.

# 🗁 Indexed Access with Generic Types

Indexed access types become more powerful when combined with generics.

```typescript
type User = {
  id: number;
  name: string;
  email: string;
};

type PropertyValue<T, K extends keyof T> = T[K];
```

Usage:

```typescript
type UserId = PropertyValue<User, "id">;

type UserName = PropertyValue<User, "name">;

type UserEmail = PropertyValue<User, "email">;
```

The generic constraint `K extends keyof T` ensures that the supplied key exists in the source type.

For example:

```typescript
type InvalidProperty = PropertyValue<User, "age">;
```

This produces a compilation error because `"age"` is not a property of `User`.

# 🗁 Generic Property Getter

You can use indexed access types to build strongly typed functions.

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
  id: 1,
  name: "John",
  active: true
};

const userId = getProperty(user, "id");
const userName = getProperty(user, "name");
const userStatus = getProperty(user, "active");
```

TypeScript infers the following types:

```typescript
// number
const userId = getProperty(user, "id");

// string
const userName = getProperty(user, "name");

// boolean
const userStatus = getProperty(user, "active");
```

The return type automatically depends on the selected property.

# 🗁 Indexed Access with Union Types

When a property contains a union type, indexed access preserves that union.

```typescript
type User = {
  id: number;
  name: string;
  status: "active" | "inactive";
};

type UserStatus = User["status"];
```

Result:

```typescript
type UserStatus = "active" | "inactive";
```

You can also combine multiple properties:

```typescript
type UserValue = User["id" | "status"];
```

Result:

```typescript
type UserValue = number | "active" | "inactive";
```

# 🗁 Indexed Access with Optional Properties

When accessing an optional property, the resulting type includes `undefined`.

```typescript
type User = {
  id: number;
  name?: string;
};

type UserName = User["name"];
```

Result:

```typescript
type UserName = string | undefined;
```

This reflects the possibility that the property may not exist on an object.

You can remove `undefined` using the `NonNullable` utility type:

```typescript
type RequiredUserName = NonNullable<User["name"]>;
```

Result:

```typescript
type RequiredUserName = string;
```

# 🗁 Indexed Access with Function Types

You can access a function property's type and combine it with utility types such as `ReturnType` and `Parameters`.

```typescript
type UserService = {
  getUser: (id: number) => {
    id: number;
    name: string;
  };
};

type GetUserFunction = UserService["getUser"];

type GetUserParameters = Parameters<GetUserFunction>;

type GetUserReturn = ReturnType<GetUserFunction>;
```

Result:

```typescript
type GetUserFunction = (
  id: number
) => {
  id: number;
  name: string;
};

type GetUserParameters = [id: number];

type GetUserReturn = {
  id: number;
  name: string;
};
```

This technique is useful when working with service layers, API clients, and callback definitions.

# 🗁 Practical Example: API Response Types

You can derive individual types from an API response structure.

```typescript
type ApiResponse = {
  success: boolean;
  data: {
    id: number;
    name: string;
    email: string;
  };
  message: string;
};

type ApiUser = ApiResponse["data"];

type ApiUserId = ApiUser["id"];

type ApiUserName = ApiUser["name"];
```

Result:

```typescript
type ApiUser = {
  id: number;
  name: string;
  email: string;
};

type ApiUserId = number;

type ApiUserName = string;
```

If the API response structure changes, the derived types automatically reflect those changes.

# 🗁 Practical Example: Deriving Types from Constants

Indexed access types are frequently used with `typeof` and `as const`.

```typescript
const roles = ["admin", "editor", "user"] as const;

type Role = typeof roles[number];
```

Result:

```typescript
type Role = "admin" | "editor" | "user";
```

You can also derive property types from constant objects:

```typescript
const config = {
  theme: "dark",
  language: "en",
  notifications: true
} as const;

type Config = typeof config;

type Theme = Config["theme"];

type Language = Config["language"];
```

Result:

```typescript
type Theme = "dark";

type Language = "en";
```

The `as const` assertion preserves literal types instead of widening them to general types such as `string` and `boolean`.

# 🗁 Practical Example: Reusable Select Options

You can derive a union of allowed values from an existing object.

```typescript
const userStatuses = {
  ACTIVE: "active",
  INACTIVE: "inactive",
  PENDING: "pending"
} as const;

type UserStatus =
  typeof userStatuses[keyof typeof userStatuses];
```

Result:

```typescript
type UserStatus = "active" | "inactive" | "pending";
```

This ensures that the type remains synchronized with the actual status definitions.

# 🗁 Common Mistakes

**Using a property that does not exist**

```typescript
type User = {
  id: number;
  name: string;
};

// Error: Property "age" does not exist.
type UserAge = User["age"];
```

Use a valid property key or constrain generic keys with `keyof`.

**Forgetting array indexing with `number`**

```typescript
type Users = {
  id: number;
  name: string;
}[];

type User = Users[number];
```

Using `number` retrieves the array element type.

**Confusing `typeof` and indexed access**

```typescript
const user = {
  id: 1,
  name: "John"
};

type User = typeof user;

type UserId = User["id"];
```

`typeof user` retrieves the type of the variable, while `User["id"]` retrieves the type of its `id` property.

# 🗁 Best Practices

* Use indexed access types to avoid duplicating property type definitions.
* Combine `keyof` and indexed access for reusable generic utilities.
* Use `T[number]` to extract array or tuple element types.
* Use `typeof` when deriving types from existing JavaScript values.
* Use `as const` when literal values should remain narrow.
* Use generic constraints such as `K extends keyof T` to ensure valid property access.
* Combine indexed access with utility types when extracting function parameters or return types.
* Prefer deriving types from a single source of truth instead of maintaining duplicate definitions.

# 🗁 Summary

Indexed access types retrieve property types from existing TypeScript types.

Key concepts:

* `T["property"]` retrieves a specific property type.
* `T["key1" | "key2"]` retrieves multiple property types as a union.
* `T[keyof T]` retrieves all property value types as a union.
* `T[number]` extracts array or tuple element types.
* `T["nested"]["property"]` accesses nested property types.
* `K extends keyof T` ensures valid generic property access.
* `typeof` and `as const` help derive types from existing values.

Indexed access types are an essential building block for advanced TypeScript type transformations and reusable generic utilities.
