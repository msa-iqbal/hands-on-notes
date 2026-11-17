# JavaScript Switch Statement

The **`switch` statement** is a conditional control-flow statement that executes different blocks of code based on the value of an expression.

It provides an alternative to multiple `if...else if` statements when comparing one expression against several possible values.

```javascript
const day = "Monday";

switch (day) {
    case "Monday":
        console.log("Start of the work week");
        break;

    case "Friday":
        console.log("Almost weekend");
        break;

    case "Sunday":
        console.log("Weekend");
        break;

    default:
        console.log("Regular day");
}
```

**Output**

```text
Start of the work week
```

JavaScript evaluates `day`, compares its value with each `case`, and executes the matching case.

# 🗁 Switch Statement Syntax

```javascript
switch (expression) {
    case value1:
        // Code for value1
        break;

    case value2:
        // Code for value2
        break;

    case value3:
        // Code for value3
        break;

    default:
        // Code when no case matches
}
```

**Components**

| Component    | Description                               |
| ------------ | ----------------------------------------- |
| `switch`     | Starts the selection statement            |
| `expression` | Value evaluated once before case matching |
| `case`       | Defines a possible matching value         |
| `break`      | Exits the switch statement                |
| `default`    | Executes when no case matches             |

# 🗁 The switch Expression

The expression inside `switch` is evaluated once. Its resulting value is compared against the values specified by the `case` clauses.

```javascript
const role = "admin";

switch (role) {
    case "admin":
        console.log("Administrator");
        break;

    case "editor":
        console.log("Editor");
        break;

    case "user":
        console.log("Regular user");
        break;

    default:
        console.log("Unknown role");
}
```

The expression can be a variable, function call, or another valid expression.

```javascript
function getStatus() {
    return 200;
}

switch (getStatus()) {
    case 200:
        console.log("Success");
        break;

    case 404:
        console.log("Not found");
        break;

    default:
        console.log("Unknown status");
}
```

# 🗁 The case Clause

A `case` clause defines a value that JavaScript compares with the switch expression.

```javascript
const color = "blue";

switch (color) {
    case "red":
        console.log("Red color");
        break;

    case "blue":
        console.log("Blue color");
        break;

    case "green":
        console.log("Green color");
        break;
}
```

JavaScript uses strict equality (`===`) for case matching.

Therefore, values must match in both type and value.

```javascript
const value = "10";

switch (value) {
    case 10:
        console.log("Number ten");
        break;

    case "10":
        console.log("String ten");
        break;
}
```

**Output**

```text
String ten
```

A string containing `"10"` does not match the number `10`.

# 🗁 The break Statement

The `break` statement terminates the current switch statement and transfers execution to the statement following it.

```javascript
const day = 2;

switch (day) {
    case 1:
        console.log("Monday");
        break;

    case 2:
        console.log("Tuesday");
        break;

    case 3:
        console.log("Wednesday");
        break;
}
```

**Output**

```text
Tuesday
```

Without `break`, execution continues into subsequent case clauses. This behavior is called **fall-through**.

```javascript
const day = 2;

switch (day) {
    case 1:
        console.log("Monday");

    case 2:
        console.log("Tuesday");

    case 3:
        console.log("Wednesday");
}
```

**Output**

```text
Tuesday
Wednesday
```

JavaScript begins execution at the matching case and continues until it encounters a `break` or reaches the end of the switch statement.

# 🗁 The default Clause

The `default` clause executes when no case matches the switch expression.

It is optional and can appear anywhere inside the switch statement, although placing it at the end is the common convention.

```javascript
const browser = "Safari";

switch (browser) {
    case "Chrome":
        console.log("Google Chrome");
        break;

    case "Firefox":
        console.log("Mozilla Firefox");
        break;

    case "Edge":
        console.log("Microsoft Edge");
        break;

    default:
        console.log("Other browser");
}
```

**Output**

```text
Other browser
```

The `default` clause does not require a `break` when it is the final clause.

# 🗁 Multiple Cases with the Same Execution Block

Multiple `case` clauses can share the same execution block.

This is useful when different values should produce the same result.

```javascript
const day = "Saturday";

switch (day) {
    case "Saturday":
    case "Sunday":
        console.log("Weekend");
        break;

    case "Monday":
    case "Tuesday":
    case "Wednesday":
    case "Thursday":
    case "Friday":
        console.log("Weekday");
        break;

    default:
        console.log("Invalid day");
}
```

**Output**

```text
Weekend
```

This technique intentionally uses fall-through between matching case labels.

# 🗁 Switch with Multiple Statements

Each case can contain multiple statements.

```javascript
const role = "admin";

switch (role) {
    case "admin":
        console.log("Role: Administrator");
        console.log("Permission: Full access");
        console.log("Dashboard: Admin panel");
        break;

    case "editor":
        console.log("Role: Editor");
        console.log("Permission: Content management");
        console.log("Dashboard: Editor panel");
        break;

    default:
        console.log("Role: Guest");
        console.log("Permission: Read-only");
}
```

A case can contain variable declarations, function calls, calculations, and other valid JavaScript statements.

# 🗁 Switch with Block Scope

The `switch` statement does not automatically create a separate lexical scope for each case clause.

This can cause problems when declaring variables with `let` or `const` in different cases.

**Potential problem**

```javascript
const role = "admin";

switch (role) {
    case "admin":
        const message = "Administrator";
        console.log(message);
        break;

    case "user":
        // const message = "Regular user";
        // SyntaxError: Identifier 'message' has already been declared
        break;
}
```

Even though the declarations appear in different cases, they belong to the same switch block.

**Using braces to create separate scopes**

```javascript
const role = "admin";

switch (role) {
    case "admin": {
        const message = "Administrator";
        console.log(message);
        break;
    }

    case "user": {
        const message = "Regular user";
        console.log(message);
        break;
    }
}
```

Each case now has its own block scope.

# 🗁 Switch with Function Calls

A switch statement can execute different functions based on a selected value.

```javascript
function createUser() {
    console.log("Creating user...");
}

function updateUser() {
    console.log("Updating user...");
}

function deleteUser() {
    console.log("Deleting user...");
}

const action = "update";

switch (action) {
    case "create":
        createUser();
        break;

    case "update":
        updateUser();
        break;

    case "delete":
        deleteUser();
        break;

    default:
        console.log("Unknown action");
}
```

This pattern is useful for command handlers, application actions, and event processing.

# 🗁 Switch with Return

When using `switch` inside a function, `return` can replace `break` because it exits the entire function.

```javascript
function getDayName(day) {
    switch (day) {
        case 1:
            return "Monday";

        case 2:
            return "Tuesday";

        case 3:
            return "Wednesday";

        default:
            return "Unknown day";
    }
}

console.log(getDayName(2)); // "Tuesday"
console.log(getDayName(7)); // "Unknown day"
```

Once a `return` statement executes, the function ends immediately.

# 🗁 Switch vs if...else

Both `switch` and `if...else` support conditional execution, but they are suited to different situations.

**Using switch**

```javascript
const status = 404;

switch (status) {
    case 200:
        console.log("Success");
        break;

    case 404:
        console.log("Not found");
        break;

    case 500:
        console.log("Server error");
        break;

    default:
        console.log("Unknown status");
}
```

**Using if...else**

```javascript
const status = 404;

if (status === 200) {
    console.log("Success");
} else if (status === 404) {
    console.log("Not found");
} else if (status === 500) {
    console.log("Server error");
} else {
    console.log("Unknown status");
}
```

**Comparison**

| Feature | `switch` | `if...else` |
| --- | --- | --- |
| Primary use | Matching one expression against values | Evaluating arbitrary conditions |
| Comparison | Strict equality (`===`) | Supports all Boolean expressions |
| Range conditions | Not directly supported | Supported |
| Multiple matching values | Shared cases through fall-through | Logical operators |
| Fall-through | Possible | Not applicable |
| Readability | Useful for many discrete values | Useful for complex conditions |

**Use `switch` when:**

* Comparing one expression against several fixed values.
* Handling application states or command types.
* Matching HTTP status codes.
* Mapping enum-like values to behavior.

**Use `if...else` when:**

* Checking numeric ranges.
* Combining multiple independent conditions.
* Evaluating complex Boolean expressions.
* Comparing different variables.

# 🗁 Switch with Range Conditions

A standard `switch` statement compares values using strict equality. It does not directly support range comparisons.

However, `switch (true)` allows case expressions to evaluate to Boolean values.

```javascript
const marks = 85;

switch (true) {
    case marks >= 90:
        console.log("Grade A");
        break;

    case marks >= 80:
        console.log("Grade B");
        break;

    case marks >= 70:
        console.log("Grade C");
        break;

    default:
        console.log("Grade F");
}
```

**Output**

```text
Grade B
```

Each case expression evaluates to `true` or `false`, and the switch expression is also `true`.

JavaScript selects the first case whose result is strictly equal to `true`.

Although valid, `if...else if` is generally more natural and readable for range-based conditions.

# 🗁 Practical Examples

**Day of the week**

```javascript
const day = 3;

switch (day) {
    case 1:
        console.log("Monday");
        break;

    case 2:
        console.log("Tuesday");
        break;

    case 3:
        console.log("Wednesday");
        break;

    case 4:
        console.log("Thursday");
        break;

    case 5:
        console.log("Friday");
        break;

    case 6:
        console.log("Saturday");
        break;

    case 7:
        console.log("Sunday");
        break;

    default:
        console.log("Invalid day");
}
```

**Simple calculator**

```javascript
const num1 = 20;
const num2 = 10;
const operator = "+";

let result;

switch (operator) {
    case "+":
        result = num1 + num2;
        break;

    case "-":
        result = num1 - num2;
        break;

    case "*":
        result = num1 * num2;
        break;

    case "/":
        result = num2 !== 0
            ? num1 / num2
            : "Cannot divide by zero";
        break;

    default:
        result = "Invalid operator";
}

console.log(result); // 30
```

**HTTP status code handler**

```javascript
const statusCode = 404;

switch (statusCode) {
    case 200:
        console.log("OK");
        break;

    case 201:
        console.log("Created");
        break;

    case 400:
        console.log("Bad Request");
        break;

    case 401:
        console.log("Unauthorized");
        break;

    case 403:
        console.log("Forbidden");
        break;

    case 404:
        console.log("Not Found");
        break;

    case 500:
        console.log("Internal Server Error");
        break;

    default:
        console.log("Unhandled HTTP status");
}
```

**User role permissions**

```javascript
const role = "editor";

switch (role) {
    case "admin":
        console.log("Create, read, update, and delete");
        break;

    case "editor":
        console.log("Create, read, and update");
        break;

    case "viewer":
        console.log("Read only");
        break;

    default:
        console.log("No permissions");
}
```

**Application theme selection**

```javascript
const theme = "dark";

function applyTheme(theme) {
    switch (theme) {
        case "light":
            document.body.classList.remove("dark");
            document.body.classList.add("light");
            break;

        case "dark":
            document.body.classList.remove("light");
            document.body.classList.add("dark");
            break;

        case "system":
            console.log("Use system theme preference");
            break;

        default:
            console.log("Unknown theme");
    }
}

applyTheme(theme);
```

# 🗁 Common Mistakes

**Forgetting break**

```javascript
const value = 1;

switch (value) {
    case 1:
        console.log("One");

    case 2:
        console.log("Two");
}
```

Both messages execute because there is no `break`.

Use `break` unless fall-through is intentional.

**Using the wrong data type**

```javascript
const value = "10";

switch (value) {
    case 10:
        console.log("Matched");
        break;

    default:
        console.log("No match");
}
```

The string `"10"` does not strictly equal the number `10`.

Use consistent data types or convert the value before comparison.

**Duplicating case values**

```javascript
switch ("admin") {
    case "admin":
        console.log("Administrator");
        break;

    case "admin":
        console.log("Another administrator");
        break;
}
```

Only the first matching case executes. The second duplicate case is effectively unreachable.

Keep case values unique unless there is a deliberate reason for a different structure.

**Using multiple conditions as if switch supported ranges**

```javascript
const age = 25;

switch (age) {
    case age >= 18:
        console.log("Adult");
        break;
}
```

The case expression evaluates to a Boolean, which is compared against the numeric switch value. It does not perform a range match.

Use `if...else` for range comparisons.

**Declaring duplicate lexical variables across cases**

```javascript
switch ("admin") {
    case "admin":
        const message = "Admin";
        break;

    case "user":
        // const message = "User"; // SyntaxError
        break;
}
```

Wrap case bodies in braces when separate lexical scopes are required.

# 🗁 Best Practices

* Use `switch` when comparing one expression against multiple fixed values.
* Use `if...else` for ranges and complex Boolean conditions.
* Remember that case matching uses strict equality.
* Use `break` to prevent accidental fall-through.
* Use intentional fall-through only when multiple cases share behavior.
* Include a `default` clause when unmatched values need handling.
* Keep case bodies focused and readable.
* Use braces around case bodies when declaring local `let` or `const` variables.
* Prefer `return` when a switch statement directly determines a function's result.
* Avoid duplicate case values.
* Keep input types consistent before matching values.

# 🗁 Key Takeaways

* The `switch` statement selects a code block based on an expression.
* The expression is evaluated once.
* Each `case` defines a possible matching value.
* Case matching uses strict equality (`===`).
* `break` terminates the switch statement.
* Missing `break` statements can cause fall-through.
* `default` handles unmatched values.
* Multiple cases can share one execution block.
* Case bodies can use block scopes with braces.
* `return` can replace `break` inside functions.
* `switch` is suitable for discrete value matching, while `if...else` is generally better for complex conditions and ranges.
