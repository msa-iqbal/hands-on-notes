# JavaScript Variables

JavaScript variables are named storage locations used to hold data that a program can access and manipulate.

Variables allow developers to store information such as strings, numbers, Boolean values, objects, arrays, and function references.

They are fundamental to programming because applications need to store, retrieve, update, and process data during execution.

JavaScript provides three keywords for declaring variables:

* `var`
* `let`
* `const`

Understanding variables and their behavior is essential for writing reliable and maintainable JavaScript applications.

# 🗁 What Is a Variable?

A variable is a named binding that allows a program to reference a value.

Example:

```javascript
let username = "Muhammad";

console.log(username);
```

Output:

```text
Muhammad
```

In this example:

* `let` declares a variable.
* `username` is the variable name.
* `"Muhammad"` is the assigned value.
* `console.log()` accesses and displays the variable's value.

Variables can also be updated after declaration when their declaration type permits reassignment.

```javascript
let age = 25;

age = 26;

console.log(age);
```

Output:

```text
26
```

# 🗁 Declaring Variables

Variable declaration introduces a variable into the current scope.

JavaScript supports three declaration keywords.

**Using `var`**

```javascript
var username = "John";

console.log(username);
```

**Using `let`**

```javascript
let age = 25;

console.log(age);
```

**Using `const`**

```javascript
const country = "Bangladesh";

console.log(country);
```

Each keyword has different rules regarding scope, redeclaration, reassignment, and initialization.

# 🗁 Variable Initialization

Initialization means assigning an initial value when declaring a variable.

Example:

```javascript
let username = "Muhammad";

const age = 25;

var country = "Bangladesh";
```

A variable can also be declared without an initial value when using `var` or `let`.

```javascript
let score;

var total;

console.log(score);
console.log(total);
```

Output:

```text
undefined
undefined
```

A `const` declaration must be initialized when declared.

```javascript
const age = 25;
```

The following is invalid:

```javascript
const age;
```

It causes a `SyntaxError`.

# 🗁 Variable Assignment

Assignment means storing a value in an already declared variable.

Example:

```javascript
let message;

message = "Hello JavaScript";

console.log(message);
```

Output:

```text
Hello JavaScript
```

Variables declared with `let` and `var` can be reassigned.

```javascript
let score = 10;

score = 20;

console.log(score);
```

Output:

```text
20
```

A `const` binding cannot be reassigned.

```javascript
const score = 10;

score = 20;
```

This causes a `TypeError`.

# 🗁 Variable Redeclaration

Redeclaration means declaring an identifier again within the same scope.

The three declaration keywords behave differently.

**`var` redeclaration**

A variable declared with `var` can be redeclared within the same scope.

```javascript
var username = "John";

var username = "Jane";

console.log(username);
```

Output:

```text
Jane
```

**`let` redeclaration**

A variable declared with `let` cannot be redeclared within the same scope.

```javascript
let username = "John";

let username = "Jane";
```

This causes a `SyntaxError`.

**`const` redeclaration**

A variable declared with `const` also cannot be redeclared within the same scope.

```javascript
const username = "John";

const username = "Jane";
```

This causes a `SyntaxError`.

However, a `var`, `let`, or `const` declaration can use the same identifier in a different scope when the scope rules allow it.

# 🗁 Variable Naming Rules

JavaScript identifiers must follow specific naming rules.

**Valid variable names**

```javascript
let username;

let firstName;

let user123;

let _privateValue;

let $price;

let totalAmount;
```

**Invalid variable names**

```javascript
let 123user;

let first-name;

let first name;

let const;

let user@email;
```

Important naming rules:

* Identifiers can contain letters, digits, underscores (`_`), and dollar signs (`$`).
* Identifiers cannot begin with a digit.
* Identifiers cannot contain spaces.
* Identifiers cannot contain hyphens.
* Reserved keywords cannot be used as ordinary identifiers.
* JavaScript identifiers are case-sensitive.
* Unicode characters are allowed in identifiers, although conventional English-based names are common in most projects.

# 🗁 Case Sensitivity

JavaScript variable names are case-sensitive.

This means uppercase and lowercase letters are treated as different characters.

Example:

```javascript
let username = "John";

let Username = "Jane";

let USERNAME = "Alex";

console.log(username);
console.log(Username);
console.log(USERNAME);
```

Output:

```text
John
Jane
Alex
```

These are three separate variables.

# 🗁 Variable Naming Conventions

JavaScript developers commonly use camelCase for variable names.

**Camel case**

The first word starts with a lowercase letter, and subsequent words start with uppercase letters.

```javascript
let firstName = "John";

let lastName = "Doe";

let totalPrice = 500;

let isAuthenticated = true;
```

Other naming conventions include:

**Snake case**

```javascript
let first_name = "John";

let total_price = 500;
```

**Pascal case**

```javascript
let FirstName = "John";

let TotalPrice = 500;
```

**Uppercase with underscores**

```javascript
const MAX_USERS = 100;

const API_BASE_URL = "https://api.example.com";
```

Common conventions:

* Use camelCase for ordinary variables.
* Use descriptive names that communicate purpose.
* Use uppercase letters with underscores for constants representing fixed configuration values.
* Use PascalCase for classes and constructors.
* Avoid meaningless names such as `x`, `a`, or `temp` unless their context is obvious.
* Avoid overly long names that make code difficult to read.

# 🗁 Variables Can Store Different Data Types

JavaScript is dynamically typed, so a variable can hold values of different data types during its lifetime.

Example:

```javascript
let value = "Hello";

console.log(typeof value);

value = 100;

console.log(typeof value);

value = true;

console.log(typeof value);
```

Output:

```text
string
number
boolean
```

Variables can store primitive values:

```javascript
const name = "John";

const age = 25;

const price = 99.99;

const isActive = true;

const emptyValue = null;

let undefinedValue;
```

Variables can also store reference values:

```javascript
const user = {
    name: "John",
    age: 25
};

const numbers = [10, 20, 30];

const greet = function () {
    console.log("Hello");
};
```

# 🗁 Variables and Memory

Variables provide access to values managed by the JavaScript runtime.

Consider:

```javascript
let age = 25;

let username = "John";

const user = {
    name: "John",
    age: 25
};
```

Conceptually:

* `age` is bound to a numeric value.
* `username` is bound to a string value.
* `user` is bound to an object reference.

JavaScript manages memory automatically.

When objects and other values are no longer reachable, the garbage collector may reclaim their memory.

Developers generally work with variable bindings and references rather than manually allocating and freeing memory.

# 🗁 Variable Scope

Scope determines where a variable can be accessed in a program.

JavaScript has several types of scope:

* Global scope
* Function scope
* Block scope
* Module scope

The declaration keyword affects variable scope.

**Global scope**

A variable declared outside functions and blocks may be accessible throughout its script or module context, depending on the declaration type and execution environment.

```javascript
const appName = "My Application";

function showAppName() {
    console.log(appName);
}

showAppName();
```

Output:

```text
My Application
```

**Function scope**

Variables declared with `var` inside a function are accessible throughout that function.

```javascript
function greet() {
    var message = "Hello";

    console.log(message);
}

greet();
```

**Block scope**

Variables declared with `let` and `const` are limited to their enclosing block.

```javascript
if (true) {
    let message = "Hello";

    const username = "John";

    console.log(message);
    console.log(username);
}
```

The variables cannot be accessed outside that block.

```javascript
if (true) {
    let message = "Hello";
}

console.log(message);
```

This causes a `ReferenceError`.

# 🗁 Variable Hoisting

Hoisting describes how JavaScript processes declarations before executing code within a scope.

Different declaration types have different initialization behavior.

**`var` hoisting**

A `var` declaration is initialized with `undefined` during scope initialization.

```javascript
console.log(username);

var username = "John";
```

Output:

```text
undefined
```

Conceptually, this behaves similarly to:

```javascript
var username;

console.log(username);

username = "John";
```

**`let` hoisting**

A `let` declaration is hoisted but remains uninitialized until execution reaches its declaration.

```javascript
console.log(age);

let age = 25;
```

This causes a `ReferenceError` because the variable is in the Temporal Dead Zone.

**`const` hoisting**

A `const` declaration is also hoisted but remains uninitialized until its declaration is evaluated.

```javascript
console.log(country);

const country = "Bangladesh";
```

This also causes a `ReferenceError`.

# 🗁 Temporal Dead Zone

The Temporal Dead Zone (TDZ) is the period between entering a scope and evaluating a `let` or `const` declaration.

During this period, accessing the variable causes a `ReferenceError`.

Example:

```javascript
{
    console.log(username);

    let username = "John";
}
```

The variable exists in the scope but cannot be accessed before initialization.

The same rule applies to `const`.

```javascript
{
    console.log(age);

    const age = 25;
}
```

This behavior helps prevent accidental access to variables before initialization.

# 🗁 Variable Mutation vs Reassignment

Mutation and reassignment are different operations.

**Reassignment**

Reassignment changes which value a variable binding refers to.

```javascript
let age = 25;

age = 30;

console.log(age);
```

Output:

```text
30
```

**Mutation**

Mutation changes the contents or properties of an existing object or array.

```javascript
const user = {
    name: "John",
    age: 25
};

user.age = 30;

console.log(user);
```

Output:

```text
{ name: 'John', age: 30 }
```

A `const` variable cannot be reassigned, but an object or array stored in a `const` binding can still be mutated.

Example with an array:

```javascript
const numbers = [10, 20, 30];

numbers.push(40);

console.log(numbers);
```

Output:

```text
[10, 20, 30, 40]
```

However, assigning a different array to the same `const` binding is not allowed.

```javascript
const numbers = [10, 20, 30];

numbers = [40, 50, 60];
```

This causes a `TypeError`.

# 🗁 Variable Shadowing

Variable shadowing occurs when a variable declared in an inner scope uses the same name as a variable in an outer scope.

Example:

```javascript
const username = "John";

function greet() {
    const username = "Jane";

    console.log(username);
}

greet();

console.log(username);
```

Output:

```text
Jane
John
```

The inner variable shadows the outer variable within its scope.

Shadowing is allowed when the declaration rules permit it.

# 🗁 Global Variables

A global variable is available across a broad execution scope.

In a classic browser script, top-level `var` declarations create properties on the global object, while top-level `let` and `const` declarations do not.

Example:

```javascript
var globalVar = "A";

let globalLet = "B";

const globalConst = "C";

console.log(globalThis.globalVar);

console.log(globalThis.globalLet);

console.log(globalThis.globalConst);
```

In a classic browser script, the output is:

```text
A
undefined
undefined
```

In JavaScript modules, top-level declarations are module-scoped rather than global-object properties.

Global variables should be used carefully because they can create naming conflicts and make application state harder to manage.

# 🗁 Multiple Variable Declarations

JavaScript allows multiple variables to be declared in a single declaration statement.

Example:

```javascript
let firstName = "John",
    lastName = "Doe",
    age = 25;

console.log(firstName, lastName, age);
```

Output:

```text
John Doe 25
```

Multiple declarations are valid, but separate declarations can improve readability.

```javascript
const firstName = "John";

const lastName = "Doe";

const age = 25;
```

# 🗁 Variable Destructuring

Destructuring allows values to be extracted from arrays or objects and assigned to variables.

**Array destructuring**

```javascript
const colors = ["red", "green", "blue"];

const [first, second, third] = colors;

console.log(first);
console.log(second);
console.log(third);
```

Output:

```text
red
green
blue
```

**Object destructuring**

```javascript
const user = {
    name: "John",
    age: 25
};

const { name, age } = user;

console.log(name);
console.log(age);
```

Output:

```text
John
25
```

Destructuring is useful when working with function results, API responses, and complex data structures.

# 🗁 Best Practices

* Prefer `const` when a variable binding does not need reassignment.
* Use `let` when a variable must be reassigned.
* Avoid `var` in modern JavaScript unless there is a specific compatibility or scoping reason.
* Use meaningful camelCase variable names.
* Initialize variables close to where they are used.
* Avoid unnecessary global variables.
* Keep variable scope as narrow as practical.
* Avoid redeclaring variables in the same scope.
* Understand the difference between reassignment and mutation.
* Use `const` with objects and arrays when their bindings should remain stable.
* Avoid accessing `let` and `const` variables before initialization.
* Prefer separate declarations when they improve readability.

# 🗁 Practical Example

The following example demonstrates variable declarations, assignment, object mutation, destructuring, and calculations.

```javascript
const product = {
    name: "Laptop",
    price: 1000,
    quantity: 2
};

let discount = 100;

const { name, price, quantity } = product;

const subtotal = price * quantity;

const total = subtotal - discount;

const order = {
    productName: name,
    quantity,
    subtotal,
    discount,
    total
};

console.log(order);
```

Output:

```text
{
    productName: 'Laptop',
    quantity: 2,
    subtotal: 2000,
    discount: 100,
    total: 1900
}
```

This example demonstrates how variables make it possible to store and process application data in an organized way.

# 🗁 Key Takeaways

* Variables store and provide access to data.
* JavaScript provides `var`, `let`, and `const` for variable declarations.
* `var` is function-scoped, while `let` and `const` are block-scoped.
* `let` and `var` allow reassignment; `const` does not.
* `var` allows redeclaration in the same scope, while `let` and `const` do not.
* Variables can hold values of different data types.
* JavaScript identifiers are case-sensitive and follow naming rules.
* Hoisting affects how declarations behave during execution.
* `let` and `const` have a Temporal Dead Zone before initialization.
* A `const` binding can reference a mutable object or array.
* Scope determines where variables can be accessed.
* Destructuring provides a convenient way to extract values into variables.
* Prefer `const` by default and use `let` when reassignment is necessary.
