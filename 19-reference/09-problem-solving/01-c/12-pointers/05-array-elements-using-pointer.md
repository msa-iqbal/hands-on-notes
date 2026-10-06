# Access Array Elements Using Pointer

## Problem

Write a C program to access and display the elements of an array using a pointer.

## C Program

```c
#include <stdio.h>

int main(void)
{
    int n;

    printf("Enter the number of elements: ");
    scanf("%d", &n);

    int arr[n];
    int *ptr = arr;

    printf("Enter %d elements:\n", n);

    for (int i = 0; i < n; i++)
    {
        scanf("%d", &arr[i]);
    }

    printf("Array elements using pointer: ");

    for (int i = 0; i < n; i++)
    {
        printf("%d", *(ptr + i));

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
Array elements using pointer: 10 20 30 40 50
```
