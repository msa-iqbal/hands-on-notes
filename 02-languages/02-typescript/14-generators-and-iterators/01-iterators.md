# Iterators

An iterator is an object that provides a standard way to access elements from a sequence one at a time.

JavaScript and TypeScript use the iterator protocol to make different data structures iterable through a consistent interface.

Iterators are used by:

* Arrays.
* Strings.
* Maps.
* Sets.
* Generators.
* Custom iterable objects.
* Many JavaScript built-in APIs.

TypeScript provides built-in types such as `Iterator`, `Iterable`, and `IteratorResult` to describe iterator behavior.

# 🗁 What Is an Iterator?

An iterator is an object that implements a `next()` method.

Each call to `next()` returns an object containing:

* `value`: The current value produced by the iterator.
* `done`: A boolean indicating whether iteration has finished.

Example:

```typescript
const iterator = [10, 20, 30].values();

console.log(iterator.next());
// { value: 10, done: false }

console.log(iterator.next());
// { value: 20, done: false }

console.log(iterator.next());
// { value: 30, done: false }

console.log(iterator.next());
// { value: undefined, done: true }
```

The iterator produces one value at a time.

Once iteration is complete, `done` becomes `true`.

# 🗁 The Iterator Protocol

The iterator protocol defines how an object produces a sequence of values.

An iterator must provide a `next()` method that returns an `IteratorResult`.

Conceptually, the structure looks like this:

```typescript
interface IteratorResult<T, TReturn = any> {
  done?: boolean;
  value: T | TReturn;
}

interface Iterator<T, TReturn = any, TNext = any> {
  next(...args: [] | [TNext]): IteratorResult<T, TReturn>;
}
```

These interfaces describe the general iterator contract.

The exact standard library declarations include additional details and overloads.

# 🗁 Creating a Custom Iterator

You can create an iterator manually by implementing the `next()` method.

```typescript
const numbers: Iterator<number> = {
  current: 1,

  next() {
    if (this.current <= 3) {
      return {
        value: this.current++,
        done: false,
      };
    }

    return {
      value: undefined,
      done: true,
    };
  },
} as Iterator<number> & { current: number };
```

Using the iterator:

```typescript
console.log(numbers.next());
console.log(numbers.next());
console.log(numbers.next());
console.log(numbers.next());
```

A cleaner approach is to define a dedicated iterator class.

```typescript
class NumberIterator implements Iterator<number> {
  private current = 1;

  next(): IteratorResult<number> {
    if (this.current <= 3) {
      return {
        value: this.current++,
        done: false,
      };
    }

    return {
      value: undefined,
      done: true,
    };
  }
}
```

Usage:

```typescript
const iterator = new NumberIterator();

console.log(iterator.next());
console.log(iterator.next());
console.log(iterator.next());
console.log(iterator.next());
```

The class implements the iterator contract by providing a compatible `next()` method.

# 🗁 Understanding IteratorResult

`IteratorResult` represents the result of calling an iterator's `next()` method.

It is a discriminated union in TypeScript's standard library.

It consists of two possible result types:

**IteratorYieldResult**

Represents a value produced by the iterator.

```typescript
interface IteratorYieldResult<TYield> {
  done?: false;
  value: TYield;
}
```

**IteratorReturnResult**

Represents the completion of iteration.

```typescript
interface IteratorReturnResult<TReturn> {
  done: true;
  value: TReturn;
}
```

The complete result type is conceptually:

```typescript
type IteratorResult<T, TReturn = any> =
  | IteratorYieldResult<T>
  | IteratorReturnResult<TReturn>;
```

The distinction allows TypeScript to narrow the result based on `done`.

```typescript
function readIterator(
  iterator: Iterator<number, string>
): void {
  const result = iterator.next();

  if (result.done) {
    console.log("Finished:", result.value);
  } else {
    console.log("Value:", result.value);
  }
}
```

Inside the `else` branch, TypeScript knows that `value` is a number.

Inside the `if` branch, TypeScript knows that `value` is a string.

# 🗁 Using Built-in Iterators

JavaScript collections provide built-in iterator methods.

**Array iterator**

```typescript
const fruits = ["Apple", "Banana", "Orange"];

const iterator = fruits.values();

console.log(iterator.next().value);
console.log(iterator.next().value);
console.log(iterator.next().value);
```

**String iterator**

```typescript
const text = "ABC";

const iterator = text[Symbol.iterator]();

console.log(iterator.next());
console.log(iterator.next());
console.log(iterator.next());
```

**Map iterator**

```typescript
const users = new Map<number, string>([
  [1, "Alex"],
  [2, "John"],
]);

const iterator = users.entries();

console.log(iterator.next());
console.log(iterator.next());
```

**Set iterator**

```typescript
const ids = new Set([10, 20, 30]);

const iterator = ids.values();

console.log(iterator.next());
console.log(iterator.next());
console.log(iterator.next());
```

These built-in iterators follow the same iterator protocol.

# 🗁 Iterator Return Types

The `Iterator` type accepts three generic parameters:

```typescript
Iterator<TYield, TReturn, TNext>
```

| Parameter | Purpose |
| --- | --- |
| `TYield` | Type of values produced during iteration |
| `TReturn` | Type of the final return value |
| `TNext` | Type of values passed into `next()` |

Example:

```typescript
const iterator: Iterator<number, string, boolean> = {
  next(value?: boolean) {
    if (value === true) {
      return {
        value: "Finished",
        done: true,
      };
    }

    return {
      value: 100,
      done: false,
    };
  },
};
```

The iterator can produce numbers, finish with a string, and accept boolean input.

The standard library's `Iterator` interface uses a tuple-based parameter signature to model the optional `next()` argument.

# 🗁 Iterator and the for...of Loop

The `for...of` loop consumes iterable objects by using their iterators.

```typescript
const numbers = [10, 20, 30];

for (const number of numbers) {
  console.log(number);
}
```

Conceptually, this loop repeatedly calls the iterator's `next()` method until iteration finishes.

However, an object that only implements `Iterator` is not necessarily iterable.

To use an object directly with `for...of`, it must also implement the iterable protocol.

# 🗁 Iterator vs Iterable

An iterator and an iterable are related but different concepts.

**Iterator**

An object that provides a `next()` method.

```typescript
const iterator = [1, 2, 3].values();

iterator.next();
```

**Iterable**

An object that provides a method at `Symbol.iterator` that returns an iterator.

```typescript
const numbers = [1, 2, 3];

for (const number of numbers) {
  console.log(number);
}
```

The key distinction:

* An iterator produces values.
* An iterable provides a way to obtain an iterator.

An object can implement both protocols.

# 🗁 Iterator Helpers in TypeScript

Modern TypeScript versions provide an `IteratorObject` type for modeling built-in iterator objects.

It represents iterators that also expose the iterable protocol and iterator helper methods.

For example:

```typescript
const iterator = [1, 2, 3].values();

const doubled = iterator.map((value) => value * 2);

console.log([...doubled]);
```

The iterator helper methods include operations such as:

* `map()`
* `filter()`
* `take()`
* `drop()`
* `flatMap()`
* `reduce()`
* `toArray()`
* `some()`
* `every()`
* `find()`

These methods allow lazy processing of iterator values.

Their availability depends on the JavaScript runtime and TypeScript library configuration.

# 🗁 Iterator Types in TypeScript

TypeScript provides several iterator-related types.

| Type | Purpose |
| --- | --- |
| `Iterator<T>` | Describes an object that produces values through `next()` |
| `IteratorResult<T, TReturn>` | Describes the result of an iterator operation |
| `Iterable<T>` | Describes an object that provides an iterator |
| `IterableIterator<T>` | Describes an object that is both iterable and an iterator |
| `IteratorObject<T>` | Describes modern iterator objects with iterable behavior and helper methods |
| `Generator<T, TReturn, TNext>` | Describes generator objects |

These types help describe iteration behavior in reusable APIs.

# 🗁 Best Practices

* Use built-in iterators when working with standard collections.
* Implement the iterator protocol when building custom sequence objects.
* Use `IteratorResult` to describe the result of `next()`.
* Distinguish between iterators and iterables.
* Use `for...of` when manual iterator control is unnecessary.
* Use generator functions when manually implementing iterator state becomes complicated.
* Consider runtime support before using modern iterator helper methods.
* Avoid unnecessary type assertions when a class can implement the iterator interface directly.

# 🗁 Summary

An iterator is an object that produces values one at a time through its `next()` method.

Key concepts:

* The iterator protocol defines the `next()` method.
* `IteratorResult` describes each iteration result.
* `done` indicates whether iteration has completed.
* `Iterator` describes iterator objects.
* `Iterable` describes objects that provide iterators.
* Built-in collections expose iterator methods.
* `for...of` consumes iterable objects.
* `IteratorObject` models modern built-in iterator objects.
* Generators provide a convenient way to create iterators.

Understanding iterators is essential for working with JavaScript iteration, custom collections, generators, and lazy data processing.
