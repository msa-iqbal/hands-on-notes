# JavaScript Number

The `Number` type is a primitive data type in JavaScript used to represent numeric values, including integers and floating-point numbers.

Unlike some programming languages that have separate types for integers and floating-point numbers, JavaScript uses a single `Number` type for most ordinary numeric calculations.

JavaScript numbers follow the IEEE 754 double-precision floating-point standard.

**Example:**

```javascript
const age = 25;
const price = 99.99;
const temperature = -10;

console.log(age);
console.log(price);
console.log(temperature);
```

# 🗁 Creating Numbers

Numbers can be created using numeric literals.

**Integer:**

```javascript
const age = 25;
const quantity = 100;
const year = 2026;
```

**Floating-Point Number:**

```javascript
const price = 99.99;
const temperature = 36.5;
const percentage = 87.75;
```

**Negative Number:**

```javascript
const balance = -500;
const temperature = -12.5;
```

**Scientific Notation:**

JavaScript supports scientific notation using `e` or `E`.

```javascript
const largeNumber = 2e6;
const smallNumber = 5e-3;

console.log(largeNumber); // 2000000
console.log(smallNumber); // 0.005
```

Scientific notation represents a number multiplied by a power of ten.

# 🗁 Number Range and Precision

JavaScript uses IEEE 754 double-precision floating-point representation.

The `Number` type provides approximately 15–17 significant decimal digits of precision.

The largest safe integer is:

```javascript
console.log(Number.MAX_SAFE_INTEGER);
// 9007199254740991
```

The smallest safe integer is:

```javascript
console.log(Number.MIN_SAFE_INTEGER);
// -9007199254740991
```

An integer is considered safe when it can be represented exactly and compared correctly with other integers.

**Example:**

```javascript
const first = 9007199254740991;
const second = 9007199254740992;

console.log(Number.isSafeInteger(first));  // true
console.log(Number.isSafeInteger(second)); // false
```

For integers outside this safe range, use `BigInt` when exact integer arithmetic is required.

# 🗁 Floating-Point Precision

JavaScript floating-point numbers cannot represent every decimal fraction exactly.

For example:

```javascript
console.log(0.1 + 0.2);

// 0.30000000000000004
```

This happens because many decimal fractions cannot be represented exactly in binary floating-point format.

**Comparing Floating-Point Numbers**

Avoid relying on direct equality when calculations involve floating-point precision.

```javascript
const result = 0.1 + 0.2;

console.log(result === 0.3); // false
```

A common approach is to compare the difference against a small tolerance:

```javascript
const result = 0.1 + 0.2;
const expected = 0.3;

const isEqual = Math.abs(result - expected) < Number.EPSILON;

console.log(isEqual); // true
```

For more complex calculations, choose a tolerance appropriate to the magnitude and precision requirements of the values.

**Working with Currency**

Avoid relying on floating-point arithmetic for exact monetary calculations.

One common approach is to store amounts in the smallest currency unit, such as cents or paisa.

```javascript
const priceInCents = 1999;
const quantity = 3;

const totalInCents = priceInCents * quantity;

console.log(totalInCents); // 5997
```

For financial applications requiring more complex decimal arithmetic, consider a decimal arithmetic library.

# 🗁 Special Numeric Values

JavaScript provides several special numeric values.

**NaN**

`NaN` means Not-a-Number.

It represents an invalid numeric result.

```javascript
const result = Number("hello");

console.log(result); // NaN
console.log(typeof result); // number
```

Interestingly, `NaN` is not equal to itself:

```javascript
console.log(NaN === NaN); // false
```

Use `Number.isNaN()` to check for `NaN`:

```javascript
console.log(Number.isNaN(NaN)); // true
console.log(Number.isNaN(100)); // false
```

**Infinity**

Infinity represents a numeric value larger than any finite number.

```javascript
console.log(1 / 0); // Infinity
console.log(Infinity > 1000000); // true
```

**Negative Infinity**

Negative Infinity represents a value smaller than any finite number.

```javascript
console.log(-1 / 0); // -Infinity
console.log(-Infinity < -1000000); // true
```

**Positive Zero and Negative Zero**

JavaScript supports both positive zero and negative zero.

```javascript
const positiveZero = 0;
const negativeZero = -0;

console.log(positiveZero === negativeZero); // true
console.log(Object.is(positiveZero, negativeZero)); // false
```

They behave similarly in most operations, but certain operations can distinguish them:

```javascript
console.log(1 / positiveZero); // Infinity
console.log(1 / negativeZero); // -Infinity
```

# 🗁 Number Constants

JavaScript provides useful constants through the `Number` object.

| Constant | Description |
| --- | --- |
| `Number.MAX_VALUE` | Largest finite representable number |
| `Number.MIN_VALUE` | Smallest positive representable number |
| `Number.MAX_SAFE_INTEGER` | Largest safe integer |
| `Number.MIN_SAFE_INTEGER` | Smallest safe integer |
| `Number.EPSILON` | Difference between 1 and the next representable Number |
| `Number.POSITIVE_INFINITY` | Positive Infinity |
| `Number.NEGATIVE_INFINITY` | Negative Infinity |
| `Number.NaN` | Not-a-Number |

**Example:**

```javascript
console.log(Number.MAX_VALUE);
console.log(Number.MIN_VALUE);

console.log(Number.MAX_SAFE_INTEGER);
console.log(Number.MIN_SAFE_INTEGER);

console.log(Number.EPSILON);

console.log(Number.POSITIVE_INFINITY);
console.log(Number.NEGATIVE_INFINITY);

console.log(Number.NaN);
```

Note that `Number.MIN_VALUE` is not the most negative number. It is the smallest positive number greater than zero that JavaScript can represent.

# 🗁 Checking Numeric Values

JavaScript provides several methods for checking numeric values.

**Number.isFinite()**

Checks whether a value is a finite number.

```javascript
console.log(Number.isFinite(100));       // true
console.log(Number.isFinite(99.99));     // true
console.log(Number.isFinite(Infinity));  // false
console.log(Number.isFinite(NaN));       // false
```

Unlike the global `isFinite()` function, `Number.isFinite()` does not convert non-number values before checking.

```javascript
console.log(Number.isFinite("100")); // false
console.log(isFinite("100"));        // true
```

**Number.isInteger()**

Checks whether a value is an integer.

```javascript
console.log(Number.isInteger(10));    // true
console.log(Number.isInteger(10.5));  // false
console.log(Number.isInteger("10"));  // false
```

**Number.isSafeInteger()**

Checks whether a value is a safe integer.

```javascript
console.log(Number.isSafeInteger(100)); // true

console.log(
    Number.isSafeInteger(9007199254740992)
); // false
```

**Number.isNaN()**

Checks whether a value is specifically `NaN`.

```javascript
console.log(Number.isNaN(NaN));       // true
console.log(Number.isNaN("hello"));   // false
console.log(Number.isNaN(undefined)); // false
```

# 🗁 Number Conversion

JavaScript provides several ways to convert values into numbers.

**Using Number()**

The `Number()` function converts a value into a number.

```javascript
console.log(Number("100"));  // 100
console.log(Number("99.99")); // 99.99
console.log(Number(true));   // 1
console.log(Number(false));  // 0
console.log(Number(null));   // 0
console.log(Number("hello")); // NaN
```

**Using parseInt()**

`parseInt()` parses an integer from a string.

```javascript
console.log(parseInt("100"));       // 100
console.log(parseInt("100.99"));    // 100
console.log(parseInt("25px"));      // 25
console.log(parseInt("hello"));     // NaN
```

You can specify the number base using the radix argument.

```javascript
console.log(parseInt("1010", 2));  // 10
console.log(parseInt("FF", 16));   // 255
console.log(parseInt("77", 8));    // 63
```

**Using parseFloat()**

`parseFloat()` parses a floating-point number from a string.

```javascript
console.log(parseFloat("99.99"));    // 99.99
console.log(parseFloat("25.50px"));  // 25.5
console.log(parseFloat("hello"));    // NaN
```

**Using Unary Plus**

The unary plus operator converts a value into a number.

```javascript
console.log(+"100");  // 100
console.log(+"99.99"); // 99.99
console.log(+true);   // 1
console.log(+false);  // 0
console.log(+"hello"); // NaN
```

# 🗁 Number Formatting

JavaScript provides methods for formatting numeric values.

**toFixed()**

Returns a string representing a number with a specified number of digits after the decimal point.

```javascript
const price = 99.4567;

console.log(price.toFixed(2)); // "99.46"
console.log(price.toFixed(1)); // "99.5"
console.log(price.toFixed(0)); // "99"
```

**toPrecision()**

Returns a string representing a number with a specified number of significant digits.

```javascript
const number = 123.456;

console.log(number.toPrecision(3)); // "123"
console.log(number.toPrecision(5)); // "123.46"
console.log(number.toPrecision(6)); // "123.456"
```

**toExponential()**

Returns a string representing a number in exponential notation.

```javascript
const number = 123456;

console.log(number.toExponential());
// "1.23456e+5"

console.log(number.toExponential(2));
// "1.23e+5"
```

**toString()**

Converts a number into a string.

```javascript
const number = 100;

console.log(number.toString()); // "100"
```

It can also accept a radix:

```javascript
const number = 255;

console.log(number.toString(2));  // "11111111"
console.log(number.toString(8));  // "377"
console.log(number.toString(16)); // "ff"
```

# 🗁 Number Arithmetic

JavaScript supports common arithmetic operators.

| Operator | Description | Example |
| --- | --- | --- |
| `+` | Addition | `10 + 5` |
| `-` | Subtraction | `10 - 5` |
| `*` | Multiplication | `10 * 5` |
| `/` | Division | `10 / 5` |
| `%` | Remainder | `10 % 3` |
| `**` | Exponentiation | `2 ** 3` |
| `++` | Increment | `count++` |
| `--` | Decrement | `count--` |

**Example:**

```javascript
const first = 10;
const second = 3;

console.log(first + second); // 13
console.log(first - second); // 7
console.log(first * second); // 30
console.log(first / second); // 3.333...
console.log(first % second); // 1
console.log(first ** second); // 1000
```

# 🗁 The Math Object

JavaScript provides the built-in `Math` object for mathematical operations.

**Common Math Methods**

| Method | Description |
| --- | --- |
| `Math.round()` | Rounds to the nearest integer |
| `Math.floor()` | Rounds down |
| `Math.ceil()` | Rounds up |
| `Math.trunc()` | Removes the fractional part |
| `Math.abs()` | Returns the absolute value |
| `Math.max()` | Returns the largest value |
| `Math.min()` | Returns the smallest value |
| `Math.sqrt()` | Returns the square root |
| `Math.cbrt()` | Returns the cube root |
| `Math.pow()` | Returns a number raised to a power |
| `Math.random()` | Returns a pseudorandom number from 0 inclusive to 1 exclusive |
| `Math.sign()` | Returns the sign of a number |
| `Math.log()` | Returns the natural logarithm |
| `Math.log10()` | Returns the base-10 logarithm |

**Examples:**

```javascript
console.log(Math.round(4.6)); // 5
console.log(Math.floor(4.9)); // 4
console.log(Math.ceil(4.1));  // 5
console.log(Math.trunc(4.9)); // 4

console.log(Math.abs(-10)); // 10

console.log(Math.max(10, 20, 30)); // 30
console.log(Math.min(10, 20, 30)); // 10

console.log(Math.sqrt(64)); // 8
console.log(Math.cbrt(27)); // 3

console.log(Math.pow(2, 3)); // 8
```

**Generating a Random Integer**

Generate a random integer between 1 and 10, inclusive:

```javascript
const randomNumber = Math.floor(Math.random() * 10) + 1;

console.log(randomNumber);
```

# 🗁 International Number Formatting

The `Intl.NumberFormat` API formats numbers according to locale and currency conventions.

**Locale Formatting:**

```javascript
const number = 1234567.89;

const formatted = new Intl.NumberFormat("en-US").format(number);

console.log(formatted); // 1,234,567.89
```

**Currency Formatting:**

```javascript
const price = 1234.5;

const formatted = new Intl.NumberFormat("en-US", {
    style: "currency",
    currency: "USD"
}).format(price);

console.log(formatted); // $1,234.50
```

**Bangladeshi Taka Formatting:**

```javascript
const amount = 123456.78;

const formatted = new Intl.NumberFormat("en-BD", {
    style: "currency",
    currency: "BDT"
}).format(amount);

console.log(formatted);
```

Formatting behavior depends on the locale and runtime's internationalization data.

# 🗁 Best Practices

* Use `Number.isNaN()` instead of relying on comparisons with `NaN`.
* Use `Number.isFinite()` when validating finite numeric values.
* Use `Number.isSafeInteger()` when exact integer representation matters.
* Remember that JavaScript floating-point arithmetic can produce precision errors.
* Use `BigInt` for integers outside the safe integer range.
* Avoid mixing `Number` and `BigInt` directly in arithmetic.
* Use `Number()` when strict numeric conversion is required.
* Use `parseInt()` and `parseFloat()` when intentionally parsing numeric prefixes from strings.
* Specify a radix when using `parseInt()`.
* Use `Intl.NumberFormat` for locale-aware number and currency formatting.
* Avoid using floating-point values directly for exact financial calculations.
* Validate external input before performing calculations.

# 🗁 Common Mistakes

**Assuming Decimal Arithmetic Is Exact**

```javascript
console.log(0.1 + 0.2); // 0.30000000000000004
```

Use an appropriate tolerance or a decimal arithmetic strategy.

**Using `isNaN()` Without Understanding Coercion**

```javascript
console.log(isNaN("hello"));       // true
console.log(Number.isNaN("hello")); // false
```

The global `isNaN()` converts values before checking. `Number.isNaN()` checks whether the value itself is `NaN`.

**Assuming `Number.MIN_VALUE` Is the Most Negative Number**

```javascript
console.log(Number.MIN_VALUE); // Smallest positive Number
console.log(-Number.MAX_VALUE); // Most negative finite Number
```

**Confusing `parseInt()` with `Number()`**

```javascript
console.log(Number("25px"));  // NaN
console.log(parseInt("25px")); // 25
```

`Number()` requires the complete input to represent a valid numeric value, while `parseInt()` can parse an integer prefix.

**Exceeding the Safe Integer Range**

```javascript
const value = 9007199254740992;

console.log(Number.isSafeInteger(value)); // false
```

Use BigInt when exact large integer arithmetic is required.

**Mixing BigInt and Number**

```javascript
const first = 10;
const second = 20n;

// Throws TypeError
// console.log(first + second);
```

Convert explicitly when the values are within the required range and the conversion is appropriate.

# 🗁 Summary

* `Number` is JavaScript's primary numeric primitive type.
* It represents integers and floating-point values.
* JavaScript numbers use IEEE 754 double-precision floating-point representation.
* The maximum safe integer is `9007199254740991`.
* Floating-point calculations can produce precision errors.
* `NaN`, `Infinity`, `-Infinity`, `0`, and `-0` are important numeric values.
* `Number.isFinite()`, `Number.isInteger()`, `Number.isSafeInteger()`, and `Number.isNaN()` help validate numeric values.
* `Number()`, `parseInt()`, `parseFloat()`, and unary plus provide numeric conversion.
* `toFixed()`, `toPrecision()`, and `toExponential()` provide number formatting.
* The `Math` object provides mathematical operations.
* `Intl.NumberFormat` provides locale-aware formatting.
* BigInt should be used when exact integers exceed the safe integer range.

# 🗁 Practice

**Exercise: Identify Number Types**

Create variables containing:

* A positive integer.
* A negative integer.
* A floating-point number.
* A number written in scientific notation.

Use `typeof` to inspect each value.

**Exercise: Floating-Point Precision**

Predict the output:

```javascript
const first = 0.1;
const second = 0.2;

console.log(first + second);
console.log(first + second === 0.3);
```

Explain why the equality comparison produces its result.

**Exercise: Numeric Validation**

Determine the output:

```javascript
console.log(Number.isNaN(NaN));
console.log(Number.isNaN("hello"));

console.log(Number.isFinite(Infinity));
console.log(Number.isFinite(100));

console.log(Number.isInteger(10.5));
console.log(Number.isSafeInteger(9007199254740992));
```

**Exercise: Number Conversion**

Predict the output:

```javascript
console.log(Number("100"));
console.log(Number("100px"));

console.log(parseInt("100.99"));
console.log(parseFloat("100.99px"));

console.log(+"50");
console.log(Number(true));
```

**Exercise: Number Formatting**

Create a program that formats the following number:

```javascript
const price = 1234.5678;
```

Use:

* `toFixed()` to display two decimal places.
* `toPrecision()` to display five significant digits.
* `toExponential()` to display exponential notation.
* `toString()` to convert it into a string.

**Exercise: Random Number Generator**

Create a function named `generateRandomNumber` that accepts a minimum and maximum value and returns a random integer within that inclusive range.

Example:

```javascript
console.log(generateRandomNumber(1, 10));
console.log(generateRandomNumber(50, 100));
```

**Exercise: Practical Price Calculator**

Create a function named `calculateTotal` that:

* Accepts a product price.
* Accepts a quantity.
* Calculates the total price.
* Returns the total rounded to two decimal places.

Example:

```javascript
console.log(calculateTotal(19.99, 3)); // 59.97
console.log(calculateTotal(12.50, 4)); // 50.00
```

Consider how floating-point precision affects the calculation.
