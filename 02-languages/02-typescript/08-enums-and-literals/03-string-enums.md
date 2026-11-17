# String Enums

String enums are TypeScript enums whose members are associated with string values.

They provide meaningful names for a fixed set of string constants and are useful when values need to remain readable in application code, logs, databases, and API communication.

String enums are commonly used for:

* User roles.
* Order statuses.
* Payment methods.
* Application states.
* Notification types.
* Authentication providers.
* Workflow stages.
* Configuration options.

Example:

```typescript
enum Status {
  Pending = "pending",
  Approved = "approved",
  Rejected = "rejected",
}
```

Unlike numeric enums, string enum members must have explicitly assigned string values.

# 🗁 Basic Syntax

Use the `enum` keyword and assign a string value to each member.

```typescript
enum EnumName {
  MemberOne = "value-one",
  MemberTwo = "value-two",
  MemberThree = "value-three",
}
```

Example:

```typescript
enum Direction {
  North = "north",
  South = "south",
  East = "east",
  West = "west",
}
```

Access enum members using dot notation:

```typescript
console.log(Direction.North); // "north"
console.log(Direction.South); // "south"
```

# 🗁 Explicit String Values

Every string enum member must have an explicit string initializer.

```typescript
enum UserRole {
  Admin = "admin",
  Editor = "editor",
  Viewer = "viewer",
}
```

Unlike numeric enums, TypeScript does not automatically generate string values.

The following declaration is invalid:

```typescript
// Error: Enum member must have initializer.
enum UserRole {
  Admin,
  Editor,
  Viewer,
}
```

# 🗁 Using String Enums

String enums can be assigned to variables.

```typescript
enum Status {
  Pending = "pending",
  Processing = "processing",
  Completed = "completed",
}

const currentStatus: Status = Status.Pending;

console.log(currentStatus); // "pending"
```

You can compare enum members:

```typescript
if (currentStatus === Status.Pending) {
  console.log("Order is waiting");
}
```

# 🗁 String Enums with Functions

String enums can be used as function parameter and return types.

```typescript
enum PaymentStatus {
  Pending = "pending",
  Paid = "paid",
  Failed = "failed",
}

function getPaymentMessage(status: PaymentStatus): string {
  switch (status) {
    case PaymentStatus.Pending:
      return "Payment is pending";

    case PaymentStatus.Paid:
      return "Payment completed";

    case PaymentStatus.Failed:
      return "Payment failed";
  }
}
```

Usage:

```typescript
console.log(getPaymentMessage(PaymentStatus.Paid));
```

Output:

```text
Payment completed
```

You can also return a string enum member:

```typescript
function getDefaultStatus(): PaymentStatus {
  return PaymentStatus.Pending;
}
```

# 🗁 String Enums with Objects

String enums are useful for defining object property values.

```typescript
enum AccountStatus {
  Active = "active",
  Suspended = "suspended",
  Closed = "closed",
}

type Account = {
  id: number;
  username: string;
  status: AccountStatus;
};

const account: Account = {
  id: 101,
  username: "alice",
  status: AccountStatus.Active,
};
```

Access the status:

```typescript
if (account.status === AccountStatus.Active) {
  console.log("Account is active");
}
```

# 🗁 String Enums with Interfaces

Interfaces can use string enums as property types.

```typescript
enum OrderStatus {
  Pending = "pending",
  Processing = "processing",
  Shipped = "shipped",
  Delivered = "delivered",
}

interface Order {
  id: number;
  total: number;
  status: OrderStatus;
}
```

Create an order:

```typescript
const order: Order = {
  id: 1001,
  total: 2500,
  status: OrderStatus.Processing,
};
```

# 🗁 String Enums with Arrays

You can create arrays containing string enum members.

```typescript
enum Permission {
  Read = "read",
  Write = "write",
  Update = "update",
  Delete = "delete",
}

const permissions: Permission[] = [
  Permission.Read,
  Permission.Write,
  Permission.Update,
];
```

Iterate over the permissions:

```typescript
for (const permission of permissions) {
  console.log(permission);
}
```

# 🗁 String Enums with Classes

Classes can use string enums for properties, constructor parameters, and methods.

```typescript
enum EmployeeRole {
  Manager = "manager",
  Developer = "developer",
  Designer = "designer",
}

class Employee {
  constructor(
    public name: string,
    public role: EmployeeRole,
  ) {}

  displayRole(): void {
    console.log(`${this.name}: ${this.role}`);
  }
}
```

Usage:

```typescript
const employee = new Employee(
  "Alice",
  EmployeeRole.Developer,
);

employee.displayRole();
```

Output:

```text
Alice: developer
```

# 🗁 String Enums and API Communication

String enums are particularly useful when communicating with APIs because their values are human-readable.

Example:

```typescript
enum OrderStatus {
  Pending = "pending",
  Processing = "processing",
  Completed = "completed",
  Cancelled = "cancelled",
}

type Order = {
  id: number;
  status: OrderStatus;
};

const order: Order = {
  id: 1001,
  status: OrderStatus.Processing,
};

const payload = JSON.stringify(order);

console.log(payload);
```

Output:

```json
{
  "id": 1001,
  "status": "processing"
}
```

The serialized value is descriptive and does not depend on automatically assigned numeric identifiers.

# 🗁 String Enums and Runtime Validation

TypeScript types are removed during compilation. Therefore, API responses and other external data sources must be validated at runtime.

A string enum does not automatically validate incoming strings.

Example:

```typescript
enum Status {
  Pending = "pending",
  Approved = "approved",
  Rejected = "rejected",
}

function isStatus(value: unknown): value is Status {
  return (
    typeof value === "string" &&
    Object.values(Status).some((status) => status === value)
  );
}
```

Usage:

```typescript
const apiValue: unknown = "approved";

if (isStatus(apiValue)) {
  const status: Status = apiValue;
  console.log("Valid status:", status);
} else {
  console.log("Invalid status");
}
```

This approach checks whether the external value matches one of the declared enum values.

# 🗁 Iterating Over String Enums

String enums do not generate reverse mappings.

Example:

```typescript
enum Direction {
  North = "north",
  South = "south",
  East = "east",
  West = "west",
}
```

Get member names:

```typescript
console.log(Object.keys(Direction));
```

Output:

```text
["North", "South", "East", "West"]
```

Get member values:

```typescript
console.log(Object.values(Direction));
```

Output:

```text
["north", "south", "east", "west"]
```

Get both names and values:

```typescript
console.log(Object.entries(Direction));
```

Output:

```text
[
  ["North", "north"],
  ["South", "south"],
  ["East", "east"],
  ["West", "west"]
]
```

# 🗁 No Reverse Mapping

Unlike numeric enums, string enums do not support reverse mapping.

```typescript
enum Status {
  Pending = "pending",
  Approved = "approved",
  Rejected = "rejected",
}

console.log(Status.Approved); // "approved"

// Not a reverse mapping:
// console.log(Status["approved"]);
```

The string value does not provide access to the member name through the enum object.

If you need to retrieve a member name from a value, use an explicit lookup:

```typescript
function getStatusName(value: string): string | undefined {
  return Object.entries(Status).find(
    ([, enumValue]) => enumValue === value,
  )?.[0];
}

console.log(getStatusName("approved")); // "Approved"
```

# 🗁 String Enums vs String Literal Unions

String enums and string literal unions can both represent a fixed set of string values.

String enum:

```typescript
enum Status {
  Pending = "pending",
  Approved = "approved",
  Rejected = "rejected",
}

let status: Status = Status.Pending;
```

String literal union:

```typescript
type Status =
  | "pending"
  | "approved"
  | "rejected";

let status: Status = "pending";
```

Comparison:

| Feature | String Enum | String Literal Union |
| --- | --- | --- |
| Declaration | `enum` | `type` |
| Runtime object | Yes | No |
| Explicit member names | Yes | No |
| Runtime enumeration | `Object.values()` | Requires a separate value source |
| Serialization | String values | String values |
| Reverse mapping | Not supported | Not applicable |
| Typical usage | Named runtime constants | Compile-time value constraints |

Choose based on whether you need a runtime enum object or only a compile-time type.

# 🗁 String Enums and Type Compatibility

String enum members are distinct enum types.

```typescript
enum Status {
  Pending = "pending",
  Approved = "approved",
}

let status: Status = Status.Pending;
```

A plain string with the same value is not generally assignable to the enum type.

```typescript
// Error: Type '"pending"' is not assignable to type 'Status'.

// status = "pending";
```

Use the enum member:

```typescript
status = Status.Pending;
```

If you need to accept plain strings from an external source, validate them before treating them as enum values.

# 🗁 Common Mistakes

**Forgetting explicit string values**

String enum members cannot rely on automatic numbering or automatic string generation.

```typescript
enum Status {
  Pending = "pending",
  Approved = "approved",
}
```

**Confusing member names with member values**

```typescript
enum Status {
  Pending = "pending",
}

console.log(Status.Pending); // "pending"
```

`Pending` is the member name, while `"pending"` is its value.

**Expecting reverse mapping**

String enums do not generate numeric-style reverse mappings.

Use an explicit lookup when the member name is required.

**Assigning plain strings directly**

A plain string is not generally assignable to a string enum type, even if its contents match a member value.

Use the corresponding enum member or validate external input.

**Assuming TypeScript validates API responses**

TypeScript types disappear at runtime. Validate external values before using them as enum members.

**Using inconsistent serialized values**

Choose stable string values for API contracts and database records. Changing an enum value can affect existing integrations and stored data.

# 🗁 Best Practices

* Use descriptive enum and member names.
* Assign explicit string values to every member.
* Prefer readable values for API communication and serialization.
* Keep serialized values stable when they are part of an external contract.
* Validate values received from APIs or other untrusted sources.
* Use `Object.values()` when runtime enumeration is needed.
* Avoid expecting reverse mapping.
* Consider string literal unions when a runtime enum object is unnecessary.
* Use exhaustive switch statements when handling all possible members.

# 🗁 Summary

* String enums associate named members with string values.
* Every string enum member requires an explicit initializer.
* String enum values are human-readable.
* They work with variables, functions, objects, arrays, interfaces, and classes.
* They are useful for API communication and serialization.
* String enums do not generate reverse mappings.
* `Object.keys()`, `Object.values()`, and `Object.entries()` can enumerate their members.
* Plain strings are not generally assignable to string enum types.
* Runtime validation is necessary for untrusted external values.
* String literal unions are an alternative when a runtime enum object is unnecessary.

# 🗁 Related Topics

* Enums
* Numeric Enums
* Const Enums
* Literal Types
* Template Literal Types
* As Const
* Union Types
* Type Narrowing
* Runtime Validation
* API Data Validation
