# Set

Write a C++ program to demonstrate an STL `set` that stores unique sorted values.

## Program

```cpp
#include <iostream>
#include <set>
using namespace std;

int main() {
    set<int> numbers = {30, 10, 20, 10, 30};

    cout << "Set elements: ";

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
Set elements: 10 20 30
Number of elements: 3
```
