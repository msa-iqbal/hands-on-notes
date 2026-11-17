# Object Spread

Object spread is a JavaScript feature that allows you to copy the enumerable own properties of an object into another object.

It uses the spread syntax (`...`) inside object literals.

Object spread is commonly used for:

* Copying objects.
* Merging multiple objects.
* Updating object properties.
* Creating objects with additional properties.
* Removing properties through destructuring and rest syntax.
* Managing application state immutably.

Example:

```javascript
const user = {
  name: "Muhammad",
  age: 25
};

const userCopy = {
  ...user
};

console.log(userCopy);
```

Output:

```javascript
{
  name: "Muhammad",
  age: 25
}
```

The spread syntax copies the enumerable own properties from `user` into a new object.

# 🗁 Basic Object Spread Syntax

The syntax uses three dots followed by an object expression.

```javascript
const newObject = {
  ...existingObject
};
```

Example:

```javascript
const product = {
  name: "Laptop",
  price: 1000,
  category: "Electronics"
};

const copy = {
  ...product
};

console.log(copy);
```

Output:

```javascript
{
  name: "Laptop",
  price: 1000,
  category: "Electronics"
}
```

The original object remains unchanged.

# 🗁 Copying Objects

Object spread creates a new object containing the source object's enumerable own properties.

```javascript
const user = {
  name: "Muhammad",
  age: 25
};

const userCopy = {
  ...user
};

userCopy.name = "Alex";

console.log(user.name);
console.log(userCopy.name);
```

Output:

```text
Muhammad
Alex
```

The top-level properties are copied into a different object.

Therefore, changing a copied primitive property does not modify the original property.

# 🗁 Shallow Copy

Object spread creates a shallow copy, not a deep copy.

A shallow copy duplicates the top-level property values, but nested objects remain shared references.

```javascript
const user = {
  name: "Muhammad",
  address: {
    city: "Dhaka",
    country: "Bangladesh"
  }
};

const userCopy = {
  ...user
};

userCopy.name = "Alex";
userCopy.address.city = "Chittagong";

console.log(user.name);
console.log(user.address.city);
```

Output:

```text
Muhammad
Chittagong
```

The top-level `name` property is independent.

However, `address` still references the same nested object in both objects.

# 🗁 Creating a Shallow Copy with Nested Updates

You can use nested object spread to copy specific levels of an object.

```javascript
const user = {
  name: "Muhammad",
  address: {
    city: "Dhaka",
    country: "Bangladesh"
  }
};

const updatedUser = {
  ...user,
  address: {
    ...user.address,
    city: "Chittagong"
  }
};

console.log(updatedUser);
console.log(user);
```

Output:

```javascript
{
  name: "Muhammad",
  address: {
    city: "Chittagong",
    country: "Bangladesh"
  }
}

{
  name: "Muhammad",
  address: {
    city: "Dhaka",
    country: "Bangladesh"
  }
}
```

Each copied level gets a new object reference.

This technique is useful for immutable updates to nested application state.

# 🗁 Merging Objects

Object spread can combine properties from multiple objects into a new object.

```javascript
const user = {
  name: "Muhammad",
  age: 25
};

const additionalInfo = {
  profession: "Software Engineer",
  country: "Bangladesh"
};

const profile = {
  ...user,
  ...additionalInfo
};

console.log(profile);
```

Output:

```javascript
{
  name: "Muhammad",
  age: 25,
  profession: "Software Engineer",
  country: "Bangladesh"
}
```

Properties from both objects are included in the resulting object.

# 🗁 Handling Duplicate Properties

When multiple spread sources contain the same property name, the property from the rightmost source takes precedence.

```javascript
const defaults = {
  theme: "light",
  language: "en",
  fontSize: 14
};

const preferences = {
  theme: "dark",
  fontSize: 16
};

const settings = {
  ...defaults,
  ...preferences
};

console.log(settings);
```

Output:

```javascript
{
  theme: "dark",
  language: "en",
  fontSize: 16
}
```

The `preferences` object overrides matching properties from `defaults`.

# 🗁 Overriding Properties with Explicit Values

You can combine spread syntax with explicitly defined properties.

```javascript
const user = {
  name: "Muhammad",
  age: 25,
  country: "Bangladesh"
};

const updatedUser = {
  ...user,
  age: 26
};

console.log(updatedUser);
```

Output:

```javascript
{
  name: "Muhammad",
  age: 26,
  country: "Bangladesh"
}
```

The explicit `age` property appears after the spread and overrides the original value.

The reverse order produces a different result:

```javascript
const updatedUser = {
  age: 26,
  ...user
};

console.log(updatedUser.age);
```

Output:

```text
25
```

The spread source appears later, so its `age` value takes precedence.

# 🗁 Combining Multiple Sources

You can spread multiple objects into a single object literal.

```javascript
const personalInfo = {
  name: "Muhammad",
  age: 25
};

const professionalInfo = {
  profession: "Software Engineer",
  experience: 5
};

const locationInfo = {
  city: "Dhaka",
  country: "Bangladesh"
};

const profile = {
  ...personalInfo,
  ...professionalInfo,
  ...locationInfo
};

console.log(profile);
```

Output:

```javascript
{
  name: "Muhammad",
  age: 25,
  profession: "Software Engineer",
  experience: 5,
  city: "Dhaka",
  country: "Bangladesh"
}
```

This is useful when different parts of an application manage separate pieces of data.

# 🗁 Object Spread with Empty Objects

Spreading an empty object does not add any properties.

```javascript
const user = {
  name: "Muhammad"
};

const result = {
  ...user,
  ...{}
};

console.log(result);
```

Output:

```javascript
{
  name: "Muhammad"
}
```

This can be useful when conditionally including optional object data.

# 🗁 Conditional Object Properties

Object spread can conditionally include properties.

```javascript
const isAdmin = true;

const user = {
  name: "Muhammad",
  ...(isAdmin && {
    role: "Administrator"
  })
};

console.log(user);
```

Output:

```javascript
{
  name: "Muhammad",
  role: "Administrator"
}
```

When `isAdmin` is `false`, the expression evaluates to `false`. Spreading a boolean primitive contributes no enumerable own properties.

```javascript
const isAdmin = false;

const user = {
  name: "Muhammad",
  ...(isAdmin && {
    role: "Administrator"
  })
};

console.log(user);
```

Output:

```javascript
{
  name: "Muhammad"
}
```

For more complex conditions, a conditional expression can make the intention clearer.

```javascript
const isAdmin = true;

const user = {
  name: "Muhammad",
  ...(isAdmin ? { role: "Administrator" } : {})
};
```

# 🗁 Object Spread with Arrays

Arrays can also be spread into object literals.

Their enumerable own properties are copied using their indexes as string keys.

```javascript
const colors = ["red", "green", "blue"];

const colorObject = {
  ...colors
};

console.log(colorObject);
```

Output:

```javascript
{
  "0": "red",
  "1": "green",
  "2": "blue"
}
```

Array indexes become object property names.

# 🗁 Object Spread with Strings

Strings can be spread into objects because their indexed character properties are enumerable own properties.

```javascript
const language = "JS";

const result = {
  ...language
};

console.log(result);
```

Output:

```javascript
{
  "0": "J",
  "1": "S"
}
```

# 🗁 Object Spread with Other Primitive Values

Object spread can be used with primitive values.

Most primitives contribute no enumerable own properties.

```javascript
console.log({ ...42 });
console.log({ ...true });
console.log({ ...null });
console.log({ ...undefined });
```

Output:

```javascript
{}
{}
{}
{}
```

Strings are different because their indexed character properties are enumerable.

Object spread does not throw an error when spreading `null` or `undefined`.

# 🗁 Object Spread vs Object.assign()

Both object spread and `Object.assign()` can copy enumerable own properties from source objects.

However, they have different behaviors.

**Using object spread**

```javascript
const user = {
  name: "Muhammad",
  age: 25
};

const copy = {
  ...user
};
```

**Using Object.assign()**

```javascript
const user = {
  name: "Muhammad",
  age: 25
};

const copy = Object.assign({}, user);
```

Comparison:

| Object Spread | Object.assign() |
| --- | --- |
| Uses `...` syntax | Uses a method |
| Creates a new object literal | Copies into a target object |
| Does not mutate the source object | Mutates the target object |
| Copies enumerable own properties | Copies enumerable own properties |
| Performs shallow copying | Performs shallow copying |
| Defines properties on the new object | Uses ordinary property assignment on the target |

A significant difference appears when source properties have getters or setters, or when the target has setters. Object spread creates data properties on the new object using the retrieved values, while `Object.assign()` uses property assignment on its target.

# 🗁 Object Spread and Property Descriptors

Object spread copies property values, not the original property descriptors.

```javascript
const source = {};

Object.defineProperty(source, "name", {
  value: "Muhammad",
  enumerable: true,
  writable: false,
  configurable: false
});

const copy = {
  ...source
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

Non-enumerable properties and symbol properties are also handled according to their enumerability:

* Enumerable own string-keyed properties are copied.
* Enumerable own symbol-keyed properties are copied.
* Non-enumerable properties are not copied.
* Inherited properties are not copied.

# 🗁 Removing Properties with Rest and Spread

Object destructuring and the rest syntax can be combined to create a new object without a particular property.

```javascript
const user = {
  id: 101,
  name: "Muhammad",
  email: "muhammad@example.com",
  password: "secret"
};

const { password, ...safeUser } = user;

console.log(safeUser);
```

Output:

```javascript
{
  id: 101,
  name: "Muhammad",
  email: "muhammad@example.com"
}
```

The `password` property is excluded from the new object.

This technique is useful for shaping data, although it should not replace proper security controls when handling sensitive information.

# 🗁 Practical Example: Updating Application State

Object spread is commonly used in state-management patterns where existing objects should not be mutated.

```javascript
const state = {
  user: {
    name: "Muhammad",
    age: 25
  },
  theme: "light"
};

const updatedState = {
  ...state,
  user: {
    ...state.user,
    age: 26
  }
};

console.log(updatedState);
console.log(state);
```

Output:

```javascript
{
  user: {
    name: "Muhammad",
    age: 26
  },
  theme: "light"
}

{
  user: {
    name: "Muhammad",
    age: 25
  },
  theme: "light"
}
```

The original state remains unchanged.

This pattern is widely used in React state updates and other immutable state-management approaches.

# 🗁 Common Mistakes

**Assuming object spread performs a deep copy**

```javascript
const original = {
  address: {
    city: "Dhaka"
  }
};

const copy = {
  ...original
};

copy.address.city = "Chittagong";

console.log(original.address.city);
```

Output:

```text
Chittagong
```

Nested objects remain shared references.

**Forgetting property precedence**

```javascript
const defaults = {
  theme: "light"
};

const settings = {
  theme: "dark",
  ...defaults
};

console.log(settings.theme);
```

Output:

```text
light
```

The rightmost matching property wins.

**Expecting inherited properties to be copied**

Object spread copies only enumerable own properties, not inherited properties.

**Assuming spread preserves property descriptors**

Spread creates ordinary data properties rather than preserving the source descriptors.

**Confusing object spread with array spread**

Both use `...`, but their behavior depends on the surrounding syntax.

```javascript
const numbers = [1, 2, 3];

const arrayCopy = [...numbers];

const objectCopy = {
  ...numbers
};

console.log(arrayCopy);
console.log(objectCopy);
```

Output:

```javascript
[1, 2, 3]

{
  "0": 1,
  "1": 2,
  "2": 3
}
```

# 🗁 Best Practices

* Use object spread to create shallow copies of objects.
* Use object spread to merge related configuration objects.
* Place overriding properties after spread sources.
* Use nested spread when updating nested state immutably.
* Remember that nested objects and arrays retain their references unless copied separately.
* Avoid unnecessary repeated copying of large nested structures.
* Use destructuring with rest syntax when excluding properties.
* Use `Object.assign()` when its target-mutation behavior is specifically required.
* Do not assume spread preserves property descriptors or prototypes.

# 🗁 Key Takeaways

* Object spread uses the `...` syntax inside object literals.
* It copies enumerable own properties into a new object.
* Object spread creates a shallow copy, not a deep copy.
* Multiple objects can be merged using spread syntax.
* Properties appearing later override matching properties appearing earlier.
* Nested objects remain shared references unless separately copied.
* Object spread can conditionally include properties.
* It works with arrays, strings, and other values, following their enumerable own properties.
* Object spread and `Object.assign()` have different target-mutation and property-setting behavior.
* Object spread is an important tool for immutable object updates.
