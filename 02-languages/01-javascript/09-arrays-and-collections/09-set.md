# JavaScript `Set`

A JavaScript `Set` is a built-in collection that stores unique values.

Unlike arrays, a `Set` does not allow duplicate values. If the same value is added multiple times, it is stored only once.

Sets are useful when working with collections that require uniqueness, such as:
s
* Removing duplicate values from arrays.
* Tracking unique user IDs.
* Managing selected items.
* Storing unique permissions.
* Tracking visited pages or nodes.
* Checking whether a value already exists.
* Performing mathematical set operations.

# 🗁 Creating a Set

### 🖆 Set Constructor

Create a Set using the `Set` constructor.

```javascript
const numbers = new Set();

console.log(numbers);
// Set(0) {}
```

A Set can also be initialized with an iterable, such as an array or string.

### 🖆 Creating a Set from an Array

```javascript
const numbers = new Set([1, 2, 3, 4, 5]);

console.log(numbers);
// Set(5) { 1, 2, 3, 4, 5 }
```

### 🖆 Creating a Set with Duplicate Values

Duplicate values are automatically removed.

```javascript
const numbers = new Set([1, 2, 2, 3, 3, 4]);

console.log(numbers);
// Set(4) { 1, 2, 3, 4 }
```

### 🖆 Creating a Set from a String

Strings are iterable, so each character becomes an element.

```javascript
const letters = new Set("javascript");

console.log(letters);
// Set(8) { 'j', 'a', 'v', 's', 'c', 'r', 'i', 'p', 't' }
```

Repeated characters are stored only once.

# 🗁 Set Properties and Methods

JavaScript provides several properties and methods for working with Sets.

| Property or method | Description |
| --- | --- |
| `size` | Returns the number of unique elements |
| `add()` | Adds a value |
| `has()` | Checks whether a value exists |
| `delete()` | Removes a value |
| `clear()` | Removes all values |
| `values()` | Returns an iterator of values |
| `keys()` | Returns an iterator of values |
| `entries()` | Returns an iterator of `[value, value]` pairs |
| `forEach()` | Executes a callback for each value |
| `Symbol.iterator` | Provides the default iterator |

# 🗁 Adding Elements with `add()`

The `add()` method inserts a value into a Set.

### 🖆 Basic Usage

```javascript
const languages = new Set();

languages.add("JavaScript");
languages.add("TypeScript");
languages.add("Python");

console.log(languages);
// Set(3) { 'JavaScript', 'TypeScript', 'Python' }
```

### 🖆 Adding Duplicate Values

Adding an existing value does not create a duplicate.

```javascript
const numbers = new Set();

numbers.add(10);
numbers.add(20);
numbers.add(10);

console.log(numbers);
// Set(2) { 10, 20 }
```

### 🖆 Chaining `add()`

The `add()` method returns the Set itself, allowing method chaining.

```javascript
const numbers = new Set();

numbers
    .add(10)
    .add(20)
    .add(30);

console.log(numbers);
// Set(3) { 10, 20, 30 }
```

# 🗁 Checking Elements with `has()`

The `has()` method checks whether a value exists in a Set.

It returns `true` if the value exists; otherwise, it returns `false`.

### 🖆 Basic Usage

```javascript
const fruits = new Set(["Apple", "Banana", "Mango"]);

console.log(fruits.has("Apple"));
// true

console.log(fruits.has("Orange"));
// false
```

### 🖆 Checking User Permissions

```javascript
const permissions = new Set([
    "read",
    "write",
    "delete"
]);

const canRead = permissions.has("read");
const canPublish = permissions.has("publish");

console.log(canRead);
// true

console.log(canPublish);
// false
```

### 🖆 Checking Unique IDs

```javascript
const registeredUsers = new Set([101, 102, 103]);

const userId = 102;

if (registeredUsers.has(userId)) {
    console.log("User already exists");
} else {
    console.log("New user");
}
```

# 🗁 Removing Elements

### 🖆 Using `delete()`

The `delete()` method removes a specific value.

It returns `true` if the value was found and removed, or `false` if it did not exist.

```javascript
const fruits = new Set(["Apple", "Banana", "Mango"]);

const deleted = fruits.delete("Banana");

console.log(deleted);
// true

console.log(fruits);
// Set(2) { 'Apple', 'Mango' }
```

Removing a nonexistent value:

```javascript
console.log(fruits.delete("Orange"));
// false
```

### 🖆 Using `clear()`

The `clear()` method removes every element from a Set.

```javascript
const numbers = new Set([10, 20, 30]);

numbers.clear();

console.log(numbers);
// Set(0) {}
```

# 🗁 Getting the Size of a Set

The `size` property returns the number of unique elements.

```javascript
const numbers = new Set([10, 20, 20, 30]);

console.log(numbers.size);
// 3
```

Unlike arrays, Sets use `size` instead of `length`.

```javascript
const fruits = new Set(["Apple", "Banana"]);

console.log(fruits.size);
// 2
```

# 🗁 Iterating Over a Set

Sets preserve insertion order. Iteration follows the order in which values were first inserted.

### 🖆 Using `for...of`

```javascript
const fruits = new Set(["Apple", "Banana", "Mango"]);

for (const fruit of fruits) {
    console.log(fruit);
}
```

Output:

```text
Apple
Banana
Mango
```

### 🖆 Using `forEach()`

```javascript
const numbers = new Set([10, 20, 30]);

numbers.forEach(number => {
    console.log(number);
});
```

The callback receives the value twice, followed by the Set itself.

```javascript
const numbers = new Set([10, 20, 30]);

numbers.forEach((value, key, set) => {
    console.log(value, key, set);
});
```

For a Set, `value` and `key` are identical.

This behavior maintains compatibility with the `forEach()` interface used by Maps.

### 🖆 Using `values()`

The `values()` method returns an iterator containing the Set's values.

```javascript
const fruits = new Set(["Apple", "Banana", "Mango"]);

const iterator = fruits.values();

console.log(iterator.next());
// { value: 'Apple', done: false }
```

### 🖆 Using `keys()`

For Sets, `keys()` behaves exactly like `values()`.

```javascript
const numbers = new Set([10, 20, 30]);

console.log([...numbers.keys()]);
// [10, 20, 30]
```

### 🖆 Using `entries()`

The `entries()` method returns key-value pairs.

Since Sets do not have separate keys and values, each pair contains the same value twice.

```javascript
const numbers = new Set([10, 20, 30]);

console.log([...numbers.entries()]);
// [[10, 10], [20, 20], [30, 30]]
```

### 🖆 Using the Spread Operator

Sets are iterable, so they can be converted into arrays.

```javascript
const fruits = new Set(["Apple", "Banana", "Mango"]);

const fruitArray = [...fruits];

console.log(fruitArray);
// ["Apple", "Banana", "Mango"]
```

### 🖆 Using `Array.from()`

```javascript
const numbers = new Set([10, 20, 30]);

const numberArray = Array.from(numbers);

console.log(numberArray);
// [10, 20, 30]
```

# 🗁 Understanding Value Uniqueness

A Set determines uniqueness using the SameValueZero equality algorithm.

This means:

* Duplicate primitive values are treated as equal.
* `NaN` is considered equal to another `NaN`.
* Positive zero and negative zero are considered equal.
* Objects are compared by reference, not by their contents.

### 🖆 Primitive Values

```javascript
const values = new Set([
    10,
    10,
    "JavaScript",
    "JavaScript",
    true,
    true
]);

console.log(values);
// Set(3) { 10, 'JavaScript', true }
```

### 🖆 `NaN` Values

```javascript
const numbers = new Set([NaN, NaN, 10]);

console.log(numbers.size);
// 2
```

Only one `NaN` is stored.

### 🖆 Positive and Negative Zero

```javascript
const numbers = new Set([0, -0]);

console.log(numbers.size);
// 1
```

### 🖆 Object References

Objects with identical properties are still considered different when they have different references.

```javascript
const users = new Set();

users.add({ id: 1, name: "Alice" });
users.add({ id: 1, name: "Alice" });

console.log(users.size);
// 2
```

Although both objects contain the same data, they are separate object instances.

### 🖆 Reusing the Same Object Reference

```javascript
const user = { id: 1, name: "Alice" };

const users = new Set();

users.add(user);
users.add(user);

console.log(users.size);
// 1
```

Both additions refer to the same object.

# 🗁 Removing Duplicates from Arrays

One of the most common uses of Set is removing duplicate values from an array.

### 🖆 Using the Spread Operator

```javascript
const numbers = [1, 2, 2, 3, 3, 4, 4, 5];

const uniqueNumbers = [...new Set(numbers)];

console.log(uniqueNumbers);
// [1, 2, 3, 4, 5]
```

### 🖆 Using `Array.from()`

```javascript
const fruits = [
    "Apple",
    "Banana",
    "Apple",
    "Mango",
    "Banana"
];

const uniqueFruits = Array.from(new Set(fruits));

console.log(uniqueFruits);
// ["Apple", "Banana", "Mango"]
```

### 🖆 Creating a Reusable Function

```javascript
function uniqueArray(array) {
    return [...new Set(array)];
}

console.log(uniqueArray([1, 2, 2, 3, 3]));
// [1, 2, 3]

console.log(uniqueArray(["a", "b", "a"]));
// ["a", "b"]
```

This approach removes duplicate primitive values and preserves their first insertion order.

# 🗁 Set Operations

A Set represents a collection of unique values. This makes it useful for mathematical set operations.

Modern JavaScript provides built-in methods for common operations.

Consider two Sets:

```javascript
const setA = new Set([1, 2, 3, 4]);
const setB = new Set([3, 4, 5, 6]);
```

### 🖆 Union

The union of two Sets contains every unique value found in either Set.

Use `union()`:

```javascript
const result = setA.union(setB);

console.log(result);
// Set(6) { 1, 2, 3, 4, 5, 6 }
```

### 🖆 Intersection

The intersection contains values that exist in both Sets.

Use `intersection()`:

```javascript
const result = setA.intersection(setB);

console.log(result);
// Set(2) { 3, 4 }
```

### 🖆 Difference

The difference contains values that exist in the first Set but not in the second.

Use `difference()`:

```javascript
const result = setA.difference(setB);

console.log(result);
// Set(2) { 1, 2 }
```

The reverse difference is:

```javascript
const result = setB.difference(setA);

console.log(result);
// Set(2) { 5, 6 }
```

### 🖆 Symmetric Difference

The symmetric difference contains values that exist in either Set, but not in both.

Use `symmetricDifference()`:

```javascript
const result = setA.symmetricDifference(setB);

console.log(result);
// Set(4) { 1, 2, 5, 6 }
```

### 🖆 Checking Subsets

A Set is a subset of another Set when every element of the first Set exists in the second.

Use `isSubsetOf()`:

```javascript
const setA = new Set([1, 2]);
const setB = new Set([1, 2, 3, 4]);

console.log(setA.isSubsetOf(setB));
// true
```

### 🖆 Checking Supersets

A Set is a superset of another Set when it contains every element of that Set.

Use `isSupersetOf()`:

```javascript
console.log(setB.isSupersetOf(setA));
// true
```

### 🖆 Checking Disjoint Sets

Two Sets are disjoint when they have no elements in common.

Use `isDisjointFrom()`:

```javascript
const setA = new Set([1, 2]);
const setB = new Set([3, 4]);

console.log(setA.isDisjointFrom(setB));
// true
```

These Set methods are available in modern JavaScript environments that implement the ECMAScript Set methods.

# 🗁 Practical Set Examples

### 🖆 Managing Unique User IDs

```javascript
const activeUsers = new Set();

activeUsers.add(101);
activeUsers.add(102);
activeUsers.add(103);

function activateUser(userId) {
    activeUsers.add(userId);
}

function deactivateUser(userId) {
    activeUsers.delete(userId);
}

function isUserActive(userId) {
    return activeUsers.has(userId);
}

activateUser(104);

console.log(isUserActive(104));
// true

deactivateUser(102);

console.log(isUserActive(102));
// false
```

### 🖆 Tracking Visited Pages

```javascript
const visitedPages = new Set();

function visitPage(path) {
    visitedPages.add(path);
}

visitPage("/home");
visitPage("/products");
visitPage("/about");
visitPage("/home");

console.log(visitedPages.size);
// 3
```

The homepage is stored only once.

### 🖆 Managing Selected Items

```javascript
const selectedItems = new Set();

function selectItem(id) {
    selectedItems.add(id);
}

function deselectItem(id) {
    selectedItems.delete(id);
}

function toggleItem(id) {
    if (selectedItems.has(id)) {
        selectedItems.delete(id);
    } else {
        selectedItems.add(id);
    }
}

selectItem(101);
selectItem(102);
toggleItem(101);

console.log(selectedItems);
// Set(1) { 102 }
```

### 🖆 Comparing User Permissions

```javascript
const requiredPermissions = new Set([
    "read",
    "write"
]);

const userPermissions = new Set([
    "read",
    "write",
    "delete"
]);

const hasRequiredPermissions =
    requiredPermissions.isSubsetOf(userPermissions);

console.log(hasRequiredPermissions);
// true
```

### 🖆 Finding Common Interests

```javascript
const userAInterests = new Set([
    "JavaScript",
    "React",
    "Node.js"
]);

const userBInterests = new Set([
    "React",
    "Node.js",
    "Python"
]);

const commonInterests = userAInterests.intersection(
    userBInterests
);

console.log([...commonInterests]);
// ["React", "Node.js"]
```

# 🗁 Set vs Array

Both arrays and Sets can store collections of values, but they serve different purposes.

| Feature | Array | Set |
| --- | --- | --- |
| Duplicate values | Allowed | Not allowed |
| Maintains insertion order | Yes | Yes |
| Index-based access | Yes | No |
| Direct membership method | `includes()` | `has()` |
| Number of elements | `length` | `size` |
| Add elements | `push()` | `add()` |
| Remove elements | `pop()`, `splice()`, etc. | `delete()` |
| Iteration | Supported | Supported |
| Direct sorting | `sort()` | No built-in `sort()` |
| Mathematical set operations | Manual implementation | Built-in methods |

### 🖆 When to Use an Array

Use an array when:

* Duplicate values are meaningful.
* You need index-based access.
* Element ordering is important.
* You need methods such as `map()`, `filter()`, and `reduce()`.
* You need to sort or manipulate elements by position.

### 🖆 When to Use a Set

Use a Set when:

* Every value must be unique.
* You frequently check whether a value exists.
* You need efficient membership checks.
* You need union, intersection, or difference operations.
* You want to track unique identifiers or selections.

# 🗁 Performance Considerations

* Set implementations generally provide average sublinear access time for membership operations, as required by the ECMAScript specification.
* `has()` is generally more efficient than repeatedly searching large arrays with `includes()`.
* Adding an existing value does not create a duplicate.
* Converting an array into a Set generally requires processing each element.
* Iterating through a Set takes time proportional to the number of elements.
* Set operations such as union and intersection are useful for working with unique collections.
* Avoid converting between arrays and Sets unnecessarily.

For example:

```javascript
const users = [101, 102, 103, 104];

const userSet = new Set(users);

console.log(userSet.has(103));
// true
```

If membership checks are frequent, maintaining a Set can be more suitable than repeatedly searching the original array.

# 🗁 Common Mistakes

### 🖆 Using `length` Instead of `size`

Incorrect:

```javascript
const numbers = new Set([1, 2, 3]);

console.log(numbers.length);
// undefined
```

Correct:

```javascript
console.log(numbers.size);
// 3
```

### 🖆 Expecting Index-Based Access

Sets do not support index-based access.

Incorrect:

```javascript
const numbers = new Set([10, 20, 30]);

console.log(numbers[0]);
// undefined
```

Convert the Set to an array when index-based access is required.

```javascript
const values = [...numbers];

console.log(values[0]);
// 10
```

### 🖆 Expecting Objects with Identical Properties to Be Duplicates

```javascript
const users = new Set([
    { id: 1 },
    { id: 1 }
]);

console.log(users.size);
// 2
```

Objects are compared by reference.

To enforce uniqueness by a property such as `id`, use an appropriate data-processing strategy.

```javascript
const users = [
    { id: 1, name: "Alice" },
    { id: 2, name: "Bob" },
    { id: 1, name: "Alice Updated" }
];

const uniqueUsers = [
    ...new Map(users.map(user => [user.id, user])).values()
];

console.log(uniqueUsers);
```

This example keeps the last object encountered for each ID.

### 🖆 Expecting `add()` to Return a Boolean

The `add()` method returns the Set itself, not a boolean.

```javascript
const numbers = new Set();

const result = numbers.add(10);

console.log(result === numbers);
// true
```

Use `has()` to check whether a value exists.

### 🖆 Expecting Set to Sort Values Automatically

A Set preserves insertion order; it does not automatically sort values.

```javascript
const numbers = new Set([30, 10, 20]);

console.log([...numbers]);
// [30, 10, 20]
```

Convert it to an array and sort when necessary.

```javascript
const sortedNumbers = [...numbers].sort((a, b) => a - b);

console.log(sortedNumbers);
// [10, 20, 30]
```

# 🗁 Best Practices

* Use Sets when uniqueness is a requirement.
* Use `has()` for membership checks.
* Use `size` to count elements.
* Use `delete()` to remove individual values.
* Use `clear()` when all elements need to be removed.
* Use the spread operator or `Array.from()` to convert Sets into arrays.
* Remember that objects are compared by reference.
* Use native Set operations for mathematical collection operations.
* Avoid using Sets when index-based access or duplicate values are required.
* Consider maintaining a Set for repeated membership checks against large collections.

# 🗁 Summary

A JavaScript `Set` is a collection of unique values that preserves insertion order.

Key points:

* Sets automatically prevent duplicate values.
* Sets can store primitive values and object references.
* `add()` inserts values.
* `has()` checks whether a value exists.
* `delete()` removes a specific value.
* `clear()` removes all values.
* `size` returns the number of unique elements.
* Sets support iteration using `for...of`, `forEach()`, and iterators.
* The spread operator converts a Set into an array.
* Modern Sets support union, intersection, difference, symmetric difference, and relationship checks.
* Objects are compared by reference rather than their contents.

**Remember:** Use an Array when you need an ordered, index-based collection that allows duplicates. Use a Set when you need a collection of unique values with efficient membership checks.
