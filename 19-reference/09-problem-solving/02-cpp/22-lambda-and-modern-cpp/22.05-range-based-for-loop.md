# Range-Based for Loop

A range-based `for` loop provides a simple way to iterate over arrays, containers, and other ranges.

## Basic Syntax

```cpp
for (type variable : collection) {
    // code
}
```

## Example 1: Array

```cpp
#include <iostream>
using namespace std;

int main() {
    int numbers[] = {10, 20, 30, 40, 50};

    for (int number : numbers) {
        cout << number << " ";
    }

    return 0;
}
```

### Output

```text
10 20 30 40 50
```

## Example 2: Vector

```cpp
#include <iostream>
#include <vector>
using namespace std;

int main() {
    vector<int> numbers = {10, 20, 30, 40};

    for (int number : numbers) {
        cout << number << " ";
    }

    return 0;
}
```

## Example 3: Using auto

```cpp
#include <iostream>
#include <vector>
using namespace std;

int main() {
    vector<int> numbers = {10, 20, 30};

    for (auto number : numbers) {
        cout << number << " ";
    }

    return 0;
}
```

## Example 4: Modify Elements With Reference

```cpp
#include <iostream>
#include <vector>
using namespace std;

int main() {
    vector<int> numbers = {1, 2, 3, 4, 5};

    for (auto& number : numbers) {
        number *= 2;
    }

    for (auto number : numbers) {
        cout << number << " ";
    }

    return 0;
}
```

### Output

```text
2 4 6 8 10
```

## Example 5: const Reference

Use `const auto&` when you want to avoid copying and do not want to modify elements.

```cpp
#include <iostream>
#include <vector>
using namespace std;

int main() {
    vector<string> names = {
        "Alice",
        "Bob",
        "Charlie"
    };

    for (const auto& name : names) {
        cout << name << endl;
    }

    return 0;
}
```

## Example 6: Map

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

    for (const auto& item : ages) {
        cout << item.first << " = " << item.second << endl;
    }

    return 0;
}
```

## Example 7: Character Array

```cpp
#include <iostream>
using namespace std;

int main() {
    char letters[] = {'A', 'B', 'C', 'D'};

    for (char letter : letters) {
        cout << letter << " ";
    }

    return 0;
}
```

## Example 8: Nested Range-Based Loop

```cpp
#include <iostream>
using namespace std;

int main() {
    int matrix[2][3] = {
        {1, 2, 3},
        {4, 5, 6}
    };

    for (const auto& row : matrix) {
        for (int value : row) {
            cout << value << " ";
        }

        cout << endl;
    }

    return 0;
}
```

### Output

```text
1 2 3
4 5 6
```

## Key Points

- Range-based `for` was introduced in C++11.
- It simplifies iteration.
- Use `auto` when appropriate.
- Use `auto&` to modify elements.
- Use `const auto&` to avoid copying while preventing modification.
