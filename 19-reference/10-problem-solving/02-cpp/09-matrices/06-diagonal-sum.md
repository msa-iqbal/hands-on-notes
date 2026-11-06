# Diagonal Sum of Matrix

Write a C++ program to calculate the sum of the main diagonal and secondary diagonal of a square matrix.

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

    int mainDiagonalSum = 0;
    int secondaryDiagonalSum = 0;

    for (int i = 0; i < n; i++) {
        mainDiagonalSum += matrix[i][i];
        secondaryDiagonalSum += matrix[i][n - i - 1];
    }

    cout << "Main diagonal sum = " << mainDiagonalSum << endl;
    cout << "Secondary diagonal sum = "
         << secondaryDiagonalSum << endl;

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
Main diagonal sum = 15
Secondary diagonal sum = 15
```
