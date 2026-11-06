# Sum of Upper and Lower Triangle of a Matrix

## Problem

Write a C program to find the sum of the elements in the upper triangular and lower triangular portions of a square matrix.

For the upper triangle, include the main diagonal.

For the lower triangle, include the main diagonal.

## C Program

```c
#include <stdio.h>

int main(void)
{
    int n;
    int upperSum = 0;
    int lowerSum = 0;

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
        for (int j = 0; j < n; j++)
        {
            if (i <= j)
                upperSum += matrix[i][j];

            if (i >= j)
                lowerSum += matrix[i][j];
        }
    }

    printf("Upper triangular sum = %d\n", upperSum);
    printf("Lower triangular sum = %d\n", lowerSum);

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
Upper triangular sum = 21
Lower triangular sum = 27
```
