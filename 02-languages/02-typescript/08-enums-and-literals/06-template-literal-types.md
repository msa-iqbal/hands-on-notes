# Template Literal Types

Template literal types are a TypeScript feature that allows you to create new string literal types by combining existing string literal types.

They use syntax similar to JavaScript template literals, but operate at the type level rather than generating runtime strings.

Template literal types are useful for:

* Creating structured string types.
* Defining event names.
* Generating API route types.
* Building CSS class names.
* Defining property-based event handlers.
* Creating naming conventions.
* Transforming string literal types.
* Building strongly typed configuration keys.

Example:

```typescript
type Greeting = `Hello, ${string}`;

const message: Greeting = "Hello, Alice";
```

Valid:

```typescript
const first: Greeting = "Hello, Bob";
const second: Greeting = "Hello, World";
```

Invalid:

```typescript
// TypeScript error
// const third: Greeting = "Good morning, Alice";
```

# 🗁 Basic Syntax

Template literal types use backticks and `${}` placeholders.

```typescript
type TypeName = `prefix-${SomeType}-suffix`;
```

Example:

```typescript
type UserId = `user-${number}`;

const id: UserId = "user-101";
```

The `number` type allows numeric values in the string pattern.

# 🗁 Combining String Literal Types

You can combine existing string literal types.

```typescript
type Color = "red" | "green" | "blue";

type ColorClass = `bg-${Color}`;
```

The resulting type is equivalent to:

```typescript
type ColorClass =
  | "bg-red"
  | "bg-green"
  | "bg-blue";
```

Usage:

```typescript
const background: ColorClass = "bg-red";
const foreground: ColorClass = "bg-blue";
```

Invalid:

```typescript
// TypeScript error
// const invalid: ColorClass = "bg-yellow";
```

# 🗁 Multiple Placeholders

Template literal types can contain multiple placeholders.

```typescript
type Size = "small" | "medium" | "large";
type Color = "red" | "green" | "blue";

type ButtonClass = `${Size}-${Color}`;
```

The resulting type contains every combination:

```typescript
type ButtonClass =
  | "small-red"
  | "small-green"
  | "small-blue"
  | "medium-red"
  | "medium-green"
  | "medium-blue"
  | "large-red"
  | "large-green"
  | "large-blue";
```

Usage:

```typescript
const button: ButtonClass = "medium-blue";
```

# 🗁 Union Distribution

When a placeholder contains a union, TypeScript generates combinations of its possible members.

```typescript
type Method = "GET" | "POST";
type Resource = "users" | "products";

type ApiEndpoint = `${Method} /${Resource}`;
```

The resulting type is:

```typescript
type ApiEndpoint =
  | "GET /users"
  | "GET /products"
  | "POST /users"
  | "POST /products";
```

This behavior is useful for creating structured types from multiple independent unions.

# 🗁 Template Literal Types with Primitive Types

Template literal types can combine string literal types with broader primitive types.

**String**

```typescript
type Message = `Hello ${string}`;

const message: Message = "Hello Developer";
```

**Number**

```typescript
type UserId = `user-${number}`;

const userId: UserId = "user-123";
```

**Boolean**

```typescript
type FeatureFlag = `feature-${boolean}`;

const flag: FeatureFlag = "feature-true";
```

**BigInt**

```typescript
type BigIntIdentifier = `id-${bigint}`;

const identifier: BigIntIdentifier = "id-100";
```

These patterns constrain the overall string format, although they do not provide full runtime validation.

# 🗁 Template Literal Types with Type Aliases

Type aliases make template literal types reusable.

```typescript
type Environment = "development" | "production";
type Service = "api" | "database" | "cache";

type ServiceName = `${Environment}-${Service}`;
```

Usage:

```typescript
const apiService: ServiceName = "development-api";
const databaseService: ServiceName = "production-database";
```

Invalid:

```typescript
// TypeScript error
// const service: ServiceName = "testing-api";
```

# 🗁 Template Literal Types with Object Keys

Template literal types can generate new property names from existing object keys.

```typescript
type User = {
  name: string;
  age: number;
  email: string;
};

type UserEventNames = `${keyof User}Changed`;
```

The resulting type is:

```typescript
type UserEventNames =
  | "nameChanged"
  | "ageChanged"
  | "emailChanged";
```

Usage:

```typescript
function onUserEvent(event: UserEventNames): void {
  console.log(event);
}

onUserEvent("nameChanged");
onUserEvent("emailChanged");
```

# 🗁 Template Literal Types with Keyof

The `keyof` operator produces a union of an object's property keys.

Combining `keyof` with template literal types allows you to create string types based on those keys.

```typescript
type Product = {
  id: number;
  name: string;
  price: number;
};

type ProductEvent = `${keyof Product}Changed`;
```

The resulting type is:

```typescript
type ProductEvent =
  | "idChanged"
  | "nameChanged"
  | "priceChanged";
```

This is useful when building event systems based on object properties.

# 🗁 Template Literal Types with Generic Types

Generic types allow template literal patterns to be reused with different inputs.

```typescript
type Prefix<T extends string> = `app-${T}`;

type UserPrefix = Prefix<"user">;
type AdminPrefix = Prefix<"admin">;
```

The resulting types are:

```typescript
type UserPrefix = "app-user";
type AdminPrefix = "app-admin";
```

Another example:

```typescript
type EventName<T extends string> = `${T}Changed`;

type UserEvent = EventName<"username">;
type ProductEvent = EventName<"price">;
```

# 🗁 Template Literal Types with Mapped Types

Template literal types can generate property names inside mapped types.

```typescript
type User = {
  name: string;
  age: number;
};

type Getters<T> = {
  [K in keyof T as `get${Capitalize<string & K>}`]: () => T[K];
};
```

Apply the mapped type:

```typescript
type UserGetters = Getters<User>;
```

The resulting type is:

```typescript
type UserGetters = {
  getName: () => string;
  getAge: () => number;
};
```

Usage:

```typescript
const userGetters: UserGetters = {
  getName: () => "Alice",
  getAge: () => 25,
};
```

# 🗁 String Manipulation Utility Types

TypeScript provides intrinsic string manipulation types that work with string literal types.

These utilities can transform strings at the type level.

**Uppercase**

Converts a string literal to uppercase.

```typescript
type Status = "active";

type UpperStatus = Uppercase<Status>;

// "ACTIVE"
```

**Lowercase**

Converts a string literal to lowercase.

```typescript
type Status = "ACTIVE";

type LowerStatus = Lowercase<Status>;

// "active"
```

**Capitalize**

Converts the first character to uppercase.

```typescript
type Name = "alice";

type CapitalizedName = Capitalize<Name>;

// "Alice"
```

**Uncapitalize**

Converts the first character to lowercase.

```typescript
type Name = "Alice";

type UncapitalizedName = Uncapitalize<Name>;

// "alice"
```

These utilities are especially useful when generating property names and event names.

# 🗁 Template Literal Types with Uppercase

You can transform generated strings using `Uppercase`.

```typescript
type Method = "get" | "post" | "delete";

type UpperMethod = Uppercase<Method>;
```

Result:

```typescript
type UpperMethod = "GET" | "POST" | "DELETE";
```

Combine it with a template literal type:

```typescript
type HttpMethod = `${Uppercase<Method>} /users`;
```

Result:

```typescript
type HttpMethod =
  | "GET /users"
  | "POST /users"
  | "DELETE /users";
```

# 🗁 Template Literal Types with Capitalize

`Capitalize` is useful for generating method names and property names.

```typescript
type Action = "create" | "update" | "delete";

type ActionMethod = `handle${Capitalize<Action>}`;
```

Result:

```typescript
type ActionMethod =
  | "handleCreate"
  | "handleUpdate"
  | "handleDelete";
```

# 🗁 Strongly Typed Event Names

One practical use of template literal types is creating event names from object properties.

```typescript
type User = {
  name: string;
  age: number;
  email: string;
};

type UserEvent = `${keyof User}Changed`;
```

You can build a generic event listener:

```typescript
type PropEventSource<T> = {
  on<K extends string & keyof T>(
    event: `${K}Changed`,
    callback: (value: T[K]) => void,
  ): void;
};
```

Create an object with the event interface:

```typescript
declare const userEvents: PropEventSource<User>;
```

Usage:

```typescript
userEvents.on("nameChanged", (value) => {
  console.log(value.toUpperCase());
});

userEvents.on("ageChanged", (value) => {
  console.log(value.toFixed(0));
});
```

TypeScript infers the callback parameter from the selected property.

For example, `nameChanged` provides a `string`, while `ageChanged` provides a `number`.

Invalid event names are rejected:

```typescript
// TypeScript error
// userEvents.on("addressChanged", () => {});
```

# 🗁 Strongly Typed API Routes

Template literal types can define structured API route patterns.

```typescript
type Resource = "users" | "products" | "orders";

type ApiRoute = `/api/${Resource}`;

const usersRoute: ApiRoute = "/api/users";
const productsRoute: ApiRoute = "/api/products";
```

You can also include numeric identifiers:

```typescript
type ResourceId = `/api/users/${number}`;

const userRoute: ResourceId = "/api/users/101";
```

For a more reusable pattern:

```typescript
type ApiRoute<T extends string> = `/api/${T}`;

type UserRoute = ApiRoute<"users">;
type ProductRoute = ApiRoute<"products">;
```

# 🗁 Strongly Typed CSS Classes

Template literal types can help define CSS naming conventions.

```typescript
type Color = "red" | "green" | "blue";
type Shade = 100 | 200 | 300 | 400 | 500;

type BackgroundClass = `bg-${Color}-${Shade}`;
```

Valid values include:

```typescript
const first: BackgroundClass = "bg-red-100";
const second: BackgroundClass = "bg-blue-500";
```

Invalid:

```typescript
// TypeScript error
// const invalid: BackgroundClass = "bg-yellow-100";
```

# 🗁 Extracting Parts of String Types

Template literal types can infer portions of strings using `infer`.

Example:

```typescript
type ExtractId<T> =
  T extends `user-${infer Id}`
    ? Id
    : never;

type UserId = ExtractId<"user-123">;
```

The resulting type is:

```typescript
type UserId = "123";
```

Another example:

```typescript
type ExtractEvent<T> =
  T extends `${infer Property}Changed`
    ? Property
    : never;

type EventProperty = ExtractEvent<"nameChanged">;
```

The resulting type is:

```typescript
type EventProperty = "name";
```

This technique is useful for parsing structured string types.

# 🗁 Splitting String Literal Types

You can recursively split a string literal type using template literal types and conditional types.

```typescript
type Split<
  T extends string,
  Separator extends string,
> =
  T extends `${infer First}${Separator}${infer Rest}`
    ? [First, ...Split<Rest, Separator>]
    : T extends ""
      ? []
      : [T];
```

Usage:

```typescript
type Parts = Split<"users/products/orders", "/">;
```

Result:

```typescript
type Parts = ["users", "products", "orders"];
```

This technique demonstrates how template literal types can be used for compile-time string manipulation.

# 🗁 String Replacement

You can implement type-level string replacement using conditional types and `infer`.

```typescript
type Replace<
  T extends string,
  Search extends string,
  Replacement extends string,
> =
  Search extends ""
    ? T
    : T extends `${infer Start}${Search}${infer End}`
      ? `${Start}${Replacement}${End}`
      : T;
```

Usage:

```typescript
type Result = Replace<
  "hello-world",
  "-",
  "_"
>;
```

Result:

```typescript
type Result = "hello_world";
```

This example replaces the first matching occurrence.

# 🗁 Template Literal Types and Runtime Strings

Template literal types exist only in the TypeScript type system.

They do not automatically validate or transform runtime strings.

Example:

```typescript
type UserId = `user-${number}`;

function getUser(id: UserId): void {
  console.log(id);
}

getUser("user-101");
```

The type checks the argument during compilation.

If a value comes from an API or another external source, TypeScript cannot guarantee that it follows the pattern at runtime.

Runtime validation is still necessary:

```typescript
type UserId = `user-${number}`;

function isUserId(value: string): value is UserId {
  return /^user-\d+$/.test(value);
}

const externalValue = "user-101";

if (isUserId(externalValue)) {
  getUser(externalValue);
}
```

# 🗁 Union Explosion and Performance

When multiple placeholders contain unions, TypeScript generates combinations of their possible values.

```typescript
type Color = "red" | "green" | "blue";
type Size = "small" | "medium" | "large";
type State = "active" | "disabled";

type ClassName = `${Color}-${Size}-${State}`;
```

This creates:

* 3 color options.
* 3 size options.
* 2 state options.
* 18 possible combinations.

The number of generated combinations grows multiplicatively.

Large unions can create very large types and slow down type checking.

Avoid unnecessarily combining large unions in template literal types.

# 🗁 Common Mistakes

**Using ordinary string types instead of literal unions**

```typescript
type Color = string;

type Background = `bg-${Color}`;
```

This allows a much broader set of strings than a fixed color union.

Use explicit literal values when strict restrictions are required.

**Forgetting backticks**

Template literal types require backticks.

```typescript
type UserId = `user-${number}`;
```

**Assuming compile-time patterns validate runtime input**

Template literal types do not replace runtime validation.

**Creating excessively large unions**

Combining many large unions can generate an enormous number of possible types.

**Confusing template literal types with JavaScript template literals**

JavaScript template literals generate runtime strings. TypeScript template literal types generate compile-time types.

# 🗁 Best Practices

* Use template literal types for structured string patterns.
* Combine them with union types to generate controlled values.
* Use `keyof` to derive string types from object properties.
* Use mapped types to generate property names.
* Use `Capitalize`, `Uncapitalize`, `Uppercase`, and `Lowercase` for string transformations.
* Use `infer` when extracting parts of string literal types.
* Keep generated unions manageable.
* Use runtime validation for external strings.
* Prefer simpler types when a template literal type adds unnecessary complexity.

# 🗁 Summary

* Template literal types create new string types using template literal syntax.
* They combine string literals, unions, and other supported types.
* Multiple placeholders generate combinations of possible values.
* They work with `keyof`, generic types, and mapped types.
* String manipulation utilities can transform literal strings.
* `infer` can extract parts of structured string types.
* They support advanced patterns such as typed events and API routes.
* They operate at compile time, not runtime.
* Large generated unions can affect compiler performance.

# 🗁 Related Topics

* Literal Types
* Union Types
* Type Aliases
* Generic Types
* Keyof
* Mapped Types
* Conditional Types
* Infer
* String Manipulation Types
* Type-Level Programming
* Runtime Validation
