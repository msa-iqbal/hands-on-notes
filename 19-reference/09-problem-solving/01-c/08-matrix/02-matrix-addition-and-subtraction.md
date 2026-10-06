# Matrix Addition and Subtraction

## Problem

Write a C program to add and subtract two matrices of the same size.

## C Program

```c
#include <stdio.h>

int main(void)
{
    int rows, columns;

    printf("Enter number of rows: ");
    scanf("%d", &rows);

    printf("Enter number of columns: ");
    scanf("%d", &columns);

    int a[rows][columns];
    int b[rows][columns];
    int sum[rows][columns];
    int difference[rows][columns];

    printf("Enter elements of first matrix:\n");

    for (int i = 0; i < rows; i++)
    {
        for (int j = 0; j < columns; j++)
        {
            scanf("%d", &a[i][j]);
        }
    }

    printf("Enter elements of second matrix:\n");

    for (int i = 0; i < rows; i++)
    {
        for (int j = 0; j < columns; j++)
        {
            scanf("%d", &b[i][j]);
        }
    }

    for (int i = 0; i < rows; i++)
    {
        for (int j = 0; j < columns; j++)
        {
            sum[i][j] = a[i][j] + b[i][j];
            difference[i][j] = a[i][j] - b[i][j];
        }
    }

    printf("Matrix Addition:\n");

    for (int i = 0; i < rows; i++)
    {
        for (int j = 0; j < columns; j++)
        {
            printf("%d ", sum[i][j]);
        }

        printf("\n");
    }

    printf("Matrix Subtraction:\n");

    for (int i = 0; i < rows; i++)
    {
        for (int j = 0; j < columns; j++)
        {
            printf("%d ", difference[i][j]);
        }

        printf("\n");
    }

    return 0;
}
```

## Sample Output

```text
Enter number of rows: 2
Enter number of columns: 2
Enter elements of first matrix:
1 2
3 4
Enter elements of second matrix:
5 6
7 8
Matrix Addition:
6 8
10 12
Matrix Subtraction:
-4 -4
-4 -4
```
