# JavaScript Symbols

**Symbol** is a primitive data type in JavaScript that represents a unique identifier.

Symbols are commonly used as object property keys when developers need unique properties that do not accidentally conflict with other property names.

They are useful for:

* Creating unique property keys
* Avoiding property name collisions
* Defining object behavior
* Implementing custom iteration
* Working with well-known symbols
* Creating internal object metadata
* Supporting JavaScript metaprogramming

# 🗁 What Is a Symbol?

A Symbol is a primitive value created using the `Symbol()` function.

Example:

```javascript
const id = Symbol();

console.log(id);
console.log(typeof id); // symbol
```

Each call to `Symbol()` creates a unique symbol.

```javascript
const first = Symbol();
const second = Symbol();

console.log(first === second); // false
```

Even when two symbols have the same description, they remain unique.

```javascript
const first = Symbol("id");
const second = Symbol("id");

console.log(first === second); // false
```

# 🗁 Creating Symbols

Symbols are created using the `Symbol()` function.

Syntax:

```javascript
const symbol = Symbol(description);
```

The description is optional and is primarily used for debugging.

Example:

```javascript
const userId = Symbol("userId");
const productId = Symbol("productId");

console.log(userId);
console.log(productId);
```

Output:

```text
Symbol(userId)
Symbol(productId)
```

A symbol description does not determine its identity.

```javascript
const first = Symbol("identifier");
const second = Symbol("identifier");

console.log(first === second); // false
```

# 🗁 Symbols Are Primitive Values

Symbols are primitive values, like strings, numbers, booleans, and BigInts.

Example:

```javascript
const id = Symbol("id");

console.log(typeof id); // symbol
console.log(id instanceof Symbol); // false
```

A symbol primitive is not an instance of the `Symbol` constructor.

Symbols cannot be created using `new Symbol()`.

```javascript
// const id = new Symbol("id");
// TypeError
```

# 🗁 Symbol as an Object Property Key

Symbols can be used as object property keys.

Example:

```javascript
const userId = Symbol("userId");

const user = {
    name: "Alice",
    age: 25,

    [userId]: 1001
};

console.log(user.name); // Alice
console.log(user[userId]); // 1001
```

The computed property syntax allows a symbol to be used as a property key.

# 🗁 Avoiding Property Name Collisions

Symbols are useful when multiple parts of an application need to add properties to the same object.

Example:

```javascript
const firstModuleId = Symbol("id");
const secondModuleId = Symbol("id");

const user = {
    [firstModuleId]: "Module A",
    [secondModuleId]: "Module B"
};

console.log(user[firstModuleId]);
console.log(user[secondModuleId]);
```

Both properties can coexist because their symbol keys are unique.

With ordinary string keys, assigning the same property name would overwrite the previous value.

# 🗁 Symbol Property Access

Symbol-keyed properties cannot be accessed using ordinary dot notation.

Example:

```javascript
const id = Symbol("id");

const user = {
    [id]: 1001
};

console.log(user[id]); // 1001
```

The following does not access the symbol property:

```javascript
console.log(user.id);
```

It attempts to access a string property named `"id"`.

# 🗁 Symbol Properties Are Not Enumerable

Symbol-keyed properties are excluded from several common property enumeration operations.

Example:

```javascript
const id = Symbol("id");

const user = {
    name: "Alice",
    age: 25,
    [id]: 1001
};

console.log(Object.keys(user));
// ["name", "age"]

console.log(Object.values(user));
// ["Alice", 25]

console.log(Object.entries(user));
// [["name", "Alice"], ["age", 25]]
```

Symbol properties are also ignored by `for...in`.

```javascript
for (const key in user) {
    console.log(key);
}
```

Output:

```text
name
age
```

However, symbol properties are not inherently private.

They can be discovered using reflection methods.

# 🗁 Retrieving Symbol Properties

JavaScript provides methods for retrieving symbol-keyed properties.

**Object.getOwnPropertySymbols()**

Returns an object's own symbol property keys.

```javascript
const id = Symbol("id");

const user = {
    name: "Alice",
    [id]: 1001
};

const symbols = Object.getOwnPropertySymbols(user);

console.log(symbols);
console.log(user[symbols[0]]);
```

**Reflect.ownKeys()**

Returns all own string and symbol property keys.

```javascript
const id = Symbol("id");

const user = {
    name: "Alice",
    [id]: 1001
};

console.log(Reflect.ownKeys(user));
```

# 🗁 Symbol.for()

`Symbol.for()` retrieves or creates a symbol in the global symbol registry.

Unlike `Symbol()`, repeated calls with the same registry key return the same symbol.

Example:

```javascript
const first = Symbol.for("userId");
const second = Symbol.for("userId");

console.log(first === second); // true
```

The registry allows different parts of an application to retrieve the same registered symbol.

# 🗁 Symbol.keyFor()

`Symbol.keyFor()` retrieves the registry key of a globally registered symbol.

Example:

```javascript
const userId = Symbol.for("userId");

console.log(Symbol.keyFor(userId));
// userId
```

For a symbol created using `Symbol()`, the result is `undefined`.

```javascript
const id = Symbol("userId");

console.log(Symbol.keyFor(id));
// undefined
```

# 🗁 Local Symbols vs Global Symbols

| Feature | Local Symbol | Global Registry Symbol |
| --- | --- | --- |
| Creation | `Symbol()` | `Symbol.for()` |
| Identity | Unique per creation | Shared by registry key |
| Registry membership | No | Yes |
| Lookup | No registry lookup | `Symbol.keyFor()` |
| Typical use | Unique local property keys | Shared identifiers |

Example:

```javascript
const local = Symbol("status");

const global = Symbol.for("status");

console.log(Symbol.keyFor(local));
// undefined

console.log(Symbol.keyFor(global));
// status
```

# 🗁 Symbol as a Constant

Symbols can be used as unique constants.

Example:

```javascript
const STATUS = {
    PENDING: Symbol("pending"),
    APPROVED: Symbol("approved"),
    REJECTED: Symbol("rejected")
};

function processRequest(status) {
    if (status === STATUS.PENDING) {
        return "Request is pending";
    }

    if (status === STATUS.APPROVED) {
        return "Request approved";
    }

    if (status === STATUS.REJECTED) {
        return "Request rejected";
    }

    return "Unknown status";
}

console.log(processRequest(STATUS.APPROVED));
```

This provides unique values that cannot accidentally collide with unrelated strings.

# 🗁 Well-Known Symbols

**Well-Known Symbols** are built-in symbols provided by JavaScript.

They define or customize the behavior of objects for specific language operations.

Common well-known symbols include:

* `Symbol.iterator`
* `Symbol.asyncIterator`
* `Symbol.toPrimitive`
* `Symbol.toStringTag`
* `Symbol.hasInstance`
* `Symbol.isConcatSpreadable`
* `Symbol.species`
* `Symbol.match`
* `Symbol.replace`
* `Symbol.search`
* `Symbol.split`
* `Symbol.toStringTag`
* `Symbol.unscopables`

# 🗁 Symbol.iterator

`Symbol.iterator` defines the default iterator for an object.

It is used by iterable-consuming language features such as `for...of` and spread syntax.

Example:

```javascript
const numbers = {
    values: [10, 20, 30],

    [Symbol.iterator]() {
        return this.values[Symbol.iterator]();
    }
};

console.log([...numbers]);
```

Output:

```javascript
[10, 20, 30]
```

# 🗁 Symbol.asyncIterator

`Symbol.asyncIterator` defines the asynchronous iterator method of an object.

It is used by `for await...of`.

Example:

```javascript
const numbers = {
    values: [10, 20, 30],

    async *[Symbol.asyncIterator]() {
        for (const value of this.values) {
            yield value;
        }
    }
};

async function displayNumbers() {
    for await (const number of numbers) {
        console.log(number);
    }
}

displayNumbers();
```

This allows an object to provide values through asynchronous iteration.

# 🗁 Symbol.toPrimitive

`Symbol.toPrimitive` customizes how an object converts into a primitive value.

It receives a hint describing the expected primitive conversion.

Possible hints:

* `"string"`
* `"number"`
* `"default"`

Example:

```javascript
const user = {
    name: "Alice",
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

console.log(String(user));
// Alice

console.log(Number(user));
// 25

console.log(user + "");
// Alice (25)
```

# 🗁 Symbol.toStringTag

`Symbol.toStringTag` customizes the built-in object tag used by `Object.prototype.toString()`.

Example:

```javascript
class User {
    get [Symbol.toStringTag]() {
        return "User";
    }
}

const user = new User();

console.log(Object.prototype.toString.call(user));
// [object User]
```

This can provide a more descriptive object tag.

# 🗁 Symbol.hasInstance

`Symbol.hasInstance` customizes the behavior of the `instanceof` operator.

Example:

```javascript
class EvenNumber {
    static [Symbol.hasInstance](value) {
        return Number.isInteger(value) && value % 2 === 0;
    }
}

console.log(2 instanceof EvenNumber);
// true

console.log(3 instanceof EvenNumber);
// false
```

The `instanceof` operator now uses the custom check defined by the class.

# 🗁 Symbol.isConcatSpreadable

`Symbol.isConcatSpreadable` controls whether an object is flattened into an array by `Array.prototype.concat()`.

Example:

```javascript
const numbers = [1, 2];

const values = [3, 4];

values[Symbol.isConcatSpreadable] = false;

const result = numbers.concat(values);

console.log(result);
// [1, 2, [3, 4]]
```

By default, arrays are spread into the resulting array.

Setting this symbol to `false` prevents that behavior.

# 🗁 Symbol.species

`Symbol.species` allows a class to customize the constructor used by certain built-in methods when creating derived objects.

Example:

```javascript
class MyArray extends Array {
    static get [Symbol.species]() {
        return Array;
    }
}

const numbers = new MyArray(1, 2, 3);

const result = numbers.map(number => number * 2);

console.log(result);
// [2, 4, 6]

console.log(result instanceof MyArray);
// false

console.log(result instanceof Array);
// true
```

The `map()` method creates a normal Array instead of another `MyArray`.

# 🗁 Symbol.match

`Symbol.match` customizes how an object behaves when used with the string `match()` method.

Example:

```javascript
const matcher = {
    [Symbol.match](text) {
        return text.includes("JavaScript");
    }
};

console.log("I love JavaScript".match(matcher));
// true

console.log("I love Python".match(matcher));
// false
```

# 🗁 Symbol.replace

`Symbol.replace` customizes replacement behavior when an object is passed to `String.prototype.replace()`.

Example:

```javascript
const replacer = {
    [Symbol.replace](text) {
        return text.replaceAll("JavaScript", "TypeScript");
    }
};

const result = "I love JavaScript".replace(replacer);

console.log(result);
// I love TypeScript
```

# 🗁 Symbol.search

`Symbol.search` customizes how an object behaves when used with `String.prototype.search()`.

Example:

```javascript
const searcher = {
    [Symbol.search](text) {
        return text.indexOf("JavaScript");
    }
};

console.log("I love JavaScript".search(searcher));
// 7
```

# 🗁 Symbol.split

`Symbol.split` customizes how an object behaves when used with `String.prototype.split()`.

Example:

```javascript
const separator = {
    [Symbol.split](text) {
        return text.split(" ");
    }
};

console.log("Hello JavaScript World".split(separator));
// ["Hello", "JavaScript", "World"]
```

# 🗁 Symbol.unscopables

`Symbol.unscopables` specifies properties that should be excluded from lookup within a `with` statement.

Example:

```javascript
const object = {
    name: "Alice",
    age: 25,

    [Symbol.unscopables]: {
        age: true
    }
};
```

This symbol is mainly relevant to language compatibility and legacy `with` statement behavior.

The `with` statement is not recommended in modern JavaScript.

# 🗁 Symbols and Object Property Descriptors

Symbol properties support ordinary property descriptors.

Example:

```javascript
const id = Symbol("id");

const user = {};

Object.defineProperty(user, id, {
    value: 1001,
    writable: false,
    enumerable: false,
    configurable: false
});

console.log(user[id]);
// 1001

console.log(Object.getOwnPropertySymbols(user));
// [Symbol(id)]
```

A symbol property can be writable, enumerable, and configurable just like a string-keyed property.

# 🗁 Symbols and JSON

Symbol-keyed properties are ignored by `JSON.stringify()`.

Example:

```javascript
const id = Symbol("id");

const user = {
    name: "Alice",
    age: 25,
    [id]: 1001
};

console.log(JSON.stringify(user));
```

Output:

```json
{"name":"Alice","age":25}
```

Symbol values are also not serialized as ordinary JSON values.

```javascript
console.log(JSON.stringify(Symbol("id")));
// undefined
```

# 🗁 Symbols and Object Spread

Object spread copies enumerable own string-keyed and symbol-keyed properties.

Example:

```javascript
const id = Symbol("id");

const user = {
    name: "Alice",
    [id]: 1001
};

const copy = {
    ...user
};

console.log(copy.name);
// Alice

console.log(copy[id]);
// 1001
```

Non-enumerable properties are not copied by object spread.

# 🗁 Symbols and Object.assign()

`Object.assign()` also copies enumerable own string-keyed and symbol-keyed properties.

Example:

```javascript
const id = Symbol("id");

const source = {
    name: "Alice",
    [id]: 1001
};

const target = Object.assign({}, source);

console.log(target.name);
// Alice

console.log(target[id]);
// 1001
```

# 🗁 Symbols and Privacy

Symbols can make property names less likely to collide with ordinary property names.

However, symbols do not provide true privacy.

Example:

```javascript
const secret = Symbol("secret");

const user = {
    name: "Alice",
    [secret]: "hidden value"
};

const symbols = Object.getOwnPropertySymbols(user);

console.log(user[symbols[0]]);
// hidden value
```

Code that can access the object can discover its own symbol properties.

For private class fields, use the `#` syntax.

```javascript
class User {
    #password = "secret";

    getPassword() {
        return this.#password;
    }
}

const user = new User();

console.log(user.getPassword());
// secret
```

# 🗁 Practical Example: Internal Metadata

Symbols can store metadata on objects without using ordinary string property names.

Example:

```javascript
const metadata = Symbol("metadata");

function attachMetadata(object, value) {
    object[metadata] = value;
}

const user = {
    name: "Alice"
};

attachMetadata(user, {
    role: "Developer",
    active: true
});

console.log(user[metadata]);
```

The metadata is accessible through the symbol reference but does not appear in ordinary `Object.keys()` output.

# 🗁 Practical Example: Unique Event Types

Symbols can provide unique identifiers for event types.

Example:

```javascript
const EVENTS = {
    USER_CREATED: Symbol("user-created"),
    USER_UPDATED: Symbol("user-updated"),
    USER_DELETED: Symbol("user-deleted")
};

function handleEvent(event) {
    switch (event.type) {
        case EVENTS.USER_CREATED:
            console.log("User created");
            break;

        case EVENTS.USER_UPDATED:
            console.log("User updated");
            break;

        case EVENTS.USER_DELETED:
            console.log("User deleted");
            break;

        default:
            console.log("Unknown event");
    }
}

handleEvent({
    type: EVENTS.USER_CREATED
});
```

# 🗁 Common Mistakes

**Assuming symbols with identical descriptions are equal**

```javascript
const first = Symbol("id");
const second = Symbol("id");

console.log(first === second);
// false
```

**Using `new Symbol()`**

```javascript
// const id = new Symbol("id");
// TypeError
```

**Assuming symbols are completely private**

Symbol properties can be discovered using `Object.getOwnPropertySymbols()` or `Reflect.ownKeys()`.

**Expecting symbols to appear in Object.keys()**

```javascript
const id = Symbol("id");

const user = {
    name: "Alice",
    [id]: 1001
};

console.log(Object.keys(user));
// ["name"]
```

**Confusing Symbol() with Symbol.for()**

`Symbol()` creates a unique symbol, while `Symbol.for()` retrieves or creates a globally registered symbol.

# 🗁 Best Practices

* Use symbols when unique property keys are required.
* Prefer descriptive symbol descriptions for debugging.
* Use `Symbol.for()` only when shared registry identity is needed.
* Use `Symbol.keyFor()` to inspect global registry keys.
* Use well-known symbols to customize standard JavaScript behavior.
* Do not treat symbol properties as secure private storage.
* Use private class fields for actual language-level private state.
* Remember that symbol-keyed properties are included in `Reflect.ownKeys()`.
* Understand how symbols behave with object spread, JSON serialization, and property enumeration.
* Avoid creating symbols when ordinary string keys are sufficient.

# 🗁 Key Takeaways

* Symbol is a JavaScript primitive data type.
* Each `Symbol()` call creates a unique value.
* Symbols can be used as object property keys.
* Symbol descriptions are for identification and debugging, not equality.
* `Symbol.for()` uses the global symbol registry.
* `Symbol.keyFor()` retrieves a registered symbol's key.
* Well-known symbols customize built-in language behavior.
* `Symbol.iterator` defines an object's default iterator.
* Symbol-keyed properties are ignored by `Object.keys()` and JSON serialization.
* `Reflect.ownKeys()` and `Object.getOwnPropertySymbols()` can retrieve symbol properties.
* Symbols do not provide true privacy.

# 🗁 Related Topics

* Primitive Types
* Objects
* Property Descriptors
* Iterators
* Generators
* Proxy
* Reflect
* Classes
* Private Fields
* Metaprogramming
