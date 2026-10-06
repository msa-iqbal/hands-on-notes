# Unordered Set

Write a C++ program to demonstrate an STL `unordered_set`.

## Program

```cpp
#include <iostream>
#include <unordered_set>
using namespace std;

int main() {
    unordered_set<int> numbers = {
        10, 20, 30, 20, 10
    };

    cout << "Unordered set elements: ";

    for (int number : numbers) {
        cout << number << " ";
    }

    cout << endl;

    cout << "Number of elements: "
         << numbers.size() << endl;

    return 0;
}
```

## Sample Output

```text
Unordered set elements: 30 20 10
Number of elements: 3
```

> The iteration order of an `unordered_set` is not guaranteed.
