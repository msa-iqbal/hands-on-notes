# Even or Odd

Write a C++ program to determine whether an integer is even or odd.

## Program

```cpp
#include <iostream>
using namespace std;

int main() {
    int number;

    cout << "Enter an integer: ";
    cin >> number;

    if (number % 2 == 0) {
        cout << number << " is even." << endl;
    } else {
        cout << number << " is odd." << endl;
    }

    return 0;
}
```

## Sample Output

```text
Enter an integer: 24
24 is even.
```
