# Constructor

A constructor is a special method inside a JavaScript class that initializes newly created instances.

It is automatically executed when an object is created using the `new` operator.

The constructor is commonly used to:

* Initialize instance properties.
* Assign values passed through arguments.
* Set default values.
* Perform initial object setup.
* Validate initial data.

Example:

```javascript
class User {
  constructor(name, email) {
    this.name = name;
    this.email = email;
  }
}

const user = new User(
  "Muhammad",
  "muhammad@example.com"
);

console.log(user.name);
console.log(user.email);
```

Output:

```text
Muhammad
muhammad@example.com
```

Here:

* `constructor()` is the constructor method.
* `name` and `email` are constructor parameters.
* `this.name` and `this.email` initialize instance properties.
* `new User()` creates a new instance and executes the constructor.

# 🗁 Constructor Syntax

A constructor is declared using the `constructor` keyword inside a class.

```javascript
class ClassName {
  constructor(parameters) {
    // Initialize instance properties
  }
}
```

Example:

```javascript
class Product {
  constructor(name, price) {
    this.name = name;
    this.price = price;
  }
}

const product = new Product("Laptop", 1200);

console.log(product);
```

Output:

```javascript
Product {
  name: "Laptop",
  price: 1200
}
```

# 🗁 The `new` Operator and Constructor Execution

When you create an instance using `new`, JavaScript performs several operations.

```javascript
class User {
  constructor(name) {
    this.name = name;

    console.log("Constructor executed");
  }
}

const user = new User("Muhammad");
```

Output:

```text
Constructor executed
```

Conceptually, the `new` operation performs these steps:

* Creates a new empty object.
* Sets the new object's prototype to the class's `prototype` object.
* Executes the constructor with `this` referring to the new object.
* Returns the new object, unless the constructor explicitly returns another object.

Example:

```javascript
class User {
  constructor(name) {
    this.name = name;
  }
}

const user = new User("Muhammad");

console.log(user instanceof User);
console.log(user.name);
```

Output:

```text
true
Muhammad
```

# 🗁 Constructor Parameters

Constructors can accept parameters to initialize instances with different values.

```javascript
class User {
  constructor(name, age, role) {
    this.name = name;
    this.age = age;
    this.role = role;
  }
}

const user1 = new User(
  "Muhammad",
  25,
  "Developer"
);

const user2 = new User(
  "Alex",
  30,
  "Designer"
);

console.log(user1);
console.log(user2);
```

Output:

```javascript
User {
  name: "Muhammad",
  age: 25,
  role: "Developer"
}

User {
  name: "Alex",
  age: 30,
  role: "Designer"
}
```

Each instance receives its own property values.

# 🗁 Default Constructor

A class does not need to define a constructor explicitly.

If a class has no constructor, JavaScript provides a default constructor.

```javascript
class User {}

const user = new User();

console.log(user instanceof User);
```

Output:

```text
true
```

For a base class, the default constructor behaves like an empty constructor:

```javascript
constructor() {}
```

For a derived class, the default constructor forwards its arguments to the parent constructor:

```javascript
constructor(...args) {
  super(...args);
}
```

# 🗁 Constructor with Default Parameters

Constructors support JavaScript default parameters.

```javascript
class User {
  constructor(
    name = "Guest",
    role = "Visitor"
  ) {
    this.name = name;
    this.role = role;
  }
}

const user1 = new User();
const user2 = new User("Muhammad");
const user3 = new User("Alex", "Developer");

console.log(user1);
console.log(user2);
console.log(user3);
```

Output:

```javascript
User {
  name: "Guest",
  role: "Visitor"
}

User {
  name: "Muhammad",
  role: "Visitor"
}

User {
  name: "Alex",
  role: "Developer"
}
```

Default values are used when the corresponding argument is omitted or explicitly passed as `undefined`.

They are not used when the argument is `null`.

# 🗁 Constructor with Destructured Parameters

You can use object destructuring to make constructors easier to work with when an object has many properties.

```javascript
class User {
  constructor({
    name,
    email,
    age,
    role = "User"
  }) {
    this.name = name;
    this.email = email;
    this.age = age;
    this.role = role;
  }
}

const user = new User({
  name: "Muhammad",
  email: "muhammad@example.com",
  age: 25
});

console.log(user);
```

Output:

```javascript
User {
  name: "Muhammad",
  email: "muhammad@example.com",
  age: 25,
  role: "User"
}
```

This approach makes object creation more readable and avoids relying on positional arguments.

You can also provide a default object parameter:

```javascript
class User {
  constructor({
    name = "Guest",
    role = "User"
  } = {}) {
    this.name = name;
    this.role = role;
  }
}

const user = new User();

console.log(user);
```

Output:

```javascript
User {
  name: "Guest",
  role: "User"
}
```

# 🗁 Constructor with Validation

A constructor can validate incoming values before initializing an instance.

```javascript
class User {
  constructor(name, age) {
    if (typeof name !== "string" || name.trim() === "") {
      throw new TypeError("Name must be a non-empty string");
    }

    if (!Number.isInteger(age) || age < 0) {
      throw new RangeError("Age must be a non-negative integer");
    }

    this.name = name;
    this.age = age;
  }
}

const user = new User("Muhammad", 25);

console.log(user);
```

Output:

```javascript
User {
  name: "Muhammad",
  age: 25
}
```

Invalid input prevents the instance from being successfully created.

```javascript
const user = new User("", -5);
```

This throws a `TypeError` because the name is invalid.

# 🗁 The `this` Keyword Inside a Constructor

Inside a class constructor, `this` refers to the newly created instance when invoked through `new`.

```javascript
class User {
  constructor(name) {
    this.name = name;
  }
}

const user1 = new User("Muhammad");
const user2 = new User("Alex");

console.log(user1.name);
console.log(user2.name);
```

Output:

```text
Muhammad
Alex
```

Each constructor invocation initializes a different object.

# 🗁 Only One Constructor Is Allowed

A JavaScript class can define only one constructor method.

This is invalid:

```javascript
class User {
  constructor(name) {
    this.name = name;
  }

  constructor(name, age) {
    this.name = name;
    this.age = age;
  }
}
```

JavaScript throws a `SyntaxError`.

If you need to support different initialization patterns, use default parameters, destructuring, or conditional logic inside one constructor.

Example:

```javascript
class User {
  constructor(name, age = 18) {
    this.name = name;
    this.age = age;
  }
}

const user1 = new User("Muhammad");
const user2 = new User("Alex", 30);

console.log(user1.age);
console.log(user2.age);
```

Output:

```text
18
30
```

# 🗁 Constructor Return Behavior

A constructor normally returns the newly created instance automatically.

However, JavaScript has special rules for explicit return statements.

**Returning an object**

If a base class constructor explicitly returns an object, that object becomes the result of the `new` expression.

```javascript
class User {
  constructor() {
    this.name = "Muhammad";

    return {
      name: "Alex"
    };
  }
}

const user = new User();

console.log(user.name);
console.log(user instanceof User);
```

Output:

```text
Alex
false
```

The explicitly returned object replaces the normally initialized instance.

**Returning a primitive value**

Primitive return values are ignored in constructors.

```javascript
class User {
  constructor() {
    this.name = "Muhammad";

    return 100;
  }
}

const user = new User();

console.log(user.name);
console.log(user instanceof User);
```

Output:

```text
Muhammad
true
```

The newly created instance is returned.

In derived class constructors, returning an object is also allowed. However, returning a non-`undefined` primitive value throws a `TypeError`.

# 🗁 Constructor and Instance Properties

Properties initialized in a constructor become own properties of each instance.

```javascript
class User {
  constructor(name) {
    this.name = name;
  }
}

const user1 = new User("Muhammad");
const user2 = new User("Alex");

console.log(Object.hasOwn(user1, "name"));
console.log(Object.hasOwn(user2, "name"));
```

Output:

```text
true
true
```

Each instance owns its own `name` property.

# 🗁 Constructor and Prototype Methods

Methods defined in the class body are stored on the prototype, not recreated inside the constructor for every instance.

```javascript
class User {
  constructor(name) {
    this.name = name;
  }

  introduce() {
    return `Hello, ${this.name}`;
  }
}

const user1 = new User("Muhammad");
const user2 = new User("Alex");

console.log(
  user1.introduce === user2.introduce
);
```

Output:

```text
true
```

Both instances share the same method implementation.

This is generally preferable to creating a new function inside every constructor invocation.

# 🗁 Constructor and Instance Field Initialization

JavaScript supports class fields as another way to initialize instance properties.

```javascript
class User {
  name = "Guest";
  role = "User";

  constructor(name) {
    if (name !== undefined) {
      this.name = name;
    }
  }
}

const user = new User("Muhammad");

console.log(user);
```

Output:

```javascript
User {
  name: "Muhammad",
  role: "User"
}
```

For a base class, instance fields are initialized before the constructor body executes.

The constructor can then override those initial values.

# 🗁 Constructor and Inheritance Preview

When a class extends another class, its constructor must call `super()` before accessing `this`.

```javascript
class Person {
  constructor(name) {
    this.name = name;
  }
}

class Developer extends Person {
  constructor(name, language) {
    super(name);

    this.language = language;
  }
}

const developer = new Developer(
  "Muhammad",
  "JavaScript"
);

console.log(developer.name);
console.log(developer.language);
```

Output:

```text
Muhammad
JavaScript
```

The `super()` call invokes the parent constructor and initializes the inherited state.

Constructor behavior in inheritance will be covered in the dedicated inheritance and `super` topics.

# 🗁 Practical Example: Product Model

A constructor can initialize a reusable product model with validation and default values.

```javascript
class Product {
  constructor({
    id,
    name,
    price,
    stock = 0
  }) {
    if (!id) {
      throw new Error("Product ID is required");
    }

    if (!name || name.trim() === "") {
      throw new Error("Product name is required");
    }

    if (typeof price !== "number" || price < 0) {
      throw new RangeError("Invalid product price");
    }

    if (!Number.isInteger(stock) || stock < 0) {
      throw new RangeError("Invalid stock quantity");
    }

    this.id = id;
    this.name = name;
    this.price = price;
    this.stock = stock;
  }

  getDetails() {
    return {
      id: this.id,
      name: this.name,
      price: this.price,
      stock: this.stock
    };
  }
}

const product = new Product({
  id: "P-101",
  name: "Mechanical Keyboard",
  price: 85,
  stock: 20
});

console.log(product.getDetails());
```

Output:

```javascript
{
  id: "P-101",
  name: "Mechanical Keyboard",
  price: 85,
  stock: 20
}
```

This example combines constructor parameters, destructuring, default values, validation, and instance initialization.

# 🗁 Common Mistakes

**Using multiple constructors**

A class supports only one constructor method.

**Forgetting the `new` operator**

Classes cannot be invoked like ordinary functions.

**Confusing parameters with instance properties**

A constructor parameter is a local variable. Assigning it to `this` creates an instance property.

**Returning an object unintentionally**

Returning an object from a base class constructor replaces the newly initialized instance.

**Accessing `this` before `super()`**

A derived class constructor must call `super()` before accessing `this`.

**Duplicating methods inside constructors**

Creating methods as instance properties inside constructors can allocate a new function for every instance unnecessarily.

**Skipping input validation**

Invalid initialization data can create objects in inconsistent states.

# 🗁 Best Practices

* Keep constructors focused on initializing instance state.
* Use meaningful parameter names.
* Use default parameters for simple fallback values.
* Prefer object destructuring when constructors accept many configuration values.
* Validate important input before assigning properties.
* Avoid complex business logic inside constructors.
* Keep reusable instance methods on the class prototype.
* Use class fields when they make initialization clearer.
* Use `super()` correctly in derived classes.
* Avoid returning replacement objects from constructors unless there is a specific design requirement.

# 🗁 Key Takeaways

* A constructor initializes newly created class instances.
* It executes automatically when an instance is created using `new`.
* A class can define only one constructor.
* Classes without an explicit constructor receive a default constructor.
* Constructors can accept parameters, default values, and destructured objects.
* The `this` keyword refers to the new instance during normal construction.
* Constructors can validate data before initializing an instance.
* Explicitly returning an object from a base class constructor replaces the new instance.
* Primitive return values are ignored in base class constructors.
* Instance properties are created on individual objects.
* Normal class methods are shared through the prototype.
* Derived class constructors must call `super()` before accessing `this`.
