# Hash Table

A **hash table** stores data using a hash function that maps keys to positions.

C++ provides hash-table-based containers such as:

- `unordered_map`

- `unordered_set`

Average lookup, insertion, and deletion are typically **O(1)**, while worst-case complexity can be **O(n)**.

## 1. `unordered_map`

```cpp
#include <iostream>
#include <unordered_map>
using namespace std;

int main() {
    unordered_map<string, int> ages;

    ages["Alice"] = 25;
    ages["Bob"] = 30;
    ages["Charlie"] = 28;

    cout << ages["Alice"] << endl;

    return 0;
}
```

### Output

```text
25
```

## 2. Insert with `insert()`

```cpp
unordered_map<string, int> scores;

scores.insert({"Alice", 90});
scores.insert({"Bob", 85});
scores.insert({"Charlie", 95});
```

## 3. Search with `find()`

```cpp
auto result = scores.find("Alice");

if (result != scores.end()) {
    cout << "Found: " << result->second << endl;
} else {
    cout << "Not Found" << endl;
}
```

### Output

```text
Found: 90
```

## 4. Check with `count()`

```cpp
if (scores.count("Bob")) {
    cout << "Bob exists" << endl;
}
```

### Output

```text
Bob exists
```

## 5. Erase an Element

```cpp
scores.erase("Bob");
```

## 6. Iterate Through a Hash Table

```cpp
#include <iostream>
#include <unordered_map>
using namespace std;

int main() {
    unordered_map<string, int> scores;

    scores["Alice"] = 90;
    scores["Bob"] = 85;
    scores["Charlie"] = 95;

    for (const auto& item : scores) {
        cout << item.first << ": "
             << item.second << endl;
    }

    return 0;
}
```

The iteration order of an `unordered_map` is not guaranteed to be sorted.

## 7. `unordered_set`

```cpp
#include <iostream>
#include <unordered_set>
using namespace std;

int main() {
    unordered_set<int> numbers;

    numbers.insert(10);
    numbers.insert(20);
    numbers.insert(30);
    numbers.insert(20);

    for (int number : numbers) {
        cout << number << " ";
    }

    return 0;
}
```

Duplicate values are not stored.

## 8. Hash Collisions

A **collision** occurs when multiple keys map to the same hash-table bucket.

Conceptually:

```text
key A ──┐
        ├──> bucket 5
key B ──┘
```

Hash-table implementations handle collisions internally.

## `unordered_map` vs `map`

|Feature|`unordered_map`|`map`|
|---|---|---|
|Ordering|Unordered|Ordered by key|
|Typical lookup|O(1) average|O(log n)|
|Worst-case lookup|O(n)|O(log n)|
|Main structure|Hash table|Typically balanced tree|
