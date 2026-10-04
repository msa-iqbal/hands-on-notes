# Structured Bindings

**Structured bindings** allow multiple variables to be initialized from the elements of an object.

They were introduced in **C++17**.

## Basic Syntax

```cpp
auto [a, b] = object;
```

## Example 1: Pair

```cpp
#include <iostream>
#include <utility>
using namespace std;

int main() {
    pair<string, int> student = {"Alice", 20};

    auto [name, age] = student;

    cout << name << endl;
    cout << age << endl;

    return 0;
}
```

### Output

```text
Alice
20
```

## Example 2: Tuple

```cpp
#include <iostream>
#include <tuple>
using namespace std;

int main() {
    tuple<string, int, double> data = {
        "Alice",
        25,
        75.5
    };

    auto [name, age, score] = data;

    cout << name << endl;
    cout << age << endl;
    cout << score << endl;

    return 0;
}
```

## Example 3: Array

```cpp
#include <iostream>
using namespace std;

int main() {
    int values[3] = {10, 20, 30};

    auto [a, b, c] = values;

    cout << a << " ";
    cout << b << " ";
    cout << c << endl;

    return 0;
}
```

### Output

```text
10 20 30
```

## Example 4: Map

Structured bindings are particularly useful with maps.

```cpp
#include <iostream>
#include <map>
using namespace std;

int main() {
    map<string, int> ages = {
        {"Alice", 20},
        {"Bob", 25},
        {"Charlie", 30}
    };

    for (const auto& [name, age] : ages) {
        cout << name << " = " << age << endl;
    }

    return 0;
}
```

## Example 5: Modify Map Values

```cpp
#include <iostream>
#include <map>
using namespace std;

int main() {
    map<string, int> scores = {
        {"Alice", 80},
        {"Bob", 70}
    };

    for (auto& [name, score] : scores) {
        score += 10;
    }

    for (const auto& [name, score] : scores) {
        cout << name << " = " << score << endl;
    }

    return 0;
}
```

### Output

```text
Alice = 90
Bob = 80
```

## Example 6: References

```cpp
#include <iostream>
#include <utility>
using namespace std;

int main() {
    pair<string, int> data = {"Alice", 20};

    auto& [name, age] = data;

    age = 25;

    cout << data.second << endl;

    return 0;
}
```

### Output

```text
25
```

## Example 7: const Structured Binding

```cpp
#include <iostream>
#include <utility>
using namespace std;

int main() {
    const pair<int, int> coordinates = {10, 20};

    const auto& [x, y] = coordinates;

    cout << x << ", " << y << endl;

    return 0;
}
```

## Key Points

- Structured bindings require C++17 or later.
- They simplify working with `pair`, `tuple`, arrays, and map entries.
- `auto [a, b]` creates bindings.
- `auto& [a, b]` creates reference bindings.
- They are especially useful with range-based loops.
