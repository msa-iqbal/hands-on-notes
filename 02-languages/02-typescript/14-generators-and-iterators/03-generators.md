# Generators

Generators are special functions that can pause execution, produce values one at a time, and resume execution from where they stopped.

Unlike regular functions, which execute completely when called, generator functions maintain their execution state between calls.

TypeScript supports JavaScript generators and provides the `Generator<TYield, TReturn, TNext>` type to describe them.

Generators are useful for:

* Creating custom iterators.
* Producing values lazily.
* Processing large datasets.
* Implementing state machines.
* Managing sequences of operations.
* Building asynchronous workflows when combined with other mechanisms.

# 🗁 What Is a Generator?

A generator is a function declared using the `function*` syntax.

It uses the `yield` keyword to produce values.

```typescript
function* generateNumbers() {
  yield 10;
  yield 20;
  yield 30;
}
```

Calling a generator function does not immediately execute its body.

Instead, it returns a generator object.

```typescript
const generator = generateNumbers();

console.log(generator.next());
// { value: 10, done: false }

console.log(generator.next());
// { value: 20, done: false }

console.log(generator.next());
// { value: 30, done: false }

console.log(generator.next());
// { value: undefined, done: true }
```

Each call to `next()` resumes execution until the next `yield` statement or until the function completes.

# 🗁 Generator Function Syntax

The basic syntax of a generator function is:

```typescript
function* functionName() {
  yield value;
}
```

The `*` indicates that the function is a generator function.

The `yield` keyword pauses execution and produces a value.

Example:

```typescript
function* getMessages() {
  console.log("First message");

  yield "Hello";

  console.log("Second message");

  yield "Welcome";

  console.log("Finished");
}
```

Usage:

```typescript
const messages = getMessages();

messages.next();
// Logs: First message

messages.next();
// Logs: Second message

messages.next();
// Logs: Finished
```

The function executes incrementally rather than running all at once.

# 🗁 Generator Execution Flow

A generator maintains its execution state between calls.

```typescript
function* counter() {
  console.log("Started");

  yield 1;

  console.log("Resumed");

  yield 2;

  console.log("Completed");

  return 3;
}
```

Execution:

```typescript
const generator = counter();

console.log(generator.next());

console.log(generator.next());

console.log(generator.next());
```

Output:

```text
Started
{ value: 1, done: false }

Resumed
{ value: 2, done: false }

Completed
{ value: 3, done: true }
```

The execution sequence is:

* Calling `counter()` creates a generator object.
* The first `next()` starts execution.
* The first `yield` pauses execution.
* The second `next()` resumes execution.
* The second `yield` pauses execution again.
* The third `next()` completes the function.

# 🗁 The yield Keyword

The `yield` keyword produces a value and pauses the generator.

```typescript
function* numbers() {
  yield 10;
  yield 20;
  yield 30;
}
```

Each `yield` produces one value.

A generator can contain multiple `yield` expressions, conditional statements, loops, and other normal JavaScript logic.

```typescript
function* getNumbers(max: number) {
  for (let i = 1; i <= max; i++) {
    yield i;
  }
}
```

Usage:

```typescript
const generator = getNumbers(3);

console.log(generator.next());
console.log(generator.next());
console.log(generator.next());
console.log(generator.next());
```

Output:

```text
{ value: 1, done: false }
{ value: 2, done: false }
{ value: 3, done: false }
{ value: undefined, done: true }
```

# 🗁 The return Keyword in Generators

A generator can use `return` to complete execution and provide a final value.

```typescript
function* getValues() {
  yield 10;
  yield 20;

  return 30;
}
```

Usage:

```typescript
const generator = getValues();

console.log(generator.next());
// { value: 10, done: false }

console.log(generator.next());
// { value: 20, done: false }

console.log(generator.next());
// { value: 30, done: true }
```

The returned value has `done: true`.

Once a generator finishes, subsequent calls to `next()` do not resume its execution.

```typescript
console.log(generator.next());
// { value: undefined, done: true }
```

A generator's return value is different from its yielded values.

# 🗁 Generator Type in TypeScript

TypeScript provides the `Generator` interface to describe generator objects.

Its generic parameters are:

```typescript
Generator<TYield, TReturn, TNext>
```

| Parameter | Purpose |
| --- | --- |
| `TYield` | Type of values produced by `yield` |
| `TReturn` | Type of the final value returned by `return` |
| `TNext` | Type of values passed into the generator through `next()` |

Example:

```typescript
function* getNumbers(): Generator<number, string, unknown> {
  yield 10;
  yield 20;

  return "Completed";
}
```

The generator:

* Produces numbers.
* Returns a string when completed.
* Accepts an `unknown` input type through `next()`.

The return type can be inferred in many cases, but explicit annotations make complex generator contracts easier to understand.

# 🗁 Generator Yield Types

The first generic parameter describes the values produced by `yield`.

```typescript
function* getUsers(): Generator<string> {
  yield "Alex";
  yield "John";
  yield "Sarah";
}
```

All yielded values must be strings.

Invalid example:

```typescript
function* getUsers(): Generator<string> {
  yield "Alex";

  // Type error: number is not assignable to string.
  yield 100;
}
```

If multiple yield types are needed, use a union.

```typescript
function* getValues(): Generator<string | number> {
  yield "Hello";
  yield 100;
}
```

# 🗁 Generator Return Types

The second generic parameter describes the final value returned by the generator.

```typescript
function* processData(): Generator<number, string> {
  yield 10;
  yield 20;

  return "Processing completed";
}
```

The yielded values are numbers, while the final return value is a string.

```typescript
const generator = processData();

console.log(generator.next());
// { value: 10, done: false }

console.log(generator.next());
// { value: 20, done: false }

console.log(generator.next());
// { value: "Processing completed", done: true }
```

The `Generator<number, string>` type describes this distinction.

# 🗁 Passing Values Through next()

The `next()` method can send values back into a suspended generator.

The value passed to `next()` becomes the result of the previous `yield` expression.

```typescript
function* conversation(): Generator<string, void, string> {
  const name = yield "What is your name?";

  yield `Hello, ${name}!`;
}
```

Usage:

```typescript
const generator = conversation();

console.log(generator.next());
// { value: "What is your name?", done: false }

console.log(generator.next("Alex"));
// { value: "Hello, Alex!", done: false }

console.log(generator.next());
// { value: undefined, done: true }
```

The third generic parameter, `string`, describes the type accepted by `next()`.

The first call to `next()` should normally be made without an argument because the generator has not reached a `yield` expression yet.

# 🗁 Generator with Multiple Input Values

Generators can receive multiple values across different suspension points.

```typescript
function* calculate(): Generator<number, number, number> {
  const first = yield 10;

  const second = yield 20;

  return first + second;
}
```

Usage:

```typescript
const generator = calculate();

console.log(generator.next());
// { value: 10, done: false }

console.log(generator.next(5));
// { value: 20, done: false }

console.log(generator.next(15));
// { value: 20, done: true }
```

The final value is `5 + 15`.

This pattern allows generators to behave like bidirectional communication mechanisms.

# 🗁 Generators with for...of

Generator objects implement the iterable protocol.

Therefore, they can be consumed with `for...of`.

```typescript
function* getNumbers(): Generator<number> {
  yield 10;
  yield 20;
  yield 30;
}

for (const number of getNumbers()) {
  console.log(number);
}
```

Output:

```text
10
20
30
```

The loop automatically consumes the generator until it completes.

The final value returned through `return` is not included in `for...of` iteration.

```typescript
function* getValues(): Generator<number, number> {
  yield 10;
  yield 20;

  return 30;
}

for (const value of getValues()) {
  console.log(value);
}
```

Output:

```text
10
20
```

The returned value `30` is available through the final `next()` result, not through normal iteration.

# 🗁 Generator with Spread Syntax

Because generators are iterable, their yielded values can be expanded using spread syntax.

```typescript
function* getNumbers(): Generator<number> {
  yield 10;
  yield 20;
  yield 30;
}

const numbers = [...getNumbers()];

console.log(numbers);
```

Output:

```text
[10, 20, 30]
```

Only yielded values are included. The final return value is excluded.

# 🗁 Generator Delegation with yield*

The `yield*` expression delegates iteration to another iterable or generator.

```typescript
function* first() {
  yield 10;
  yield 20;
}

function* second() {
  yield* first();

  yield 30;
  yield 40;
}
```

Usage:

```typescript
console.log([...second()]);
```

Output:

```text
[10, 20, 30, 40]
```

The second generator delegates part of its iteration to the first generator.

This is useful for composing multiple generators.

# 🗁 Delegating to Arrays and Other Iterables

The `yield*` expression can delegate to any compatible iterable.

```typescript
function* getNumbers(): Generator<number> {
  yield* [10, 20, 30];

  yield* new Set([40, 50]);

  yield 60;
}
```

Usage:

```typescript
console.log([...getNumbers()]);
```

Output:

```text
[10, 20, 30, 40, 50, 60]
```

This allows values from different iterable sources to be combined.

# 🗁 Generator Methods: next(), return(), and throw()

Generator objects provide three important methods.

**next()**

Resumes execution and returns the next iterator result.

```typescript
function* getNumbers() {
  yield 10;
  yield 20;
}

const generator = getNumbers();

console.log(generator.next());
console.log(generator.next());
```

**return()**

Terminates the generator and returns a specified value.

```typescript
function* getNumbers() {
  yield 10;
  yield 20;
  yield 30;
}

const generator = getNumbers();

console.log(generator.next());

console.log(generator.return(100));
```

The generator is closed, and its subsequent calls report completion.

**throw()**

Injects an exception into the suspended generator at its current suspension point.

```typescript
function* process() {
  try {
    yield "Started";
    yield "Processing";
  } catch (error) {
    yield "An error occurred";
  }
}

const generator = process();

console.log(generator.next());

console.log(generator.throw(new Error("Failure")));

console.log(generator.next());
```

The generator can catch and handle the injected exception using `try...catch`.

# 🗁 Generator with try...finally

Generators support normal JavaScript error-handling and cleanup mechanisms.

```typescript
function* processData(): Generator<string> {
  try {
    yield "Processing";
    yield "Almost completed";
  } finally {
    console.log("Cleanup executed");
  }
}
```

Usage:

```typescript
const generator = processData();

console.log(generator.next());

console.log(generator.return(undefined));
```

The `finally` block executes when the generator is closed through `return()`.

This is useful when iteration manages resources or requires cleanup.

# 🗁 Lazy Evaluation with Generators

Generators produce values only when requested.

This behavior is called lazy evaluation.

```typescript
function* generateNumbers(max: number): Generator<number> {
  for (let i = 1; i <= max; i++) {
    console.log(`Generating ${i}`);

    yield i;
  }
}
```

Usage:

```typescript
const generator = generateNumbers(1000000);

console.log(generator.next());

console.log(generator.next());

console.log(generator.next());
```

Only three values are generated.

The remaining values are not computed until the generator is resumed.

This can reduce memory usage when working with large or potentially unbounded sequences.

# 🗁 Infinite Generators

Generators can represent infinite sequences.

```typescript
function* infiniteCounter(): Generator<number> {
  let count = 1;

  while (true) {
    yield count++;
  }
}
```

Usage:

```typescript
const counter = infiniteCounter();

console.log(counter.next().value);
console.log(counter.next().value);
console.log(counter.next().value);
```

Output:

```text
1
2
3
```

The generator continues producing values whenever `next()` is called.

Use a termination condition when consuming infinite generators.

```typescript
for (const value of infiniteCounter()) {
  if (value > 5) {
    break;
  }

  console.log(value);
}
```

The loop stops after five values.

# 🗁 Practical Example: Pagination

Generators can simplify processing paginated data.

```typescript
interface User {
  id: number;
  name: string;
}

function* paginate(
  users: User[],
  pageSize: number
): Generator<User[]> {
  for (let index = 0; index < users.length; index += pageSize) {
    yield users.slice(index, index + pageSize);
  }
}
```

Usage:

```typescript
const users: User[] = [
  { id: 1, name: "Alex" },
  { id: 2, name: "John" },
  { id: 3, name: "Sarah" },
  { id: 4, name: "David" },
  { id: 5, name: "Emma" },
];

for (const page of paginate(users, 2)) {
  console.log(page);
}
```

Output:

```text
[
  { id: 1, name: "Alex" },
  { id: 2, name: "John" }
]

[
  { id: 3, name: "Sarah" },
  { id: 4, name: "David" }
]

[
  { id: 5, name: "Emma" }
]
```

Each page is produced only when requested.

# 🗁 Practical Example: Custom Range Generator

A range generator produces numbers between two boundaries.

```typescript
function* range(
  start: number,
  end: number,
  step = 1
): Generator<number> {
  if (step === 0) {
    throw new Error("Step cannot be zero");
  }

  if (step > 0) {
    for (let value = start; value <= end; value += step) {
      yield value;
    }
  } else {
    for (let value = start; value >= end; value += step) {
      yield value;
    }
  }
}
```

Usage:

```typescript
console.log([...range(1, 5)]);

console.log([...range(0, 10, 2)]);

console.log([...range(5, 1, -1)]);
```

Output:

```text
[1, 2, 3, 4, 5]

[0, 2, 4, 6, 8, 10]

[5, 4, 3, 2, 1]
```

The generator produces values without creating an intermediate array.

# 🗁 Generator Functions vs Regular Functions

| Feature | Regular Function | Generator Function |
| --- | --- | --- |
| Declaration | `function` | `function*` |
| Execution | Runs when called | Runs when resumed |
| Return value | Normal return value | Generator object |
| Pause execution | No | Yes, with `yield` |
| Resume execution | No | With `next()` |
| Multiple values | Usually through a collection | Can yield values individually |
| Iteration support | Not automatically | Generator objects are iterable |
| Execution state | Does not preserve suspended state | Preserves suspended state |

# 🗁 Generator vs Async Generator

A normal generator produces values synchronously.

An asynchronous generator produces values asynchronously.

**Generator**

```typescript
function* numbers(): Generator<number> {
  yield 10;
  yield 20;
}
```

**Async generator**

```typescript
async function* numbers(): AsyncGenerator<number> {
  yield 10;
  yield 20;
}
```

Normal generators use `next()` synchronously.

Async generators return promises from `next()` and are commonly consumed using `for await...of`.

# 🗁 Best Practices

* Use generators when values should be produced incrementally.
* Prefer generators for lazy sequence processing.
* Use explicit generator return types for reusable APIs.
* Use `yield*` to compose iterable sequences.
* Handle generator cleanup with `try...finally` when necessary.
* Avoid unbounded iteration without a termination strategy.
* Use ordinary functions when pausing and resuming execution provides no benefit.
* Use async generators when values depend on asynchronous operations.
* Remember that spread syntax and array conversion eagerly consume the entire generator.

# 🗁 Summary

Generators are special functions that can pause and resume execution while producing values.

Key concepts:

* Generator functions use `function*`.
* The `yield` keyword produces values and pauses execution.
* `next()` resumes the generator.
* `return` completes the generator and provides a final value.
* `Generator<TYield, TReturn, TNext>` describes generator types.
* `yield*` delegates iteration to another iterable.
* Generator objects implement the iterable protocol.
* Generators support lazy evaluation.
* Infinite generators can produce unbounded sequences.
* `return()` and `throw()` provide additional execution control.

Generators are useful for building custom iterators, processing large datasets, and creating efficient lazy sequences.
