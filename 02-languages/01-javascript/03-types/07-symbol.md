# JavaScript Symbol

A **Symbol** is a primitive data type introduced in ECMAScript 2015 (ES6). It represents a unique and immutable identifier.

Symbols are primarily used as unique object property keys to avoid naming collisions, create internal object properties, and define special behaviors through well-known symbols.

```javascript
const id = Symbol();

console.log(id);        // Symbol()
console.log(typeof id); // "symbol"
```

Every call to `Symbol()` creates a new, unique symbol, even when the descriptions are identical.

```javascript
const first = Symbol("id");
const second = Symbol("id");

console.log(first === second); // false
```

# 🗁 Creating Symbols

Use the `Symbol()` function to create a symbol primitive.

```javascript
const userId = Symbol();

const productId = Symbol("product-id");
const orderId = Symbol("order-id");

console.log(userId);    // Symbol()
console.log(productId); // Symbol(product-id)
console.log(orderId);   // Symbol(order-id)
```

The optional argument is called a **description**. It helps developers identify symbols during debugging.

The description does not determine symbol identity.

```javascript
const a = Symbol("key");
const b = Symbol("key");

console.log(a === b); // false
```

Symbols cannot be created using the `new` operator.

```javascript
// const id = new Symbol("id"); // TypeError
```

# 🗁 Symbol Uniqueness

Each symbol created with `Symbol()` is unique.

Even if two symbols have the same description, they are different values.

```javascript
const userId = Symbol("id");
const adminId = Symbol("id");

console.log(userId === adminId); // false
console.log(userId == adminId);  // false
```

This uniqueness makes symbols useful when multiple parts of an application need property keys without accidentally overwriting one another.

# 🗁 Symbols as Object Property Keys

Symbols can be used as object property keys.

Unlike string property keys, symbol property keys do not conflict with ordinary string keys, even when their descriptions look identical.

```javascript
const id = Symbol("id");

const user = {
    name: "John",
    [id]: 101
};

console.log(user.name); // "John"
console.log(user[id]);  // 101
```

Use square brackets to define or access a symbol-keyed property.

```javascript
const secretKey = Symbol("secret");

const account = {};

account[secretKey] = "private-data";

console.log(account[secretKey]); // "private-data"
```

**Avoid using dot notation for symbol keys.**

```javascript
const id = Symbol("id");

const user = {
    [id]: 123
};

console.log(user[id]); // 123

// user.id is a different string-keyed property.
console.log(user.id); // undefined
```

# 🗁 Preventing Property Name Collisions

Symbols are useful when different modules or libraries add properties to the same object.

Consider two modules that both need an `id` property.

```javascript
const moduleA = {
    id: "user-123"
};

const moduleB = {
    id: "session-456"
};
```

Using the same string property name can cause one module to overwrite another when properties are combined.

Symbols avoid this problem.

```javascript
const userId = Symbol("id");
const sessionId = Symbol("id");

const data = {
    [userId]: "user-123",
    [sessionId]: "session-456"
};

console.log(data[userId]);    // "user-123"
console.log(data[sessionId]); // "session-456"
```

Both properties coexist because their symbol keys are unique.

# 🗁 Symbol Properties Are Not Enumerable

Symbol-keyed properties are not included in many common object enumeration operations.

For example, `Object.keys()` returns only enumerable string-keyed property names.

```javascript
const id = Symbol("id");

const user = {
    name: "John",
    age: 25,
    [id]: 101
};

console.log(Object.keys(user));
// ["name", "age"]

console.log(Object.values(user));
// ["John", 25]

console.log(Object.entries(user));
// [["name", "John"], ["age", 25]]
```

Symbol properties are also ignored by `for...in`.

```javascript
for (const key in user) {
    console.log(key);
}

// name
// age
```

However, symbol properties are not automatically private. They can still be discovered using reflection methods.

# 🗁 Accessing Symbol Properties

JavaScript provides methods for retrieving symbol properties.

**`Object.getOwnPropertySymbols()`**

Returns an array containing the object's own symbol-keyed properties.

```javascript
const id = Symbol("id");
const role = Symbol("role");

const user = {
    name: "John",
    [id]: 101,
    [role]: "admin"
};

const symbols = Object.getOwnPropertySymbols(user);

console.log(symbols);
// [Symbol(id), Symbol(role)]

console.log(user[symbols[0]]); // 101
```

**`Reflect.ownKeys()`**

Returns all own property keys, including string and symbol keys.

```javascript
const id = Symbol("id");

const user = {
    name: "John",
    [id]: 101
};

console.log(Reflect.ownKeys(user));
// ["name", Symbol(id)]
```

**`Object.getOwnPropertyDescriptors()`**

Returns property descriptors for all own properties, including symbol-keyed properties.

```javascript
const id = Symbol("id");

const user = {
    name: "John",
    [id]: 101
};

console.log(Object.getOwnPropertyDescriptors(user));
```

# 🗁 Global Symbol Registry

JavaScript provides a global symbol registry that allows symbols to be shared across different parts of an application or execution contexts that use the same registry.

Use `Symbol.for()` to create or retrieve a symbol from the registry.

```javascript
const first = Symbol.for("application.id");
const second = Symbol.for("application.id");

console.log(first === second); // true
```

If a symbol with the specified key already exists in the registry, `Symbol.for()` returns it. Otherwise, it creates and registers a new symbol.

Compare it with ordinary symbol creation:

```javascript
const localSymbol = Symbol("application.id");

const globalSymbol = Symbol.for("application.id");

console.log(localSymbol === globalSymbol); // false
```

The ordinary symbol is not automatically registered in the global symbol registry.

# 🗁 Symbol.keyFor()

Use `Symbol.keyFor()` to retrieve the registry key associated with a registered symbol.

```javascript
const id = Symbol.for("user.id");

console.log(Symbol.keyFor(id)); // "user.id"
```

For a symbol that was not registered using `Symbol.for()`, the result is `undefined`.

```javascript
const localId = Symbol("user.id");

console.log(Symbol.keyFor(localId)); // undefined
```

The description of a symbol is different from its registry key.

```javascript
const id = Symbol("user.id");

console.log(id.description); // "user.id"
console.log(Symbol.keyFor(id)); // undefined
```

# 🗁 Symbol.description

The `description` property returns the optional description supplied when creating a symbol.

```javascript
const id = Symbol("user-id");

console.log(id.description); // "user-id"
```

A symbol created without a description has an `undefined` description.

```javascript
const id = Symbol();

console.log(id.description); // undefined
```

An empty description is different from an absent description.

```javascript
const first = Symbol("");
const second = Symbol();

console.log(first.description);  // ""
console.log(second.description); // undefined
```

# 🗁 Well-Known Symbols

JavaScript provides built-in symbols called **well-known symbols**.

These symbols allow developers to customize or control specific language behaviors.

They are exposed as properties of the `Symbol` constructor.

Important well-known symbols include:

| Symbol | Purpose |
| --- | --- |
| `Symbol.iterator` | Defines an object's default iterator |
| `Symbol.asyncIterator` | Defines an object's asynchronous iterator |
| `Symbol.toPrimitive` | Customizes primitive conversion |
| `Symbol.toStringTag` | Customizes the result of `Object.prototype.toString()` |
| `Symbol.hasInstance` | Customizes behavior of the `instanceof` operator |
| `Symbol.isConcatSpreadable` | Controls whether an object is flattened by `concat()` |
| `Symbol.match` | Customizes behavior in string matching |
| `Symbol.replace` | Customizes string replacement behavior |
| `Symbol.search` | Customizes string searching behavior |
| `Symbol.split` | Customizes string splitting behavior |
| `Symbol.species` | Controls constructors used by certain derived objects |
| `Symbol.unscopables` | Excludes properties from certain `with` environment lookups |

# 🗁 Symbol.iterator

`Symbol.iterator` defines the default iterator for an object.

It allows an object to be used with iteration constructs such as `for...of`.

```javascript
const collection = {
    values: [10, 20, 30],

    [Symbol.iterator]() {
        let index = 0;
        const values = this.values;

        return {
            next() {
                if (index < values.length) {
                    return {
                        value: values[index++],
                        done: false
                    };
                }

                return {
                    value: undefined,
                    done: true
                };
            }
        };
    }
};

for (const value of collection) {
    console.log(value);
}

// 10
// 20
// 30
```

The iterator protocol is covered in more detail in the JavaScript iterators section.

# 🗁 Symbol.toPrimitive

`Symbol.toPrimitive` allows an object to define how it converts into a primitive value.

The method receives a hint:

* `"string"`
* `"number"`
* `"default"`

```javascript
const user = {
    name: "John",
    age: 25,

    [Symbol.toPrimitive](hint) {
        if (hint === "string") {
            return this.name;
        }

        if (hint === "number") {
            return this.age;
        }

        return `${this.name} (${this.age})`;
    }
};

console.log(String(user)); // "John"
console.log(Number(user)); // 25
console.log(user + "");    // "John (25)"
```

This provides control over how an object behaves during primitive conversion.

# 🗁 Symbol.toStringTag

`Symbol.toStringTag` customizes the built-in object tag returned by `Object.prototype.toString()`.

```javascript
const user = {
    name: "John",

    get [Symbol.toStringTag]() {
        return "User";
    }
};

console.log(Object.prototype.toString.call(user));
// [object User]
```

This is useful when defining custom object types that should expose a meaningful tag.

# 🗁 Symbol.hasInstance

`Symbol.hasInstance` customizes the behavior of the `instanceof` operator.

```javascript
class EvenNumber {
    static [Symbol.hasInstance](value) {
        return typeof value === "number" && value % 2 === 0;
    }
}

console.log(4 instanceof EvenNumber); // true
console.log(5 instanceof EvenNumber); // false
```

Normally, `instanceof` checks whether a constructor's prototype appears in an object's prototype chain. `Symbol.hasInstance` allows a constructor to customize that behavior.

# 🗁 Symbols and JSON

Symbol-keyed properties are ignored by `JSON.stringify()`.

```javascript
const id = Symbol("id");

const user = {
    name: "John",
    [id]: 101
};

console.log(JSON.stringify(user));
// {"name":"John"}
```

Symbol values also do not serialize as ordinary JSON values.

```javascript
console.log(JSON.stringify(Symbol("id"))); // undefined

console.log(JSON.stringify({
    id: Symbol("id")
})); // "{}"
```

This behavior is important when working with APIs and JSON-based storage.

# 🗁 Symbol Conversion

Symbols have specific conversion rules.

**Explicit string conversion**

Use `String()` to convert a symbol into its descriptive string representation.

```javascript
const id = Symbol("user-id");

console.log(String(id)); // "Symbol(user-id)"
```

**Template literals**

Implicit string conversion of a symbol using a template literal throws a `TypeError`.

```javascript
const id = Symbol("user-id");

// console.log(`${id}`); // TypeError

console.log(String(id)); // "Symbol(user-id)"
```

**Numeric conversion**

Symbols cannot be converted into numbers.

```javascript
const id = Symbol("id");

// console.log(Number(id)); // TypeError
```

**Boolean conversion**

Symbols are truthy.

```javascript
const id = Symbol("id");

console.log(Boolean(id)); // true
console.log(!!id);        // true
```

# 🗁 Symbols Are Not Private Properties

Symbol-keyed properties are sometimes described as hidden properties, but they are not truly private.

They are excluded from common enumeration methods, but can still be accessed through reflection.

```javascript
const secret = Symbol("secret");

const user = {
    name: "John",
    [secret]: "private-looking-value"
};

const symbols = Object.getOwnPropertySymbols(user);

console.log(user[symbols[0]]);
// "private-looking-value"
```

For actual language-level private class fields, JavaScript provides the `#` private field syntax.

```javascript
class User {
    #password = "secret";

    getPassword() {
        return this.#password;
    }
}

const user = new User();

console.log(user.getPassword()); // "secret"

// console.log(user.#password); // SyntaxError
```

# 🗁 Practical Examples

**Creating collision-resistant property keys**

```javascript
const INTERNAL_ID = Symbol("internal-id");

function createUser(name, id) {
    return {
        name,
        [INTERNAL_ID]: id
    };
}

const user = createUser("John", 101);

console.log(user.name);          // "John"
console.log(user[INTERNAL_ID]);  // 101
```

**Sharing symbols across modules**

```javascript
// shared-keys.js
export const USER_ID = Symbol.for("app.user.id");
```

```javascript
// user-service.js
import { USER_ID } from "./shared-keys.js";

export function createUser(id, name) {
    return {
        [USER_ID]: id,
        name
    };
}
```

```javascript
// user-controller.js
import { USER_ID } from "./shared-keys.js";

const user = {
    [USER_ID]: 101,
    name: "John"
};

console.log(user[USER_ID]); // 101
```

The shared module provides a consistent symbol reference. `Symbol.for()` can also retrieve the same registered symbol using its registry key.

**Custom iterable object**

```javascript
const range = {
    start: 1,
    end: 3,

    *[Symbol.iterator]() {
        for (let value = this.start; value <= this.end; value++) {
            yield value;
        }
    }
};

console.log([...range]); // [1, 2, 3]
```

This example uses a generator method to implement the iterator protocol.

# 🗁 Best Practices

* Use symbols when unique property keys are required.
* Provide meaningful descriptions to make debugging easier.
* Use `Symbol()` when a unique local identifier is needed.
* Use `Symbol.for()` when a symbol must be shared through the global registry.
* Use well-known symbols when customizing built-in JavaScript behavior.
* Do not treat symbol properties as secure or truly private.
* Use private class fields (`#field`) when actual property privacy is required.
* Remember that `Object.keys()`, `Object.values()`, and `Object.entries()` ignore symbol-keyed properties.
* Use `Reflect.ownKeys()` or `Object.getOwnPropertySymbols()` when inspecting symbol properties.
* Avoid unnecessary symbols when ordinary string keys are sufficient.

# 🗁 Key Takeaways

* Symbol is a primitive data type introduced in ES6.
* Every `Symbol()` call creates a unique symbol.
* Symbol descriptions help identify symbols but do not determine identity.
* Symbols can be used as object property keys.
* Symbol-keyed properties are ignored by common string-key enumeration methods.
* `Symbol.for()` retrieves or creates a symbol in the global symbol registry.
* `Symbol.keyFor()` retrieves a registered symbol's key.
* Well-known symbols customize JavaScript language behavior.
* Symbols are not truly private or secure.
* JavaScript provides private class fields for actual language-level privacy.
