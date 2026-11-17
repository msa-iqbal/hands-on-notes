# TypeScript Pick Utility Type

The `Pick<T, K>` utility type constructs a new type by selecting a specific set of properties from an existing type.

It is useful when you need a smaller version of an existing type without redefining its properties.

Common use cases include API response models, form data, database queries, component props, and data transfer objects (DTOs).

# 🗁 Basic Syntax

```typescript
Pick<Type, Keys>
```

* `Type` represents the original type.
* `Keys` represents the property keys to select.
* The resulting type contains only the selected properties.

# 🗁 Basic Example

Consider an interface with several properties.

```typescript
interface User {
  id: number;
  name: string;
  email: string;
  age: number;
  address: string;
}
```

Suppose you only need the user's name and email.

```typescript
type UserContact = Pick<User, "name" | "email">;
```

The resulting type is equivalent to:

```typescript
type UserContact = {
  name: string;
  email: string;
};
```

You can now create an object using only those properties.

```typescript
const contact: UserContact = {
  name: "Alice",
  email: "alice@example.com"
};
```

Properties that were not selected are not part of the resulting type.

# 🗁 How Pick Works

Conceptually, TypeScript defines `Pick<T, K>` using a mapped type.

```typescript
type Pick<T, K extends keyof T> = {
  [P in K]: T[P];
};
```

Explanation:

* `keyof T` extracts all property keys from type `T`.
* `K extends keyof T` ensures that selected keys belong to the original type.
* `P in K` iterates over the selected keys.
* `T[P]` preserves the original property type.

The `extends keyof T` constraint prevents selecting properties that do not exist in the original type.

# 🗁 Pick Multiple Properties

You can select multiple properties by using a union of property names.

```typescript
interface Product {
  id: number;
  name: string;
  price: number;
  description: string;
  category: string;
  stock: number;
}

type ProductSummary = Pick<
  Product,
  "id" | "name" | "price"
>;

const product: ProductSummary = {
  id: 1,
  name: "Laptop",
  price: 1200
};
```

Only the selected properties are required in `ProductSummary`.

# 🗁 Pick with Interfaces

`Pick<T, K>` works directly with interfaces.

```typescript
interface Employee {
  id: number;
  name: string;
  email: string;
  department: string;
  salary: number;
}

type EmployeeInfo = Pick<
  Employee,
  "id" | "name" | "department"
>;

const employee: EmployeeInfo = {
  id: 101,
  name: "Alice",
  department: "Engineering"
};
```

The original interface remains unchanged.

# 🗁 Pick with Type Aliases

You can also use `Pick` with object type aliases.

```typescript
type Settings = {
  theme: "light" | "dark";
  language: string;
  notifications: boolean;
  autoSave: boolean;
};

type DisplaySettings = Pick<
  Settings,
  "theme" | "language"
>;

const settings: DisplaySettings = {
  theme: "dark",
  language: "en"
};
```

# 🗁 Pick with Optional Properties

`Pick` preserves the optionality of selected properties.

```typescript
interface User {
  id: number;
  name: string;
  email?: string;
  phone?: string;
}

type UserContact = Pick<
  User,
  "name" | "email" | "phone"
>;
```

The resulting type is equivalent to:

```typescript
type UserContact = {
  name: string;
  email?: string;
  phone?: string;
};
```

You do not need to provide optional properties.

```typescript
const contact: UserContact = {
  name: "Alice"
};
```

# 🗁 Pick with Readonly Properties

`Pick` preserves existing readonly modifiers.

```typescript
interface User {
  readonly id: number;
  name: string;
  email: string;
}

type UserIdentity = Pick<User, "id" | "name">;

const user: UserIdentity = {
  id: 1,
  name: "Alice"
};

// Error: id remains readonly.
user.id = 2;

// Valid: name is writable.
user.name = "Bob";
```

# 🗁 Pick with Functions

You can use `Pick` to define the properties that a function needs.

```typescript
interface User {
  id: number;
  name: string;
  email: string;
  password: string;
  role: string;
}

function displayUser(
  user: Pick<User, "name" | "email">
): void {
  console.log(`Name: ${user.name}`);
  console.log(`Email: ${user.email}`);
}

const user: User = {
  id: 1,
  name: "Alice",
  email: "alice@example.com",
  password: "secret",
  role: "admin"
};

displayUser(user);
```

The function only depends on the selected properties, even though the supplied object contains additional properties.

# 🗁 Practical Example: API Response Types

A backend user model may contain sensitive or internal properties that should not be included in a public API response.

```typescript
interface User {
  id: number;
  name: string;
  email: string;
  passwordHash: string;
  role: string;
  createdAt: Date;
}

type PublicUser = Pick<
  User,
  "id" | "name" | "email"
>;

function getPublicUser(user: User): PublicUser {
  return {
    id: user.id,
    name: user.name,
    email: user.email
  };
}

const user: User = {
  id: 1,
  name: "Alice",
  email: "alice@example.com",
  passwordHash: "hashed-password",
  role: "admin",
  createdAt: new Date()
};

const response = getPublicUser(user);

console.log(response);
```

This creates a clearly defined public response shape.

`Pick` only defines the TypeScript type. The function must still construct the actual response object and deliberately exclude properties that should not be exposed.

# 🗁 Practical Example: Form Data

A user registration form may require only a subset of the complete user model.

```typescript
interface User {
  id: number;
  name: string;
  email: string;
  password: string;
  role: string;
  createdAt: Date;
}

type RegistrationForm = Pick<
  User,
  "name" | "email" | "password"
>;

function registerUser(data: RegistrationForm): void {
  console.log("Registering user...");
  console.log(data);
}

registerUser({
  name: "Alice",
  email: "alice@example.com",
  password: "strong-password"
});
```

This keeps form input types separate from the complete application data model.

# 🗁 Pick with Nested Objects

`Pick` selects properties at the top level. It does not automatically select or modify nested properties.

```typescript
interface User {
  id: number;
  name: string;
  address: {
    city: string;
    country: string;
  };
  contact: {
    email: string;
    phone: string;
  };
}

type UserAddress = Pick<User, "name" | "address">;
```

The resulting type is equivalent to:

```typescript
type UserAddress = {
  name: string;
  address: {
    city: string;
    country: string;
  };
};
```

You can select specific nested properties by combining `Pick` with another `Pick`.

```typescript
type UserCity = {
  name: User["name"];
  address: Pick<User["address"], "city">;
};
```

Now `UserCity` contains only the user's name and address city.

# 🗁 Pick with Other Utility Types

Utility types can be combined to create more specialized types.

**Pick with Partial**

Select specific properties and make them optional.

```typescript
interface User {
  id: number;
  name: string;
  email: string;
  age: number;
}

type UserUpdate = Partial<
  Pick<User, "name" | "email" | "age">
>;

const update: UserUpdate = {
  name: "Alice Smith"
};
```

**Pick with Readonly**

Select specific properties and make them read-only.

```typescript
type UserIdentity = Readonly<
  Pick<User, "id" | "name">
>;

const identity: UserIdentity = {
  id: 1,
  name: "Alice"
};

// Error: id is readonly.
identity.id = 2;
```

**Pick with Required**

Select specific properties and make them required.

```typescript
interface User {
  id: number;
  name?: string;
  email?: string;
}

type RequiredContact = Required<
  Pick<User, "name" | "email">
>;

const contact: RequiredContact = {
  name: "Alice",
  email: "alice@example.com"
};
```

# 🗁 Pick vs Omit

`Pick<T, K>` selects the properties you want to keep, while `Omit<T, K>` removes the properties you specify.

```typescript
interface User {
  id: number;
  name: string;
  email: string;
  password: string;
}
```

Using `Pick`:

```typescript
type PublicUser = Pick<User, "id" | "name" | "email">;
```

Using `Omit`:

```typescript
type PublicUser = Omit<User, "password">;
```

Both produce equivalent property structures in this example.

| Utility Type | Purpose |
|---|---|
| `Pick<T, K>` | Keeps selected properties |
| `Omit<T, K>` | Removes selected properties |

# 🗁 Important Considerations

* `Pick<T, K>` is a compile-time utility type.
* It does not create or copy an object at runtime.
* Selected keys must exist in the original type.
* It preserves property types, optionality, and readonly modifiers.
* It selects only top-level properties.
* It does not automatically remove sensitive data from runtime objects.
* It works with interfaces, type aliases, and other compatible object types.

# 🗁 Best Practices

* Use `Pick` when you need a specific subset of an existing type.
* Use it to define API response models and form input types.
* Combine it with `Partial`, `Required`, and `Readonly` for specialized types.
* Prefer `Pick` over duplicating property declarations when the selected properties should stay synchronized with the original type.
* Explicitly construct API responses instead of assuming a type transformation removes runtime properties.
* Keep selected property lists small and meaningful.

# 🗁 Key Takeaways

* `Pick<T, K>` creates a new type containing selected properties.
* It uses `keyof` and mapped types internally.
* Selected keys must belong to the original type.
* It preserves property types and modifiers.
* It is useful for API responses, form data, and smaller domain models.
* It can be combined with other utility types.
* It complements `Omit<T, K>`.
