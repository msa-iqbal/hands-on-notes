# JavaScript Iterators

**Iterators** are objects that provide a standardized way to access elements from a sequence one at a time.

JavaScript uses iterators to support sequential data access without requiring developers to know how the underlying data structure is implemented.

Iterators are fundamental to:

* `for...of` loops
* Arrays and strings
* Maps and Sets
* Generators
* Spread syntax
* Array destructuring
* Custom data structures
* Lazy evaluation
* Asynchronous iteration

# 🗁 What Is an Iterator?

An iterator is an object that implements the iterator protocol.

It must provide a `next()` method that returns an iterator result object.

Example:

```javascript
const iterator = {
    current: 1,

    next() {
        if (this.current <= 3) {
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

console.log(iterator.next());
console.log(iterator.next());
console.log(iterator.next());
console.log(iterator.next());
```

Output:

```javascript
{ value: 1, done: false }
{ value: 2, done: false }
{ value: 3, done: false }
{ value: undefined, done: true }
```

# 🗁 Iterator Protocol

The **Iterator Protocol** defines how an object produces a sequence of values.

An iterator must provide a `next()` method.

Each call to `next()` returns an object containing:

* `value`: The current iteration value.
* `done`: A Boolean indicating whether iteration has completed.

Example:

```javascript
const iterator = {
    index: 0,

    next() {
        if (this.index < 3) {
            return {
                value: this.index++,
                done: false
            };
        }

        return {
            value: undefined,
            done: true
        };
    }
};
```

The iterator protocol does not require a specific implementation or internal data structure.

# 🗁 Iterator Result Object

Every call to an iterator's `next()` method returns an iterator result object.

Example:

```javascript
const result = {
    value: "JavaScript",
    done: false
};

console.log(result.value);
console.log(result.done);
```

Output:

```text
JavaScript
false
```

When iteration is complete:

```javascript
const result = {
    value: undefined,
    done: true
};
```

The `value` property can contain any JavaScript value, including `undefined`.

The `done` property indicates whether the iterator has completed.

# 🗁 The next() Method

The `next()` method retrieves the next value from an iterator.

Example:

```javascript
const numbers = [10, 20, 30];

const iterator = numbers.values();

console.log(iterator.next());
console.log(iterator.next());
console.log(iterator.next());
console.log(iterator.next());
```

Output:

```javascript
{ value: 10, done: false }
{ value: 20, done: false }
{ value: 30, done: false }
{ value: undefined, done: true }
```

Once an iterator is completed, subsequent calls normally continue returning completed iterator results.

# 🗁 The return() Method

An iterator may implement a `return()` method to indicate that iteration should finish.

This method is especially useful when iteration stops before the sequence is exhausted.

Example:

```javascript
const iterator = {
    index: 1,

    next() {
        if (this.index <= 5) {
            return {
                value: this.index++,
                done: false
            };
        }

        return {
            value: undefined,
            done: true
        };
    },

    return() {
        console.log("Iteration stopped");

        return {
            value: undefined,
            done: true
        };
    }
};

console.log(iterator.next());

console.log(iterator.return());

console.log(iterator.next());
```

Output:

```javascript
{ value: 1, done: false }
Iteration stopped
{ value: undefined, done: true }
{ value: 2, done: false }
```

The example demonstrates that implementing `return()` does not automatically change the iterator's internal state.

A well-designed iterator should update its state when `return()` is called.

```javascript
const iterator = {
    current: 1,
    finished: false,

    next() {
        if (this.finished || this.current > 5) {
            return {
                value: undefined,
                done: true
            };
        }

        return {
            value: this.current++,
            done: false
        };
    },

    return() {
        this.finished = true;

        return {
            value: undefined,
            done: true
        };
    }
};
```

# 🗁 The throw() Method

An iterator may implement a `throw()` method for receiving an exception from a consumer.

This method is particularly important for generator-based iterators.

Example:

```javascript
function* generateNumbers() {
    try {
        yield 10;
        yield 20;
    } catch (error) {
        yield `Error: ${error.message}`;
    }
}

const iterator = generateNumbers();

console.log(iterator.next());

console.log(
    iterator.throw(new Error("Invalid operation"))
);
```

Output:

```javascript
{ value: 10, done: false }
{ value: "Error: Invalid operation", done: false }
```

Unlike `next()`, `return()` and `throw()` are not required by the basic iterator protocol.

# 🗁 What Is an Iterable?

An **Iterable** is an object that implements the iterable protocol.

It must provide a method at the key `Symbol.iterator`.

That method must return an iterator.

Example:

```javascript
const collection = {
    values: [10, 20, 30],

    [Symbol.iterator]() {
        let index = 0;
        const values = this.values;

        return {
            next() {
                if (index < values.length) {
                    return {
                        value: values[index++],
                        done: false
                    };
                }

                return {
                    value: undefined,
                    done: true
                };
            }
        };
    }
};

for (const value of collection) {
    console.log(value);
}
```

Output:

```text
10
20
30
```

# 🗁 Iterable Protocol

The **Iterable Protocol** defines how an object provides an iterator.

An iterable must implement:

```javascript
object[Symbol.iterator]()
```

This method returns an iterator object.

Conceptual structure:

```text
        Iterable Object
               |
               v
      Symbol.iterator()
               |
               v
        Iterator Object
               |
               v
            next()
               |
               v
       Iterator Result
               |
          ┌────┴────┐
          v         v
        value     done
```

# 🗁 Iterator vs Iterable

| Feature | Iterator | Iterable |
| --- | --- | --- |
| Purpose | Produces values | Provides an iterator |
| Required method | `next()` | `[Symbol.iterator]()` |
| Returns | Iterator result | Iterator |
| Main protocol | Iterator protocol | Iterable protocol |
| Example | `array.values()` result | Array |

An object can be both an iterator and an iterable.

Generator objects are a common example.

# 🗁 Built-in Iterables

JavaScript provides several built-in iterable objects.

Common examples include:

* Arrays
* Strings
* Maps
* Sets
* Typed arrays
* Arguments objects
* NodeLists in modern browsers
* Generator objects

Example:

```javascript
const array = [10, 20, 30];
const string = "JavaScript";
const set = new Set([1, 2, 3]);
const map = new Map([
    ["name", "Alice"],
    ["age", 25]
]);

console.log([...array]);
console.log([...string]);
console.log([...set]);
console.log([...map]);
```

# 🗁 Symbol.iterator

`Symbol.iterator` is a well-known symbol used to define an object's default iterator.

Example:

```javascript
const numbers = [10, 20, 30];

const iterator = numbers[Symbol.iterator]();

console.log(iterator.next());
console.log(iterator.next());
console.log(iterator.next());
```

Output:

```javascript
{ value: 10, done: false }
{ value: 20, done: false }
{ value: 30, done: false }
```

The `Symbol.iterator` method is automatically used by language features such as `for...of` and spread syntax.

# 🗁 for...of Loop

The `for...of` loop consumes values from an iterable.

Example:

```javascript
const numbers = [10, 20, 30];

for (const number of numbers) {
    console.log(number);
}
```

Conceptually, it obtains an iterator and repeatedly calls `next()` until the iterator reports completion.

A `for...of` loop also supports early termination.

```javascript
for (const number of numbers) {
    if (number === 20) {
        break;
    }

    console.log(number);
}
```

When iteration is terminated early, the loop performs iterator closing by calling the iterator's `return()` method when available.

# 🗁 for...of vs for...in

Both loops can iterate over values or properties, but they work differently.

| Feature | `for...of` | `for...in` |
| --- | --- | --- |
| Purpose | Iterates iterable values | Iterates enumerable string property keys |
| Works with arrays | Yes | Yes |
| Returns array values | Yes | No |
| Returns object keys | No | Yes |
| Uses iterator protocol | Yes | No |
| Includes inherited enumerable properties | Depends on iterable | Yes |

Example:

```javascript
const numbers = [10, 20, 30];

for (const value of numbers) {
    console.log(value);
}

for (const key in numbers) {
    console.log(key);
}
```

Output:

```text
10
20
30

0
1
2
```

Use `for...of` when consuming iterable values.

Use `for...in` when enumerating object property keys.

# 🗁 Creating a Custom Iterator

A custom iterator allows developers to define how an object produces values.

Example:

```javascript
function createRange(start, end) {
    let current = start;

    return {
        next() {
            if (current <= end) {
                return {
                    value: current++,
                    done: false
                };
            }

            return {
                value: undefined,
                done: true
            };
        }
    };
}

const range = createRange(1, 5);

console.log(range.next());
console.log(range.next());
console.log(range.next());
```

This object is an iterator, but it is not yet iterable because it does not implement `Symbol.iterator`.

# 🗁 Creating an Iterable Iterator

An object can implement both protocols by returning itself from `Symbol.iterator`.

Example:

```javascript
function createRange(start, end) {
    let current = start;

    return {
        next() {
            if (current <= end) {
                return {
                    value: current++,
                    done: false
                };
            }

            return {
                value: undefined,
                done: true
            };
        },

        [Symbol.iterator]() {
            return this;
        }
    };
}

const range = createRange(1, 5);

for (const number of range) {
    console.log(number);
}
```

Output:

```text
1
2
3
4
5
```

The object now supports both manual iteration and `for...of`.

# 🗁 Creating Iterators with Generators

Generators provide a simpler way to create custom iterators.

Example:

```javascript
function* createRange(start, end) {
    for (let number = start; number <= end; number++) {
        yield number;
    }
}

const range = createRange(1, 5);

console.log([...range]);
```

Output:

```javascript
[1, 2, 3, 4, 5]
```

Generators automatically manage the iterator state and implement the required protocols.

# 🗁 Array Iterators

Arrays provide several iterator methods.

**values()**

Returns an iterator containing array values.

```javascript
const numbers = [10, 20, 30];

const iterator = numbers.values();

console.log(iterator.next());
```

**keys()**

Returns an iterator containing array indexes.

```javascript
const numbers = ["A", "B", "C"];

const iterator = numbers.keys();

console.log([...iterator]);
// [0, 1, 2]
```

**entries()**

Returns an iterator containing index-value pairs.

```javascript
const numbers = ["A", "B", "C"];

const iterator = numbers.entries();

console.log([...iterator]);
```

Output:

```javascript
[
    [0, "A"],
    [1, "B"],
    [2, "C"]
]
```

# 🗁 String Iterators

Strings are iterable.

Their default iterator produces Unicode code points rather than individual UTF-16 code units.

Example:

```javascript
const text = "Hello";

for (const character of text) {
    console.log(character);
}
```

Output:

```text
H
e
l
l
o
```

This is particularly useful for characters represented by surrogate pairs.

```javascript
const text = "A😀B";

console.log([...text]);
// ["A", "😀", "B"]
```

# 🗁 Map and Set Iterators

Maps and Sets provide iterable interfaces.

**Map Iterator**

```javascript
const user = new Map([
    ["name", "Alice"],
    ["age", 25]
]);

for (const [key, value] of user) {
    console.log(key, value);
}
```

**Set Iterator**

```javascript
const numbers = new Set([10, 20, 30]);

for (const number of numbers) {
    console.log(number);
}
```

Both structures also provide methods such as `keys()`, `values()`, and `entries()`.

# 🗁 Spread Syntax and Iterators

Spread syntax consumes an iterable and expands its values.

Example:

```javascript
const numbers = [10, 20, 30];

console.log([...numbers]);
```

It also works with other iterables.

```javascript
const text = "JavaScript";

console.log([...text]);
```

Spread syntax exhausts the iterator to obtain all values.

For infinite iterators, this can result in an endless operation.

# 🗁 Array Destructuring and Iterators

Array destructuring uses the iterable protocol to obtain values.

Example:

```javascript
const numbers = [10, 20, 30];

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

Destructuring can also stop before an iterable is exhausted.

```javascript
const [first, second] = numbers;

console.log(first, second);
```

The iterator is closed when destructuring stops early, if its `return()` method is available.

# 🗁 Iterator Closing

**Iterator Closing** is the process of notifying an iterator that the consumer has stopped before normal completion.

JavaScript performs iterator closing in several situations, including:

* `break` inside a `for...of` loop
* `return` from a function consuming an iterator
* Exceptions during iteration
* Certain destructuring operations that stop early

Example:

```javascript
const iterable = {
    [Symbol.iterator]() {
        let index = 1;

        return {
            next() {
                return {
                    value: index++,
                    done: false
                };
            },

            return() {
                console.log("Iterator closed");

                return {
                    value: undefined,
                    done: true
                };
            }
        };
    }
};

for (const value of iterable) {
    console.log(value);

    if (value === 3) {
        break;
    }
}
```

Output:

```text
1
2
3
Iterator closed
```

Iterator closing is useful for releasing resources associated with iteration.

# 🗁 Lazy Iteration

**Lazy Iteration** produces values only when they are requested.

Unlike eager operations that build an entire result collection, lazy iterators can generate values incrementally.

Example:

```javascript
function* generateNumbers(limit) {
    for (let number = 1; number <= limit; number++) {
        console.log(`Producing ${number}`);

        yield number;
    }
}

const numbers = generateNumbers(5);

console.log(numbers.next().value);
console.log(numbers.next().value);
```

Output:

```text
Producing 1
1
Producing 2
2
```

Only two values have been produced.

# 🗁 Infinite Iterators

An iterator can produce an unlimited sequence of values.

Example:

```javascript
function createInfiniteIterator() {
    let number = 1;

    return {
        next() {
            return {
                value: number++,
                done: false
            };
        },

        [Symbol.iterator]() {
            return this;
        }
    };
}

const numbers = createInfiniteIterator();

console.log(numbers.next().value); // 1
console.log(numbers.next().value); // 2
console.log(numbers.next().value); // 3
```

Infinite iterators should be consumed manually or with controlled iteration.

Do not pass an infinite iterator directly to operations that attempt to consume the entire sequence, such as array spread.

# 🗁 Iterator vs Generator

| Feature | Iterator | Generator |
| --- | --- | --- |
| Definition | Object implementing iterator protocol | Special function producing an iterator |
| Creation | Manual or built-in methods | `function*` |
| State management | Usually manual | Automatic |
| Value production | `next()` | `yield` |
| Execution suspension | Must be implemented manually | Built in |
| Complexity | Can be more verbose | Usually simpler |
| Iterable support | Must be implemented separately when needed | Automatic |

Generators are a convenient way to implement iterators, but not all iterators are generators.

# 🗁 Common Mistakes

**Assuming every object is iterable**

```javascript
const user = {
    name: "Alice"
};

// for (const value of user) {
//     console.log(value);
// }
```

Ordinary objects are not iterable by default.

**Confusing keys with values**

```javascript
const numbers = [10, 20, 30];

console.log([...numbers.keys()]);
// [0, 1, 2]
```

The `keys()` method returns indexes, not array values.

**Reusing an exhausted iterator**

```javascript
const iterator = [10, 20, 30].values();

console.log([...iterator]);
// [10, 20, 30]

console.log([...iterator]);
// []
```

Create a new iterator when the sequence needs to be consumed again.

**Treating an iterator as an array**

```javascript
const iterator = [10, 20, 30].values();

// iterator.map() is not available
```

An iterator does not automatically provide array methods such as `map()` and `filter()`.

**Consuming an infinite iterator completely**

```javascript
function* infiniteNumbers() {
    let number = 1;

    while (true) {
        yield number++;
    }
}

// [...infiniteNumbers()] never completes.
```

Use controlled iteration instead.

# 🗁 Best Practices

* Use `for...of` for consuming iterable values.
* Use `Symbol.iterator` to define custom iterable behavior.
* Use generators when manual iterator state management becomes complicated.
* Return valid iterator result objects from `next()`.
* Implement `return()` when early termination requires cleanup.
* Avoid sharing stateful iterators unintentionally.
* Remember that iterators are usually consumed only once.
* Use lazy iteration when processing large or potentially infinite sequences.
* Avoid converting large iterators into arrays unnecessarily.
* Test custom iterators with normal completion, early termination, and exceptions.

# 🗁 Key Takeaways

* An iterator produces values one at a time.
* The iterator protocol requires a `next()` method.
* The `next()` method returns an object containing `value` and `done`.
* An iterable provides an iterator through `Symbol.iterator`.
* An object can be both an iterator and an iterable.
* Arrays, strings, Maps, Sets, and generators are built-in iterable examples.
* `for...of`, spread syntax, and array destructuring consume iterables.
* Generators provide a convenient way to create custom iterators.
* Iterator closing allows consumers to signal early termination.
* Lazy and infinite iteration can reduce unnecessary data generation.

# 🗁 Related Topics

* Generators
* Async Iterators
* Async Generators
* Symbol.iterator
* Symbol
* Arrays
* Map
* Set
* for...of
* Spread Syntax
* Destructuring
* Lazy Evaluation
