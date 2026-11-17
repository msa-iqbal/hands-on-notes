# JavaScript for...in Loop

The **`for...in` loop** is an iteration statement used to iterate over the enumerable property keys of an object.

It allows you to access object property names without manually retrieving them through methods such as `Object.keys()`.

The loop iterates over property keys, not property values.

**Example**

```javascript
const user = {
    name: "John",
    age: 25,
    city: "London"
};

for (const key in user) {
    console.log(key);
}
```

**Output**

```text
name
age
city
```

The variable `key` receives each enumerable property name.

To access the corresponding property value, use bracket notation.

```javascript
for (const key in user) {
    console.log(key, user[key]);
}
```

**Output**

```text
name John
age 25
city London
```

# 🗁 Syntax

The basic syntax is:

```javascript
for (variable in object) {
    // Code to execute for each property key
}
```

Where:

* `variable` receives the current property key.
* `object` is the object whose enumerable properties are being iterated.
* The loop body executes once for each enumerated property key.

You can declare the iteration variable using `const`, `let`, or `var`.

**Using const**

```javascript
const user = {
    name: "Alice",
    age: 28
};

for (const key in user) {
    console.log(key);
}
```

**Using let**

```javascript
const user = {
    name: "Alice",
    age: 28
};

for (let key in user) {
    key = key.toUpperCase();

    console.log(key);
}
```

**Output**

```text
NAME
AGE
```

Using `const` is generally preferred when you do not need to reassign the iteration variable.

# 🗁 How for...in Works

The `for...in` loop enumerates the enumerable property keys of an object.

Its behavior includes:

* Enumerating own enumerable string-keyed properties.
* Including inherited enumerable string-keyed properties.
* Skipping non-enumerable properties.
* Skipping symbol-keyed properties.

Property enumeration order is defined by JavaScript rules, although relying on `for...in` for a particular ordering is generally unnecessary.

**Example**

```javascript
const user = {
    name: "John",
    age: 25,
    city: "London"
};

for (const key in user) {
    console.log(key);
}
```

**Output**

```text
name
age
city
```

The loop retrieves each enumerable property key and assigns it to `key`.

# 🗁 Iterating Over Object Properties

The most common use of `for...in` is iterating over object properties.

**Example**

```javascript
const product = {
    id: 101,
    name: "Laptop",
    price: 1200,
    brand: "Dell"
};

for (const key in product) {
    console.log(key);
}
```

**Output**

```text
id
name
price
brand
```

**Accessing property values**

```javascript
for (const key in product) {
    console.log(`${key}: ${product[key]}`);
}
```

**Output**

```text
id: 101
name: Laptop
price: 1200
brand: Dell
```

**Building a formatted object summary**

```javascript
const user = {
    name: "Alice",
    age: 28,
    role: "Developer"
};

for (const key in user) {
    console.log(`${key.toUpperCase()}: ${user[key]}`);
}
```

**Output**

```text
NAME: Alice
AGE: 28
ROLE: Developer
```

# 🗁 Iterating Over Arrays

Although `for...in` can iterate over arrays, it is generally not recommended for processing array elements.

It iterates over enumerable property keys, which are typically array indexes represented as strings.

**Example**

```javascript
const fruits = ["Apple", "Banana", "Orange"];

for (const index in fruits) {
    console.log(index);
}
```

**Output**

```text
0
1
2
```

To access array values, use bracket notation.

```javascript
for (const index in fruits) {
    console.log(fruits[index]);
}
```

**Output**

```text
Apple
Banana
Orange
```

For ordinary array iteration, prefer `for...of`.

```javascript
for (const fruit of fruits) {
    console.log(fruit);
}
```

**Output**

```text
Apple
Banana
Orange
```

# 🗁 Inherited Properties

One important characteristic of `for...in` is that it can enumerate inherited enumerable properties.

JavaScript objects can inherit properties through their prototype chain.

**Example**

```javascript
const parent = {
    country: "Bangladesh"
};

const user = Object.create(parent);

user.name = "John";
user.age = 25;

for (const key in user) {
    console.log(key);
}
```

**Output**

```text
name
age
country
```

The loop includes:

* `name` and `age`, which are own properties.
* `country`, which is inherited from `parent`.

This behavior matters when working with objects that have custom prototypes.

# 🗁 Object.hasOwn()

To process only an object's own properties, use `Object.hasOwn()` inside the loop.

**Example**

```javascript
const parent = {
    country: "Bangladesh"
};

const user = Object.create(parent);

user.name = "John";
user.age = 25;

for (const key in user) {
    if (!Object.hasOwn(user, key)) {
        continue;
    }

    console.log(key, user[key]);
}
```

**Output**

```text
name John
age 25
```

`Object.hasOwn()` returns:

* `true` if the object has the specified property as its own property.
* `false` if the property is inherited or does not exist.

It is generally preferred over calling `hasOwnProperty()` directly because an object may override that method or have no prototype.

**Alternative using hasOwnProperty()**

```javascript
for (const key in user) {
    if (Object.prototype.hasOwnProperty.call(user, key)) {
        console.log(key, user[key]);
    }
}
```

# 🗁 Object.keys() vs for...in

`Object.keys()` returns an array of an object's own enumerable string-keyed property names.

Unlike `for...in`, it does not include inherited properties.

**Example using Object.keys()**

```javascript
const parent = {
    country: "Bangladesh"
};

const user = Object.create(parent);

user.name = "John";
user.age = 25;

console.log(Object.keys(user));
```

**Output**

```javascript
["name", "age"]
```

**Example using for...in**

```javascript
for (const key in user) {
    console.log(key);
}
```

**Output**

```text
name
age
country
```

Use `Object.keys()` when you need an array of own enumerable property names.

Use `for...in` when you specifically need to enumerate enumerable string property keys, including inherited ones.

# 🗁 Object.values() and Object.entries()

JavaScript provides other methods for object iteration.

**Object.values()**

Returns an array containing an object's own enumerable property values.

```javascript
const user = {
    name: "John",
    age: 25,
    city: "London"
};

for (const value of Object.values(user)) {
    console.log(value);
}
```

**Output**

```text
John
25
London
```

**Object.entries()**

Returns an array containing an object's own enumerable key-value pairs.

```javascript
for (const [key, value] of Object.entries(user)) {
    console.log(`${key}: ${value}`);
}
```

**Output**

```text
name: John
age: 25
city: London
```

These methods are useful when you want to combine object iteration with `for...of`.

# 🗁 Enumerable and Non-Enumerable Properties

JavaScript properties have an `enumerable` attribute.

A property is enumerable when its descriptor has `enumerable: true`.

Properties created through ordinary object literal syntax are enumerable by default.

Properties created with `Object.defineProperty()` are non-enumerable by default unless explicitly configured.

**Example**

```javascript
const user = {
    name: "John",
    age: 25
};

Object.defineProperty(user, "password", {
    value: "secret",
    enumerable: false
});

for (const key in user) {
    console.log(key);
}
```

**Output**

```text
name
age
```

The `password` property is not included because it is non-enumerable.

You can inspect a property's enumerability using `propertyIsEnumerable()`.

```javascript
console.log(user.propertyIsEnumerable("name"));     // true
console.log(user.propertyIsEnumerable("password")); // false
```

# 🗁 Symbol Properties

`for...in` does not enumerate symbol-keyed properties.

**Example**

```javascript
const id = Symbol("id");

const user = {
    name: "John",
    age: 25,
    [id]: 101
};

for (const key in user) {
    console.log(key);
}
```

**Output**

```text
name
age
```

The symbol property is ignored.

Use `Object.getOwnPropertySymbols()` or `Reflect.ownKeys()` when you need to retrieve symbol keys.

```javascript
console.log(Object.getOwnPropertySymbols(user));
```

**Output**

```javascript
[Symbol(id)]
```

# 🗁 break and continue with for...in

The `for...in` loop supports both `break` and `continue`.

**Using break**

The `break` statement terminates the loop.

```javascript
const user = {
    name: "John",
    age: 25,
    city: "London",
    role: "Developer"
};

for (const key in user) {
    if (key === "city") {
        break;
    }

    console.log(key);
}
```

**Output**

```text
name
age
```

**Using continue**

The `continue` statement skips the current property.

```javascript
for (const key in user) {
    if (key === "age") {
        continue;
    }

    console.log(key);
}
```

**Output**

```text
name
city
role
```

# 🗁 Labeled for...in Loops

Labels can be used with nested `for...in` loops to control which enclosing loop is affected by `break` or `continue`.

**Example**

```javascript
const users = {
    user1: {
        name: "John",
        active: true
    },
    user2: {
        name: "Alice",
        active: false
    },
    user3: {
        name: "David",
        active: true
    }
};

outerLoop:
for (const userKey in users) {
    const user = users[userKey];

    for (const property in user) {
        if (property === "active" && !user[property]) {
            continue outerLoop;
        }

        console.log(property, user[property]);
    }
}
```

**Output**

```text
name John
active true
name David
active true
```

When an inactive user is found, the current inner loop is abandoned and the next outer iteration begins.

# 🗁 Practical Examples

**Display all configuration properties**

```javascript
const config = {
    host: "localhost",
    port: 3000,
    environment: "development",
    debug: true
};

for (const key in config) {
    console.log(`${key} = ${config[key]}`);
}
```

**Output**

```text
host = localhost
port = 3000
environment = development
debug = true
```

**Count object properties**

```javascript
const user = {
    name: "John",
    age: 25,
    city: "London",
    role: "Developer"
};

let count = 0;

for (const key in user) {
    if (Object.hasOwn(user, key)) {
        count++;
    }
}

console.log(count); // 4
```

**Find a property by value**

```javascript
const user = {
    name: "John",
    age: 25,
    city: "London"
};

const searchValue = "London";

for (const key in user) {
    if (Object.hasOwn(user, key) && user[key] === searchValue) {
        console.log(`Found in property: ${key}`);
    }
}
```

**Output**

```text
Found in property: city
```

**Filter sensitive properties**

```javascript
const user = {
    id: 101,
    name: "John",
    email: "john@example.com",
    password: "secret",
    token: "abc123"
};

const sensitiveProperties = ["password", "token"];

for (const key in user) {
    if (!Object.hasOwn(user, key)) {
        continue;
    }

    if (sensitiveProperties.includes(key)) {
        continue;
    }

    console.log(`${key}: ${user[key]}`);
}
```

**Output**

```text
id: 101
name: John
email: john@example.com
```

**Convert an object into formatted text**

```javascript
const product = {
    name: "Laptop",
    price: 1200,
    brand: "Dell"
};

let result = "";

for (const key in product) {
    if (!Object.hasOwn(product, key)) {
        continue;
    }

    result += `${key}: ${product[key]}\n`;
}

console.log(result);
```

**Output**

```text
name: Laptop
price: 1200
brand: Dell
```

# 🗁 for...in vs for...of

These loops have different purposes.

| Feature | for...in | for...of |
| --- | --- | --- |
| Iterates over | Enumerable property keys | Iterable values |
| Common use | Objects | Arrays, strings, Maps, Sets |
| Array result | Index keys | Element values |
| Plain object support | Yes | No, unless iterable |
| Inherited enumerable properties | Included | Not applicable to ordinary value iteration |
| Uses | Property enumeration | Iterable protocol |

**for...in example**

```javascript
const fruits = ["Apple", "Banana", "Orange"];

for (const index in fruits) {
    console.log(index);
}
```

**Output**

```text
0
1
2
```

**for...of example**

```javascript
for (const fruit of fruits) {
    console.log(fruit);
}
```

**Output**

```text
Apple
Banana
Orange
```

Remember:

* Use `for...in` for enumerable property keys.
* Use `for...of` for iterable values.

# 🗁 Common Mistakes

**Confusing property keys with values**

```javascript
const user = {
    name: "John",
    age: 25
};

for (const value in user) {
    console.log(value);
}
```

This outputs property keys, not values.

Correct:

```javascript
for (const key in user) {
    console.log(user[key]);
}
```

**Ignoring inherited properties**

```javascript
const parent = {
    role: "admin"
};

const user = Object.create(parent);

user.name = "John";

for (const key in user) {
    console.log(key);
}
```

This includes both `name` and the inherited `role`.

Use `Object.hasOwn()` if only own properties should be processed.

**Using for...in for ordinary array iteration**

```javascript
const numbers = [10, 20, 30];

for (const index in numbers) {
    console.log(numbers[index]);
}
```

Although this works, `for...of` is generally more suitable when processing array values.

**Assuming symbol properties are included**

```javascript
const id = Symbol("id");

const user = {
    name: "John",
    [id]: 101
};

for (const key in user) {
    console.log(key);
}
```

The symbol property is skipped.

Use `Reflect.ownKeys()` when all own property keys, including symbols and non-enumerable keys, are required.

**Modifying object properties during iteration**

Adding, deleting, or changing properties while enumerating an object can make iteration behavior harder to reason about.

Prefer preparing the object before iteration or collecting the keys first when predictable processing is important.

# 🗁 Best Practices

* Use `for...in` primarily for object property enumeration.
* Use `Object.hasOwn()` when only own properties should be processed.
* Prefer `for...of` for array and iterable value iteration.
* Use `Object.keys()`, `Object.values()`, and `Object.entries()` when their array-based results are more convenient.
* Use `Reflect.ownKeys()` when all own keys, including symbols and non-enumerable properties, are needed.
* Avoid relying on inherited properties unless they are intentionally part of the operation.
* Avoid modifying an object during enumeration.
* Prefer `const` for iteration variables when reassignment is unnecessary.
* Use bracket notation to access a property dynamically through its key.

# 🗁 Key Takeaways

* `for...in` iterates over enumerable string-keyed property names.
* It works with ordinary JavaScript objects.
* It can include inherited enumerable properties.
* It does not enumerate symbol-keyed or non-enumerable properties.
* Use `object[key]` to access property values.
* Use `Object.hasOwn()` to restrict iteration to own properties.
* Use `Object.keys()`, `Object.values()`, and `Object.entries()` for alternative object iteration approaches.
* Prefer `for...of` for iterable values such as array elements.
* Use `for...in` when the keys of an object's enumerable properties are what you need.
