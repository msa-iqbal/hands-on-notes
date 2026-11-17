# Readonly Arrays

A readonly array is an array whose elements cannot be modified through a particular reference.

TypeScript provides readonly array types to prevent operations that change the array, such as:

* Adding elements.
* Removing elements.
* Replacing elements.
* Sorting elements in place.
* Reversing elements in place.

Readonly arrays are useful when you want to pass collections to functions without allowing those functions to modify the original array.

# 🗁 Declaring Readonly Arrays

TypeScript provides two common syntaxes for declaring readonly arrays.

**Using `readonly`**

```typescript
const numbers: readonly number[] = [10, 20, 30];
```

**Using `ReadonlyArray<T>`**

```typescript
const numbers: ReadonlyArray<number> = [10, 20, 30];
```

Both syntaxes represent the same readonly array type.

The generic syntax is useful when working with complex element types.

```typescript
interface User {
  id: number;
  name: string;
}

const users: ReadonlyArray<User> = [
  { id: 1, name: "Alice" },
  { id: 2, name: "Bob" },
];
```

# 🗁 Accessing Readonly Array Elements

Readonly arrays allow normal read operations.

```typescript
const fruits: readonly string[] = [
  "Apple",
  "Banana",
  "Orange",
];

console.log(fruits[0]);
console.log(fruits.length);
console.log(fruits.includes("Banana"));
```

You can also iterate over readonly arrays.

```typescript
const numbers: readonly number[] = [10, 20, 30];

for (const number of numbers) {
  console.log(number);
}
```

Readonly arrays support methods that do not modify the original array, including:

* `map()`
* `filter()`
* `reduce()`
* `find()`
* `findIndex()`
* `includes()`
* `slice()`
* `concat()`
* `forEach()`

# 🗁 Preventing Array Modification

TypeScript rejects operations that modify a readonly array.

```typescript
const numbers: readonly number[] = [10, 20, 30];

// Type errors:
numbers.push(40);
numbers.pop();
numbers.shift();
numbers.unshift(5);
numbers.splice(1, 1);
numbers.sort();
numbers.reverse();

numbers[0] = 100;
```

These operations are rejected because they would change the array.

# 🗁 Readonly Arrays with Type Aliases

You can define reusable readonly array types using type aliases.

```typescript
type ReadonlyNumbers = readonly number[];

const values: ReadonlyNumbers = [10, 20, 30];
```

For structured data:

```typescript
interface Product {
  id: number;
  name: string;
  price: number;
}

type ProductList = ReadonlyArray<Product>;

const products: ProductList = [
  {
    id: 1,
    name: "Keyboard",
    price: 50,
  },
  {
    id: 2,
    name: "Mouse",
    price: 25,
  },
];
```

# 🗁 Readonly Arrays in Function Parameters

Readonly arrays are particularly useful in function parameters.

They communicate that a function can read the collection but cannot modify it through the supplied reference.

```typescript
function calculateTotal(prices: readonly number[]): number {
  return prices.reduce((total, price) => total + price, 0);
}

const prices = [100, 200, 300];

console.log(calculateTotal(prices));
```

Output:

```text
600
```

The function can access every element, but it cannot add, remove, or replace elements.

```typescript
function processNumbers(numbers: readonly number[]): void {
  console.log(numbers.length);

  // Type error:
  numbers.push(40);

  // Type error:
  numbers[0] = 100;
}
```

# 🗁 Passing Mutable Arrays to Readonly Parameters

A normal mutable array can be passed to a function that expects a readonly array.

```typescript
function printNames(names: readonly string[]): void {
  names.forEach(name => console.log(name));
}

const users = ["Alice", "Bob", "Charlie"];

printNames(users);
```

This works because a function that only reads an array does not require permission to modify it.

The reverse assignment is not allowed:

```typescript
const readonlyNumbers: readonly number[] = [10, 20, 30];

// Type error:
const mutableNumbers: number[] = readonlyNumbers;
```

A readonly array cannot safely be treated as mutable because that would allow modifications through the mutable reference.

# 🗁 Creating Mutable Copies of Readonly Arrays

If you need to modify a readonly array's contents, create a new mutable array.

**Using spread syntax**

```typescript
const original: readonly number[] = [10, 20, 30];

const mutableCopy: number[] = [...original];

mutableCopy.push(40);

console.log(original);
console.log(mutableCopy);
```

Output:

```text
[10, 20, 30]
[10, 20, 30, 40]
```

**Using `Array.from()`**

```typescript
const original: readonly string[] = ["Apple", "Banana"];

const mutableCopy = Array.from(original);

mutableCopy.push("Orange");
```

Both approaches create a new array.

# 🗁 Readonly Arrays vs `const`

The `const` keyword and readonly array types solve different problems.

```typescript
const numbers = [10, 20, 30];

numbers.push(40); // Allowed

// Type error:
numbers = [50, 60];
```

Here:

* `const` prevents reassignment of the variable.
* It does not prevent modification of the array.
* `readonly` prevents array modification through that reference.

Compare:

```typescript
const numbers: readonly number[] = [10, 20, 30];

// Type error:
numbers.push(40);

// Type error:
numbers = [50, 60];
```

# 🗁 Readonly Arrays vs `as const`

The `as const` assertion creates a readonly tuple when applied to an array literal.

```typescript
const colors = ["red", "green", "blue"] as const;
```

The inferred type is:

```typescript
readonly ["red", "green", "blue"]
```

This is more specific than:

```typescript
const colors: readonly string[] = [
  "red",
  "green",
  "blue",
];
```

The differences:

| Feature | `readonly string[]` | `as const` |
| --- | --- | --- |
| Array modification | Prevented | Prevented |
| Length | Can vary | Fixed by the tuple type |
| Element types | `string` | Literal string types |
| Element replacement | Prevented | Prevented |
| Typical use | Readonly collections | Fixed literal collections |

# 🗁 Readonly Arrays and Object Mutability

Readonly arrays prevent modification of the array structure, but they do not automatically make nested objects readonly.

```typescript
interface User {
  name: string;
}

const users: readonly User[] = [
  { name: "Alice" },
  { name: "Bob" },
];

// Type error:
users.push({ name: "Charlie" });

// Allowed:
users[0].name = "Alex";
```

The array cannot be modified through this reference, but its object elements remain mutable.

If you also want to prevent modification of object properties, declare those properties as readonly.

```typescript
interface User {
  readonly name: string;
}

const users: readonly User[] = [
  { name: "Alice" },
  { name: "Bob" },
];

// Type errors:
users.push({ name: "Charlie" });
users[0].name = "Alex";
```

# 🗁 Readonly Arrays and Generic Functions

Generic functions can accept readonly arrays while preserving their element types.

```typescript
function getFirst<T>(items: readonly T[]): T | undefined {
  return items[0];
}

const numbers = [10, 20, 30];

const firstNumber = getFirst(numbers);

// Inferred type: number | undefined
```

This function works with mutable and readonly arrays.

```typescript
const names: readonly string[] = ["Alice", "Bob"];

const firstName = getFirst(names);

// Inferred type: string | undefined
```

Readonly array parameters make generic utility functions more flexible because they accept both mutable and readonly collections.

# 🗁 Readonly Arrays and Non-Mutating Methods

Modern JavaScript provides methods that return modified copies instead of changing the original array.

Examples include:

**`toSorted()`**

```typescript
const numbers: readonly number[] = [30, 10, 20];

const sorted = numbers.toSorted((a, b) => a - b);

console.log(sorted);
console.log(numbers);
```

**`toReversed()`**

```typescript
const numbers: readonly number[] = [10, 20, 30];

const reversed = numbers.toReversed();
```

**`toSpliced()`**

```typescript
const numbers: readonly number[] = [10, 20, 30, 40];

const updated = numbers.toSpliced(1, 2, 100);
```

These methods return new arrays and do not modify the original collection.

They require compatible JavaScript runtime support and TypeScript library definitions.

# 🗁 Readonly Is a Compile-Time Restriction

TypeScript readonly restrictions are enforced by the type checker. They do not automatically freeze an array at runtime.

```typescript
const numbers: readonly number[] = [10, 20, 30];

// TypeScript error:
// numbers.push(40);
```

However, JavaScript code or another mutable reference can still modify the same underlying array.

```typescript
const mutableNumbers = [10, 20, 30];

const readonlyNumbers: readonly number[] = mutableNumbers;

mutableNumbers.push(40);

console.log(readonlyNumbers);
```

Output:

```text
[10, 20, 30, 40]
```

Both variables refer to the same array.

If runtime immutability is required, JavaScript provides `Object.freeze()`:

```typescript
const numbers = Object.freeze([10, 20, 30]);
```

`Object.freeze()` prevents changes to the array's own properties at runtime. It is shallow, so nested objects are not automatically frozen.

# 🗁 Common Mistakes

**Assuming `const` makes an array readonly**

```typescript
const numbers = [10, 20, 30];

numbers.push(40); // Allowed
```

Use an explicit readonly type when modification should be prohibited.

**Trying to modify readonly arrays**

```typescript
const names: readonly string[] = ["Alice", "Bob"];

// Type error:
names.push("Charlie");
```

Create a mutable copy if modification is required.

**Assuming readonly arrays make nested objects immutable**

```typescript
interface User {
  name: string;
}

const users: readonly User[] = [
  { name: "Alice" },
];

users[0].name = "Bob"; // Allowed
```

Use readonly object properties when nested data must also be protected.

**Assuming readonly provides runtime immutability**

Readonly is a compile-time restriction. Use runtime mechanisms such as `Object.freeze()` when necessary.

# 🗁 Best Practices

* Use readonly arrays for function parameters when the function only reads the collection.
* Prefer `readonly T[]` for concise readonly array declarations.
* Use `ReadonlyArray<T>` when the generic form improves readability.
* Create mutable copies with spread syntax or `Array.from()` when modification is required.
* Use readonly object properties when nested objects must also be protected.
* Understand the difference between `const`, `readonly`, `as const`, and `Object.freeze()`.
* Prefer non-mutating array methods when preserving the original collection is important.
* Avoid unnecessary mutable access to shared data.

# 🗁 Summary

* Readonly arrays prevent direct modification through a readonly reference.
* TypeScript supports `readonly T[]` and `ReadonlyArray<T>`.
* Read operations such as `map()`, `filter()`, and `reduce()` remain available.
* Mutable arrays can be passed to readonly array parameters.
* Readonly arrays cannot safely be assigned to mutable array variables.
* Spread syntax and `Array.from()` can create mutable copies.
* Readonly arrays do not automatically make nested objects immutable.
* Readonly restrictions are compile-time checks, not runtime freezing.

# 🗁 Related Topics

* Arrays
* Tuples
* Optional Tuple Elements
* Rest Elements
* Named Tuples
* Object Types
* Readonly Properties
* Generics
* Type Assertions
