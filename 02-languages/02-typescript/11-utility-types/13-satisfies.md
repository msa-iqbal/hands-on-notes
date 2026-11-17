# Satisfies Operator

The `satisfies` operator checks whether an expression conforms to a specified type without changing the inferred type of that expression.

It was introduced in TypeScript 4.9.

Before `satisfies`, developers often used type annotations or type assertions to validate object structures. However, these approaches can affect how TypeScript infers the expression's type.

The `satisfies` operator provides a way to validate compatibility while preserving useful type information.

# 🗁 Syntax

```typescript
expression satisfies Type
```

* `expression`: The value or expression being checked.
* `Type`: The expected type or structural constraint.
* Result: The original expression with its inferred type preserved, provided it satisfies the constraint.

# 🗁 Basic Example

```typescript
type User = {
  id: number;
  name: string;
  active: boolean;
};

const user = {
  id: 1,
  name: "John",
  active: true,
} satisfies User;
```

TypeScript verifies that the object satisfies the `User` type.

The expression retains its inferred type:

```typescript
// {
//   id: number;
//   name: string;
//   active: boolean;
// }
```

# 🗁 Satisfies vs Type Annotation

A type annotation explicitly declares the variable's type.

```typescript
type Color = "red" | "green" | "blue";

const color: Color = "red";
```

The variable `color` has the type `Color`.

Using `satisfies` validates the expression while preserving its more specific inferred type.

```typescript
const color = "red" satisfies Color;

// Inferred type:
// "red"
```

This distinction becomes especially useful when working with object properties.

# 🗁 Satisfies vs Type Assertion

A type assertion tells TypeScript to treat a value as a particular type.

```typescript
type Config = {
  host: string;
  port: number;
};

const config = {
  host: "localhost",
  port: 3000,
} as Config;
```

An assertion does not perform runtime validation and can suppress certain compatibility checks.

Using `satisfies` checks compatibility instead:

```typescript
const config = {
  host: "localhost",
  port: 3000,
} satisfies Config;
```

For validating an expression against a known type, `satisfies` is generally preferable to an assertion.

# 🗁 Preserving Literal Types

One of the most useful features of `satisfies` is preserving specific inferred information.

```typescript
type RGB = [number, number, number];

type ColorConfig = {
  red: string | RGB;
  green: string | RGB;
  blue: string | RGB;
};

const colors = {
  red: [255, 0, 0],
  green: "#00ff00",
  blue: [0, 0, 255],
} satisfies ColorConfig;
```

TypeScript validates the object against `ColorConfig` while retaining information about the individual property values.

For example:

```typescript
const red = colors.red;

// Inferred as:
// [number, number, number]
```

Without `satisfies`, a broad annotation could make the property type `string | RGB`.

# 🗁 Validating Object Properties

The operator is useful when you want to ensure that an object contains valid properties and values.

```typescript
type UserRole = "admin" | "editor" | "viewer";

type UserPermissions = {
  role: UserRole;
  canEdit: boolean;
  canDelete: boolean;
};

const adminPermissions = {
  role: "admin",
  canEdit: true,
  canDelete: true,
} satisfies UserPermissions;
```

If a required property is missing or a property has an incompatible value, TypeScript reports an error.

```typescript
const invalidPermissions = {
  role: "admin",
  canEdit: true,
  // canDelete is missing
} satisfies UserPermissions;

// TypeScript error:
// Property 'canDelete' is missing.
```

# 🗁 Validating Configuration Objects

`satisfies` is especially useful for application configuration.

```typescript
type Environment = {
  development: {
    apiUrl: string;
    debug: boolean;
  };
  production: {
    apiUrl: string;
    debug: boolean;
  };
};

const config = {
  development: {
    apiUrl: "http://localhost:3000",
    debug: true,
  },
  production: {
    apiUrl: "https://api.example.com",
    debug: false,
  },
} satisfies Environment;
```

The configuration is checked against the expected structure without unnecessarily widening its inferred property types.

# 🗁 Using Satisfies with Record

You can combine `satisfies` with `Record` to validate objects with specific keys and value types.

```typescript
type Page = "home" | "about" | "contact";

const routes = {
  home: "/",
  about: "/about",
  contact: "/contact",
} satisfies Record<Page, string>;
```

TypeScript checks that every required key exists and each value is a string.

It also detects unexpected properties on a fresh object literal.

```typescript
const routes = {
  home: "/",
  about: "/about",
  contact: "/contact",
  dashboard: "/dashboard",
} satisfies Record<Page, string>;

// TypeScript error:
// 'dashboard' is not an allowed property.
```

# 🗁 Using Satisfies with Function Types

You can use `satisfies` to validate that a function matches a required signature.

```typescript
type Calculator = (
  a: number,
  b: number
) => number;

const add = ((a, b) => a + b) satisfies Calculator;
```

TypeScript checks the function parameters and return type against `Calculator`.

The expression retains its inferred function type.

# 🗁 Using Satisfies with Arrays

The operator can validate array structures while retaining useful inferred information.

```typescript
type User = {
  id: number;
  name: string;
};

const users = [
  { id: 1, name: "John" },
  { id: 2, name: "Alice" },
] satisfies User[];
```

The array is checked against `User[]`, and TypeScript retains the inferred object structure.

# 🗁 Combining Satisfies with as const

You can combine `satisfies` with `as const` when you want both validation and deeply readonly literal inference.

```typescript
type ButtonConfig = {
  label: string;
  variant: "primary" | "secondary";
  disabled: boolean;
};

const button = {
  label: "Submit",
  variant: "primary",
  disabled: false,
} as const satisfies ButtonConfig;
```

The resulting properties retain literal types:

```typescript
// {
//   readonly label: "Submit";
//   readonly variant: "primary";
//   readonly disabled: false;
// }
```

This is useful for defining immutable configuration objects.

# 🗁 Satisfies Does Not Change Runtime Values

The `satisfies` operator exists only for TypeScript's type checking.

```typescript
const settings = {
  theme: "dark",
  enabled: true,
} satisfies {
  theme: string;
  enabled: boolean;
};
```

The operator does not:

* Validate values at runtime.
* Convert values into another type.
* Add runtime checks.
* Modify the resulting JavaScript object.

TypeScript removes the operator during compilation.

# 🗁 Important Behavior

**Satisfies checks compatibility but does not replace the inferred type.**

```typescript
type Status = "active" | "inactive";

const status = "active" satisfies Status;

// Inferred type:
// "active"
```

**Satisfies can provide contextual typing.**

The target type can provide contextual information while TypeScript infers the expression.

```typescript
type Handler = {
  execute: (value: string) => number;
};

const handler = {
  execute: value => value.length,
} satisfies Handler;
```

Here, TypeScript knows that `value` is a string because of the contextual function signature.

**Satisfies does not automatically make properties readonly.**

```typescript
const config = {
  enabled: true,
} satisfies {
  enabled: boolean;
};

config.enabled = false;

// Valid
```

Use `as const` when readonly literal inference is required.

**Satisfies is not a runtime validation mechanism.**

External data such as JSON responses still needs runtime validation before it can safely be trusted.

# 🗁 Practical Example: Application Routes

Suppose an application has a fixed set of routes. You want to validate every route while preserving the exact route values.

```typescript
type RouteName =
  | "home"
  | "users"
  | "products"
  | "settings";

const routes = {
  home: "/",
  users: "/users",
  products: "/products",
  settings: "/settings",
} satisfies Record<RouteName, string>;
```

You can safely derive individual route values:

```typescript
const homeRoute = routes.home;

// Inferred type:
// "/"
```

If a route is missing or an unexpected route is added, TypeScript reports an error.

# 🗁 Practical Example: Feature Configuration

A web application may define multiple features with different configuration values.

```typescript
type FeatureConfig = {
  enabled: boolean;
  description: string;
};

const features = {
  authentication: {
    enabled: true,
    description: "User authentication",
  },
  notifications: {
    enabled: false,
    description: "System notifications",
  },
  analytics: {
    enabled: true,
    description: "Application analytics",
  },
} satisfies Record<string, FeatureConfig>;
```

The `satisfies` operator validates the structure of every feature configuration while preserving the inferred object properties.

You can access individual values:

```typescript
const authenticationEnabled =
  features.authentication.enabled;

// Inferred type:
// boolean
```

# 🗁 Satisfies vs Other Type Techniques

| Technique | Main Purpose | Preserves Expression Inference |
| --- | --- | --- |
| Type annotation | Declares a variable's type | May widen the inferred type |
| Type assertion (`as`) | Overrides the compiler's interpretation | Uses the asserted type |
| `satisfies` | Checks compatibility with a type | Yes |
| `as const` | Preserves literal types and adds readonly modifiers | Yes, with readonly inference |

These techniques solve different problems and can be combined when appropriate.

# 🗁 Best Practices

* Use `satisfies` to validate configuration objects and structured data.
* Prefer it over type assertions when checking compatibility is the goal.
* Use it with `Record` for validating objects with known keys.
* Combine it with `as const` when immutable literal types are required.
* Use it for route definitions, feature flags, permissions, and application settings.
* Remember that contextual typing can influence expression inference.
* Do not treat `satisfies` as runtime validation.
* Use runtime schema validation when handling untrusted external data.

# 🗁 Summary

* `satisfies` checks whether an expression conforms to a specified type.
* It preserves the expression's inferred type instead of replacing it with the target type.
* It was introduced in TypeScript 4.9.
* It is useful for configuration objects, route definitions, and structured data.
* It works with `Record`, function types, arrays, and object types.
* It can be combined with `as const` for readonly literal inference.
* It performs compile-time checking and does not add runtime validation.
