# JavaScript Generators

**Generators** are special JavaScript functions that can pause their execution and resume it later.

Unlike regular functions, which execute from beginning to end when called, generator functions can suspend execution at specific points using the `yield` keyword.

Generators are useful for:

* Lazy evaluation
* Creating custom iterators
* Processing large datasets
* Implementing sequences
* Managing state across function calls
* Building asynchronous workflows
* Generating values on demand

# 🗁 What Is a Generator Function?

A generator function is declared using the `function*` syntax.

Example:

```javascript
function* generateNumbers() {
    yield 10;
    yield 20;
    yield 30;
}
```

Calling a generator function does not immediately execute its body.

Instead, it returns a **Generator Object**.

```javascript
const generator = generateNumbers();

console.log(generator);
```

The generator object provides a `next()` method that controls execution.

# 🗁 The yield Keyword

The `yield` keyword pauses generator execution and produces a value.

Example:

```javascript
function* generateNumbers() {
    yield 10;
    yield 20;
    yield 30;
}

const generator = generateNumbers();

console.log(generator.next());
console.log(generator.next());
console.log(generator.next());
console.log(generator.next());
```

Output:

```javascript
{ value: 10, done: false }
{ value: 20, done: false }
{ value: 30, done: false }
{ value: undefined, done: true }
```

Each call to `next()` resumes execution until the next `yield` statement or the function completes.

The returned object contains:

* `value`: The value produced by the generator.
* `done`: Indicates whether the generator has completed.

# 🗁 Generator Execution Flow

Consider:

```javascript
function* example() {
    console.log("Start");

    yield 1;

    console.log("Middle");

    yield 2;

    console.log("End");
}

const generator = example();

console.log("Generator created");

console.log(generator.next());
console.log(generator.next());
console.log(generator.next());
```

Output:

```text
Generator created
Start
{ value: 1, done: false }
Middle
{ value: 2, done: false }
End
{ value: undefined, done: true }
```

Execution flow:

```text
Call Generator Function
          |
          v
  Generator Object Created
          |
          v
       next()
          |
          v
   Execute Until yield
          |
          v
     Pause Execution
          |
          v
       next()
          |
          v
   Resume Execution
          |
          v
     Another yield
          |
          v
     Pause Execution
          |
          v
       next()
          |
          v
   Function Completed
```

# 🗁 Generator Function vs Regular Function

| Feature | Regular Function | Generator Function |
| --- | --- | --- |
| Declaration | `function` | `function*` |
| Execution | Runs when called | Runs when iteration starts |
| Return value | Returns a value | Returns a generator object |
| Pause execution | No | Yes, using `yield` |
| Resume execution | Cannot resume suspended state | Resumes with `next()` |
| State preservation | Through closures or objects | Maintains suspended execution state |
| Iteration | Not automatically iterable | Generator objects are iterable |

Example:

```javascript
function regularFunction() {
    return 10;
}

function* generatorFunction() {
    yield 10;
}

console.log(regularFunction());
// 10

console.log(generatorFunction());
// Generator object
```

# 🗁 Generator Objects

Calling a generator function creates a generator object.

A generator object implements the iterator protocol and the iterable protocol.

It provides these methods:

* `next()`
* `return()`
* `throw()`

Example:

```javascript
function* generateValues() {
    yield "A";
    yield "B";
    yield "C";
}

const generator = generateValues();

console.log(generator.next());
console.log(generator.next());
```

The generator object also supports iteration:

```javascript
function* generateNumbers() {
    yield 10;
    yield 20;
    yield 30;
}

for (const number of generateNumbers()) {
    console.log(number);
}
```

Output:

```text
10
20
30
```

# 🗁 The next() Method

The `next()` method resumes generator execution and returns an iterator result object.

Syntax:

```javascript
generator.next(value);
```

The optional argument becomes the result of the currently suspended `yield` expression.

Example:

```javascript
function* generatorFunction() {
    const first = yield "First";

    const second = yield first + " Second";

    return second;
}

const generator = generatorFunction();

console.log(generator.next());
console.log(generator.next("Hello"));
console.log(generator.next("World"));
```

Output:

```javascript
{ value: "First", done: false }
{ value: "Hello Second", done: false }
{ value: "World", done: true }
```

The value passed to the first `next()` call is ignored because the generator has not yet reached a suspended `yield` expression.

# 🗁 Passing Values into Generators

A generator can receive values through the `next()` method.

Example:

```javascript
function* calculator() {
    const first = yield "Enter first number";

    const second = yield "Enter second number";

    return first + second;
}

const generator = calculator();

console.log(generator.next());

console.log(generator.next(10));

console.log(generator.next(20));
```

Output:

```javascript
{ value: "Enter first number", done: false }
{ value: "Enter second number", done: false }
{ value: 30, done: true }
```

This allows generators to behave like stateful communication interfaces.

# 🗁 The return() Method

The `return()` method terminates a generator and optionally provides a final value.

Example:

```javascript
function* generateNumbers() {
    yield 10;
    yield 20;
    yield 30;
}

const generator = generateNumbers();

console.log(generator.next());

console.log(generator.return(100));

console.log(generator.next());
```

Output:

```javascript
{ value: 10, done: false }
{ value: 100, done: true }
{ value: undefined, done: true }
```

After completion, the generator cannot resume its previous execution.

A `finally` block still executes when a generator is closed.

```javascript
function* generateNumbers() {
    try {
        yield 10;
        yield 20;
    } finally {
        console.log("Generator cleanup");
    }
}

const generator = generateNumbers();

console.log(generator.next());

generator.return();
```

Output:

```text
{ value: 10, done: false }
Generator cleanup
```

# 🗁 The throw() Method

The `throw()` method injects an exception at the generator's current suspended position.

Example:

```javascript
function* generateNumbers() {
    try {
        yield 10;
        yield 20;
    } catch (error) {
        yield `Caught: ${error.message}`;
    }

    yield 30;
}

const generator = generateNumbers();

console.log(generator.next());

console.log(
    generator.throw(new Error("Something went wrong"))
);

console.log(generator.next());

console.log(generator.next());
```

Output:

```javascript
{ value: 10, done: false }
{ value: "Caught: Something went wrong", done: false }
{ value: 30, done: false }
{ value: undefined, done: true }
```

If the injected exception is not handled inside the generator, it propagates to the caller and the generator becomes completed.

# 🗁 The yield* Keyword

The `yield*` expression delegates iteration to another iterable or generator.

It allows one generator to produce values from another iterable.

Example:

```javascript
function* firstGenerator() {
    yield 10;
    yield 20;
}

function* secondGenerator() {
    yield 30;
    yield 40;
}

function* combinedGenerator() {
    yield* firstGenerator();
    yield* secondGenerator();
}

const generator = combinedGenerator();

console.log([...generator]);
```

Output:

```javascript
[10, 20, 30, 40]
```

It also works with arrays, strings, and other iterable objects.

```javascript
function* generateValues() {
    yield* [1, 2, 3];

    yield* "ABC";
}

console.log([...generateValues()]);
```

Output:

```javascript
[1, 2, 3, "A", "B", "C"]
```

# 🗁 Returning Values from Generators

A generator can use `return` to produce its final value.

Example:

```javascript
function* generateNumbers() {
    yield 10;
    yield 20;

    return 30;
}

const generator = generateNumbers();

console.log(generator.next());
console.log(generator.next());
console.log(generator.next());
```

Output:

```javascript
{ value: 10, done: false }
{ value: 20, done: false }
{ value: 30, done: true }
```

A normal `for...of` loop does not include the final return value.

```javascript
function* generateNumbers() {
    yield 10;
    yield 20;

    return 30;
}

for (const number of generateNumbers()) {
    console.log(number);
}
```

Output:

```text
10
20
```

The final value is available through the `next()` result that has `done: true`.

# 🗁 Generator as an Iterator

A generator automatically implements the iterator protocol.

This means its object provides a `next()` method that returns iterator result objects.

Example:

```javascript
function* generateColors() {
    yield "Red";
    yield "Green";
    yield "Blue";
}

const colors = generateColors();

console.log(colors.next());
console.log(colors.next());
console.log(colors.next());
console.log(colors.next());
```

It also implements the iterable protocol through `Symbol.iterator`.

```javascript
function* generateNumbers() {
    yield 1;
    yield 2;
    yield 3;
}

const generator = generateNumbers();

console.log(generator[Symbol.iterator]() === generator);
// true
```

# 🗁 Generator with Infinite Sequences

Generators can produce infinite sequences without storing all values in memory.

Example:

```javascript
function* infiniteNumbers() {
    let number = 1;

    while (true) {
        yield number++;
    }
}

const numbers = infiniteNumbers();

console.log(numbers.next().value); // 1
console.log(numbers.next().value); // 2
console.log(numbers.next().value); // 3
console.log(numbers.next().value); // 4
```

The generator produces each value only when requested.

Always use a controlled iteration strategy when working with infinite generators.

# 🗁 Lazy Evaluation

**Lazy Evaluation** means calculating values only when they are needed.

Generators naturally support lazy evaluation because execution pauses at each `yield`.

Example:

```javascript
function* generateNumbers(limit) {
    for (let number = 1; number <= limit; number++) {
        console.log(`Generating ${number}`);

        yield number;
    }
}

const numbers = generateNumbers(5);

console.log(numbers.next().value);
console.log(numbers.next().value);
```

Output:

```text
Generating 1
1
Generating 2
2
```

Only the requested values are generated.

# 🗁 Practical Example: Fibonacci Sequence

A generator can produce Fibonacci numbers one at a time.

Example:

```javascript
function* fibonacci(limit) {
    let previous = 0;
    let current = 1;

    for (let index = 0; index < limit; index++) {
        yield previous;

        [previous, current] = [
            current,
            previous + current
        ];
    }
}

console.log([...fibonacci(10)]);
```

Output:

```javascript
[0, 1, 1, 2, 3, 5, 8, 13, 21, 34]
```

# 🗁 Practical Example: Unique ID Generator

Generators can maintain state between calls to produce unique identifiers.

Example:

```javascript
function* createIdGenerator() {
    let id = 1;

    while (true) {
        yield `USER-${id++}`;
    }
}

const generateId = createIdGenerator();

console.log(generateId.next().value);
console.log(generateId.next().value);
console.log(generateId.next().value);
```

Output:

```text
USER-1
USER-2
USER-3
```

# 🗁 Practical Example: Pagination

Generators can produce data in batches.

Example:

```javascript
function* paginate(items, pageSize) {
    for (let index = 0; index < items.length; index += pageSize) {
        yield items.slice(index, index + pageSize);
    }
}

const users = [
    "Alice",
    "Bob",
    "Charlie",
    "David",
    "Eve",
    "Frank"
];

for (const page of paginate(users, 2)) {
    console.log(page);
}
```

Output:

```javascript
["Alice", "Bob"]
["Charlie", "David"]
["Eve", "Frank"]
```

This pattern is useful when processing data in manageable batches.

# 🗁 Generator Error Handling

Generators support normal JavaScript error-handling mechanisms.

Example:

```javascript
function* generateData() {
    try {
        yield "Loading";

        throw new Error("Failed to load data");
    } catch (error) {
        yield error.message;
    }

    yield "Finished";
}

const generator = generateData();

console.log(generator.next());
console.log(generator.next());
console.log(generator.next());
console.log(generator.next());
```

Output:

```javascript
{ value: "Loading", done: false }
{ value: "Failed to load data", done: false }
{ value: "Finished", done: false }
{ value: undefined, done: true }
```

# 🗁 Generator Function Types

JavaScript supports different generator function forms.

**Generator Function Declaration**

```javascript
function* generate() {
    yield 1;
}
```

**Generator Function Expression**

```javascript
const generate = function* () {
    yield 1;
};
```

**Generator Method in an Object**

```javascript
const sequence = {
    *generate() {
        yield 1;
        yield 2;
    }
};

console.log([...sequence.generate()]);
```

**Generator Method in a Class**

```javascript
class NumberSequence {
    *generate() {
        yield 1;
        yield 2;
        yield 3;
    }
}

const sequence = new NumberSequence();

console.log([...sequence.generate()]);
```

**Generator Function as a Property**

```javascript
const object = {
    generate: function* () {
        yield "A";
        yield "B";
    }
};

console.log([...object.generate()]);
```

# 🗁 Generator vs Iterator

| Feature | Generator | Custom Iterator |
| --- | --- | --- |
| Creation | `function*` | Manually implemented object |
| `next()` method | Provided automatically | Must be implemented |
| Execution state | Maintained automatically | Managed manually |
| Pausing | Uses `yield` | Implemented manually |
| Complexity | Usually simpler | Can require more code |
| Iterable protocol | Implemented automatically | Must be implemented |

Example of a custom iterator:

```javascript
const numbers = {
    current: 1,
    last: 3,

    [Symbol.iterator]() {
        return this;
    },

    next() {
        if (this.current <= this.last) {
            return {
                value: this.current++,
                done: false
            };
        }

        return {
            value: undefined,
            done: true
        };
    }
};

console.log([...numbers]);
```

Equivalent generator implementation:

```javascript
function* generateNumbers() {
    yield 1;
    yield 2;
    yield 3;
}

console.log([...generateNumbers()]);
```

Generators simplify iterator implementation by managing execution state automatically.

# 🗁 Generator vs Async Function

| Feature | Generator | Async Function |
| --- | --- | --- |
| Declaration | `function*` | `async function` |
| Main keyword | `yield` | `await` |
| Return value | Generator object | Promise |
| Execution control | Resumed through iterator methods | Resumed by Promise settlement |
| Pause mechanism | `yield` | `await` |
| Typical use | Custom iteration and lazy sequences | Asynchronous operations |

Example:

```javascript
function* generatorFunction() {
    yield 10;
    yield 20;
}

async function asyncFunction() {
    return 10;
}

console.log(generatorFunction());

console.log(asyncFunction());
// Promise
```

# 🗁 Generator Limitations

Although generators are useful, they are not suitable for every situation.

* Generator execution requires explicit iteration or method calls.
* Generator objects maintain execution state.
* A generator object is normally consumed only once.
* Generators introduce additional complexity when simple arrays or loops are sufficient.
* Infinite generators require controlled consumption.
* Generator functions cannot be declared as arrow functions.
* Generators are synchronous unless used through asynchronous generator syntax.
* Generator execution is not automatically parallel.

# 🗁 Best Practices

* Use generators for lazy sequences and custom iteration.
* Prefer `yield` for producing values incrementally.
* Use `yield*` to delegate iteration to other iterables.
* Use `try...finally` when generator cleanup is required.
* Avoid materializing large sequences unnecessarily.
* Control iteration when working with infinite generators.
* Use descriptive generator function names.
* Prefer regular functions when pausing and resuming execution provides no benefit.
* Use asynchronous generators when producing values asynchronously.

# 🗁 Key Takeaways

* Generators are special functions declared using `function*`.
* Calling a generator function returns a generator object.
* Generator execution starts when iteration begins.
* `yield` pauses execution and produces a value.
* `next()` resumes execution.
* `return()` terminates a generator.
* `throw()` injects an exception into a suspended generator.
* `yield*` delegates iteration to another iterable.
* Generator objects implement the iterator and iterable protocols.
* Generators support lazy evaluation and infinite sequences.
* Generators simplify custom iterator implementation.

# 🗁 Related Topics

* Functions
* Function Expressions
* Iterators
* Iterable Protocol
* Symbol.iterator
* for...of
* Async Functions
* Async Generators
* Promises
* Event Loop
* Lazy Evaluation
