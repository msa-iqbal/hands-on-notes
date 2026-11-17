# Union Types

Union types are a TypeScript feature that allows a value to have more than one possible type.

A union type is created using the pipe (`|`) operator.

It tells TypeScript that a value can be one of several specified types.

Union types are useful when:

* A variable can store different kinds of values.
* A function accepts multiple input types.
* API responses have different possible structures.
* Application states can have multiple representations.
* A property supports several valid values.
* You want flexibility without using `any`.

# 🗁 Basic Syntax

Use the pipe (`|`) operator to combine multiple types.

```typescript
type UnionType = TypeA | TypeB;
```

Example:

```typescript
let value: string | number;

value = "Hello";
value = 100;

// Error:
value = true;
```

The variable can contain either a string or a number, but not a boolean.

# 🗁 Union Types with Basic Types

You can combine different primitive types.

String or number:

```typescript
let id: string | number;

id = "USR-101";
id = 101;
```

String or boolean:

```typescript
let status: string | boolean;

status = "active";
status = true;
```

Number or boolean:

```typescript
let result: number | boolean;

result = 200;
result = false;
```

A union accepts values that match at least one member of the union.

# 🗁 Union Types with Multiple Types

A union can contain more than two types.

```typescript
let data: string | number | boolean;

data = "Hello";
data = 100;
data = true;
```

You can also define reusable union types.

```typescript
type Identifier = string | number;

let userId: Identifier;

userId = "USR-101";
userId = 101;
```

Using type aliases makes complex union types easier to reuse and maintain.

# 🗁 Union Types with Arrays

You can use union types with arrays in different ways.

An array containing strings or numbers:

```typescript
let values: (string | number)[];

values = [10, 20, 30];
values = ["Alice", "Bob"];
values = [10, "Alice", 20, "Bob"];
```

The parentheses are important:

```typescript
let values: (string | number)[];
```

This means each array element can be a string or a number.

Without parentheses:

```typescript
let values: string[] | number[];
```

This means the entire array must contain either strings or numbers.

Example:

```typescript
let first: string[] | number[];

first = ["Alice", "Bob"];
first = [10, 20];

// Error:
first = ["Alice", 20];
```

# 🗁 Union Types with Object Types

Union types can represent objects with different structures.

```typescript
interface Admin {
  id: number;
  name: string;
  permissions: string[];
}

interface Customer {
  id: number;
  name: string;
  shippingAddress: string;
}

type User = Admin | Customer;
```

A value of type `User` can represent either an administrator or a customer.

```typescript
const admin: User = {
  id: 1,
  name: "Alice",
  permissions: ["create", "delete"],
};

const customer: User = {
  id: 2,
  name: "Bob",
  shippingAddress: "Dhaka, Bangladesh",
};
```

However, TypeScript does not automatically allow access to properties that exist only on one member of the union.

# 🗁 Accessing Union Type Properties

When a value has a union type, TypeScript only allows direct access to members that are available on every possible union member.

```typescript
interface Admin {
  id: number;
  name: string;
  permissions: string[];
}

interface Customer {
  id: number;
  name: string;
  shippingAddress: string;
}

type User = Admin | Customer;

function displayUser(user: User): void {
  console.log(user.id);
  console.log(user.name);

  // Error:
  // console.log(user.permissions);
}
```

Both interfaces contain `id` and `name`, so those properties are safe to access.

The `permissions` property is not available on every possible member.

Use type narrowing to access member-specific properties safely.

# 🗁 Union Types with Literal Types

Union types can combine literal values.

```typescript
type Status = "pending" | "approved" | "rejected";

let orderStatus: Status;

orderStatus = "pending";
orderStatus = "approved";
orderStatus = "rejected";

// Error:
orderStatus = "completed";
```

This restricts a variable to a specific set of permitted values.

Literal unions are useful for:

* Application statuses.
* User roles.
* Environment names.
* Payment states.
* Configuration options.
* Navigation routes.

# 🗁 Union Types with Null and Undefined

Union types can represent values that may be `null` or `undefined`.

```typescript
let username: string | null;

username = "Alice";
username = null;
```

A value can also be optional:

```typescript
let email: string | undefined;

email = "alice@example.com";
email = undefined;
```

You can combine all three:

```typescript
let value: string | null | undefined;

value = "Hello";
value = null;
value = undefined;
```

This is useful when modeling missing or unavailable values.

# 🗁 Union Types in Function Parameters

Functions can accept arguments with multiple possible types.

```typescript
function printId(id: string | number): void {
  console.log(id);
}

printId(101);
printId("USR-101");
```

Both arguments are valid.

You can also define reusable parameter types.

```typescript
type Identifier = string | number;

function getUser(id: Identifier): void {
  console.log(`User ID: ${id}`);
}
```

# 🗁 Union Types in Function Return Values

A function can return one of several types.

```typescript
function getValue(): string | number {
  if (Math.random() > 0.5) {
    return "Success";
  }

  return 100;
}
```

The return value must match at least one member of the declared union.

```typescript
const result = getValue();

console.log(result);
```

The inferred type of `result` is `string | number`.

# 🗁 Union Types with Type Aliases

Type aliases are useful for defining reusable union types.

```typescript
type ID = string | number;

type Status = "active" | "inactive";

type ResponseValue = string | number | boolean;
```

Use these aliases in multiple declarations:

```typescript
interface User {
  id: ID;
  status: Status;
  metadata: ResponseValue;
}
```

This improves consistency across application models.

# 🗁 Union Types with Interfaces

Interfaces can be used as members of union types.

```typescript
interface EmailNotification {
  type: "email";
  recipient: string;
  subject: string;
}

interface SmsNotification {
  type: "sms";
  phoneNumber: string;
  message: string;
}

type Notification = EmailNotification | SmsNotification;
```

Create notification objects:

```typescript
const emailNotification: Notification = {
  type: "email",
  recipient: "alice@example.com",
  subject: "Welcome",
};

const smsNotification: Notification = {
  type: "sms",
  phoneNumber: "1234567890",
  message: "Your verification code is 123456",
};
```

This structure is particularly useful when different objects share a common purpose but have different properties.

# 🗁 Union Types vs Any

Union types and `any` both allow different kinds of values, but their type-safety behavior is very different.

Using `any`:

```typescript
let value: any;

value = "Hello";
value = 100;
value = true;

value.toUpperCase();
```

TypeScript allows the method call even when the current value might be a number or boolean.

Using a union:

```typescript
let value: string | number;

value = "Hello";
value = 100;

// Error:
value.toUpperCase();
```

TypeScript requires you to narrow the type before using members that are not shared.

| Feature | Union Types | Any |
| --- | --- | --- |
| Supports multiple types | Yes | Yes |
| Type checking | Preserved | Mostly disabled |
| Requires narrowing | When needed | No |
| Detects incompatible operations | Yes | Usually no |
| Recommended for known alternatives | Yes | No |

Prefer union types when you know the possible types of a value.

# 🗁 Union Types vs Intersection Types

Union and intersection types combine types differently.

**Union type (`|`)**

A value can satisfy one or more members of the union.

```typescript
type Value = string | number;

let value: Value;

value = "Hello";
value = 100;
```

**Intersection type (`&`)**

A value must satisfy all combined types.

```typescript
type Person = {
  name: string;
};

type Employee = {
  employeeId: number;
};

type EmployeePerson = Person & Employee;
```

Example:

```typescript
const employee: EmployeePerson = {
  name: "Alice",
  employeeId: 1001,
};
```

| Feature | Union (`\|`) | Intersection (`&`) |
| --- | --- | --- |
| Meaning | One of several types | All combined types |
| Main purpose | Alternative structures | Combined structures |
| Narrowing | Commonly required | Usually not required for combined members |
| Example | `string \| number` | `Person & Employee` |

# 🗁 Type Narrowing with Union Types

TypeScript uses type narrowing to determine which member of a union is currently available.

For example:

```typescript
function printValue(value: string | number): void {
  if (typeof value === "string") {
    console.log(value.toUpperCase());
  } else {
    console.log(value.toFixed(2));
  }
}
```

Inside the first branch, TypeScript knows that `value` is a string.

Inside the second branch, TypeScript knows that `value` is a number.

Type narrowing is covered in detail in the next topics of this section.

# 🗁 Practical Example: API Response

An API may return either a successful response or an error response.

```typescript
interface SuccessResponse {
  success: true;
  data: {
    id: number;
    name: string;
  };
}

interface ErrorResponse {
  success: false;
  error: string;
}

type ApiResponse = SuccessResponse | ErrorResponse;
```

Create a function to handle both responses:

```typescript
function handleResponse(response: ApiResponse): void {
  if (response.success) {
    console.log("User:", response.data.name);
  } else {
    console.log("Error:", response.error);
  }
}
```

Successful response:

```typescript
handleResponse({
  success: true,
  data: {
    id: 1,
    name: "Alice",
  },
});
```

Error response:

```typescript
handleResponse({
  success: false,
  error: "User not found",
});
```

The `success` property allows TypeScript to distinguish between the two response structures.

# 🗁 Practical Example: Payment Method

A payment system may support multiple payment methods.

```typescript
interface CardPayment {
  type: "card";
  cardNumber: string;
  expiryDate: string;
}

interface MobilePayment {
  type: "mobile";
  phoneNumber: string;
  provider: string;
}

interface BankTransfer {
  type: "bank";
  accountNumber: string;
  bankName: string;
}

type PaymentMethod =
  | CardPayment
  | MobilePayment
  | BankTransfer;
```

Create a payment processor:

```typescript
function processPayment(payment: PaymentMethod): void {
  switch (payment.type) {
    case "card":
      console.log(`Processing card: ${payment.cardNumber}`);
      break;

    case "mobile":
      console.log(`Processing mobile payment: ${payment.phoneNumber}`);
      break;

    case "bank":
      console.log(`Processing bank transfer: ${payment.bankName}`);
      break;
  }
}
```

Usage:

```typescript
processPayment({
  type: "card",
  cardNumber: "1234567890123456",
  expiryDate: "12/2028",
});

processPayment({
  type: "mobile",
  phoneNumber: "1234567890",
  provider: "Mobile Banking",
});

processPayment({
  type: "bank",
  accountNumber: "987654321",
  bankName: "Example Bank",
});
```

This approach provides a clear and type-safe model for multiple payment structures.

# 🗁 Common Mistakes

**Forgetting parentheses in array union types**

```typescript
// Array elements can only be strings,
// or the entire value can be a number array.
let values: string[] | number[];
```

If individual elements should support both types, use:

```typescript
let values: (string | number)[];
```

**Accessing properties without narrowing**

```typescript
interface Admin {
  permissions: string[];
}

interface Customer {
  shippingAddress: string;
}

type User = Admin | Customer;

function displayUser(user: User): void {
  // Error:
  // console.log(user.permissions);
}
```

Check the type before accessing member-specific properties.

**Using overly broad union types**

```typescript
type Value = string | number | boolean | object | null | undefined;
```

A very broad union can make code difficult to understand and maintain.

Prefer a union containing only the types that are actually expected.

**Confusing union and intersection**

```typescript
type User = {
  name: string;
} | {
  age: number;
};
```

This does not require both `name` and `age`.

Use an intersection if both properties are required:

```typescript
type User = {
  name: string;
} & {
  age: number;
};
```

**Using any instead of a known union**

```typescript
let value: any;
```

If the possible types are known, define them explicitly:

```typescript
let value: string | number;
```

# 🗁 Best Practices

* Use union types when a value has a known set of possible types.
* Prefer specific unions over `any`.
* Use type aliases for reusable union definitions.
* Use literal unions for fixed sets of permitted values.
* Use discriminant properties to distinguish object variants.
* Narrow union types before accessing member-specific properties.
* Use `null` and `undefined` intentionally to represent missing values.
* Keep union members focused on meaningful alternatives.
* Use exhaustive checking when every union member must be handled.
* Prefer discriminated unions for complex application states and API responses.

# 🗁 Summary

* Union types combine multiple possible types using the `|` operator.
* A value can match any member of a union.
* Union types support primitive types, arrays, objects, interfaces, and literal types.
* Union types can be used in variables, parameters, and return values.
* Access to member-specific properties requires type narrowing.
* Union types are more type-safe than `any`.
* Union types represent alternatives, while intersection types combine requirements.
* Discriminated unions help model different object structures.
* Union types are useful for API responses, application states, and flexible function inputs.

# 🗁 Related Topics

* Intersection Types
* Type Narrowing
* Type Guards
* Type Predicates
* Discriminated Unions
* In Operator
* Instanceof
* Exhaustiveness Checking
* Literal Types
* Type Aliases
* Object Types
* Function Parameters
