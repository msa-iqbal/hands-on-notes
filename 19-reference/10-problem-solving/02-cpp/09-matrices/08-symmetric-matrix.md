# Symmetric Matrix

Write a C++ program to check whether a square matrix is symmetric.

A matrix is symmetric when:

```text
matrix[i][j] == matrix[j][i]
```

for every valid pair of indices.

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

    bool symmetric = true;

    for (int i = 0; i < n; i++) {
        for (int j = 0; j < n; j++) {
            if (matrix[i][j] != matrix[j][i]) {
                symmetric = false;
                break;
            }
        }

        if (!symmetric) {
            break;
        }
    }

    if (symmetric) {
        cout << "The matrix is symmetric." << endl;
    } else {
        cout << "The matrix is not symmetric." << endl;
    }

    return 0;
}
```

## Sample Output

```text
Enter matrix size: 3
Enter matrix elements:
1 2 3
2 4 5
3 5 6
The matrix is symmetric.
```
