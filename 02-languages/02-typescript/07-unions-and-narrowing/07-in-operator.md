# In Operator

The `in` operator is a JavaScript operator that checks whether a specified property exists in an object or anywhere in its prototype chain.

In TypeScript, the `in` operator also works as a **type-narrowing mechanism**.

It helps TypeScript identify which member of a union type contains a particular property.

The `in` operator is especially useful when:

* Working with union types.
* Checking whether an object contains a property.
* Narrowing object types dynamically.
* Handling API responses with different structures.
* Working with optional properties.
* Implementing type-safe conditional logic.

# 🗁 Basic Syntax

```typescript
"propertyName" in object
```

The expression returns a boolean:

* `true` if the property exists in the object or its prototype chain.
* `false` if the property does not exist.

Example:

```typescript
const user = {
  name: "Alice",
  email: "alice@example.com",
};

console.log("name" in user); // true
console.log("age" in user); // false
```

# 🗁 Basic Type Narrowing

Consider two object types:

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

Both types have a common `name` property, but their other properties are different.

Use the `in` operator to identify the object structure:

```typescript
function displayUser(user: User): void {
  if ("permissions" in user) {
    console.log("Admin permissions:", user.permissions);
  } else {
    console.log("Customer address:", user.address);
  }
}
```

TypeScript narrows the type automatically:

* If `permissions` exists, `user` is narrowed to `Admin`.
* Otherwise, `user` is narrowed to `Customer`.

# 🗁 Using in with Multiple Union Members

The `in` operator can narrow unions containing multiple object types.

```typescript
type EmailNotification = {
  recipient: string;
  subject: string;
};

type SmsNotification = {
  phoneNumber: string;
  message: string;
};

type PushNotification = {
  deviceToken: string;
  title: string;
  body: string;
};

type Notification =
  | EmailNotification
  | SmsNotification
  | PushNotification;
```

Handle each notification:

```typescript
function sendNotification(
  notification: Notification,
): void {
  if ("recipient" in notification) {
    console.log("Sending email to:", notification.recipient);
  } else if ("phoneNumber" in notification) {
    console.log("Sending SMS to:", notification.phoneNumber);
  } else {
    console.log("Sending push notification:", notification.deviceToken);
  }
}
```

Each property check narrows the union to the corresponding type.

# 🗁 Using in with if and else

The `in` operator works naturally with conditional statements.

```typescript
type Car = {
  brand: string;
  numberOfDoors: number;
};

type Motorcycle = {
  brand: string;
  hasSidecar: boolean;
};

type Vehicle = Car | Motorcycle;
```

Example:

```typescript
function describeVehicle(vehicle: Vehicle): void {
  if ("numberOfDoors" in vehicle) {
    console.log(`${vehicle.brand} has ${vehicle.numberOfDoors} doors`);
  } else {
    console.log(`${vehicle.brand} has sidecar: ${vehicle.hasSidecar}`);
  }
}
```

The condition determines which properties are available inside each branch.

# 🗁 Using in with switch-like Logic

The `in` operator can be combined with multiple conditional checks to handle several object structures.

```typescript
type File = {
  fileName: string;
  size: number;
};

type Directory = {
  directoryName: string;
  children: string[];
};

type SymbolicLink = {
  linkName: string;
  target: string;
};

type FileSystemEntry = File | Directory | SymbolicLink;
```

Handle the entries:

```typescript
function describeEntry(entry: FileSystemEntry): void {
  if ("fileName" in entry) {
    console.log("File:", entry.fileName);
    console.log("Size:", entry.size);
  } else if ("directoryName" in entry) {
    console.log("Directory:", entry.directoryName);
    console.log("Children:", entry.children);
  } else {
    console.log("Symbolic link:", entry.linkName);
    console.log("Target:", entry.target);
  }
}
```

# 🗁 Using in with Optional Properties

Optional properties require special attention.

Consider:

```typescript
type Admin = {
  name: string;
  permissions?: string[];
};

type Customer = {
  name: string;
  address: string;
};

type User = Admin | Customer;
```

Check for `permissions`:

```typescript
function displayUser(user: User): void {
  if ("permissions" in user) {
    console.log(user.permissions);
  }
}
```

Because `permissions` is optional on `Admin`, TypeScript cannot guarantee that its value is an array. It may be `undefined`.

Also, when a property is optional in a union member, TypeScript can retain that member in both the true and false branches of the `in` check.

Use an additional value check when necessary:

```typescript
function displayUser(user: User): void {
  if ("permissions" in user && user.permissions !== undefined) {
    console.log("Permissions:", user.permissions);
  } else if ("address" in user) {
    console.log("Address:", user.address);
  } else {
    console.log("No permissions assigned");
  }
}
```

The property existence check and the value check serve different purposes.

# 🗁 Using in with API Responses

API responses often have different structures depending on whether an operation succeeds or fails.

```typescript
type SuccessResponse = {
  data: {
    id: number;
    name: string;
  };
};

type ErrorResponse = {
  error: string;
  statusCode: number;
};

type ApiResponse = SuccessResponse | ErrorResponse;
```

Handle the response:

```typescript
function handleResponse(response: ApiResponse): void {
  if ("data" in response) {
    console.log("User:", response.data.name);
  } else {
    console.log("Error:", response.error);
    console.log("Status:", response.statusCode);
  }
}
```

This provides type-safe access to the properties of each response structure.

# 🗁 Using in with Nested Objects

The `in` operator can check properties on nested objects.

```typescript
type EmailUser = {
  profile: {
    email: string;
  };
};

type PhoneUser = {
  profile: {
    phone: string;
  };
};

type User = EmailUser | PhoneUser;
```

Example:

```typescript
function displayContact(user: User): void {
  if ("email" in user.profile) {
    console.log("Email:", user.profile.email);
  } else {
    console.log("Phone:", user.profile.phone);
  }
}
```

TypeScript narrows the nested object based on the property check.

# 🗁 Using in with Unknown

The `in` operator can help inspect values typed as `unknown`.

However, the value must first be narrowed to a non-null object.

```typescript
function isObject(value: unknown): value is object {
  return typeof value === "object" && value !== null;
}
```

Create a property-checking function:

```typescript
function hasName(value: unknown): value is { name: unknown } {
  return (
    typeof value === "object" &&
    value !== null &&
    "name" in value
  );
}
```

Usage:

```typescript
const input: unknown = {
  name: "Alice",
  age: 25,
};

if (hasName(input)) {
  console.log(input.name);
}
```

The custom type predicate tells TypeScript that the checked value has a `name` property.

Notice that the property type is `unknown`, not automatically `string`. A separate check is needed to validate its value.

```typescript
if (hasName(input) && typeof input.name === "string") {
  console.log(input.name.toUpperCase());
}
```

# 🗁 Creating a Reusable Property Guard

You can create a generic type guard for checking whether an object contains a property.

```typescript
function hasProperty<K extends PropertyKey>(
  value: unknown,
  key: K,
): value is Record<K, unknown> {
  return (
    typeof value === "object" &&
    value !== null &&
    key in value
  );
}
```

Usage:

```typescript
const response: unknown = {
  message: "Operation completed",
};

if (hasProperty(response, "message")) {
  console.log(response.message);
}
```

This function:

* Accepts an unknown value.
* Checks that the value is a non-null object.
* Checks whether the specified property exists.
* Narrows the value to an object containing that property with an `unknown` value.

It does not validate the property's actual value type.

# 🗁 Using in with Classes

The `in` operator can check whether an instance exposes a particular property.

```typescript
class Admin {
  constructor(
    public name: string,
    public permissions: string[],
  ) {}
}

class Customer {
  constructor(
    public name: string,
    public address: string,
  ) {}
}

type User = Admin | Customer;
```

Example:

```typescript
function displayUser(user: User): void {
  if ("permissions" in user) {
    console.log("Admin:", user.name);
    console.log(user.permissions);
  } else {
    console.log("Customer:", user.name);
    console.log(user.address);
  }
}
```

This checks for a property rather than checking the actual class constructor.

# 🗁 Using in with Generic Types

The `in` operator can narrow generic union types when their members have distinguishable properties.

```typescript
type Success<T> = {
  data: T;
};

type Failure = {
  error: string;
};

type Result<T> = Success<T> | Failure;
```

Create a handler:

```typescript
function handleResult<T>(result: Result<T>): void {
  if ("data" in result) {
    console.log("Success:", result.data);
  } else {
    console.log("Failure:", result.error);
  }
}
```

The property check identifies the appropriate member without requiring a specific type argument.

# 🗁 in Operator vs Discriminated Unions

Both techniques can narrow union types, but they use different approaches.

**Using the in operator:**

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

function displayUser(user: User): void {
  if ("permissions" in user) {
    console.log(user.permissions);
  } else {
    console.log(user.address);
  }
}
```

**Using a discriminated union:**

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

Comparison:

| Feature | in Operator | Discriminated Union |
| --- | --- | --- |
| Narrowing mechanism | Property existence | Literal property value |
| Requires a shared discriminant | No | Yes |
| Works with existing object structures | Yes | Requires suitable structure |
| Explicit variant identification | Indirect | Direct |
| Exhaustive checking | Requires additional care | Straightforward with `never` |

Use the `in` operator when property existence naturally distinguishes object structures.

Use discriminated unions when you control the data model and can define explicit variants.

# 🗁 JavaScript Runtime Behavior

The `in` operator is a JavaScript runtime operator, not just a TypeScript feature.

It checks properties on an object and its prototype chain.

Example:

```typescript
const user = {
  name: "Alice",
};

console.log("name" in user); // true
console.log("toString" in user); // true
```

The `toString` property exists through the object's prototype chain.

To check only an object's own properties, use `Object.hasOwn()`:

```typescript
console.log(Object.hasOwn(user, "name")); // true
console.log(Object.hasOwn(user, "toString")); // false
```

These operations have different purposes:

* `in` checks own and inherited properties.
* `Object.hasOwn()` checks only own properties.

# 🗁 Important Considerations

**The right-hand side must be an object**

The `in` operator cannot be used with primitive values on the right-hand side.

```typescript
console.log("name" in { name: "Alice" }); // true
```

This is invalid:

```typescript
// Runtime error
console.log("name" in "Alice");
```

**The property must be a valid property key**

The left-hand side must be a property key, which can be a string or symbol.

```typescript
const key = Symbol("id");

const user = {
  [key]: 101,
};

console.log(key in user); // true
```

**Property existence does not guarantee a valid value**

```typescript
const user = {
  name: undefined,
};

console.log("name" in user); // true
```

The property exists even though its value is `undefined`.

Use a separate value check when the value itself matters.

**Optional properties can appear in both narrowing branches**

When a union member declares a property as optional, TypeScript may retain that member in both branches of an `in` check.

Do not assume that a successful or unsuccessful property check always identifies exactly one union member.

# 🗁 Best Practices

* Use `in` to narrow object unions based on property existence.
* Prefer discriminated unions when you control the data structure and need explicit variants.
* Check that an unknown value is a non-null object before using `in`.
* Use custom type predicates for reusable runtime validation.
* Remember that property existence does not validate its value type.
* Handle optional properties carefully.
* Use `Object.hasOwn()` when inherited properties should not count.
* Avoid using broad or overlapping object structures that make narrowing ambiguous.

# 🗁 Summary

* The `in` operator checks whether a property exists in an object or its prototype chain.
* It returns a boolean.
* TypeScript uses it to narrow union types.
* It works with object unions, nested objects, classes, and generic types.
* Optional properties can affect narrowing behavior.
* The operator does not validate the property's value type.
* Unknown values must be narrowed to non-null objects before using `in`.
* `Object.hasOwn()` checks only an object's own properties.
* The `in` operator is useful when property existence distinguishes object structures.

# 🗁 Related Topics

* Union Types
* Type Narrowing
* Type Guards
* Type Predicates
* Discriminated Unions
* Instanceof
* Unknown Type
* Object Types
* Property Keys
* Runtime Type Checking
