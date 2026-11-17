# JavaScript Default Parameters

**Default parameters** allow JavaScript functions to assign default values to parameters when the caller does not provide an argument or explicitly passes `undefined`.

They were introduced in **ECMAScript 2015 (ES6)** and provide a cleaner alternative to manually checking whether function arguments exist.

Default parameters make functions more predictable, readable, and easier to maintain.

# 🗁 Basic Syntax

```javascript
function functionName(parameter = defaultValue) {
  // Function body
}
```

Example:

```javascript
function greet(name = "Guest") {
  console.log(`Hello, ${name}!`);
}

greet("Alex");
greet();
```

Output:

```text
Hello, Alex!
Hello, Guest!
```

Here:

* `name` is the parameter.
* `"Guest"` is the default value.
* If an argument is provided, it is used.
* If the argument is omitted, the default value is used.

# 🗁 Default Parameters with Multiple Parameters

A function can define default values for multiple parameters.

```javascript
function createUser(name = "Guest", role = "User", active = true) {
  return {
    name,
    role,
    active
  };
}

console.log(createUser("Alex", "Admin", false));
console.log(createUser());
```

Output:

```javascript
{
  name: "Alex",
  role: "Admin",
  active: false
}

{
  name: "Guest",
  role: "User",
  active: true
}
```

Each parameter independently uses its default value when its corresponding argument is omitted or is `undefined`.

# 🗁 Default Parameters with `undefined`

When a function receives `undefined` for a parameter, JavaScript uses that parameter's default value.

```javascript
function greet(name = "Guest") {
  console.log(`Hello, ${name}!`);
}

greet(undefined);
```

Output:

```text
Hello, Guest!
```

The same behavior occurs when the argument is omitted:

```javascript
greet();
```

Output:

```text
Hello, Guest!
```

# 🗁 Default Parameters with `null`

Unlike `undefined`, `null` does not trigger a default parameter.

```javascript
function greet(name = "Guest") {
  console.log(`Hello, ${name}!`);
}

greet(null);
```

Output:

```text
Hello, null!
```

JavaScript only applies the default when the argument is omitted or its value is `undefined`.

# 🗁 Default Parameters with Other Falsy Values

Default parameters do not replace other falsy values such as:

* `null`
* `false`
* `0`
* `""`
* `NaN`

Example:

```javascript
function showValue(value = 100) {
  console.log(value);
}

showValue(undefined);
showValue(null);
showValue(false);
showValue(0);
showValue("");
```

Output:

```text
100
null
false
0

```

This behavior differs from the logical OR (`||`) operator, which replaces every falsy value.

# 🗁 Default Parameters vs Logical OR

Before ES6, developers commonly used the logical OR operator to provide default values.

```javascript
function createUser(name) {
  name = name || "Guest";

  return name;
}

console.log(createUser());
console.log(createUser("Alex"));
console.log(createUser(""));
```

Output:

```text
Guest
Alex
Guest
```

The problem is that `||` treats all falsy values as missing.

A default parameter behaves differently:

```javascript
function createUser(name = "Guest") {
  return name;
}

console.log(createUser());
console.log(createUser("Alex"));
console.log(createUser(""));
console.log(createUser(null));
```

Output:

```text
Guest
Alex

null
```

Comparison:

| Input | Default Parameter | Logical OR (`||`) |
|---|---|---|
| Omitted | Default value | Default value |
| `undefined` | Default value | Default value |
| `null` | `null` | Default value |
| `false` | `false` | Default value |
| `0` | `0` | Default value |
| `""` | `""` | Default value |

Use default parameters when the default should apply only to omitted or `undefined` arguments.

# 🗁 Default Parameters vs Nullish Coalescing

The nullish coalescing operator (`??`) provides another way to assign fallback values.

It uses the fallback only when the left-hand value is `null` or `undefined`.

```javascript
const username = null ?? "Guest";

console.log(username);
```

Output:

```text
Guest
```

Comparison:

| Input | Default Parameter | `||` | `??` |
|---|---|---|---|
| `undefined` | Default | Default | Default |
| `null` | `null` | Default | Default |
| `false` | `false` | Default | `false` |
| `0` | `0` | Default | `0` |
| `""` | `""` | Default | `""` |

The appropriate choice depends on the intended fallback behavior.

# 🗁 Default Parameters with Expressions

Default values can be expressions rather than simple literals.

```javascript
function calculateTotal(price, quantity = 1) {
  return price * quantity;
}

console.log(calculateTotal(100));
console.log(calculateTotal(100, 5));
```

Output:

```text
100
500
```

Default expressions are evaluated when the function is called and the default is needed.

# 🗁 Default Parameters with Function Calls

A default parameter can use the result of another function call.

```javascript
function getDefaultName() {
  console.log("Generating default name...");

  return "Guest";
}

function greet(name = getDefaultName()) {
  console.log(`Hello, ${name}!`);
}

greet("Alex");
greet();
```

Output:

```text
Hello, Alex!
Generating default name...
Hello, Guest!
```

Notice that `getDefaultName()` is called only when the default value is required.

# 🗁 Default Parameters Can Reference Earlier Parameters

A default parameter can reference a parameter declared before it.

```javascript
function createUser(name, username = name.toLowerCase()) {
  return {
    name,
    username
  };
}

console.log(createUser("Alex"));
```

Output:

```javascript
{
  name: "Alex",
  username: "alex"
}
```

The `username` default expression uses the previously initialized `name` parameter.

However, a default parameter cannot access a later parameter that has not yet been initialized.

```javascript
function example(a = b, b = 10) {
  return a + b;
}

example();
```

This throws a `ReferenceError` because `b` is accessed while it is in its temporal dead zone.

The correct order is:

```javascript
function example(a = 10, b = a) {
  return a + b;
}

console.log(example());
```

Output:

```text
20
```

# 🗁 Default Parameters with Destructuring

Default parameters work with object and array destructuring.

**Object destructuring:**

```javascript
function createUser({ name = "Guest", role = "User" } = {}) {
  return {
    name,
    role
  };
}

console.log(createUser());
console.log(createUser({ name: "Alex" }));
```

Output:

```javascript
{
  name: "Guest",
  role: "User"
}

{
  name: "Alex",
  role: "User"
}
```

The `= {}` provides a default object when the entire argument is omitted or `undefined`.

Without it, calling `createUser()` would throw a `TypeError` because JavaScript cannot destructure `undefined`.

**Array destructuring:**

```javascript
function createCoordinates([x = 0, y = 0] = []) {
  return {
    x,
    y
  };
}

console.log(createCoordinates());
console.log(createCoordinates([10]));
console.log(createCoordinates([10, 20]));
```

Output:

```javascript
{
  x: 0,
  y: 0
}

{
  x: 10,
  y: 0
}

{
  x: 10,
  y: 20
}
```

# 🗁 Default Parameters with Arrow Functions

Arrow functions support default parameters in the same way as traditional functions.

```javascript
const greet = (name = "Guest") => {
  return `Hello, ${name}!`;
};

console.log(greet());
console.log(greet("Alex"));
```

Output:

```text
Hello, Guest!
Hello, Alex!
```

They also support implicit returns:

```javascript
const calculatePrice = (price, quantity = 1) => price * quantity;

console.log(calculatePrice(100));
console.log(calculatePrice(100, 3));
```

Output:

```text
100
300
```

# 🗁 Default Parameters and the `arguments` Object

In non-strict traditional functions, default parameters change the behavior of the `arguments` object.

When a function uses default parameters, its parameter bindings are not synchronized with the corresponding indexed properties of `arguments`.

```javascript
function showValue(value = 10) {
  value = 20;

  console.log(value);
  console.log(arguments[0]);
}

showValue(5);
```

Output:

```text
20
5
```

The parameter variable changes to `20`, but `arguments[0]` retains the originally supplied argument, `5`.

When the argument is omitted:

```javascript
function showValue(value = 10) {
  console.log(value);
  console.log(arguments.length);
  console.log(arguments[0]);
}

showValue();
```

Output:

```text
10
0
undefined
```

The default value initializes the parameter, but it does not create an argument in the `arguments` object.

# 🗁 Default Parameter Initialization

Default parameters are evaluated during each function call, not when the function is defined.

Consider an object created as a default value:

```javascript
function createSettings(settings = {}) {
  settings.enabled = true;

  return settings;
}

const first = createSettings();
const second = createSettings();

console.log(first === second);
```

Output:

```text
false
```

Each call creates a new object because the default expression `{}` is evaluated separately for each invocation.

This avoids the shared mutable default object problem found in some other programming languages.

# 🗁 Practical Example: API Configuration

Default parameters are useful when building functions that accept optional configuration values.

```javascript
function fetchUsers(
  endpoint = "/api/users",
  method = "GET",
  headers = {}
) {
  const options = {
    method,
    headers
  };

  console.log("Endpoint:", endpoint);
  console.log("Options:", options);
}

fetchUsers();

fetchUsers("/api/admins", "POST", {
  "Content-Type": "application/json"
});
```

Output:

```text
Endpoint: /api/users
Options: { method: 'GET', headers: {} }

Endpoint: /api/admins
Options: {
  method: 'POST',
  headers: { 'Content-Type': 'application/json' }
}
```

This example demonstrates how default parameters can simplify APIs and configuration functions.

# 🗁 Common Mistakes

**Assuming `null` triggers a default**

```javascript
function greet(name = "Guest") {
  console.log(name);
}

greet(null);
```

Output:

```text
null
```

Only omitted and `undefined` arguments trigger the default.

**Using logical OR when falsy values are valid**

```javascript
function calculate(value) {
  value = value || 10;

  return value;
}

console.log(calculate(0));
```

Output:

```text
10
```

If `0` is a valid value, use a default parameter or an appropriate nullish check.

**Forgetting to provide a default object for destructuring**

```javascript
function createUser({ name = "Guest" }) {
  return name;
}

createUser();
```

This throws a `TypeError`.

Correct:

```javascript
function createUser({ name = "Guest" } = {}) {
  return name;
}

console.log(createUser());
```

Output:

```text
Guest
```

**Referencing later parameters**

```javascript
function calculate(a = b, b = 10) {
  return a + b;
}
```

This can throw a `ReferenceError` when the default for `a` attempts to access `b` before initialization.

**Assuming default values are evaluated once**

```javascript
function generateId(id = Math.random()) {
  return id;
}

console.log(generateId());
console.log(generateId());
```

Each call generates a new default value.

# 🗁 Best Practices

* Use default parameters for optional function inputs.
* Prefer default parameters over manual `undefined` checks.
* Use `??` when both `null` and `undefined` should trigger a fallback.
* Use `||` only when all falsy values should trigger a fallback.
* Keep default expressions simple and predictable.
* Avoid expensive operations in defaults unless they are genuinely needed.
* Use default objects when destructuring optional configuration arguments.
* Place required parameters before optional parameters whenever possible.
* Document complex defaults when their behavior is not obvious.

# 🗁 Key Takeaways

* Default parameters provide fallback values for function parameters.
* They were introduced in ES6.
* Defaults are used when arguments are omitted or explicitly `undefined`.
* `null`, `false`, `0`, and empty strings do not trigger defaults.
* Default values can be expressions or function calls.
* Default parameters can reference earlier parameters.
* They work with traditional functions, arrow functions, and destructuring.
* Default expressions are evaluated when needed for each function call.
* They help create flexible and predictable function interfaces.
