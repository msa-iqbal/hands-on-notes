# Reference Variable

Write a C++ program to create a reference variable and use it to access and modify the original variable.

## Program

```cpp
#include <iostream>
using namespace std;

int main() {
    int number = 25;

    int& reference = number;

    cout << "Original value: " << number << endl;
    cout << "Reference value: " << reference << endl;

    reference = 50;

    cout << "Value after modification: " << number << endl;

    return 0;
}
```

## Sample Output

```text
Original value: 25
Reference value: 25
Value after modification: 50
```
