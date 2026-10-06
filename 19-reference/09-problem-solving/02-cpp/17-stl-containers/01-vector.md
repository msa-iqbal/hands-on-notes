# Vector

Write a C++ program to demonstrate a `vector` and perform basic operations.

## Program

```cpp
#include <iostream>
#include <vector>
using namespace std;

int main() {
    vector<int> numbers = {10, 20, 30};

    numbers.push_back(40);
    numbers.push_back(50);

    cout << "Vector elements: ";

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
Vector elements: 10 20 30 40 50
Size: 5
First element: 10
Last element: 50
```
