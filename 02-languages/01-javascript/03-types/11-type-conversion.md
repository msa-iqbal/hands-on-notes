# JavaScript Type Conversion

**Type conversion** is the process of changing a value from one data type to another.

JavaScript provides several built-in functions and operators that allow developers to convert values explicitly.

For example:

```javascript
const value = "100";

const number = Number(value);

console.log(number);        // 100
console.log(typeof number); // "number"
```

Type conversion is particularly important when working with:

* User input from HTML forms.
* API responses.
* JSON data.
* Database values.
* Mathematical calculations.
* Conditional expressions.
* External data sources.

JavaScript supports both explicit and implicit type conversion. This document focuses on explicit conversion and the built-in conversion mechanisms.

# 🗁 Explicit Type Conversion

Explicit type conversion happens when a developer intentionally converts a value into another type.

JavaScript provides several conversion functions:

| Function | Purpose |
| --- | --- |
| `String()` | Converts a value into a string |
| `Number()` | Converts a value into a number |
| `Boolean()` | Converts a value into a Boolean |
| `BigInt()` | Converts a compatible value into a BigInt |
| `parseInt()` | Parses an integer from a string |
| `parseFloat()` | Parses a floating-point number from a string |

```javascript
const value = "100";

console.log(String(value));  // "100"
console.log(Number(value));  // 100
console.log(Boolean(value)); // true
```

# 🗁 String Conversion

String conversion transforms a value into its string representation.

The recommended explicit conversion method is `String()`.

```javascript
console.log(String(100));       // "100"
console.log(String(10.5));      // "10.5"
console.log(String(true));      // "true"
console.log(String(false));     // "false"
console.log(String(null));      // "null"
console.log(String(undefined)); // "undefined"
console.log(String(100n));      // "100"
```

**Converting arrays into strings**

Arrays are converted into comma-separated string representations.

```javascript
console.log(String([1, 2, 3])); // "1,2,3"
console.log(String(["a", "b", "c"])); // "a,b,c"
console.log(String([])); // ""
```

Array string conversion uses the array's `toString()` behavior, which delegates to `join()`.

**Converting objects into strings**

Ordinary objects generally produce `"[object Object]"`.

```javascript
const user = {
    name: "John",
    age: 25
};

console.log(String(user));
// "[object Object]"
```

For structured object data, use `JSON.stringify()` when JSON serialization is appropriate.

```javascript
console.log(JSON.stringify(user));
// '{"name":"John","age":25}'
```

**Using toString()**

Many JavaScript values provide a `toString()` method.

```javascript
const number = 100;

console.log(number.toString()); // "100"
console.log(true.toString());   // "true"
```

Unlike `String()`, calling `toString()` directly on `null` or `undefined` throws a `TypeError`.

```javascript
// null.toString();      // TypeError
// undefined.toString(); // TypeError

console.log(String(null)); // "null"
console.log(String(undefined)); // "undefined"
```

# 🗁 Number Conversion

Number conversion transforms a value into a JavaScript number.

The recommended explicit conversion function is `Number()`.

```javascript
console.log(Number("100"));     // 100
console.log(Number("10.5"));    // 10.5
console.log(Number(""));        // 0
console.log(Number(" "));       // 0
console.log(Number(true));      // 1
console.log(Number(false));     // 0
console.log(Number(null));      // 0
console.log(Number(undefined)); // NaN
```

**Converting numeric strings**

```javascript
const age = "25";
const price = "99.99";

console.log(Number(age));   // 25
console.log(Number(price)); // 99.99
```

**Converting Boolean values**

```javascript
console.log(Number(true));  // 1
console.log(Number(false)); // 0
```

**Converting null and undefined**

```javascript
console.log(Number(null));      // 0
console.log(Number(undefined)); // NaN
```

**Converting invalid strings**

```javascript
console.log(Number("hello")); // NaN
console.log(Number("100px")); // NaN
console.log(Number("10abc")); // NaN
```

`Number()` attempts to convert the entire string. If the string does not represent a valid numeric value, the result is `NaN`.

# 🗁 parseInt()

`parseInt()` parses an integer from a string.

It reads the string from left to right and stops when it encounters a character that cannot be part of the integer.

```javascript
console.log(parseInt("100"));    // 100
console.log(parseInt("100.50")); // 100
console.log(parseInt("25px"));   // 25
console.log(parseInt("10abc"));  // 10
```

**Using a radix**

The second argument specifies the numeral system, known as the radix.

```javascript
console.log(parseInt("10", 10)); // 10
console.log(parseInt("10", 2));  // 2
console.log(parseInt("10", 8));  // 8
console.log(parseInt("10", 16)); // 16
```

Common radix values:

| Radix | Number System |
| --- | --- |
| `2` | Binary |
| `8` | Octal |
| `10` | Decimal |
| `16` | Hexadecimal |

Always specify the radix when parsing integers unless you have a specific reason not to.

```javascript
console.log(parseInt("1010", 2));  // 10
console.log(parseInt("FF", 16));   // 255
console.log(parseInt("075", 10));  // 75
```

**Invalid input**

```javascript
console.log(parseInt("hello", 10)); // NaN
console.log(parseInt("10.5", 10));  // 10
console.log(parseInt("", 10));      // NaN
```

# 🗁 parseFloat()

`parseFloat()` parses a floating-point number from a string.

It supports decimal values and scientific notation.

```javascript
console.log(parseFloat("10.5"));      // 10.5
console.log(parseFloat("99.99"));     // 99.99
console.log(parseFloat("25.75px"));   // 25.75
console.log(parseFloat("10.5abc"));  // 10.5
console.log(parseFloat("1.5e2"));    // 150
```

**Invalid input**

```javascript
console.log(parseFloat("hello")); // NaN
console.log(parseFloat(""));      // NaN
console.log(parseFloat("abc10")); // NaN
```

# 🗁 Number() vs parseInt() vs parseFloat()

These conversion methods have different parsing behavior.

| Input | `Number()` | `parseInt()` | `parseFloat()` |
| --- | ---: | ---: | ---: |
| `"100"` | `100` | `100` | `100` |
| `"10.5"` | `10.5` | `10` | `10.5` |
| `"25px"` | `NaN` | `25` | `25` |
| `"10abc"` | `NaN` | `10` | `10` |
| `""` | `0` | `NaN` | `NaN` |
| `"  "` | `0` | `NaN` | `NaN` |
| `"1.5e2"` | `150` | `1` | `150` |
| `"hello"` | `NaN` | `NaN` | `NaN` |

**When to use each method**

* Use `Number()` when the entire input should represent a valid number.
* Use `parseInt()` when parsing an integer prefix from a string.
* Use `parseFloat()` when parsing a decimal prefix from a string.

# 🗁 Boolean Conversion

Boolean conversion transforms a value into either `true` or `false`.

Use `Boolean()` for explicit conversion.

```javascript
console.log(Boolean(1));         // true
console.log(Boolean(0));         // false
console.log(Boolean("Hello"));   // true
console.log(Boolean(""));        // false
console.log(Boolean(null));      // false
console.log(Boolean(undefined)); // false
console.log(Boolean([]));        // true
console.log(Boolean({}));        // true
```

**Falsy values**

JavaScript has a small, well-defined set of falsy values:

```javascript
console.log(Boolean(false));     // false
console.log(Boolean(0));         // false
console.log(Boolean(-0));        // false
console.log(Boolean(0n));        // false
console.log(Boolean(""));        // false
console.log(Boolean(null));      // false
console.log(Boolean(undefined)); // false
console.log(Boolean(NaN));       // false
```

Everything else is truthy, including empty arrays and empty objects.

```javascript
console.log(Boolean([]));      // true
console.log(Boolean({}));      // true
console.log(Boolean("false")); // true
console.log(Boolean("0"));     // true
```

# 🗁 BigInt Conversion

`BigInt()` converts compatible values into BigInt values.

BigInt is used for integers that exceed the safe integer range of JavaScript's `Number` type.

```javascript
console.log(BigInt(100));    // 100n
console.log(BigInt("100")); // 100n
console.log(BigInt("9007199254740993")); // 9007199254740993n
```

**Important considerations**

* BigInt cannot represent fractional values.
* A string must contain a valid integer representation.
* Converting an already imprecise Number cannot recover its original precision.
* BigInt and Number cannot be mixed directly in most arithmetic operations.

```javascript
console.log(BigInt("10.5")); // SyntaxError

const bigNumber = 100n;
const normalNumber = 10;

// console.log(bigNumber + normalNumber); // TypeError

console.log(bigNumber + BigInt(normalNumber)); // 110n
```

For large integer values, converting directly from a string is often safer than converting from a Number.

# 🗁 Symbol Conversion

Symbols are unique primitive values.

They can be explicitly converted into strings using `String()`.

```javascript
const id = Symbol("user-id");

console.log(String(id)); // "Symbol(user-id)"
```

However, directly calling `toString()` also works:

```javascript
console.log(id.toString()); // "Symbol(user-id)"
```

Symbols cannot be implicitly converted into strings through ordinary concatenation.

```javascript
const id = Symbol("id");

// console.log("ID: " + id); // TypeError

console.log("ID: " + String(id)); // "ID: Symbol(id)"
```

# 🗁 Unary Plus Conversion

The unary plus (`+`) operator converts a value into a number.

```javascript
console.log(+"100");   // 100
console.log(+"10.5");  // 10.5
console.log(+"");      // 0
console.log(+"hello"); // NaN
console.log(+true);    // 1
console.log(+false);   // 0
console.log(+null);    // 0
```

It provides a shorter alternative to `Number()`.

```javascript
const value = "50";

console.log(Number(value)); // 50
console.log(+value);        // 50
```

For readability, `Number()` is often preferable in application code.

# 🗁 Converting Values to Integers

JavaScript provides several methods for converting or rounding numbers to integer values.

**Math.trunc()**

Removes the fractional part without rounding.

```javascript
console.log(Math.trunc(10.9));  // 10
console.log(Math.trunc(10.1));  // 10
console.log(Math.trunc(-10.9)); // -10
```

**Math.floor()**

Rounds a number down toward negative infinity.

```javascript
console.log(Math.floor(10.9));  // 10
console.log(Math.floor(10.1));  // 10
console.log(Math.floor(-10.1)); // -11
```

**Math.ceil()**

Rounds a number up toward positive infinity.

```javascript
console.log(Math.ceil(10.1));  // 11
console.log(Math.ceil(10.9));  // 11
console.log(Math.ceil(-10.1)); // -10
```

**Math.round()**

Rounds to the nearest integer. Halfway values are rounded toward positive infinity.

```javascript
console.log(Math.round(10.4));  // 10
console.log(Math.round(10.5));  // 11
console.log(Math.round(10.9));  // 11
console.log(Math.round(-10.5)); // -10
```

**Comparison**

| Method | Behavior |
| --- | --- |
| `Math.trunc()` | Removes the fractional part |
| `Math.floor()` | Rounds toward negative infinity |
| `Math.ceil()` | Rounds toward positive infinity |
| `Math.round()` | Rounds to the nearest integer |

These methods return Number values. They do not validate whether the original input is a valid numeric string.

# 🗁 Converting Between Binary, Octal, Decimal, and Hexadecimal

JavaScript represents ordinary numbers internally using the Number type, but it supports parsing and formatting different numeral systems.

**Binary to decimal**

```javascript
const binary = "1010";

const decimal = parseInt(binary, 2);

console.log(decimal); // 10
```

**Hexadecimal to decimal**

```javascript
const hexadecimal = "FF";

const decimal = parseInt(hexadecimal, 16);

console.log(decimal); // 255
```

**Decimal to binary**

```javascript
const decimal = 10;

console.log(decimal.toString(2)); // "1010"
```

**Decimal to hexadecimal**

```javascript
const decimal = 255;

console.log(decimal.toString(16)); // "ff"
```

**Decimal to octal**

```javascript
const decimal = 64;

console.log(decimal.toString(8)); // "100"
```

# 🗁 Explicit Conversion vs Implicit Coercion

Explicit conversion is performed intentionally by developers, while implicit coercion happens automatically during JavaScript operations.

```javascript
// Explicit conversion
const explicitNumber = Number("100");

console.log(explicitNumber); // 100

// Implicit coercion
const implicitNumber = "100" - 0;

console.log(implicitNumber); // 100
```

| Feature | Explicit Conversion | Implicit Coercion |
| --- | --- | --- |
| Initiated by | Developer | JavaScript |
| Control | Direct | Depends on language rules |
| Readability | Usually clearer | Can be less obvious |
| Example | `Number("10")` | `"10" - 0` |
| Predictability | Easier to reason about | Requires understanding coercion rules |

Prefer explicit conversion when the intended data type matters.

# 🗁 Practical Examples

**Convert HTML form input into a number**

HTML input values are strings, including values from inputs with `type="number"`.

```javascript
const input = document.querySelector("#quantity");

const quantity = Number(input.value);

console.log(quantity);
```

**Validate numeric input**

```javascript
function parseQuantity(value) {
    const quantity = Number(value);

    if (!Number.isFinite(quantity)) {
        throw new TypeError("Quantity must be a finite number");
    }

    if (quantity < 0) {
        throw new RangeError("Quantity cannot be negative");
    }

    return quantity;
}

console.log(parseQuantity("25")); // 25
```

**Convert API data**

```javascript
const response = {
    id: "101",
    price: "99.99",
    available: "true"
};

const product = {
    id: Number(response.id),
    price: Number(response.price),
    available: response.available === "true"
};

console.log(product);
```

**Format a number as a string**

```javascript
const price = 1250;

const formattedPrice = String(price);

console.log(formattedPrice);        // "1250"
console.log(typeof formattedPrice); // "string"
```

**Convert a value to Boolean**

```javascript
function isValidValue(value) {
    return Boolean(value);
}

console.log(isValidValue("Hello")); // true
console.log(isValidValue(""));      // false
```

# 🗁 Common Mistakes

**Using parseInt() when decimal precision is required**

```javascript
console.log(parseInt("19.99", 10)); // 19
```

Use `parseFloat()` or `Number()` when decimals must be preserved.

```javascript
console.log(parseFloat("19.99")); // 19.99
console.log(Number("19.99"));     // 19.99
```

**Assuming Number() parses numeric prefixes**

```javascript
console.log(Number("100px")); // NaN
```

Use a parsing function only when extracting a numeric prefix is intentional.

```javascript
console.log(parseInt("100px", 10)); // 100
```

**Confusing empty strings with invalid numeric strings**

```javascript
console.log(Number(""));      // 0
console.log(Number("hello")); // NaN
```

If empty input should be rejected, validate it separately.

```javascript
function parseRequiredNumber(value) {
    if (typeof value === "string" && value.trim() === "") {
        throw new TypeError("A value is required");
    }

    const number = Number(value);

    if (!Number.isFinite(number)) {
        throw new TypeError("A valid finite number is required");
    }

    return number;
}
```

**Using Boolean() to parse textual Boolean values**

```javascript
console.log(Boolean("false")); // true
console.log(Boolean("true"));  // true
```

Both strings are non-empty, so both are truthy.

Parse textual Boolean values explicitly.

```javascript
function parseBoolean(value) {
    if (value === "true") return true;
    if (value === "false") return false;

    throw new TypeError("Expected 'true' or 'false'");
}

console.log(parseBoolean("true"));  // true
console.log(parseBoolean("false")); // false
```

**Losing precision when converting large integers**

```javascript
const value = Number("9007199254740993");

console.log(value); // 9007199254740992
```

Use BigInt when exact integer precision is required beyond the safe integer range.

```javascript
const value = BigInt("9007199254740993");

console.log(value); // 9007199254740993n
```

# 🗁 Best Practices

* Use `Number()` for explicit numeric conversion.
* Use `String()` for explicit string conversion.
* Use `Boolean()` when Boolean conversion is intended.
* Use `parseInt()` for integer-prefix parsing.
* Always specify the radix when using `parseInt()`.
* Use `parseFloat()` when decimal-prefix parsing is required.
* Validate user input before using converted values.
* Use `Number.isFinite()` to reject `NaN` and positive or negative infinity.
* Handle empty strings separately when they should not represent zero.
* Use `BigInt` for integers that exceed the safe integer range.
* Avoid converting already-imprecise Number values into BigInt when exact precision matters.
* Prefer explicit conversion over relying on implicit coercion in application logic.

# 🗁 Key Takeaways

* Type conversion changes a value from one data type to another.
* Explicit conversion is performed intentionally by developers.
* `String()` converts values into strings.
* `Number()` converts values into numbers.
* `Boolean()` converts values into Boolean values.
* `parseInt()` extracts an integer from a string.
* `parseFloat()` extracts a floating-point number from a string.
* `BigInt()` converts compatible values into BigInt values.
* Unary plus (`+`) provides shorthand numeric conversion.
* `Math.trunc()`, `Math.floor()`, `Math.ceil()`, and `Math.round()` provide different integer-rounding behaviors.
* `Number()`, `parseInt()`, and `parseFloat()` have different parsing rules.
* Proper conversion and validation make JavaScript applications more predictable and reliable.
