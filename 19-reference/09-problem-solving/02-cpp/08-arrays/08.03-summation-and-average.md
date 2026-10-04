# Summation and Average of Array

Write a C++ program to calculate the sum and average of all elements in an array.

## Program

```cpp
#include <iostream>
#include <iomanip>
using namespace std;

int main() {
    int n;

    cout << "Enter array size: ";
    cin >> n;

    int arr[n];
    long long sum = 0;

    cout << "Enter " << n << " elements: ";

    for (int i = 0; i < n; i++) {
        cin >> arr[i];
        sum += arr[i];
    }

    double average = static_cast<double>(sum) / n;

    cout << "Sum = " << sum << endl;
    cout << fixed << setprecision(2);
    cout << "Average = " << average << endl;

    return 0;
}
```

## Sample Output

```text
Enter array size: 5
Enter 5 elements: 10 20 30 40 50
Sum = 150
Average = 30.00
```
