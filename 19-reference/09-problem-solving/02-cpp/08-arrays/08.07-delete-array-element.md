# Delete Array Element

Write a C++ program to delete an element from an array at a specified position.

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

    int position;

    cout << "Enter position to delete (1-" << n << "): ";
    cin >> position;

    if (position < 1 || position > n) {
        cout << "Invalid position." << endl;
        return 0;
    }

    for (int i = position - 1; i < n - 1; i++) {
        arr[i] = arr[i + 1];
    }

    n--;

    cout << "Array after deletion: ";

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
Enter position to delete (1-5): 3
Array after deletion: 10 20 40 50
```
