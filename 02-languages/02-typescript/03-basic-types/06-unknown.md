# TypeScript Unknown Type

The `unknown` type is a special TypeScript type that represents a value whose actual type is not currently known.

It can hold values of any type, just like `any`. However, unlike `any`, TypeScript does not allow you to perform arbitrary operations on an `unknown` value without first checking or narrowing its type.

The `unknown` type is particularly useful when working with external data, API responses, JSON parsing, user input, and other dynamically typed values.

# 🗁 Understanding Unknown

A variable declared as `unknown` can store values of different types.

```typescript
let value: unknown;

value = "Hello TypeScript";
value = 100;
value = true;
value = null;
value = undefined;
value = { name: "John" };
value = [10, 20, 30];
```

All these assignments are valid because every value is assignable to `unknown`.

# 🗁 Declaring Unknown Variables

You can explicitly declare a variable using the `unknown` type.

```typescript
let username: unknown = "John";

console.log(username);

username = 25;

console.log(username);

username = true;

console.log(username);
```

Unlike `any`, TypeScript prevents you from directly using an unknown value as if it had a specific type.

```typescript
let value: unknown = "Hello";

// TypeScript error:
// value.toUpperCase();
```

You must first establish the actual type of the value.

# 🗁 Unknown and Type Checking

TypeScript requires type checking before performing operations on an `unknown` value.

```typescript
let value: unknown = "Hello";

if (typeof value === "string") {
    console.log(value.toUpperCase());
}
```

Inside the conditional block, TypeScript knows that `value` is a string.

You can also check for numbers:

```typescript
let value: unknown = 100;

if (typeof value === "number") {
    console.log(value.toFixed(2));
}
```

This process is called type narrowing.

# 🗁 Unknown and Any

Both `any` and `unknown` can hold values of any type, but they provide different levels of type safety.

**Using any:**

```typescript
let value: any = "Hello";

value.toUpperCase();
value.toFixed(2);
value.someUnknownMethod();
```

TypeScript allows all these operations because `any` disables type checking.

**Using unknown:**

```typescript
let value: unknown = "Hello";

// TypeScript errors:
// value.toUpperCase();
// value.toFixed(2);
// value.someUnknownMethod();
```

You must check the value before using it.

```typescript
let value: unknown = "Hello";

if (typeof value === "string") {
    console.log(value.toUpperCase());
}
```

Comparison:

| Feature | `any` | `unknown` |
| --- | --- | --- |
| Accepts any value | Yes | Yes |
| Requires type checking before operations | No | Yes |
| Allows arbitrary property access | Yes | No |
| Allows arbitrary function calls | Yes | No |
| Assignable to specific types without narrowing | Yes | No |
| Preserves type safety | No | Yes |

Prefer `unknown` whenever the actual type of a value is not known.

# 🗁 Unknown and Type Assignability

A value of any type can be assigned to an `unknown` variable.

```typescript
let value: unknown;

let username: string = "John";
let age: number = 25;
let isActive: boolean = true;

value = username;
value = age;
value = isActive;
```

However, an `unknown` value cannot be assigned directly to a specific type.

```typescript
let value: unknown = "Hello";

// TypeScript error:
// let username: string = value;
```

You must narrow the type or use a type assertion.

```typescript
let value: unknown = "Hello";

if (typeof value === "string") {
    let username: string = value;

    console.log(username.toUpperCase());
}
```

# 🗁 Type Narrowing with Unknown

Type narrowing allows TypeScript to determine the actual type of an unknown value.

Several techniques can be used.

**Using typeof:**

```typescript
function displayValue(value: unknown): void {
    if (typeof value === "string") {
        console.log(value.toUpperCase());
    } else if (typeof value === "number") {
        console.log(value.toFixed(2));
    } else if (typeof value === "boolean") {
        console.log(value ? "Active" : "Inactive");
    }
}

displayValue("hello");
displayValue(100);
displayValue(true);
```

**Using instanceof:**

```typescript
function processDate(value: unknown): void {
    if (value instanceof Date) {
        console.log(value.toISOString());
    }
}

processDate(new Date());
```

**Using equality checks:**

```typescript
function displayValue(value: unknown): void {
    if (value === null) {
        console.log("Value is null");
        return;
    }

    if (value === undefined) {
        console.log("Value is undefined");
        return;
    }

    console.log(value);
}
```

# 🗁 Unknown and Object Validation

An unknown value can represent an object, but TypeScript does not allow direct property access until the object has been checked.

```typescript
let user: unknown = {
    name: "John",
    age: 25
};

// TypeScript error:
// console.log(user.name);
```

You can first check whether the value is a non-null object.

```typescript
let user: unknown = {
    name: "John",
    age: 25
};

if (typeof user === "object" && user !== null) {
    console.log(user);
}
```

To access a specific property, you can use the `in` operator.

```typescript
let user: unknown = {
    name: "John",
    age: 25
};

if (
    typeof user === "object" &&
    user !== null &&
    "name" in user
) {
    console.log(user.name);
}
```

For stronger validation, check the property's type as well.

```typescript
function displayUser(value: unknown): void {
    if (
        typeof value === "object" &&
        value !== null &&
        "name" in value &&
        typeof value.name === "string"
    ) {
        console.log(value.name.toUpperCase());
    }
}

displayUser({
    name: "John",
    age: 25
});
```

# 🗁 Unknown and Type Assertions

A type assertion tells TypeScript to treat a value as a specific type.

```typescript
let value: unknown = "Hello TypeScript";

let message = value as string;

console.log(message.toUpperCase());
```

You can also use angle-bracket assertion syntax in TypeScript files that are not using JSX.

```typescript
let value: unknown = "Hello";

let message = <string>value;

console.log(message);
```

Type assertions do not perform runtime validation or conversion.

This means an incorrect assertion can still cause runtime errors.

```typescript
let value: unknown = 100;

let message = value as string;

// Compiles, but throws a runtime error.
message.toUpperCase();
```

Use assertions only when you have independently established that the value has the asserted type.

# 🗁 Unknown in Function Parameters

The `unknown` type is useful when a function accepts values of different types but must validate them before processing.

```typescript
function processValue(value: unknown): void {
    if (typeof value === "string") {
        console.log(`String: ${value.toUpperCase()}`);
    } else if (typeof value === "number") {
        console.log(`Number: ${value.toFixed(2)}`);
    } else {
        console.log("Unsupported value");
    }
}

processValue("hello");
processValue(100);
processValue(true);
```

This approach makes the function flexible while preserving type safety.

# 🗁 Unknown as a Function Return Type

A function can return `unknown` when its result may have different types.

```typescript
function getValue(): unknown {
    const values = [
        "Hello",
        100,
        true
    ];

    return values[Math.floor(Math.random() * values.length)];
}

const result = getValue();

if (typeof result === "string") {
    console.log(result.toUpperCase());
} else if (typeof result === "number") {
    console.log(result.toFixed(2));
} else if (typeof result === "boolean") {
    console.log(result);
}
```

The caller must narrow the returned value before performing type-specific operations.

# 🗁 Unknown in Catch Blocks

JavaScript allows any value to be thrown, not only instances of `Error`.

For this reason, TypeScript supports treating caught exceptions as `unknown`.

```typescript
try {
    throw new Error("Something went wrong");
} catch (error: unknown) {
    if (error instanceof Error) {
        console.log(error.message);
    } else {
        console.log("An unknown error occurred");
    }
}
```

Enable the `useUnknownInCatchVariables` compiler option to make catch variables default to `unknown`.

```json
{
    "compilerOptions": {
        "useUnknownInCatchVariables": true
    }
}
```

This option is included when `strict` mode is enabled.

# 🗁 Unknown with JSON.parse

The standard TypeScript declaration for `JSON.parse()` returns `any`.

You can assign its result to `unknown` to prevent unsafe operations.

```typescript
const json = '{"name":"John","age":25}';

const user: unknown = JSON.parse(json);
```

Now TypeScript requires validation before accessing the data.

```typescript
if (
    typeof user === "object" &&
    user !== null &&
    "name" in user &&
    typeof user.name === "string"
) {
    console.log(user.name);
}
```

This is safer than directly trusting the result of `JSON.parse()`.

For complex data, create a reusable validation function or use a runtime schema validation library.

# 🗁 Unknown with API Responses

API responses are external data, so their structure should not automatically be trusted.

You can represent an API response as `unknown`.

```typescript
async function getUser(): Promise<unknown> {
    const response = await fetch(
        "https://api.example.com/users/1"
    );

    if (!response.ok) {
        throw new Error("Failed to fetch user");
    }

    return response.json();
}
```

Validate the response before using it.

```typescript
async function displayUser(): Promise<void> {
    const user = await getUser();

    if (
        typeof user === "object" &&
        user !== null &&
        "name" in user &&
        typeof user.name === "string"
    ) {
        console.log(user.name);
    }
}
```

The function checks the HTTP response status and validates the required property before accessing it.

# 🗁 Unknown in Arrays

An array can contain values of any type by using `unknown[]`.

```typescript
const values: unknown[] = [
    "Hello",
    100,
    true,
    null,
    undefined,
    { name: "John" }
];
```

Each element must be checked before performing type-specific operations.

```typescript
for (const value of values) {
    if (typeof value === "string") {
        console.log(value.toUpperCase());
    } else if (typeof value === "number") {
        console.log(value.toFixed(2));
    } else if (typeof value === "boolean") {
        console.log(value ? "Yes" : "No");
    }
}
```

This is safer than using `any[]` because TypeScript prevents unchecked operations on individual elements.

# 🗁 Unknown and Type Guards

A type guard is a condition or function that helps TypeScript identify the actual type of a value.

You can create a custom type guard using a type predicate.

```typescript
interface User {
    name: string;
    age: number;
}

function isUser(value: unknown): value is User {
    return (
        typeof value === "object" &&
        value !== null &&
        "name" in value &&
        typeof value.name === "string" &&
        "age" in value &&
        typeof value.age === "number"
    );
}
```

Use the type guard to validate an unknown value.

```typescript
const data: unknown = {
    name: "John",
    age: 25
};

if (isUser(data)) {
    console.log(data.name);
    console.log(data.age);
}
```

After the condition succeeds, TypeScript treats `data` as a `User`.

# 🗁 Best Practices

* Prefer `unknown` over `any` when the actual type of a value is not known.
* Validate unknown values before accessing their properties.
* Use `typeof` for primitive type checks.
* Use `instanceof` for class instance checks.
* Use custom type guards for reusable object validation.
* Avoid unnecessary type assertions.
* Treat API responses and user input as untrusted data.
* Use `unknown` for caught errors when appropriate.
* Return `unknown` from functions that produce dynamically typed values.
* Convert validated unknown values into specific application types.

# 🗁 Summary

The `unknown` type represents a value whose actual type is not known.

Important concepts include:

* Declaring unknown variables.
* Assigning different types to unknown variables.
* Understanding the difference between `unknown` and `any`.
* Type assignability.
* Type narrowing with `typeof` and `instanceof`.
* Object validation.
* Type assertions.
* Using unknown in functions and return types.
* Handling exceptions, JSON, and API responses.
* Creating custom type guards.
* Using unknown in arrays.

The key principle is that `unknown` accepts any value but requires you to establish its type before using it. This makes it a safer alternative to `any` when working with dynamic data.
