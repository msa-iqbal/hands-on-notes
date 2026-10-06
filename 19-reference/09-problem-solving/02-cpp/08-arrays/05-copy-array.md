# Copy Array

Write a C++ program to copy all elements from one array into another array.

## Program

```cpp
#include <iostream>
using namespace std;

int main() {
    int n;

    cout << "Enter array size: ";
    cin >> n;

    int source[n];
    int destination[n];

    cout << "Enter " << n << " elements: ";

    for (int i = 0; i < n; i++) {
        cin >> source[i];
    }

    for (int i = 0; i < n; i++) {
        destination[i] = source[i];
    }

    cout << "Original array: ";

    for (int i = 0; i < n; i++) {
        cout << source[i] << " ";
    }

    cout << endl;

    cout << "Copied array: ";

    for (int i = 0; i < n; i++) {
        cout << destination[i] << " ";
    }

    cout << endl;

    return 0;
}
```

## Sample Output

```text
Enter array size: 5
Enter 5 elements: 10 20 30 40 50
Original array: 10 20 30 40 50
Copied array: 10 20 30 40 50
```
