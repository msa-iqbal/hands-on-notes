# Linear Search

Write a C++ program to search for an element in an array using linear search.

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

    int value;

    cout << "Enter value to search: ";
    cin >> value;

    int position = -1;

    for (int i = 0; i < n; i++) {
        if (arr[i] == value) {
            position = i;
            break;
        }
    }

    if (position != -1) {
        cout << "Element found at position: " << position + 1 << endl;
    } else {
        cout << "Element not found." << endl;
    }

    return 0;
}
```

## Sample Output

```text
Enter array size: 5
Enter 5 elements: 10 20 30 40 50
Enter value to search: 30
Element found at position: 3
```
