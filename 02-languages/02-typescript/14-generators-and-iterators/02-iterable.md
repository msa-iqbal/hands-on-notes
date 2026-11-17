# Iterable

An iterable is an object that implements the iterable protocol, allowing its elements to be accessed one at a time.

In JavaScript and TypeScript, an object is iterable when it provides a method identified by `Symbol.iterator`.

That method returns an iterator.

Iterables are used by many JavaScript features, including:

* `for...of` loops.
* Array destructuring.
* Spread syntax.
* `yield*` expressions.
* Built-in collection constructors.
* APIs that accept iterable values.

TypeScript provides the `Iterable<T>` interface to describe iterable objects.

# 🗁 What Is an Iterable?

An iterable is an object that provides a method at `Symbol.iterator`.

```typescript
const numbers = [10, 20, 30];

const iterator = numbers[Symbol.iterator]();

console.log(iterator.next());
// { value: 10, done: false }

console.log(iterator.next());
// { value: 20, done: false }
```

The `Symbol.iterator` method returns an iterator.

The returned iterator produces values through its `next()` method.

# 🗁 The Iterable Protocol

The iterable protocol defines how an object provides an iterator.

An iterable must implement a method identified by `Symbol.iterator`.

Conceptually, the protocol looks like this:

```typescript
interface Iterable<T> {
  [Symbol.iterator](): Iterator<T>;
}
```

The method must return an iterator capable of producing values of type `T`.

For example:

```typescript
const numbers: Iterable<number> = [10, 20, 30];

for (const number of numbers) {
  console.log(number);
}
```

Arrays already implement the iterable protocol, so they can be assigned to `Iterable<number>`.

# 🗁 Iterable vs Iterator

An iterable and an iterator serve different purposes.

**Iterable**

An object that provides an iterator through `Symbol.iterator`.

```typescript
const numbers = [10, 20, 30];

const iterator = numbers[Symbol.iterator]();
```

**Iterator**

An object that produces values through `next()`.

```typescript
console.log(iterator.next());
console.log(iterator.next());
```

The distinction:

| Iterable | Iterator |
| --- | --- |
| Implements `Symbol.iterator` | Implements `next()` |
| Provides an iterator | Produces values |
| Can be used with `for...of` | Can be consumed manually |
| May create a new iterator each time | Maintains iteration state |

An object can implement both protocols.

# 🗁 Built-in Iterable Objects

JavaScript provides several built-in iterable objects.

**Arrays**

```typescript
const fruits = ["Apple", "Banana", "Orange"];

for (const fruit of fruits) {
  console.log(fruit);
}
```

**Strings**

```typescript
const message = "Hello";

for (const character of message) {
  console.log(character);
}
```

String iteration processes Unicode code points rather than individual UTF-16 code units.

**Maps**

```typescript
const users = new Map([
  [1, "Alex"],
  [2, "John"],
]);

for (const [id, name] of users) {
  console.log(id, name);
}
```

**Sets**

```typescript
const ids = new Set([10, 20, 30]);

for (const id of ids) {
  console.log(id);
}
```

These built-in objects implement the iterable protocol.

# 🗁 Creating a Custom Iterable

You can create your own iterable object by implementing `Symbol.iterator`.

```typescript
class NumberCollection implements Iterable<number> {
  private numbers = [10, 20, 30];

  [Symbol.iterator](): Iterator<number> {
    let index = 0;

    const numbers = this.numbers;

    return {
      next(): IteratorResult<number> {
        if (index < numbers.length) {
          return {
            value: numbers[index++],
            done: false,
          };
        }

        return {
          value: undefined,
          done: true,
        };
      },
    };
  }
}
```

Using the custom iterable:

```typescript
const collection = new NumberCollection();

for (const number of collection) {
  console.log(number);
}
```

Output:

```text
10
20
30
```

The class implements `Iterable<number>` by providing the required `Symbol.iterator` method.

# 🗁 Creating a Reusable Iterable

An important characteristic of many iterables is that they can create a new iterator each time.

```typescript
class NumberCollection implements Iterable<number> {
  constructor(private numbers: number[]) {}

  [Symbol.iterator](): Iterator<number> {
    let index = 0;

    return {
      next: (): IteratorResult<number> => {
        if (index < this.numbers.length) {
          return {
            value: this.numbers[index++],
            done: false,
          };
        }

        return {
          value: undefined,
          done: true,
        };
      },
    };
  }
}
```

Each call to `Symbol.iterator` creates a new iterator with its own index.

```typescript
const collection = new NumberCollection([10, 20, 30]);

const firstIterator = collection[Symbol.iterator]();

const secondIterator = collection[Symbol.iterator]();

console.log(firstIterator.next().value);
// 10

console.log(firstIterator.next().value);
// 20

console.log(secondIterator.next().value);
// 10
```

The second iterator starts from the beginning because it maintains independent iteration state.

# 🗁 Using Iterable with for...of

The `for...of` loop automatically obtains an iterator from an iterable.

```typescript
const numbers: Iterable<number> = [10, 20, 30];

for (const number of numbers) {
  console.log(number);
}
```

Conceptually, JavaScript performs these operations:

* Calls `numbers[Symbol.iterator]()`.
* Calls the returned iterator's `next()` method.
* Processes each yielded value.
* Stops when the iterator reports `done: true`.

You normally do not need to implement this process manually.

# 🗁 Using Iterable with Spread Syntax

The spread operator can expand iterable values into individual elements.

```typescript
const numbers: Iterable<number> = [10, 20, 30];

const copiedNumbers = [...numbers];

console.log(copiedNumbers);
```

Output:

```text
[10, 20, 30]
```

Spread syntax works with any object that implements the iterable protocol.

# 🗁 Using Iterable with Array Destructuring

Iterable values can be destructured into variables.

```typescript
const numbers: Iterable<number> = [10, 20, 30];

const [first, second, third] = numbers;

console.log(first);
console.log(second);
console.log(third);
```

Output:

```text
10
20
30
```

Destructuring consumes values from the iterable's iterator.

# 🗁 Using Iterable with Collection Constructors

Several JavaScript collection constructors accept iterable values.

**Set**

```typescript
const numbers: Iterable<number> = [10, 20, 20, 30];

const uniqueNumbers = new Set(numbers);

console.log([...uniqueNumbers]);
```

Output:

```text
[10, 20, 30]
```

**Map**

```typescript
const entries: Iterable<[number, string]> = [
  [1, "Alex"],
  [2, "John"],
];

const users = new Map(entries);

console.log(users.get(1));
```

Output:

```text
Alex
```

The iterable provides the values used to initialize the collection.

# 🗁 Iterable with Function Parameters

You can accept iterable values as function parameters.

This makes functions more flexible because callers are not restricted to arrays.

```typescript
function calculateTotal(numbers: Iterable<number>): number {
  let total = 0;

  for (const number of numbers) {
    total += number;
  }

  return total;
}
```

The function accepts different iterable types:

```typescript
calculateTotal([10, 20, 30]);

calculateTotal(new Set([10, 20, 30]));

calculateTotal(new Map([[1, 10], [2, 20]]).values());
```

Each input provides numbers through the iterable protocol.

# 🗁 Iterable and ReadonlyArray

`Iterable<T>` and `ReadonlyArray<T>` describe different capabilities.

`ReadonlyArray<T>` represents an array whose elements cannot be modified through that reference.

```typescript
function printNumbers(numbers: ReadonlyArray<number>): void {
  for (const number of numbers) {
    console.log(number);
  }
}
```

`Iterable<T>` describes any object that can provide an iterator.

```typescript
function printNumbers(numbers: Iterable<number>): void {
  for (const number of numbers) {
    console.log(number);
  }
}
```

The difference:

| `Iterable<T>` | `ReadonlyArray<T>` |
| --- | --- |
| Describes iterable behavior | Describes a readonly array |
| Accepts arrays, sets, maps' values, and custom iterables | Accepts readonly arrays and compatible array values |
| Does not guarantee indexing | Supports indexed access |
| Does not guarantee a `length` property | Provides a readonly `length` property |
| Does not guarantee array methods | Provides readonly array methods |

Use `Iterable<T>` when a function only needs to consume values.

Use `ReadonlyArray<T>` when the function needs array-specific operations.

# 🗁 Iterable with AsyncIterable

JavaScript also provides the asynchronous iterable protocol.

An asynchronous iterable implements `Symbol.asyncIterator` instead of `Symbol.iterator`.

```typescript
interface AsyncIterable<T> {
  [Symbol.asyncIterator](): AsyncIterator<T>;
}
```

Asynchronous iterables are consumed with `for await...of`.

```typescript
async function processItems(
  items: AsyncIterable<number>
): Promise<void> {
  for await (const item of items) {
    console.log(item);
  }
}
```

This is useful for asynchronous data sources such as streams, paginated APIs, and database cursors.

# 🗁 Iterable Type Inference

TypeScript can infer iterable types from their values.

```typescript
const numbers = [10, 20, 30];

const iterable: Iterable<number> = numbers;
```

You can also explicitly annotate custom iterable classes:

```typescript
class StringCollection implements Iterable<string> {
  private values = ["TypeScript", "JavaScript"];

  [Symbol.iterator](): Iterator<string> {
    return this.values[Symbol.iterator]();
  }
}
```

The implementation reuses the array's built-in iterator.

# 🗁 Compiler Configuration

TypeScript's iterable type declarations are provided through standard library definitions.

For projects using modern JavaScript features, a suitable configuration might be:

```json
{
  "compilerOptions": {
    "target": "ES2022",
    "lib": ["ES2022", "DOM"],
    "strict": true
  }
}
```

The selected `lib` determines which JavaScript API declarations TypeScript recognizes.

Compiler library declarations do not automatically add runtime support. The JavaScript environment must support the features used by the application.

# 🗁 Common Iterable Errors

**Type is not iterable**

```typescript
const user = {
  id: 101,
  name: "Alex",
};

// Error: The object does not implement the iterable protocol.
for (const value of user) {
  console.log(value);
}
```

An ordinary object is not automatically iterable.

Use an iterable structure:

```typescript
const values = Object.values(user);

for (const value of values) {
  console.log(value);
}
```

**Missing Symbol.iterator**

```typescript
class NumberCollection {
  values = [10, 20, 30];
}

const collection = new NumberCollection();

// Error: NumberCollection is not iterable.
for (const value of collection) {
  console.log(value);
}
```

Implement the iterable protocol to fix this:

```typescript
class NumberCollection implements Iterable<number> {
  values = [10, 20, 30];

  [Symbol.iterator](): Iterator<number> {
    return this.values[Symbol.iterator]();
  }
}
```

**Using an iterator as an iterable**

```typescript
const iterator = [10, 20, 30].values();

// An iterator is not necessarily iterable.
```

Modern built-in iterators are also iterable, but a custom object implementing only `next()` does not automatically satisfy `Iterable<T>`.

If an object needs to support both protocols, implement `Symbol.iterator` as well.

# 🗁 Best Practices

* Use `Iterable<T>` when a function only needs to consume a sequence of values.
* Prefer built-in iterable objects when they already satisfy your requirements.
* Implement `Symbol.iterator` when creating custom synchronous iterable objects.
* Keep iterator state independent when the iterable should support repeated iteration.
* Use `ReadonlyArray<T>` when array-specific functionality is required.
* Use `AsyncIterable<T>` for asynchronous sequences.
* Remember that type declarations do not create runtime iterable behavior.
* Avoid converting iterables into arrays unless array-specific operations are necessary.

# 🗁 Summary

An iterable is an object that implements the iterable protocol through `Symbol.iterator`.

Key concepts:

* `Iterable<T>` describes iterable objects.
* `Symbol.iterator` returns an iterator.
* Iterators produce values through `next()`.
* Arrays, strings, maps, and sets are built-in iterables.
* Custom classes can implement `Iterable<T>`.
* `for...of`, spread syntax, and destructuring consume iterable values.
* `Iterable<T>` is more general than `ReadonlyArray<T>`.
* `AsyncIterable<T>` supports asynchronous iteration.

Understanding iterables is essential for creating reusable collection APIs and working with JavaScript's iteration mechanisms.
