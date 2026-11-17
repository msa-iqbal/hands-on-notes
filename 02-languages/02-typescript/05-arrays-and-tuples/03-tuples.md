# Tuples

A tuple is a TypeScript array type that represents a fixed sequence of elements where each position can have a specific type.

Unlike regular arrays, which generally allow any number of elements of the same element type, tuples describe the expected type of each position.

Tuples are useful when working with:

* Coordinates.
* RGB color values.
* Key-value pairs.
* Function return values.
* Structured records with a fixed number of elements.
* Data that follows a predictable positional structure.

# 🗁 Declaring Tuples

Use square brackets with the types of each element in order.

```typescript
let user: [number, string, boolean];

user = [1, "Alice", true];
```

The tuple describes:

* Position `0`: `number`
* Position `1`: `string`
* Position `2`: `boolean`

Each position must contain a value of its declared type.

# 🗁 Tuple Initialization

You can initialize a tuple when declaring it.

```typescript
const user: [number, string, boolean] = [
  1,
  "Alice",
  true,
];
```

TypeScript checks the element types and positions.

Incorrect assignments produce errors:

```typescript
let user: [number, string, boolean];

// Type error: incorrect element types.
user = ["Alice", 1, true];

// Type error: missing an element.
user = [1, "Alice"];

// Type error: too many elements.
user = [1, "Alice", true, "Admin"];
```

# 🗁 Accessing Tuple Elements

Tuple elements are accessed using their indexes.

```typescript
const user: [number, string, boolean] = [
  1,
  "Alice",
  true,
];

console.log(user[0]); // 1
console.log(user[1]); // Alice
console.log(user[2]); // true
```

TypeScript knows the type of each position.

```typescript
const user: [number, string, boolean] = [
  1,
  "Alice",
  true,
];

const id = user[0];      // number
const name = user[1];    // string
const active = user[2];  // boolean
```

# 🗁 Updating Tuple Elements

Tuple elements can be updated if the new value matches the type of that position.

```typescript
let user: [number, string, boolean] = [
  1,
  "Alice",
  true,
];

user[1] = "Bob";
user[2] = false;

console.log(user);
```

Output:

```text
[1, "Bob", false]
```

Assigning an incompatible value produces a type error.

```typescript
let user: [number, string, boolean] = [
  1,
  "Alice",
  true,
];

// Type error:
user[0] = "Bob";
```

# 🗁 Tuple Length

A regular tuple has a known number of elements.

```typescript
const coordinates: [number, number] = [10, 20];

console.log(coordinates.length); // 2
```

A tuple with three required elements has a length of three.

```typescript
const rgb: [number, number, number] = [255, 100, 50];
```

However, TypeScript's tuple type checking has an important distinction: tuples are array types, so methods such as `push()` and `pop()` are still available in some cases.

```typescript
const coordinates: [number, number] = [10, 20];

coordinates.push(30);

console.log(coordinates);
```

The `push()` operation is accepted by TypeScript, even though it makes the runtime array longer than the declared tuple shape.

This is one reason to use readonly tuples when the positional structure must remain protected.

# 🗁 Tuple with Union Types

A tuple can contain union types at specific positions.

```typescript
let user: [number, string | number, boolean];

user = [1, "Alice", true];

user = [2, 25, false];
```

The second position accepts either a string or a number.

Each position still follows its declared type.

# 🗁 Named Tuples

Named tuples allow you to assign meaningful labels to tuple elements.

```typescript
type User = [
  id: number,
  name: string,
  active: boolean
];

const user: User = [1, "Alice", true];
```

The labels improve readability without changing the underlying tuple structure.

You can access elements using indexes as usual.

```typescript
console.log(user[0]);
console.log(user[1]);
console.log(user[2]);
```

Named tuple elements are particularly useful when returning multiple values from a function.

# 🗁 Tuple as a Function Return Type

Tuples are useful when a function needs to return multiple values.

```typescript
function getUser(): [number, string, boolean] {
  return [1, "Alice", true];
}

const user = getUser();

console.log(user[0]);
console.log(user[1]);
console.log(user[2]);
```

You can use named tuples to make the returned values clearer.

```typescript
type UserResult = [
  id: number,
  name: string,
  active: boolean
];

function getUser(): UserResult {
  return [1, "Alice", true];
}

const [id, name, active] = getUser();
```

# 🗁 Tuple Destructuring

You can extract tuple elements into separate variables using array destructuring.

```typescript
const user: [number, string, boolean] = [
  1,
  "Alice",
  true,
];

const [id, name, active] = user;

console.log(id);
console.log(name);
console.log(active);
```

TypeScript preserves the types of the individual positions.

```typescript
const coordinates: [number, number] = [10, 20];

const [x, y] = coordinates;

// x: number
// y: number
```

You can also skip elements:

```typescript
const user: [number, string, boolean] = [
  1,
  "Alice",
  true,
];

const [, name] = user;

console.log(name);
```

# 🗁 Tuple Rest Elements

Tuples can contain rest elements that represent a variable number of elements.

```typescript
type StringNumberTuple = [string, ...number[]];

const values: StringNumberTuple = [
  "Scores",
  10,
  20,
  30,
];
```

The first element must be a string, followed by zero or more numbers.

Tuple rest elements are covered in detail in the dedicated Rest Elements topic.

# 🗁 Readonly Tuples

Use the `readonly` modifier to prevent modification of tuple elements.

```typescript
const coordinates: readonly [number, number] = [10, 20];

// Type errors:
coordinates[0] = 100;
coordinates.push(30);
```

You can also use `as const`:

```typescript
const colors = ["red", "green", "blue"] as const;
```

The inferred type is:

```typescript
readonly ["red", "green", "blue"]
```

Readonly tuples are useful for fixed configuration values and immutable data.

# 🗁 Tuple Type Inference

TypeScript can infer tuple types in certain contexts, but ordinary array literals are generally inferred as arrays.

```typescript
const user = [1, "Alice"];

// Inferred as (string | number)[]
```

If you need a tuple, provide an explicit type:

```typescript
const user: [number, string] = [1, "Alice"];
```

Or use a const assertion:

```typescript
const user = [1, "Alice"] as const;

// Inferred as readonly [1, "Alice"]
```

The const assertion preserves literal types and makes the tuple readonly.

# 🗁 Tuples vs Arrays

| Feature | Array | Tuple |
| --- | --- | --- |
| Element types | Usually one element type | Can differ by position |
| Length | Generally variable | Known from its type |
| Position-specific types | No | Yes |
| Descriptive labels | No | Named tuples support labels |
| Typical use | Collections of similar values | Fixed positional structures |
| Example | `number[]` | `[number, string]` |

# 🗁 Practical Example: Coordinates

Tuples are useful for representing coordinates.

```typescript
type Point = [number, number];

const location: Point = [23.8103, 90.4125];

const [latitude, longitude] = location;

console.log(`Latitude: ${latitude}`);
console.log(`Longitude: ${longitude}`);
```

# 🗁 Practical Example: API Response

A tuple can represent a simple status and message pair.

```typescript
type ApiResult = [
  status: number,
  message: string
];

function getApiResult(): ApiResult {
  return [200, "Request successful"];
}

const [status, message] = getApiResult();

console.log(status);
console.log(message);
```

For complex API responses, interfaces or object types are generally more descriptive than positional tuples.

# 🗁 Practical Example: RGB Colors

```typescript
type RGB = [red: number, green: number, blue: number];

const color: RGB = [255, 100, 50];

const [red, green, blue] = color;

console.log(`RGB(${red}, ${green}, ${blue})`);
```

# 🗁 Common Mistakes

**Using an array when a tuple is required**

```typescript
const user: [number, string] = [1, "Alice"];
```

A regular array type does not enforce the same positional structure.

**Using the wrong element order**

```typescript
const user: [number, string] = ["Alice", 1];
```

The first element must be a number and the second must be a string.

**Assuming tuples are automatically immutable**

```typescript
const user: [number, string] = [1, "Alice"];

user[0] = 2; // Allowed
```

Use readonly tuples when modification must be prevented.

**Assuming tuple methods cannot change the array length**

Methods such as `push()` can modify a tuple at runtime and may be accepted by TypeScript. Use readonly tuples when preserving the declared structure is important.

**Using tuples for complex records**

Tuples are position-dependent. For complex data, interfaces and object types often provide better readability.

# 🗁 Best Practices

* Use tuples when element positions have specific meanings and types.
* Use named tuples to improve readability.
* Prefer interfaces for complex structured records.
* Use readonly tuples for fixed and immutable values.
* Use explicit tuple annotations when TypeScript would otherwise infer an array.
* Use destructuring when working with tuple return values.
* Avoid unnecessarily long tuples because positional data becomes harder to understand.
* Document the meaning of tuple elements when their purpose is not obvious.

# 🗁 Summary

* Tuples represent fixed positional structures with specific element types.
* Each tuple position has its own declared type.
* Tuple elements can be accessed and updated using indexes.
* Named tuples improve readability.
* Tuple destructuring extracts individual elements.
* Rest elements support variable-length tuple structures.
* Readonly tuples prevent direct modification.
* Tuples are useful for coordinates, RGB values, and multiple function return values.

# 🗁 Related Topics

* Arrays
* Readonly Arrays
* Optional Tuple Elements
* Rest Elements
* Named Tuples
* Union Types
* Function Return Types
* Type Aliases
* Readonly Properties
