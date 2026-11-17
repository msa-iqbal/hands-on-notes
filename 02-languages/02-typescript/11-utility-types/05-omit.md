# TypeScript Omit Utility Type

The `Omit<T, K>` utility type constructs a new type by excluding specific properties from an existing type.

It is useful when you want to reuse an existing type while removing properties that are unnecessary, sensitive, or irrelevant to a particular operation.

Common use cases include API response models, form data, database models, and component properties.

# 🗁 Basic Syntax

```typescript
Omit<Type, Keys>
```

* `Type` represents the original type.
* `Keys` represents the property keys to exclude.
* The resulting type contains all properties except the selected keys.

# 🗁 Basic Example

Consider an interface containing several properties.

```typescript
interface User {
  id: number;
  name: string;
  email: string;
  password: string;
  age: number;
}
```

Suppose you want a user type without the password property.

```typescript
type PublicUser = Omit<User, "password">;
```

The resulting type is equivalent to:

```typescript
type PublicUser = {
  id: number;
  name: string;
  email: string;
  age: number;
};
```

You can now create an object without the excluded property.

```typescript
const user: PublicUser = {
  id: 1,
  name: "Alice",
  email: "alice@example.com",
  age: 25
};
```

# 🗁 How Omit Works

Conceptually, TypeScript defines `Omit<T, K>` using `Pick`, `Exclude`, and `keyof`.

```typescript
type Omit<T, K extends PropertyKey> = Pick<
  T,
  Exclude<keyof T, K>
>;
```

Explanation:

* `keyof T` extracts all property keys from the original type.
* `Exclude<keyof T, K>` removes the selected keys.
* `Pick<T, ...>` constructs a new type using the remaining keys.
* `PropertyKey` represents valid property keys: `string`, `number`, or `symbol`.

# 🗁 Omit Multiple Properties

You can exclude multiple properties by providing a union of property names.

```typescript
interface Employee {
  id: number;
  name: string;
  email: string;
  salary: number;
  department: string;
  password: string;
}

type EmployeePublicInfo = Omit<
  Employee,
  "salary" | "password"
>;

const employee: EmployeePublicInfo = {
  id: 101,
  name: "Alice",
  email: "alice@example.com",
  department: "Engineering"
};
```

Both `salary` and `password` are excluded from the resulting type.

# 🗁 Omit with Interfaces

`Omit<T, K>` works directly with interfaces.

```typescript
interface Product {
  id: number;
  name: string;
  price: number;
  description: string;
  internalCode: string;
}

type PublicProduct = Omit<
  Product,
  "internalCode"
>;

const product: PublicProduct = {
  id: 1,
  name: "Laptop",
  price: 1200,
  description: "Professional laptop"
};
```

The original `Product` interface remains unchanged.

# 🗁 Omit with Type Aliases

You can also use `Omit` with object type aliases.

```typescript
type Settings = {
  theme: string;
  language: string;
  notifications: boolean;
  internalDebugMode: boolean;
};

type PublicSettings = Omit<
  Settings,
  "internalDebugMode"
>;

const settings: PublicSettings = {
  theme: "dark",
  language: "en",
  notifications: true
};
```

# 🗁 Omit with Optional Properties

`Omit` preserves the optionality of the remaining properties.

```typescript
interface User {
  id: number;
  name: string;
  email?: string;
  phone?: string;
  password: string;
}

type UserContact = Omit<User, "password" | "id">;
```

The resulting type is equivalent to:

```typescript
type UserContact = {
  name: string;
  email?: string;
  phone?: string;
};
```

Optional properties remain optional.

# 🗁 Omit with Readonly Properties

`Omit` preserves readonly modifiers on properties that remain in the resulting type.

```typescript
interface User {
  readonly id: number;
  name: string;
  email: string;
  password: string;
}

type PublicUser = Omit<User, "password">;

const user: PublicUser = {
  id: 1,
  name: "Alice",
  email: "alice@example.com"
};

// Error: id remains readonly.
user.id = 2;

// Valid: name is writable.
user.name = "Bob";
```

# 🗁 Omit with Functions

You can use `Omit` to define the properties a function should not accept.

```typescript
interface User {
  id: number;
  name: string;
  email: string;
  password: string;
  role: string;
}

type UserRegistration = Omit<
  User,
  "id" | "role"
>;

function registerUser(data: UserRegistration): void {
  console.log("Registering user...");
  console.log(data);
}

registerUser({
  name: "Alice",
  email: "alice@example.com",
  password: "strong-password"
});
```

The registration function accepts only the properties required for registration.

# 🗁 Practical Example: API Response Types

A backend model may contain internal properties that should not appear in a public API response.

```typescript
interface User {
  id: number;
  name: string;
  email: string;
  passwordHash: string;
  internalRole: string;
  createdAt: Date;
}

type PublicUser = Omit<
  User,
  "passwordHash" | "internalRole"
>;

function getPublicUser(user: User): PublicUser {
  return {
    id: user.id,
    name: user.name,
    email: user.email,
    createdAt: user.createdAt
  };
}

const user: User = {
  id: 1,
  name: "Alice",
  email: "alice@example.com",
  passwordHash: "hashed-password",
  internalRole: "administrator",
  createdAt: new Date()
};

const response = getPublicUser(user);

console.log(response);
```

The resulting response contains only the properties included in `PublicUser`.

`Omit` changes the TypeScript type, not the runtime object. You must explicitly construct the response without excluded properties.

# 🗁 Practical Example: Update Payloads

When updating a database record, some properties should not be accepted from the client.

```typescript
interface Product {
  id: number;
  name: string;
  price: number;
  createdAt: Date;
  updatedAt: Date;
}

type ProductUpdate = Omit<
  Product,
  "id" | "createdAt" | "updatedAt"
>;

function updateProduct(
  id: number,
  data: ProductUpdate
): void {
  console.log(`Updating product ${id}`);
  console.log(data);
}

updateProduct(1, {
  name: "Gaming Laptop",
  price: 1500
});
```

The client provides only the properties it is allowed to update.

For a real update operation where each property is optional, combine `Omit` with `Partial`.

```typescript
type PartialProductUpdate = Partial<
  Omit<Product, "id" | "createdAt" | "updatedAt">
>;

const update: PartialProductUpdate = {
  price: 1400
};
```

# 🗁 Omit with Nested Objects

`Omit` operates only on top-level properties.

```typescript
interface User {
  id: number;
  name: string;
  address: {
    city: string;
    country: string;
    postalCode: string;
  };
}

type UserWithoutId = Omit<User, "id">;
```

The nested `address` object remains unchanged.

```typescript
const user: UserWithoutId = {
  name: "Alice",
  address: {
    city: "Dhaka",
    country: "Bangladesh",
    postalCode: "1200"
  }
};
```

To exclude a nested property, apply `Omit` to the nested type separately.

```typescript
type UserWithoutPostalCode = Omit<User, "id"> & {
  address: Omit<User["address"], "postalCode">;
};

const user: UserWithoutPostalCode = {
  name: "Alice",
  address: {
    city: "Dhaka",
    country: "Bangladesh"
  }
};
```

# 🗁 Omit with Other Utility Types

You can combine `Omit` with other utility types to create specialized types.

**Omit with Partial**

Exclude selected properties and make the remaining properties optional.

```typescript
interface User {
  id: number;
  name: string;
  email: string;
  password: string;
  role: string;
}

type UserUpdate = Partial<
  Omit<User, "id" | "password" | "role">
>;

const update: UserUpdate = {
  name: "Alice Smith"
};
```

**Omit with Required**

Exclude selected properties and make all remaining properties required.

```typescript
interface User {
  id: number;
  name?: string;
  email?: string;
  password: string;
}

type CompleteUser = Required<
  Omit<User, "password">
>;

const user: CompleteUser = {
  id: 1,
  name: "Alice",
  email: "alice@example.com"
};
```

**Omit with Readonly**

Exclude selected properties and make the remaining properties read-only.

```typescript
type ReadonlyUser = Readonly<
  Omit<User, "password">
>;
```

# 🗁 Omit vs Pick

`Omit<T, K>` removes selected properties, while `Pick<T, K>` keeps only selected properties.

```typescript
interface User {
  id: number;
  name: string;
  email: string;
  password: string;
  role: string;
}
```

Using `Omit`:

```typescript
type PublicUser = Omit<User, "password">;
```

Using `Pick`:

```typescript
type PublicUser = Pick<
  User,
  "id" | "name" | "email" | "role"
>;
```

Both produce equivalent property structures in this example.

| Utility Type | Purpose |
|---|---|
| `Pick<T, K>` | Keeps selected properties |
| `Omit<T, K>` | Removes selected properties |

Use `Pick` when you know which properties you want to keep. Use `Omit` when you know which properties you want to exclude.

# 🗁 Important Considerations

* `Omit<T, K>` is a compile-time utility type.
* It does not remove properties from an object at runtime.
* It can exclude multiple properties using a union of keys.
* It preserves the modifiers of remaining properties.
* It operates only on top-level properties.
* Its key parameter can include keys that do not exist in the original type; those keys simply have no effect.
* It does not provide runtime data validation or security.

# 🗁 Best Practices

* Use `Omit` to create specialized types from existing models.
* Use it for API response types and update payloads.
* Combine it with `Partial` when update fields should be optional.
* Combine it with `Readonly` when remaining properties should not be reassigned.
* Prefer `Pick` when defining a small set of explicitly allowed properties.
* Explicitly construct objects when excluded properties must not be present at runtime.
* Validate external input independently of TypeScript utility types.

# 🗁 Key Takeaways

* `Omit<T, K>` creates a new type without selected properties.
* It is implemented using `Pick`, `Exclude`, and `keyof`.
* It preserves the types and modifiers of remaining properties.
* It supports excluding multiple properties.
* It is useful for API responses, database updates, and specialized data models.
* It only changes the type system, not runtime objects.
* It complements `Pick<T, K>`.
