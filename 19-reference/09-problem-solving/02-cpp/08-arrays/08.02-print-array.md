# Print Array

Write a C++ program to print all elements of an array.

## Program

```cpp
#include <iostream>
using namespace std;

int main() {
    int n;

    cout << "Enter array size: ";
    cin >> n;

    int arr[n];

    cout << "Enter " << n << " elements: ";

    for (int i = 0; i < n; i++) {
        cin >> arr[i];
    }

    cout << "Array elements:" << endl;

    for (int i = 0; i < n; i++) {
        cout << "arr[" << i << "] = " << arr[i] << endl;
    }

    return 0;
}
```

## Sample Output

```text
Enter array size: 5
Enter 5 elements: 10 20 30 40 50
Array elements:
arr[0] = 10
arr[1] = 20
arr[2] = 30
arr[3] = 40
arr[4] = 50
```
