# Matrix Subtraction

Write a C++ program to subtract one matrix from another matrix of the same dimensions.

## Program

```cpp
#include <iostream>
using namespace std;

int main() {
    int rows, columns;

    cout << "Enter number of rows: ";
    cin >> rows;

    cout << "Enter number of columns: ";
    cin >> columns;

    int matrix1[rows][columns];
    int matrix2[rows][columns];
    int result[rows][columns];

    cout << "Enter first matrix:" << endl;

    for (int i = 0; i < rows; i++) {
        for (int j = 0; j < columns; j++) {
            cin >> matrix1[i][j];
        }
    }

    cout << "Enter second matrix:" << endl;

    for (int i = 0; i < rows; i++) {
        for (int j = 0; j < columns; j++) {
            cin >> matrix2[i][j];
        }
    }

    for (int i = 0; i < rows; i++) {
        for (int j = 0; j < columns; j++) {
            result[i][j] = matrix1[i][j] - matrix2[i][j];
        }
    }

    cout << "Difference of matrices:" << endl;

    for (int i = 0; i < rows; i++) {
        for (int j = 0; j < columns; j++) {
            cout << result[i][j] << " ";
        }

        cout << endl;
    }

    return 0;
}
```

## Sample Output

```text
Enter number of rows: 2
Enter number of columns: 2
Enter first matrix:
10 20
30 40
Enter second matrix:
1 2
3 4
Difference of matrices:
9 18
27 36
```
