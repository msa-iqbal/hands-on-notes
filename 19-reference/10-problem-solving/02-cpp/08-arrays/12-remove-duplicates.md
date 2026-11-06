# Remove Duplicates from Array

Write a C++ program to remove duplicate elements from an array.

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

    int newSize = 0;

    for (int i = 0; i < n; i++) {
        bool duplicate = false;

        for (int j = 0; j < newSize; j++) {
            if (arr[i] == arr[j]) {
                duplicate = true;
                break;
            }
        }

        if (!duplicate) {
            arr[newSize] = arr[i];
            newSize++;
        }
    }

    cout << "Array after removing duplicates: ";

    for (int i = 0; i < newSize; i++) {
        cout << arr[i] << " ";
    }

    cout << endl;

    return 0;
}
```

## Sample Output

```text
Enter array size: 8
Enter 8 elements: 10 20 10 30 20 40 30 50
Array after removing duplicates: 10 20 30 40 50
```
