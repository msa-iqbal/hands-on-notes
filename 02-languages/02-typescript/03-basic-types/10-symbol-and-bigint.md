# Symbol and BigInt

JavaScript provides two primitive types beyond the commonly used `string`, `number`, and `boolean` types:

* `symbol` — represents a unique, immutable identifier.
* `bigint` — represents integers larger than the range supported by JavaScript's `number` type.

TypeScript supports both types and provides static type checking for their usage.

# 🗁 Symbol

A `symbol` is a primitive value that is guaranteed to be unique.

Create a symbol using the `Symbol()` function:

```typescript
const id = Symbol();

console.log(typeof id); // "symbol"
```

Even when two symbols have the same description, they are different values:

```typescript
const first = Symbol("id");
const second = Symbol("id");

console.log(first === second); // false
```

The description is only a debugging label. It does not determine symbol identity.

# 🗁 Symbol Type Annotation

Use the `symbol` type annotation for variables that can hold symbol values:

```typescript
let identifier: symbol;

identifier = Symbol("user-id");
identifier = Symbol("product-id");
```

TypeScript also infers the `symbol` type when a variable is explicitly annotated:

```typescript
const token: symbol = Symbol("token");
```

# 🗁 Unique Symbol Type

TypeScript provides `unique symbol`, a more specific type representing one particular symbol value.

A `unique symbol` must be declared using `const` or a `static readonly` property.

```typescript
const userId: unique symbol = Symbol("user-id");
const productId: unique symbol = Symbol("product-id");
```

Each `unique symbol` has its own distinct type:

```typescript
const firstId: unique symbol = Symbol("id");
const secondId: unique symbol = Symbol("id");

// These values have different unique symbol types.
```

They cannot be assigned to each other's variables:

```typescript
const first: unique symbol = Symbol("first");
const second: unique symbol = Symbol("second");

// Error: Type 'typeof second' is not assignable to type 'typeof first'.
const result: typeof first = second;
```

Use `typeof` to refer to the type of a particular unique symbol.

```typescript
const applicationId: unique symbol = Symbol("application-id");

let id: typeof applicationId = applicationId;
```

# 🗁 Symbol as Object Property Keys

Symbols can be used as object property keys.

Unlike string keys, symbol keys do not conflict with other properties that happen to use the same description.

```typescript
const internalId = Symbol("id");

const user = {
    name: "Alice",
    [internalId]: 101
};

console.log(user.name); // "Alice"
console.log(user[internalId]); // 101
```

TypeScript can also define symbol-keyed properties through computed property names:

```typescript
const metadataKey: unique symbol = Symbol("metadata");

interface User {
    name: string;
    [metadataKey]: {
        role: string;
    };
}

const user: User = {
    name: "Alice",
    [metadataKey]: {
        role: "admin"
    }
};
```

# 🗁 Global Symbol Registry

JavaScript provides `Symbol.for()` to retrieve or create a symbol in a global symbol registry.

```typescript
const first = Symbol.for("shared-id");
const second = Symbol.for("shared-id");

console.log(first === second); // true
```

Unlike `Symbol()`, calling `Symbol.for()` with the same registry key returns the same symbol.

Retrieve its registry key using `Symbol.keyFor()`:

```typescript
const shared = Symbol.for("application-id");

console.log(Symbol.keyFor(shared)); // "application-id"
```

The return type of `Symbol.keyFor()` is `string | undefined` because a symbol created with `Symbol()` does not have a global registry key.

```typescript
const local = Symbol("local-id");

const key = Symbol.keyFor(local);

console.log(key); // undefined
```

# 🗁 BigInt

`bigint` represents integer values larger than the maximum safe integer supported by JavaScript's `number` type.

Create a BigInt by appending `n` to an integer literal:

```typescript
const largeNumber = 9007199254740993n;

console.log(largeNumber);
console.log(typeof largeNumber); // "bigint"
```

You can also use the `BigInt()` function:

```typescript
const value = BigInt("9007199254740993");

console.log(value); // 9007199254740993n
```

# 🗁 BigInt Type Annotation

Use the `bigint` type annotation when declaring variables:

```typescript
let accountBalance: bigint;

accountBalance = 1000000000000000000n;
accountBalance = BigInt("2500000000000000000");
```

TypeScript also infers the `bigint` type:

```typescript
const population = 8000000000n;
```

# 🗁 BigInt Arithmetic

BigInt supports common arithmetic operations:

```typescript
const first: bigint = 100n;
const second: bigint = 50n;

console.log(first + second); // 150n
console.log(first - second); // 50n
console.log(first * second); // 5000n
console.log(first / second); // 2n
console.log(first % second); // 0n
console.log(first ** 2n); // 10000n
```

BigInt division truncates the fractional part:

```typescript
const result = 10n / 3n;

console.log(result); // 3n
```

# 🗁 BigInt and Number Are Different Types

BigInt and Number cannot be mixed directly in arithmetic operations.

```typescript
const regularNumber: number = 100;
const largeInteger: bigint = 200n;

// Error: Operator '+' cannot be applied to types 'number' and 'bigint'.
const result = regularNumber + largeInteger;
```

Convert values explicitly when necessary:

```typescript
const regularNumber = 100;
const largeInteger = 200n;

const result = BigInt(regularNumber) + largeInteger;

console.log(result); // 300n
```

Converting BigInt to Number can lose precision when the value exceeds JavaScript's safe integer range.

```typescript
const largeValue = 9007199254740993n;

const converted = Number(largeValue);

console.log(converted); // Precision may be lost.
```

# 🗁 BigInt Comparisons

BigInt supports comparison operators.

```typescript
const first = 100n;
const second = 200n;

console.log(first < second); // true
console.log(first > second); // false
console.log(first === second); // false
console.log(first !== second); // true
```

Loose equality can compare BigInt and Number values, but strict equality considers their types:

```typescript
console.log(100n == 100);  // true
console.log(100n === 100); // false
```

Prefer strict equality to avoid unexpected type conversions.

# 🗁 BigInt Literal Types

TypeScript can infer literal BigInt types when values are declared with `const`.

```typescript
const minimum = 10n;
const maximum = 100n;
```

These variables retain their specific literal types.

Use the general `bigint` type when a variable needs to accept different BigInt values:

```typescript
let amount: bigint = 100n;

amount = 200n;
amount = 500n;
```

# 🗁 Compiler Configuration

BigInt requires an appropriate JavaScript target.

For TypeScript projects, use an ES2020 or newer target:

```json
{
    "compilerOptions": {
        "target": "ES2020",
        "strict": true
    }
}
```

The `target` option controls the JavaScript language version TypeScript emits. BigInt cannot be safely downleveled to older JavaScript targets because it requires native runtime support.

# 🗁 JSON Serialization

The native `JSON.stringify()` method does not serialize BigInt values directly.

```typescript
const data = {
    id: 100n
};

// Throws a TypeError.
JSON.stringify(data);
```

Convert BigInt values to strings before serialization:

```typescript
const data = {
    id: 100n
};

const json = JSON.stringify(data, (_key, value) =>
    typeof value === "bigint" ? value.toString() : value
);

console.log(json); // {"id":"100"}
```

Convert the string back to BigInt when parsing:

```typescript
const parsed = JSON.parse(json, (_key, value) =>
    typeof value === "string" && /^\d+$/.test(value)
        ? BigInt(value)
        : value
);

console.log(parsed.id); // 100n
```

In production applications, use a schema or explicit serialization strategy to distinguish BigInt fields from ordinary numeric strings.

# 🗁 Symbol vs BigInt

| Feature | Symbol | BigInt |
| --- | --- | --- |
| TypeScript type | `symbol` | `bigint` |
| Purpose | Unique identifiers | Arbitrarily large integers |
| Creation | `Symbol()` | `123n` or `BigInt()` |
| Mutable | No | No |
| Arithmetic | Not supported | Supported |
| Equality | Supported | Supported |
| Object property keys | Supported | Not directly supported |
| JSON serialization | Symbol properties are generally ignored | Throws by default |
| Common use cases | Internal keys, metadata, protocols | Large counters, identifiers, precise integer calculations |

# 🗁 Best Practices

* Use `symbol` when you need unique property keys or identifiers.
* Use `unique symbol` when a particular symbol must have its own distinct compile-time type.
* Use `Symbol.for()` when symbols need to be shared through the global symbol registry.
* Use `bigint` when integer values exceed the safe range of JavaScript's `number` type.
* Avoid mixing `number` and `bigint` in arithmetic expressions.
* Be careful when converting BigInt values to Number because precision may be lost.
* Convert BigInt values to strings before JSON serialization.
* Configure TypeScript with an ES2020 or newer target when using BigInt.

# 🗁 Summary

* `symbol` represents a unique primitive value.
* `unique symbol` represents the specific identity of a symbol at the type level.
* `Symbol.for()` provides access to a global symbol registry.
* `bigint` represents integers of arbitrary size.
* BigInt values use the `n` suffix or the `BigInt()` constructor.
* BigInt and Number cannot be mixed directly in arithmetic.
* BigInt requires native runtime support and an appropriate TypeScript target.
