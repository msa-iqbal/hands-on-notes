# TypeScript Static Members

Static members are properties and methods that belong to a class itself rather than individual instances of that class.

They are useful for functionality or data shared across all instances, such as utility methods, constants, counters, and factory methods.

# 🗁 Basic Static Properties

Use the `static` keyword to declare a static property.

```typescript
class AppConfig {
  static appName = "My Application";
  static version = "1.0.0";
}

console.log(AppConfig.appName);
console.log(AppConfig.version);
```

Access static properties through the class name, not an instance.

```typescript
const config = new AppConfig();

// Valid
console.log(AppConfig.appName);

// Error: Property 'appName' does not exist on type 'AppConfig'
console.log(config.appName);
```

# 🗁 Static Methods

Static methods also belong to the class itself.

```typescript
class MathUtils {
  static add(a: number, b: number): number {
    return a + b;
  }

  static multiply(a: number, b: number): number {
    return a * b;
  }
}

console.log(MathUtils.add(10, 5));      // 15
console.log(MathUtils.multiply(4, 3));  // 12
```

You do not need to create an instance to call a static method.

# 🗁 Static Members vs Instance Members

Instance members belong to individual objects, while static members belong to the class.

```typescript
class User {
  static totalUsers = 0;

  name: string;

  constructor(name: string) {
    this.name = name;
    User.totalUsers++;
  }

  introduce(): string {
    return `Hello, I am ${this.name}`;
  }

  static getTotalUsers(): number {
    return User.totalUsers;
  }
}

const user1 = new User("Alice");
const user2 = new User("Bob");

console.log(user1.introduce());
console.log(user2.introduce());

console.log(User.getTotalUsers()); // 2
```

In this example:

* `totalUsers` is shared by the class.
* `getTotalUsers()` is a static method.
* `name` belongs to each instance.
* `introduce()` is an instance method.

# 🗁 Accessing Static Members Inside a Class

Use the class name to access static members from other static methods.

```typescript
class Calculator {
  static defaultValue = 10;

  static getDefaultValue(): number {
    return Calculator.defaultValue;
  }

  static doubleDefaultValue(): number {
    return Calculator.defaultValue * 2;
  }
}

console.log(Calculator.getDefaultValue());     // 10
console.log(Calculator.doubleDefaultValue());  // 20
```

You can also use `this` inside a static method to refer to the class on which the method is invoked.

```typescript
class Counter {
  static count = 0;

  static increment(): void {
    this.count++;
  }

  static getCount(): number {
    return this.count;
  }
}

Counter.increment();
Counter.increment();

console.log(Counter.getCount()); // 2
```

# 🗁 Static Methods Cannot Directly Access Instance Members

Static methods do not have access to instance-specific properties or methods because they execute without an instance.

```typescript
class Person {
  name = "Alice";

  static getName(): string {
    // Error: An instance member cannot be accessed directly.
    // return this.name;
    return "No instance available";
  }
}
```

To access instance members, pass an instance to the static method.

```typescript
class Person {
  constructor(public name: string) {}

  static getName(person: Person): string {
    return person.name;
  }
}

const person = new Person("Alice");

console.log(Person.getName(person)); // Alice
```

# 🗁 Static Readonly Properties

Combine `static` and `readonly` to define class-level constants.

```typescript
class Application {
  static readonly APP_NAME = "My Application";
  static readonly MAX_USERS = 100;
}

console.log(Application.APP_NAME);
console.log(Application.MAX_USERS);

// Error: Cannot assign to a read-only property.
Application.MAX_USERS = 200;
```

Static readonly properties are useful for configuration values and constants shared throughout an application.

# 🗁 Static Members and Inheritance

Static members are inherited by derived classes.

```typescript
class Animal {
  static category = "Living Being";

  static describe(): string {
    return `Category: ${this.category}`;
  }
}

class Dog extends Animal {}

console.log(Dog.category);
console.log(Dog.describe());
```

Output:

```text
Living Being
Category: Living Being
```

A derived class can also define its own static member with the same name.

```typescript
class Animal {
  static category = "Animal";
}

class Dog extends Animal {
  static category = "Dog";
}

console.log(Animal.category); // Animal
console.log(Dog.category);   // Dog
```

The derived class's static property shadows the inherited property when accessed through the derived class.

# 🗁 Static Blocks

Static initialization blocks allow you to execute initialization logic when a class is initialized.

```typescript
class Database {
  static connectionStatus: string;

  static {
    console.log("Initializing database configuration...");

    Database.connectionStatus = "Disconnected";
  }
}

console.log(Database.connectionStatus);
```

A class can contain multiple static initialization blocks. They execute in declaration order.

```typescript
class Configuration {
  static settings: string[] = [];

  static {
    this.settings.push("Database configured");
  }

  static {
    this.settings.push("Cache configured");
  }
}

console.log(Configuration.settings);
```

Static initialization blocks are useful when initializing static properties requires multiple statements or more complex logic.

# 🗁 Practical Example: Application Configuration

```typescript
class AppConfig {
  static readonly APP_NAME = "Task Manager";
  static readonly API_URL = "https://api.example.com";
  static readonly API_VERSION = "v1";

  static getApiEndpoint(): string {
    return `${this.API_URL}/${this.API_VERSION}`;
  }

  static displayInfo(): void {
    console.log(`Application: ${this.APP_NAME}`);
    console.log(`API Endpoint: ${this.getApiEndpoint()}`);
  }
}

AppConfig.displayInfo();
```

Output:

```text
Application: Task Manager
API Endpoint: https://api.example.com/v1
```

This pattern is useful for application-wide configuration that does not need individual class instances.

# 🗁 Practical Example: ID Generator

```typescript
class IDGenerator {
  private static currentId = 0;

  static generate(): number {
    this.currentId++;
    return this.currentId;
  }

  static reset(): void {
    this.currentId = 0;
  }
}

console.log(IDGenerator.generate()); // 1
console.log(IDGenerator.generate()); // 2
console.log(IDGenerator.generate()); // 3

IDGenerator.reset();

console.log(IDGenerator.generate()); // 1
```

The static counter is shared across every call to the class method.

# 🗁 Best Practices

* Use static members for functionality that belongs to the class rather than individual instances.
* Use `static readonly` for class-level constants.
* Avoid storing mutable global application state in static properties unless shared state is intentional.
* Keep static methods independent of instance-specific data.
* Prefer instance methods when behavior depends on an object's properties.
* Use descriptive names for static utility methods and constants.
* Avoid creating unnecessary class instances just to access static functionality.

# 🗁 Key Takeaways

* Static properties and methods belong to the class itself.
* Use the `static` keyword to declare static members.
* Access static members through the class name.
* Static methods cannot directly access instance members.
* Static members are inherited by derived classes.
* `static readonly` is useful for class-level constants.
* Static initialization blocks execute when a class is initialized.
