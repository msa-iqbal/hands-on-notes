# Multiset

Write a C++ program to demonstrate an STL `multiset` that allows duplicate values.

## Program

```cpp
#include <iostream>
#include <set>
using namespace std;

int main() {
    multiset<int> numbers = {30, 10, 20, 10, 30};

    cout << "Multiset elements: ";

    for (int number : numbers) {
        cout << number << " ";
    }

    cout << endl;

    cout << "Number of elements: "
         << numbers.size() << endl;

    cout << "Count of 10: "
         << numbers.count(10) << endl;

    return 0;
}
```

## Sample Output

```text
Multiset elements: 10 10 20 30 30
Number of elements: 5
Count of 10: 2
```
