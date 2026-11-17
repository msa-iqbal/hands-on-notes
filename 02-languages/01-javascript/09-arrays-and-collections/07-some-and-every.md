# JavaScript Array `some()` and `every()`

JavaScript provides the `some()` and `every()` array methods to test whether array elements satisfy a specified condition.

Both methods execute a callback function and return a boolean value (`true` or `false`).

They are commonly used for:

* Validating array data.
* Checking user permissions.
* Verifying product availability.
* Testing application conditions.
* Checking whether records meet business rules.
* Implementing form and data validation.
* Evaluating collections without manually iterating over them.

The main difference:

* `some()` checks whether **at least one element** satisfies a condition.
* `every()` checks whether **all elements** satisfy a condition.

# 🗁 JavaScript `some()`

The `some()` method tests whether at least one element in an array passes a specified condition.

It returns `true` as soon as a matching element is found.

If no element satisfies the condition, it returns `false`.

### 🖆 Syntax

```javascript
const result = array.some(callback);
```

Complete syntax:

```javascript
const result = array.some(
    (element, index, array) => {
        // Return true if the condition is satisfied.
    }
);
```

The callback receives three arguments:

| Parameter | Description |
| --- | --- |
| `element` | Current element being processed |
| `index` | Index of the current element |
| `array` | Original array being processed |

### 🖆 Basic Usage

Check whether an array contains at least one even number.

```javascript
const numbers = [1, 3, 5, 8, 9];

const result = numbers.some(number => number % 2 === 0);

console.log(result);
// true
```

The result is `true` because `8` is even.

### 🖆 Checking Whether a Number Exists

```javascript
const numbers = [10, 20, 30, 40];

const hasThirty = numbers.some(number => number === 30);

console.log(hasThirty);
// true
```

For simple exact-value checks, `includes()` is usually more appropriate.

```javascript
const hasThirty = numbers.includes(30);
```

### 🖆 Checking Whether Any Number Exceeds a Limit

```javascript
const temperatures = [22, 25, 28, 35, 30];

const hasHighTemperature = temperatures.some(
    temperature => temperature > 30
);

console.log(hasHighTemperature);
// true
```

### 🖆 Checking Whether Any String Matches a Pattern

```javascript
const emails = [
    "alice@example.com",
    "bob@example.com",
    "invalid-email"
];

const hasInvalidEmail = emails.some(email => {
    return !email.includes("@");
});

console.log(hasInvalidEmail);
// true
```

This is a basic example of validation, not a complete email-address validation strategy.

# 🗁 Working with Objects Using `some()`

### 🖆 Checking Whether a User Exists

```javascript
const users = [
    { id: 1, name: "Alice" },
    { id: 2, name: "Bob" },
    { id: 3, name: "Charlie" }
];

const exists = users.some(user => user.id === 2);

console.log(exists);
// true
```

### 🖆 Checking Whether Any User Is an Administrator

```javascript
const users = [
    { id: 1, name: "Alice", role: "user" },
    { id: 2, name: "Bob", role: "admin" },
    { id: 3, name: "Charlie", role: "user" }
];

const hasAdmin = users.some(user => user.role === "admin");

console.log(hasAdmin);
// true
```

### 🖆 Checking Product Availability

```javascript
const products = [
    { name: "Keyboard", stock: 0 },
    { name: "Mouse", stock: 15 },
    { name: "Monitor", stock: 0 }
];

const hasAvailableProduct = products.some(
    product => product.stock > 0
);

console.log(hasAvailableProduct);
// true
```

### 🖆 Checking Whether Any Order Is Pending

```javascript
const orders = [
    { id: 101, status: "completed" },
    { id: 102, status: "pending" },
    { id: 103, status: "cancelled" }
];

const hasPendingOrder = orders.some(
    order => order.status === "pending"
);

console.log(hasPendingOrder);
// true
```

# 🗁 JavaScript `every()`

The `every()` method tests whether all elements in an array satisfy a specified condition.

It returns `true` only when every existing array element passes the condition.

If at least one element fails, it returns `false`.

### 🖆 Syntax

```javascript
const result = array.every(callback);
```

Complete syntax:

```javascript
const result = array.every(
    (element, index, array) => {
        // Return true if the condition is satisfied.
    }
);
```

The callback receives three arguments:

| Parameter | Description |
| --- | --- |
| `element` | Current element being processed |
| `index` | Index of the current element |
| `array` | Original array being processed |

### 🖆 Basic Usage

Check whether all numbers are positive.

```javascript
const numbers = [10, 20, 30, 40];

const result = numbers.every(number => number > 0);

console.log(result);
// true
```

Every number is greater than zero, so the result is `true`.

### 🖆 Checking Whether All Numbers Are Even

```javascript
const numbers = [2, 4, 6, 8];

const result = numbers.every(number => number % 2 === 0);

console.log(result);
// true
```

If even one number is odd, the result becomes `false`.

```javascript
const numbers = [2, 4, 5, 8];

const result = numbers.every(number => number % 2 === 0);

console.log(result);
// false
```

### 🖆 Checking Whether All Values Are Strings

```javascript
const values = ["JavaScript", "TypeScript", "Python"];

const result = values.every(value => typeof value === "string");

console.log(result);
// true
```

### 🖆 Checking a Numeric Range

```javascript
const scores = [75, 80, 90, 85];

const result = scores.every(score => {
    return score >= 0 && score <= 100;
});

console.log(result);
// true
```

# 🗁 Working with Objects Using `every()`

### 🖆 Checking Whether All Users Are Active

```javascript
const users = [
    { id: 1, name: "Alice", active: true },
    { id: 2, name: "Bob", active: true },
    { id: 3, name: "Charlie", active: true }
];

const allActive = users.every(user => user.active);

console.log(allActive);
// true
```

### 🖆 Checking Whether All Products Are Available

```javascript
const products = [
    { name: "Keyboard", stock: 10 },
    { name: "Mouse", stock: 5 },
    { name: "Monitor", stock: 8 }
];

const allAvailable = products.every(product => product.stock > 0);

console.log(allAvailable);
// true
```

### 🖆 Checking Whether All Orders Are Completed

```javascript
const orders = [
    { id: 101, status: "completed" },
    { id: 102, status: "completed" },
    { id: 103, status: "completed" }
];

const allCompleted = orders.every(
    order => order.status === "completed"
);

console.log(allCompleted);
// true
```

### 🖆 Validating Required Object Properties

```javascript
const users = [
    { id: 1, name: "Alice", email: "alice@example.com" },
    { id: 2, name: "Bob", email: "bob@example.com" },
    { id: 3, name: "Charlie", email: "charlie@example.com" }
];

const allValid = users.every(user => {
    return Boolean(user.id && user.name && user.email);
});

console.log(allValid);
// true
```

This example checks that the required properties contain truthy values. Real validation may require additional checks for types, formats, and business rules.

# 🗁 Understanding Short-Circuit Behavior

Both methods stop processing elements as soon as their final result is determined.

### 🖆 `some()` Stops at the First Match

```javascript
const numbers = [1, 3, 5, 8, 10];

const result = numbers.some(number => {
    console.log("Checking:", number);

    return number % 2 === 0;
});

console.log(result);
```

Output:

```text
Checking: 1
Checking: 3
Checking: 5
Checking: 8
true
```

The callback is not executed for `10` because `some()` already found a matching element.

### 🖆 `every()` Stops at the First Failure

```javascript
const numbers = [2, 4, 6, 7, 8];

const result = numbers.every(number => {
    console.log("Checking:", number);

    return number % 2 === 0;
});

console.log(result);
```

Output:

```text
Checking: 2
Checking: 4
Checking: 6
Checking: 7
false
```

The callback is not executed for `8` because `every()` already found an element that failed the condition.

# 🗁 Important Difference Between `some()` and `every()`

| Feature | `some()` | `every()` |
| --- | --- | --- |
| Purpose | Check whether at least one element matches | Check whether all elements match |
| Returns | Boolean | Boolean |
| Stops when | First truthy callback result | First falsy callback result |
| Empty array | `false` | `true` |
| Creates a new array | No | No |
| Modifies original array | No, by itself | No, by itself |

### 🖆 Comparison Example

```javascript
const numbers = [2, 4, 6, 7];

const hasEven = numbers.some(number => number % 2 === 0);
const allEven = numbers.every(number => number % 2 === 0);

console.log(hasEven);
// true

console.log(allEven);
// false
```

At least one number is even, but not every number is even.

# 🗁 Empty Array Behavior

An important distinction between these methods is how they handle empty arrays.

### 🖆 `some()` with an Empty Array

```javascript
const numbers = [];

const result = numbers.some(number => number > 0);

console.log(result);
// false
```

No element exists that satisfies the condition.

### 🖆 `every()` with an Empty Array

```javascript
const numbers = [];

const result = numbers.every(number => number > 0);

console.log(result);
// true
```

This behavior is known as *vacuous truth*.

The statement "every element satisfies the condition" is considered true when there are no elements that violate it.

This behavior is important when validating collections.

For example:

```javascript
const tasks = [];

const allCompleted = tasks.every(task => task.completed);

console.log(allCompleted);
// true
```

If an application requires at least one task and requires all tasks to be completed, check both conditions:

```javascript
const allCompleted = tasks.length > 0 &&
    tasks.every(task => task.completed);
```

# 🗁 Combining `some()` and `every()`

These methods can be combined to express more complex conditions.

### 🖆 Checking Whether All Users Are Active and at Least One Is an Admin

```javascript
const users = [
    { id: 1, name: "Alice", role: "admin", active: true },
    { id: 2, name: "Bob", role: "user", active: true },
    { id: 3, name: "Charlie", role: "user", active: true }
];

const allActive = users.every(user => user.active);

const hasAdmin = users.some(user => user.role === "admin");

const isValid = allActive && hasAdmin;

console.log(isValid);
// true
```

### 🖆 Checking Whether a Collection Contains Both Valid and Invalid Records

```javascript
const scores = [75, 85, 120, 90];

const hasValidScore = scores.some(
    score => score >= 0 && score <= 100
);

const allValidScores = scores.every(
    score => score >= 0 && score <= 100
);

console.log(hasValidScore);
// true

console.log(allValidScores);
// false
```

### 🖆 Checking Permissions

```javascript
const permissions = [
    { name: "read", allowed: true },
    { name: "write", allowed: true },
    { name: "delete", allowed: false }
];

const canPerformAnyAction = permissions.some(
    permission => permission.allowed
);

const canPerformAllActions = permissions.every(
    permission => permission.allowed
);

console.log(canPerformAnyAction);
// true

console.log(canPerformAllActions);
// false
```

# 🗁 Practical Use Cases

### 🖆 Form Validation

Check whether any required field is empty.

```javascript
const formData = {
    name: "Alice",
    email: "",
    password: "secret123"
};

const values = Object.values(formData);

const hasEmptyField = values.some(value => value.trim() === "");

console.log(hasEmptyField);
// true
```

Check whether all required fields are filled.

```javascript
const allFieldsFilled = values.every(
    value => value.trim() !== ""
);

console.log(allFieldsFilled);
// false
```

### 🖆 Shopping Cart Validation

Check whether every cart item has sufficient stock.

```javascript
const cart = [
    { productId: 101, quantity: 2, stock: 10 },
    { productId: 102, quantity: 5, stock: 8 },
    { productId: 103, quantity: 4, stock: 3 }
];

const allItemsAvailable = cart.every(item => {
    return item.quantity <= item.stock;
});

console.log(allItemsAvailable);
// false
```

### 🖆 Checking for Expired Products

```javascript
const products = [
    { name: "Milk", expired: false },
    { name: "Bread", expired: false },
    { name: "Cheese", expired: true }
];

const hasExpiredProduct = products.some(product => product.expired);

console.log(hasExpiredProduct);
// true
```

### 🖆 Checking Whether All Tests Passed

```javascript
const testResults = [
    { name: "Authentication", passed: true },
    { name: "Authorization", passed: true },
    { name: "Payment", passed: true }
];

const allTestsPassed = testResults.every(test => test.passed);

console.log(allTestsPassed);
// true
```

# 🗁 Important Characteristics

### 🖆 Both Methods Return Boolean Values

Regardless of the callback's return type, the final result is always a boolean.

```javascript
const numbers = [1, 2, 3];

const result = numbers.some(number => number);

console.log(result);
// true
```

The number `1` is truthy, so `some()` returns `true`.

### 🖆 Both Methods Skip Empty Slots

Unlike `find()`, `some()` and `every()` do not invoke their callbacks for empty slots in sparse arrays.

```javascript
const numbers = [1, , 3];

const result = numbers.every(number => number !== undefined);

console.log(result);
// true
```

The empty slot is skipped, so the callback only evaluates `1` and `3`.

### 🖆 Both Methods Are Non-Mutating by Default

They do not directly modify the original array.

```javascript
const numbers = [1, 2, 3, 4];

const result = numbers.some(number => number > 2);

console.log(numbers);
// [1, 2, 3, 4]
```

However, callback functions can still modify the original array or objects if explicitly written to do so.

# 🗁 Common Mistakes

### 🖆 Confusing `some()` with `every()`

Incorrect assumption:

```javascript
const numbers = [2, 4, 5, 8];

const result = numbers.some(number => number % 2 === 0);

console.log(result);
// true
```

This does not mean every number is even.

Use `every()` for that requirement:

```javascript
const result = numbers.every(number => number % 2 === 0);

console.log(result);
// false
```

### 🖆 Forgetting Empty Array Behavior

```javascript
const users = [];

const allActive = users.every(user => user.active);

console.log(allActive);
// true
```

If an empty collection should be considered invalid, check its length explicitly.

```javascript
const allActive = users.length > 0 &&
    users.every(user => user.active);
```

### 🖆 Using These Methods to Retrieve Elements

Incorrect:

```javascript
const numbers = [10, 20, 30];

const result = numbers.some(number => number > 15);

console.log(result);
// true
```

`some()` returns a boolean, not the matching element.

Use `find()` when you need the first matching element.

```javascript
const result = numbers.find(number => number > 15);

console.log(result);
// 20
```

### 🖆 Using `map()` for Boolean Checks

Avoid:

```javascript
const numbers = [2, 4, 6];

const result = numbers.map(number => number > 0);

console.log(result);
// [true, true, true]
```

If you only need to check whether all elements satisfy a condition, use `every()`.

```javascript
const result = numbers.every(number => number > 0);

console.log(result);
// true
```

# 🗁 Performance Considerations

* Both methods have a worst-case time complexity of \(O(n)\).
* `some()` may stop early when it finds a matching element.
* `every()` may stop early when it finds a failing element.
* Neither method creates a new array.
* Both are suitable for condition checking without allocating intermediate arrays.
* Avoid expensive callback operations when checking large collections.
* For repeated membership checks, consider `Set` instead of repeatedly searching an array.

# 🗁 Best Practices

* Use `some()` when at least one element must satisfy a condition.
* Use `every()` when all elements must satisfy a condition.
* Use `find()` when you need the first matching element.
* Use `filter()` when you need all matching elements.
* Handle empty arrays explicitly when business rules require non-empty collections.
* Keep callback conditions simple and readable.
* Prefer explicit comparisons over confusing truthy or falsy expressions.
* Avoid side effects inside callbacks.
* Take advantage of short-circuit behavior for efficient condition checking.

# 🗁 Summary

The `some()` and `every()` methods are used to evaluate conditions across array elements.

Key points:

* `some()` returns `true` if at least one element satisfies the condition.
* `every()` returns `true` only if all existing elements satisfy the condition.
* Both return boolean values.
* Both stop processing as soon as the final result is known.
* Both skip empty slots in sparse arrays.
* Neither creates a new array.
* `some()` returns `false` for an empty array.
* `every()` returns `true` for an empty array.

**Remember:**

* `some()` means at least one.
* `every()` means all.
