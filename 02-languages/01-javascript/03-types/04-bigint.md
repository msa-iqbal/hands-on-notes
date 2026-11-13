# JavaScript BigInt

`BigInt` is a primitive data type in JavaScript used to represent integers larger than the safe integer range of the `Number` type.

JavaScript's ordinary `Number` type uses IEEE 754 double-precision floating-point representation. It can represent integers exactly only within the safe integer range:

\[
-(2^{53}-1) \text{ to } (2^{53}-1)
\]

When applications need exact integer calculations beyond this range, `BigInt` provides an alternative.

**Example:**

```javascript
const regularNumber = 9007199254740991;
const largeInteger = 9007199254740992n;

console.log(regularNumber);
console.log(largeInteger);

console.log(typeof regularNumber); // number
console.log(typeof largeInteger);  // bigint
```

# 🗁 Creating BigInt Values

There are two common ways to create BigInt values.

**Using the `n` Suffix**

Append `n` to an integer literal.

```javascript
const first = 100n;
const second = 123456789012345678901234567890n;

console.log(first);
console.log(second);
```

**Using the BigInt() Function**

The `BigInt()` function converts supported values into BigInt.

```javascript
const first = BigInt(100);
const second = BigInt("123456789012345678901234567890");

console.log(first);  // 100n
console.log(second);
```

The input must represent an integer.

```javascript
console.log(BigInt("123")); // 123n
console.log(BigInt("12.5")); // Throws SyntaxError
```

A Number passed to `BigInt()` must already be an integer. If it is outside the safe integer range, precision may already have been lost before conversion.

```javascript
const value = BigInt(9007199254740991);

console.log(value);
```

For very large values, prefer a string or BigInt literal:

```javascript
const value = BigInt("900719925474099312345");

console.log(value);
```

# 🗁 BigInt Range

Unlike Number, BigInt does not have a fixed maximum or minimum integer value defined by the JavaScript language specification.

Its practical limits depend on available memory and JavaScript engine implementation limits.

```javascript
const large = 1234567890123456789012345678901234567890n;

console.log(large);
```

BigInt supports both positive and negative integers.

```javascript
const positive = 100n;
const negative = -100n;
const zero = 0n;

console.log(positive);
console.log(negative);
console.log(zero);
```

# 🗁 BigInt Arithmetic

BigInt supports common arithmetic operations.

| Operator | Description | Example |
| --- | --- | --- |
| `+` | Addition | `10n + 5n` |
| `-` | Subtraction | `10n - 5n` |
| `*` | Multiplication | `10n * 5n` |
| `/` | Division | `10n / 5n` |
| `%` | Remainder | `10n % 3n` |
| `**` | Exponentiation | `2n ** 3n` |

**Example:**

```javascript
const first = 20n;
const second = 6n;

console.log(first + second); // 26n
console.log(first - second); // 14n
console.log(first * second); // 120n
console.log(first / second); // 3n
console.log(first % second); // 2n
console.log(2n ** 3n);       // 8n
```

# 🗁 BigInt Division

BigInt division returns an integer result.

The fractional part is discarded rather than rounded.

```javascript
const result = 10n / 3n;

console.log(result); // 3n
```

Division truncates toward zero.

```javascript
console.log(10n / 3n);  // 3n
console.log(-10n / 3n); // -3n
```

If fractional precision is required, convert values to Number only when the precision and range requirements allow it, or use an appropriate decimal arithmetic implementation.

# 🗁 BigInt Modulo and Negative Values

The remainder operator (`%`) works with BigInt.

The result has the sign of the dividend.

```javascript
console.log(10n % 3n);   // 1n
console.log(-10n % 3n);  // -1n
console.log(10n % -3n);  // 1n
console.log(-10n % -3n); // -1n
```

# 🗁 BigInt and Number Compatibility

BigInt and Number are different primitive types.

JavaScript does not allow them to be mixed directly in arithmetic operations.

**Incorrect:**

```javascript
const first = 10;
const second = 20n;

// Throws TypeError
// console.log(first + second);
```

**Correct: Convert Number to BigInt**

```javascript
const first = 10;
const second = 20n;

const result = BigInt(first) + second;

console.log(result); // 30n
```

This conversion is appropriate when the Number represents an integer and is within the safe integer range.

**Convert BigInt to Number**

```javascript
const bigNumber = 100n;

const result = Number(bigNumber);

console.log(result); // 100
```

Converting a large BigInt into Number can lose precision or produce Infinity.

```javascript
const bigNumber = 900719925474099312345n;

const result = Number(bigNumber);

console.log(result);
```

Avoid converting large BigInt values to Number when exactness matters.

# 🗁 BigInt Comparisons

BigInt supports comparison operators.

**Strict Equality**

```javascript
console.log(10n === 10); // false
console.log(10n === 10n); // true
```

Strict equality compares both value and type.

**Loose Equality**

```javascript
console.log(10n == 10); // true
```

Loose equality can compare BigInt and Number values through JavaScript's equality rules.

Prefer strict equality in most application code.

**Relational Comparisons**

```javascript
console.log(10n > 5);   // true
console.log(10n < 20);  // true
console.log(10n >= 10); // true
```

Relational comparisons can compare BigInt and Number values.

# 🗁 BigInt Increment and Decrement

BigInt supports increment and decrement operators.

```javascript
let count = 10n;

count++;

console.log(count); // 11n

count--;

console.log(count); // 10n
```

Compound assignment operators are also supported.

```javascript
let total = 100n;

total += 50n;
total -= 20n;
total *= 2n;

console.log(total); // 260n
```

# 🗁 BigInt Bitwise Operations

BigInt supports bitwise operators.

```javascript
const first = 10n;
const second = 6n;

console.log(first & second); // 2n
console.log(first | second); // 14n
console.log(first ^ second); // 12n
console.log(~first);        // -11n
```

BigInt also supports left and right shift operations.

```javascript
const value = 8n;

console.log(value << 1n); // 16n
console.log(value >> 1n); // 4n
```

Unsigned right shift (`>>>`) is not supported for BigInt because BigInt uses signed integer semantics.

```javascript
// Throws TypeError
// console.log(8n >>> 1n);
```

# 🗁 BigInt and Boolean Conversion

BigInt values can be converted to Boolean.

`0n` is falsy, while every other BigInt value is truthy.

```javascript
console.log(Boolean(0n));  // false
console.log(Boolean(1n));  // true
console.log(Boolean(-1n)); // true
```

BigInt can be used in conditional statements.

```javascript
const balance = 1000n;

if (balance) {
    console.log("Balance available");
} else {
    console.log("No balance");
}
```

# 🗁 BigInt and typeof

The `typeof` operator identifies BigInt values.

```javascript
const value = 12345678901234567890n;

console.log(typeof value); // bigint
```

You can use it for type checking:

```javascript
function processInteger(value) {
    if (typeof value === "bigint") {
        console.log("BigInt value:", value);
    } else {
        console.log("A BigInt value is required.");
    }
}

processInteger(100n);
processInteger(100);
```

# 🗁 BigInt and Math Object

The built-in `Math` object does not accept BigInt values for its numeric methods.

```javascript
const value = 100n;

// Throws TypeError
// Math.sqrt(value);
```

Convert to Number when the value is within the required numeric range and precision is acceptable:

```javascript
const value = 100n;

console.log(Math.sqrt(Number(value))); // 10
```

For large integer operations, implement the required logic using BigInt-compatible arithmetic.

# 🗁 BigInt and JSON

The standard `JSON.stringify()` method does not serialize BigInt values by default.

```javascript
const user = {
    id: 12345678901234567890n
};

// Throws TypeError
// JSON.stringify(user);
```

A common solution is to convert BigInt values into strings.

```javascript
const user = {
    id: 12345678901234567890n
};

const json = JSON.stringify(user, (key, value) => {
    return typeof value === "bigint"
        ? value.toString()
        : value;
});

console.log(json);
```

The resulting JSON contains a string:

```json
{
    "id": "12345678901234567890"
}
```

To restore BigInt values, use a JSON reviver:

```javascript
const parsed = JSON.parse(json, (key, value) => {
    if (key === "id" && typeof value === "string") {
        return BigInt(value);
    }

    return value;
});

console.log(parsed.id);        // 12345678901234567890n
console.log(typeof parsed.id); // bigint
```

Use a clear serialization convention so ordinary strings containing digits are not accidentally converted into BigInt values.

# 🗁 BigInt and String Conversion

BigInt values can be converted into strings.

```javascript
const value = 12345678901234567890n;

console.log(String(value));
console.log(value.toString());
```

You can also combine BigInt with strings:

```javascript
const balance = 5000n;

console.log("Balance: " + balance);
console.log(`Balance: ${balance}`);
```

# 🗁 BigInt Practical Use Cases

BigInt is useful when exact integer calculations require values beyond the safe integer range.

Common applications include:

* Large database identifiers.
* High-precision integer counters.
* Cryptographic calculations.
* Large mathematical computations.
* Financial systems that represent monetary values using integer minor units.
* Applications working with large binary or integer representations.

**Example: Large Identifier**

```javascript
const userId = 987654321098765432109876543210n;

console.log(userId);
```

**Example: Large Counter**

```javascript
let counter = 0n;

counter += 1000000000000000000n;
counter += 1000000000000000000n;

console.log(counter);
```

**Example: Exact Integer Monetary Units**

```javascript
const priceInCents = 1999n;
const quantity = 3n;

const totalInCents = priceInCents * quantity;

console.log(totalInCents); // 5997n
```

BigInt provides exact integer arithmetic, but it does not provide decimal fractions. Representing monetary values in minor units is one way to avoid floating-point decimal errors.

# 🗁 Best Practices

* Use BigInt when exact integer calculations exceed the safe integer range of Number.
* Use the `n` suffix for BigInt literals.
* Use strings when converting very large integer values from external data.
* Avoid converting large BigInt values to Number when precision matters.
* Do not mix Number and BigInt directly in arithmetic operations.
* Use explicit conversion when the numeric range and precision are understood.
* Remember that BigInt division truncates toward zero.
* Use strings or a defined serialization strategy when working with JSON.
* Avoid passing BigInt values directly to Math methods.
* Use strict equality (`===`) when comparing values.
* Validate external numeric input before converting it to BigInt.

# 🗁 Common Mistakes

**Mixing BigInt and Number**

Incorrect:

```javascript
const first = 10n;
const second = 20;

// Throws TypeError
// console.log(first + second);
```

Correct:

```javascript
const first = 10n;
const second = 20n;

console.log(first + second);
```

**Expecting Decimal Division**

```javascript
console.log(10n / 3n); // 3n
```

BigInt division truncates the fractional part.

**Using an Invalid BigInt Literal**

Incorrect:

```javascript
// const value = 10.5n;
```

BigInt literals cannot contain decimal fractions.

**Converting Unsafe Numbers to BigInt**

Incorrect:

```javascript
const value = BigInt(9007199254740992);

console.log(value);
```

The Number may already have lost precision before conversion.

Better:

```javascript
const value = BigInt("9007199254740992");

console.log(value);
```

**Serializing BigInt Directly to JSON**

Incorrect:

```javascript
const data = {
    id: 100n
};

// JSON.stringify(data); // Throws TypeError
```

Convert BigInt values to strings or use an explicit serialization strategy.

**Using Math Methods Directly**

Incorrect:

```javascript
const value = 100n;

// Math.max(value, 200n); // Throws TypeError
```

Use BigInt-compatible comparisons or convert values when appropriate.

# 🗁 Summary

* BigInt is a primitive data type for arbitrary-precision integers.
* BigInt literals use the `n` suffix.
* `BigInt()` converts supported integer values into BigInt.
* BigInt supports arithmetic, comparison, and bitwise operations.
* BigInt division truncates toward zero.
* BigInt and Number cannot be mixed directly in arithmetic.
* BigInt can be converted to Number, but precision may be lost.
* `typeof` returns `"bigint"` for BigInt values.
* BigInt does not support decimal fractions.
* The Math object does not directly accept BigInt values.
* `JSON.stringify()` does not serialize BigInt values by default.
* BigInt is useful for large identifiers, counters, and exact integer calculations.

# 🗁 Practice

**Exercise: Create BigInt Values**

Create three BigInt values using:

* A BigInt literal.
* The `BigInt()` function with an integer.
* The `BigInt()` function with a numeric string.

Print each value and inspect its type.

**Exercise: BigInt Arithmetic**

Predict the output:

```javascript
const first = 100n;
const second = 30n;

console.log(first + second);
console.log(first - second);
console.log(first * second);
console.log(first / second);
console.log(first % second);
console.log(first ** 2n);
```

**Exercise: BigInt and Number**

Identify the problem and correct the following code:

```javascript
const price = 100n;
const quantity = 5;

const total = price * quantity;

console.log(total);
```

**Exercise: Division**

Predict the output:

```javascript
console.log(20n / 3n);
console.log(-20n / 3n);
console.log(20n % 3n);
console.log(-20n % 3n);
```

Explain why BigInt division does not return a fractional value.

**Exercise: Large Integer Precision**

Compare these values:

```javascript
const first = 9007199254740992;
const second = 9007199254740993n;

console.log(first);
console.log(second);

console.log(Number.isSafeInteger(first));
console.log(typeof second);
```

Explain why BigInt is useful for large integer values.

**Exercise: BigInt JSON Serialization**

Create an object containing a BigInt identifier.

Serialize it into JSON by converting the BigInt value into a string.

Parse the JSON again and restore the identifier as a BigInt.

**Exercise: Practical Counter**

Create a function named `incrementCounter` that accepts a BigInt value and returns the value increased by one.

Example:

```javascript
console.log(incrementCounter(100n)); // 101n
console.log(incrementCounter(999999999999999999n));
```

**Exercise: Large Integer Calculator**

Create a function named `calculate` that accepts two BigInt values and returns an object containing:

* Addition.
* Subtraction.
* Multiplication.
* Division.
* Remainder.

Test the function using large integer values.
