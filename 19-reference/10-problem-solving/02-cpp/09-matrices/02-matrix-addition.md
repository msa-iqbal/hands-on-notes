# Matrix Addition

Write a C++ program to add two matrices of the same dimensions.

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
            result[i][j] = matrix1[i][j] + matrix2[i][j];
        }
    }

    cout << "Sum of matrices:" << endl;

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
1 2
3 4
Enter second matrix:
5 6
7 8
Sum of matrices:
6 8
10 12
```
