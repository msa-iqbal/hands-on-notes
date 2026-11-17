# Serialization

Serialization is the process of converting data structures or objects into a format that can be stored, transmitted, or reconstructed later.

The reverse process is called deserialization.

Serialization is commonly used in:

* API communication.
* Database storage.
* Local Storage and Session Storage.
* File handling.
* Application state persistence.
* Distributed systems.
* Message queues.
* Network communication.

JavaScript provides several mechanisms for serialization, including JSON serialization through `JSON.stringify()`.

Example:

```javascript
const user = {
  id: 101,
  name: "Alice",
  role: "Developer"
};

// Serialization
const serialized = JSON.stringify(user);

// Deserialization
const deserialized = JSON.parse(serialized);

console.log(deserialized.name); // Alice
```

# 🗁 Understanding Serialization

Serialization converts an in-memory representation into a transferable or storable representation.

Consider a JavaScript object:

```javascript
const user = {
  id: 101,
  name: "Alice",
  isActive: true
};
```

The object exists as a JavaScript runtime value.

It can be serialized into JSON:

```json
{
  "id": 101,
  "name": "Alice",
  "isActive": true
}
```

The JSON representation is text that can be stored in a file, transmitted over a network, or saved in browser storage.

The general workflow is:

```text
JavaScript Data
      |
      v
Serialization
      |
      v
Serialized Representation
      |
      v
Storage / Network / File
      |
      v
Serialized Representation
      |
      v
Deserialization
      |
      v
JavaScript Data
```

# 🗁 Serialization vs Deserialization

These operations are opposites.

| Feature | Serialization | Deserialization |
| --- | --- | --- |
| Purpose | Convert data into a transferable representation | Reconstruct data from a serialized representation |
| Direction | Object to serialized data | Serialized data to object |
| JSON method | `JSON.stringify()` | `JSON.parse()` |
| Common use | Sending or storing data | Receiving or loading data |

Example:

```javascript
const product = {
  id: 1,
  name: "Laptop",
  price: 1200
};

// Serialize
const json = JSON.stringify(product);

// Deserialize
const restoredProduct = JSON.parse(json);

console.log(restoredProduct);
```

# 🗁 JSON Serialization

JSON is one of the most commonly used serialization formats in JavaScript.

It supports:

* Objects.
* Arrays.
* Strings.
* Numbers.
* Booleans.
* `null`.

Example:

```javascript
const order = {
  id: 501,
  customer: "Alice",
  items: [
    {
      product: "Keyboard",
      quantity: 1
    },
    {
      product: "Mouse",
      quantity: 2
    }
  ],
  paid: true
};

const serializedOrder = JSON.stringify(order);

console.log(serializedOrder);
```

Deserialization:

```javascript
const restoredOrder = JSON.parse(serializedOrder);

console.log(restoredOrder.items[0].product);
```

JSON is language-independent, making it useful for communication between frontend and backend applications.

# 🗁 Limitations of JSON Serialization

JSON does not preserve every JavaScript value or object type.

Some values are omitted, converted, or cause serialization errors.

| JavaScript Value | JSON Serialization Behavior |
| --- | --- |
| String | Preserved |
| Number | Preserved, except non-finite numbers become `null` |
| Boolean | Preserved |
| `null` | Preserved |
| `undefined` in object | Property omitted |
| `undefined` in array | Converted to `null` |
| Function in object | Property omitted |
| Symbol-valued property | Property omitted |
| BigInt | Throws `TypeError` by default |
| Date | Converted into a string |
| Map | Usually becomes `{}` |
| Set | Usually becomes `{}` |
| Circular reference | Throws `TypeError` |

Example:

```javascript
const data = {
  name: "Alice",
  createdAt: new Date("2026-11-14T10:30:00Z"),
  settings: new Map([
    ["theme", "dark"]
  ])
};

const json = JSON.stringify(data);

console.log(json);
```

The Date becomes a string, while the Map does not retain its entries.

Applications must account for these limitations when designing serialization formats.

# 🗁 Custom Serialization with toJSON()

JavaScript objects can define a `toJSON()` method to customize their serialized representation.

Example:

```javascript
const user = {
  id: 101,
  name: "Alice",
  password: "secret",

  toJSON() {
    return {
      id: this.id,
      name: this.name
    };
  }
};

console.log(JSON.stringify(user));
```

Output:

```json
{
  "id": 101,
  "name": "Alice"
}
```

The `toJSON()` method controls what `JSON.stringify()` produces for the object.

This is useful when an object should expose only a specific data representation.

# 🗁 Custom Serialization with a Replacer

The replacer function can transform values during JSON serialization.

Example:

```javascript
const user = {
  name: "Alice",
  age: 25,
  password: "secret"
};

const json = JSON.stringify(user, (key, value) => {
  if (key === "password") {
    return undefined;
  }

  return value;
});

console.log(json);
```

The password property is excluded from the serialized output.

A replacer is useful for filtering or transforming data, but sensitive information should also be controlled at the data-model and API-design levels.

# 🗁 Custom Deserialization with a Reviver

The reviver function allows values to be transformed during JSON parsing.

Example:

```javascript
const json = `{
  "name": "Alice",
  "createdAt": "2026-11-14T10:30:00.000Z"
}`;

const user = JSON.parse(json, (key, value) => {
  if (key === "createdAt") {
    return new Date(value);
  }

  return value;
});

console.log(user.createdAt instanceof Date); // true
```

The reviver restores the Date object from its serialized string representation.

# 🗁 Serializing Date Objects

JSON does not have a dedicated Date data type.

JavaScript Date objects are converted into ISO 8601 strings.

Example:

```javascript
const event = {
  title: "Meeting",
  scheduledAt: new Date("2026-11-14T10:00:00Z")
};

const json = JSON.stringify(event);

console.log(json);
```

Output:

```json
{
  "title": "Meeting",
  "scheduledAt": "2026-11-14T10:00:00.000Z"
}
```

When restoring the object, convert the date string explicitly:

```javascript
const restoredEvent = JSON.parse(json, (key, value) => {
  if (key === "scheduledAt") {
    return new Date(value);
  }

  return value;
});
```

For complex applications, define which fields represent dates rather than converting every string that resembles a date.

# 🗁 Serializing Map and Set

Map and Set instances require custom handling because JSON does not directly preserve their collection types.

**Map serialization**

Convert a Map into an array of entries:

```javascript
const userRoles = new Map([
  ["Alice", "admin"],
  ["Bob", "editor"]
]);

const json = JSON.stringify([...userRoles]);

console.log(json);
```

Output:

```json
[
  ["Alice", "admin"],
  ["Bob", "editor"]
]
```

Restore it:

```javascript
const restoredRoles = new Map(JSON.parse(json));

console.log(restoredRoles.get("Alice"));
```

**Set serialization**

Convert a Set into an array:

```javascript
const skills = new Set([
  "JavaScript",
  "TypeScript",
  "Node.js"
]);

const json = JSON.stringify([...skills]);
```

Restore it:

```javascript
const restoredSkills = new Set(JSON.parse(json));

console.log(restoredSkills.has("JavaScript"));
```

The application must know which serialized values should be reconstructed as Maps or Sets.

# 🗁 Serializing BigInt

BigInt values cannot be serialized directly by standard `JSON.stringify()` behavior.

A common approach is converting them into strings.

```javascript
const account = {
  id: 9007199254740993n
};

const json = JSON.stringify(account, (key, value) => {
  if (typeof value === "bigint") {
    return {
      $type: "BigInt",
      value: value.toString()
    };
  }

  return value;
});

console.log(json);
```

Output:

```json
{
  "id": {
    "$type": "BigInt",
    "value": "9007199254740993"
  }
}
```

Restore the BigInt using a reviver:

```javascript
const restoredAccount = JSON.parse(json, (key, value) => {
  if (
    value &&
    typeof value === "object" &&
    value.$type === "BigInt"
  ) {
    return BigInt(value.value);
  }

  return value;
});

console.log(typeof restoredAccount.id); // bigint
```

Tagged representations should be designed carefully to avoid collisions with ordinary application data.

# 🗁 Structured Cloning

The structured clone algorithm is another mechanism for copying structured JavaScript data.

JavaScript exposes it through:

```javascript
structuredClone(value);
```

Example:

```javascript
const original = {
  name: "Alice",
  createdAt: new Date(),
  skills: new Set(["JavaScript", "TypeScript"])
};

const clone = structuredClone(original);

console.log(clone);
console.log(clone.createdAt instanceof Date); // true
console.log(clone.skills instanceof Set); // true
```

Unlike JSON serialization, structured cloning supports many additional built-in types.

It also supports circular references.

Example:

```javascript
const original = {
  name: "Alice"
};

original.self = original;

const clone = structuredClone(original);

console.log(clone.self === clone); // true
```

However, structured cloning is primarily a data-copying mechanism, not a general-purpose serialization format for files or network protocols.

It does not preserve functions, DOM nodes, or arbitrary prototype chains.

# 🗁 JSON Serialization vs Structured Cloning

| Feature | JSON Serialization | Structured Cloning |
| --- | --- | --- |
| Main purpose | Convert data into JSON text | Copy structured data |
| API | `JSON.stringify()` and `JSON.parse()` | `structuredClone()` |
| Output | JSON string | JavaScript value |
| Date | Converted to string | Preserved as Date |
| Map | Requires custom conversion | Supported |
| Set | Requires custom conversion | Supported |
| BigInt | Not directly supported | Supported |
| Circular references | Not supported | Supported |
| Functions | Not supported | Not supported |
| Network transmission | Commonly used | Not directly a standard text format |

Choose JSON when data needs to be exchanged or stored in a portable text format.

Choose structured cloning when copying supported JavaScript data structures.

# 🗁 Serialization in Local Storage

Local Storage stores strings, so serialization is needed for structured data.

Example:

```javascript
const preferences = {
  theme: "dark",
  language: "en",
  notifications: true
};

localStorage.setItem(
  "preferences",
  JSON.stringify(preferences)
);
```

Restore the data:

```javascript
const storedPreferences = localStorage.getItem("preferences");

const preferences = storedPreferences
  ? JSON.parse(storedPreferences)
  : null;

console.log(preferences);
```

This pattern is useful for persisting application preferences.

Remember that Local Storage is not suitable for storing sensitive authentication credentials or large datasets.

# 🗁 Serialization in Session Storage

Session Storage also stores strings.

Example:

```javascript
const formData = {
  firstName: "Alice",
  lastName: "Smith",
  email: "alice@example.com"
};

sessionStorage.setItem(
  "formData",
  JSON.stringify(formData)
);
```

Restore it:

```javascript
const storedData = sessionStorage.getItem("formData");

if (storedData !== null) {
  const formData = JSON.parse(storedData);

  console.log(formData.email);
}
```

This is useful for preserving temporary form state during a browser tab session.

# 🗁 Serialization in API Communication

Serialization is essential when sending structured data to backend services.

Example:

```javascript
async function createProduct() {
  const product = {
    name: "Mechanical Keyboard",
    price: 120,
    inStock: true
  };

  const response = await fetch("/api/products", {
    method: "POST",
    headers: {
      "Content-Type": "application/json"
    },
    body: JSON.stringify(product)
  });

  if (!response.ok) {
    throw new Error(`Request failed: ${response.status}`);
  }

  return response.json();
}
```

The request body contains serialized JSON text.

The backend parses this text into a data structure appropriate for its programming language.

# 🗁 Serialization in Web Workers

Web Workers execute JavaScript in a separate execution context.

Messages sent between the main thread and workers use structured cloning by default.

Main thread:

```javascript
const worker = new Worker("/worker.js");

worker.postMessage({
  type: "PROCESS_DATA",
  payload: {
    values: [10, 20, 30]
  }
});
```

Worker:

```javascript
self.addEventListener("message", (event) => {
  const { type, payload } = event.data;

  if (type === "PROCESS_DATA") {
    const total = payload.values.reduce(
      (sum, value) => sum + value,
      0
    );

    self.postMessage({
      type: "RESULT",
      total
    });
  }
});
```

Structured cloning allows supported data to be transferred between execution contexts without manually converting it into JSON strings.

Some objects also support transferable ownership, which can avoid copying large underlying data buffers.

# 🗁 Serialization Formats

JSON is not the only serialization format.

Common formats include:

| Format | Characteristics |
| --- | --- |
| JSON | Human-readable, widely supported, text-based |
| XML | Structured, tag-based text format |
| YAML | Human-readable configuration and data format |
| Protocol Buffers | Compact, schema-based binary format |
| MessagePack | Compact binary representation of structured data |
| CBOR | Binary data format designed for compactness and extensibility |

Different formats are appropriate for different requirements.

JSON is common in web APIs, while binary formats may be useful when compactness, schema enforcement, or performance is important.

# 🗁 Serialization vs Encoding vs Encryption

These concepts are related but different.

| Concept | Purpose | Example |
| --- | --- | --- |
| Serialization | Convert structured data into a transferable representation | `JSON.stringify()` |
| Encoding | Represent data in a particular character or byte format | UTF-8, Base64 |
| Encryption | Protect confidentiality using cryptographic techniques | AES-GCM |

Example:

```javascript
const user = {
  name: "Alice"
};

const json = JSON.stringify(user);
```

This serializes the object.

Encoding the JSON string as UTF-8 bytes is a separate operation.

Encryption is another separate operation used when confidentiality is required.

Serialization alone does not provide security or confidentiality.

# 🗁 Common Mistakes

**Assuming JSON preserves every JavaScript type**

JSON has a limited data model. Functions, symbols, BigInt, Map, and Set require special handling.

**Using JSON serialization for every deep-cloning requirement**

JSON serialization changes or discards unsupported values and cannot handle circular references.

**Forgetting to deserialize stored data**

Local Storage and Session Storage return strings. Parse serialized JSON before accessing object properties.

**Assuming Date objects are automatically restored**

Date values become strings during JSON serialization. Convert them back explicitly.

**Ignoring circular references**

Circular references cause `JSON.stringify()` to throw a `TypeError`.

**Trusting serialized data without validation**

Data loaded from storage or external systems can be malformed or manipulated. Validate it before using it.

# 🗁 Best Practices

* Choose a serialization format based on application requirements.
* Use JSON for interoperable text-based data exchange.
* Define clear schemas for serialized data.
* Handle unsupported JavaScript types explicitly.
* Use `structuredClone()` for supported in-memory deep copies.
* Validate deserialized data.
* Avoid serializing unnecessary or sensitive information.
* Version persistent data structures when their format may change.
* Handle malformed stored data gracefully.
* Separate serialization, encoding, and encryption responsibilities.

# 🗁 Summary

Serialization converts structured data into a representation suitable for storage or transmission. Deserialization reconstructs data from that representation.

Important concepts include:

* Serialization and deserialization.
* JSON serialization.
* Replacer and reviver functions.
* Custom `toJSON()` methods.
* Date, Map, Set, and BigInt handling.
* Structured cloning.
* Serialization in browser storage.
* Serialization in API communication.
* Web Worker message cloning.
* JSON, XML, YAML, and binary formats.
* Differences between serialization, encoding, and encryption.

Understanding serialization is essential for building applications that exchange, persist, and reconstruct structured data reliably.
