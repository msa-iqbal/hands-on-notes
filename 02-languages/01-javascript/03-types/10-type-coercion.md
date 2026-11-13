# JavaScript Type Coercion

**Type coercion** is the process by which JavaScript automatically converts a value from one data type to another during an operation or expression.

JavaScript is a dynamically typed language, so values of different types can participate in the same expression.

For example, JavaScript may convert a number into a string when using the addition operator with a string.

```javascript
const result = 10 + "5";

console.log(result);        // "105"
console.log(typeof result); // "string"
```

Here, JavaScript converts the number `10` into the string `"10"` before performing string concatenation.

Type coercion is an important part of understanding JavaScript operators, comparisons, conditions, and expressions.

# 🗁 Types of Type Coercion

JavaScript has two main forms of type conversion:

* **Implicit coercion:** JavaScript automatically converts values during an operation.
* **Explicit conversion:** Developers intentionally convert values using functions or operators.

```javascript
// Implicit coercion
console.log("10" + 5); // "105"

// Explicit conversion
console.log(Number("10") + 5); // 15
```

# 🗁 Implicit Type Coercion

Implicit coercion occurs when JavaScript automatically converts values to make an operation possible.

The conversion depends on the operator and the values involved.

```javascript
console.log("10" + 5);  // "105"
console.log("10" - 5);  // 5
console.log("10" * 5);  // 50
console.log("10" / 5);  // 2
```

The addition operator (`+`) performs string concatenation when one operand becomes a string.

The subtraction, multiplication, and division operators generally convert operands to numbers.

# 🗁 String Coercion

String coercion occurs when JavaScript converts a value into a string.

It commonly happens when using the addition operator with a string.

```javascript
console.log("Hello " + "World"); // "Hello World"
console.log("Age: " + 25);       // "Age: 25"
console.log("Active: " + true);  // "Active: true"
console.log("Value: " + null);   // "Value: null"
console.log("Value: " + undefined); // "Value: undefined"
```

JavaScript converts the non-string operands into strings before concatenation.

**String coercion with arrays**

Arrays are converted into strings using their string representation, which is based on `Array.prototype.join()`.

```javascript
console.log([1, 2, 3] + ""); // "1,2,3"
console.log(["a", "b"] + ""); // "a,b"
console.log([] + ""); // ""
```

**String coercion with objects**

Ordinary objects usually convert to the string `"[object Object]"`.

```javascript
console.log({ name: "John" } + "");
// "[object Object]"
```

This behavior can be customized using object conversion methods such as `Symbol.toPrimitive`.

# 🗁 Numeric Coercion

Numeric coercion occurs when JavaScript converts a value into a number.

It commonly happens with arithmetic operators other than addition.

```javascript
console.log("10" - 5); // 5
console.log("10" * 2); // 20
console.log("20" / 4); // 5
console.log("10" % 3); // 1
```

JavaScript also converts Boolean and null values into numbers during numeric operations.

```javascript
console.log(true * 10);  // 10
console.log(false * 10); // 0

console.log(null * 10); // 0
console.log(undefined * 10); // NaN
```

**Common numeric coercion results**

| Original Value | Numeric Result |
| --- | ---: |
| `"10"` | `10` |
| `""` | `0` |
| `" "` | `0` |
| `true` | `1` |
| `false` | `0` |
| `null` | `0` |
| `undefined` | `NaN` |
| `"hello"` | `NaN` |

```javascript
console.log("10" - 0); // 10
console.log("" - 0);   // 0
console.log(true - 0); // 1
console.log(null - 0); // 0
console.log(undefined - 0); // NaN
console.log("hello" - 0); // NaN
```

# 🗁 Boolean Coercion

Boolean coercion converts a value into `true` or `false`.

It commonly occurs in conditional statements and logical operations.

```javascript
if ("Hello") {
    console.log("This executes");
}

if (0) {
    console.log("This does not execute");
}
```

JavaScript uses truthiness rules to determine the Boolean interpretation of a value.

**Falsy values**

The following values are falsy:

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

Almost every other value is truthy.

```javascript
console.log(Boolean("false")); // true
console.log(Boolean("0"));     // true
console.log(Boolean([]));      // true
console.log(Boolean({}));      // true
console.log(Boolean(function () {})); // true
```

# 🗁 Addition Operator and Type Coercion

The addition operator (`+`) has two primary behaviors:

* Numeric addition.
* String concatenation.

When either primitive operand becomes a string, the operation performs string concatenation.

```javascript
console.log(10 + 20);       // 30
console.log("10" + 20);     // "1020"
console.log(10 + "20");     // "1020"
console.log("10" + "20");   // "1020"
```

**Left-to-right evaluation**

Multiple addition operations are evaluated from left to right.

```javascript
console.log(10 + 20 + "30");
// "3030"
```

Explanation:

```javascript
10 + 20       // 30
30 + "30"     // "3030"
```

Changing the order changes the result.

```javascript
console.log("10" + 20 + 30);
// "102030"
```

Explanation:

```javascript
"10" + 20       // "1020"
"1020" + 30     // "102030"
```

# 🗁 Subtraction Operator and Type Coercion

The subtraction operator (`-`) converts operands to numbers.

```javascript
console.log("20" - 10); // 10
console.log("20" - "5"); // 15
console.log(true - false); // 1
console.log(null - 5); // -5
```

If conversion fails, the result becomes `NaN`.

```javascript
console.log("hello" - 5); // NaN
```

# 🗁 Multiplication and Division Coercion

Multiplication (`*`) and division (`/`) also perform numeric coercion.

```javascript
console.log("10" * 5); // 50
console.log("20" / 4); // 5

console.log("10" * "2"); // 20
console.log("20" / "5"); // 4
```

Invalid numeric conversions produce `NaN`.

```javascript
console.log("hello" * 5); // NaN
console.log("hello" / 2); // NaN
```

# 🗁 Equality Coercion

JavaScript has two main equality operators:

* Loose equality (`==`)
* Strict equality (`===`)

They behave differently when comparing values of different types.

**Loose equality (`==`)**

Loose equality performs type coercion when comparing certain values.

```javascript
console.log(10 == "10");   // true
console.log(true == 1);    // true
console.log(false == 0);  // true
console.log(null == undefined); // true
```

**Strict equality (`===`)**

Strict equality does not perform type coercion.

```javascript
console.log(10 === "10");   // false
console.log(true === 1);    // false
console.log(false === 0);  // false
console.log(null === undefined); // false
```

**Comparison table**

| Expression | `==` | `===` |
| --- | --- | --- |
| `10` and `"10"` | `true` | `false` |
| `true` and `1` | `true` | `false` |
| `false` and `0` | `true` | `false` |
| `null` and `undefined` | `true` | `false` |
| `10` and `10` | `true` | `true` |
| `"hello"` and `"hello"` | `true` | `true` |

Strict equality is generally preferred because it avoids many unexpected coercion results.

# 🗁 Relational Comparison Coercion

Relational operators compare values using specific conversion rules.

The operators include:

* Greater than (`>`)
* Less than (`<`)
* Greater than or equal (`>=`)
* Less than or equal (`<=`)

When both operands are strings, JavaScript compares them lexicographically using their UTF-16 code units.

```javascript
console.log("apple" < "banana"); // true
console.log("20" < "100");       // false
```

When both operands are strings, their values are not converted to numbers.

When operands are not both strings, JavaScript generally converts them to primitives and then numbers for comparison.

```javascript
console.log("20" < 100); // true
console.log("5" > 3);    // true
console.log(true > false); // true
```

Invalid numeric conversion can produce `NaN`, making relational comparisons return `false`.

```javascript
console.log("hello" > 5);  // false
console.log("hello" < 5);  // false
console.log("hello" >= 5); // false
```

# 🗁 Logical Operator Coercion

Logical operators perform Boolean evaluation but do not necessarily return Boolean values.

The three main logical operators are:

* AND (`&&`)
* OR (`||`)
* NOT (`!`)

**Logical AND (`&&`)**

Returns the first falsy operand or the final operand if all operands are truthy.

```javascript
console.log(true && "Hello"); // "Hello"
console.log(false && "Hello"); // false

console.log(10 && 20); // 20
console.log(0 && 20);  // 0
```

**Logical OR (`||`)**

Returns the first truthy operand or the final operand if all operands are falsy.

```javascript
console.log(false || "Guest"); // "Guest"
console.log(true || "Guest");  // true

console.log(0 || 100); // 100
console.log(10 || 100); // 10
```

**Logical NOT (`!`)**

Always returns a Boolean value after converting the operand to its Boolean equivalent and inverting it.

```javascript
console.log(!true);  // false
console.log(!false); // true

console.log(!0);       // true
console.log(!"Hello"); // false
```

# 🗁 Loose Equality Conversion Rules

The loose equality operator (`==`) follows defined comparison rules rather than simply converting every value into the same type.

Some important examples:

**Number and string**

The string is converted to a number.

```javascript
console.log(10 == "10"); // true
console.log(0 == "");    // true
```

**Boolean and number**

The Boolean is converted to a number.

```javascript
console.log(true == 1);  // true
console.log(false == 0); // true
```

**Null and undefined**

These two values are loosely equal to each other.

```javascript
console.log(null == undefined); // true
```

**Null and other values**

`null` is not loosely equal to ordinary values such as zero or an empty string.

```javascript
console.log(null == 0);  // false
console.log(null == ""); // false
```

**Object and primitive**

When an object is compared with a primitive, JavaScript attempts to convert the object to a primitive value.

```javascript
console.log([10] == 10); // true
console.log([] == 0);    // true
console.log([1, 2] == "1,2"); // true
```

These conversions can be surprising, so strict equality is generally easier to reason about.

# 🗁 Object-to-Primitive Coercion

When JavaScript needs to convert an object into a primitive value, it follows the object-to-primitive conversion process.

The conversion can involve:

* `Symbol.toPrimitive`
* `valueOf()`
* `toString()`

The exact method and order depend on the conversion hint and the object's implementation.

**Using valueOf()**

```javascript
const user = {
    valueOf() {
        return 100;
    }
};

console.log(user + 50); // 150
```

**Using toString()**

```javascript
const user = {
    toString() {
        return "John";
    }
};

console.log(user + ""); // "John"
```

**Using Symbol.toPrimitive**

`Symbol.toPrimitive` provides explicit control over primitive conversion.

```javascript
const product = {
    name: "Laptop",
    price: 1000,

    [Symbol.toPrimitive](hint) {
        if (hint === "number") {
            return this.price;
        }

        if (hint === "string") {
            return this.name;
        }

        return `${this.name}: ${this.price}`;
    }
};

console.log(Number(product)); // 1000
console.log(String(product)); // "Laptop"
console.log(product + "");    // "Laptop: 1000"
```

# 🗁 Common Coercion Examples

Understanding these expressions helps avoid unexpected results.

```javascript
console.log([] + []);       // ""
console.log([] + {});       // "[object Object]"
console.log({} + []);       // "[object Object]" in an expression context
console.log([1, 2] + [3, 4]); // "1,23,4"

console.log("5" - 2);       // 3
console.log("5" + 2);       // "52"

console.log(true + true);   // 2
console.log(false + 10);    // 10

console.log(null + 1);      // 1
console.log(undefined + 1); // NaN
```

The `{}` expression at the beginning of a statement can be interpreted as a block rather than an object literal. Use parentheses when you specifically want an object expression.

```javascript
console.log(({}) + []); // "[object Object]"
```

# 🗁 Practical Examples

**Handling form input**

HTML form input values are generally strings, even when the user enters numeric characters.

```javascript
const inputValue = "25";

const age = Number(inputValue);

console.log(age);        // 25
console.log(typeof age); // "number"
```

**Calculating a total price**

```javascript
const price = "100";
const quantity = "3";

const total = Number(price) * Number(quantity);

console.log(total); // 300
```

**Providing fallback values**

```javascript
const username = "";

const displayName = username || "Guest";

console.log(displayName); // "Guest"
```

When zero or an empty string is a valid value, use nullish coalescing instead.

```javascript
const quantity = 0;

const result = quantity ?? 10;

console.log(result); // 0
```

**Validating user input**

```javascript
function calculateDiscount(price, discount) {
    const numericPrice = Number(price);
    const numericDiscount = Number(discount);

    if (
        !Number.isFinite(numericPrice) ||
        !Number.isFinite(numericDiscount)
    ) {
        throw new TypeError("Price and discount must be valid numbers");
    }

    return numericPrice - (numericPrice * numericDiscount / 100);
}

console.log(calculateDiscount("100", "10")); // 90
```

# 🗁 Common Mistakes

**Assuming addition always performs numeric addition**

```javascript
console.log("10" + 5); // "105"
```

Convert values explicitly when numeric addition is intended.

```javascript
console.log(Number("10") + 5); // 15
```

**Using loose equality without understanding conversion**

```javascript
console.log(false == 0); // true
console.log("" == 0);    // true
```

Prefer strict equality in most application code.

```javascript
console.log(false === 0); // false
console.log("" === 0);    // false
```

**Assuming every falsy value is null or undefined**

```javascript
console.log(Boolean(0));     // false
console.log(Boolean(""));    // false
console.log(Boolean(false)); // false
```

Use the nullish coalescing operator when only `null` and `undefined` should trigger a fallback.

**Forgetting that form inputs return strings**

```javascript
const quantity = "5";

console.log(quantity + 1); // "51"
```

Convert the input before performing arithmetic.

```javascript
console.log(Number(quantity) + 1); // 6
```

**Assuming logical operators always return Boolean values**

```javascript
console.log("Hello" && 100); // 100
console.log("" || "Guest");  // "Guest"
```

Logical AND and OR return operands, not necessarily Boolean values.

# 🗁 Best Practices

* Understand implicit coercion before relying on it.
* Prefer explicit conversion when working with external input.
* Use `Number()` when converting values into numbers.
* Use `String()` when an explicit string representation is required.
* Use `Boolean()` when explicit Boolean conversion improves clarity.
* Prefer strict equality (`===`) over loose equality (`==`) in most situations.
* Use `??` when only `null` and `undefined` should trigger fallback values.
* Remember that `+` can perform either addition or string concatenation.
* Validate numeric input with `Number.isFinite()` when finite numbers are required.
* Be especially careful when combining objects, arrays, and primitives in expressions.
* Avoid depending on complicated implicit coercion in application logic.

# 🗁 Key Takeaways

* Type coercion converts values from one data type to another.
* Implicit coercion happens automatically during operations.
* Explicit conversion is performed intentionally by developers.
* The addition operator can perform numeric addition or string concatenation.
* Arithmetic operators generally perform numeric coercion.
* Boolean coercion follows JavaScript truthiness rules.
* Loose equality (`==`) performs certain type conversions.
* Strict equality (`===`) does not perform type coercion.
* Logical AND and OR return operands rather than necessarily returning Boolean values.
* Objects can define primitive conversion behavior using `Symbol.toPrimitive`, `valueOf()`, and `toString()`.
* Understanding coercion helps prevent unexpected results and makes JavaScript code more predictable.
