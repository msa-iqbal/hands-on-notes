# Matrix Multiplication

Write a C++ program to multiply two matrices.

## Program

```cpp
#include <iostream>
using namespace std;

int main() {
    int rows1, columns1, rows2, columns2;

    cout << "Enter rows and columns of first matrix: ";
    cin >> rows1 >> columns1;

    cout << "Enter rows and columns of second matrix: ";
    cin >> rows2 >> columns2;

    if (columns1 != rows2) {
        cout << "Matrix multiplication is not possible." << endl;
        return 0;
    }

    int matrix1[rows1][columns1];
    int matrix2[rows2][columns2];
    int result[rows1][columns2];

    cout << "Enter first matrix:" << endl;

    for (int i = 0; i < rows1; i++) {
        for (int j = 0; j < columns1; j++) {
            cin >> matrix1[i][j];
        }
    }

    cout << "Enter second matrix:" << endl;

    for (int i = 0; i < rows2; i++) {
        for (int j = 0; j < columns2; j++) {
            cin >> matrix2[i][j];
        }
    }

    for (int i = 0; i < rows1; i++) {
        for (int j = 0; j < columns2; j++) {
            result[i][j] = 0;

            for (int k = 0; k < columns1; k++) {
                result[i][j] += matrix1[i][k] * matrix2[k][j];
            }
        }
    }

    cout << "Result of matrix multiplication:" << endl;

    for (int i = 0; i < rows1; i++) {
        for (int j = 0; j < columns2; j++) {
            cout << result[i][j] << " ";
        }

        cout << endl;
    }

    return 0;
}
```

## Sample Output

```text
Enter rows and columns of first matrix: 2 2
Enter rows and columns of second matrix: 2 2
Enter first matrix:
1 2
3 4
Enter second matrix:
5 6
7 8
Result of matrix multiplication:
19 22
43 50
```
