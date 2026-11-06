# Transpose Matrix

Write a C++ program to find the transpose of a matrix.

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

    int matrix[rows][columns];

    cout << "Enter matrix elements:" << endl;

    for (int i = 0; i < rows; i++) {
        for (int j = 0; j < columns; j++) {
            cin >> matrix[i][j];
        }
    }

    cout << "Original matrix:" << endl;

    for (int i = 0; i < rows; i++) {
        for (int j = 0; j < columns; j++) {
            cout << matrix[i][j] << " ";
        }

        cout << endl;
    }

    cout << "Transpose matrix:" << endl;

    for (int j = 0; j < columns; j++) {
        for (int i = 0; i < rows; i++) {
            cout << matrix[i][j] << " ";
        }

        cout << endl;
    }

    return 0;
}
```

## Sample Output

```text
Enter number of rows: 2
Enter number of columns: 3
Enter matrix elements:
1 2 3
4 5 6
Original matrix:
1 2 3
4 5 6
Transpose matrix:
1 4
2 5
3 6
```
