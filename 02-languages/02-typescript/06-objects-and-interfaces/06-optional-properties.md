# Optional Properties

Optional properties are properties that do not have to be present in an object.

TypeScript allows you to define optional properties using the question mark (`?`) after a property name.

Optional properties are useful when an object can contain different sets of information depending on the application requirements.

They help you:

* Define flexible object structures.
* Represent incomplete or partially available data.
* Model optional API response fields.
* Create reusable configuration objects.
* Handle optional user profile information.
* Avoid requiring unnecessary properties.

# 🗁 Basic Syntax

Add `?` after a property name to make it optional.

```typescript
interface User {
  id: number;
  name: string;
  email?: string;
}
```

In this example:

* `id` is required.
* `name` is required.
* `email` is optional.

You can create an object without the optional property:

```typescript
const user: User = {
  id: 1,
  name: "Alice",
};
```

You can also provide the optional property:

```typescript
const user: User = {
  id: 2,
  name: "Bob",
  email: "bob@example.com",
};
```

Both objects are valid.

# 🗁 Optional Properties in Type Aliases

Optional properties also work with type aliases.

```typescript
type Product = {
  id: number;
  name: string;
  description?: string;
  price?: number;
};
```

Example:

```typescript
const product: Product = {
  id: 101,
  name: "Laptop",
};
```

The `description` and `price` properties are not required.

# 🗁 Optional Properties in Object Types

You can also define optional properties directly in an object type annotation.

```typescript
const user: {
  id: number;
  name: string;
  phone?: string;
} = {
  id: 1,
  name: "Alice",
};
```

This is useful for small object structures that do not need a separate interface or type alias.

# 🗁 Reading Optional Properties

An optional property may be missing from an object.

When you access it, TypeScript considers its value potentially `undefined`.

```typescript
interface User {
  name: string;
  email?: string;
}

const user: User = {
  name: "Alice",
};

console.log(user.email);
```

The inferred type of `user.email` is:

```typescript
string | undefined
```

You must account for the possibility of `undefined` when using the value.

# 🗁 Checking Optional Properties

Use an `if` statement to check whether an optional property has a usable value.

```typescript
interface User {
  name: string;
  email?: string;
}

const user: User = {
  name: "Alice",
};

if (user.email !== undefined) {
  console.log(user.email.toLowerCase());
}
```

TypeScript narrows the property to `string` inside the condition.

You can also use a truthiness check:

```typescript
if (user.email) {
  console.log(user.email.toLowerCase());
}
```

However, truthiness checks also reject empty strings. Use an explicit `undefined` check when an empty string is a valid value.

# 🗁 Optional Chaining

Optional chaining (`?.`) allows you to access a property only when its preceding value is not `null` or `undefined`.

```typescript
interface User {
  name: string;
  email?: string;
}

const user: User = {
  name: "Alice",
};

console.log(user.email?.toLowerCase());
```

If `email` is `undefined`, the expression evaluates to `undefined` instead of throwing an error.

Optional chaining is useful for nested optional properties.

```typescript
interface Address {
  city?: string;
}

interface User {
  name: string;
  address?: Address;
}

const user: User = {
  name: "Alice",
};

console.log(user.address?.city);
```

# 🗁 Nullish Coalescing with Optional Properties

The nullish coalescing operator (`??`) provides a fallback value when an expression evaluates to `null` or `undefined`.

```typescript
interface User {
  name: string;
  email?: string;
}

const user: User = {
  name: "Alice",
};

const email = user.email ?? "No email provided";

console.log(email);
```

Output:

```text
No email provided
```

Unlike the logical OR operator (`||`), nullish coalescing does not replace valid falsy values such as an empty string or `0`.

# 🗁 Optional Properties vs Undefined Union

These two declarations look similar but have different assignment behavior.

Optional property:

```typescript
interface User {
  email?: string;
}
```

Required property that accepts `undefined`:

```typescript
interface User {
  email: string | undefined;
}
```

The first declaration allows the property to be omitted.

```typescript
const user: User = {};
```

The second declaration requires the property to exist, even when its value is `undefined`.

```typescript
interface User {
  email: string | undefined;
}

const user: User = {
  email: undefined,
};
```

| Declaration | Can omit property? | Can assign `undefined`? |
| --- | --- | --- |
| `email?: string` | Yes | Yes, unless `exactOptionalPropertyTypes` is enabled |
| `email: string \| undefined` | No | Yes |

# 🗁 exactOptionalPropertyTypes

The TypeScript compiler option `exactOptionalPropertyTypes` changes how optional properties behave during assignment.

When disabled, an optional property generally accepts an explicit `undefined` value.

```typescript
interface User {
  email?: string;
}

const user: User = {
  email: undefined,
};
```

When enabled, an optional property means that the property may be absent. It does not automatically mean that an explicitly present property may contain `undefined`.

```typescript
interface User {
  email?: string;
}

// Error with exactOptionalPropertyTypes enabled.
const user: User = {
  email: undefined,
};
```

To allow both omission and explicit `undefined`, declare the union:

```typescript
interface User {
  email?: string | undefined;
}
```

Enable the option in `tsconfig.json`:

```json
{
  "compilerOptions": {
    "strict": true,
    "exactOptionalPropertyTypes": true
  }
}
```

This option is useful when the distinction between an absent property and a property explicitly set to `undefined` matters.

# 🗁 Optional Properties with Readonly

A property can be both optional and readonly.

```typescript
interface User {
  id: number;
  readonly email?: string;
}
```

The property can be omitted when creating the object, but it cannot be reassigned afterward.

```typescript
const user: User = {
  id: 1,
  email: "alice@example.com",
};

// Error: cannot assign to readonly property.
user.email = "new@example.com";
```

# 🗁 Optional Properties in Interface Extension

Optional properties are inherited when one interface extends another.

```typescript
interface User {
  id: number;
  name: string;
  email?: string;
}

interface Admin extends User {
  permissions: string[];
}
```

Example:

```typescript
const admin: Admin = {
  id: 1,
  name: "Alice",
  permissions: ["manage-users"],
};
```

The inherited `email` property remains optional.

An extending interface can also make an optional property required when the new declaration is compatible.

```typescript
interface User {
  email?: string;
}

interface VerifiedUser extends User {
  email: string;
}
```

A `VerifiedUser` must provide an email address.

# 🗁 Optional Function Properties

An interface can define optional function-valued properties.

```typescript
interface User {
  name: string;
  onLogin?: () => void;
}
```

Example:

```typescript
const user: User = {
  name: "Alice",
  onLogin() {
    console.log("User logged in");
  },
};
```

Because `onLogin` may be absent, check it before calling:

```typescript
user.onLogin?.();
```

This safely calls the function only when it exists.

# 🗁 Optional Method Signatures

Methods can also be optional.

```typescript
interface Logger {
  log(message: string): void;
  error?(message: string): void;
}
```

Example:

```typescript
const logger: Logger = {
  log(message) {
    console.log(message);
  },
};

logger.log("Application started");
logger.error?.("Something went wrong");
```

The `error` method is not required when implementing the interface.

# 🗁 Optional Properties in API Responses

API responses often contain fields that are not always available.

```typescript
interface ApiUser {
  id: number;
  name: string;
  email?: string;
  avatar?: string;
  phone?: string;
}
```

Example response:

```typescript
const user: ApiUser = {
  id: 1,
  name: "Alice",
  avatar: "https://example.com/avatar.png",
};
```

Safely display optional information:

```typescript
console.log(`Name: ${user.name}`);
console.log(`Email: ${user.email ?? "Not available"}`);
console.log(`Phone: ${user.phone ?? "Not provided"}`);
```

Optional properties are particularly useful when modeling partial or variable API response structures.

# 🗁 Practical Example: User Profile

```typescript
interface UserProfile {
  readonly id: number;
  username: string;
  email: string;
  bio?: string;
  avatar?: string;
  website?: string;
  location?: string;
}

const profile: UserProfile = {
  id: 1,
  username: "alice",
  email: "alice@example.com",
  bio: "Software developer",
  location: "Dhaka, Bangladesh",
};
```

Create a function that displays available profile information:

```typescript
function displayProfile(profile: UserProfile): void {
  console.log(`Username: ${profile.username}`);
  console.log(`Email: ${profile.email}`);

  if (profile.bio) {
    console.log(`Bio: ${profile.bio}`);
  }

  if (profile.avatar) {
    console.log(`Avatar: ${profile.avatar}`);
  }

  if (profile.website) {
    console.log(`Website: ${profile.website}`);
  }

  if (profile.location) {
    console.log(`Location: ${profile.location}`);
  }
}

displayProfile(profile);
```

The function handles optional fields without requiring every profile to contain all possible information.

# 🗁 Common Mistakes

**Treating an optional property as always available**

```typescript
interface User {
  email?: string;
}

const user: User = {};

user.email.toLowerCase();
```

This causes a TypeScript error because `email` may be `undefined`.

Use optional chaining:

```typescript
user.email?.toLowerCase();
```

**Confusing optional properties with nullable properties**

An optional property may be absent:

```typescript
interface User {
  email?: string;
}
```

A nullable property must exist but may contain `null`:

```typescript
interface User {
  email: string | null;
}
```

These represent different data models.

**Using truthiness checks when empty strings are valid**

```typescript
if (user.email) {
  console.log(user.email);
}
```

This condition does not execute when `email` is an empty string.

Use:

```typescript
if (user.email !== undefined) {
  console.log(user.email);
}
```

**Forgetting that optional methods may not exist**

```typescript
interface Logger {
  error?(): void;
}

declare const logger: Logger;

// Unsafe:
logger.error();
```

Use:

```typescript
logger.error?.();
```

# 🗁 Best Practices

* Use optional properties only when a field is genuinely allowed to be absent.
* Use `?` to represent optional object members.
* Use `null` explicitly when it has a meaningful domain-specific meaning.
* Check optional values before accessing their members.
* Use optional chaining for safe property and method access.
* Use nullish coalescing for sensible fallback values.
* Enable `strict` mode to improve type safety.
* Consider `exactOptionalPropertyTypes` when absence and explicit `undefined` must be distinguished.
* Avoid making too many properties optional because it weakens the object contract.
* Model API responses according to their actual data structure.

# 🗁 Summary

* Optional properties use the `?` modifier.
* Optional properties do not have to be present in an object.
* Accessing an optional property may produce `undefined`.
* Optional properties are supported in interfaces, type aliases, and object type annotations.
* Optional chaining provides safe access to optional values.
* Nullish coalescing provides fallback values.
* Optional properties can also be readonly or function-valued.
* Optional properties are inherited through interface extension.
* `exactOptionalPropertyTypes` controls whether explicit `undefined` is accepted.
* Optional properties are useful for flexible object structures and API response models.

# 🗁 Related Topics

* Object Types
* Type Aliases
* Interfaces
* Interface Extension
* Interface Merging
* Readonly Properties
* Union Types
* Undefined
* Null
* Optional Chaining
* Nullish Coalescing
* Type Narrowing
