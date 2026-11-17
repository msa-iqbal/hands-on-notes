# JavaScript for...of Loop

The **`for...of` loop** is an iteration statement used to iterate over the values of an iterable object.

It provides a simple and readable way to access individual elements without manually managing an index or counter.

JavaScript supports `for...of` with many built-in iterable objects, including:

* Arrays
* Strings
* Maps
* Sets
* Typed arrays
* Generators
* Other objects implementing the iterable protocol

**Example**

```javascript
const fruits = ["Apple", "Banana", "Orange"];

for (const fruit of fruits) {
    console.log(fruit);
}
```

**Output**

```text
Apple
Banana
Orange
```

During each iteration, `fruit` receives the next value from the array.

# 🗁 Syntax

The basic syntax is:

```javascript
for (variable of iterable) {
    // Code to execute for each value
}
```

Where:

* `variable` receives the current value from the iterable.
* `iterable` is an object that provides an iterator.
* The loop body executes once for each value.

You can declare the iteration variable using `const`, `let`, or `var`.

**Using const**

```javascript
const numbers = [10, 20, 30];

for (const number of numbers) {
    console.log(number);
}
```

**Using let**

```javascript
const numbers = [10, 20, 30];

for (let number of numbers) {
    number *= 2;
    console.log(number);
}
```

**Output**

```text
20
40
60
```

Using `const` is generally preferred when you do not need to reassign the iteration variable.

# 🗁 How for...of Works

The `for...of` loop uses JavaScript's iterable protocol.

An iterable object provides a method identified by `Symbol.iterator`.

That method returns an iterator object, which provides a `next()` method.

Each call to `next()` returns an iterator result containing:

* `value`: The current yielded value.
* `done`: A boolean indicating whether iteration has completed.

**Conceptual iterator result**

```javascript
{
    value: "Apple",
    done: false
}
```

When iteration finishes:

```javascript
{
    value: undefined,
    done: true
}
```

The `for...of` loop handles this process automatically.

**Example**

```javascript
const numbers = [10, 20, 30];

const iterator = numbers[Symbol.iterator]();

console.log(iterator.next());
console.log(iterator.next());
console.log(iterator.next());
console.log(iterator.next());
```

**Output**

```javascript
{ value: 10, done: false }
{ value: 20, done: false }
{ value: 30, done: false }
{ value: undefined, done: true }
```

The `for...of` loop provides a convenient abstraction over this iterator mechanism.

# 🗁 Iterating Over Arrays

Arrays are iterable, so `for...of` can access their elements directly.

**Example**

```javascript
const numbers = [10, 20, 30, 40, 50];

for (const number of numbers) {
    console.log(number);
}
```

**Output**

```text
10
20
30
40
50
```

**Calculate the sum of array elements**

```javascript
const numbers = [10, 20, 30, 40, 50];

let sum = 0;

for (const number of numbers) {
    sum += number;
}

console.log(sum); // 150
```

**Find the largest number**

```javascript
const numbers = [15, 42, 8, 64, 23];

let largest = numbers[0];

for (const number of numbers) {
    if (number > largest) {
        largest = number;
    }
}

console.log(largest); // 64
```

**Iterating over an array of objects**

```javascript
const users = [
    { id: 1, name: "John" },
    { id: 2, name: "Alice" },
    { id: 3, name: "David" }
];

for (const user of users) {
    console.log(user.id, user.name);
}
```

**Output**

```text
1 John
2 Alice
3 David
```

# 🗁 Iterating Over Strings

Strings are iterable in JavaScript.

A `for...of` loop iterates over Unicode code points rather than individual UTF-16 code units.

This makes it more suitable than index-based iteration for many Unicode characters.

**Example**

```javascript
const language = "JavaScript";

for (const character of language) {
    console.log(character);
}
```

**Output**

```text
J
a
v
a
S
c
r
i
p
t
```

**Iterating over Unicode characters**

```javascript
const text = "A😀B";

for (const character of text) {
    console.log(character);
}
```

**Output**

```text
A
😀
B
```

The emoji is processed as one code point even though it occupies two UTF-16 code units.

# 🗁 Iterating Over Sets

A `Set` stores unique values and is iterable.

The `for...of` loop accesses each value in insertion order.

**Example**

```javascript
const numbers = new Set([10, 20, 30, 20, 40]);

for (const number of numbers) {
    console.log(number);
}
```

**Output**

```text
10
20
30
40
```

Duplicate values are automatically removed by the `Set`.

**Practical example**

```javascript
const roles = new Set([
    "admin",
    "editor",
    "viewer",
    "admin"
]);

for (const role of roles) {
    console.log(role);
}
```

**Output**

```text
admin
editor
viewer
```

# 🗁 Iterating Over Maps

A `Map` stores key-value pairs and is iterable.

By default, iterating over a `Map` using `for...of` returns an array containing the key and value for each entry.

**Example**

```javascript
const user = new Map([
    ["name", "John"],
    ["age", 25],
    ["city", "London"]
]);

for (const entry of user) {
    console.log(entry);
}
```

**Output**

```javascript
["name", "John"]
["age", 25]
["city", "London"]
```

**Using array destructuring**

You can directly extract keys and values using destructuring.

```javascript
const user = new Map([
    ["name", "John"],
    ["age", 25],
    ["city", "London"]
]);

for (const [key, value] of user) {
    console.log(`${key}: ${value}`);
}
```

**Output**

```text
name: John
age: 25
city: London
```

**Iterating over keys**

```javascript
const user = new Map([
    ["name", "John"],
    ["age", 25]
]);

for (const key of user.keys()) {
    console.log(key);
}
```

**Output**

```text
name
age
```

**Iterating over values**

```javascript
for (const value of user.values()) {
    console.log(value);
}
```

**Output**

```text
John
25
```

# 🗁 Iterating Over Typed Arrays

Typed arrays are iterable objects designed to work with binary numeric data.

Examples include:

* `Int8Array`
* `Uint8Array`
* `Int16Array`
* `Uint16Array`
* `Int32Array`
* `Uint32Array`
* `Float32Array`
* `Float64Array`
* `BigInt64Array`
* `BigUint64Array`

**Example**

```javascript
const numbers = new Int8Array([10, 20, 30, 40]);

for (const number of numbers) {
    console.log(number);
}
```

**Output**

```text
10
20
30
40
```

Typed arrays are useful when working with binary protocols, files, network data, and other low-level data structures.

# 🗁 Iterating Over Generators

Generator functions produce iterable sequences of values using the `yield` keyword.

A generator function returns a generator object, which can be consumed using `for...of`.

**Example**

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

**Output**

```text
10
20
30
```

The loop requests values from the generator until it finishes.

**Generating a sequence dynamically**

```javascript
function* countUpTo(limit) {
    for (let i = 1; i <= limit; i++) {
        yield i;
    }
}

for (const number of countUpTo(5)) {
    console.log(number);
}
```

**Output**

```text
1
2
3
4
5
```

# 🗁 Iterating Over Custom Iterables

JavaScript allows objects to become iterable by implementing the iterable protocol through `Symbol.iterator`.

**Example**

```javascript
const range = {
    start: 1,
    end: 5,

    [Symbol.iterator]() {
        let current = this.start;
        const end = this.end;

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
};

for (const number of range) {
    console.log(number);
}
```

**Output**

```text
1
2
3
4
5
```

The custom object is iterable because it implements `Symbol.iterator` and returns an iterator with a `next()` method.

# 🗁 Accessing Indexes with entries()

The `for...of` loop provides values directly, not indexes.

When both an index and a value are required, use the array's `entries()` method.

**Example**

```javascript
const fruits = ["Apple", "Banana", "Orange"];

for (const [index, fruit] of fruits.entries()) {
    console.log(index, fruit);
}
```

**Output**

```text
0 Apple
1 Banana
2 Orange
```

`entries()` returns an iterable iterator that yields index-value pairs.

**Practical example**

```javascript
const products = ["Laptop", "Mouse", "Keyboard"];

for (const [index, product] of products.entries()) {
    console.log(`Product ${index + 1}: ${product}`);
}
```

**Output**

```text
Product 1: Laptop
Product 2: Mouse
Product 3: Keyboard
```

# 🗁 Using break and continue

The `for...of` loop supports both `break` and `continue`.

**Using break**

The `break` statement terminates the loop.

```javascript
const numbers = [10, 20, 30, 40, 50];

for (const number of numbers) {
    if (number === 30) {
        break;
    }

    console.log(number);
}
```

**Output**

```text
10
20
```

**Using continue**

The `continue` statement skips the current iteration.

```javascript
const numbers = [10, 20, 30, 40, 50];

for (const number of numbers) {
    if (number === 30) {
        continue;
    }

    console.log(number);
}
```

**Output**

```text
10
20
40
50
```

# 🗁 Labeled for...of Loops

Labels can be used with `for...of` loops to control nested iteration.

**Example**

```javascript
const groups = [
    [1, 2],
    [3, 4],
    [5, 6]
];

outerLoop:
for (const group of groups) {
    for (const number of group) {
        if (number === 4) {
            continue outerLoop;
        }

        console.log(number);
    }
}
```

**Output**

```text
1
2
3
5
6
```

When the value `4` is encountered, the current inner iteration and the remaining values in that group are skipped. Execution continues with the next outer iteration.

# 🗁 for...of vs Traditional for Loop

Both loops can iterate over arrays, but they provide different approaches.

**Traditional for loop**

```javascript
const fruits = ["Apple", "Banana", "Orange"];

for (let i = 0; i < fruits.length; i++) {
    console.log(fruits[i]);
}
```

**for...of loop**

```javascript
const fruits = ["Apple", "Banana", "Orange"];

for (const fruit of fruits) {
    console.log(fruit);
}
```

| Feature | Traditional for | for...of |
| --- | --- | --- |
| Iteration | Usually index-based | Value-based |
| Index access | Directly available | Use `entries()` when needed |
| Counter management | Manual | Automatic |
| Iterable support | Requires index-based access or other logic | Works with iterables |
| Readability | Useful for index-dependent logic | Usually simpler for value iteration |
| Custom iteration | Manual implementation | Uses iterator protocol |

Use a traditional `for` loop when you need precise control over indexes, increments, or iteration conditions.

Use `for...of` when you primarily need to process values.

# 🗁 for...of vs for...in

These loops serve different purposes.

| Feature | for...of | for...in |
| --- | --- | --- |
| Iterates over | Iterable values | Enumerable property keys |
| Common use | Arrays, strings, Maps, Sets | Object properties |
| Array result | Element values | Index keys as strings |
| Uses | `Symbol.iterator` | Property enumeration |
| Works with plain objects by default | No | Yes |

**Using for...of**

```javascript
const fruits = ["Apple", "Banana", "Orange"];

for (const fruit of fruits) {
    console.log(fruit);
}
```

**Output**

```text
Apple
Banana
Orange
```

**Using for...in**

```javascript
const fruits = ["Apple", "Banana", "Orange"];

for (const index in fruits) {
    console.log(index);
}
```

**Output**

```text
0
1
2
```

Remember:

* `for...of` iterates over values.
* `for...in` iterates over enumerable property keys.

# 🗁 Common Mistakes

**Using for...of with a plain object**

```javascript
const user = {
    name: "John",
    age: 25
};

for (const value of user) {
    console.log(value);
}
```

This produces a `TypeError` because ordinary objects do not implement the iterable protocol by default.

To iterate over object values, use `Object.values()`.

```javascript
for (const value of Object.values(user)) {
    console.log(value);
}
```

**Reassigning a const iteration variable**

```javascript
const numbers = [10, 20, 30];

for (const number of numbers) {
    number = number * 2;
}
```

This produces a `TypeError` because a `const` variable cannot be reassigned.

Use `let` if reassignment is required.

```javascript
for (let number of numbers) {
    number *= 2;
    console.log(number);
}
```

**Assuming for...of provides indexes**

```javascript
const fruits = ["Apple", "Banana", "Orange"];

for (const fruit of fruits) {
    console.log(fruit.index);
}
```

This does not provide the array index. Use `entries()` when you need both the index and value.

**Modifying an array during iteration**

```javascript
const numbers = [1, 2, 3, 4];

for (const number of numbers) {
    if (number === 2) {
        numbers.splice(1, 1);
    }

    console.log(number);
}
```

Modifying an array while iterating can affect which elements are visited because the iterator observes the array's changing contents.

Prefer creating a new array with methods such as `filter()` when removing elements.

```javascript
const numbers = [1, 2, 3, 4];

const filteredNumbers = numbers.filter(number => number !== 2);

console.log(filteredNumbers); // [1, 3, 4]
```

**Assuming for...of works with every object**

An object must be iterable to work with `for...of`.

For ordinary objects, use `Object.keys()`, `Object.values()`, or `Object.entries()`.

# 🗁 Best Practices

* Prefer `for...of` when processing iterable values.
* Use `const` when the iteration variable does not need reassignment.
* Use `entries()` when both indexes and values are required.
* Use `Map` and `Set` directly with `for...of`.
* Use `Object.entries()` when iterating over an object's key-value pairs.
* Avoid modifying collections during iteration unless the behavior is intentional.
* Use `break` and `continue` when they make iteration logic clearer.
* Prefer traditional `for` loops when index-based control is necessary.
* Understand the iterable protocol when implementing custom iterators.
* Use generators when values should be produced lazily.

# 🗁 Key Takeaways

* `for...of` iterates over values from iterable objects.
* It does not require manual index or counter management.
* Arrays, strings, Maps, Sets, typed arrays, and generators support iteration.
* It uses the iterable protocol through `Symbol.iterator`.
* The iterator protocol provides values through `next()`.
* Use `entries()` to access both indexes and values.
* Use `break` to terminate iteration and `continue` to skip an iteration.
* Plain objects are not iterable by default.
* `for...of` is generally preferred for straightforward value-based iteration.
