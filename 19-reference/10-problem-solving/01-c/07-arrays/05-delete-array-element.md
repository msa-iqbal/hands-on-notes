# Delete an Element from an Array

## Problem

Write a C program to delete an element from an array at a specified position.

The position is considered 1-based.

## C Program

```c
#include <stdio.h>

int main(void)
{
    int n;
    int position;

    printf("Enter the number of elements: ");
    scanf("%d", &n);

    int arr[n];

    printf("Enter %d elements:\n", n);

    for (int i = 0; i < n; i++)
    {
        scanf("%d", &arr[i]);
    }

    printf("Enter the position to delete: ");
    scanf("%d", &position);

    if (position < 1 || position > n)
    {
        printf("Invalid position.\n");
        return 0;
    }

    for (int i = position - 1; i < n - 1; i++)
    {
        arr[i] = arr[i + 1];
    }

    n--;

    printf("Array after deletion: ");

    for (int i = 0; i < n; i++)
    {
        printf("%d", arr[i]);

        if (i < n - 1)
            printf(" ");
    }

    printf("\n");

    return 0;
}
```

## Sample Output

```text
Enter the number of elements: 5
Enter 5 elements:
10 20 30 40 50
Enter the position to delete: 3
Array after deletion: 10 20 40 50
```
