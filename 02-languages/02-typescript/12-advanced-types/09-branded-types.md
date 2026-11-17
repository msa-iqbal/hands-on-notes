# Branded Types

Branded types are a TypeScript technique used to create distinct types from existing primitive or structural types.

TypeScript uses structural typing by default. This means two types with the same structure are generally considered compatible, even when they represent completely different concepts.

Branded types introduce an additional type-level marker that distinguishes otherwise identical types.

They are useful for:

* Distinguishing identifiers such as `UserId` and `ProductId`.
* Representing validated data.
* Preventing accidental mixing of semantically different values.
* Creating domain-specific types.
* Enforcing stronger type safety at application boundaries.

# 🗁 Understanding the Problem

Consider two identifiers:

```typescript
type UserId = number;
type ProductId = number;
```

Both types are aliases of `number`.

TypeScript therefore allows them to be used interchangeably:

```typescript
const userId: UserId = 101;
const productId: ProductId = 202;

const selectedUser: UserId = productId;
```

This compiles successfully, even though assigning a product identifier to a user identifier may be a logical error.

The problem is that TypeScript sees both types as `number`.

Branded types solve this problem by adding a distinguishing marker.

# 🗁 Basic Branded Type

A common approach uses an intersection type with a unique symbol property.

```typescript
declare const brand: unique symbol;

type UserId = number & {
  readonly [brand]: "UserId";
};

type ProductId = number & {
  readonly [brand]: "ProductId";
};
```

Now the two types are distinct.

```typescript
declare const userId: UserId;
declare const productId: ProductId;

const selectedUser: UserId = userId;

// Type error
const invalidUser: UserId = productId;
```

Although both branded types are based on `number`, their additional properties make them incompatible.

# 🗁 Understanding `unique symbol`

The `unique symbol` type represents a symbol with a distinct identity.

```typescript
declare const userBrand: unique symbol;
declare const productBrand: unique symbol;
```

These symbols have different identities, even though both have the `unique symbol` type.

They can be used as property keys to distinguish branded types.

```typescript
type UserId = number & {
  readonly [userBrand]: true;
};

type ProductId = number & {
  readonly [productBrand]: true;
};
```

The `declare` keyword tells TypeScript that the symbol exists for type checking. It does not create a runtime symbol.

The brand property is a compile-time marker in this pattern.

# 🗁 Creating a Generic Brand Utility

Instead of defining every branded type manually, you can create a reusable utility.

```typescript
declare const brand: unique symbol;

type Brand<T, Name extends string> = T & {
  readonly [brand]: Name;
};
```

Now you can define different branded types:

```typescript
type UserId = Brand<number, "UserId">;

type ProductId = Brand<number, "ProductId">;

type OrderId = Brand<number, "OrderId">;

type EmailAddress = Brand<string, "EmailAddress">;
```

Each brand has a different identity based on its `Name` parameter.

Example:

```typescript
declare const userId: UserId;
declare const productId: ProductId;

const validUser: UserId = userId;

// Type error
const invalidUser: UserId = productId;
```

This generic pattern makes branded types easier to maintain across large applications.

# 🗁 Creating Branded Values Safely

A regular value cannot automatically become a branded value.

```typescript
type UserId = Brand<number, "UserId">;

const id: UserId = 101;
```

TypeScript reports an error because a regular number does not contain the brand marker.

You can create a controlled conversion function:

```typescript
function createUserId(value: number): UserId {
  return value as UserId;
}

const userId = createUserId(101);
```

However, this function only converts the type. It does not validate whether the number is a valid user identifier.

A better implementation validates the input:

```typescript
function createUserId(value: number): UserId {
  if (!Number.isInteger(value) || value <= 0) {
    throw new Error("Invalid user ID");
  }

  return value as UserId;
}

const userId = createUserId(101);
```

Now the function validates the runtime value before assigning the branded type.

# 🗁 Branded Types with String Values

Branded types are not limited to numbers.

They can also distinguish strings that represent different domain concepts.

```typescript
type Brand<T, Name extends string> = T & {
  readonly [brand]: Name;
};

type EmailAddress = Brand<string, "EmailAddress">;

type Username = Brand<string, "Username">;

type AccessToken = Brand<string, "AccessToken">;
```

These types are all based on `string`, but TypeScript treats them as distinct.

```typescript
declare const email: EmailAddress;
declare const username: Username;

const validEmail: EmailAddress = email;

// Type error
const invalidEmail: EmailAddress = username;
```

# 🗁 Validating Email Addresses

A branded email type can ensure that application code receives email values through a validation function.

```typescript
type EmailAddress = Brand<string, "EmailAddress">;

function createEmailAddress(value: string): EmailAddress {
  const emailPattern = /^[^\s@]+@[^\s@]+\.[^\s@]+$/;

  if (!emailPattern.test(value)) {
    throw new Error("Invalid email address");
  }

  return value as EmailAddress;
}
```

Usage:

```typescript
const email = createEmailAddress("user@example.com");

function sendWelcomeEmail(address: EmailAddress): void {
  console.log(`Sending email to ${address}`);
}

sendWelcomeEmail(email);
```

A plain string cannot be passed directly:

```typescript
sendWelcomeEmail("user@example.com");
```

TypeScript reports an error because the argument has not been assigned the `EmailAddress` brand.

This pattern helps enforce validation boundaries in an application.

# 🗁 Branded Types with Object Types

Branded types can also be applied to objects.

```typescript
type Brand<T, Name extends string> = T & {
  readonly [brand]: Name;
};

type AdminUser = Brand<
  {
    id: number;
    name: string;
  },
  "AdminUser"
>;

type CustomerUser = Brand<
  {
    id: number;
    name: string;
  },
  "CustomerUser"
>;
```

Although both object types have the same visible properties, their brands distinguish them.

```typescript
declare const admin: AdminUser;
declare const customer: CustomerUser;

const validAdmin: AdminUser = admin;

// Type error
const invalidAdmin: AdminUser = customer;
```

This can be useful when different domain entities share the same structure but have different meanings.

# 🗁 Nominal Typing with Branded Types

TypeScript primarily uses structural typing.

In nominal type systems, types are distinguished by their declared identities rather than only their structure.

Branded types provide a practical way to approximate nominal typing in TypeScript.

Consider:

```typescript
type Meters = number;

type Kilometers = number;
```

These types are structurally identical.

A branded implementation makes them distinct:

```typescript
type Meters = Brand<number, "Meters">;

type Kilometers = Brand<number, "Kilometers">;
```

Now TypeScript prevents accidental assignments between them.

```typescript
declare const distanceInMeters: Meters;
declare const distanceInKilometers: Kilometers;

const meters: Meters = distanceInMeters;

// Type error
const invalidMeters: Meters = distanceInKilometers;
```

This is especially useful when working with measurements, currencies, identifiers, and other domain-specific values.

# 🗁 Branded Types for Currency

Different currencies should not be mixed accidentally.

```typescript
type USD = Brand<number, "USD">;

type EUR = Brand<number, "EUR">;

function createUSD(value: number): USD {
  if (!Number.isFinite(value) || value < 0) {
    throw new Error("Invalid USD amount");
  }

  return value as USD;
}

function createEUR(value: number): EUR {
  if (!Number.isFinite(value) || value < 0) {
    throw new Error("Invalid EUR amount");
  }

  return value as EUR;
}
```

Usage:

```typescript
const priceUSD = createUSD(100);

const priceEUR = createEUR(90);

function chargeUSD(amount: USD): void {
  console.log(`Charging $${amount}`);
}

chargeUSD(priceUSD);

// Type error
chargeUSD(priceEUR);
```

This prevents accidental currency substitution.

However, branded types do not perform currency conversion or guarantee that arithmetic operations preserve the brand.

# 🗁 Branded Types and Type Guards

You can combine branded types with user-defined type guards.

```typescript
type PositiveNumber = Brand<number, "PositiveNumber">;

function isPositiveNumber(
  value: number
): value is PositiveNumber {
  return Number.isFinite(value) && value > 0;
}
```

Usage:

```typescript
function calculateDiscount(value: number): void {
  if (isPositiveNumber(value)) {
    const discount: PositiveNumber = value;

    console.log(discount);
  }
}
```

The type predicate `value is PositiveNumber` tells TypeScript that the value can be treated as a branded type inside the successful branch.

This is useful when validation is performed without throwing exceptions.

# 🗁 Branded Types and Runtime Validation

Branded types do not automatically validate data.

For example:

```typescript
type UserId = Brand<number, "UserId">;

const userId = 101 as UserId;
```

The assertion tells TypeScript to trust the developer.

It does not verify that the value is a valid user ID.

For external data, validate values before assigning branded types.

```typescript
function parseUserId(value: unknown): UserId {
  if (
    typeof value !== "number" ||
    !Number.isInteger(value) ||
    value <= 0
  ) {
    throw new Error("Invalid user ID");
  }

  return value as UserId;
}
```

Usage:

```typescript
const input: unknown = 101;

const userId = parseUserId(input);
```

This provides a clear boundary between untrusted input and validated domain values.

# 🗁 Branded Types with API Data

Consider an API that returns user information.

```typescript
type UserId = Brand<number, "UserId">;

type User = {
  id: UserId;
  name: string;
  email: EmailAddress;
};
```

You can create a parser that validates external data:

```typescript
function parseUser(input: unknown): User {
  if (
    typeof input !== "object" ||
    input === null ||
    !("id" in input) ||
    !("name" in input) ||
    !("email" in input)
  ) {
    throw new Error("Invalid user data");
  }

  const data = input as {
    id: unknown;
    name: unknown;
    email: unknown;
  };

  if (
    typeof data.id !== "number" ||
    !Number.isInteger(data.id) ||
    data.id <= 0
  ) {
    throw new Error("Invalid user ID");
  }

  if (typeof data.name !== "string") {
    throw new Error("Invalid user name");
  }

  if (typeof data.email !== "string") {
    throw new Error("Invalid email");
  }

  return {
    id: data.id as UserId,
    name: data.name,
    email: createEmailAddress(data.email),
  };
}
```

The parser validates external values and creates a trusted application-level object.

For production applications, a runtime validation library can reduce the amount of manual validation code.

# 🗁 Branded Types vs Type Assertions

These concepts are related but serve different purposes.

| Feature | Type Assertion | Branded Type |
| --- | --- | --- |
| Purpose | Tells TypeScript to trust a type conversion | Creates a distinct domain-specific type |
| Runtime validation | No | No, unless implemented separately |
| Prevents accidental mixing | Not by itself | Yes, between differently branded types |
| Type-level distinction | Depends on asserted type | Explicit brand marker |
| Typical use | Working with known values | Domain modeling and validated values |

Example:

```typescript
type UserId = Brand<number, "UserId">;

const userId = 101 as UserId;
```

The assertion is the mechanism used to assign the brand. The brand is what provides the additional type distinction.

# 🗁 Branded Types vs Enums

Enums can also represent distinct sets of values, but they solve a different problem.

```typescript
enum UserRole {
  Admin,
  Customer,
}
```

A branded type instead distinguishes values that share the same underlying primitive representation.

```typescript
type UserId = Brand<number, "UserId">;

type ProductId = Brand<number, "ProductId">;
```

Use enums when representing a predefined set of named values.

Use branded types when representing values that share a primitive type but have different domain meanings.

# 🗁 Limitations of Branded Types

Branded types improve compile-time safety, but they have limitations.

* They do not exist as runtime validation mechanisms.
* Type assertions can bypass their intended restrictions.
* They can be lost through operations that produce a new primitive value.
* They require controlled constructors or parsing functions to establish trust.
* They do not automatically preserve brands through arithmetic or string operations.
* They add complexity if applied to every primitive value unnecessarily.

For example:

```typescript
type UserId = Brand<number, "UserId">;

declare const userId: UserId;

const nextId = userId + 1;
```

The result of the arithmetic operation is a regular `number`, not a `UserId`.

If the result must remain branded, validate and explicitly construct the new value.

# 🗁 Best Practices

* Use a shared generic `Brand<T, Name>` utility.
* Prefer `unique symbol` for an internal brand marker.
* Keep brand markers private to the module when practical.
* Create constructor or parser functions for branded values.
* Validate external input before assigning a brand.
* Avoid exposing unrestricted type assertions throughout the application.
* Use descriptive brand names such as `UserId`, `EmailAddress`, and `OrderId`.
* Apply brands to domain values where accidental mixing would cause meaningful bugs.
* Do not confuse compile-time branding with runtime security or validation.

# 🗁 Summary

Branded types allow TypeScript developers to create distinct types from existing primitive or structural types.

Key concepts:

* TypeScript uses structural typing by default.
* Branded types add a unique marker to distinguish otherwise compatible types.
* `unique symbol` is commonly used to create the brand marker.
* Intersection types combine the original type with the brand.
* Generic brand utilities make branded types reusable.
* Validation functions establish trusted branded values.
* Branded types approximate nominal typing in TypeScript.
* They provide compile-time safety but do not perform runtime validation automatically.

Branded types are especially valuable in domain-driven applications where values may share the same primitive representation but have different meanings.
