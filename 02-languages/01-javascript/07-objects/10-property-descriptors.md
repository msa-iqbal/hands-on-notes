# Property Descriptors

Property descriptors are objects that describe the characteristics and behavior of an object's individual properties.

JavaScript properties have attributes that determine:

* Whether a property can be modified.
* Whether a property can be deleted or reconfigured.
* Whether a property appears during enumeration.
* Whether a property stores a value or uses getter and setter functions.

For example:

```javascript
const user = {
  name: "Muhammad"
};

console.log(
  Object.getOwnPropertyDescriptor(user, "name")
);
```

Output:

```javascript
{
  value: "Muhammad",
  writable: true,
  enumerable: true,
  configurable: true
}
```

These attributes are called property descriptors.

# 🗁 Types of Property Descriptors

JavaScript supports two main types of property descriptors:

* Data descriptors.
* Accessor descriptors.

**Data descriptors**

Data descriptors define properties that store values.

They use these attributes:

| Attribute | Description |
| --- | --- |
| `value` | The property's stored value |
| `writable` | Determines whether the value can be changed |
| `enumerable` | Determines whether the property appears in enumeration |
| `configurable` | Determines whether the property can be deleted or reconfigured |

**Accessor descriptors**

Accessor descriptors define properties using getter and setter functions.

They use these attributes:

| Attribute | Description |
| --- | --- |
| `get` | Function executed when the property is accessed |
| `set` | Function executed when a value is assigned |
| `enumerable` | Determines whether the property appears in enumeration |
| `configurable` | Determines whether the property can be deleted or reconfigured |

A single property cannot simultaneously be a data descriptor and an accessor descriptor.

# 🗁 Understanding Data Descriptors

Consider a normal object:

```javascript
const user = {
  name: "Muhammad",
  age: 25
};
```

JavaScript creates ordinary data properties for `name` and `age`.

You can inspect their descriptors using `Object.getOwnPropertyDescriptor()`.

```javascript
console.log(
  Object.getOwnPropertyDescriptor(user, "name")
);
```

Output:

```javascript
{
  value: "Muhammad",
  writable: true,
  enumerable: true,
  configurable: true
}
```

The property can be read, updated, enumerated, and deleted.

# 🗁 The `value` Attribute

The `value` attribute represents the value stored in a data property.

```javascript
const user = {};

Object.defineProperty(user, "name", {
  value: "Muhammad"
});

console.log(user.name);
```

Output:

```text
Muhammad
```

The property stores `"Muhammad"`.

# 🗁 The `writable` Attribute

The `writable` attribute determines whether a data property's value can be changed through ordinary assignment.

When `writable` is `true`, the property can be reassigned.

```javascript
const user = {};

Object.defineProperty(user, "name", {
  value: "Muhammad",
  writable: true
});

user.name = "Alex";

console.log(user.name);
```

Output:

```text
Alex
```

When `writable` is `false`, the property cannot be changed through ordinary assignment.

```javascript
const user = {};

Object.defineProperty(user, "name", {
  value: "Muhammad",
  writable: false
});

user.name = "Alex";

console.log(user.name);
```

Output:

```text
Muhammad
```

In non-strict mode, the assignment is silently ignored.

In strict mode, assigning a different value to a non-writable property throws a `TypeError`.

```javascript
"use strict";

const user = {};

Object.defineProperty(user, "name", {
  value: "Muhammad",
  writable: false
});

user.name = "Alex";
```

This throws a `TypeError`.

# 🗁 The `enumerable` Attribute

The `enumerable` attribute determines whether a property appears in common property enumeration operations.

When `enumerable` is `true`, the property appears in operations such as:

* `Object.keys()`
* `Object.values()`
* `Object.entries()`
* `for...in` when the property is own and enumerable

Example:

```javascript
const user = {};

Object.defineProperty(user, "name", {
  value: "Muhammad",
  enumerable: true
});

console.log(Object.keys(user));
```

Output:

```javascript
["name"]
```

When `enumerable` is `false`, the property is excluded from these enumeration operations.

```javascript
const user = {};

Object.defineProperty(user, "name", {
  value: "Muhammad",
  enumerable: false
});

console.log(Object.keys(user));
console.log(user.name);
```

Output:

```javascript
[]
Muhammad
```

The property still exists and can be accessed directly.

It is simply non-enumerable.

# 🗁 The `configurable` Attribute

The `configurable` attribute determines whether a property can be deleted or reconfigured.

When `configurable` is `true`, the property can normally be deleted or have its descriptor changed.

```javascript
const user = {};

Object.defineProperty(user, "name", {
  value: "Muhammad",
  configurable: true
});

delete user.name;

console.log(user.name);
```

Output:

```text
undefined
```

When `configurable` is `false`, the property cannot be deleted through ordinary `delete` operations or reconfigured to incompatible descriptor settings.

```javascript
const user = {};

Object.defineProperty(user, "name", {
  value: "Muhammad",
  configurable: false
});

delete user.name;

console.log(user.name);
```

Output:

```text
Muhammad
```

In non-strict mode, the deletion is silently unsuccessful.

In strict mode, attempting to delete a non-configurable property throws a `TypeError`.

A non-configurable data property can still have its value changed if `writable` is `true`. However, its `writable` attribute cannot be changed back from `false` to `true`.

# 🗁 The `Object.defineProperty()` Method

`Object.defineProperty()` creates or modifies a property using a property descriptor.

Syntax:

```javascript
Object.defineProperty(
  object,
  propertyKey,
  descriptor
);
```

Parameters:

| Parameter | Description |
| --- | --- |
| `object` | The object on which the property should be defined |
| `propertyKey` | The name or symbol of the property |
| `descriptor` | The descriptor containing the property's attributes |

Example:

```javascript
const user = {};

Object.defineProperty(user, "name", {
  value: "Muhammad",
  writable: true,
  enumerable: true,
  configurable: true
});

console.log(user.name);
```

Output:

```text
Muhammad
```

The method returns the original object.

# 🗁 Default Descriptor Values

An important detail is that attributes omitted from a descriptor passed to `Object.defineProperty()` default to `false`.

Example:

```javascript
const user = {};

Object.defineProperty(user, "name", {
  value: "Muhammad"
});

console.log(
  Object.getOwnPropertyDescriptor(user, "name")
);
```

Output:

```javascript
{
  value: "Muhammad",
  writable: false,
  enumerable: false,
  configurable: false
}
```

This differs from properties created using ordinary object literals.

```javascript
const user = {
  name: "Muhammad"
};

console.log(
  Object.getOwnPropertyDescriptor(user, "name")
);
```

Output:

```javascript
{
  value: "Muhammad",
  writable: true,
  enumerable: true,
  configurable: true
}
```

Remember this distinction when creating properties programmatically.

# 🗁 Modifying Existing Property Descriptors

`Object.defineProperty()` can also modify an existing property's descriptor.

```javascript
const user = {
  name: "Muhammad"
};

Object.defineProperty(user, "name", {
  writable: false
});

console.log(
  Object.getOwnPropertyDescriptor(user, "name")
);
```

Output:

```javascript
{
  value: "Muhammad",
  writable: false,
  enumerable: true,
  configurable: true
}
```

Attributes that are not specified remain unchanged when the existing property is reconfigured.

# 🗁 Defining Multiple Properties with `Object.defineProperties()`

`Object.defineProperties()` allows you to define multiple properties in a single operation.

Syntax:

```javascript
Object.defineProperties(object, descriptors);
```

Example:

```javascript
const user = {};

Object.defineProperties(user, {
  name: {
    value: "Muhammad",
    writable: true,
    enumerable: true,
    configurable: true
  },

  age: {
    value: 25,
    writable: true,
    enumerable: true,
    configurable: true
  },

  role: {
    value: "Developer",
    writable: false,
    enumerable: true,
    configurable: false
  }
});

console.log(user);
```

Output:

```javascript
{
  name: "Muhammad",
  age: 25,
  role: "Developer"
}
```

This method is useful when an object needs several properties with custom behavior.

# 🗁 Accessor Property Descriptors

Accessor properties use getter and setter functions instead of storing a value directly.

A getter executes when a property is read.

A setter executes when a property is assigned.

Example:

```javascript
const user = {
  firstName: "Muhammad",
  lastName: "Iqbal",

  get fullName() {
    return `${this.firstName} ${this.lastName}`;
  }
};

console.log(user.fullName);
```

Output:

```text
Muhammad Iqbal
```

The `fullName` property uses a getter.

It behaves like a property rather than a method call.

# 🗁 Defining a Getter with `Object.defineProperty()`

You can define an accessor property using `Object.defineProperty()`.

```javascript
const user = {
  firstName: "Muhammad",
  lastName: "Iqbal"
};

Object.defineProperty(user, "fullName", {
  get() {
    return `${this.firstName} ${this.lastName}`;
  },

  enumerable: true,
  configurable: true
});

console.log(user.fullName);
```

Output:

```text
Muhammad Iqbal
```

The getter calculates the property's value whenever it is accessed.

# 🗁 Defining a Setter with `Object.defineProperty()`

A setter allows you to control how values are assigned to an accessor property.

```javascript
const user = {
  firstName: "Muhammad",
  lastName: "Iqbal"
};

Object.defineProperty(user, "fullName", {
  get() {
    return `${this.firstName} ${this.lastName}`;
  },

  set(value) {
    const [firstName, ...lastName] = value.split(" ");

    this.firstName = firstName;
    this.lastName = lastName.join(" ");
  },

  enumerable: true,
  configurable: true
});

user.fullName = "Alex Morgan";

console.log(user.firstName);
console.log(user.lastName);
console.log(user.fullName);
```

Output:

```text
Alex
Morgan
Alex Morgan
```

The setter receives the assigned value and updates the underlying properties.

# 🗁 Inspecting Property Descriptors

`Object.getOwnPropertyDescriptor()` retrieves the descriptor of a specific own property.

Syntax:

```javascript
Object.getOwnPropertyDescriptor(object, propertyKey);
```

Example:

```javascript
const user = {
  name: "Muhammad"
};

const descriptor = Object.getOwnPropertyDescriptor(
  user,
  "name"
);

console.log(descriptor);
```

Output:

```javascript
{
  value: "Muhammad",
  writable: true,
  enumerable: true,
  configurable: true
}
```

If the property does not exist directly on the object, the method returns `undefined`.

It does not search the prototype chain.

# 🗁 Getting All Property Descriptors

`Object.getOwnPropertyDescriptors()` returns the descriptors of all own properties of an object.

```javascript
const user = {
  name: "Muhammad",
  age: 25
};

const descriptors = Object.getOwnPropertyDescriptors(user);

console.log(descriptors);
```

Output:

```javascript
{
  name: {
    value: "Muhammad",
    writable: true,
    enumerable: true,
    configurable: true
  },

  age: {
    value: 25,
    writable: true,
    enumerable: true,
    configurable: true
  }
}
```

This method is particularly useful when copying property descriptors rather than copying only property values.

# 🗁 Copying Property Descriptors

Object spread and `Object.assign()` copy property values, not the original property descriptors.

To preserve descriptors, use `Object.getOwnPropertyDescriptors()` together with `Object.defineProperties()`.

```javascript
const original = {};

Object.defineProperty(original, "name", {
  value: "Muhammad",
  writable: false,
  enumerable: true,
  configurable: false
});

const copy = Object.defineProperties(
  {},
  Object.getOwnPropertyDescriptors(original)
);

console.log(
  Object.getOwnPropertyDescriptor(copy, "name")
);
```

Output:

```javascript
{
  value: "Muhammad",
  writable: false,
  enumerable: true,
  configurable: false
}
```

The descriptor attributes are preserved.

This approach creates a new ordinary object, so it does not automatically preserve the original object's prototype.

# 🗁 Property Descriptors and Object Spread

Object spread creates new data properties using the values retrieved from the source object.

It does not preserve the original property's descriptor attributes.

```javascript
const original = {};

Object.defineProperty(original, "name", {
  value: "Muhammad",
  writable: false,
  enumerable: true,
  configurable: false
});

const copy = {
  ...original
};

console.log(
  Object.getOwnPropertyDescriptor(copy, "name")
);
```

Output:

```javascript
{
  value: "Muhammad",
  writable: true,
  enumerable: true,
  configurable: true
}
```

The copied property becomes a normal writable, enumerable, and configurable data property.

# 🗁 Property Descriptors and `Object.freeze()`

`Object.freeze()` prevents changes to an object's own properties.

For ordinary data properties, freezing makes them non-writable and non-configurable.

```javascript
const user = {
  name: "Muhammad",
  age: 25
};

Object.freeze(user);

user.name = "Alex";
delete user.age;

console.log(user);
```

Output:

```javascript
{
  name: "Muhammad",
  age: 25
}
```

In non-strict mode, these unsuccessful operations are silently ignored.

In strict mode, they throw a `TypeError`.

You can inspect the result:

```javascript
console.log(
  Object.getOwnPropertyDescriptor(user, "name")
);
```

Output:

```javascript
{
  value: "Muhammad",
  writable: false,
  enumerable: true,
  configurable: false
}
```

`Object.freeze()` is shallow. Nested objects can still be modified unless they are frozen separately.

# 🗁 Property Descriptors and `Object.seal()`

`Object.seal()` prevents properties from being added or removed and makes existing own properties non-configurable.

However, writable data properties can still have their values changed.

```javascript
const user = {
  name: "Muhammad",
  age: 25
};

Object.seal(user);

user.name = "Alex";
delete user.age;

user.role = "Developer";

console.log(user);
```

Output:

```javascript
{
  name: "Alex",
  age: 25
}
```

The existing `name` property remains writable, but properties cannot be added or deleted.

# 🗁 Property Descriptors and `Object.preventExtensions()`

`Object.preventExtensions()` prevents new properties from being added to an object.

Existing properties can still be modified or deleted according to their descriptors.

```javascript
const user = {
  name: "Muhammad"
};

Object.preventExtensions(user);

user.name = "Alex";
user.age = 25;

console.log(user);
```

Output:

```javascript
{
  name: "Alex"
}
```

The existing property can be updated, but the new `age` property is not added.

# 🗁 Comparing Object Property Controls

JavaScript provides three important methods for restricting object modifications.

| Method | Add Properties | Delete Properties | Modify Writable Values |
| --- | --- | --- | --- |
| `Object.preventExtensions()` | No | Yes, if configurable | Yes |
| `Object.seal()` | No | No | Yes |
| `Object.freeze()` | No | No | No |

These methods affect an object's own properties and extensibility. They do not automatically freeze or seal nested objects.

# 🗁 Practical Example: Creating a Read-Only Configuration

Property descriptors can be used to define configuration values that should not be reassigned.

```javascript
const config = {};

Object.defineProperties(config, {
  apiUrl: {
    value: "https://api.example.com",
    writable: false,
    enumerable: true,
    configurable: false
  },

  environment: {
    value: "production",
    writable: false,
    enumerable: true,
    configurable: false
  }
});

console.log(config.apiUrl);
console.log(config.environment);
```

Output:

```text
https://api.example.com
production
```

Both properties are readable but cannot be reassigned through ordinary assignment.

# 🗁 Practical Example: Validating Property Assignment

A setter can validate values before updating an object.

```javascript
const product = {
  _price: 100
};

Object.defineProperty(product, "price", {
  get() {
    return this._price;
  },

  set(value) {
    if (typeof value !== "number" || value < 0) {
      throw new TypeError(
        "Price must be a non-negative number"
      );
    }

    this._price = value;
  },

  enumerable: true,
  configurable: true
});

product.price = 250;

console.log(product.price);
```

Output:

```text
250
```

Invalid values are rejected:

```javascript
product.price = -50;
```

This throws a `TypeError`.

# 🗁 Common Mistakes

**Assuming descriptor attributes default to `true`**

Properties created with `Object.defineProperty()` default to non-writable, non-enumerable, and non-configurable when those attributes are omitted.

**Confusing writable with configurable**

* `writable` controls whether a data property's value can change.
* `configurable` controls whether the property can be deleted or reconfigured.

**Assuming non-enumerable means inaccessible**

A non-enumerable property can still be accessed directly.

**Assuming getters and setters store values**

Accessor properties execute functions when accessed or assigned. They do not use the `value` and `writable` attributes.

**Combining data and accessor descriptors**

A property descriptor cannot contain both data-specific attributes and accessor-specific attributes.

**Assuming Object.freeze() deeply freezes objects**

Freezing an object does not automatically freeze nested objects.

**Assuming Object.assign() preserves descriptors**

`Object.assign()` copies values into a target using property assignment. It does not preserve the original descriptor attributes.

# 🗁 Best Practices

* Use `Object.defineProperty()` when a property needs custom behavior.
* Use `Object.getOwnPropertyDescriptor()` to inspect individual properties.
* Use `Object.getOwnPropertyDescriptors()` when you need all own property descriptors.
* Understand the difference between data and accessor descriptors.
* Use getters for computed properties when property-like access improves readability.
* Use setters when assignment requires validation or controlled updates.
* Use `Object.freeze()` when an object's own properties should not be modified.
* Remember that freezing and sealing are shallow operations.
* Avoid complex getters and setters with unexpected side effects.
* Prefer ordinary object properties unless custom descriptor behavior is necessary.

# 🗁 Key Takeaways

* Property descriptors define the behavior and characteristics of object properties.
* JavaScript supports data descriptors and accessor descriptors.
* Data descriptors use `value` and `writable`.
* Accessor descriptors use `get` and `set`.
* Both descriptor types support `enumerable` and `configurable`.
* `Object.defineProperty()` creates or modifies a property using a descriptor.
* `Object.defineProperties()` defines multiple properties.
* `Object.getOwnPropertyDescriptor()` retrieves one property's descriptor.
* `Object.getOwnPropertyDescriptors()` retrieves all own property descriptors.
* `Object.freeze()`, `Object.seal()`, and `Object.preventExtensions()` restrict different kinds of object modifications.
* Understanding descriptors provides precise control over JavaScript object behavior.
