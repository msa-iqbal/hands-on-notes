# Create and Display a Matrix

## Problem

Write a C program to create a matrix by taking the number of rows and columns from the user, then display the matrix.

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

    int matrix[rows][columns];

    printf("Enter matrix elements:\n");

    for (int i = 0; i < rows; i++)
    {
        for (int j = 0; j < columns; j++)
        {
            scanf("%d", &matrix[i][j]);
        }
    }

    printf("Matrix:\n");

    for (int i = 0; i < rows; i++)
    {
        for (int j = 0; j < columns; j++)
        {
            printf("%d ", matrix[i][j]);
        }

        printf("\n");
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
Matrix:
1 2 3
4 5 6
```
