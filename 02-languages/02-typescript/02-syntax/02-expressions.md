# TypeScript Expressions

An expression is a combination of values, variables, operators, and function calls that produces a value.

Expressions are fundamental building blocks of TypeScript programs. They are used in variable assignments, calculations, conditions, function arguments, return statements, and many other operations.

Example:

```typescript
const result: number = 10 + 20;
```

Here, `10 + 20` is an expression that evaluates to `30`.

# 🗁 What Is an Expression?

An expression is any valid piece of code that evaluates to a value.

Examples:

```typescript
10 + 20
```

```typescript
"Hello" + " World"
```

```typescript
age >= 18
```

```typescript
user.name
```

```typescript
calculateTotal(100, 5)
```

Each expression produces a value that can be used by another part of the program.

# 🗁 Expression vs Statement

Expressions and statements are closely related, but they serve different purposes.

| Expression | Statement |
| --- | --- |
| Produces a value | Performs an instruction |
| Can be used inside another expression | Usually cannot be used as a value |
| Examples: `10 + 20`, `age > 18` | Examples: `if`, `for`, `while` |
| Can appear inside assignments | Controls program execution |

Example:

```typescript
const total: number = 10 + 20;
```

In this example:

* `10 + 20` is an expression.
* `const total: number = 10 + 20;` is a variable declaration statement.

Another example:

```typescript
if (10 > 5) {
    console.log("Condition is true");
}
```

The expression `10 > 5` produces a boolean value, which the `if` statement uses to decide whether to execute its block.

# 🗁 Arithmetic Expressions

Arithmetic expressions perform mathematical calculations.

Common arithmetic operators include:

| Operator | Name | Example | Result |
| --- | --- | --- | --- |
| `+` | Addition | `10 + 5` | `15` |
| `-` | Subtraction | `10 - 5` | `5` |
| `*` | Multiplication | `10 * 5` | `50` |
| `/` | Division | `10 / 5` | `2` |
| `%` | Modulus | `10 % 3` | `1` |
| `**` | Exponentiation | `2 ** 3` | `8` |

Example:

```typescript
const a: number = 10;
const b: number = 5;

const addition: number = a + b;
const subtraction: number = a - b;
const multiplication: number = a * b;
const division: number = a / b;
const remainder: number = a % b;

console.log(addition);
console.log(subtraction);
console.log(multiplication);
console.log(division);
console.log(remainder);
```

# 🗁 String Expressions

String expressions combine or manipulate string values.

**String concatenation:**

```typescript
const firstName: string = "John";
const lastName: string = "Smith";

const fullName: string = firstName + " " + lastName;

console.log(fullName);
```

Output:

```text
John Smith
```

**Template literal expression:**

```typescript
const username: string = "John";
const age: number = 25;

const message: string = `My name is ${username} and I am ${age} years old.`;

console.log(message);
```

Template literals make it easier to embed expressions inside strings.

# 🗁 Comparison Expressions

Comparison expressions compare two values and produce a boolean result.

| Operator | Meaning | Example |
| --- | --- | --- |
| `==` | Loose equality | `10 == "10"` |
| `===` | Strict equality | `10 === 10` |
| `!=` | Loose inequality | `10 != "5"` |
| `!==` | Strict inequality | `10 !== "10"` |
| `>` | Greater than | `10 > 5` |
| `<` | Less than | `5 < 10` |
| `>=` | Greater than or equal | `10 >= 10` |
| `<=` | Less than or equal | `5 <= 10` |

Example:

```typescript
const age: number = 25;

console.log(age > 18);
console.log(age < 18);
console.log(age === 25);
console.log(age !== 20);
```

Output:

```text
true
false
true
true
```

Strict equality (`===`) is generally preferred because it compares values without performing implicit type coercion.

# 🗁 Logical Expressions

Logical expressions combine or invert boolean conditions.

**Logical AND (`&&`):**

Returns a truthy or falsy operand according to JavaScript's short-circuit evaluation.

```typescript
const age: number = 25;
const hasLicense: boolean = true;

const canDrive: boolean = age >= 18 && hasLicense;

console.log(canDrive);
```

**Logical OR (`||`):**

Returns the first truthy operand or the final operand if none is truthy.

```typescript
const isAdmin: boolean = false;
const isModerator: boolean = true;

const hasAccess: boolean = isAdmin || isModerator;

console.log(hasAccess);
```

**Logical NOT (`!`):**

Converts an operand to a boolean and reverses its truthiness.

```typescript
const isLoggedIn: boolean = false;

console.log(!isLoggedIn);
```

Output:

```text
true
```

# 🗁 Assignment Expressions

Assignment expressions assign values to variables.

```typescript
let score: number = 10;

score = 20;

console.log(score);
```

TypeScript supports compound assignment operators.

| Operator | Example | Equivalent |
| --- | --- | --- |
| `+=` | `score += 5` | `score = score + 5` |
| `-=` | `score -= 5` | `score = score - 5` |
| `*=` | `score *= 2` | `score = score * 2` |
| `/=` | `score /= 2` | `score = score / 2` |
| `%=` | `score %= 3` | `score = score % 3` |
| `**=` | `score **= 2` | `score = score ** 2` |

Example:

```typescript
let total: number = 100;

total += 50;
total -= 20;
total *= 2;

console.log(total);
```

Output:

```text
260
```

# 🗁 Conditional (Ternary) Expressions

The conditional operator `?:` evaluates a condition and returns one of two expressions.

Syntax:

```typescript
condition ? expressionIfTrue : expressionIfFalse;
```

Example:

```typescript
const age: number = 20;

const status: string = age >= 18 ? "Adult" : "Minor";

console.log(status);
```

Output:

```text
Adult
```

Ternary expressions are useful for simple conditional value assignments.

For complex conditions, use `if...else` statements for better readability.

# 🗁 Function Call Expressions

A function call expression invokes a function and produces its return value.

```typescript
function add(a: number, b: number): number {
    return a + b;
}

const result: number = add(10, 20);

console.log(result);
```

Here:

```typescript
add(10, 20)
```

is a function call expression that evaluates to `30`.

A function returning `void` does not provide a meaningful return value:

```typescript
function greet(): void {
    console.log("Hello!");
}

greet();
```

# 🗁 Property Access Expressions

Property access expressions retrieve properties or methods from objects.

Example:

```typescript
const user = {
    name: "John",
    age: 25
};

console.log(user.name);
console.log(user.age);
```

Both `user.name` and `user.age` are property access expressions.

Bracket notation is another way to access properties:

```typescript
console.log(user["name"]);
console.log(user["age"]);
```

Bracket notation is especially useful when the property name is stored in a variable.

```typescript
const property: "name" | "age" = "name";

console.log(user[property]);
```

# 🗁 Array Expressions

Array expressions create arrays or access their elements.

**Array creation:**

```typescript
const numbers: number[] = [10, 20, 30, 40, 50];
```

**Array element access:**

```typescript
const firstNumber: number = numbers[0];

console.log(firstNumber);
```

**Array method expressions:**

```typescript
const doubled: number[] = numbers.map((number) => number * 2);

console.log(doubled);
```

Output:

```text
[20, 40, 60, 80, 100]
```

# 🗁 Object Expressions

Object expressions create objects containing properties and values.

```typescript
const user = {
    id: 1,
    name: "John",
    email: "john@example.com"
};

console.log(user);
```

Object expressions can also contain computed property names and methods.

```typescript
const propertyName: string = "name";

const user = {
    id: 1,
    [propertyName]: "John",

    greet(): string {
        return `Hello, ${this.name}!`;
    },

    name: "John"
};

console.log(user.greet());
```

# 🗁 Increment and Decrement Expressions

Increment and decrement expressions increase or decrease a numeric variable by one.

**Increment (`++`):**

```typescript
let count: number = 5;

count++;

console.log(count);
```

Output:

```text
6
```

**Decrement (`--`):**

```typescript
let count: number = 5;

count--;

console.log(count);
```

Output:

```text
4
```

Both operators support prefix and postfix forms.

**Prefix:**

```typescript
let number: number = 5;

const result: number = ++number;

console.log(result);
console.log(number);
```

Output:

```text
6
6
```

**Postfix:**

```typescript
let number: number = 5;

const result: number = number++;

console.log(result);
console.log(number);
```

Output:

```text
5
6
```

The prefix form increments before producing the expression's value. The postfix form produces the original value before incrementing the variable.

# 🗁 Type Assertion Expressions

Type assertions tell TypeScript to treat a value as a particular type.

They do not perform runtime type conversion or validation.

Example:

```typescript
const input = document.querySelector("#username") as HTMLInputElement;

console.log(input.value);
```

Here, the assertion tells TypeScript to treat the selected element as an `HTMLInputElement`.

You can also use angle-bracket syntax in contexts where JSX is not involved:

```typescript
const value: unknown = "Hello";

const message: string = <string>value;
```

The `as` syntax is generally preferred because it is compatible with TSX files.

# 🗁 Nullish Coalescing Expressions

The nullish coalescing operator (`??`) returns the right-hand value when the left-hand value is `null` or `undefined`.

Example:

```typescript
const username: string | null = null;

const displayName: string = username ?? "Guest";

console.log(displayName);
```

Output:

```text
Guest
```

Unlike the logical OR operator (`||`), nullish coalescing does not treat values such as `0`, `false`, or `""` as missing.

```typescript
const quantity: number = 0;

const result: number = quantity ?? 10;

console.log(result);
```

Output:

```text
0
```

# 🗁 Optional Chaining Expressions

Optional chaining (`?.`) safely accesses properties or methods when an intermediate value may be `null` or `undefined`.

Example:

```typescript
interface User {
    profile?: {
        email?: string;
    };
}

const user: User = {};

const email = user.profile?.email;

console.log(email);
```

Output:

```text
undefined
```

Without optional chaining, directly accessing a property on an undefined value could cause a runtime error.

# 🗁 Operator Precedence in Expressions

Operator precedence determines the order in which operators are evaluated.

Example:

```typescript
const result: number = 10 + 5 * 2;

console.log(result);
```

Output:

```text
20
```

Multiplication has higher precedence than addition.

Use parentheses to make evaluation order explicit:

```typescript
const result: number = (10 + 5) * 2;

console.log(result);
```

Output:

```text
30
```

Parentheses improve readability and help avoid unintended calculations.

# 🗁 Expression Evaluation

Expressions are evaluated according to JavaScript's rules.

Example:

```typescript
const result: number = 10 + 20 * 2 - 5;

console.log(result);
```

Evaluation:

```text
10 + 20 * 2 - 5
10 + 40 - 5
50 - 5
45
```

Output:

```text
45
```

Understanding evaluation order is important when working with complex expressions.

# 🗁 Best Practices

* Keep expressions simple and readable.
* Use parentheses when operator precedence is not immediately obvious.
* Prefer strict equality (`===` and `!==`) over loose equality.
* Use template literals for readable string interpolation.
* Use ternary expressions only for simple conditional logic.
* Use optional chaining when accessing potentially missing properties.
* Use nullish coalescing when only `null` and `undefined` should trigger a fallback.
* Avoid unnecessary side effects inside complex expressions.
* Use TypeScript type annotations and assertions carefully.
* Do not assume type assertions validate runtime data.

# 🗁 Summary

Expressions are fundamental components of TypeScript programs because they produce values that can be used in calculations, assignments, conditions, and function calls.

You have learned about:

* Arithmetic expressions.
* String expressions.
* Comparison expressions.
* Logical expressions.
* Assignment expressions.
* Conditional expressions.
* Function call expressions.
* Property access expressions.
* Array and object expressions.
* Increment and decrement expressions.
* Type assertion expressions.
* Nullish coalescing.
* Optional chaining.
* Operator precedence and expression evaluation.

Understanding expressions makes it easier to write readable, predictable, and maintainable TypeScript code.
