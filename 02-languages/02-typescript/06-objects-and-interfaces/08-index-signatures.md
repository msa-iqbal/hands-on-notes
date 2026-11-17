# Index Signatures

Index signatures are a TypeScript feature that allows you to define the types of properties when an object's property names are not known in advance.

Normally, an interface or type alias defines specific property names:

```typescript
interface User {
  id: number;
  name: string;
  email: string;
}
```

However, some objects contain dynamic property names, such as:

* Configuration objects.
* Dictionaries and lookup tables.
* Key-value collections.
* HTTP headers.
* Translation dictionaries.
* Dynamic API response data.
* Environment variables.

Index signatures allow you to describe these flexible object structures while maintaining type safety.

# 🗁 Basic Syntax

An index signature defines the type of keys and their corresponding values.

```typescript
interface InterfaceName {
  [key: string]: ValueType;
}
```

The syntax contains:

* `key`: A name representing the property key.
* `string`: The allowed key type.
* `ValueType`: The type of each property value.

Example:

```typescript
interface User {
  [key: string]: string;
}
```

This interface allows an object to contain any number of string-keyed properties whose values must be strings.

```typescript
const user: User = {
  name: "Alice",
  email: "alice@example.com",
  city: "Dhaka",
};
```

All properties must satisfy the index signature's value type.

# 🗁 String Index Signatures

A string index signature allows an object to have dynamically named string properties.

```typescript
interface Dictionary {
  [key: string]: string;
}
```

Example:

```typescript
const countries: Dictionary = {
  bd: "Bangladesh",
  us: "United States",
  jp: "Japan",
};
```

You can access properties using either dot notation or bracket notation.

```typescript
console.log(countries.bd);
console.log(countries["us"]);
```

Bracket notation is especially useful when the property name is stored in a variable.

```typescript
const countryCode = "bd";

console.log(countries[countryCode]);
```

# 🗁 Number Index Signatures

A number index signature allows properties to be accessed using numeric keys.

```typescript
interface NumberDictionary {
  [index: number]: string;
}
```

Example:

```typescript
const users: NumberDictionary = {
  0: "Alice",
  1: "Bob",
  2: "Charlie",
};
```

Access the values:

```typescript
console.log(users[0]);
console.log(users[1]);
```

JavaScript converts numeric object property keys to strings internally. TypeScript nevertheless distinguishes number and string index signatures in its type system.

# 🗁 String and Number Index Signatures Together

An interface can contain both string and number index signatures.

However, the numeric index signature's value type must be assignable to the string index signature's value type.

This is because JavaScript converts numeric object keys into strings.

```typescript
interface Dictionary {
  [key: string]: string | number;
  [index: number]: string;
}
```

This is valid because `string` is assignable to `string | number`.

Example:

```typescript
const dictionary: Dictionary = {
  name: "Alice",
  age: 25,
  0: "First item",
  1: "Second item",
};
```

An incompatible declaration produces an error:

```typescript
interface Dictionary {
  [key: string]: string;
  [index: number]: number;
}
```

The numeric index signature is invalid because `number` is not assignable to `string`.

# 🗁 Symbol Index Signatures

TypeScript also supports symbol index signatures.

```typescript
interface SymbolDictionary {
  [key: symbol]: string;
}
```

Example:

```typescript
const metadataKey = Symbol("metadata");

const metadata: SymbolDictionary = {
  [metadataKey]: "Application metadata",
};
```

Access the value:

```typescript
console.log(metadata[metadataKey]);
```

Symbol index signatures are useful when working with objects that use symbol-based property keys.

# 🗁 Index Signatures with Explicit Properties

An interface can combine explicitly declared properties with an index signature.

```typescript
interface User {
  id: number;
  name: string;
  [key: string]: string | number;
}
```

Example:

```typescript
const user: User = {
  id: 1,
  name: "Alice",
  age: 25,
  city: "Dhaka",
};
```

The index signature must be compatible with every explicitly declared property.

This means the value type of `id` and `name` must be assignable to `string | number`.

An incompatible declaration produces an error:

```typescript
interface User {
  id: number;
  [key: string]: string;
}
```

The `id` property is incompatible because its type is `number`, not `string`.

# 🗁 Index Signatures with Union Types

An index signature can use a union type to support multiple value types.

```typescript
interface Settings {
  [key: string]: string | number | boolean;
}
```

Example:

```typescript
const settings: Settings = {
  appName: "My Application",
  port: 3000,
  debug: true,
};
```

Every property value must match at least one type in the union.

# 🗁 Readonly Index Signatures

An index signature can be declared readonly to prevent assigning new values through that reference.

```typescript
interface ReadonlyDictionary {
  readonly [key: string]: string;
}
```

Example:

```typescript
const countries: ReadonlyDictionary = {
  bd: "Bangladesh",
  us: "United States",
};

// Error:
countries.bd = "New Value";

// Error:
countries.jp = "Japan";
```

Readonly index signatures prevent writes to indexed properties through the declared type.

They do not freeze the underlying JavaScript object at runtime.

# 🗁 Optional Index Signatures

TypeScript does not support optional index signatures directly.

The following syntax is invalid:

```typescript
interface Dictionary {
  // Invalid syntax:
  [key: string]?: string;
}
```

Index signatures already describe properties that may or may not exist.

If you need a particular property to be optional, declare it separately.

```typescript
interface User {
  id: number;
  email?: string;
  [key: string]: string | number | undefined;
}
```

The explicit index signature must accommodate the types of all declared properties.

# 🗁 Template String Pattern Index Signatures

TypeScript supports template string pattern index signatures.

They allow property names to match a particular string pattern.

```typescript
interface DataAttributes {
  [key: `data-${string}`]: string;
}
```

Example:

```typescript
const attributes: DataAttributes = {
  "data-id": "101",
  "data-name": "Alice",
  "data-role": "admin",
};
```

A property that does not match the required pattern is rejected when it is not declared separately.

```typescript
const attributes: DataAttributes = {
  "data-id": "101",

  // Error:
  title: "User profile",
};
```

Template string pattern index signatures are useful for modeling structured property names such as HTML data attributes and custom configuration keys.

# 🗁 Index Signatures with Type Aliases

Index signatures also work with type aliases.

```typescript
type Dictionary = {
  [key: string]: string;
};
```

Example:

```typescript
const translations: Dictionary = {
  hello: "Hello",
  goodbye: "Goodbye",
  welcome: "Welcome",
};
```

# 🗁 Index Signatures with Generics

Generics allow you to create reusable dictionary types.

```typescript
interface Dictionary<T> {
  [key: string]: T;
}
```

The type parameter `T` determines the value type.

String dictionary:

```typescript
const users: Dictionary<string> = {
  first: "Alice",
  second: "Bob",
};
```

Number dictionary:

```typescript
const scores: Dictionary<number> = {
  Alice: 95,
  Bob: 88,
};
```

Boolean dictionary:

```typescript
const permissions: Dictionary<boolean> = {
  create: true,
  update: false,
  delete: false,
};
```

# 🗁 Record Utility Type vs Index Signatures

TypeScript provides the `Record<K, V>` utility type for creating object types with specified key and value types.

Using an index signature:

```typescript
interface UserRoles {
  [key: string]: string;
}
```

Using `Record`:

```typescript
type UserRoles = Record<string, string>;
```

Both describe an object with string keys and string values.

Example:

```typescript
const roles: UserRoles = {
  alice: "admin",
  bob: "customer",
};
```

However, `Record` is more flexible when you need to define a fixed set of keys.

```typescript
type Role = "admin" | "customer" | "editor";

type Permissions = Record<Role, boolean>;
```

Example:

```typescript
const permissions: Permissions = {
  admin: true,
  customer: false,
  editor: true,
};
```

All three keys are required.

| Feature | Index Signature | Record |
| --- | --- | --- |
| Purpose | Describe indexed properties | Construct object types from key and value types |
| Dynamic string keys | Yes | Yes |
| Fixed key unions | Possible with mapped types | Directly supported |
| Generic support | Yes | Yes |
| Typical use | Flexible dictionaries | Fixed or dynamic key-value structures |

# 🗁 Index Signatures vs Map

Index signatures describe the structure of ordinary JavaScript objects.

The `Map` class is a separate JavaScript collection designed for key-value storage.

Object with an index signature:

```typescript
interface UserDictionary {
  [key: string]: string;
}

const users: UserDictionary = {
  first: "Alice",
  second: "Bob",
};

console.log(users["first"]);
```

Using `Map`:

```typescript
const users = new Map<string, string>();

users.set("first", "Alice");
users.set("second", "Bob");

console.log(users.get("first"));
```

| Feature | Index Signature | Map |
| --- | --- | --- |
| Data structure | Object | Map collection |
| Key types | String, number, symbol, and supported patterns | Any value |
| Key ordering | Object property ordering rules | Insertion order |
| Built-in size property | No | Yes |
| Built-in iteration methods | No dedicated collection API | Yes |
| JSON serialization | Naturally supported for enumerable string-keyed data | Requires conversion |
| Common use | Typed objects and dictionaries | Dynamic key-value collections |

Choose the structure that best fits the application requirements.

# 🗁 Practical Example: Application Configuration

Index signatures are useful for configuration objects with dynamic keys.

```typescript
interface AppConfig {
  appName: string;
  version: string;
  [key: string]: string | number | boolean;
}
```

Create a configuration:

```typescript
const config: AppConfig = {
  appName: "My Application",
  version: "1.0.0",
  port: 3000,
  debug: true,
  environment: "production",
};
```

Access configuration values:

```typescript
console.log(config.appName);
console.log(config["port"]);
console.log(config["debug"]);
```

# 🗁 Practical Example: Translation Dictionary

A translation dictionary can contain dynamic keys representing messages.

```typescript
interface Translations {
  [key: string]: string;
}
```

Create translations:

```typescript
const translations: Translations = {
  welcome: "Welcome",
  login: "Login",
  logout: "Logout",
  dashboard: "Dashboard",
  settings: "Settings",
};
```

Create a reusable translation function:

```typescript
function translate(
  dictionary: Translations,
  key: string,
): string {
  return dictionary[key] ?? key;
}
```

Usage:

```typescript
console.log(translate(translations, "welcome"));
console.log(translate(translations, "dashboard"));
console.log(translate(translations, "unknown"));
```

# 🗁 Practical Example: HTTP Headers

HTTP headers are commonly represented as string-keyed objects.

```typescript
interface HttpHeaders {
  [headerName: string]: string;
}
```

Create headers:

```typescript
const headers: HttpHeaders = {
  "Content-Type": "application/json",
  Authorization: "Bearer example-token",
  Accept: "application/json",
};
```

Access a header dynamically:

```typescript
const headerName = "Content-Type";

console.log(headers[headerName]);
```

# 🗁 Practical Example: Dynamic User Metadata

An application may allow users to store additional metadata fields.

```typescript
interface User {
  id: number;
  name: string;
  metadata: {
    [key: string]: string | number | boolean;
  };
}
```

Create a user:

```typescript
const user: User = {
  id: 1,
  name: "Alice",
  metadata: {
    department: "Engineering",
    experience: 5,
    verified: true,
    country: "Bangladesh",
  },
};
```

Access metadata:

```typescript
console.log(user.metadata["department"]);
console.log(user.metadata["experience"]);
```

# 🗁 Common Mistakes

**Using incompatible explicit properties**

```typescript
interface User {
  id: number;

  // Error: number is not assignable to string.
  [key: string]: string;
}
```

Use a compatible value type:

```typescript
interface User {
  id: number;
  [key: string]: string | number;
}
```

**Assuming every possible key exists**

```typescript
interface Dictionary {
  [key: string]: string;
}

const dictionary: Dictionary = {
  hello: "Hello",
};

const value = dictionary["missing"];
```

Even though the index signature declares `string` values, the requested key might not exist at runtime.

Enable `noUncheckedIndexedAccess` to make indexed access account for potentially missing properties.

```json
{
  "compilerOptions": {
    "strict": true,
    "noUncheckedIndexedAccess": true
  }
}
```

With this option, an indexed access such as `dictionary["missing"]` has the type `string | undefined`.

**Confusing object index signatures with arrays**

```typescript
interface User {
  [key: string]: string;
}
```

This does not define an array. It defines an object with string-keyed properties.

Use an array type when the data represents an ordered collection:

```typescript
const users: string[] = ["Alice", "Bob"];
```

**Trying to declare optional index signatures**

```typescript
interface Dictionary {
  // Invalid:
  [key: string]?: string;
}
```

Index signatures already allow keys to be absent. Declare individual optional properties separately when needed.

**Using overly broad value types**

```typescript
interface Data {
  [key: string]: any;
}
```

Using `any` disables much of TypeScript's type checking.

Prefer a specific value type or `unknown` when the structure is not yet known.

# 🗁 Best Practices

* Use index signatures when property names are genuinely dynamic.
* Prefer explicit properties for known and important object fields.
* Choose the narrowest practical value type.
* Use union types when multiple value types are expected.
* Use readonly index signatures when indexed properties should not be modified.
* Use generic index signatures for reusable dictionary structures.
* Consider `Record<K, V>` for fixed or reusable key-value object types.
* Enable `noUncheckedIndexedAccess` when missing keys should be represented explicitly.
* Use `Map` when keys can be arbitrary values or collection-specific operations are needed.
* Avoid using `any` in index signatures unless there is a specific reason.
* Document dynamic property naming conventions.
* Keep explicit properties compatible with the index signature.

# 🗁 Summary

* Index signatures describe objects with dynamically named properties.
* String index signatures support string-keyed properties.
* Number index signatures support numeric property access.
* Symbol index signatures support symbol-keyed properties.
* Multiple index signatures can coexist when their value types are compatible.
* Explicit properties must satisfy the index signature's value type.
* Readonly index signatures prevent assignment through the declared type.
* Template string pattern index signatures constrain property names.
* Generic index signatures support reusable dictionary types.
* `Record<K, V>` is an alternative for constructing key-value object types.
* `noUncheckedIndexedAccess` helps account for missing indexed properties.
* Index signatures are useful for dictionaries, configuration objects, metadata, and dynamic API structures.

# 🗁 Related Topics

* Object Types
* Type Aliases
* Interfaces
* Interface Extension
* Interface Merging
* Optional Properties
* Readonly Properties
* Generic Types
* Record Utility Type
* Mapped Types
* Keyof Type Operator
* Arrays
* Map and Set
