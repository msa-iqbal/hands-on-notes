# Matrix Multiplication

## Problem

Write a C program to multiply two matrices.

Matrix multiplication is possible when the number of columns in the first matrix is equal to the number of rows in the second matrix.

## C Program

```c
#include <stdio.h>

int main(void)
{
    int rows1, columns1;
    int rows2, columns2;

    printf("Enter rows and columns of first matrix: ");
    scanf("%d %d", &rows1, &columns1);

    printf("Enter rows and columns of second matrix: ");
    scanf("%d %d", &rows2, &columns2);

    if (columns1 != rows2)
    {
        printf("Matrix multiplication is not possible.\n");
        return 0;
    }

    int a[rows1][columns1];
    int b[rows2][columns2];
    int result[rows1][columns2];

    printf("Enter elements of first matrix:\n");

    for (int i = 0; i < rows1; i++)
    {
        for (int j = 0; j < columns1; j++)
        {
            scanf("%d", &a[i][j]);
        }
    }

    printf("Enter elements of second matrix:\n");

    for (int i = 0; i < rows2; i++)
    {
        for (int j = 0; j < columns2; j++)
        {
            scanf("%d", &b[i][j]);
        }
    }

    for (int i = 0; i < rows1; i++)
    {
        for (int j = 0; j < columns2; j++)
        {
            result[i][j] = 0;

            for (int k = 0; k < columns1; k++)
            {
                result[i][j] += a[i][k] * b[k][j];
            }
        }
    }

    printf("Result of matrix multiplication:\n");

    for (int i = 0; i < rows1; i++)
    {
        for (int j = 0; j < columns2; j++)
        {
            printf("%d ", result[i][j]);
        }

        printf("\n");
    }

    return 0;
}
```

## Sample Output

```text
Enter rows and columns of first matrix: 2 2
Enter rows and columns of second matrix: 2 2
Enter elements of first matrix:
1 2
3 4
Enter elements of second matrix:
5 6
7 8
Result of matrix multiplication:
19 22
43 50
```
