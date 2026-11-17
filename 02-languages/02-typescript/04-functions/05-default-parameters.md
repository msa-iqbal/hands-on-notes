# Default Parameters

Default parameters allow you to assign a default value to a function parameter.

When a function is called without providing an argument for a defaulted parameter, TypeScript uses the default value.

Default parameters make functions more flexible by reducing the need for callers to provide every argument.

# 🗁 Basic Default Parameter

Use the assignment operator (`=`) to define a default parameter value.

```typescript
function greet(name: string = "Guest"): string {
    return `Hello, ${name}!`;
}

console.log(greet());        // Hello, Guest!
console.log(greet("Alice")); // Hello, Alice!
```

Here:

* `name` is the function parameter.
* `"Guest"` is the default value.
* If no argument is provided, `"Guest"` is used.
* If an argument is provided, it replaces the default value.

# 🗁 Default Parameter Type Inference

TypeScript can automatically infer the parameter type from its default value.

```typescript
function greet(name = "Guest"): string {
    return `Hello, ${name}!`;
}
```

TypeScript infers `name` as `string`.

You can also explicitly annotate the parameter type:

```typescript
function greet(name: string = "Guest"): string {
    return `Hello, ${name}!`;
}
```

Explicit annotations can make function signatures easier to understand.

# 🗁 Multiple Default Parameters

A function can have multiple parameters with default values.

```typescript
function createUser(
    name: string = "Guest",
    age: number = 18,
    isActive: boolean = true
): void {
    console.log({
        name,
        age,
        isActive
    });
}

createUser();
createUser("Alice");
createUser("Bob", 25);
createUser("Charlie", 30, false);
```

Each omitted parameter uses its corresponding default value.

# 🗁 Passing Undefined to Default Parameters

When you explicitly pass `undefined`, TypeScript and JavaScript use the default value.

```typescript
function calculatePrice(
    price: number,
    discount: number = 0
): number {
    return price - discount;
}

console.log(calculatePrice(100));          // 100
console.log(calculatePrice(100, 20));      // 80
console.log(calculatePrice(100, undefined)); // 100
```

Passing `undefined` behaves like omitting the argument for that parameter.

# 🗁 Passing Null to Default Parameters

Passing `null` does not trigger a default value.

```typescript
function greet(name: string | null = "Guest"): string {
    return name ?? "Guest";
}

console.log(greet());        // Guest
console.log(greet("Alice")); // Alice
console.log(greet(null));    // Guest
```

The default value is applied only when the argument is omitted or explicitly `undefined`.

In this example, `??` handles the separate `null` case.

# 🗁 Default Parameters and Parameter Order

Default parameters can appear before required parameters.

```typescript
function createProduct(
    name: string = "Unknown",
    price: number
): void {
    console.log(name, price);
}
```

This is valid TypeScript, but callers must provide the required argument in its positional slot.

```typescript
createProduct(undefined, 100);
createProduct("Laptop", 1200);
```

A required parameter cannot be omitted simply because an earlier parameter has a default value.

For this reason, placing default parameters after required parameters is usually easier to use.

```typescript
function createProduct(
    price: number,
    name: string = "Unknown"
): void {
    console.log(name, price);
}

createProduct(100);
createProduct(1200, "Laptop");
```

# 🗁 Default Parameters vs Optional Parameters

Both default and optional parameters allow callers to omit arguments, but their behavior differs.

**Default parameter:**

```typescript
function greet(name: string = "Guest"): string {
    return `Hello, ${name}!`;
}

console.log(greet()); // Hello, Guest!
```

**Optional parameter:**

```typescript
function greet(name?: string): string {
    return `Hello, ${name ?? "Guest"}!`;
}

console.log(greet()); // Hello, Guest!
```

| Feature | Default Parameter | Optional Parameter |
| --- | --- | --- |
| Syntax | `name = "Guest"` | `name?: string` |
| Can be omitted | Yes | Yes |
| Value when omitted | Default value | `undefined` |
| Type inside function | `string` | `string \| undefined` |
| Explicit `undefined` | Uses default | Remains `undefined` |
| Explicit `null` | Does not trigger default | Allowed only if type permits |

Use default parameters when the function should automatically apply a fallback value.

Use optional parameters when the function needs to distinguish between a supplied value and an omitted value.

# 🗁 Default Parameters with Expressions

Default values can be expressions, not just literal values.

```typescript
function getCurrentDate(
    date: Date = new Date()
): Date {
    return date;
}

console.log(getCurrentDate());
```

The default expression is evaluated each time the function is called without an argument.

Another example:

```typescript
function generateId(
    prefix: string = "USER",
    id: number = Math.floor(Math.random() * 1000)
): string {
    return `${prefix}-${id}`;
}

console.log(generateId());
console.log(generateId("ADMIN"));
```

This is useful when default values depend on runtime information.

# 🗁 Default Parameters with Objects

Default parameters can use object values.

```typescript
type User = {
    name: string;
    role: string;
};

function createUser(
    user: User = {
        name: "Guest",
        role: "visitor"
    }
): User {
    return user;
}

console.log(createUser());
console.log(createUser({
    name: "Alice",
    role: "admin"
}));
```

Be careful when using mutable objects as default values in JavaScript. A default expression is evaluated on each function call, so a newly created object is used for each omitted argument.

# 🗁 Default Parameters with Arrays

You can also use arrays as default parameter values.

```typescript
function displayTags(
    tags: string[] = []
): void {
    console.log(tags);
}

displayTags();
displayTags(["typescript", "javascript"]);
```

Using `[]` creates a new array whenever the argument is omitted.

# 🗁 Default Parameters with Callbacks

Default values can also be assigned to callback parameters.

```typescript
type Logger = (message: string) => void;

function executeTask(
    taskName: string,
    logger: Logger = console.log
): void {
    logger(`Executing: ${taskName}`);
}

executeTask("Database backup");

executeTask("Application build", (message) => {
    console.error(message);
});
```

This allows a function to provide a default behavior while still allowing callers to customize it.

# 🗁 Default Parameters with Function Types

A reusable function type can describe a parameter that callers may omit because it has a default value in the implementation.

```typescript
type Greeting = (name?: string) => string;

const greet: Greeting = (name = "Guest") => {
    return `Hello, ${name}!`;
};

console.log(greet());
console.log(greet("Alice"));
```

A default parameter is optional to the caller. In a function type, represent that optionality with `?`.

# 🗁 Practical Example

Consider a pagination function that provides default page and page-size values.

```typescript
type PaginationResult = {
    page: number;
    pageSize: number;
    startIndex: number;
    endIndex: number;
};

function paginate(
    page: number = 1,
    pageSize: number = 10
): PaginationResult {
    const startIndex = (page - 1) * pageSize;
    const endIndex = startIndex + pageSize;

    return {
        page,
        pageSize,
        startIndex,
        endIndex
    };
}

console.log(paginate());

console.log(paginate(2));

console.log(paginate(3, 20));
```

This function provides sensible defaults while allowing callers to customize pagination.

# 🗁 Best Practices

* Use default parameters when a function has a sensible fallback value.
* Place default parameters after required parameters whenever possible.
* Use explicit parameter types when they improve readability.
* Remember that `undefined` triggers a default value, but `null` does not.
* Avoid unnecessary default values that hide missing or invalid input.
* Use fresh object or array expressions when mutable defaults are needed.
* Keep default behavior predictable and well documented.
* Prefer configuration objects when a function has many optional settings.

# 🗁 Summary

* Default parameters provide fallback values for function arguments.
* Use `=` to assign a default value.
* TypeScript can infer parameter types from default values.
* Multiple default parameters are supported.
* Omitting an argument or passing `undefined` triggers its default.
* Passing `null` does not trigger a default.
* Default parameters can use expressions, objects, arrays, and callbacks.
* In function type definitions, use optional parameter syntax to represent arguments that callers can omit.
