# JavaScript Parameters and Arguments

In JavaScript, **parameters** and **arguments** are fundamental concepts used to pass information into functions.

They allow functions to receive external values, process them, and produce different results depending on the supplied data.

Although these terms are closely related, they have different meanings:

* **Parameters** are variables listed in a function's definition.
* **Arguments** are the actual values passed to a function when it is called.

# 🗁 Understanding Parameters

Parameters act as placeholders for values that a function receives.

```javascript
function greet(name) {
  console.log(`Hello, ${name}!`);
}

greet("Alex");
```

Output:

```text
Hello, Alex!
```

In this example:

* `name` is a parameter.
* `"Alex"` is an argument.
* The function receives the argument and uses it through the parameter.

A function can define zero, one, or multiple parameters.

# 🗁 Understanding Arguments

Arguments are the actual values supplied when invoking a function.

```javascript
function add(a, b) {
  return a + b;
}

const result = add(10, 20);

console.log(result);
```

Output:

```text
30
```

Here:

| Parameter | Argument |
| --- | --- |
| `a` | `10` |
| `b` | `20` |

JavaScript assigns the supplied arguments to their corresponding parameters based on their positions.

# 🗁 Parameters and Arguments with Multiple Values

A function can accept multiple parameters and arguments.

```javascript
function createUser(name, age, role) {
  console.log(`Name: ${name}`);
  console.log(`Age: ${age}`);
  console.log(`Role: ${role}`);
}

createUser("Alex", 25, "Developer");
```

Output:

```text
Name: Alex
Age: 25
Role: Developer
```

The argument order determines which parameter receives each value.

# 🗁 Missing Arguments

JavaScript does not require every parameter to receive an argument.

If an argument is omitted, the corresponding parameter receives `undefined`, unless a default parameter value is defined.

```javascript
function greet(name, message) {
  console.log(name);
  console.log(message);
}

greet("Alex");
```

Output:

```text
Alex
undefined
```

The second parameter receives `undefined` because no corresponding argument was supplied.

# 🗁 Extra Arguments

JavaScript also allows functions to receive more arguments than the number of declared parameters.

```javascript
function add(a, b) {
  return a + b;
}

console.log(add(10, 20, 30));
```

Output:

```text
30
```

The third argument (`30`) is not assigned to a declared parameter.

It is not automatically an error. Extra arguments can be accessed through the `arguments` object in traditional functions or through rest parameters.

# 🗁 Arguments Object

Traditional JavaScript functions have access to a special array-like object called `arguments`.

It contains the arguments passed to the function.

```javascript
function showArguments() {
  console.log(arguments);
}

showArguments("Alex", 25, "Developer");
```

The output resembles:

```text
[Arguments] {
  '0': 'Alex',
  '1': 25,
  '2': 'Developer'
}
```

Each argument can be accessed using its numeric index.

```javascript
function showArguments() {
  console.log(arguments[0]);
  console.log(arguments[1]);
  console.log(arguments[2]);
}

showArguments("Alex", 25, "Developer");
```

Output:

```text
Alex
25
Developer
```

# 🗁 Arguments Object Properties

The `arguments` object provides several useful properties and methods.

**`arguments.length`**

Returns the number of arguments passed to the function.

```javascript
function countArguments() {
  console.log(arguments.length);
}

countArguments(10, 20, 30, 40);
```

Output:

```text
4
```

**`arguments[index]`**

Accesses an argument by its position.

```javascript
function showFirstArgument() {
  console.log(arguments[0]);
}

showFirstArgument("JavaScript", "TypeScript");
```

Output:

```text
JavaScript
```

**`arguments.callee`**

Historically, `arguments.callee` referenced the currently executing function.

It is deprecated and forbidden in strict mode. Avoid using it in modern JavaScript.

# 🗁 Converting Arguments into an Array

The `arguments` object is array-like, but it is not a real array.

It does not directly support all array methods, such as `map()`, `filter()`, and `reduce()`.

You can convert it into an array using `Array.from()`.

```javascript
function processArguments() {
  const args = Array.from(arguments);

  console.log(args);
  console.log(args.map(value => value * 2));
}

processArguments(1, 2, 3);
```

Output:

```text
[1, 2, 3]
[2, 4, 6]
```

You can also use the spread syntax:

```javascript
function processArguments() {
  const args = [...arguments];

  console.log(args);
}

processArguments("HTML", "CSS", "JavaScript");
```

Output:

```text
["HTML", "CSS", "JavaScript"]
```

# 🗁 Rest Parameters vs Arguments Object

Rest parameters provide a modern way to collect multiple function arguments into a real array.

```javascript
function showArguments(...args) {
  console.log(args);
}

showArguments(10, 20, 30);
```

Output:

```text
[10, 20, 30]
```

Comparison:

| Feature | `arguments` | Rest Parameters |
| --- | --- | --- |
| Type | Array-like object | Real array |
| Available in | Traditional functions | Functions that declare them |
| Arrow functions | No own `arguments` | Supported |
| Array methods | Not directly available | Available |
| Syntax | Built-in identifier | `...args` |
| Position | Available inside traditional functions | Must be the last parameter |

Rest parameters are generally preferred in modern JavaScript when collecting an arbitrary number of arguments.

# 🗁 Parameter Destructuring

JavaScript allows parameters to receive objects or arrays and extract their values directly through destructuring.

**Object parameter destructuring:**

```javascript
function displayUser({ name, age }) {
  console.log(`Name: ${name}`);
  console.log(`Age: ${age}`);
}

displayUser({
  name: "Alex",
  age: 25
});
```

Output:

```text
Name: Alex
Age: 25
```

The function extracts `name` and `age` from the supplied object.

**Array parameter destructuring:**

```javascript
function displayCoordinates([x, y]) {
  console.log(`X: ${x}`);
  console.log(`Y: ${y}`);
}

displayCoordinates([10, 20]);
```

Output:

```text
X: 10
Y: 20
```

Destructuring is useful when a function accepts structured data.

# 🗁 Passing Objects as Arguments

When an object is passed to a function, JavaScript passes the value of its reference.

This means the function parameter and the original variable refer to the same object.

```javascript
const user = {
  name: "Alex",
  age: 25
};

function updateUser(person) {
  person.age = 30;
}

updateUser(user);

console.log(user);
```

Output:

```javascript
{
  name: "Alex",
  age: 30
}
```

The object was modified through the function parameter, so the change is visible through the original variable.

# 🗁 Passing Primitive Values as Arguments

Primitive values are passed by value.

A function receives its own parameter variable containing the primitive value.

```javascript
let number = 10;

function updateNumber(value) {
  value = 50;
}

updateNumber(number);

console.log(number);
```

Output:

```text
10
```

Changing the parameter does not change the original variable.

# 🗁 JavaScript Uses Pass-by-Value

JavaScript uses **pass-by-value** semantics for function arguments.

This includes objects.

When an object is passed, the value being copied is the object reference. Consequently, the parameter and the original variable can refer to the same object.

However, reassigning the parameter does not reassign the original variable.

```javascript
const user = {
  name: "Alex"
};

function replaceUser(person) {
  person = {
    name: "John"
  };
}

replaceUser(user);

console.log(user.name);
```

Output:

```text
Alex
```

The parameter `person` was reassigned to a different object, but the original `user` variable still references the original object.

This distinction is important when designing functions that modify objects.

# 🗁 Function Parameters with Different Data Types

Parameters can receive different JavaScript data types.

```javascript
function displayValue(value) {
  console.log(value);
}

displayValue("JavaScript");
displayValue(100);
displayValue(true);
displayValue(null);
displayValue({ name: "Alex" });
displayValue([10, 20, 30]);
```

JavaScript does not require parameter types to be declared.

The same parameter can receive different types of values in different calls.

# 🗁 Parameters and Return Values

Parameters provide input to a function, while the `return` statement provides output.

```javascript
function calculateTotal(price, quantity) {
  return price * quantity;
}

const total = calculateTotal(100, 5);

console.log(total);
```

Output:

```text
500
```

This creates a simple input-processing-output pattern:

* Inputs: `price` and `quantity`.
* Processing: multiplication.
* Output: the returned total.

# 🗁 Parameters and Arguments with Arrow Functions

Arrow functions also support parameters and arguments.

```javascript
const multiply = (a, b) => {
  return a * b;
};

console.log(multiply(5, 6));
```

Output:

```text
30
```

Arrow functions do not have their own `arguments` object.

Use rest parameters when you need to collect multiple arguments:

```javascript
const sum = (...numbers) => {
  return numbers.reduce((total, number) => total + number, 0);
};

console.log(sum(10, 20, 30, 40));
```

Output:

```text
100
```

# 🗁 Practical Example: Shopping Cart

A function can accept multiple arguments to calculate the total price of shopping cart items.

```javascript
function calculateCartTotal(price, quantity, discount) {
  const subtotal = price * quantity;
  const discountAmount = subtotal * (discount / 100);

  return subtotal - discountAmount;
}

const total = calculateCartTotal(100, 5, 10);

console.log(`Total: ${total}`);
```

Output:

```text
Total: 450
```

The function receives:

* `price`: 100
* `quantity`: 5
* `discount`: 10%

It calculates the subtotal, applies the discount, and returns the final amount.

# 🗁 Common Mistakes

**Confusing parameters with arguments**

```javascript
function greet(name) {
  console.log(name);
}

greet("Alex");
```

* `name` is a parameter.
* `"Alex"` is an argument.

**Assuming missing arguments cause errors**

```javascript
function add(a, b) {
  return a + b;
}

console.log(add(10));
```

Output:

```text
NaN
```

The missing `b` parameter receives `undefined`, and `10 + undefined` produces `NaN`.

**Assuming extra arguments are automatically rejected**

```javascript
function greet(name) {
  console.log(name);
}

greet("Alex", 25, "Developer");
```

The function executes normally. The extra arguments are simply not assigned to declared parameters.

**Assuming objects are passed by reference**

JavaScript passes values, including object references, by value. Mutating the referenced object and reassigning the parameter are different operations.

**Using `arguments` inside arrow functions**

Arrow functions do not have their own `arguments` object. Use rest parameters instead.

# 🗁 Best Practices

* Use meaningful parameter names.
* Keep the number of parameters manageable.
* Use object parameters when a function needs many related configuration values.
* Use destructuring when it improves readability.
* Prefer rest parameters over the `arguments` object.
* Validate important inputs when functions require specific values.
* Avoid modifying input objects unexpectedly.
* Keep functions focused on a clear responsibility.
* Document complex parameter requirements.
* Use default parameters when appropriate for optional inputs.

# 🗁 Key Takeaways

* Parameters are placeholders defined in a function declaration or expression.
* Arguments are actual values passed during a function call.
* JavaScript assigns arguments to parameters by position.
* Missing arguments become `undefined` unless defaults are provided.
* Extra arguments are allowed.
* Traditional functions have an array-like `arguments` object.
* Rest parameters collect arguments into a real array.
* JavaScript uses pass-by-value semantics, including when passing object references.
* Object and array destructuring can be used directly in function parameters.
* Parameters define function inputs, while return values define function outputs.
