# Arrays

An array is a data structure used to store multiple values in a single variable.

In TypeScript, arrays are strongly typed. You can specify the type of elements an array can contain, allowing TypeScript to detect invalid values during development.

Arrays are commonly used to store:

* Lists of strings.
* Collections of numbers.
* User records.
* API response data.
* Objects and other structured values.

# 🗁 Declaring Arrays

TypeScript provides two common syntaxes for declaring arrays.

**Using the array type syntax**

```typescript
let names: string[] = ["Alice", "Bob", "Charlie"];

let ages: number[] = [20, 25, 30];

let isActive: boolean[] = [true, false, true];
```

The syntax is:

```typescript
let variableName: ElementType[];
```

**Using the generic `Array` type**

```typescript
let names: Array<string> = ["Alice", "Bob", "Charlie"];

let ages: Array<number> = [20, 25, 30];

let isActive: Array<boolean> = [true, false, true];
```

Both syntaxes represent the same array types.

# 🗁 Type Inference in Arrays

TypeScript can automatically infer the element type from the initial values.

```typescript
let numbers = [10, 20, 30];

let fruits = ["Apple", "Banana", "Orange"];

let activeStates = [true, false, true];
```

Inferred types:

```typescript
// number[]
// string[]
// boolean[]
```

TypeScript uses these inferred types to check future assignments.

```typescript
let numbers = [10, 20, 30];

numbers.push(40);      // Valid
numbers.push("Hello"); // Type error
```

# 🗁 Empty Arrays

When declaring an empty array, explicitly specify its element type.

```typescript
let names: string[] = [];

let numbers: number[] = [];

let users: User[] = [];
```

This ensures TypeScript knows which values can be added later.

```typescript
const names: string[] = [];

names.push("Alice");
names.push("Bob");
```

# 🗁 Arrays of Objects

You can create arrays containing objects that follow a specific structure.

```typescript
interface User {
  id: number;
  name: string;
  email: string;
}

const users: User[] = [
  {
    id: 1,
    name: "Alice",
    email: "alice@example.com",
  },
  {
    id: 2,
    name: "Bob",
    email: "bob@example.com",
  },
];
```

TypeScript checks every object against the `User` interface.

Invalid properties or missing required properties produce type errors.

# 🗁 Accessing Array Elements

Array elements are accessed using zero-based indexes.

```typescript
const fruits: string[] = [
  "Apple",
  "Banana",
  "Orange",
];

console.log(fruits[0]); // Apple
console.log(fruits[1]); // Banana
console.log(fruits[2]); // Orange
```

The first element has index `0`.

With `noUncheckedIndexedAccess` enabled, TypeScript includes `undefined` in the type of indexed access because the requested index might not exist.

```typescript
const fruits: string[] = ["Apple", "Banana"];

const fruit = fruits[10];

// With noUncheckedIndexedAccess:
// string | undefined
```

# 🗁 Updating Array Elements

You can update an element by assigning a new value at an existing index.

```typescript
let fruits: string[] = ["Apple", "Banana"];

fruits[1] = "Orange";

console.log(fruits);
```

Output:

```text
["Apple", "Orange"]
```

The assigned value must match the array's element type.

# 🗁 Array Length

The `length` property returns the number of elements in an array.

```typescript
const numbers: number[] = [10, 20, 30, 40];

console.log(numbers.length);
```

Output:

```text
4
```

You can also modify an array's length.

```typescript
const numbers = [10, 20, 30, 40];

numbers.length = 2;

console.log(numbers);
```

Output:

```text
[10, 20]
```

Reducing the length removes elements from the end of the array.

# 🗁 Adding and Removing Elements

JavaScript provides several array methods for modifying arrays.

**`push()`**

Adds one or more elements to the end.

```typescript
const numbers: number[] = [10, 20];

numbers.push(30);

console.log(numbers);
```

Output:

```text
[10, 20, 30]
```

**`pop()`**

Removes and returns the last element.

```typescript
const numbers: number[] = [10, 20, 30];

const removed = numbers.pop();

console.log(removed); // 30
console.log(numbers); // [10, 20]
```

**`unshift()`**

Adds elements to the beginning.

```typescript
const numbers: number[] = [20, 30];

numbers.unshift(10);

console.log(numbers);
```

Output:

```text
[10, 20, 30]
```

**`shift()`**

Removes and returns the first element.

```typescript
const numbers: number[] = [10, 20, 30];

const removed = numbers.shift();

console.log(removed); // 10
console.log(numbers); // [20, 30]
```

# 🗁 Searching Arrays

You can search for elements using built-in array methods.

**`includes()`**

Checks whether an array contains a value.

```typescript
const fruits: string[] = ["Apple", "Banana", "Orange"];

console.log(fruits.includes("Banana")); // true
console.log(fruits.includes("Mango"));  // false
```

**`indexOf()`**

Returns the first matching element's index or `-1` if it does not exist.

```typescript
const numbers: number[] = [10, 20, 30, 20];

console.log(numbers.indexOf(20)); // 1
console.log(numbers.indexOf(50)); // -1
```

**`find()`**

Returns the first element matching a condition.

```typescript
const numbers: number[] = [10, 20, 30, 40];

const result = numbers.find(number => number > 20);

console.log(result);
```

Output:

```text
30
```

The return type is `number | undefined` because no matching element might exist.

**`findIndex()`**

Returns the index of the first matching element or `-1`.

```typescript
const numbers: number[] = [10, 20, 30, 40];

const index = numbers.findIndex(number => number > 20);

console.log(index);
```

Output:

```text
2
```

# 🗁 Iterating Over Arrays

You can iterate through arrays using different JavaScript constructs.

**Using `for...of`**

```typescript
const fruits: string[] = ["Apple", "Banana", "Orange"];

for (const fruit of fruits) {
  console.log(fruit);
}
```

**Using `forEach()`**

```typescript
const numbers: number[] = [10, 20, 30];

numbers.forEach((number, index) => {
  console.log(index, number);
});
```

**Using a traditional `for` loop**

```typescript
const numbers: number[] = [10, 20, 30];

for (let index = 0; index < numbers.length; index++) {
  console.log(numbers[index]);
}
```

# 🗁 Transforming Arrays

Array transformation methods create new arrays or calculate new values.

**`map()`**

Transforms every element and returns a new array.

```typescript
const numbers: number[] = [1, 2, 3, 4];

const doubled = numbers.map(number => number * 2);

console.log(doubled);
```

Output:

```text
[2, 4, 6, 8]
```

**`filter()`**

Returns a new array containing elements that satisfy a condition.

```typescript
const numbers: number[] = [10, 15, 20, 25, 30];

const evenNumbers = numbers.filter(number => number % 2 === 0);

console.log(evenNumbers);
```

Output:

```text
[10, 20, 30]
```

**`reduce()`**

Combines array elements into a single accumulated value.

```typescript
const numbers: number[] = [10, 20, 30, 40];

const total = numbers.reduce(
  (sum, number) => sum + number,
  0
);

console.log(total);
```

Output:

```text
100
```

# 🗁 Sorting Arrays

The `sort()` method sorts array elements in place.

For numbers, provide a comparison function to ensure numeric sorting.

```typescript
const numbers: number[] = [40, 10, 30, 20];

numbers.sort((a, b) => a - b);

console.log(numbers);
```

Output:

```text
[10, 20, 30, 40]
```

Descending order:

```typescript
numbers.sort((a, b) => b - a);
```

For strings:

```typescript
const fruits: string[] = ["Orange", "Apple", "Banana"];

fruits.sort();

console.log(fruits);
```

`sort()` mutates the original array. Use `toSorted()` when you need a sorted copy without modifying the original array.

```typescript
const numbers = [40, 10, 30, 20];

const sortedNumbers = numbers.toSorted((a, b) => a - b);

console.log(sortedNumbers);
console.log(numbers);
```

`toSorted()` requires a JavaScript runtime and TypeScript library definitions that support ES2023.

# 🗁 Combining Arrays

You can combine arrays using the spread operator or `concat()`.

**Using spread syntax**

```typescript
const first: number[] = [10, 20];
const second: number[] = [30, 40];

const combined = [...first, ...second];

console.log(combined);
```

Output:

```text
[10, 20, 30, 40]
```

**Using `concat()`**

```typescript
const first: number[] = [10, 20];
const second: number[] = [30, 40];

const combined = first.concat(second);
```

Both approaches create a new array.

# 🗁 Slicing and Splicing

**`slice()`**

Returns a shallow copy of a portion of an array without modifying the original.

```typescript
const numbers: number[] = [10, 20, 30, 40, 50];

const result = numbers.slice(1, 4);

console.log(result);
```

Output:

```text
[20, 30, 40]
```

The ending index is excluded.

**`splice()`**

Adds, removes, or replaces elements in the original array.

```typescript
const numbers: number[] = [10, 20, 30, 40];

numbers.splice(1, 2);

console.log(numbers);
```

Output:

```text
[10, 40]
```

You can also insert elements:

```typescript
const numbers: number[] = [10, 40];

numbers.splice(1, 0, 20, 30);

console.log(numbers);
```

Output:

```text
[10, 20, 30, 40]
```

# 🗁 Multidimensional Arrays

An array can contain other arrays.

A two-dimensional array is commonly used to represent rows and columns.

```typescript
const matrix: number[][] = [
  [1, 2, 3],
  [4, 5, 6],
  [7, 8, 9],
];

console.log(matrix[0][0]); // 1
console.log(matrix[1][2]); // 6
console.log(matrix[2][1]); // 8
```

You can also declare arrays with more dimensions:

```typescript
const cube: number[][][] = [
  [
    [1, 2],
    [3, 4],
  ],
];
```

# 🗁 Union Type Arrays

An array can contain multiple types using a union type.

```typescript
let values: (string | number)[] = [
  "Alice",
  25,
  "Bob",
  30,
];

values.push("Charlie");
values.push(35);

// Type error:
values.push(true);
```

Parentheses are important:

```typescript
let values: (string | number)[];
```

This means an array whose elements can be strings or numbers.

By contrast:

```typescript
let values: string | number[];
```

This means either a string or an array of numbers.

# 🗁 Readonly Array Preview

TypeScript supports readonly arrays that prevent direct modification through a particular reference.

```typescript
const numbers: readonly number[] = [10, 20, 30];

// Type errors:
numbers.push(40);
numbers[0] = 100;
```

Another syntax is:

```typescript
const numbers: ReadonlyArray<number> = [10, 20, 30];
```

Readonly arrays are useful when a function should receive data without modifying it.

This topic is covered in detail in the next file.

# 🗁 TypeScript Array Type Syntax Comparison

| Syntax | Description |
| --- | --- |
| `string[]` | Array of strings |
| `number[]` | Array of numbers |
| `boolean[]` | Array of booleans |
| `User[]` | Array of `User` objects |
| `(string \| number)[]` | Array of strings or numbers |
| `Array<string>` | Generic array of strings |
| `Array<User>` | Generic array of `User` objects |
| `readonly string[]` | Readonly array of strings |
| `ReadonlyArray<number>` | Readonly array of numbers |
| `number[][]` | Two-dimensional number array |

# 🗁 Common Mistakes

**Assigning an incompatible element**

```typescript
const numbers: number[] = [10, 20, 30];

// Type error:
numbers.push("Hello");
```

Use values that match the declared element type.

**Forgetting parentheses around union types**

```typescript
// Array elements can be strings or numbers.
const values: (string | number)[] = ["Alice", 25];
```

**Assuming `find()` always returns an element**

```typescript
const numbers = [10, 20, 30];

const result = numbers.find(number => number > 100);

// result may be undefined.
```

Always handle the possibility of `undefined`.

**Sorting numbers without a comparison function**

```typescript
const numbers = [100, 20, 3];

numbers.sort();

console.log(numbers);
```

Default sorting converts elements to strings and compares them lexicographically.

Use:

```typescript
numbers.sort((a, b) => a - b);
```

**Confusing `slice()` and `splice()`**

* `slice()` returns a portion without changing the original array.
* `splice()` modifies the original array.

# 🗁 Best Practices

* Explicitly type empty arrays.
* Use `string[]`, `number[]`, or another appropriate element type.
* Use interfaces or type aliases for arrays of structured objects.
* Prefer `map()` for transformations and `filter()` for selection.
* Handle `undefined` when using methods such as `find()`.
* Use `toSorted()` when you need immutable sorting.
* Use readonly arrays when consumers should not modify data.
* Avoid using `any[]` unless dynamic, unchecked values are genuinely required.
* Use meaningful variable names that describe the collection.

# 🗁 Summary

* Arrays store multiple values in a single variable.
* TypeScript supports `ElementType[]` and `Array<ElementType>` syntax.
* Type inference can determine array element types automatically.
* Arrays can contain primitive values, objects, union types, or nested arrays.
* Built-in methods support adding, removing, searching, transforming, and sorting elements.
* TypeScript checks array operations against the declared element type.
* Readonly arrays prevent direct modification through readonly references.

# 🗁 Related Topics

* Basic Types
* Object Types
* Union Types
* Type Aliases
* Interfaces
* Readonly Arrays
* Tuples
* Optional Tuple Elements
* Rest Elements
* Named Tuples
