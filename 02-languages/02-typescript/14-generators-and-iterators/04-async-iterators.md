# Async Iterators

Async iterators are objects that produce values asynchronously, one at a time.

Unlike regular iterators, which return iteration results immediately, async iterators return promises that resolve to iteration results.

TypeScript provides built-in types such as `AsyncIterator<T>`, `AsyncIterable<T>`, and `AsyncGenerator<TYield, TReturn, TNext>` to describe asynchronous iteration.

Async iterators are useful for:

* Processing API responses.
* Reading data streams.
* Handling paginated data.
* Processing database cursors.
* Consuming asynchronous events.
* Reading files incrementally.
* Working with asynchronous generators.

# 🗁 What Is an Async Iterator?

An async iterator is an object that implements the `next()` method and returns a promise containing an iterator result.

```typescript
const iterator: AsyncIterator<number> = {
  current: 1,

  async next(): Promise<IteratorResult<number>> {
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
} as AsyncIterator<number> & { current: number };
```

Using the async iterator:

```typescript
async function run(): Promise<void> {
  const iterator = createIterator();

  console.log(await iterator.next());
  console.log(await iterator.next());
  console.log(await iterator.next());
  console.log(await iterator.next());
}

function createIterator(): AsyncIterator<number> {
  let current = 1;

  return {
    async next(): Promise<IteratorResult<number>> {
      if (current <= 3) {
        return {
          value: current++,
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

run();
```

Each call to `next()` returns a promise, so `await` is required to obtain the iteration result.

# 🗁 The Async Iterator Protocol

The async iterator protocol defines how an object produces values asynchronously.

An async iterator must provide a method at `next()` that returns a promise-like iteration result.

Conceptually:

```typescript
interface AsyncIterator<T, TReturn = any, TNext = any> {
  next(...args: [] | [TNext]):
    Promise<IteratorResult<T, TReturn>>;

  return?(
    value?: TReturn | PromiseLike<TReturn>
  ): Promise<IteratorResult<T, TReturn>>;

  throw?(
    error?: any
  ): Promise<IteratorResult<T, TReturn>>;
}
```

The standard TypeScript library definitions include additional details and overloads.

The important difference is that an async iterator returns its iteration result asynchronously.

# 🗁 What Is an Async Iterable?

An async iterable is an object that provides an async iterator through `Symbol.asyncIterator`.

TypeScript describes this behavior using the `AsyncIterable<T>` interface.

```typescript
interface AsyncIterable<T> {
  [Symbol.asyncIterator](): AsyncIterator<T>;
}
```

Example:

```typescript
const numbers: AsyncIterable<number> = {
  async *[Symbol.asyncIterator]() {
    yield 10;
    yield 20;
    yield 30;
  },
};
```

The object provides an asynchronous iterator through `Symbol.asyncIterator`.

# 🗁 Async Iterator vs Async Iterable

These concepts are related but have different responsibilities.

| Async Iterator | Async Iterable |
| --- | --- |
| Produces values asynchronously | Provides an async iterator |
| Implements `next()` | Implements `Symbol.asyncIterator` |
| Returns promises from iteration methods | Returns an async iterator |
| Can be consumed manually | Can be consumed with `for await...of` |

An object can implement both protocols.

# 🗁 The Symbol.asyncIterator Symbol

`Symbol.asyncIterator` is a built-in JavaScript symbol used to identify the async iterator method.

```typescript
const data = {
  async *[Symbol.asyncIterator]() {
    yield "First";
    yield "Second";
    yield "Third";
  },
};
```

Obtaining the iterator:

```typescript
const iterator = data[Symbol.asyncIterator]();

console.log(await iterator.next());
console.log(await iterator.next());
console.log(await iterator.next());
console.log(await iterator.next());
```

The method returns an async iterator.

# 🗁 The for await...of Loop

The `for await...of` loop consumes asynchronous iterables.

It automatically obtains an async iterator and awaits each iteration result.

```typescript
async function processNumbers(
  numbers: AsyncIterable<number>
): Promise<void> {
  for await (const number of numbers) {
    console.log(number);
  }
}
```

Using the function:

```typescript
async function* generateNumbers() {
  yield 10;
  yield 20;
  yield 30;
}

processNumbers(generateNumbers());
```

The loop waits for each iteration result before processing the next value.

It can also consume ordinary synchronous iterables by awaiting their iteration results.

# 🗁 Creating an Async Generator

An async generator is created using `async function*`.

It combines asynchronous execution with generator functionality.

```typescript
async function* generateNumbers(): AsyncGenerator<number> {
  yield 10;
  yield 20;
  yield 30;
}
```

Usage:

```typescript
async function run(): Promise<void> {
  const generator = generateNumbers();

  console.log(await generator.next());
  console.log(await generator.next());
  console.log(await generator.next());
  console.log(await generator.next());
}

run();
```

Each call to `next()` returns a promise.

The generator can pause at each `yield` and resume asynchronously.

# 🗁 Async Generator with Delay

An async generator can perform asynchronous operations before producing values.

```typescript
function delay(milliseconds: number): Promise<void> {
  return new Promise((resolve) => {
    setTimeout(resolve, milliseconds);
  });
}

async function* generateNumbers(): AsyncGenerator<number> {
  for (let number = 1; number <= 3; number++) {
    await delay(1000);

    yield number;
  }
}
```

Usage:

```typescript
async function run(): Promise<void> {
  for await (const number of generateNumbers()) {
    console.log(number);
  }
}

run();
```

Each number is produced after the delay.

This is useful when values depend on asynchronous operations.

# 🗁 AsyncGenerator Type

TypeScript provides the `AsyncGenerator` interface.

Its generic parameters are:

```typescript
AsyncGenerator<TYield, TReturn, TNext>
```

| Parameter | Purpose |
| --- | --- |
| `TYield` | Type of asynchronously produced values |
| `TReturn` | Type of the final returned value |
| `TNext` | Type of values passed into the generator |

Example:

```typescript
async function* process(): AsyncGenerator<number, string, boolean> {
  const shouldContinue = yield 10;

  if (shouldContinue) {
    yield 20;
  }

  return "Completed";
}
```

The generator:

* Produces numbers.
* Returns a string when completed.
* Accepts boolean values through `next()`.

# 🗁 Async Generator with Return Values

Async generators can return a final value.

```typescript
async function* processData(): AsyncGenerator<number, string> {
  yield 10;
  yield 20;

  return "Processing completed";
}
```

Usage:

```typescript
async function run(): Promise<void> {
  const generator = processData();

  console.log(await generator.next());
  console.log(await generator.next());
  console.log(await generator.next());
}

run();
```

The final result contains the string and `done: true`.

As with synchronous generators, `for await...of` processes yielded values but does not expose the final return value.

# 🗁 Passing Values Through Async next()

Async generators support sending values back into the suspended generator.

```typescript
async function* conversation(): AsyncGenerator<string, void, string> {
  const name = yield "What is your name?";

  yield `Welcome, ${name}!`;
}
```

Usage:

```typescript
async function run(): Promise<void> {
  const generator = conversation();

  console.log(await generator.next());

  console.log(await generator.next("Alex"));

  console.log(await generator.next());
}

run();
```

The value passed to `next()` becomes the result of the previous `yield` expression.

# 🗁 Async Generator Methods

Async generators provide three important methods.

**next()**

Resumes the generator and returns a promise containing the next iteration result.

```typescript
async function* numbers() {
  yield 10;
  yield 20;
}

async function run(): Promise<void> {
  const generator = numbers();

  console.log(await generator.next());
  console.log(await generator.next());
}
```

**return()**

Requests that the generator complete and returns a final iteration result.

```typescript
async function* numbers() {
  yield 10;
  yield 20;
  yield 30;
}

async function run(): Promise<void> {
  const generator = numbers();

  console.log(await generator.next());

  console.log(await generator.return(undefined));
}
```

**throw()**

Injects an exception into the suspended generator.

```typescript
async function* process() {
  try {
    yield "Started";
    yield "Processing";
  } catch {
    yield "Error handled";
  }
}

async function run(): Promise<void> {
  const generator = process();

  console.log(await generator.next());

  console.log(await generator.throw(new Error("Failure")));

  console.log(await generator.next());
}
```

These methods provide control over the generator's execution lifecycle.

# 🗁 Async Iteration with API Pagination

Async generators are particularly useful for paginated APIs.

Instead of loading every page at once, an async generator can request and yield each page incrementally.

```typescript
interface User {
  id: number;
  name: string;
}

interface ApiResponse {
  data: User[];
  nextPage: number | null;
}

async function fetchUsers(
  page: number
): Promise<ApiResponse> {
  const response = await fetch(`/api/users?page=${page}`);

  if (!response.ok) {
    throw new Error(`Request failed: ${response.status}`);
  }

  return response.json() as Promise<ApiResponse>;
}
```

Create an async generator:

```typescript
async function* getAllUsers(): AsyncGenerator<User> {
  let page: number | null = 1;

  while (page !== null) {
    const result = await fetchUsers(page);

    for (const user of result.data) {
      yield user;
    }

    page = result.nextPage;
  }
}
```

Consume the results:

```typescript
async function run(): Promise<void> {
  for await (const user of getAllUsers()) {
    console.log(user);
  }
}
```

Each user is yielded as the corresponding page is retrieved.

This avoids requiring the entire paginated dataset to be loaded into memory before processing begins.

# 🗁 Async Iteration with Readable Streams

Web Streams API provides readable streams that support asynchronous iteration in modern JavaScript environments.

```typescript
async function readStream(
  stream: ReadableStream<Uint8Array>
): Promise<void> {
  for await (const chunk of stream) {
    console.log(chunk);
  }
}
```

Each chunk is processed as it becomes available.

This is useful for:

* Reading network response bodies.
* Processing large files.
* Handling streamed data.
* Processing data without buffering the entire stream.

The runtime must support async iteration for readable streams.

# 🗁 Async Iterator Error Handling

Errors can occur while requesting asynchronous values.

Use `try...catch` to handle errors during iteration.

```typescript
async function* generateData(): AsyncGenerator<number> {
  yield 10;

  throw new Error("Data generation failed");
}
```

Consume the generator safely:

```typescript
async function run(): Promise<void> {
  try {
    for await (const value of generateData()) {
      console.log(value);
    }
  } catch (error: unknown) {
    if (error instanceof Error) {
      console.error(error.message);
    }
  }
}
```

The error is propagated to the consumer and can be handled using normal asynchronous error-handling techniques.

# 🗁 Cleanup with try...finally

Async generators can use `finally` blocks to release resources when iteration completes or is interrupted.

```typescript
async function* readData(): AsyncGenerator<number> {
  try {
    yield 10;
    yield 20;
    yield 30;
  } finally {
    console.log("Cleanup completed");
  }
}
```

Usage:

```typescript
async function run(): Promise<void> {
  for await (const value of readData()) {
    console.log(value);

    if (value === 10) {
      break;
    }
  }
}

run();
```

When the loop exits early, it closes the iterator and allows the generator's cleanup logic to run.

This is important when generators manage resources such as streams or database cursors.

# 🗁 Async Generator Delegation with yield*

Async generators can delegate iteration using `yield*`.

```typescript
async function* first(): AsyncGenerator<number> {
  yield 10;
  yield 20;
}

async function* second(): AsyncGenerator<number> {
  yield* first();

  yield 30;
  yield 40;
}
```

Usage:

```typescript
async function run(): Promise<void> {
  for await (const value of second()) {
    console.log(value);
  }
}

run();
```

The second generator delegates part of its iteration to the first generator.

Async generator delegation also supports compatible synchronous iterables.

# 🗁 Synchronous vs Asynchronous Iterators

| Feature | Synchronous Iterator | Asynchronous Iterator |
| --- | --- | --- |
| Protocol method | `Symbol.iterator` | `Symbol.asyncIterator` |
| Iteration method | `next()` | `next()` |
| Return value | `IteratorResult` | Promise of `IteratorResult` |
| Consumption | `for...of` | `for await...of` |
| Generator syntax | `function*` | `async function*` |
| TypeScript type | `Iterator<T>` | `AsyncIterator<T>` |
| Iterable type | `Iterable<T>` | `AsyncIterable<T>` |
| Generator type | `Generator<T>` | `AsyncGenerator<T>` |

# 🗁 Async Iterable vs Promise of Iterable

These types represent different concepts.

**AsyncIterable**

Produces values through asynchronous iteration.

```typescript
async function* getNumbers(): AsyncGenerator<number> {
  yield 10;
  yield 20;
}
```

**Promise of Iterable**

Resolves to an iterable object.

```typescript
async function getNumbers(): Promise<number[]> {
  return [10, 20, 30];
}
```

The difference:

* `AsyncIterable<T>` produces values incrementally.
* `Promise<Iterable<T>>` resolves to an iterable after an asynchronous operation.

Use `AsyncIterable<T>` when values should become available progressively.

Use `Promise<Iterable<T>>` when the complete iterable is obtained as one asynchronous result.

# 🗁 Compiler Configuration

Async iteration requires suitable TypeScript library declarations and JavaScript runtime support.

Example configuration:

```json
{
  "compilerOptions": {
    "target": "ES2022",
    "lib": ["ES2022", "DOM", "DOM.AsyncIterable"],
    "strict": true
  }
}
```

For older compilation targets, TypeScript may require additional configuration such as `downlevelIteration` and compatible runtime support.

The compiler configuration does not automatically add missing runtime features.

# 🗁 Best Practices

* Use `AsyncIterable<T>` when consuming asynchronous sequences.
* Use `async function*` to create asynchronous generators.
* Prefer `for await...of` for consuming async iterables.
* Handle errors with `try...catch`.
* Use `try...finally` when generators manage resources.
* Use asynchronous generators for paginated APIs and streaming data.
* Avoid collecting an entire stream into memory when incremental processing is sufficient.
* Consider early termination and cleanup behavior.
* Check runtime compatibility before using async iteration APIs.
* Distinguish between `AsyncIterable<T>` and `Promise<Iterable<T>>`.

# 🗁 Summary

Async iterators provide a standard way to consume values asynchronously, one at a time.

Key concepts:

* `AsyncIterator<T>` describes asynchronous iterator objects.
* `AsyncIterable<T>` describes objects that provide async iterators.
* `Symbol.asyncIterator` implements the async iterable protocol.
* Async iterator methods return promises.
* `for await...of` consumes asynchronous iterables.
* `async function*` creates asynchronous generators.
* `AsyncGenerator<TYield, TReturn, TNext>` describes async generator types.
* `yield*` delegates iteration.
* `try...finally` supports cleanup during early termination.
* Async iterators are useful for APIs, streams, pagination, and incremental data processing.

Understanding async iterators helps you build efficient applications that process asynchronous data without requiring everything to be loaded at once.
