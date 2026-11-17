# Access Modifiers

Access modifiers in TypeScript control where class properties and methods can be accessed.

They are used to implement encapsulation by controlling access to internal class members.

TypeScript provides three primary access modifiers:

* `public`
* `protected`
* `private`

It also supports JavaScript's native private field syntax using `#`.

Access modifiers help define clear boundaries between a class's public API and its internal implementation.

# 🗁 Public

The `public` modifier allows a class member to be accessed from anywhere.

It is the default access level for class members.

```typescript
class User {
  public name: string;

  constructor(name: string) {
    this.name = name;
  }

  public greet(): void {
    console.log(`Hello, ${this.name}`);
  }
}
```

Usage:

```typescript
const user = new User("Alice");

console.log(user.name);

user.greet();

user.name = "Bob";
```

Output:

```text
Alice
Hello, Alice
```

The `public` modifier is optional because class members are public by default.

```typescript
class User {
  name: string;

  greet(): void {
    console.log(`Hello, ${this.name}`);
  }
}
```

Both examples have the same accessibility.

# 🗁 Protected

The `protected` modifier allows a member to be accessed:

* Inside the class where it is declared.
* Inside subclasses that inherit from that class.

It cannot be accessed directly from outside the class hierarchy.

```typescript
class Animal {
  protected name: string;

  constructor(name: string) {
    this.name = name;
  }

  protected makeSound(): void {
    console.log("Animal makes a sound");
  }
}

class Dog extends Animal {
  bark(): void {
    console.log(`${this.name} is barking`);
    this.makeSound();
  }
}
```

Usage:

```typescript
const dog = new Dog("Buddy");

dog.bark();

// Error: Property 'name' is protected
// console.log(dog.name);

// Error: Property 'makeSound' is protected
// dog.makeSound();
```

Output:

```text
Buddy is barking
Animal makes a sound
```

The subclass can access `name` and `makeSound()` because they are protected members inherited from `Animal`.

# 🗁 Private

The `private` modifier restricts access to the class in which the member is declared.

It cannot be accessed directly from outside that class or from subclasses.

```typescript
class BankAccount {
  private balance: number;

  constructor(initialBalance: number) {
    this.balance = initialBalance;
  }

  deposit(amount: number): void {
    this.balance += amount;
  }

  getBalance(): number {
    return this.balance;
  }
}
```

Usage:

```typescript
const account = new BankAccount(1000);

account.deposit(500);

console.log(account.getBalance());

// Error: Property 'balance' is private
// console.log(account.balance);
```

Output:

```text
1500
```

The `balance` property can only be accessed directly inside `BankAccount`.

Public methods provide controlled access to the private data.

# 🗁 Private Members and Inheritance

A subclass cannot directly access private members declared in its parent class.

```typescript
class User {
  private password: string;

  constructor(password: string) {
    this.password = password;
  }
}

class Admin extends User {
  showPassword(): void {
    // Error: Property 'password' is private
    // console.log(this.password);
  }
}
```

If a subclass needs access to a parent member, use `protected` instead.

```typescript
class User {
  protected role: string = "user";
}

class Admin extends User {
  showRole(): void {
    console.log(this.role);
  }
}
```

# 🗁 Comparing Access Modifiers

| Feature | public | protected | private |
| --- | --- | --- | --- |
| Accessible inside declaring class | Yes | Yes | Yes |
| Accessible outside class | Yes | No | No |
| Accessible in subclasses | Yes | Yes | No |
| Default access level | Yes | No | No |
| TypeScript compile-time restriction | No | Yes | Yes |

The key distinction is the accessibility boundary:

* `public`: Anywhere.
* `protected`: Declaring class and subclasses.
* `private`: Declaring class only.

# 🗁 Constructor Access Modifiers

Access modifiers can also be applied to constructors.

A `public` constructor allows normal instantiation.

```typescript
class User {
  public constructor(
    public name: string,
  ) {}
}

const user = new User("Alice");
```

A `protected` constructor prevents direct instantiation outside the class and its subclasses.

```typescript
class DatabaseConnection {
  protected constructor() {}
}

// Error: Constructor is protected
// const connection = new DatabaseConnection();

class MySQLConnection extends DatabaseConnection {
  constructor() {
    super();
  }
}

const connection = new MySQLConnection();
```

A `private` constructor prevents external instantiation and subclassing.

```typescript
class Singleton {
  private constructor() {}

  static create(): Singleton {
    return new Singleton();
  }
}

const instance = Singleton.create();

// Error: Constructor is private
// const another = new Singleton();
```

Private constructors are useful for controlling object creation, such as implementing factory methods or singleton patterns.

# 🗁 Access Modifiers in Getters and Setters

Access modifiers can be applied to getters and setters.

```typescript
class User {
  private _email: string;

  constructor(email: string) {
    this._email = email;
  }

  public get email(): string {
    return this._email;
  }

  public set email(value: string) {
    if (!value.includes("@")) {
      throw new Error("Invalid email address");
    }

    this._email = value;
  }
}
```

Usage:

```typescript
const user = new User("alice@example.com");

console.log(user.email);

user.email = "bob@example.com";

console.log(user.email);
```

The private `_email` property cannot be accessed directly from outside the class.

The public getter and setter provide controlled access.

# 🗁 TypeScript private vs JavaScript Private Fields

TypeScript's `private` modifier and JavaScript's `#` private field syntax are different.

**TypeScript private**

```typescript
class User {
  private password: string;

  constructor(password: string) {
    this.password = password;
  }
}
```

TypeScript enforces the restriction during type checking.

However, the `private` modifier is generally erased during JavaScript compilation. It does not provide the same runtime privacy guarantee as JavaScript private fields.

**JavaScript private fields**

```typescript
class User {
  #password: string;

  constructor(password: string) {
    this.#password = password;
  }

  verifyPassword(password: string): boolean {
    return this.#password === password;
  }
}
```

JavaScript enforces `#password` privacy at runtime.

Attempting to access it outside the class results in a runtime error.

```typescript
const user = new User("secret");

console.log(user.verifyPassword("secret"));

// Syntax error:
// console.log(user.#password);
```

JavaScript private fields are also checked by TypeScript.

# 🗁 TypeScript private vs JavaScript Private Fields Comparison

| Feature | TypeScript `private` | JavaScript `#private` |
| --- | --- | --- |
| Compile-time restriction | Yes | Yes |
| Runtime privacy | No | Yes |
| Native JavaScript feature | No | Yes |
| Visible as ordinary property at runtime | Generally yes | No |
| Access from subclasses | No | No |
| Compatible with older JavaScript targets | Yes, as a TypeScript modifier | Depends on compilation target and output |

Use TypeScript `private` when compile-time access control is sufficient.

Use JavaScript `#private` when runtime-enforced privacy is required.

# 🗁 Access Modifiers and Structural Compatibility

TypeScript generally uses structural typing.

However, private and protected members introduce an important exception.

When comparing class instance types, private and protected members must originate from the same class declaration to be compatible.

```typescript
class User {
  private id: number;

  constructor(id: number) {
    this.id = id;
  }
}

class Admin {
  private id: number;

  constructor(id: number) {
    this.id = id;
  }
}

let user: User;

const admin = new Admin(1);

// Error: Private members originate from different classes
// user = admin;
```

Even though both classes have the same property name and type, their private members originate from different declarations.

# 🗁 Practical Example: Bank Account

The following example demonstrates encapsulation using access modifiers.

```typescript
class BankAccount {
  public readonly accountNumber: string;

  protected owner: string;

  private balance: number;

  constructor(
    accountNumber: string,
    owner: string,
    initialBalance: number,
  ) {
    this.accountNumber = accountNumber;
    this.owner = owner;
    this.balance = initialBalance;
  }

  public deposit(amount: number): void {
    if (amount <= 0) {
      throw new Error("Deposit amount must be positive");
    }

    this.balance += amount;
  }

  public withdraw(amount: number): void {
    if (amount <= 0) {
      throw new Error("Withdrawal amount must be positive");
    }

    if (amount > this.balance) {
      throw new Error("Insufficient balance");
    }

    this.balance -= amount;
  }

  public getBalance(): number {
    return this.balance;
  }
}

class SavingsAccount extends BankAccount {
  public showOwner(): void {
    console.log(`Account owner: ${this.owner}`);
  }
}
```

Usage:

```typescript
const account = new SavingsAccount(
  "ACC-1001",
  "Alice",
  5000,
);

account.deposit(1000);

account.withdraw(500);

account.showOwner();

console.log(account.accountNumber);
console.log(account.getBalance());
```

Output:

```text
Account owner: Alice
ACC-1001
5500
```

The example demonstrates:

* `public`: Account operations and account number are externally accessible.
* `protected`: The owner is accessible within the class hierarchy.
* `private`: The balance is accessible only inside `BankAccount`.
* `readonly`: The account number cannot be reassigned after initialization.

# 🗁 Common Mistakes

**Assuming members are private by default**

Class properties and methods are public by default.

Use explicit access modifiers when they communicate important design boundaries.

**Using protected for everything**

Protected members expose implementation details to subclasses.

Use them only when subclasses genuinely need access.

**Assuming TypeScript private guarantees runtime security**

TypeScript's `private` modifier is a compile-time restriction, not a security boundary.

Use JavaScript `#private` fields when runtime privacy is necessary.

**Accessing private parent members in subclasses**

Private members are not directly accessible in subclasses.

Use protected members or public/protected methods to expose the required behavior.

**Making every property public**

Public properties allow external code to modify internal state without validation.

Use private properties and controlled methods or accessors when invariants must be maintained.

# 🗁 Best Practices

* Keep the public API as small as practical.
* Prefer `private` for internal implementation details.
* Use `protected` only when subclasses need access.
* Use `public` for intentionally exposed members.
* Use `readonly` for values that should not be reassigned.
* Use getters and setters when access requires validation or controlled behavior.
* Choose JavaScript `#private` when runtime privacy matters.
* Avoid exposing sensitive internal state unnecessarily.
* Keep inheritance boundaries clear.
* Do not confuse access modifiers with application-level authorization or security controls.

# 🗁 Summary

* TypeScript provides `public`, `protected`, and `private` access modifiers.
* Public members can be accessed from anywhere.
* Protected members can be accessed inside the declaring class and its subclasses.
* Private members can only be accessed inside the declaring class.
* Access modifiers apply to class properties, methods, and constructors.
* TypeScript `private` provides compile-time access restrictions.
* JavaScript `#private` fields provide runtime-enforced privacy.
* Private and protected members affect class type compatibility.
* Access modifiers help implement encapsulation and maintain controlled class APIs.

# 🗁 Related Topics

* Class Basics
* Readonly Properties
* Constructors
* Inheritance
* Abstract Classes
* Implements
* Static Members
* Parameter Properties
* Encapsulation
* JavaScript Private Fields
* Structural Typing
