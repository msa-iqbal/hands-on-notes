# Sum of Diagonal Elements

## Problem

Write a C program to find the sum of the main diagonal elements of a square matrix.

## C Program

```c
#include <stdio.h>

int main(void)
{
    int n;
    int diagonalSum = 0;

    printf("Enter the size of the square matrix: ");
    scanf("%d", &n);

    int matrix[n][n];

    printf("Enter matrix elements:\n");

    for (int i = 0; i < n; i++)
    {
        for (int j = 0; j < n; j++)
        {
            scanf("%d", &matrix[i][j]);
        }
    }

    for (int i = 0; i < n; i++)
    {
        diagonalSum += matrix[i][i];
    }

    printf("Sum of main diagonal = %d\n", diagonalSum);

    return 0;
}
```

## Sample Output

```text
Enter the size of the square matrix: 3
Enter matrix elements:
1 2 3
4 5 6
7 8 9
Sum of main diagonal = 15
```
