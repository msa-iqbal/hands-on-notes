# TypeScript Number Type

The `number` type represents numeric values in TypeScript. It supports integers, floating-point numbers, positive and negative values, decimal values, and special numeric values.

TypeScript uses JavaScript's standard `number` type, which is based on the IEEE 754 double-precision floating-point format.

Unlike some programming languages, TypeScript does not have separate built-in types for integers and floating-point numbers.

# 🗁 Declaring Number Variables

Use the `number` keyword to explicitly declare a numeric variable.

```typescript
let age: number = 25;
let price: number = 99.99;
let temperature: number = -5;
let quantity: number = 1000;

console.log(age);
console.log(price);
console.log(temperature);
console.log(quantity);
```

TypeScript also supports type inference.

```typescript
let score = 100;

// TypeScript infers score as number.
score = 150;

// TypeScript error
// score = "100";
```

# 🗁 Integer and Floating-Point Numbers

The `number` type supports both integer and floating-point values.

**Integer values:**

```typescript
const age: number = 25;
const quantity: number = 100;
const year: number = 2026;
```

**Floating-point values:**

```typescript
const price: number = 99.99;
const percentage: number = 75.5;
const pi: number = 3.14159;
```

Both integer and floating-point values belong to the same `number` type.

# 🗁 Positive and Negative Numbers

TypeScript supports positive, negative, and zero values.

```typescript
const positive: number = 100;
const negative: number = -100;
const zero: number = 0;

console.log(positive);
console.log(negative);
console.log(zero);
```

# 🗁 Number Literal Formats

JavaScript and TypeScript support several numeric literal formats.

**Decimal notation:**

```typescript
const decimal: number = 100;
```

**Binary notation:**

Binary numbers use the `0b` or `0B` prefix.

```typescript
const binary: number = 0b1010;

console.log(binary); // 10
```

**Octal notation:**

Octal numbers use the `0o` or `0O` prefix.

```typescript
const octal: number = 0o12;

console.log(octal); // 10
```

**Hexadecimal notation:**

Hexadecimal numbers use the `0x` or `0X` prefix.

```typescript
const hexadecimal: number = 0xFF;

console.log(hexadecimal); // 255
```

**Scientific notation:**

Scientific notation uses `e` or `E` to represent powers of ten.

```typescript
const largeNumber: number = 1e6;
const smallNumber: number = 1e-6;

console.log(largeNumber); // 1000000
console.log(smallNumber); // 0.000001
```

# 🗁 Numeric Separators

Numeric separators (`_`) improve the readability of large numeric literals.

```typescript
const population: number = 170_000_000;
const accountBalance: number = 1_500_000;
const hexadecimal: number = 0xFF_FF_FF;

console.log(population);
console.log(accountBalance);
console.log(hexadecimal);
```

Underscores are ignored when interpreting numeric literals.

They cannot be placed arbitrarily. For example, they cannot appear at the beginning or end of a numeric literal or directly next to its decimal point.

# 🗁 Arithmetic Operations

The `number` type supports standard arithmetic operators.

| Operator | Operation | Example |
| --- | --- | --- |
| `+` | Addition | `10 + 5` |
| `-` | Subtraction | `10 - 5` |
| `*` | Multiplication | `10 * 5` |
| `/` | Division | `10 / 5` |
| `%` | Remainder | `10 % 3` |
| `**` | Exponentiation | `2 ** 3` |

Example:

```typescript
const a: number = 20;
const b: number = 6;

console.log(a + b);  // 26
console.log(a - b);  // 14
console.log(a * b);  // 120
console.log(a / b);  // 3.333...
console.log(a % b);  // 2
console.log(a ** 2); // 400
```

# 🗁 Number Precision

JavaScript numbers use IEEE 754 double-precision floating-point representation.

This provides approximately 15 to 17 significant decimal digits of precision.

However, some decimal fractions cannot be represented exactly in binary floating-point.

```typescript
const result: number = 0.1 + 0.2;

console.log(result); // 0.30000000000000004
```

This behavior is not specific to TypeScript. It comes from JavaScript's underlying number representation.

For calculations requiring decimal precision, such as financial applications, consider using integer-based representations or a dedicated decimal arithmetic library.

Example using integer arithmetic for monetary values:

```typescript
const priceInCents: number = 1999;
const taxInCents: number = 200;

const totalInCents: number = priceInCents + taxInCents;

console.log(totalInCents); // 2199
console.log(totalInCents / 100); // 21.99
```

# 🗁 Special Numeric Values

JavaScript and TypeScript support several special numeric values.

**NaN**

`NaN` means "Not a Number". It represents an invalid numeric result.

```typescript
const result: number = Number("Hello");

console.log(result); // NaN
console.log(Number.isNaN(result)); // true
```

A notable behavior is that `NaN` is not equal to itself.

```typescript
console.log(NaN === NaN); // false
```

Use `Number.isNaN()` to check whether a value is `NaN`.

**Infinity**

`Infinity` represents positive infinity.

```typescript
const result: number = 1 / 0;

console.log(result); // Infinity
```

**Negative Infinity**

`-Infinity` represents negative infinity.

```typescript
const result: number = -1 / 0;

console.log(result); // -Infinity
```

**Negative Zero**

JavaScript distinguishes positive zero and negative zero internally.

```typescript
const positiveZero: number = 0;
const negativeZero: number = -0;

console.log(positiveZero === negativeZero); // true
console.log(Object.is(positiveZero, negativeZero)); // false
```

# 🗁 Number Constants

JavaScript provides the `Number` object with useful numeric constants.

| Constant | Description |
| --- | --- |
| `Number.MAX_VALUE` | Largest positive finite number |
| `Number.MIN_VALUE` | Smallest positive number greater than zero |
| `Number.MAX_SAFE_INTEGER` | Largest safely representable integer |
| `Number.MIN_SAFE_INTEGER` | Smallest safely representable integer |
| `Number.EPSILON` | Difference between 1 and the next representable number |
| `Number.POSITIVE_INFINITY` | Positive infinity |
| `Number.NEGATIVE_INFINITY` | Negative infinity |
| `Number.NaN` | Not-a-Number value |

Example:

```typescript
console.log(Number.MAX_VALUE);
console.log(Number.MIN_VALUE);

console.log(Number.MAX_SAFE_INTEGER);
// 9007199254740991

console.log(Number.MIN_SAFE_INTEGER);
// -9007199254740991

console.log(Number.EPSILON);
console.log(Number.POSITIVE_INFINITY);
console.log(Number.NEGATIVE_INFINITY);
console.log(Number.NaN);
```

# 🗁 Safe Integers

JavaScript can represent integers beyond the safe integer range, but not every integer in that range is represented uniquely.

The safe integer range is:

```typescript
Number.MIN_SAFE_INTEGER
Number.MAX_SAFE_INTEGER
```

Example:

```typescript
const a: number = 9007199254740991;
const b: number = 9007199254740992;

console.log(Number.isSafeInteger(a)); // true
console.log(Number.isSafeInteger(b)); // false
```

Use `Number.isSafeInteger()` to verify whether an integer can be represented safely.

For integers larger than the safe integer range, consider using `bigint`.

# 🗁 Common Number Methods

JavaScript provides several methods for validating, converting, and formatting numbers.

| Method | Description |
| --- | --- |
| `Number.isNaN()` | Checks whether a value is `NaN` |
| `Number.isFinite()` | Checks whether a value is finite |
| `Number.isInteger()` | Checks whether a value is an integer |
| `Number.isSafeInteger()` | Checks whether an integer is safely representable |
| `Number.parseInt()` | Parses an integer from a string |
| `Number.parseFloat()` | Parses a floating-point number from a string |
| `toFixed()` | Formats a number using a fixed number of decimal places |
| `toPrecision()` | Formats a number to a specified number of significant digits |
| `toString()` | Converts a number into a string |
| `toExponential()` | Formats a number using exponential notation |
| `toLocaleString()` | Formats a number according to locale conventions |

# 🗁 Checking Numeric Values

**Number.isNaN()**

```typescript
const value: number = Number("Hello");

console.log(Number.isNaN(value)); // true
```

**Number.isFinite()**

Checks whether a value is a finite number.

```typescript
console.log(Number.isFinite(100));       // true
console.log(Number.isFinite(Infinity));  // false
console.log(Number.isFinite(NaN));       // false
```

**Number.isInteger()**

```typescript
console.log(Number.isInteger(100));   // true
console.log(Number.isInteger(100.5)); // false
```

**Number.isSafeInteger()**

```typescript
console.log(Number.isSafeInteger(100)); // true

console.log(
    Number.isSafeInteger(9007199254740992)
); // false
```

# 🗁 Parsing Numeric Strings

Use `Number.parseInt()` to parse an integer from a string.

```typescript
const value: number = Number.parseInt("100", 10);

console.log(value); // 100
```

The second argument specifies the radix, or base. Using `10` explicitly indicates decimal parsing.

Use `Number.parseFloat()` to parse a floating-point number.

```typescript
const price: number = Number.parseFloat("99.99");

console.log(price); // 99.99
```

Both parsing methods can parse a valid numeric prefix.

```typescript
console.log(Number.parseInt("100px", 10)); // 100
console.log(Number.parseFloat("12.5kg"));  // 12.5
```

By contrast, `Number()` requires the entire non-whitespace string to represent a valid numeric value.

```typescript
console.log(Number("100"));    // 100
console.log(Number("100px"));  // NaN
console.log(Number(""));       // 0
```

For strict input validation, validate the original string before accepting a parsed result.

# 🗁 Formatting Numbers

**toFixed()**

Returns a string representing a number with a specified number of digits after the decimal point.

```typescript
const price: number = 99.5678;

console.log(price.toFixed(2)); // "99.57"
console.log(price.toFixed(1)); // "99.6"
console.log(price.toFixed(0)); // "100"
```

**toPrecision()**

Formats a number using a specified number of significant digits.

```typescript
const value: number = 123.456;

console.log(value.toPrecision(4)); // "123.5"
console.log(value.toPrecision(3)); // "123"
```

**toExponential()**

Formats a number using exponential notation.

```typescript
const value: number = 12345;

console.log(value.toExponential(2)); // "1.23e+4"
```

**toString()**

Converts a number into a string.

```typescript
const value: number = 255;

console.log(value.toString());    // "255"
console.log(value.toString(2));   // "11111111"
console.log(value.toString(16));  // "ff"
```

**toLocaleString()**

Formats numbers according to locale conventions.

```typescript
const amount: number = 1234567.89;

console.log(amount.toLocaleString("en-US"));
// 1,234,567.89

console.log(amount.toLocaleString("de-DE"));
// 1.234.567,89
```

For currency formatting:

```typescript
const amount: number = 1500;

const formatted = amount.toLocaleString("en-US", {
    style: "currency",
    currency: "USD"
});

console.log(formatted); // $1,500.00
```

# 🗁 Number Type with Type Aliases

Type aliases can provide meaningful names for numeric values.

```typescript
type UserId = number;
type ProductPrice = number;
type Quantity = number;

const userId: UserId = 101;
const price: ProductPrice = 99.99;
const quantity: Quantity = 5;
```

These aliases improve readability, but they do not create distinct runtime types. All three remain `number` types.

# 🗁 Number Type with Union Types

Union types allow variables to accept multiple types.

```typescript
let value: number | string;

value = 100;
value = "100";

// TypeScript error
// value = true;
```

This is useful when working with numeric values that may arrive from APIs or form inputs as strings.

```typescript
function calculateTotal(value: number | string): number {
    const amount = Number(value);

    if (!Number.isFinite(amount)) {
        throw new Error("Invalid numeric value");
    }

    return amount * 2;
}

console.log(calculateTotal(100));   // 200
console.log(calculateTotal("150")); // 300
```

# 🗁 BigInt vs Number

The `number` type cannot safely represent every integer beyond `Number.MAX_SAFE_INTEGER`.

JavaScript provides `bigint` for arbitrary-precision integer arithmetic.

```typescript
const numberValue: number = 100;
const bigIntValue: bigint = 100n;

console.log(numberValue);
console.log(bigIntValue);
```

The two types cannot be mixed directly in arithmetic operations.

```typescript
const a: number = 10;
const b: bigint = 20n;

// TypeScript error
// const result = a + b;
```

Convert values explicitly when the conversion is appropriate.

```typescript
const a: number = 10;
const b: bigint = 20n;

const result: bigint = BigInt(a) + b;

console.log(result); // 30n
```

Use `number` for ordinary numeric calculations and `bigint` when arbitrary-precision integer arithmetic is required.

# 🗁 Best Practices

* Use `number` for ordinary numeric values.
* Remember that integers and floating-point values share the same type.
* Use numeric separators to improve readability of large numbers.
* Understand floating-point precision limitations.
* Avoid relying on direct decimal equality for calculations involving floating-point values.
* Use integer-based representations or decimal libraries for precise financial calculations.
* Use `Number.isNaN()` instead of comparing values directly with `NaN`.
* Use `Number.isFinite()` to validate finite numeric results.
* Use `Number.isSafeInteger()` when integer precision matters.
* Specify the radix when using `Number.parseInt()`.
* Validate user input before accepting parsed numeric values.
* Use `bigint` when integer values exceed the safe integer range.
* Avoid mixing `number` and `bigint` directly.

# 🗁 Summary

The TypeScript `number` type represents integer and floating-point numeric values.

Important concepts include:

* Number declarations and type inference.
* Integer, decimal, binary, octal, hexadecimal, and scientific notation.
* Numeric separators.
* Arithmetic operations.
* Floating-point precision.
* Special values such as `NaN` and `Infinity`.
* Safe integer limits.
* Number methods and formatting.
* Numeric string conversion.
* Differences between `number` and `bigint`.

Understanding the `number` type is essential for calculations, financial data processing, measurements, counters, application logic, and data validation.
