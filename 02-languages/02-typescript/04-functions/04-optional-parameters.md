# Optional Parameters

Optional parameters allow a function to accept arguments that callers are not required to provide.

In TypeScript, you declare an optional parameter using the question mark (`?`) after its name.

Optional parameters are useful when a function has inputs that are not always necessary.

# 🗁 Basic Optional Parameter

Use `?` to mark a parameter as optional.

```typescript
function greet(name: string, age?: number): void {
    console.log(`Hello, ${name}!`);

    if (age !== undefined) {
        console.log(`Age: ${age}`);
    }
}

greet("Alice");
greet("Bob", 25);
```

Here:

* `name` is a required parameter.
* `age` is an optional parameter.
* The function can be called with one or two arguments.

# 🗁 Optional Parameter Type

An optional parameter is treated as potentially `undefined` inside the function.

```typescript
function printMessage(message?: string): void {
    console.log(message);
}

printMessage("Hello");
printMessage();
```

Conceptually, the parameter has a type equivalent to:

```typescript
string | undefined
```

You must account for the possibility of `undefined` before performing operations that require a string.

```typescript
function formatName(name?: string): string {
    if (name === undefined) {
        return "Unknown";
    }

    return name.toUpperCase();
}

console.log(formatName("Alice")); // ALICE
console.log(formatName());        // Unknown
```

# 🗁 Optional Parameters Must Follow Required Parameters

Required parameters must appear before optional parameters.

```typescript
// Valid
function createUser(name: string, age?: number): void {
    console.log(name, age);
}
```

The following declaration is invalid:

```typescript
// Invalid
function createUser(age?: number, name: string): void {
    console.log(age, name);
}
```

TypeScript does not allow a required parameter after an optional parameter.

# 🗁 Multiple Optional Parameters

A function can have multiple optional parameters.

```typescript
function createProfile(
    name: string,
    age?: number,
    city?: string
): void {
    console.log({
        name,
        age,
        city
    });
}

createProfile("Alice");

createProfile("Bob", 25);

createProfile("Charlie", 30, "London");
```

All optional parameters must follow required parameters.

# 🗁 Passing Undefined to Optional Parameters

An optional parameter can be explicitly passed as `undefined`.

```typescript
function calculatePrice(
    price: number,
    discount?: number
): number {
    return price - (discount ?? 0);
}

console.log(calculatePrice(100));          // 100
console.log(calculatePrice(100, 20));      // 80
console.log(calculatePrice(100, undefined)); // 100
```

When `undefined` is passed, the function handles it as an absent value.

# 🗁 Optional Parameters vs Default Parameters

Optional parameters and default parameters both allow callers to omit arguments, but they behave differently.

**Optional parameter:**

```typescript
function greet(name?: string): void {
    console.log(name);
}

greet("Alice"); // Alice
greet();        // undefined
```

**Default parameter:**

```typescript
function greet(name: string = "Guest"): void {
    console.log(name);
}

greet("Alice"); // Alice
greet();        // Guest
```

Key differences:

| Feature | Optional Parameter | Default Parameter |
| --- | --- | --- |
| Syntax | `name?: string` | `name: string = "Guest"` |
| Can be omitted | Yes | Yes |
| Value when omitted | `undefined` | Default value |
| Type inside function | `string \| undefined` | `string` |
| Can explicitly pass `undefined` | Yes | Yes, triggers default |
| Can explicitly pass `null` | Only if type permits | Only if type permits |

Use optional parameters when the absence of a value is meaningful. Use default parameters when a fallback value should be applied automatically.

# 🗁 Optional Parameters with Union Types

An optional parameter can also accept multiple types.

```typescript
function displayValue(value?: string | number): void {
    if (value === undefined) {
        console.log("No value provided");
    } else {
        console.log(value);
    }
}

displayValue("Hello");
displayValue(100);
displayValue();
```

The effective parameter type is:

```typescript
string | number | undefined
```

Use type narrowing before performing type-specific operations.

# 🗁 Optional Callback Parameters

Optional parameters can be used in callback function signatures.

```typescript
type Logger = (
    message: string,
    category?: string
) => void;

const logMessage: Logger = (message, category) => {
    console.log(`[${category ?? "GENERAL"}] ${message}`);
};

logMessage("Application started");
logMessage("Database connected", "DATABASE");
```

The callback implementation must handle the possibility that the optional argument is `undefined`.

# 🗁 Optional Parameters with Function Types

You can define optional parameters in reusable function types.

```typescript
type SearchFunction = (
    keyword: string,
    limit?: number
) => string[];

const search: SearchFunction = (keyword, limit) => {
    const results = [
        `${keyword} result 1`,
        `${keyword} result 2`,
        `${keyword} result 3`
    ];

    return limit === undefined
        ? results
        : results.slice(0, limit);
};

console.log(search("TypeScript"));
console.log(search("TypeScript", 2));
```

This allows multiple function implementations to follow the same optional parameter contract.

# 🗁 Optional Parameters with Strict Mode

When `strictNullChecks` is enabled, TypeScript requires you to handle the possibility of `undefined`.

```typescript
function getLength(value?: string): number {
    // Error: 'value' is possibly 'undefined'.
    // return value.length;

    return value?.length ?? 0;
}
```

The optional chaining operator (`?.`) safely accesses a property when the value is not `null` or `undefined`.

The nullish coalescing operator (`??`) provides a fallback when the result is `null` or `undefined`.

# 🗁 Optional Parameters vs Optional Object Properties

Optional parameters and optional object properties use similar syntax, but they apply to different things.

**Optional parameter:**

```typescript
function greet(name?: string): void {
    console.log(name ?? "Guest");
}

greet();
```

**Optional object property:**

```typescript
type User = {
    name: string;
    age?: number;
};

const user: User = {
    name: "Alice"
};
```

An optional parameter controls whether an argument can be omitted. An optional property controls whether an object can omit a particular property.

# 🗁 Practical Example

Consider a product search function that accepts an optional category and result limit.

```typescript
type Product = {
    id: number;
    name: string;
    category: string;
};

const products: Product[] = [
    { id: 1, name: "Laptop", category: "Electronics" },
    { id: 2, name: "Phone", category: "Electronics" },
    { id: 3, name: "Chair", category: "Furniture" },
    { id: 4, name: "Desk", category: "Furniture" }
];

function searchProducts(
    keyword: string,
    category?: string,
    limit?: number
): Product[] {
    let results = products.filter((product) => {
        const matchesKeyword = product.name
            .toLowerCase()
            .includes(keyword.toLowerCase());

        const matchesCategory =
            category === undefined ||
            product.category === category;

        return matchesKeyword && matchesCategory;
    });

    if (limit !== undefined) {
        results = results.slice(0, limit);
    }

    return results;
}

console.log(searchProducts("phone"));

console.log(searchProducts("laptop", "Electronics"));

console.log(searchProducts("a", "Furniture", 1));
```

This example demonstrates how optional parameters make a function flexible without requiring multiple function definitions.

# 🗁 Best Practices

* Use `?` when callers should be allowed to omit an argument.
* Place required parameters before optional parameters.
* Handle `undefined` inside functions that use optional parameters.
* Use default parameters when a fallback value is appropriate.
* Avoid adding too many optional parameters to a single function.
* Consider an options object when a function has several optional configuration values.
* Use strict null checking to catch unsafe access to optional values.
* Keep optional parameter behavior predictable and clearly documented.

# 🗁 Summary

* Optional parameters allow callers to omit arguments.
* Use `?` after a parameter name to make it optional.
* Optional parameters are potentially `undefined`.
* Required parameters must appear before optional parameters.
* Multiple optional parameters are supported.
* Optional parameters can be explicitly passed as `undefined`.
* Default parameters provide fallback values, while optional parameters do not.
* Optional parameters work with function types and callbacks.
* Use type narrowing, optional chaining, and nullish coalescing to handle missing values safely.
