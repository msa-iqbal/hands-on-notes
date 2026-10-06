# Upper and Lower Triangle Sum

Write a C++ program to calculate the sums of the upper and lower triangular parts of a square matrix.

## Program

```cpp
#include <iostream>
using namespace std;

int main() {
    int n;

    cout << "Enter matrix size: ";
    cin >> n;

    int matrix[n][n];

    cout << "Enter matrix elements:" << endl;

    for (int i = 0; i < n; i++) {
        for (int j = 0; j < n; j++) {
            cin >> matrix[i][j];
        }
    }

    int upperSum = 0;
    int lowerSum = 0;

    for (int i = 0; i < n; i++) {
        for (int j = 0; j < n; j++) {
            if (i <= j) {
                upperSum += matrix[i][j];
            }

            if (i >= j) {
                lowerSum += matrix[i][j];
            }
        }
    }

    cout << "Upper triangular sum = " << upperSum << endl;
    cout << "Lower triangular sum = " << lowerSum << endl;

    return 0;
}
```

## Sample Output

```text
Enter matrix size: 3
Enter matrix elements:
1 2 3
4 5 6
7 8 9
Upper triangular sum = 21
Lower triangular sum = 34
```
