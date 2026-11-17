# TypeScript Parameter Properties

Parameter properties are a TypeScript feature that allows you to declare and initialize class properties directly through constructor parameters.

They provide a concise alternative to declaring class properties separately and assigning values inside the constructor.

# 🗁 The Traditional Approach

Normally, you declare class properties and initialize them through constructor parameters.

```typescript
class User {
  name: string;
  email: string;
  age: number;

  constructor(name: string, email: string, age: number) {
    this.name = name;
    this.email = email;
    this.age = age;
  }
}

const user = new User("Alice", "alice@example.com", 25);

console.log(user.name);
console.log(user.email);
console.log(user.age);
```

This approach works, but it requires repetitive property declarations and assignments.

# 🗁 Using Parameter Properties

TypeScript allows you to combine property declaration and initialization using access modifiers or `readonly` in constructor parameters.

```typescript
class User {
  constructor(
    public name: string,
    public email: string,
    public age: number
  ) {}
}

const user = new User("Alice", "alice@example.com", 25);

console.log(user.name);
console.log(user.email);
console.log(user.age);
```

TypeScript automatically:

* Declares the corresponding class properties.
* Assigns constructor arguments to those properties.
* Applies the specified access modifiers.

# 🗁 Supported Parameter Property Modifiers

Parameter properties support these modifiers:

| Modifier | Purpose |
| --- | --- |
| `public` | Creates a publicly accessible property |
| `private` | Creates a property accessible only within the class |
| `protected` | Creates a property accessible within the class and subclasses |
| `readonly` | Prevents reassignment after initialization |

You can combine `readonly` with an access modifier.

# 🗁 Public Parameter Properties

A `public` parameter property creates a property that can be accessed from outside the class.

```typescript
class Product {
  constructor(
    public name: string,
    public price: number
  ) {}
}

const product = new Product("Laptop", 1200);

console.log(product.name);  // Laptop
console.log(product.price); // 1200

product.price = 1100;

console.log(product.price); // 1100
```

The `public` modifier is also the default visibility of ordinary class properties, but it must be explicitly written in a constructor parameter to create a parameter property.

# 🗁 Private Parameter Properties

A `private` parameter property creates a property that can only be accessed within its declaring class.

```typescript
class BankAccount {
  constructor(
    private accountNumber: string,
    private balance: number
  ) {}

  getBalance(): number {
    return this.balance;
  }

  deposit(amount: number): void {
    this.balance += amount;
  }
}

const account = new BankAccount("123456789", 5000);

console.log(account.getBalance()); // 5000

account.deposit(1000);

console.log(account.getBalance()); // 6000

// Error: accountNumber is private.
console.log(account.accountNumber);
```

Private parameter properties are useful for encapsulating internal class data.

# 🗁 Protected Parameter Properties

A `protected` parameter property is accessible within the declaring class and its subclasses.

```typescript
class Employee {
  constructor(
    protected name: string,
    protected salary: number
  ) {}
}

class Manager extends Employee {
  displayInfo(): void {
    console.log(`Name: ${this.name}`);
    console.log(`Salary: ${this.salary}`);
  }
}

const manager = new Manager("Alice", 5000);

manager.displayInfo();

// Error: name is protected.
console.log(manager.name);
```

This is useful when a base class needs to share properties with derived classes without exposing them publicly.

# 🗁 Readonly Parameter Properties

A `readonly` parameter property can be initialized through the constructor but cannot be reassigned afterward.

```typescript
class User {
  constructor(
    public readonly id: number,
    public readonly username: string
  ) {}
}

const user = new User(101, "alice");

console.log(user.id);
console.log(user.username);

// Error: Cannot assign to a read-only property.
user.username = "bob";
```

The `readonly` modifier is useful for identifiers and values that should remain unchanged after object construction.

# 🗁 Combining Access Modifiers and Readonly

You can combine an access modifier with `readonly`.

```typescript
class Configuration {
  constructor(
    public readonly appName: string,
    private readonly apiKey: string,
    protected readonly environment: string
  ) {}

  getApiKey(): string {
    return this.apiKey;
  }
}

const config = new Configuration(
  "Task Manager",
  "secret-key",
  "production"
);

console.log(config.appName);
console.log(config.getApiKey());
```

Each parameter creates a property with the specified visibility and mutability restrictions.

# 🗁 Parameter Properties with Optional Parameters

Parameter properties can also use optional parameters.

```typescript
class User {
  constructor(
    public name: string,
    public email?: string
  ) {}
}

const user1 = new User("Alice");
const user2 = new User("Bob", "bob@example.com");

console.log(user1.email); // undefined
console.log(user2.email); // bob@example.com
```

An optional parameter property has a type that includes `undefined`.

# 🗁 Parameter Properties with Default Values

You can provide default values to parameter properties.

```typescript
class User {
  constructor(
    public name: string,
    public role: string = "user",
    public active: boolean = true
  ) {}
}

const user1 = new User("Alice");

const user2 = new User(
  "Bob",
  "admin",
  false
);

console.log(user1);
console.log(user2);
```

Default values are used when the corresponding arguments are omitted or explicitly passed as `undefined`.

# 🗁 Parameter Properties with Type Annotations

You can specify explicit types for parameter properties.

```typescript
class Product {
  constructor(
    public id: number,
    public name: string,
    public price: number,
    public available: boolean
  ) {}
}

const product = new Product(
  1,
  "Keyboard",
  75,
  true
);
```

TypeScript also supports type inference for parameters with default values.

```typescript
class Settings {
  constructor(
    public theme = "light",
    public notifications = true
  ) {}
}

const settings = new Settings();

console.log(settings.theme);
console.log(settings.notifications);
```

# 🗁 Parameter Properties and Constructor Logic

Parameter properties can be used alongside ordinary constructor logic.

```typescript
class Employee {
  constructor(
    public name: string,
    public salary: number,
    public department: string
  ) {
    if (salary < 0) {
      throw new Error("Salary cannot be negative");
    }

    this.name = name.trim();
    this.department = department.toUpperCase();
  }
}

const employee = new Employee(
  " Alice ",
  5000,
  "development"
);

console.log(employee.name);       // Alice
console.log(employee.department); // DEVELOPMENT
```

The parameter properties are initialized automatically, and additional constructor logic can modify their values when they are writable.

# 🗁 Parameter Properties vs Ordinary Constructor Parameters

Not every constructor parameter creates a class property.

```typescript
class User {
  constructor(
    public name: string,
    age: number
  ) {
    console.log(`Creating user with age: ${age}`);
  }
}

const user = new User("Alice", 25);

console.log(user.name); // Alice

// Error: age is not a class property.
console.log(user.age);
```

Only parameters with an access modifier or `readonly` become parameter properties.

# 🗁 Important Limitations

* Parameter properties can only be declared in constructor parameter lists.
* They require a constructor implementation, not just a constructor declaration in a type-only context.
* Ordinary constructor parameters do not automatically become class properties.
* A parameter property cannot use `#private` syntax.
* Parameter properties are a TypeScript feature and do not represent a special JavaScript runtime construct.
* Avoid repeating a parameter property name in a separate class property declaration.

# 🗁 Best Practices

* Use parameter properties when constructor parameters directly initialize class properties.
* Use `private` or `protected` to encapsulate internal state.
* Use `readonly` for properties that should not be reassigned after construction.
* Prefer ordinary constructor parameters when values are only needed during initialization.
* Keep constructors readable by moving complex initialization logic into dedicated methods when appropriate.
* Avoid excessively long constructor parameter lists; consider using a configuration object when a class requires many inputs.

# 🗁 Key Takeaways

* Parameter properties combine property declaration and constructor initialization.
* Use `public`, `private`, `protected`, or `readonly` to create parameter properties.
* Access modifiers control property visibility.
* `readonly` prevents reassignment after initialization.
* Optional and default parameter properties are supported.
* Ordinary constructor parameters do not automatically become class properties.
* Parameter properties reduce boilerplate and make class constructors more concise.
