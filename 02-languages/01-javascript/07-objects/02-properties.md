# Object Properties

Object properties are named values stored inside JavaScript objects.

Each property consists of:

* **Key:** The property's name.
* **Value:** The data associated with that key.

Example:

```javascript
const user = {
  name: "Muhammad",
  age: 25,
  isActive: true
};
```

In this example:

| Property Key | Property Value |
| --- | --- |
| `name` | `"Muhammad"` |
| `age` | `25` |
| `isActive` | `true` |

JavaScript object properties can contain any type of value, including primitive values, objects, arrays, and functions.

# 🗁 Property Keys

JavaScript object property keys are strings or symbols.

When other primitive values are used as property keys, JavaScript generally converts them to strings.

```javascript
const user = {
  name: "Muhammad",
  100: "User ID",
  true: "Active"
};

console.log(user.name);
console.log(user["100"]);
console.log(user["true"]);
```

Output:

```text
Muhammad
User ID
Active
```

The numeric key `100` becomes the string `"100"`, and the boolean key `true` becomes the string `"true"`.

Symbol keys are an exception because they remain symbols.

```javascript
const id = Symbol("id");

const user = {
  name: "Muhammad",
  [id]: 101
};

console.log(user[id]);
```

Output:

```text
101
```

# 🗁 Accessing Properties

JavaScript provides two primary ways to access object properties.

**Dot notation**

```javascript
const user = {
  name: "Muhammad",
  age: 25
};

console.log(user.name);
console.log(user.age);
```

Dot notation is suitable when the property name is a known, valid identifier.

**Bracket notation**

```javascript
const user = {
  name: "Muhammad",
  "first-name": "Muhammad",
  age: 25
};

console.log(user["name"]);
console.log(user["first-name"]);
```

Bracket notation is useful when:

* Property names contain spaces or special characters.
* Property names are stored in variables.
* Property names are determined dynamically.
* The property key is a symbol.

# 🗁 Dynamic Property Access

Bracket notation allows property names to be determined at runtime.

```javascript
const user = {
  name: "Muhammad",
  age: 25,
  profession: "Software Engineer"
};

const property = "profession";

console.log(user[property]);
```

Output:

```text
Software Engineer
```

This is particularly useful when processing dynamic data or building reusable functions.

Example:

```javascript
function getProperty(object, key) {
  return object[key];
}

const user = {
  name: "Muhammad",
  age: 25
};

console.log(getProperty(user, "name"));
console.log(getProperty(user, "age"));
```

Output:

```text
Muhammad
25
```

# 🗁 Adding Properties

Properties can be added when creating an object or afterward.

**During object creation**

```javascript
const user = {
  name: "Muhammad",
  age: 25
};
```

**After object creation**

```javascript
const user = {};

user.name = "Muhammad";
user.age = 25;

console.log(user);
```

Output:

```javascript
{
  name: "Muhammad",
  age: 25
}
```

**Using bracket notation**

```javascript
const user = {};

const property = "email";

user[property] = "muhammad@example.com";

console.log(user);
```

Output:

```javascript
{
  email: "muhammad@example.com"
}
```

# 🗁 Modifying Properties

Existing property values can be changed through assignment.

```javascript
const user = {
  name: "Muhammad",
  age: 25
};

user.age = 26;

console.log(user.age);
```

Output:

```text
26
```

The property remains part of the object, but its value changes.

# 🗁 Deleting Properties

The `delete` operator removes an object's property.

```javascript
const user = {
  name: "Muhammad",
  age: 25,
  email: "muhammad@example.com"
};

delete user.email;

console.log(user);
```

Output:

```javascript
{
  name: "Muhammad",
  age: 25
}
```

Deleting a property is different from assigning `undefined`.

```javascript
const user = {
  name: "Muhammad",
  age: 25
};

user.age = undefined;

console.log(user.age);
console.log(Object.hasOwn(user, "age"));
```

Output:

```text
undefined
true
```

The property still exists.

By contrast:

```javascript
delete user.age;

console.log(Object.hasOwn(user, "age"));
```

Output:

```text
false
```

# 🗁 Checking Property Existence

JavaScript provides several ways to check whether a property exists.

**The in operator**

The `in` operator checks both own properties and inherited properties.

```javascript
const user = {
  name: "Muhammad",
  age: 25
};

console.log("name" in user);
console.log("email" in user);
```

Output:

```text
true
false
```

**Object.hasOwn()**

Checks whether a property belongs directly to the object.

```javascript
const user = {
  name: "Muhammad"
};

console.log(Object.hasOwn(user, "name"));
console.log(Object.hasOwn(user, "toString"));
```

Output:

```text
true
false
```

**hasOwnProperty()**

An older instance method that also checks own properties.

```javascript
const user = {
  name: "Muhammad"
};

console.log(user.hasOwnProperty("name"));
```

Output:

```text
true
```

`Object.hasOwn()` is generally preferable because it also works with objects that do not inherit from `Object.prototype` or that override `hasOwnProperty`.

# 🗁 Property Descriptors

Every ordinary object property has an associated property descriptor.

A property descriptor defines the characteristics of a property.

For data properties, the descriptor includes:

| Attribute | Description |
| --- | --- |
| `value` | The property's stored value |
| `writable` | Whether the value can be changed through assignment |
| `enumerable` | Whether the property appears in certain enumeration operations |
| `configurable` | Whether the property can be deleted or its descriptor changed |

Example:

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

JavaScript also supports accessor properties, which use `get` and `set` functions instead of a stored data value.

Property descriptors are covered in detail in the property descriptors topic.

# 🗁 Enumerable and Non-Enumerable Properties

An enumerable property is included in certain property enumeration operations.

By default, properties created through object literals are enumerable.

```javascript
const user = {
  name: "Muhammad",
  age: 25
};

console.log(Object.keys(user));
```

Output:

```javascript
["name", "age"]
```

A property can be made non-enumerable using `Object.defineProperty()`.

```javascript
const user = {
  name: "Muhammad"
};

Object.defineProperty(user, "secret", {
  value: "Hidden",
  enumerable: false
});

console.log(Object.keys(user));
console.log(user.secret);
```

Output:

```text
["name"]
Hidden
```

The property still exists and can be accessed directly, but it is excluded from `Object.keys()`.

# 🗁 Own Properties and Inherited Properties

An object's properties can be classified as own or inherited.

**Own properties**

Properties defined directly on the object.

**Inherited properties**

Properties available through the object's prototype chain.

```javascript
const parent = {
  role: "Developer"
};

const user = Object.create(parent);

user.name = "Muhammad";

console.log(user.name);
console.log(user.role);

console.log(Object.hasOwn(user, "name"));
console.log(Object.hasOwn(user, "role"));
```

Output:

```text
Muhammad
Developer
true
false
```

The `name` property belongs directly to `user`, while `role` is inherited from `parent`.

# 🗁 Property Shorthand

When a variable name matches an object property name, JavaScript allows shorthand property syntax.

```javascript
const name = "Muhammad";
const age = 25;

const user = {
  name,
  age
};

console.log(user);
```

Output:

```javascript
{
  name: "Muhammad",
  age: 25
}
```

This is equivalent to:

```javascript
const user = {
  name: name,
  age: age
};
```

# 🗁 Computed Property Names

Computed property names allow expressions to determine property keys dynamically during object creation.

Use square brackets inside an object literal.

```javascript
const property = "name";

const user = {
  [property]: "Muhammad"
};

console.log(user.name);
```

Output:

```text
Muhammad
```

Expressions can also be used to construct property names.

```javascript
const prefix = "user";

const account = {
  [`${prefix}Id`]: 101,
  [`${prefix}Name`]: "Muhammad"
};

console.log(account);
```

Output:

```javascript
{
  userId: 101,
  userName: "Muhammad"
}
```

# 🗁 Optional Chaining with Properties

Optional chaining (`?.`) safely accesses nested properties when an intermediate value may be `null` or `undefined`.

```javascript
const user = {
  name: "Muhammad",
  address: {
    city: "Dhaka"
  }
};

console.log(user.address?.city);
console.log(user.contact?.email);
```

Output:

```text
Dhaka
undefined
```

Without optional chaining, accessing a property through an undefined intermediate value would throw a `TypeError`.

Optional chaining does not make every property access safe. It only protects the part of the access chain where it is used.

# 🗁 Property Access with Nullish Coalescing

The nullish coalescing operator (`??`) provides a fallback when a property value is `null` or `undefined`.

```javascript
const user = {
  name: "Muhammad",
  age: 0
};

const name = user.name ?? "Unknown";
const age = user.age ?? 18;

console.log(name);
console.log(age);
```

Output:

```text
Muhammad
0
```

Unlike the logical OR operator (`||`), nullish coalescing does not replace valid falsy values such as `0`, `false`, or an empty string.

# 🗁 Property Enumeration

JavaScript provides several methods for retrieving object properties.

| Method | Own Properties | Enumerable Only | Symbol Keys |
| --- | --- | --- | --- |
| `Object.keys()` | Yes | Yes | No |
| `Object.values()` | Yes | Yes | No |
| `Object.entries()` | Yes | Yes | No |
| `Object.getOwnPropertyNames()` | Yes | No | No |
| `Object.getOwnPropertySymbols()` | Yes | No | Yes |
| `Reflect.ownKeys()` | Yes | No | Yes |
| `for...in` | Own and inherited | Yes | No |

Example:

```javascript
const user = {
  name: "Muhammad",
  age: 25
};

for (const key in user) {
  console.log(key, user[key]);
}
```

Output:

```text
name Muhammad
age 25
```

Use `Object.keys()`, `Object.entries()`, or `Reflect.ownKeys()` depending on whether you need enumerable properties, key-value pairs, or all own keys.

# 🗁 Practical Example: Dynamic Object Updates

```javascript
const user = {
  id: 101,
  name: "Muhammad",
  email: "muhammad@example.com",
  isActive: true
};

function updateProperty(object, key, value) {
  if (Object.hasOwn(object, key)) {
    object[key] = value;

    return true;
  }

  return false;
}

updateProperty(user, "name", "Iqbal");
updateProperty(user, "isActive", false);

console.log(user);
```

Output:

```javascript
{
  id: 101,
  name: "Iqbal",
  email: "muhammad@example.com",
  isActive: false
}
```

This example demonstrates dynamic property access and controlled updates.

# 🗁 Best Practices

* Use dot notation for known property names.
* Use bracket notation for dynamic property access.
* Use `Object.hasOwn()` to check direct property ownership.
* Prefer `Object.defineProperty()` when property characteristics need explicit control.
* Avoid using `delete` as a routine state-management strategy.
* Use optional chaining for potentially missing nested values.
* Use nullish coalescing when only `null` and `undefined` should trigger a fallback.
* Understand the difference between own and inherited properties.
* Avoid confusing a missing property with a property whose value is `undefined`.
* Choose property enumeration methods based on the properties you need.

# 🗁 Key Takeaways

* JavaScript object keys are strings or symbols.
* Properties can be accessed using dot notation and bracket notation.
* Properties can be added, modified, and deleted.
* The `in` operator checks own and inherited properties.
* `Object.hasOwn()` checks only own properties.
* Property descriptors control property characteristics.
* Properties can be enumerable or non-enumerable.
* Computed property names support dynamic keys.
* Optional chaining safely accesses potentially missing nested properties.
* JavaScript provides multiple methods for inspecting and enumerating properties.
