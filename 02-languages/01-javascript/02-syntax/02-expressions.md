# JavaScript Expressions

JavaScript expressions are combinations of values, variables, operators, and function calls that produce a value when evaluated.

Expressions are fundamental to JavaScript because they are used in variable assignments, calculations, conditions, function arguments, return values, and many other parts of a program.

Understanding expressions helps developers write more predictable, readable, and maintainable JavaScript code.

# 🗁 What Is an Expression?

An expression is any valid piece of JavaScript code that evaluates to a value.

Example:

```javascript
10 + 20;
```

The expression evaluates to:

```javascript
30;
```

Another example:

```javascript
const result = 10 + 20;
```

Here:

* `10 + 20` is an expression.
* The expression produces `30`.
* The assignment statement stores that value in `result`.

Expressions can be simple or complex.

```javascript
const name = "Muhammad";

const age = 25;

const isAdult = age >= 18;

const message = `Hello, ${name}`;

const total = (10 + 20) * 2;
```

# 🗁 Types of JavaScript Expressions

JavaScript supports several categories of expressions.

| Expression type | Purpose | Example |
| --- | --- | --- |
| Literal expressions | Represent fixed values | `100`, `"Hello"`, `true` |
| Identifier expressions | Access variables or bindings | `username` |
| Arithmetic expressions | Perform mathematical operations | `10 + 20` |
| Assignment expressions | Assign values | `age = 25` |
| Comparison expressions | Compare values | `age >= 18` |
| Logical expressions | Combine or invert conditions | `isActive && isVerified` |
| Conditional expressions | Select a value based on a condition | `age >= 18 ? "Adult" : "Minor"` |
| Function expressions | Define or evaluate functions | `function() {}` |
| Arrow function expressions | Define functions using arrow syntax | `() => 10` |
| Object expressions | Create objects | `{ name: "John" }` |
| Array expressions | Create arrays | `[10, 20, 30]` |
| Template literal expressions | Construct strings dynamically | `` `Hello ${name}` `` |
| Member expressions | Access object properties | `user.name` |
| Call expressions | Invoke functions | `greet()` |
| New expressions | Create instances | `new Date()` |

# 🗁 Literal Expressions

Literal expressions represent values directly in JavaScript code.

Examples:

```javascript
100;

25.5;

"Hello World";

'JavaScript';

true;

false;

null;
```

JavaScript also supports array and object literals.

```javascript
const numbers = [10, 20, 30];

const user = {
    name: "John",
    age: 25
};
```

Literal expressions provide values without requiring an existing variable.

# 🗁 Identifier Expressions

An identifier expression references a variable, constant, function, or another accessible binding.

Example:

```javascript
const username = "Muhammad";

username;
```

The identifier `username` evaluates to:

```text
Muhammad
```

Identifiers can also participate in larger expressions.

```javascript
const price = 100;
const quantity = 3;

const total = price * quantity;
```

Here, `price` and `quantity` are identifier expressions.

# 🗁 Arithmetic Expressions

Arithmetic expressions perform mathematical calculations.

JavaScript supports operators such as:

* Addition (`+`)
* Subtraction (`-`)
* Multiplication (`*`)
* Division (`/`)
* Remainder (`%`)
* Exponentiation (`**`)

Example:

```javascript
const addition = 10 + 5;
const subtraction = 10 - 5;
const multiplication = 10 * 5;
const division = 10 / 5;
const remainder = 10 % 3;
const exponentiation = 2 ** 3;

console.log(addition);
console.log(subtraction);
console.log(multiplication);
console.log(division);
console.log(remainder);
console.log(exponentiation);
```

Output:

```text
15
5
50
2
1
8
```

# 🗁 Assignment Expressions

Assignment expressions assign values to variables or object properties.

Example:

```javascript
let age = 20;

age = 25;

console.log(age);
```

Output:

```text
25
```

The assignment operator (`=`) evaluates to the assigned value.

```javascript
let first;
let second;

first = second = 100;

console.log(first);
console.log(second);
```

Output:

```text
100
100
```

JavaScript also supports compound assignment expressions.

```javascript
let score = 10;

score += 5;
score -= 2;
score *= 3;
score /= 2;

console.log(score);
```

Output:

```text
19.5
```

# 🗁 Comparison Expressions

Comparison expressions compare two values and produce a Boolean result.

Examples:

```javascript
const a = 10;
const b = 20;

console.log(a > b);
console.log(a < b);
console.log(a === b);
console.log(a !== b);
console.log(a >= 10);
console.log(b <= 20);
```

Output:

```text
false
true
false
true
true
true
```

Comparison expressions are frequently used in conditional statements and loops.

# 🗁 Logical Expressions

Logical expressions combine or invert Boolean conditions.

JavaScript provides three primary logical operators:

* Logical AND (`&&`)
* Logical OR (`||`)
* Logical NOT (`!`)

Example:

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

Logical operators can also return non-Boolean values because JavaScript uses short-circuit evaluation.

```javascript
const username = "";

const displayName = username || "Guest";

console.log(displayName);
```

Output:

```text
Guest
```

The `||` operator returns the first truthy operand or the last operand if none is truthy.

# 🗁 Conditional Expressions

The conditional operator (`?:`) evaluates a condition and returns one of two values.

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

Conditional expressions are useful when a simple decision needs to produce a value.

Nested conditional expressions are possible, but excessive nesting can reduce readability.

# 🗁 Function Expressions

A function expression creates a function and produces that function as a value.

Example:

```javascript
const greet = function () {
    return "Hello, JavaScript!";
};

console.log(greet());
```

Output:

```text
Hello, JavaScript!
```

Unlike a function declaration, a function expression can be assigned to a variable, passed as an argument, or returned from another function.

# 🗁 Arrow Function Expressions

Arrow functions provide a concise syntax for creating function expressions.

Example:

```javascript
const add = (a, b) => {
    return a + b;
};

console.log(add(10, 20));
```

Output:

```text
30
```

For a single expression, an arrow function can use an implicit return.

```javascript
const multiply = (a, b) => a * b;

console.log(multiply(5, 4));
```

Output:

```text
20
```

Arrow functions are expressions and can be used wherever a function value is expected.

# 🗁 Object Expressions

Object expressions create object values using object literal syntax.

Example:

```javascript
const user = {
    name: "Muhammad",
    age: 25,
    isActive: true
};

console.log(user.name);
```

Output:

```text
Muhammad
```

Objects can contain nested expressions, including function expressions.

```javascript
const calculator = {
    add: (a, b) => a + b,
    subtract: (a, b) => a - b
};

console.log(calculator.add(10, 5));
```

Output:

```text
15
```

# 🗁 Array Expressions

Array expressions create arrays using array literal syntax.

Example:

```javascript
const numbers = [10, 20, 30, 40];

console.log(numbers[0]);
```

Output:

```text
10
```

Array elements can contain any valid expressions.

```javascript
const a = 10;
const b = 20;

const values = [
    a,
    b,
    a + b,
    a * b
];

console.log(values);
```

Output:

```text
[10, 20, 30, 200]
```

# 🗁 Template Literal Expressions

Template literals allow expressions to be embedded inside strings using `${}`.

Example:

```javascript
const name = "Muhammad";
const age = 25;

const message = `My name is ${name} and I am ${age} years old.`;

console.log(message);
```

Output:

```text
My name is Muhammad and I am 25 years old.
```

Expressions inside template literals are evaluated before the resulting string is created.

```javascript
const price = 100;
const quantity = 3;

const message = `Total: ${price * quantity}`;

console.log(message);
```

Output:

```text
Total: 300
```

# 🗁 Member Expressions

Member expressions access properties of objects or arrays.

JavaScript supports dot notation and bracket notation.

**Dot notation**

```javascript
const user = {
    name: "John",
    age: 25
};

console.log(user.name);
```

**Bracket notation**

```javascript
const user = {
    name: "John",
    age: 25
};

console.log(user["age"]);
```

Bracket notation is especially useful when the property name is stored in a variable.

```javascript
const property = "name";

console.log(user[property]);
```

# 🗁 Call Expressions

Call expressions invoke functions or methods.

Example:

```javascript
function greet() {
    return "Hello!";
}

const message = greet();

console.log(message);
```

Output:

```text
Hello!
```

Method calls are also call expressions.

```javascript
const numbers = [10, 20, 30];

numbers.push(40);

console.log(numbers);
```

Output:

```text
[10, 20, 30, 40]
```

# 🗁 New Expressions

A `new` expression creates an instance of a constructor or class.

Example:

```javascript
const currentDate = new Date();

console.log(currentDate);
```

Another example:

```javascript
class User {
    constructor(name) {
        this.name = name;
    }
}

const user = new User("Muhammad");

console.log(user.name);
```

Output:

```text
Muhammad
```

# 🗁 Expressions Inside Other Expressions

Expressions can be combined and nested to create more complex calculations.

Example:

```javascript
const price = 100;
const quantity = 3;
const discount = 20;

const total = (price * quantity) - discount;

console.log(total);
```

Output:

```text
280
```

The multiplication expression is evaluated before subtraction because of operator precedence.

Parentheses can make evaluation order explicit.

```javascript
const result = (10 + 5) * (20 - 10);

console.log(result);
```

Output:

```text
150
```

# 🗁 Expression Evaluation

When JavaScript evaluates an expression, it computes a result according to the language's rules.

Consider:

```javascript
const result = 10 + 20 * 2;
```

JavaScript evaluates multiplication before addition.

```text
20 * 2 = 40
10 + 40 = 50
```

Therefore:

```javascript
console.log(result);
```

Output:

```text
50
```

Parentheses can change the evaluation order.

```javascript
const result = (10 + 20) * 2;

console.log(result);
```

Output:

```text
60
```

# 🗁 Expressions vs Statements

Expressions and statements serve different roles.

| Expression | Statement |
| --- | --- |
| Produces a value | Performs an action or controls execution |
| Can be used inside other expressions | Generally organizes program execution |
| Can appear inside assignments | Can contain expressions |
| Example: `10 + 20` | Example: `const total = 10 + 20;` |
| Example: `age >= 18` | Example: `if (age >= 18) {}` |

Consider:

```javascript
const age = 25;

if (age >= 18) {
    console.log("Adult");
}
```

* `age >= 18` is a comparison expression.
* `console.log("Adult")` is an expression statement.
* `if (...) { ... }` is a conditional statement.
* `const age = 25` is a declaration statement.

# 🗁 Practical Example

The following example combines multiple expression types to calculate an order total.

```javascript
const product = {
    name: "Keyboard",
    price: 50
};

const quantity = 3;
const discountRate = 0.10;

const subtotal = product.price * quantity;

const discount = subtotal * discountRate;

const total = subtotal - discount;

const message = `
Product: ${product.name}
Quantity: ${quantity}
Subtotal: $${subtotal}
Discount: $${discount}
Total: $${total}
`;

console.log(message);
```

Output:

```text
Product: Keyboard
Quantity: 3
Subtotal: $150
Discount: $15
Total: $135
```

This example demonstrates:

* Object member expressions
* Arithmetic expressions
* Assignment expressions
* Template literal expressions
* Variable identifier expressions

# 🗁 Best Practices

* Keep complex expressions readable.
* Use parentheses when they improve clarity.
* Avoid deeply nested conditional expressions.
* Prefer descriptive variable names.
* Use strict equality (`===`) when comparing values unless coercion is intentional.
* Avoid expressions with unnecessary side effects.
* Break complicated calculations into smaller, meaningful variables.
* Use template literals for readable dynamic strings.
* Understand operator precedence when combining multiple operators.

# 🗁 Key Takeaways

* An expression is a valid piece of JavaScript code that evaluates to a value.
* Expressions can contain literals, identifiers, operators, function calls, and other expressions.
* Arithmetic expressions perform calculations.
* Comparison and logical expressions are commonly used in decision-making.
* Conditional expressions select values based on conditions.
* Function and arrow function expressions create function values.
* Object and array expressions create data structures.
* Member expressions access properties, while call expressions invoke functions.
* Expressions can be nested and combined.
* Statements organize program execution, while expressions produce values.
