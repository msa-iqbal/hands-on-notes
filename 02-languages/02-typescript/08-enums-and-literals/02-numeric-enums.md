# Numeric Enums

Numeric enums are TypeScript enums whose members are associated with numeric values.

They provide meaningful names for numeric constants and are useful when representing a fixed set of related numeric values.

Numeric enums are commonly used for:

* Application statuses.
* User roles.
* Permission levels.
* Directions.
* Priority levels.
* Error codes.
* Workflow states.
* Numeric configuration values.

Example:

```typescript
enum Status {
  Pending,
  Approved,
  Rejected,
}

console.log(Status.Pending);  // 0
console.log(Status.Approved); // 1
console.log(Status.Rejected); // 2
```

TypeScript automatically assigns numeric values when explicit values are not provided.

# 🗁 Basic Syntax

Use the `enum` keyword to define a numeric enum.

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

By default, the first member receives `0`, and each subsequent member is incremented by `1`.

# 🗁 Default Numeric Values

When no values are explicitly assigned, TypeScript starts numbering from zero.

```typescript
enum Status {
  Pending,
  Processing,
  Shipped,
  Delivered,
}
```

The resulting values are:

| Member | Numeric Value |
| --- | ---: |
| Pending | 0 |
| Processing | 1 |
| Shipped | 2 |
| Delivered | 3 |

Access the values:

```typescript
console.log(Status.Pending);    // 0
console.log(Status.Processing); // 1
console.log(Status.Shipped);    // 2
console.log(Status.Delivered);  // 3
```

# 🗁 Explicit Numeric Values

You can assign specific numeric values to enum members.

```typescript
enum Status {
  Pending = 10,
  Processing = 20,
  Shipped = 30,
  Delivered = 40,
}
```

Usage:

```typescript
console.log(Status.Pending);    // 10
console.log(Status.Processing); // 20
console.log(Status.Shipped);    // 30
console.log(Status.Delivered);  // 40
```

Explicit values are useful when numeric identifiers must match an existing system or external contract.

# 🗁 Automatic Incrementing After Explicit Values

When an enum member has an explicit numeric value, subsequent members automatically increment from that value.

```typescript
enum Status {
  Pending = 10,
  Processing,
  Shipped,
  Delivered,
}
```

Values:

| Member | Value |
| --- | ---: |
| Pending | 10 |
| Processing | 11 |
| Shipped | 12 |
| Delivered | 13 |

The next member receives the previous numeric value plus one.

# 🗁 Mixing Explicit and Implicit Values

You can assign explicit values to selected members while allowing other members to be automatically incremented.

```typescript
enum Priority {
  Low = 1,
  Normal,
  High = 10,
  Critical,
}
```

Result:

| Member | Value |
| --- | ---: |
| Low | 1 |
| Normal | 2 |
| High | 10 |
| Critical | 11 |

Each implicit member continues from the immediately preceding numeric member.

# 🗁 Accessing Enum Members

Use dot notation to access numeric enum members.

```typescript
enum Direction {
  North,
  South,
  East,
  West,
}

const direction = Direction.North;

console.log(direction);
```

Output:

```text
0
```

You can also compare enum members:

```typescript
if (direction === Direction.North) {
  console.log("Moving north");
}
```

# 🗁 Reverse Mapping

Numeric enums support reverse mapping.

Reverse mapping means you can retrieve an enum member's name using its numeric value.

```typescript
enum Status {
  Pending = 0,
  Approved = 1,
  Rejected = 2,
}
```

Forward mapping:

```typescript
console.log(Status.Approved); // 1
```

Reverse mapping:

```typescript
console.log(Status[1]); // Approved
console.log(Status[2]); // Rejected
```

This behavior is generated for regular numeric enums.

# 🗁 Understanding Reverse Mapping Internally

A numeric enum generates a JavaScript object containing both forward and reverse mappings.

Conceptually, the compiled JavaScript looks like:

```javascript
var Status;

(function (Status) {
  Status[Status["Pending"] = 0] = "Pending";
  Status[Status["Approved"] = 1] = "Approved";
  Status[Status["Rejected"] = 2] = "Rejected";
})(Status || (Status = {}));
```

The resulting object behaves approximately like:

```javascript
{
  0: "Pending",
  1: "Approved",
  2: "Rejected",
  Pending: 0,
  Approved: 1,
  Rejected: 2
}
```

This is why both forward and reverse access work.

# 🗁 Iterating Over Numeric Enums

Because numeric enums contain reverse mappings, `Object.keys()` and `Object.values()` return both member names and numeric values.

Example:

```typescript
enum Direction {
  North,
  South,
  East,
  West,
}

console.log(Object.keys(Direction));
```

Output:

```text
["0", "1", "2", "3", "North", "South", "East", "West"]
```

To retrieve only member names:

```typescript
const names = Object.keys(Direction).filter(
  (key) => Number.isNaN(Number(key)),
);

console.log(names);
```

Output:

```text
["North", "South", "East", "West"]
```

To retrieve only numeric values:

```typescript
const values = Object.values(Direction).filter(
  (value): value is number => typeof value === "number",
);

console.log(values);
```

Output:

```text
[0, 1, 2, 3]
```

# 🗁 Numeric Enums with Variables

Numeric enum members can be assigned to variables with the enum type.

```typescript
enum UserRole {
  Admin,
  Editor,
  Viewer,
}

const role: UserRole = UserRole.Admin;

console.log(role);
```

The variable can hold values belonging to the enum's numeric domain.

# 🗁 Numeric Enums with Functions

Use numeric enums as function parameter and return types.

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

    default:
      return "Unknown signal";
  }
}
```

Usage:

```typescript
console.log(getInstruction(TrafficLight.Red));
console.log(getInstruction(TrafficLight.Green));
```

You can also return an enum member from a function:

```typescript
function getDefaultRole(): UserRole {
  return UserRole.Viewer;
}
```

# 🗁 Numeric Enums with Objects

Numeric enums can be used as object property types.

```typescript
enum AccountStatus {
  Active,
  Suspended,
  Closed,
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

# 🗁 Numeric Enums with Arrays

You can create arrays containing numeric enum members.

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

Iterate over the permissions:

```typescript
for (const permission of permissions) {
  console.log(permission);
}
```

# 🗁 Numeric Enums with Interfaces

Interfaces can use numeric enums as property types.

```typescript
enum OrderStatus {
  Pending,
  Processing,
  Completed,
  Cancelled,
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

# 🗁 Numeric Enums with Classes

Classes can use numeric enums for properties, constructor parameters, and methods.

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
    console.log(`${this.name}: ${EmployeeRole[this.role]}`);
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
Alice: Developer
```

# 🗁 Numeric Enums with Bit Flags

Numeric enums can represent bit flags.

Bit flags allow multiple independent options to be combined into one numeric value using bitwise operators.

Assign each member a distinct power of two.

```typescript
enum Permission {
  None = 0,
  Read = 1 << 0,
  Write = 1 << 1,
  Update = 1 << 2,
  Delete = 1 << 3,
}
```

The values are:

| Member | Binary | Decimal |
| --- | --- | ---: |
| None | 0000 | 0 |
| Read | 0001 | 1 |
| Write | 0010 | 2 |
| Update | 0100 | 4 |
| Delete | 1000 | 8 |

Combine permissions using bitwise OR:

```typescript
const userPermissions =
  Permission.Read |
  Permission.Write;

console.log(userPermissions); // 3
```

Check whether a permission exists using bitwise AND:

```typescript
const canRead =
  (userPermissions & Permission.Read) === Permission.Read;

const canDelete =
  (userPermissions & Permission.Delete) === Permission.Delete;

console.log(canRead);   // true
console.log(canDelete); // false
```

Add another permission:

```typescript
const updatedPermissions =
  userPermissions | Permission.Update;
```

Remove a permission:

```typescript
const permissionsWithoutWrite =
  updatedPermissions & ~Permission.Write;
```

Bit flags are useful for compact permission systems and configuration options.

# 🗁 Numeric Enums with Computed Values

Enum members can use constant expressions.

```typescript
enum Configuration {
  Timeout = 1000 * 30,
  MaxRetries = 3,
  RetryDelay = 1000 * 5,
}
```

Usage:

```typescript
console.log(Configuration.Timeout);    // 30000
console.log(Configuration.MaxRetries); // 3
console.log(Configuration.RetryDelay); // 5000
```

Numeric enum members can also use references to earlier enum members:

```typescript
enum FileSize {
  Small = 10,
  Medium = Small * 2,
  Large = Medium * 2,
}
```

# 🗁 Numeric Enums and Type Safety

Numeric enums provide type checking, but their compatibility rules have some important details.

```typescript
enum Status {
  Pending,
  Approved,
  Rejected,
}

let currentStatus: Status = Status.Pending;

currentStatus = Status.Approved;
```

A plain number is not generally assignable to a numeric enum.

```typescript
let status: Status;

// TypeScript error
// status = 10;
```

However, numeric enums have a special compatibility rule: a value of type `number` can be assigned to a numeric enum in some cases.

For example:

```typescript
enum Status {
  Pending,
  Approved,
}

let status: Status = 10;
```

This is permitted by TypeScript's numeric enum compatibility rules, even though `10` is not a declared member.

Therefore, numeric enums do not guarantee that every runtime value belongs to one of their declared members.

Validate untrusted numeric values when necessary.

# 🗁 Numeric Enums and Runtime Validation

Values received from APIs, databases, or external systems should be validated before being treated as valid enum members.

Example:

```typescript
enum Status {
  Pending = 0,
  Approved = 1,
  Rejected = 2,
}

function isStatus(value: number): value is Status {
  return (
    value === Status.Pending ||
    value === Status.Approved ||
    value === Status.Rejected
  );
}
```

Usage:

```typescript
const apiValue: number = 1;

if (isStatus(apiValue)) {
  const status: Status = apiValue;
  console.log("Valid status:", status);
} else {
  console.log("Invalid status");
}
```

This validates the value against the declared members instead of assuming that every number is valid.

# 🗁 Numeric Enums with Exhaustiveness Checking

Numeric enums can be used with exhaustive switch statements.

```typescript
enum Status {
  Pending,
  Approved,
  Rejected,
}

function assertNever(value: never): never {
  throw new Error(`Unexpected value: ${value}`);
}

function getStatusMessage(status: Status): string {
  switch (status) {
    case Status.Pending:
      return "Waiting for approval";

    case Status.Approved:
      return "Request approved";

    case Status.Rejected:
      return "Request rejected";

    default:
      return assertNever(status);
  }
}
```

If a new enum member is added, TypeScript can identify the missing case.

# 🗁 Numeric Enums vs String Enums

Numeric enums associate members with numbers.

```typescript
enum NumericStatus {
  Pending,
  Approved,
  Rejected,
}
```

String enums associate members with strings.

```typescript
enum StringStatus {
  Pending = "pending",
  Approved = "approved",
  Rejected = "rejected",
}
```

Comparison:

| Feature | Numeric Enum | String Enum |
| --- | --- | --- |
| Member values | Numbers | Strings |
| Default values | Automatically assigned | Must be assigned |
| Reverse mapping | Supported | Not generated |
| Readable serialized values | Less descriptive | More descriptive |
| Bitwise operations | Suitable | Not suitable |
| External numeric contracts | Suitable when required | Not suitable |

Choose the enum type based on how the values will be used.

# 🗁 Common Mistakes

**Assuming numbering always starts at zero**

```typescript
enum Status {
  Pending = 10,
  Approved,
  Rejected,
}
```

The values are `10`, `11`, and `12`, not `0`, `1`, and `2`.

**Forgetting automatic incrementing**

An implicit member receives the previous numeric value plus one.

Be careful when mixing explicit and implicit values.

**Using duplicate numeric values unintentionally**

```typescript
enum Status {
  Pending = 1,
  Approved = 1,
}
```

Both members have the same numeric value.

Reverse mapping becomes ambiguous because the same numeric key cannot represent both names independently.

Avoid duplicate values unless they are intentional.

**Assuming numeric enums reject all undeclared numbers**

Numeric enum compatibility allows some undeclared numeric values.

Use runtime validation when receiving numbers from external sources.

**Iterating without filtering reverse mappings**

`Object.keys()` and `Object.values()` include both forward and reverse mappings.

Filter the results when you need only names or values.

**Using numeric enums for readable API values**

If an API expects values such as `"pending"` or `"approved"`, a string enum or literal union may be a better representation.

# 🗁 Best Practices

* Use descriptive enum and member names.
* Assign explicit numeric values when integrating with external numeric contracts.
* Avoid accidental duplicate values.
* Understand automatic incrementing.
* Use powers of two for bit-flag permissions.
* Filter reverse mappings when iterating over numeric enums.
* Validate numeric values received from external sources.
* Use exhaustive checking when handling all enum members.
* Prefer string enums when readable serialized values are important.
* Avoid relying on implicit numeric values for long-lived database or API contracts.

# 🗁 Summary

* Numeric enums associate named members with numeric values.
* They start at `0` by default.
* Explicit values can be assigned to individual members.
* Subsequent members automatically increment from the previous numeric value.
* Numeric enums support reverse mapping.
* They generate runtime JavaScript objects when used as regular enums.
* They can be used with functions, objects, arrays, interfaces, and classes.
* Bit flags allow multiple permissions or options to be combined using bitwise operators.
* Numeric enum types have special compatibility rules that allow some undeclared numeric values.
* Runtime validation is important when handling untrusted numbers.

# 🗁 Related Topics

* Enums
* String Enums
* Const Enums
* Literal Types
* Template Literal Types
* As Const
* Union Types
* Bitwise Operators
* Type Narrowing
* Exhaustiveness Checking
* Runtime Validation
