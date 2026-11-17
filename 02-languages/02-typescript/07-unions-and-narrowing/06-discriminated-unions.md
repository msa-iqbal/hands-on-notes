# Discriminated Unions

Discriminated unions are a TypeScript pattern that combines multiple object types using a common property with different literal values.

The common property is called a **discriminant** or **discriminant property**.

TypeScript uses this property to identify which member of a union is currently being used.

Discriminated unions are especially useful when:

* An object can have multiple possible structures.
* API responses have different formats.
* Application states have different properties.
* Events contain different kinds of data.
* You want predictable and type-safe conditional logic.
* You need exhaustive checking for all possible variants.

# 🗁 Basic Syntax

A discriminated union consists of:

* Multiple object types.
* A shared property.
* Different literal values assigned to that property.
* A union combining the object types.

Example:

```typescript
type Success = {
  status: "success";
  data: string;
};

type Error = {
  status: "error";
  message: string;
};

type ApiResponse = Success | Error;
```

Here, `status` is the discriminant property.

It has different literal values:

* `"success"` identifies the `Success` type.
* `"error"` identifies the `Error` type.

# 🗁 Basic Example

Create a union representing different user roles.

```typescript
type Admin = {
  role: "admin";
  name: string;
  permissions: string[];
};

type Customer = {
  role: "customer";
  name: string;
  shippingAddress: string;
};

type User = Admin | Customer;
```

Create values:

```typescript
const admin: User = {
  role: "admin",
  name: "Alice",
  permissions: ["create", "delete"],
};

const customer: User = {
  role: "customer",
  name: "Bob",
  shippingAddress: "Dhaka, Bangladesh",
};
```

The `role` property identifies the specific object type.

# 🗁 Narrowing with if and else

TypeScript automatically narrows discriminated unions when you check the discriminant property.

```typescript
function displayUser(user: User): void {
  if (user.role === "admin") {
    console.log(user.permissions);
  } else {
    console.log(user.shippingAddress);
  }
}
```

TypeScript understands that:

* Inside the `if` block, `user` is `Admin`.
* Inside the `else` block, `user` is `Customer`.

You can safely access properties specific to each type.

# 🗁 Narrowing with switch

A `switch` statement is often the clearest way to handle multiple discriminated union variants.

```typescript
function displayUser(user: User): void {
  switch (user.role) {
    case "admin":
      console.log("Administrator:", user.name);
      console.log("Permissions:", user.permissions);
      break;

    case "customer":
      console.log("Customer:", user.name);
      console.log("Address:", user.shippingAddress);
      break;
  }
}
```

Each case narrows the union to the corresponding object type.

This approach is especially useful when a union contains several variants.

# 🗁 Discriminated Unions with Multiple Variants

A discriminated union can contain more than two object types.

```typescript
type Pending = {
  status: "pending";
  submittedAt: Date;
};

type Approved = {
  status: "approved";
  approvedBy: string;
};

type Rejected = {
  status: "rejected";
  reason: string;
};

type ApplicationStatus =
  | Pending
  | Approved
  | Rejected;
```

Handle each variant:

```typescript
function handleApplication(
  application: ApplicationStatus,
): void {
  switch (application.status) {
    case "pending":
      console.log("Submitted:", application.submittedAt);
      break;

    case "approved":
      console.log("Approved by:", application.approvedBy);
      break;

    case "rejected":
      console.log("Reason:", application.reason);
      break;
  }
}
```

Each status has its own relevant properties.

# 🗁 Choosing a Discriminant Property

A discriminant can be any shared property whose type is a distinguishable literal value.

Common property names include:

* `type`
* `kind`
* `status`
* `action`
* `event`
* `state`
* `role`

Example using `type`:

```typescript
type EmailNotification = {
  type: "email";
  recipient: string;
  subject: string;
};

type SmsNotification = {
  type: "sms";
  phoneNumber: string;
  message: string;
};

type Notification =
  | EmailNotification
  | SmsNotification;
```

Example using `kind`:

```typescript
type Circle = {
  kind: "circle";
  radius: number;
};

type Rectangle = {
  kind: "rectangle";
  width: number;
  height: number;
};

type Shape = Circle | Rectangle;
```

Choose a property name that clearly communicates the purpose of the variants.

# 🗁 Discriminated Unions with Function Parameters

Functions can accept discriminated union types and handle each variant safely.

```typescript
type CreateUser = {
  action: "create";
  name: string;
  email: string;
};

type DeleteUser = {
  action: "delete";
  userId: number;
};

type UserAction = CreateUser | DeleteUser;
```

Create a handler:

```typescript
function handleUserAction(action: UserAction): void {
  switch (action.action) {
    case "create":
      console.log("Creating user:", action.name);
      console.log("Email:", action.email);
      break;

    case "delete":
      console.log("Deleting user:", action.userId);
      break;
  }
}
```

Usage:

```typescript
handleUserAction({
  action: "create",
  name: "Alice",
  email: "alice@example.com",
});

handleUserAction({
  action: "delete",
  userId: 101,
});
```

TypeScript ensures that each action contains the properties required by its variant.

# 🗁 Discriminated Unions with API Responses

Discriminated unions are particularly useful for modeling API responses.

An API may return a successful response, a validation error, or a server error.

```typescript
type ApiSuccess<T> = {
  status: "success";
  data: T;
};

type ValidationError = {
  status: "validation-error";
  errors: string[];
};

type ServerError = {
  status: "server-error";
  message: string;
};

type ApiResponse<T> =
  | ApiSuccess<T>
  | ValidationError
  | ServerError;
```

Define a user model:

```typescript
type User = {
  id: number;
  name: string;
  email: string;
};
```

Create a response handler:

```typescript
function handleApiResponse(
  response: ApiResponse<User>,
): void {
  switch (response.status) {
    case "success":
      console.log("User:", response.data.name);
      break;

    case "validation-error":
      console.log("Validation errors:", response.errors);
      break;

    case "server-error":
      console.log("Server error:", response.message);
      break;
  }
}
```

This creates a clear contract for different API response structures.

# 🗁 Discriminated Unions with Application States

A common application state can be represented using a discriminated union.

Consider a data-loading operation.

```typescript
type LoadingState = {
  status: "loading";
};

type SuccessState<T> = {
  status: "success";
  data: T;
};

type ErrorState = {
  status: "error";
  error: string;
};

type AsyncState<T> =
  | LoadingState
  | SuccessState<T>
  | ErrorState;
```

Create a rendering function:

```typescript
function renderState<T>(state: AsyncState<T>): void {
  switch (state.status) {
    case "loading":
      console.log("Loading...");
      break;

    case "success":
      console.log("Data:", state.data);
      break;

    case "error":
      console.log("Error:", state.error);
      break;
  }
}
```

This prevents invalid combinations such as a loading state containing an error message and a success state without data.

# 🗁 Discriminated Unions with Events

Applications often handle different event types with different data.

```typescript
type ClickEvent = {
  type: "click";
  x: number;
  y: number;
};

type KeyboardEvent = {
  type: "keyboard";
  key: string;
};

type SubmitEvent = {
  type: "submit";
  formId: string;
};

type AppEvent =
  | ClickEvent
  | KeyboardEvent
  | SubmitEvent;
```

Handle events:

```typescript
function handleEvent(event: AppEvent): void {
  switch (event.type) {
    case "click":
      console.log(`Clicked at ${event.x}, ${event.y}`);
      break;

    case "keyboard":
      console.log(`Pressed key: ${event.key}`);
      break;

    case "submit":
      console.log(`Submitted form: ${event.formId}`);
      break;
  }
}
```

Each event exposes only the properties relevant to its type.

# 🗁 Discriminated Unions with Classes

Discriminated unions can combine class instances when they expose a common literal property.

```typescript
class Admin {
  readonly role = "admin";

  constructor(
    public name: string,
    public permissions: string[],
  ) {}
}

class Customer {
  readonly role = "customer";

  constructor(
    public name: string,
    public address: string,
  ) {}
}

type User = Admin | Customer;
```

Use the discriminant:

```typescript
function displayUser(user: User): void {
  switch (user.role) {
    case "admin":
      console.log(user.permissions);
      break;

    case "customer":
      console.log(user.address);
      break;
  }
}
```

TypeScript narrows the class union using the literal `role` property.

# 🗁 Discriminated Unions with Nested Objects

The discriminant property does not have to be at the top level of a complex data structure, but TypeScript's narrowing behavior depends on how the nested structure is modeled.

A simple nested example:

```typescript
type EmailMessage = {
  channel: {
    type: "email";
    recipient: string;
  };
  subject: string;
};

type SmsMessage = {
  channel: {
    type: "sms";
    phoneNumber: string;
  };
  message: string;
};

type Message = EmailMessage | SmsMessage;
```

Narrow using the nested discriminant:

```typescript
function sendMessage(message: Message): void {
  if (message.channel.type === "email") {
    console.log("Recipient:", message.channel.recipient);
    console.log("Subject:", message.subject);
  } else {
    console.log("Phone:", message.channel.phoneNumber);
    console.log("Message:", message.message);
  }
}
```

This allows the compiler to identify the correct message structure.

# 🗁 Exhaustive Checking with never

Exhaustive checking ensures that every possible union member is handled.

Consider:

```typescript
type Status =
  | "pending"
  | "approved"
  | "rejected";
```

Create a helper function:

```typescript
function assertNever(value: never): never {
  throw new Error(`Unexpected value: ${value}`);
}
```

Use it in a switch statement:

```typescript
function handleStatus(status: Status): string {
  switch (status) {
    case "pending":
      return "Waiting for approval";

    case "approved":
      return "Request approved";

    case "rejected":
      return "Request rejected";

    default:
      return assertNever(status);
  }
}
```

If another status is added:

```typescript
type Status =
  | "pending"
  | "approved"
  | "rejected"
  | "cancelled";
```

The compiler reports an error because `"cancelled"` is not handled.

This makes discriminated unions safer to maintain as applications grow.

# 🗁 Discriminated Unions with Optional Properties

Discriminated unions can also contain optional properties.

```typescript
type GuestUser = {
  role: "guest";
  name: string;
  email?: string;
};

type RegisteredUser = {
  role: "registered";
  name: string;
  email: string;
  passwordHash: string;
};

type User = GuestUser | RegisteredUser;
```

Handle the variants:

```typescript
function displayUser(user: User): void {
  switch (user.role) {
    case "guest":
      console.log(user.name);
      console.log(user.email ?? "No email");
      break;

    case "registered":
      console.log(user.name);
      console.log(user.email);
      break;
  }
}
```

The discriminant identifies the object variant, while optional properties represent fields that may be unavailable within that variant.

# 🗁 Discriminated Unions with Intersection Types

You can combine common properties with a discriminated union.

```typescript
type BaseEntity = {
  id: number;
  createdAt: Date;
};

type User = {
  type: "user";
  name: string;
  email: string;
};

type Product = {
  type: "product";
  title: string;
  price: number;
};

type Entity = BaseEntity & (User | Product);
```

Example:

```typescript
function displayEntity(entity: Entity): void {
  console.log("ID:", entity.id);

  switch (entity.type) {
    case "user":
      console.log("User:", entity.name);
      break;

    case "product":
      console.log("Product:", entity.title);
      break;
  }
}
```

This pattern avoids repeating common properties across multiple variants.

# 🗁 Discriminated Unions with Generic Types

Generic types make discriminated unions reusable for different data models.

```typescript
type Success<T> = {
  status: "success";
  data: T;
};

type Failure = {
  status: "failure";
  error: string;
};

type Result<T> = Success<T> | Failure;
```

Use different data types:

```typescript
type User = {
  id: number;
  name: string;
};

type Product = {
  id: number;
  title: string;
};

type UserResult = Result<User>;

type ProductResult = Result<Product>;
```

Create a reusable handler:

```typescript
function handleResult<T>(result: Result<T>): void {
  switch (result.status) {
    case "success":
      console.log(result.data);
      break;

    case "failure":
      console.log(result.error);
      break;
  }
}
```

The generic type controls the success data while the discriminant identifies the result variant.

# 🗁 Discriminated Unions vs Ordinary Union Types

Ordinary unions can combine multiple object types.

```typescript
type Admin = {
  name: string;
  permissions: string[];
};

type Customer = {
  name: string;
  address: string;
};

type User = Admin | Customer;
```

However, TypeScript needs another way to distinguish the variants.

A discriminated union provides a shared literal property:

```typescript
type Admin = {
  role: "admin";
  name: string;
  permissions: string[];
};

type Customer = {
  role: "customer";
  name: string;
  address: string;
};

type User = Admin | Customer;
```

Comparison:

| Feature | Ordinary Union | Discriminated Union |
| --- | --- | --- |
| Combines multiple types | Yes | Yes |
| Shared literal property | Not required | Required |
| Identifies variants | Through available checks | Through discriminant |
| Readability | Depends on structure | Often clearer |
| Exhaustive checking | Possible | Straightforward |

# 🗁 Discriminated Unions vs Enums

Both discriminated unions and enums can represent a fixed set of values, but they solve different problems.

Using literal types:

```typescript
type Status = "pending" | "approved" | "rejected";
```

Using an enum:

```typescript
enum Status {
  Pending = "pending",
  Approved = "approved",
  Rejected = "rejected",
}
```

Literal unions are often sufficient when you only need a set of permitted values.

Enums can be useful when you want a named runtime object representing the values.

Discriminated unions go further by associating each variant with its own object structure.

# 🗁 Common Mistakes

**Using broad string types for discriminants**

```typescript
type Admin = {
  role: string;
  permissions: string[];
};

type Customer = {
  role: string;
  address: string;
};
```

The compiler cannot distinguish these types through `role` because both use the broad `string` type.

Use literal types:

```typescript
type Admin = {
  role: "admin";
  permissions: string[];
};
```

**Using inconsistent discriminant property names**

```typescript
type Admin = {
  role: "admin";
};

type Customer = {
  type: "customer";
};
```

These objects do not share a common discriminant.

Use the same property name across all variants.

**Using overlapping literal values unintentionally**

```typescript
type Admin = {
  role: "user" | "admin";
  permissions: string[];
};

type Customer = {
  role: "user";
  address: string;
};
```

Checking `role === "user"` does not uniquely identify one member.

Choose distinct discriminant values when each variant must be distinguishable.

**Forgetting to handle a new variant**

When adding a new member to a union, existing switch statements may become incomplete.

Use `never`-based exhaustive checking to detect missing cases.

**Making every property optional instead of modeling variants**

```typescript
type User = {
  role: "admin" | "customer";
  permissions?: string[];
  address?: string;
};
```

This allows invalid combinations, such as an administrator without permissions.

Use separate object types:

```typescript
type User =
  | {
      role: "admin";
      permissions: string[];
    }
  | {
      role: "customer";
      address: string;
    };
```

# 🗁 Best Practices

* Use a shared discriminant property for related object variants.
* Prefer specific literal values over broad string or number types.
* Choose meaningful discriminant names such as `type`, `kind`, or `status`.
* Use `switch` statements when handling several variants.
* Keep each variant focused on its own properties.
* Avoid unnecessary optional properties that allow invalid object combinations.
* Use `never` for exhaustive checking.
* Combine intersections with discriminated unions to reuse common properties.
* Use generics when the same union structure must support different data types.
* Model application states explicitly rather than representing every possible field as optional.

# 🗁 Summary

* Discriminated unions combine multiple object types using a shared literal property.
* The shared property is called a discriminant.
* TypeScript uses discriminants to narrow union members automatically.
* `if`, `else`, and `switch` can handle different variants.
* Discriminated unions are useful for API responses, application states, events, and user roles.
* They can be combined with generics and intersection types.
* The `never` type supports exhaustive checking.
* Explicit variants help prevent invalid object structures.
* Discriminated unions make complex application logic easier to understand and maintain.

# 🗁 Related Topics

* Union Types
* Intersection Types
* Type Narrowing
* Type Guards
* Type Predicates
* In Operator
* Instanceof
* Exhaustiveness Checking
* Literal Types
* Generic Types
* Never Type
* Control Flow Analysis
