# Forward List

Write a C++ program to demonstrate an STL `forward_list`.

## Program

```cpp
#include <forward_list>
#include <iostream>
using namespace std;

int main() {
    forward_list<int> numbers = {20, 30, 40};

    numbers.push_front(10);

    cout << "Forward list: ";

    for (int number : numbers) {
        cout << number << " ";
    }

    cout << endl;

    numbers.remove(30);

    cout << "After removing 30: ";

    for (int number : numbers) {
        cout << number << " ";
    }

    cout << endl;

    return 0;
}
```

## Sample Output

```text
Forward list: 10 20 30 40
After removing 30: 10 20 40
```
