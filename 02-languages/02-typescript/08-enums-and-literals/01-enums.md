# Enums

An **enum (enumeration)** is a TypeScript feature that allows you to define a collection of named constants.

Enums provide meaningful names for a set of related values instead of using raw numbers or strings throughout an application.

Enums are useful when:

* Defining application states.
* Representing user roles.
* Managing order statuses.
* Defining permissions.
* Representing directions or categories.
* Creating a fixed set of named values.

Example without an enum:

```typescript
const pending = 0;
const approved = 1;
const rejected = 2;

function updateStatus(status: number): void {
  console.log(status);
}

updateStatus(approved);
```

The numbers do not communicate their meaning clearly.

Using an enum:

```typescript
enum Status {
  Pending,
  Approved,
  Rejected,
}

function updateStatus(status: Status): void {
  console.log(status);
}

updateStatus(Status.Approved);
```

The code becomes easier to read and maintain.

# 🗁 Basic Syntax

Use the `enum` keyword to declare an enumeration.

```typescript
enum EnumName {
  MemberOne,
  MemberTwo,
  MemberThree,
}
```

Example:

```typescript
enum Direction {
  North,
  South,
  East,
  West,
}
```

Access enum members using dot notation:

```typescript
console.log(Direction.North);
console.log(Direction.South);
console.log(Direction.East);
console.log(Direction.West);
```

By default, numeric enums start at `0`.

Output:

```text
0
1
2
3
```

# 🗁 Creating an Enum

Define an enum for application user roles.

```typescript
enum UserRole {
  Admin,
  Editor,
  Author,
  Subscriber,
}
```

Use its members:

```typescript
const role: UserRole = UserRole.Admin;

console.log(role);
```

Output:

```text
0
```

Use enums in functions:

```typescript
function checkAccess(role: UserRole): void {
  if (role === UserRole.Admin) {
    console.log("Full access granted");
  } else {
    console.log("Limited access granted");
  }
}

checkAccess(UserRole.Admin);
```

# 🗁 Enum Members

Each enum member represents a named value.

```typescript
enum Status {
  Pending,
  Processing,
  Shipped,
  Delivered,
}
```

Access individual members:

```typescript
Status.Pending;
Status.Processing;
Status.Shipped;
Status.Delivered;
```

Enum member names are identifiers and follow TypeScript naming conventions.

Common naming styles include:

* PascalCase: `Pending`, `InProgress`
* UPPER_SNAKE_CASE: `PENDING`, `IN_PROGRESS`

Choose one naming convention and use it consistently.

# 🗁 Enum with Variables

Enum members can be assigned to variables.

```typescript
enum OrderStatus {
  Pending,
  Processing,
  Completed,
}

const currentStatus: OrderStatus = OrderStatus.Processing;

console.log(currentStatus);
```

You can compare enum values directly:

```typescript
if (currentStatus === OrderStatus.Processing) {
  console.log("Your order is being processed");
}
```

# 🗁 Enum with Functions

Enums can be used as function parameter and return types.

```typescript
enum TrafficLight {
  Red,
  Yellow,
  Green,
}

function getInstruction(light: TrafficLight): string {
  switch (light) {
    case TrafficLight.Red:
      return "Stop";

    case TrafficLight.Yellow:
      return "Prepare to stop";

    case TrafficLight.Green:
      return "Go";
  }
}
```

Usage:

```typescript
console.log(getInstruction(TrafficLight.Red));
console.log(getInstruction(TrafficLight.Green));
```

# 🗁 Enum with Objects

Enum values can be stored in object properties.

```typescript
enum UserRole {
  Admin,
  Editor,
  Viewer,
}

type User = {
  id: number;
  name: string;
  role: UserRole;
};

const user: User = {
  id: 1,
  name: "Alice",
  role: UserRole.Admin,
};
```

Access the role:

```typescript
console.log(user.role === UserRole.Admin);
```

This makes the role property type-safe at compile time.

# 🗁 Enum with Arrays

You can create arrays containing enum members.

```typescript
enum Permission {
  Read,
  Write,
  Update,
  Delete,
}

const permissions: Permission[] = [
  Permission.Read,
  Permission.Write,
  Permission.Update,
];
```

Iterate over the array:

```typescript
for (const permission of permissions) {
  console.log(permission);
}
```

# 🗁 Enum with Type Aliases

Enums can be combined with type aliases to create more expressive application models.

```typescript
enum PaymentMethod {
  Cash,
  Card,
  BankTransfer,
}

type Payment = {
  amount: number;
  method: PaymentMethod;
};
```

Create payment records:

```typescript
const payment: Payment = {
  amount: 1500,
  method: PaymentMethod.BankTransfer,
};
```

# 🗁 Enum with Interfaces

Interfaces can use enums as property types.

```typescript
enum AccountStatus {
  Active,
  Suspended,
  Closed,
}

interface Account {
  id: number;
  username: string;
  status: AccountStatus;
}
```

Create an account:

```typescript
const account: Account = {
  id: 101,
  username: "alice",
  status: AccountStatus.Active,
};
```

# 🗁 Enum with Classes

Classes can use enums for properties, constructor parameters, and methods.

```typescript
enum EmployeeRole {
  Manager,
  Developer,
  Designer,
}

class Employee {
  constructor(
    public name: string,
    public role: EmployeeRole,
  ) {}

  displayRole(): void {
    console.log(`${this.name} is a ${EmployeeRole[this.role]}`);
  }
}
```

Create an instance:

```typescript
const employee = new Employee(
  "Alice",
  EmployeeRole.Developer,
);

employee.displayRole();
```

Output:

```text
Alice is a Developer
```

# 🗁 Enum as a Runtime Object

Unlike interfaces and type aliases, regular enums generate JavaScript runtime code.

```typescript
enum Direction {
  North,
  South,
  East,
  West,
}
```

The compiled JavaScript contains an object representing the enum and its members.

This allows runtime access:

```typescript
console.log(Direction.North);
console.log(Direction[0]);
```

Output:

```text
0
North
```

Numeric enums support reverse mapping, allowing you to access a member name from its numeric value.

String enums do not provide this automatic reverse mapping.

# 🗁 Iterating Over Enums

You can iterate over enum values at runtime.

Numeric enums contain both forward and reverse mappings, so filtering may be necessary.

```typescript
enum Direction {
  North,
  South,
  East,
  West,
}

const directions = Object.values(Direction).filter(
  (value): value is Direction =>
    typeof value === "number",
);

console.log(directions);
```

Output:

```text
[0, 1, 2, 3]
```

To retrieve member names:

```typescript
const directionNames = Object.keys(Direction).filter(
  (key) => Number.isNaN(Number(key)),
);

console.log(directionNames);
```

Output:

```text
["North", "South", "East", "West"]
```

# 🗁 Enum with Computed Members

Enum members can use constant expressions and certain runtime expressions.

```typescript
enum FileSize {
  Small = 10,
  Medium = Small * 2,
  Large = Medium * 2,
}
```

Values:

```typescript
console.log(FileSize.Small);  // 10
console.log(FileSize.Medium); // 20
console.log(FileSize.Large);  // 40
```

Computed enum members can also use expressions:

```typescript
enum Configuration {
  Timeout = 1000 * 30,
  MaxRetries = 3,
}
```

Some computed enum members are evaluated at runtime rather than being known at compile time.

# 🗁 Numeric Enums

Numeric enums are the default enum type in TypeScript.

```typescript
enum Status {
  Pending,
  Approved,
  Rejected,
}
```

Their default values are:

| Member | Value |
| --- | ---: |
| Pending | 0 |
| Approved | 1 |
| Rejected | 2 |

You can explicitly assign numeric values:

```typescript
enum Status {
  Pending = 10,
  Approved = 20,
  Rejected = 30,
}
```

Numeric enums support automatic reverse mapping.

```typescript
console.log(Status.Approved); // 20
console.log(Status[20]); // Approved
```

For more details, see `02-numeric-enums.md`.

# 🗁 String Enums

String enums assign string values to members.

```typescript
enum Status {
  Pending = "PENDING",
  Approved = "APPROVED",
  Rejected = "REJECTED",
}
```

Usage:

```typescript
const status: Status = Status.Approved;

console.log(status);
```

Output:

```text
APPROVED
```

String enums are useful when values need to be readable in logs, network requests, or stored data.

For more details, see `03-string-enums.md`.

# 🗁 Const Enums

Const enums are declared using the `const enum` syntax.

```typescript
const enum Direction {
  North,
  South,
  East,
  West,
}
```

Their members are generally inlined into generated JavaScript instead of creating a regular runtime enum object.

Example:

```typescript
const direction = Direction.North;
```

The compiled output may look like:

```javascript
const direction = 0 /* Direction.North */;
```

Const enums have additional restrictions involving compilation and module publishing.

For more details, see `04-const-enums.md`.

# 🗁 Enum vs Union Types

Enums and literal union types can both represent a fixed set of permitted values.

Using an enum:

```typescript
enum Status {
  Pending = "pending",
  Approved = "approved",
  Rejected = "rejected",
}

function updateStatus(status: Status): void {
  console.log(status);
}
```

Using a union type:

```typescript
type Status = "pending" | "approved" | "rejected";

function updateStatus(status: Status): void {
  console.log(status);
}
```

Comparison:

| Feature | Enum | Union Type |
| --- | --- | --- |
| Named members | Yes | Uses literal values |
| Runtime representation | Regular enums generate an object | No runtime representation |
| Reverse mapping | Numeric enums support it | No |
| String values | Supported | Supported |
| Type safety | Yes | Yes |
| JavaScript output | Depends on enum kind and compilation | Type declaration is erased |

Use enums when a named runtime representation is useful.

Use literal union types when you only need a set of permitted values without generating an enum object.

# 🗁 Common Mistakes

**Using numeric values without understanding their meaning**

```typescript
enum Status {
  Pending,
  Approved,
  Rejected,
}

const status = 1;
```

The value `1` is not self-explanatory.

Prefer:

```typescript
const status = Status.Approved;
```

**Assuming every enum starts at zero**

Numeric enums start at zero by default, but explicit values can change their numbering.

```typescript
enum Status {
  Pending = 10,
  Approved,
  Rejected,
}
```

Here, the values are `10`, `11`, and `12`.

**Mixing unrelated enum values**

Avoid using one enum to represent unrelated concepts.

Create separate enums for distinct domains.

**Expecting string enums to support reverse mapping**

```typescript
enum Status {
  Pending = "PENDING",
}

// Status["PENDING"] is not a generated reverse mapping.
```

String enums provide forward mappings only.

**Assuming enums are erased like interfaces**

Regular enums generate runtime JavaScript. Interfaces and type aliases do not.

Understand the runtime implications before choosing an enum.

# 🗁 Best Practices

* Use descriptive enum and member names.
* Keep each enum focused on one domain concept.
* Prefer explicit values when the numeric or string representation is part of a stable external contract.
* Use string enums when readable serialized values are useful.
* Consider literal union types when a runtime enum object is unnecessary.
* Avoid relying on implicit numeric values for persistent database or API contracts.
* Understand numeric enum reverse mapping before iterating over enum objects.
* Use `const enum` only when its compilation and publishing constraints are understood.
* Keep naming conventions consistent throughout the project.

# 🗁 Summary

* Enums represent a collection of named constants.
* They are declared using the `enum` keyword.
* Numeric enums assign numeric values to members by default.
* String enums assign explicit string values.
* Regular enums generate JavaScript runtime objects.
* Numeric enums support reverse mapping.
* Const enums are generally inlined during compilation.
* Enums can be used with variables, functions, arrays, objects, interfaces, and classes.
* Literal union types are an alternative when a runtime enum object is unnecessary.

# 🗁 Related Topics

* Numeric Enums
* String Enums
* Const Enums
* Literal Types
* Template Literal Types
* As Const
* Union Types
* Type Aliases
* Type Narrowing
* Discriminated Unions
