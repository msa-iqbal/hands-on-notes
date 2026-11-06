# Create Matrix

Write a C++ program to create a matrix by taking rows, columns, and elements as input, then display the matrix.

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

    cout << "Matrix:" << endl;

    for (int i = 0; i < rows; i++) {
        for (int j = 0; j < columns; j++) {
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
10 20 30
40 50 60
Matrix:
10 20 30
40 50 60
```
