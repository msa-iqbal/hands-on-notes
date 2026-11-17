# Private Fields

Private fields are class properties that cannot be accessed directly from outside the class in which they are declared.

JavaScript uses the `#` prefix to define private fields.

They provide true encapsulation by restricting access to an object's internal state.

Private fields help developers:

* Protect sensitive internal data.
* Prevent unauthorized property modification.
* Control access to object state.
* Hide implementation details.
* Maintain data integrity.
* Build more predictable class interfaces.

Example:

```javascript
class User {
  #password = "secret123";

  getPassword() {
    return this.#password;
  }
}

const user = new User();

console.log(user.getPassword());
```

Output:

```text
secret123
```

Direct access is not allowed:

```javascript
console.log(user.#password);
```

This produces a `SyntaxError` because private fields cannot be accessed outside their declaring class.

# 🗁 Private Field Syntax

A private field is declared using the `#` prefix followed by its name.

Syntax:

```javascript
class ClassName {
  #privateField;

  constructor(value) {
    this.#privateField = value;
  }
}
```

Example:

```javascript
class Person {
  #name;

  constructor(name) {
    this.#name = name;
  }

  getName() {
    return this.#name;
  }
}

const person = new Person("Muhammad");

console.log(person.getName());
```

Output:

```text
Muhammad
```

The `#name` field is accessible only within the `Person` class body.

# 🗁 Declaring Private Fields

Private fields must be declared inside the class body before they can be accessed.

```javascript
class User {
  #name;
  #email;
  #age;

  constructor(name, email, age) {
    this.#name = name;
    this.#email = email;
    this.#age = age;
  }
}
```

You can also initialize private fields directly during declaration.

```javascript
class User {
  #role = "user";
  #active = true;
  #loginCount = 0;
}
```

Private fields can be initialized either at declaration or inside the constructor.

# 🗁 Accessing Private Fields Inside a Class

Private fields can be accessed and modified by methods belonging to their declaring class.

```javascript
class Counter {
  #count = 0;

  increment() {
    this.#count++;
  }

  decrement() {
    this.#count--;
  }

  getCount() {
    return this.#count;
  }
}

const counter = new Counter();

counter.increment();
counter.increment();
counter.increment();

console.log(counter.getCount());

counter.decrement();

console.log(counter.getCount());
```

Output:

```text
3
2
```

The public methods provide controlled access to the private state.

# 🗁 Private Fields vs Public Fields

JavaScript classes support both public and private fields.

| Feature | Public Field | Private Field |
| --- | --- | --- |
| Syntax | `name` | `#name` |
| External access | Allowed | Not allowed |
| External modification | Allowed | Not allowed |
| Encapsulation | No inherent privacy | Enforced privacy |
| Declaration | Class body | Class body |
| Inheritance | Accessible to subclasses | Not directly accessible to subclasses |

Example:

```javascript
class User {
  name = "Muhammad";
  #password = "secret123";
}

const user = new User();

console.log(user.name);

// console.log(user.#password);
```

Output:

```text
Muhammad
```

The private field cannot be accessed outside the class.

# 🗁 Private Fields Are Not Ordinary Properties

Private fields are not normal object properties.

They are stored separately from ordinary properties and are not directly visible through standard property reflection.

Example:

```javascript
class User {
  #password = "secret123";

  constructor(name) {
    this.name = name;
  }
}

const user = new User("Muhammad");

console.log(Object.keys(user));
console.log(Object.getOwnPropertyNames(user));
console.log(Reflect.ownKeys(user));
```

Output:

```text
["name"]
["name"]
["name"]
```

The private field does not appear in these property inspection results.

# 🗁 Private Fields Cannot Be Accessed with Bracket Notation

Private fields cannot be accessed using string-based property notation.

Incorrect:

```javascript
class User {
  #password = "secret123";
}

const user = new User();

console.log(user["#password"]);
```

Output:

```text
undefined
```

The expression looks for an ordinary property named `"#password"` rather than accessing the private field.

Private field access requires the actual private identifier:

```javascript
this.#password;
```

# 🗁 Private Fields and Getters

A getter can expose controlled read access to a private field.

```javascript
class User {
  #name;

  constructor(name) {
    this.#name = name;
  }

  get name() {
    return this.#name;
  }
}

const user = new User("Muhammad");

console.log(user.name);
```

Output:

```text
Muhammad
```

The caller can read the value without directly accessing the private field.

# 🗁 Private Fields and Setters

A setter can provide controlled modification of private fields.

```javascript
class User {
  #name;

  constructor(name) {
    this.#name = name;
  }

  get name() {
    return this.#name;
  }

  set name(value) {
    if (typeof value !== "string" || !value.trim()) {
      throw new TypeError("Name must be a non-empty string");
    }

    this.#name = value.trim();
  }
}

const user = new User("Muhammad");

user.name = "Alex";

console.log(user.name);
```

Output:

```text
Alex
```

The setter validates the assigned value before updating the private field.

# 🗁 Private Methods

JavaScript also supports private methods.

A private method uses the `#` prefix and can only be called inside its declaring class.

Example:

```javascript
class User {
  #name = "Muhammad";

  #formatName() {
    return this.#name.toUpperCase();
  }

  displayName() {
    return this.#formatName();
  }
}

const user = new User();

console.log(user.displayName());
```

Output:

```text
MUHAMMAD
```

The private method `#formatName()` cannot be called directly from outside the class.

# 🗁 Private Static Fields

Private fields can also be declared as static members.

Static private fields belong to the class itself rather than individual instances.

```javascript
class Application {
  static #version = "1.0.0";

  static getVersion() {
    return this.#version;
  }
}

console.log(Application.getVersion());
```

Output:

```text
1.0.0
```

The private static field is accessed through a static method.

Direct external access is not allowed:

```javascript
console.log(Application.#version);
```

This produces a `SyntaxError`.

# 🗁 Private Static Methods

Static methods can also be private.

```javascript
class Application {
  static #validateVersion(version) {
    return typeof version === "string" && version.length > 0;
  }

  static setVersion(version) {
    if (!this.#validateVersion(version)) {
      throw new TypeError("Invalid version");
    }

    console.log(`Version set to ${version}`);
  }
}

Application.setVersion("2.0.0");
```

Output:

```text
Version set to 2.0.0
```

The private static method is available only within the class body.

# 🗁 Private Fields and Inheritance

Private fields belong exclusively to the class that declares them.

A child class cannot directly access a parent's private fields.

Example:

```javascript
class Person {
  #name = "Muhammad";

  getName() {
    return this.#name;
  }
}

class Developer extends Person {
  displayName() {
    return this.#name;
  }
}

const developer = new Developer();

console.log(developer.getName());
```

Output:

```text
Muhammad
```

Calling `developer.displayName()` throws a `TypeError` because the `Developer` class cannot access `Person`'s private field.

The parent class can expose controlled access through public or protected-style methods.

```javascript
class Person {
  #name = "Muhammad";

  getName() {
    return this.#name;
  }
}

class Developer extends Person {
  displayName() {
    console.log(this.getName());
  }
}

const developer = new Developer();

developer.displayName();
```

Output:

```text
Muhammad
```

The child class uses the public method inherited from the parent.

# 🗁 Private Fields and Inherited Methods

A private field can still be accessed by a parent class's methods when those methods execute on a child instance.

```javascript
class Person {
  #name = "Muhammad";

  getName() {
    return this.#name;
  }
}

class Developer extends Person {}

const developer = new Developer();

console.log(developer.getName());
```

Output:

```text
Muhammad
```

The method is defined inside `Person`, so it has permission to access `Person`'s private field.

Inheritance does not remove that permission.

# 🗁 Private Field Brand Checks

JavaScript uses private-field brand checks to verify whether an object has the private field declared by a particular class.

The `in` operator can check whether an object contains a specific private field.

Example:

```javascript
class User {
  #password = "secret123";

  static hasPassword(object) {
    return #password in object;
  }
}

const user = new User();

console.log(User.hasPassword(user));
console.log(User.hasPassword({}));
```

Output:

```text
true
false
```

This is useful when validating whether an object is an instance carrying a particular class's private state.

A failed brand check can also occur when directly accessing a private field on an object that does not contain that field.

# 🗁 Private Fields and Object Cloning

Private fields are not copied by ordinary object cloning techniques.

Example:

```javascript
class User {
  #password = "secret123";

  constructor(name) {
    this.name = name;
  }

  getPassword() {
    return this.#password;
  }
}

const user = new User("Muhammad");

const clone = { ...user };

console.log(clone.name);
```

Output:

```text
Muhammad
```

The spread operation copies enumerable own public properties, but it does not copy private fields.

Calling `clone.getPassword()` is not possible because the cloned object does not have the method either.

Even if a method is manually assigned to another object, calling it with an object lacking the required private field will fail its private brand check.

# 🗁 Private Fields and Object.freeze()

`Object.freeze()` does not prevent methods of a class from modifying its private fields.

Example:

```javascript
class Counter {
  #count = 0;

  increment() {
    this.#count++;
  }

  getCount() {
    return this.#count;
  }
}

const counter = new Counter();

Object.freeze(counter);

counter.increment();

console.log(counter.getCount());
```

Output:

```text
1
```

`Object.freeze()` affects ordinary object properties, not the internal private-field state.

This distinction is important when designing immutable-looking objects.

# 🗁 Protected Properties vs Private Fields

JavaScript does not provide a native `protected` access modifier like some other object-oriented languages.

A common convention is to prefix internal properties with an underscore.

Example:

```javascript
class Person {
  constructor(name) {
    this._name = name;
  }
}

class Developer extends Person {
  displayName() {
    console.log(this._name);
  }
}

const developer = new Developer("Muhammad");

developer.displayName();
```

Output:

```text
Muhammad
```

The underscore indicates that `_name` is intended for internal use.

However, it does not enforce access restrictions.

By contrast:

```javascript
class Person {
  #name = "Muhammad";
}
```

The `#name` field is enforced as private by the JavaScript language.

| Feature | Underscore Convention | Private Field |
| --- | --- | --- |
| Example | `_name` | `#name` |
| Language-enforced privacy | No | Yes |
| Accessible from subclasses | Yes | No direct access |
| Accessible externally | Yes | No |
| Runtime restriction | No | Yes |

# 🗁 Private Fields and TypeErrors

Accessing a private field from an invalid context causes a `TypeError` at runtime.

Example:

```javascript
class User {
  #name = "Muhammad";

  getName() {
    return this.#name;
  }
}

const user = new User();

const getName = user.getName;

getName();
```

This throws a `TypeError` because the method is called without the correct `this` value.

The method expects an object that contains the `User` class's private field.

One solution is to bind the method:

```javascript
const getName = user.getName.bind(user);

console.log(getName());
```

Output:

```text
Muhammad
```

Alternatively, use an arrow function as a class field when a permanently bound method is appropriate.

# 🗁 Practical Example: Bank Account

Private fields are especially useful for protecting important internal state.

```javascript
class BankAccount {
  #balance = 0;

  constructor(owner, initialBalance = 0) {
    this.owner = owner;
    this.deposit(initialBalance);
  }

  get balance() {
    return this.#balance;
  }

  deposit(amount) {
    if (typeof amount !== "number" || !Number.isFinite(amount)) {
      throw new TypeError("Amount must be a finite number");
    }

    if (amount <= 0) {
      throw new RangeError("Deposit must be positive");
    }

    this.#balance += amount;
  }

  withdraw(amount) {
    if (typeof amount !== "number" || !Number.isFinite(amount)) {
      throw new TypeError("Amount must be a finite number");
    }

    if (amount <= 0) {
      throw new RangeError("Withdrawal must be positive");
    }

    if (amount > this.#balance) {
      throw new RangeError("Insufficient balance");
    }

    this.#balance -= amount;
  }
}

const account = new BankAccount("Muhammad", 1000);

account.deposit(500);
account.withdraw(200);

console.log(account.owner);
console.log(account.balance);
```

Output:

```text
Muhammad
1300
```

The balance cannot be modified directly:

```javascript
account.#balance = 1000000;
```

This is invalid outside the class.

All balance changes must go through the controlled `deposit()` and `withdraw()` methods.

# 🗁 Practical Example: User Authentication

Private fields can store authentication-related state and expose controlled operations.

```javascript
class AuthSession {
  #token;
  #authenticated = false;

  login(token) {
    if (typeof token !== "string" || !token.trim()) {
      throw new TypeError("A valid token is required");
    }

    this.#token = token;
    this.#authenticated = true;
  }

  logout() {
    this.#token = undefined;
    this.#authenticated = false;
  }

  isAuthenticated() {
    return this.#authenticated;
  }

  getAuthorizationHeader() {
    if (!this.#authenticated) {
      throw new Error("User is not authenticated");
    }

    return `Bearer ${this.#token}`;
  }
}

const session = new AuthSession();

session.login("abc123");

console.log(session.isAuthenticated());
console.log(session.getAuthorizationHeader());

session.logout();

console.log(session.isAuthenticated());
```

Output:

```text
true
Bearer abc123
false
```

The internal token is not directly accessible through ordinary object properties.

# 🗁 Common Mistakes

* Forgetting the `#` prefix when declaring or accessing private fields.
* Accessing private fields outside their declaring class.
* Attempting to access a parent's private fields directly from a child class.
* Assuming underscore-prefixed properties are genuinely private.
* Trying to access private fields using bracket notation.
* Assuming `Object.keys()` or `Object.getOwnPropertyNames()` can reveal private fields.
* Expecting object spread to copy private fields.
* Assuming `Object.freeze()` prevents modification of private fields.
* Calling private-field-dependent methods with an incompatible `this` value.
* Attempting to access a private field before it has been declared in the class body.

# 🗁 Best Practices

* Use private fields for internal state that should not be directly accessible.
* Expose controlled operations through public methods and accessors.
* Validate data before modifying private state.
* Keep private methods focused on internal implementation details.
* Use meaningful private field names.
* Avoid exposing sensitive internal values unnecessarily.
* Prefer public methods over direct internal-state manipulation.
* Use inheritance carefully because private fields are not directly shared with subclasses.
* Use composition when several classes need to share independent internal behaviors.
* Remember that private fields provide encapsulation, not automatic encryption or secure storage.

# 🗁 Key Takeaways

* Private fields use the `#` prefix.
* Private fields must be declared inside a class body.
* They cannot be accessed directly outside their declaring class.
* Private fields provide language-enforced encapsulation.
* Public methods and getters can expose controlled access to private state.
* JavaScript supports private instance fields, private methods, and private static members.
* Private fields are not copied through object spread or ordinary property cloning.
* Child classes cannot directly access private fields declared by their parent classes.
* The `in` operator supports private-field brand checks.
* `Object.freeze()` does not prevent class methods from modifying private fields.
* Private fields are useful for building maintainable classes with controlled internal state.
