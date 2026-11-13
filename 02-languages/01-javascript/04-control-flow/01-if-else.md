# JavaScript If...Else Statements

**Conditional statements** allow JavaScript programs to execute different blocks of code depending on whether a condition is `true` or `false`.

They are an essential part of program control flow because they allow applications to make decisions dynamically.

JavaScript provides several conditional statements:

* `if`
* `else`
* `else if`
* Nested `if...else`
* Conditional (ternary) operator

```javascript
const age = 20;

if (age >= 18) {
    console.log("You are an adult");
} else {
    console.log("You are a minor");
}
```

The condition `age >= 18` evaluates to `true`, so JavaScript executes the first block.

# 🗁 The if Statement

The `if` statement executes a block of code only when its condition evaluates to `true`.

**Syntax**

```javascript
if (condition) {
    // Code executes when condition is true
}
```

**Example**

```javascript
const age = 25;

if (age >= 18) {
    console.log("You are eligible to vote");
}
```

**Output**

```text
You are eligible to vote
```

If the condition evaluates to `false`, JavaScript skips the block.

```javascript
const age = 15;

if (age >= 18) {
    console.log("You are eligible to vote");
}

console.log("Program continues...");
```

**Output**

```text
Program continues...
```

# 🗁 The if...else Statement

The `if...else` statement executes one block when the condition is `true` and another block when it is `false`.

**Syntax**

```javascript
if (condition) {
    // Executes when condition is true
} else {
    // Executes when condition is false
}
```

**Example**

```javascript
const temperature = 15;

if (temperature >= 25) {
    console.log("The weather is warm");
} else {
    console.log("The weather is cool");
}
```

**Output**

```text
The weather is cool
```

Only one of the two branches executes.

# 🗁 The else if Statement

The `else if` statement allows multiple conditions to be evaluated in sequence.

JavaScript checks conditions from top to bottom and executes the first matching branch.

**Syntax**

```javascript
if (condition1) {
    // Executes when condition1 is true
} else if (condition2) {
    // Executes when condition2 is true
} else if (condition3) {
    // Executes when condition3 is true
} else {
    // Executes when all conditions are false
}
```

**Example**

```javascript
const marks = 85;

if (marks >= 90) {
    console.log("Grade A");
} else if (marks >= 80) {
    console.log("Grade B");
} else if (marks >= 70) {
    console.log("Grade C");
} else if (marks >= 60) {
    console.log("Grade D");
} else {
    console.log("Grade F");
}
```

**Output**

```text
Grade B
```

Because `marks >= 80` is the first condition that evaluates to `true`, JavaScript executes that branch and skips the remaining branches.

# 🗁 Multiple Conditions

You can combine multiple conditions using logical operators.

**Using AND (`&&`)**

All conditions must evaluate to `true`.

```javascript
const age = 25;
const hasLicense = true;

if (age >= 18 && hasLicense) {
    console.log("You can drive");
} else {
    console.log("You cannot drive");
}
```

**Using OR (`||`)**

At least one condition must evaluate to `true`.

```javascript
const isAdmin = false;
const isEditor = true;

if (isAdmin || isEditor) {
    console.log("You can access the dashboard");
} else {
    console.log("Access denied");
}
```

**Using NOT (`!`)**

The NOT operator reverses a Boolean condition.

```javascript
const isLoggedIn = false;

if (!isLoggedIn) {
    console.log("Please log in");
}
```

# 🗁 Nested if...else Statements

A nested conditional statement is an `if...else` statement placed inside another conditional block.

It is useful when one decision depends on the result of another decision.

**Example**

```javascript
const age = 25;
const hasLicense = true;

if (age >= 18) {
    if (hasLicense) {
        console.log("You can drive");
    } else {
        console.log("You need a driving license");
    }
} else {
    console.log("You are too young to drive");
}
```

**Output**

```text
You can drive
```

Nested conditions are useful, but excessive nesting can make code difficult to understand. Consider early returns or combining conditions when appropriate.

# 🗁 Truthy and Falsy Conditions

JavaScript automatically converts values into Boolean equivalents when evaluating conditions.

This behavior is known as **truthiness**.

**Falsy values**

The following values evaluate to `false` in conditional expressions:

```javascript
if (false) {}
if (0) {}
if (-0) {}
if (0n) {}
if ("") {}
if (null) {}
if (undefined) {}
if (NaN) {}
```

**Truthy values**

Almost every other JavaScript value evaluates to `true`.

```javascript
if ("Hello") {
    console.log("Truthy string");
}

if (100) {
    console.log("Truthy number");
}

if ([]) {
    console.log("Truthy array");
}

if ({}) {
    console.log("Truthy object");
}
```

An empty array and an empty object are both truthy.

**Practical example**

```javascript
const username = "";

if (username) {
    console.log(`Welcome, ${username}`);
} else {
    console.log("Username is required");
}
```

**Output**

```text
Username is required
```

# 🗁 Conditional Operator (Ternary Operator)

The **ternary operator** is a concise alternative to a simple `if...else` statement.

It evaluates a condition and returns one of two expressions.

**Syntax**

```javascript
condition ? expressionIfTrue : expressionIfFalse;
```

**Example**

```javascript
const age = 20;

const message = age >= 18
    ? "Adult"
    : "Minor";

console.log(message); // "Adult"
```

Equivalent `if...else` implementation:

```javascript
const age = 20;

let message;

if (age >= 18) {
    message = "Adult";
} else {
    message = "Minor";
}

console.log(message);
```

**Nested ternary operators**

Ternary operators can be nested to handle multiple conditions.

```javascript
const marks = 85;

const grade = marks >= 90
    ? "A"
    : marks >= 80
        ? "B"
        : marks >= 70
            ? "C"
            : "F";

console.log(grade); // "B"
```

Although nested ternaries are valid, multiple branches are often clearer with `if...else if...else`.

# 🗁 Block Scope and Conditional Statements

Variables declared with `let` and `const` inside an `if` block are limited to that block.

```javascript
if (true) {
    const username = "John";
    let age = 25;

    console.log(username);
    console.log(age);
}

// console.log(username); // ReferenceError
// console.log(age);      // ReferenceError
```

Variables declared with `var` are not block-scoped.

```javascript
if (true) {
    var message = "Hello";
}

console.log(message); // "Hello"
```

Prefer `const` by default and use `let` when reassignment is necessary.

# 🗁 Early Return Pattern

The early return pattern is commonly used inside functions to handle conditions before executing the main logic.

It helps reduce unnecessary nesting.

**Without early returns**

```javascript
function processUser(user) {
    if (user) {
        if (user.isActive) {
            console.log("Processing user...");
        } else {
            console.log("User is inactive");
        }
    } else {
        console.log("User not found");
    }
}
```

**With early returns**

```javascript
function processUser(user) {
    if (!user) {
        console.log("User not found");
        return;
    }

    if (!user.isActive) {
        console.log("User is inactive");
        return;
    }

    console.log("Processing user...");
}

processUser({
    name: "John",
    isActive: true
});
```

The second implementation is easier to read because invalid conditions are handled immediately.

# 🗁 Practical Examples

**Check whether a number is positive, negative, or zero**

```javascript
const number = -10;

if (number > 0) {
    console.log("Positive number");
} else if (number < 0) {
    console.log("Negative number");
} else {
    console.log("Zero");
}
```

**Check whether a number is even or odd**

```javascript
const number = 7;

if (number % 2 === 0) {
    console.log("Even number");
} else {
    console.log("Odd number");
}
```

**Check voting eligibility**

```javascript
const age = 20;
const isCitizen = true;

if (age >= 18 && isCitizen) {
    console.log("Eligible to vote");
} else {
    console.log("Not eligible to vote");
}
```

**Check login credentials**

```javascript
const username = "admin";
const password = "12345";

if (username === "admin" && password === "12345") {
    console.log("Login successful");
} else {
    console.log("Invalid credentials");
}
```

**Calculate a discount**

```javascript
const purchaseAmount = 1500;

let discount = 0;

if (purchaseAmount >= 5000) {
    discount = 20;
} else if (purchaseAmount >= 2000) {
    discount = 10;
} else if (purchaseAmount >= 1000) {
    discount = 5;
}

const finalPrice = purchaseAmount -
    (purchaseAmount * discount / 100);

console.log("Discount:", `${discount}%`);
console.log("Final price:", finalPrice);
```

**Determine a person's age category**

```javascript
const age = 35;

if (age < 0) {
    console.log("Invalid age");
} else if (age < 13) {
    console.log("Child");
} else if (age < 20) {
    console.log("Teenager");
} else if (age < 60) {
    console.log("Adult");
} else {
    console.log("Senior");
}
```

**Check whether a year is a leap year**

```javascript
const year = 2024;

if (
    year % 400 === 0 ||
    (year % 4 === 0 && year % 100 !== 0)
) {
    console.log("Leap year");
} else {
    console.log("Not a leap year");
}
```

# 🗁 Common Mistakes

**Using assignment (`=`) instead of comparison (`===`)**

```javascript
let age = 18;

if (age = 20) {
    console.log("Condition is truthy");
}
```

The assignment changes the value of `age` and the assigned value is evaluated as the condition.

Use strict equality for comparison.

```javascript
if (age === 20) {
    console.log("Age is 20");
}
```

**Incorrectly ordering conditions**

```javascript
const marks = 95;

if (marks >= 60) {
    console.log("Grade D");
} else if (marks >= 90) {
    console.log("Grade A");
}
```

The first condition matches, so the second condition is never checked.

Put more specific or higher-threshold conditions first.

```javascript
if (marks >= 90) {
    console.log("Grade A");
} else if (marks >= 60) {
    console.log("Grade D");
}
```

**Using `&&` instead of `||`**

```javascript
const age = 20;

if (age < 13 && age > 19) {
    console.log("Teenager");
}
```

A number cannot satisfy both conditions simultaneously.

Use OR when either condition should match.

```javascript
if (age < 13 || age > 19) {
    console.log("Outside the teenager range");
}
```

**Assuming empty arrays are falsy**

```javascript
const users = [];

if (users) {
    console.log("Users exist");
}
```

The condition executes because an empty array is truthy.

Check its length when you need to determine whether it contains elements.

```javascript
if (users.length > 0) {
    console.log("Users exist");
} else {
    console.log("No users found");
}
```

**Creating unnecessarily nested conditions**

```javascript
if (isLoggedIn) {
    if (isAdmin) {
        console.log("Admin access granted");
    }
}
```

When appropriate, combine conditions.

```javascript
if (isLoggedIn && isAdmin) {
    console.log("Admin access granted");
}
```

# 🗁 Best Practices

* Use `if` for a single condition.
* Use `if...else` when choosing between two alternatives.
* Use `else if` for multiple mutually exclusive conditions.
* Use strict equality (`===`) for comparisons.
* Use logical operators to combine related conditions.
* Order conditional branches carefully.
* Avoid deeply nested conditional statements.
* Use early returns to reduce nesting inside functions.
* Prefer `const` and `let` instead of `var`.
* Use the ternary operator for short, simple conditional expressions.
* Avoid deeply nested ternary expressions.
* Validate input before using it in conditional logic.
* Remember that empty arrays and empty objects are truthy.

# 🗁 Key Takeaways

* Conditional statements control program execution based on conditions.
* `if` executes code when a condition is truthy.
* `else` executes when the preceding `if` condition is falsy.
* `else if` supports multiple conditional branches.
* Nested conditions allow decisions to depend on other decisions.
* Logical operators combine or reverse conditions.
* JavaScript automatically evaluates values using truthiness.
* The ternary operator provides a concise alternative to simple `if...else` statements.
* Block-scoped variables declared with `let` and `const` remain inside their conditional blocks.
* Early returns improve readability by reducing unnecessary nesting.
