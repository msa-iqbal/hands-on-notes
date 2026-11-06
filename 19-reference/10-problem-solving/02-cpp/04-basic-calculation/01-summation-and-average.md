# Summation and Average

Write a C++ program to input several numbers, calculate their sum, and find their average.

## Program

```cpp
#include <iostream>
using namespace std;

int main() {
    int n;
    double number, sum = 0;

    cout << "Enter the number of values: ";
    cin >> n;

    if (n <= 0) {
        cout << "Number of values must be positive." << endl;
        return 0;
    }

    for (int i = 1; i <= n; i++) {
        cout << "Enter value " << i << ": ";
        cin >> number;
        sum += number;
    }

    double average = sum / n;

    cout << "Sum = " << sum << endl;
    cout << "Average = " << average << endl;

    return 0;
}
```

## Sample Output

```text
Enter the number of values: 5
Enter value 1: 10
Enter value 2: 20
Enter value 3: 30
Enter value 4: 40
Enter value 5: 50
Sum = 150
Average = 30
```
