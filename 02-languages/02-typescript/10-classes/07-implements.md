# Implements

The `implements` keyword in TypeScript allows a class to declare that it satisfies the structure defined by an interface.

It ensures that a class provides the required properties and methods specified by the interface.

The `implements` keyword is commonly used for:

* Enforcing class contracts.
* Defining consistent class structures.
* Supporting abstraction.
* Improving code maintainability.
* Implementing multiple interfaces.
* Building loosely coupled application architectures.

Unlike `extends`, which establishes class inheritance, `implements` establishes a structural contract between a class and an interface.

# 🗁 What Is implements?

The `implements` keyword tells TypeScript that a class must satisfy a particular interface.

Example:

```typescript
interface User {
  name: string;
  email: string;

  login(): void;
}

class Customer implements User {
  name: string;
  email: string;

  constructor(name: string, email: string) {
    this.name = name;
    this.email = email;
  }

  login(): void {
    console.log(`${this.name} logged in`);
  }
}
```

Usage:

```typescript
const customer = new Customer(
  "Alice",
  "alice@example.com",
);

customer.login();
```

The `Customer` class implements all the required members defined by the `User` interface.

# 🗁 Basic Syntax

```typescript
interface InterfaceName {
  // Required properties and methods
}

class ClassName implements InterfaceName {
  // Implement required members
}
```

A class implementing an interface must provide compatible implementations for all required instance members.

# 🗁 Implementing Interface Properties

An interface can define properties that a class must implement.

```typescript
interface Product {
  id: number;
  title: string;
  price: number;
}

class PhysicalProduct implements Product {
  id: number;
  title: string;
  price: number;

  constructor(id: number, title: string, price: number) {
    this.id = id;
    this.title = title;
    this.price = price;
  }
}
```

Usage:

```typescript
const product = new PhysicalProduct(
  101,
  "Keyboard",
  75,
);

console.log(product.title);
```

If a required property is missing, TypeScript reports an error.

# 🗁 Implementing Interface Methods

Interfaces can define method signatures without providing implementations.

The implementing class must provide the corresponding methods.

```typescript
interface Calculator {
  add(a: number, b: number): number;
  subtract(a: number, b: number): number;
}

class BasicCalculator implements Calculator {
  add(a: number, b: number): number {
    return a + b;
  }

  subtract(a: number, b: number): number {
    return a - b;
  }
}
```

Usage:

```typescript
const calculator = new BasicCalculator();

console.log(calculator.add(10, 5));
console.log(calculator.subtract(10, 5));
```

Output:

```text
15
5
```

The class must provide compatible method signatures and implementations.

# 🗁 Implementing Multiple Interfaces

A class can implement multiple interfaces.

Separate interface names using commas.

```typescript
interface Printable {
  print(): void;
}

interface Downloadable {
  download(): void;
}

interface Shareable {
  share(): void;
}

class Document implements Printable, Downloadable, Shareable {
  print(): void {
    console.log("Printing document");
  }

  download(): void {
    console.log("Downloading document");
  }

  share(): void {
    console.log("Sharing document");
  }
}
```

Usage:

```typescript
const document = new Document();

document.print();
document.download();
document.share();
```

Output:

```text
Printing document
Downloading document
Sharing document
```

The class must satisfy the requirements of all implemented interfaces.

# 🗁 Implementing Interfaces with Optional Properties

Interfaces can define optional properties using the `?` modifier.

A class does not need to provide an optional property.

```typescript
interface User {
  name: string;
  email?: string;
}

class Customer implements User {
  name: string;

  constructor(name: string) {
    this.name = name;
  }
}
```

The class can also implement the optional property.

```typescript
class Admin implements User {
  name: string;
  email?: string;

  constructor(name: string, email?: string) {
    this.name = name;
    this.email = email;
  }
}
```

Optional properties provide flexibility when not every implementation needs the same information.

# 🗁 Implementing Interfaces with Readonly Properties

Interfaces can define readonly properties.

```typescript
interface User {
  readonly id: number;
  name: string;
}

class Customer implements User {
  readonly id: number;
  name: string;

  constructor(id: number, name: string) {
    this.id = id;
    this.name = name;
  }
}
```

Usage:

```typescript
const customer = new Customer(101, "Alice");

customer.name = "Bob";

// Error: Cannot assign to readonly property
// customer.id = 202;
```

The implementing class must preserve the readonly contract.

# 🗁 Implementing Function Interfaces

An interface can describe a function signature.

A class cannot implement a standalone function signature as a class instance method contract in the same way it implements an ordinary object interface.

However, a class can implement an interface that defines a method accepting or returning functions.

```typescript
interface EventHandler {
  handle(callback: (message: string) => void): void;
}

class Logger implements EventHandler {
  handle(callback: (message: string) => void): void {
    callback("Event handled successfully");
  }
}
```

Usage:

```typescript
const logger = new Logger();

logger.handle((message) => {
  console.log(message);
});
```

Output:

```text
Event handled successfully
```

# 🗁 Implementing Interfaces with Inheritance

A class can extend a parent class and implement one or more interfaces simultaneously.

```typescript
interface Printable {
  print(): void;
}

interface Saveable {
  save(): void;
}

class Document {
  constructor(public title: string) {}

  describe(): void {
    console.log(`Document: ${this.title}`);
  }
}

class PDFDocument extends Document implements Printable, Saveable {
  print(): void {
    console.log("Printing PDF");
  }

  save(): void {
    console.log("Saving PDF");
  }
}
```

Usage:

```typescript
const pdf = new PDFDocument("Annual Report");

pdf.describe();
pdf.print();
pdf.save();
```

A class can extend only one class but implement multiple interfaces.

# 🗁 Implementing an Interface with Access Modifiers

An interface describes the public structure that an object must provide.

A class can use access modifiers internally, but required interface members must remain publicly accessible.

```typescript
interface User {
  name: string;
  login(): void;
}

class Customer implements User {
  public name: string;

  private password: string;

  constructor(name: string, password: string) {
    this.name = name;
    this.password = password;
  }

  public login(): void {
    console.log(`${this.name} logged in`);
  }

  private validatePassword(): boolean {
    return this.password.length >= 8;
  }
}
```

The `name` property and `login()` method satisfy the interface.

The private `password` property and `validatePassword()` method are internal implementation details.

A private or protected member cannot directly satisfy a required public interface member.

# 🗁 Implementing Generic Interfaces

Classes can implement generic interfaces.

```typescript
interface Repository<T> {
  add(item: T): void;
  getAll(): T[];
}

class UserRepository implements Repository<string> {
  private users: string[] = [];

  add(user: string): void {
    this.users.push(user);
  }

  getAll(): string[] {
    return this.users;
  }
}
```

Usage:

```typescript
const repository = new UserRepository();

repository.add("Alice");
repository.add("Bob");

console.log(repository.getAll());
```

Output:

```text
["Alice", "Bob"]
```

A generic class can also implement a generic interface.

```typescript
interface Repository<T> {
  add(item: T): void;
  getAll(): T[];
}

class GenericRepository<T> implements Repository<T> {
  private items: T[] = [];

  add(item: T): void {
    this.items.push(item);
  }

  getAll(): T[] {
    return this.items;
  }
}
```

Usage:

```typescript
const userRepository = new GenericRepository<string>();

userRepository.add("Alice");
userRepository.add("Bob");

const numberRepository = new GenericRepository<number>();

numberRepository.add(10);
numberRepository.add(20);
```

This approach allows reusable classes to work with different data types.

# 🗁 Implementing Extended Interfaces

An interface can extend another interface.

A class implementing the child interface must satisfy the combined requirements.

```typescript
interface Person {
  name: string;
}

interface Employee extends Person {
  employeeId: number;
  department: string;
}

class Developer implements Employee {
  name: string;
  employeeId: number;
  department: string;

  constructor(
    name: string,
    employeeId: number,
    department: string,
  ) {
    this.name = name;
    this.employeeId = employeeId;
    this.department = department;
  }
}
```

The `Developer` class must implement the members inherited from `Person` and the members declared by `Employee`.

# 🗁 Implementing Multiple Extended Interfaces

An interface can extend multiple interfaces.

```typescript
interface Identifiable {
  id: number;
}

interface Timestamped {
  createdAt: Date;
}

interface User extends Identifiable, Timestamped {
  name: string;
}

class Customer implements User {
  id: number;
  createdAt: Date;
  name: string;

  constructor(id: number, name: string) {
    this.id = id;
    this.name = name;
    this.createdAt = new Date();
  }
}
```

This makes it possible to compose reusable interface contracts.

# 🗁 implements vs extends

Both keywords are used in class declarations, but they serve different purposes.

| Feature | implements | extends |
| --- | --- | --- |
| Primary purpose | Satisfy an interface contract | Inherit from a class |
| Used with | Interfaces | Classes |
| Inherits implementation | No | Yes |
| Requires members to be implemented | Yes | Not necessarily |
| Multiple declarations | Multiple interfaces | One parent class |
| Access to parent implementation | No | Yes |
| Runtime behavior | No direct runtime behavior | Establishes class inheritance |

Example:

```typescript
interface Printable {
  print(): void;
}

class Document {
  describe(): void {
    console.log("Document");
  }
}

class PDFDocument extends Document implements Printable {
  print(): void {
    console.log("Printing PDF");
  }
}
```

Here:

* `extends Document` inherits the `describe()` method.
* `implements Printable` requires the `print()` method.

# 🗁 implements vs Type Compatibility

TypeScript uses structural typing.

A class does not always need an explicit `implements` declaration to be compatible with an interface.

```typescript
interface User {
  name: string;
  login(): void;
}

class Customer {
  name = "Alice";

  login(): void {
    console.log("Logged in");
  }
}
```

The `Customer` class does not explicitly implement `User`, but its instance is structurally compatible with that interface.

```typescript
const user: User = new Customer();
```

Adding `implements` makes the intended relationship explicit and ensures that the class declaration satisfies the interface.

Important: `implements` does not change the inferred type of the class or automatically add missing members.

# 🗁 implements Does Not Change Class Member Types

The `implements` keyword checks compatibility with an interface. It does not automatically apply the interface's property types to class members.

Consider:

```typescript
interface User {
  name: string | number;
}

class Customer implements User {
  name = "Alice";
}
```

The class satisfies the interface because `string` is assignable to `string | number`.

However, the inferred type of `Customer.name` remains `string`.

```typescript
const customer = new Customer();

customer.name = "Bob"; // Allowed

// Error: Type 'number' is not assignable to type 'string'
// customer.name = 123;
```

The interface acts as a compatibility requirement, not as a mechanism for changing the class's inferred property types.

# 🗁 implements and Static Members

A class's `implements` declaration checks its instance side.

It does not verify the class's static members or constructor signature.

Consider:

```typescript
interface User {
  name: string;
}

class Customer implements User {
  name: string;

  constructor(name: string) {
    this.name = name;
  }

  static create(): Customer {
    return new Customer("Guest");
  }
}
```

The interface requires only an instance property named `name`.

The static `create()` method is not part of that interface contract.

If a static method or constructor signature must be checked, define a separate construct signature or constructor interface and validate the class value against it.

```typescript
interface UserConstructor {
  new (name: string): User;
  create(): User;
}

const UserClass: UserConstructor = Customer;
```

This checks the constructor and static side of the class.

# 🗁 Practical Example: Payment System

A payment system can use interfaces to define a common contract for different payment providers.

```typescript
interface PaymentProcessor {
  processPayment(amount: number): boolean;
  refundPayment(amount: number): boolean;
}

class CardPayment implements PaymentProcessor {
  processPayment(amount: number): boolean {
    console.log(`Processing card payment: ${amount}`);

    return true;
  }

  refundPayment(amount: number): boolean {
    console.log(`Refunding card payment: ${amount}`);

    return true;
  }
}

class PayPalPayment implements PaymentProcessor {
  processPayment(amount: number): boolean {
    console.log(`Processing PayPal payment: ${amount}`);

    return true;
  }

  refundPayment(amount: number): boolean {
    console.log(`Refunding PayPal payment: ${amount}`);

    return true;
  }
}
```

Create a function that accepts any compatible payment processor:

```typescript
function checkout(
  processor: PaymentProcessor,
  amount: number,
): void {
  const successful = processor.processPayment(amount);

  if (successful) {
    console.log("Payment completed successfully");
  }
}
```

Usage:

```typescript
const card = new CardPayment();
const paypal = new PayPalPayment();

checkout(card, 100);
checkout(paypal, 200);
```

The `checkout()` function does not need to know which payment provider is being used.

It depends only on the interface contract.

# 🗁 Practical Example: Repository Pattern

The repository pattern separates data access operations from application business logic.

```typescript
interface Repository<T> {
  create(item: T): void;
  findAll(): T[];
  findById(id: number): T | undefined;
  delete(id: number): boolean;
}

type User = {
  id: number;
  name: string;
  email: string;
};

class UserRepository implements Repository<User> {
  private users: User[] = [];

  create(user: User): void {
    this.users.push(user);
  }

  findAll(): User[] {
    return [...this.users];
  }

  findById(id: number): User | undefined {
    return this.users.find((user) => user.id === id);
  }

  delete(id: number): boolean {
    const index = this.users.findIndex(
      (user) => user.id === id,
    );

    if (index === -1) {
      return false;
    }

    this.users.splice(index, 1);

    return true;
  }
}
```

Usage:

```typescript
const repository = new UserRepository();

repository.create({
  id: 1,
  name: "Alice",
  email: "alice@example.com",
});

repository.create({
  id: 2,
  name: "Bob",
  email: "bob@example.com",
});

console.log(repository.findAll());

console.log(repository.findById(1));

repository.delete(2);

console.log(repository.findAll());
```

The interface defines the repository contract, while the class provides the implementation.

The same contract could be implemented by a database-backed repository without changing the application code that depends on it.

# 🗁 Common Mistakes

**Forgetting required interface members**

A class must provide all required interface members.

**Using implements instead of extends**

`implements` does not inherit class implementation. Use `extends` when inheriting from a class.

**Trying to implement an interface with private members**

Required interface members must be publicly accessible through the class instance.

**Assuming implements changes inferred property types**

The class must already satisfy the interface. The declaration does not automatically change its inferred member types.

**Expecting implements to validate static members**

The `implements` keyword checks the instance side, not the static side of a class.

**Implementing too many unrelated interfaces**

A class with too many unrelated responsibilities can become difficult to maintain. Prefer focused interfaces.

**Confusing interface inheritance with class inheritance**

Interfaces extend interfaces, while classes extend classes and implement interfaces.

# 🗁 Best Practices

* Use interfaces to define clear class contracts.
* Use `implements` to make intended contracts explicit.
* Keep interfaces focused on specific responsibilities.
* Use multiple interfaces when a class needs several independent capabilities.
* Prefer dependency inversion by depending on interfaces rather than concrete implementations.
* Use generic interfaces for reusable abstractions.
* Use `extends` for actual class inheritance.
* Use constructor interfaces when static or construction requirements must be checked.
* Avoid unnecessary interface implementations.
* Keep implementations compatible with their declared contracts.

# 🗁 Summary

* The `implements` keyword allows a class to satisfy an interface.
* Interfaces define required properties and methods.
* Classes provide the actual implementations.
* A class can implement multiple interfaces.
* A class can extend one class and implement multiple interfaces.
* Generic interfaces can be implemented by generic classes.
* Optional and readonly interface properties are supported.
* `implements` checks structural compatibility without adding runtime behavior.
* It does not automatically change inferred class member types.
* It checks the instance side, not static members or constructor signatures.
* Interfaces help create reusable, maintainable, and loosely coupled architectures.

# 🗁 Related Topics

* Class Basics
* Access Modifiers
* Readonly Properties
* Constructors
* Inheritance
* Abstract Classes
* Static Members
* Parameter Properties
* Interfaces
* Interface Extension
* Generic Interfaces
* Structural Typing
* Dependency Inversion
* Repository Pattern
