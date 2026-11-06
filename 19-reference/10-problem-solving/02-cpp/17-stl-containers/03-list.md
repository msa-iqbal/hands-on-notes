# List

Write a C++ program to demonstrate an STL `list`.

## Program

```cpp
#include <iostream>
#include <list>
using namespace std;

int main() {
    list<int> numbers = {10, 20, 30};

    numbers.push_front(5);
    numbers.push_back(40);

    cout << "List elements: ";

    for (int number : numbers) {
        cout << number << " ";
    }

    cout << endl;

    numbers.pop_front();
    numbers.pop_back();

    cout << "After removing first and last: ";

    for (int number : numbers) {
        cout << number << " ";
    }

    cout << endl;

    return 0;
}
```

## Sample Output

```text
List elements: 5 10 20 30 40
After removing first and last: 10 20 30
```
