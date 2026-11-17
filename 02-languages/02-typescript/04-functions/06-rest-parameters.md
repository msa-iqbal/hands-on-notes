# Rest Parameters

Rest parameters allow a function to accept an indefinite number of arguments.

In TypeScript, rest parameters use three dots (`...`) before the parameter name. All remaining arguments are collected into a single array.

Rest parameters are useful when the number of arguments is not known in advance.

Common use cases include:

* Calculating totals from multiple numbers.
* Processing multiple strings.
* Logging messages.
* Building utility functions.
* Creating flexible APIs.

# 🗁 Basic Rest Parameter

Use `...` before a parameter name to declare a rest parameter.

```typescript
function sum(...numbers: number[]): number {
    return numbers.reduce((total, number) => total + number, 0);
}

console.log(sum(10, 20));       // 30
console.log(sum(10, 20, 30));   // 60
console.log(sum(5, 10, 15, 20)); // 50
```

Here:

* `...numbers` collects all arguments.
* `number[]` specifies that the collected values form an array of numbers.
* `reduce()` calculates their total.

# 🗁 Rest Parameter Type

A rest parameter must have an array type.

```typescript
function displayNames(...names: string[]): void {
    names.forEach((name) => {
        console.log(name);
    });
}

displayNames("Alice", "Bob", "Charlie");
```

You can also use the generic array syntax:

```typescript
function displayNumbers(...numbers: Array<number>): void {
    console.log(numbers);
}
```

Both `string[]` and `Array<string>` represent arrays of strings.

# 🗁 Rest Parameters with Required Parameters

A function can combine required parameters with a rest parameter.

```typescript
function logMessages(
    category: string,
    ...messages: string[]
): void {
    messages.forEach((message) => {
        console.log(`[${category}] ${message}`);
    });
}

logMessages(
    "INFO",
    "Server started",
    "Database connected",
    "Application ready"
);
```

The first argument is assigned to `category`, and all remaining arguments are collected into `messages`.

# 🗁 Rest Parameter Must Be Last

A rest parameter must always be the final parameter in a function declaration.

```typescript
// Valid
function createUser(
    name: string,
    ...roles: string[]
): void {
    console.log(name, roles);
}
```

The following declaration is invalid:

```typescript
// Invalid
function createUser(
    ...roles: string[],
    name: string
): void {
    console.log(name, roles);
}
```

No parameters can appear after a rest parameter.

# 🗁 Rest Parameters with Different Types

You can use union types when a rest parameter needs to accept multiple types.

```typescript
function displayValues(
    ...values: (string | number)[]
): void {
    values.forEach((value) => {
        console.log(value);
    });
}

displayValues("Alice", 25, "Bob", 30);
```

The parameter accepts an array containing strings or numbers.

You can narrow each value before performing type-specific operations:

```typescript
function processValues(
    ...values: (string | number)[]
): void {
    values.forEach((value) => {
        if (typeof value === "string") {
            console.log(value.toUpperCase());
        } else {
            console.log(value.toFixed(2));
        }
    });
}

processValues("typescript", 100, "javascript", 200);
```

# 🗁 Rest Parameters with Tuple Types

Tuple types allow you to define the exact types and positions of elements in a rest parameter.

```typescript
function createUser(
    ...args: [string, number, boolean]
): void {
    const [name, age, isActive] = args;

    console.log({
        name,
        age,
        isActive
    });
}

createUser("Alice", 25, true);
```

The tuple defines:

* First argument: `string`.
* Second argument: `number`.
* Third argument: `boolean`.

Unlike a regular rest parameter, this tuple-based signature has a fixed argument structure.

# 🗁 Optional Elements in Rest Tuple Types

Tuple types can include optional elements.

```typescript
function createProduct(
    ...args: [string, number?]
): void {
    const [name, price] = args;

    console.log(name, price);
}

createProduct("Laptop");
createProduct("Laptop", 1200);
```

The second argument is optional because the tuple declares it with `?`.

# 🗁 Rest Parameters with Generic Types

Rest parameters can use generics to preserve the types of different arguments.

```typescript
function identityArguments<T extends unknown[]>(
    ...args: T
): T {
    return args;
}

const result = identityArguments("Alice", 25, true);
```

TypeScript infers a tuple-like type for `result`, preserving the types and positions of the supplied arguments.

A more explicit example:

```typescript
function combine<T extends unknown[]>(
    ...values: T
): T {
    return values;
}

const user = combine("Alice", 25);
const settings = combine(true, "dark", 10);
```

Generic rest parameters are useful when building flexible, type-safe utility functions.

# 🗁 Rest Parameters with Function Types

You can define rest parameters in function type aliases.

```typescript
type SumFunction = (...numbers: number[]) => number;

const sum: SumFunction = (...numbers) => {
    return numbers.reduce((total, number) => total + number, 0);
};

console.log(sum(10, 20, 30));
```

The function implementation must follow the declared rest parameter type.

# 🗁 Passing Arrays with Spread Syntax

Rest parameters and spread syntax are related but serve different purposes.

* Rest parameters collect multiple arguments into an array.
* Spread syntax expands an array or iterable into individual arguments.

Example:

```typescript
function add(a: number, b: number, c: number): number {
    return a + b + c;
}

const numbers = [10, 20, 30];

console.log(add(...numbers)); // 60
```

You can also pass an array to a rest parameter using spread syntax:

```typescript
function sum(...numbers: number[]): number {
    return numbers.reduce((total, number) => total + number, 0);
}

const values = [10, 20, 30, 40];

console.log(sum(...values)); // 100
```

The spread arguments must be compatible with the function's parameter types.

# 🗁 Rest Parameters vs Spread Syntax

| Feature | Rest Parameters | Spread Syntax |
| --- | --- | --- |
| Purpose | Collect multiple arguments | Expand an iterable or object |
| Syntax | `...args` in parameter position | `...values` in an expression |
| Result | An array of arguments | Individual expanded values or properties |
| Common use | Function declarations | Function calls, arrays, and objects |

Example comparison:

```typescript
// Rest parameter
function collect(...items: string[]): string[] {
    return items;
}

// Spread syntax
const names = ["Alice", "Bob"];

const result = collect(...names);

console.log(result);
```

# 🗁 Practical Example

Consider a shopping cart function that calculates the total price of multiple products.

```typescript
type Product = {
    name: string;
    price: number;
};

function calculateTotal(
    ...products: Product[]
): number {
    return products.reduce((total, product) => {
        return total + product.price;
    }, 0);
}

const total = calculateTotal(
    { name: "Keyboard", price: 50 },
    { name: "Mouse", price: 25 },
    { name: "Monitor", price: 200 }
);

console.log(total); // 275
```

The function accepts any number of products without requiring a separate parameter for each one.

# 🗁 Best Practices

* Use rest parameters when the number of arguments is variable.
* Always provide an appropriate array type for rest parameters.
* Keep rest parameters at the end of the parameter list.
* Use tuple types when argument positions and types must follow a specific structure.
* Use generics when you need to preserve argument types.
* Use spread syntax when passing an existing array as individual arguments.
* Avoid rest parameters when a single array parameter provides a clearer API.
* Keep function signatures simple and predictable.

# 🗁 Summary

* Rest parameters allow functions to accept multiple arguments.
* Use three dots (`...`) before the parameter name.
* Rest parameters collect arguments into an array.
* Rest parameters must have an array type.
* A rest parameter must be the final function parameter.
* Union types allow rest parameters to accept multiple value types.
* Tuple types can define fixed argument structures.
* Generic rest parameters preserve argument types.
* Spread syntax expands arrays into individual function arguments.
