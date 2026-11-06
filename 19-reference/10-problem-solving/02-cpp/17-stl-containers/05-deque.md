# Deque

Write a C++ program to demonstrate an STL `deque`.

## Program

```cpp
#include <deque>
#include <iostream>
using namespace std;

int main() {
    deque<int> numbers;

    numbers.push_back(20);
    numbers.push_back(30);

    numbers.push_front(10);
    numbers.push_front(5);

    cout << "Deque elements: ";

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
Deque elements: 5 10 20 30
After removing first and last: 10 20
```
