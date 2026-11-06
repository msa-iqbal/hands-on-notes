# Array

Write a C++ program to demonstrate the STL `array` container.

## Program

```cpp
#include <array>
#include <iostream>
using namespace std;

int main() {
    array<int, 5> numbers = {10, 20, 30, 40, 50};

    cout << "Array elements: ";

    for (int number : numbers) {
        cout << number << " ";
    }

    cout << endl;
    cout << "Size: " << numbers.size() << endl;
    cout << "First element: " << numbers.front() << endl;
    cout << "Last element: " << numbers.back() << endl;

    return 0;
}
```

## Sample Output

```text
Array elements: 10 20 30 40 50
Size: 5
First element: 10
Last element: 50
```
