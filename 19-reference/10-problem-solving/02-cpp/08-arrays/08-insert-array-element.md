# Insert Array Element

Write a C++ program to insert an element into an array at a specified position.

## Program

```cpp
#include <iostream>
using namespace std;

int main() {
    int n;

    cout << "Enter array size: ";
    cin >> n;

    int arr[n + 1];

    cout << "Enter " << n << " elements: ";

    for (int i = 0; i < n; i++) {
        cin >> arr[i];
    }

    int position;
    int value;

    cout << "Enter position to insert (1-" << n + 1 << "): ";
    cin >> position;

    cout << "Enter value: ";
    cin >> value;

    if (position < 1 || position > n + 1) {
        cout << "Invalid position." << endl;
        return 0;
    }

    for (int i = n; i >= position; i--) {
        arr[i] = arr[i - 1];
    }

    arr[position - 1] = value;
    n++;

    cout << "Array after insertion: ";

    for (int i = 0; i < n; i++) {
        cout << arr[i] << " ";
    }

    cout << endl;

    return 0;
}
```

## Sample Output

```text
Enter array size: 5
Enter 5 elements: 10 20 30 40 50
Enter position to insert (1-6): 3
Enter value: 25
Array after insertion: 10 20 25 30 40 50
```
