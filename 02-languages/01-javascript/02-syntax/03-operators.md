# JavaScript Operators

JavaScript operators are special symbols or keywords that perform operations on values and variables.

Operators are used for mathematical calculations, value assignments, comparisons, logical decisions, string manipulation, and many other programming tasks.

Understanding JavaScript operators is essential for writing expressions, implementing application logic, and controlling program behavior.

# 🗁 What Are Operators?

An operator performs an operation on one or more operands.

An **operand** is a value or expression on which an operator operates.

Example:

```javascript
const result = 10 + 20;
```

In this example:

* `10` is the first operand.
* `20` is the second operand.
* `+` is the addition operator.
* `10 + 20` is an expression.
* `30` is the resulting value.

# 🗁 Types of JavaScript Operators

JavaScript provides several categories of operators.

| Operator category | Purpose | Examples |
| --- | --- | --- |
| Arithmetic | Perform mathematical operations | `+`, `-`, `*`, `/` |
| Assignment | Assign values to variables | `=`, `+=`, `-=` |
| Comparison | Compare values | `===`, `!==`, `>` |
| Logical | Combine or invert conditions | `&&`, `\|\|`, `!` |
| Bitwise | Perform operations on binary representations | `&`, `\|`, `^` |
| String | Concatenate strings | `+`, `+=` |
| Conditional | Select a value based on a condition | `?:` |
| Unary | Operate on a single operand | `typeof`, `!`, `-` |
| Relational | Check relationships between values | `in`, `instanceof` |
| Nullish coalescing | Provide a fallback for nullish values | `??` |
| Optional chaining | Safely access properties and methods | `?.` |
| Spread | Expand iterable or object properties | `...` |
| Comma | Evaluate multiple expressions | `,` |

# 🗁 Arithmetic Operators

Arithmetic operators perform mathematical calculations.

JavaScript supports the following arithmetic operators:

| Operator | Name | Description | Example | Result |
| --- | --- | --- | --- | --- |
| `+` | Addition | Adds values | `10 + 5` | `15` |
| `-` | Subtraction | Subtracts values | `10 - 5` | `5` |
| `*` | Multiplication | Multiplies values | `10 * 5` | `50` |
| `/` | Division | Divides values | `10 / 5` | `2` |
| `%` | Remainder | Returns the remainder | `10 % 3` | `1` |
| `**` | Exponentiation | Raises a number to a power | `2 ** 3` | `8` |

Example:

```javascript
const a = 10;
const b = 3;

console.log(a + b);
console.log(a - b);
console.log(a * b);
console.log(a / b);
console.log(a % b);
console.log(a ** b);
```

Output:

```text
13
7
30
3.3333333333333335
1
1000
```

**Important:** JavaScript division by zero does not throw an exception for ordinary numbers.

```javascript
console.log(10 / 0);
console.log(-10 / 0);
console.log(0 / 0);
```

Output:

```text
Infinity
-Infinity
NaN
```

The remainder operator (`%`) returns the remainder of division, not a mathematical modulo result in every sign combination.

```javascript
console.log(10 % 3);
console.log(-10 % 3);
console.log(10 % -3);
```

Output:

```text
1
-1
1
```

# 🗁 Increment and Decrement Operators

Increment and decrement operators increase or decrease a numeric operand by one.

| Operator | Name | Operation |
| --- | --- | --- |
| `++` | Increment | Adds 1 |
| `--` | Decrement | Subtracts 1 |

**Prefix increment**

The operator appears before the operand. The value is incremented before the expression returns it.

```javascript
let count = 5;

console.log(++count);
```

Output:

```text
6
```

**Postfix increment**

The operator appears after the operand. The expression returns the original value before incrementing it.

```javascript
let count = 5;

console.log(count++);
console.log(count);
```

Output:

```text
5
6
```

**Prefix decrement**

```javascript
let count = 5;

console.log(--count);
```

Output:

```text
4
```

**Postfix decrement**

```javascript
let count = 5;

console.log(count--);
console.log(count);
```

Output:

```text
5
4
```

Increment and decrement operators can also perform numeric coercion on suitable operands.

```javascript
let value = "10";

value++;

console.log(value);
console.log(typeof value);
```

Output:

```text
11
number
```

# 🗁 Assignment Operators

Assignment operators assign values to variables or other valid assignment targets.

**Basic assignment**

The assignment operator (`=`) assigns a value.

```javascript
let age = 25;

console.log(age);
```

Output:

```text
25
```

**Compound assignment operators**

Compound assignment operators combine an operation with assignment.

| Operator | Equivalent expression |
| --- | --- |
| `+=` | `x = x + value` |
| `-=` | `x = x - value` |
| `*=` | `x = x * value` |
| `/=` | `x = x / value` |
| `%=` | `x = x % value` |
| `**=` | `x = x ** value` |
| `<<=` | `x = x << value` |
| `>>=` | `x = x >> value` |
| `>>>=` | `x = x >>> value` |
| `&=` | `x = x & value` |
| `\|=` | `x = x \| value` |
| `^=` | `x = x ^ value` |
| `&&=` | `x = x && value` |
| `\|\|=` | `x = x \|\| value` |
| `??=` | `x = x ?? value` |

Example:

```javascript
let score = 10;

score += 5;
console.log(score);

score -= 2;
console.log(score);

score *= 3;
console.log(score);

score /= 2;
console.log(score);
```

Output:

```text
15
13
39
19.5
```

**Logical assignment operators**

These operators assign values conditionally.

```javascript
let username = "";

username ||= "Guest";

console.log(username);
```

Output:

```text
Guest
```

The `||=` operator assigns the right-hand value only when the left-hand value is falsy.

```javascript
let settings = null;

settings ??= {};

console.log(settings);
```

Output:

```text
{}
```

The `??=` operator assigns the right-hand value only when the left-hand value is `null` or `undefined`.

# 🗁 Comparison Operators

Comparison operators compare two values and return a Boolean result.

| Operator | Name | Description |
| --- | --- | --- |
| `==` | Loose equality | Compares after possible type coercion |
| `===` | Strict equality | Compares without type coercion |
| `!=` | Loose inequality | Checks inequality after possible coercion |
| `!==` | Strict inequality | Checks inequality without type coercion |
| `>` | Greater than | Checks whether the left value is greater |
| `<` | Less than | Checks whether the left value is smaller |
| `>=` | Greater than or equal | Checks whether the left value is greater or equal |
| `<=` | Less than or equal | Checks whether the left value is smaller or equal |

**Loose equality (`==`)**

Loose equality allows type coercion before comparison.

```javascript
console.log(10 == "10");
console.log(0 == false);
console.log(null == undefined);
```

Output:

```text
true
true
true
```

**Strict equality (`===`)**

Strict equality compares values without performing type coercion.

```javascript
console.log(10 === "10");
console.log(0 === false);
console.log(10 === 10);
```

Output:

```text
false
false
true
```

**Inequality operators**

```javascript
console.log(10 != "10");
console.log(10 !== "10");
```

Output:

```text
false
true
```

**Relational comparisons**

```javascript
console.log(20 > 10);
console.log(20 < 10);
console.log(20 >= 20);
console.log(10 <= 20);
```

Output:

```text
true
false
true
true
```

Prefer strict equality (`===` and `!==`) for predictable comparisons unless coercion is specifically required.

# 🗁 Logical Operators

Logical operators combine conditions or invert Boolean values.

| Operator | Name | Description |
| --- | --- | --- |
| `&&` | Logical AND | Returns the first falsy operand or the last operand |
| `\|\|` | Logical OR | Returns the first truthy operand or the last operand |
| `!` | Logical NOT | Converts the operand to Boolean and negates it |

**Logical AND (`&&`)**

Returns the first falsy operand. If all operands are truthy, it returns the last operand.

```javascript
const age = 25;
const hasLicense = true;

const canDrive = age >= 18 && hasLicense;

console.log(canDrive);
```

Output:

```text
true
```

**Logical OR (`||`)**

Returns the first truthy operand. If all operands are falsy, it returns the last operand.

```javascript
const username = "";

const displayName = username || "Guest";

console.log(displayName);
```

Output:

```text
Guest
```

**Logical NOT (`!`)**

Converts the operand to a Boolean value and returns its opposite.

```javascript
console.log(!true);
console.log(!false);
console.log(!0);
console.log(!"Hello");
```

Output:

```text
false
true
true
false
```

**Double NOT (`!!`)**

Double negation converts a value into its Boolean equivalent.

```javascript
console.log(!!"JavaScript");
console.log(!!0);
console.log(!!null);
```

Output:

```text
true
false
false
```

# 🗁 Bitwise Operators

Bitwise operators perform operations on the binary representations of numbers.

For ordinary JavaScript `Number` operands, bitwise operations convert values to 32-bit integers before performing the operation.

| Operator | Name | Description |
| --- | --- | --- |
| `&` | AND | Sets bits that are set in both operands |
| `\|` | OR | Sets bits that are set in either operand |
| `^` | XOR | Sets bits that differ between operands |
| `~` | NOT | Inverts all bits |
| `<<` | Left shift | Shifts bits to the left |
| `>>` | Sign-propagating right shift | Shifts bits right while preserving the sign bit |
| `>>>` | Zero-fill right shift | Shifts bits right while filling with zeros |

**Bitwise AND (`&`)**

```javascript
console.log(5 & 3);
```

Binary representation:

```text
5 = 0101
3 = 0011
---------
    0001
```

Output:

```text
1
```

**Bitwise OR (`|`)**

```javascript
console.log(5 | 3);
```

Output:

```text
7
```

**Bitwise XOR (`^`)**

```javascript
console.log(5 ^ 3);
```

Output:

```text
6
```

**Bitwise NOT (`~`)**

```javascript
console.log(~5);
```

Output:

```text
-6
```

**Left shift (`<<`)**

```javascript
console.log(5 << 1);
```

Output:

```text
10
```

**Sign-propagating right shift (`>>`)**

```javascript
console.log(8 >> 1);
```

Output:

```text
4
```

**Zero-fill right shift (`>>>`)**

```javascript
console.log(8 >>> 1);
```

Output:

```text
4
```

Bitwise operators are useful for low-level operations, flags, masks, and compact numeric representations.

# 🗁 String Operators

JavaScript uses the addition operator (`+`) to concatenate strings.

Example:

```javascript
const firstName = "Muhammad";
const lastName = "Iqbal";

const fullName = firstName + " " + lastName;

console.log(fullName);
```

Output:

```text
Muhammad Iqbal
```

The addition assignment operator (`+=`) can also concatenate strings.

```javascript
let message = "Hello";

message += " JavaScript";

console.log(message);
```

Output:

```text
Hello JavaScript
```

When `+` is used with strings and other values, JavaScript may perform string conversion.

```javascript
console.log("Age: " + 25);
console.log("Total: " + (10 + 20));
```

Output:

```text
Age: 25
Total: 30
```

The `+` operator is overloaded: depending on its operands, it performs numeric addition or string concatenation.

# 🗁 Conditional Operator

The conditional operator (`?:`) is also called the ternary operator because it uses three operands.

Syntax:

```javascript
condition ? expressionIfTrue : expressionIfFalse;
```

Example:

```javascript
const age = 20;

const status = age >= 18 ? "Adult" : "Minor";

console.log(status);
```

Output:

```text
Adult
```

It is useful for selecting a value based on a simple condition.

Nested ternary expressions are supported, but complex conditions are often clearer with `if...else`.

# 🗁 Unary Operators

Unary operators operate on a single operand.

| Operator | Description | Example |
| --- | --- | --- |
| `+` | Converts an operand to a number | `+"10"` |
| `-` | Negates a numeric value | `-10` |
| `!` | Logical negation | `!true` |
| `~` | Bitwise negation | `~5` |
| `typeof` | Returns the type of a value | `typeof "Hello"` |
| `void` | Evaluates an expression and returns `undefined` | `void 0` |
| `delete` | Removes an object's own property | `delete user.name` |
| `++` | Increments a value | `++count` |
| `--` | Decrements a value | `--count` |

Example:

```javascript
const value = "100";

console.log(+value);
console.log(-value);
console.log(typeof value);
console.log(void 0);
```

Output:

```text
100
-100
string
undefined
```

# 🗁 Relational Operators

Relational operators test relationships between values or objects.

**The `in` operator**

Checks whether a property exists in an object or anywhere in its prototype chain.

```javascript
const user = {
    name: "John",
    age: 25
};

console.log("name" in user);
console.log("email" in user);
```

Output:

```text
true
false
```

The `in` operator also works with arrays, where it checks for an index or inherited property rather than checking an element's value.

```javascript
const numbers = [10, 20, 30];

console.log(0 in numbers);
console.log(3 in numbers);
```

Output:

```text
true
false
```

**The `instanceof` operator**

Checks whether an object's prototype chain contains the prototype of a specified constructor.

```javascript
const date = new Date();

console.log(date instanceof Date);
console.log(date instanceof Object);
```

Output:

```text
true
true
```

# 🗁 Nullish Coalescing Operator

The nullish coalescing operator (`??`) returns the right-hand operand only when the left-hand operand is `null` or `undefined`.

Example:

```javascript
const username = null;

const displayName = username ?? "Guest";

console.log(displayName);
```

Output:

```text
Guest
```

Unlike the logical OR operator (`||`), nullish coalescing preserves valid falsy values such as `0`, `false`, and an empty string.

```javascript
console.log(0 || 100);
console.log(0 ?? 100);

console.log(false || true);
console.log(false ?? true);

console.log("" || "Default");
console.log("" ?? "Default");
```

Output:

```text
100
0
true
false
Default

```

Use `??` when a fallback should apply only to missing or nullish values.

# 🗁 Optional Chaining Operator

The optional chaining operator (`?.`) safely accesses properties, methods, or elements when a reference may be `null` or `undefined`.

Example:

```javascript
const user = {
    profile: {
        name: "Muhammad"
    }
};

console.log(user.profile?.name);
console.log(user.address?.city);
```

Output:

```text
Muhammad
undefined
```

**Optional method calls**

```javascript
const user = {
    greet() {
        return "Hello!";
    }
};

console.log(user.greet?.());
console.log(user.sayGoodbye?.());
```

Output:

```text
Hello!
undefined
```

**Optional array access**

```javascript
const users = ["John", "Jane"];

console.log(users?.[0]);

const emptyUsers = null;

console.log(emptyUsers?.[0]);
```

Output:

```text
John
undefined
```

Optional chaining prevents errors when accessing properties through a nullish reference. It does not make every possible property access safe: for example, calling a property that exists but is not a function can still throw a `TypeError`.

# 🗁 Spread Syntax

Spread syntax (`...`) expands an iterable or an object's enumerable own properties into another context.

**Spread with arrays**

```javascript
const first = [10, 20, 30];

const second = [...first, 40, 50];

console.log(second);
```

Output:

```text
[10, 20, 30, 40, 50]
```

**Spread with objects**

```javascript
const user = {
    name: "John",
    age: 25
};

const updatedUser = {
    ...user,
    age: 26
};

console.log(updatedUser);
```

Output:

```text
{ name: 'John', age: 26 }
```

**Spread in function calls**

```javascript
const numbers = [10, 20, 30];

console.log(Math.max(...numbers));
```

Output:

```text
30
```

Spread syntax creates shallow copies when used to copy arrays or objects. Nested objects remain shared references unless copied separately.

# 🗁 Comma Operator

The comma operator (` , `) evaluates multiple expressions from left to right and returns the value of the final expression.

Example:

```javascript
const result = (10 + 5, 20 + 5, 30 + 5);

console.log(result);
```

Output:

```text
35
```

The earlier expressions are evaluated, but their results are discarded.

The comma operator is different from commas used as separators in function arguments, array elements, or object properties.

Example:

```javascript
let a = 1;
let b = 2;

const result = (a++, b++, a + b);

console.log(result);
```

Output:

```text
5
```

# 🗁 Operator Precedence

Operator precedence determines the order in which operators are evaluated within an expression.

Operators with higher precedence are evaluated before operators with lower precedence, subject to associativity and grouping parentheses.

Example:

```javascript
const result = 10 + 5 * 2;

console.log(result);
```

Output:

```text
20
```

Multiplication has higher precedence than addition.

Using parentheses changes the evaluation order:

```javascript
const result = (10 + 5) * 2;

console.log(result);
```

Output:

```text
30
```

**Common operator precedence**

The following table lists common operators from higher to lower precedence.

| Precedence | Operators | Description |
| --- | --- | --- |
| Highest | `()` | Grouping |
| 17 | `.` `[]` `?.` | Member access |
| 16 | `new` (with arguments), function call `()` | Construction and calls |
| 15 | `++` `--` (postfix) | Postfix increment and decrement |
| 14 | `!` `~` `+` `-` `typeof` `void` `delete` | Unary operators |
| 13 | `**` | Exponentiation |
| 12 | `*` `/` `%` | Multiplication, division, remainder |
| 11 | `+` `-` | Addition and subtraction |
| 10 | `<<` `>>` `>>>` | Bitwise shifts |
| 9 | `<` `<=` `>` `>=` `in` `instanceof` | Relational |
| 8 | `==` `!=` `===` `!==` | Equality |
| 7 | `&` | Bitwise AND |
| 6 | `^` | Bitwise XOR |
| 5 | `\|` | Bitwise OR |
| 4 | `&&` | Logical AND |
| 3 | `\|\|` `??` | Logical OR and nullish coalescing |
| 2 | `?:` | Conditional |
| 2 | `=` `+=` `-=` and other assignments | Assignment |
| 1 | `,` | Comma |

This is a practical reference for common operators rather than a complete grammar table. Some syntax has special parsing restrictions, and parentheses should be used when evaluation order is important.

# 🗁 Operator Associativity

Associativity determines the evaluation grouping when operators have the same precedence.

**Left-to-right associativity**

Most arithmetic operators are left-associative.

```javascript
const result = 20 - 5 - 3;

console.log(result);
```

Evaluation:

```text
(20 - 5) - 3
15 - 3
12
```

**Right-to-left associativity**

Assignment and exponentiation operators are right-associative.

```javascript
let a;
let b;
let c;

a = b = c = 100;

console.log(a, b, c);
```

Output:

```text
100 100 100
```

Exponentiation example:

```javascript
console.log(2 ** 3 ** 2);
```

Output:

```text
512
```

Evaluation:

```text
2 ** (3 ** 2)
2 ** 9
512
```

# 🗁 Short-Circuit Evaluation

Short-circuit evaluation means JavaScript sometimes stops evaluating an expression before reaching all operands.

This behavior is common with logical AND (`&&`), logical OR (`||`), and nullish coalescing (`??`).

**Logical AND short-circuiting**

If the first operand is falsy, the second operand is not evaluated.

```javascript
function check() {
    console.log("Function executed");

    return true;
}

console.log(false && check());
```

Output:

```text
false
```

The function is never executed.

**Logical OR short-circuiting**

If the first operand is truthy, the second operand is not evaluated.

```javascript
function check() {
    console.log("Function executed");

    return true;
}

console.log(true || check());
```

Output:

```text
true
```

**Nullish coalescing short-circuiting**

If the left operand is not nullish, the right operand is not evaluated.

```javascript
function getDefault() {
    console.log("Default executed");

    return "Guest";
}

const username = "John";

console.log(username ?? getDefault());
```

Output:

```text
John
```

Short-circuit evaluation is useful for conditional execution and fallback values.

# 🗁 Operator Overloading and Type Coercion

JavaScript operators can behave differently depending on operand types.

The addition operator (`+`) supports both numeric addition and string concatenation.

```javascript
console.log(10 + 20);
console.log("10" + 20);
```

Output:

```text
30
1020
```

Other arithmetic operators generally convert suitable operands to numbers.

```javascript
console.log("10" - 5);
console.log("10" * 2);
console.log("10" / 2);
```

Output:

```text
5
20
5
```

Some operators also behave differently for special values such as `null`, `undefined`, and `NaN`.

Understanding type coercion is important when working with JavaScript operators.

# 🗁 Practical Example

The following example combines several operator categories to calculate a student's result.

```javascript
const student = {
    name: "John",
    marks: 85,
    attendance: 90,
    isRegistered: true
};

const passingMarks = 40;

const hasPassed = student.marks >= passingMarks;

const hasGoodAttendance = student.attendance >= 75;

const isEligible =
    hasPassed &&
    hasGoodAttendance &&
    student.isRegistered;

const grade =
    student.marks >= 80
        ? "A"
        : student.marks >= 60
            ? "B"
            : student.marks >= 40
                ? "C"
                : "F";

const result = {
    name: student.name,
    marks: student.marks,
    grade,
    isEligible
};

console.log(result);
```

Output:

```text
{
    name: 'John',
    marks: 85,
    grade: 'A',
    isEligible: true
}
```

This example demonstrates:

* Comparison operators
* Logical AND
* Conditional operators
* Object property access
* Assignment expressions

# 🗁 Best Practices

* Prefer strict equality (`===` and `!==`) for predictable comparisons.
* Use parentheses to make complex expressions easier to understand.
* Avoid unnecessary nested ternary expressions.
* Understand the difference between `||` and `??`.
* Use optional chaining when accessing potentially missing properties.
* Avoid mixing multiple operations into one complicated expression.
* Be careful with increment and decrement operators inside larger expressions.
* Understand type coercion when combining different data types.
* Use bitwise operators only when their binary behavior is intended.
* Keep operator usage consistent throughout a project.

# 🗁 Key Takeaways

* Operators perform operations on operands.
* Arithmetic operators perform mathematical calculations.
* Assignment operators assign or update values.
* Comparison operators produce Boolean results.
* Logical operators combine conditions and support short-circuit evaluation.
* Bitwise operators work with binary representations.
* The conditional operator selects between two expressions.
* Unary operators operate on a single operand.
* Relational operators include `in` and `instanceof`.
* Nullish coalescing provides fallbacks for `null` and `undefined`.
* Optional chaining safely accesses potentially missing properties.
* Spread syntax expands iterable values and object properties.
* Operator precedence and associativity determine expression evaluation order.
* Type coercion can affect how operators behave.
