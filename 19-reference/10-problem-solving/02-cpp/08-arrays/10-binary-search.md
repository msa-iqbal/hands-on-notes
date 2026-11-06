# Binary Search

Write a C++ program to search for an element in a sorted array using binary search.

## Program

```cpp
#include <iostream>
using namespace std;

int main() {
    int n;

    cout << "Enter array size: ";
    cin >> n;

    int arr[n];

    cout << "Enter " << n << " sorted elements: ";

    for (int i = 0; i < n; i++) {
        cin >> arr[i];
    }

    int value;

    cout << "Enter value to search: ";
    cin >> value;

    int left = 0;
    int right = n - 1;
    int position = -1;

    while (left <= right) {
        int middle = left + (right - left) / 2;

        if (arr[middle] == value) {
            position = middle;
            break;
        } else if (arr[middle] < value) {
            left = middle + 1;
        } else {
            right = middle - 1;
        }
    }

    if (position != -1) {
        cout << "Element found at position: "
             << position + 1 << endl;
    } else {
        cout << "Element not found." << endl;
    }

    return 0;
}
```

## Sample Output

```text
Enter array size: 5
Enter 5 sorted elements: 10 20 30 40 50
Enter value to search: 40
Element found at position: 4
```
