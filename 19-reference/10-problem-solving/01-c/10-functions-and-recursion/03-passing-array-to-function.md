# Passing Array to Function

## Problem

Write a C program to pass an array to a function and display its elements.

## C Program

```c
#include <stdio.h>

void displayArray(int arr[], int size)
{
    for (int i = 0; i < size; i++)
    {
        printf("%d", arr[i]);

        if (i < size - 1)
            printf(" ");
    }

    printf("\n");
}

int main(void)
{
    int n;

    printf("Enter the number of elements: ");
    scanf("%d", &n);

    int arr[n];

    printf("Enter %d elements:\n", n);

    for (int i = 0; i < n; i++)
    {
        scanf("%d", &arr[i]);
    }

    printf("Array elements: ");
    displayArray(arr, n);

    return 0;
}
```

## Sample Output

```text
Enter the number of elements: 5
Enter 5 elements:
10 20 30 40 50
Array elements: 10 20 30 40 50
```
