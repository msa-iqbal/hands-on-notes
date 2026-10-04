# Passing Array to Function

Write a C++ program to pass an array to a function and calculate its sum.

## Program

```cpp
#include <iostream>
using namespace std;

int arraySum(int arr[], int size) {
    int sum = 0;

    for (int i = 0; i < size; i++) {
        sum += arr[i];
    }

    return sum;
}

int main() {
    int n;

    cout << "Enter array size: ";
    cin >> n;

    int arr[n];

    cout << "Enter " << n << " elements: ";

    for (int i = 0; i < n; i++) {
        cin >> arr[i];
    }

    cout << "Sum = " << arraySum(arr, n) << endl;

    return 0;
}
```

## Sample Output

```text
Enter array size: 5
Enter 5 elements: 10 20 30 40 50
Sum = 150
```
