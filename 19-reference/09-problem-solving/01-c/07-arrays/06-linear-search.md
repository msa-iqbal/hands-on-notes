# Linear Search in an Array

## Problem

Write a C program to search for an element in an array using the linear search algorithm.

## C Program

```c
#include <stdio.h>

int main(void)
{
    int n;
    int search;
    int found = -1;

    printf("Enter the number of elements: ");
    scanf("%d", &n);

    int arr[n];

    printf("Enter %d elements:\n", n);

    for (int i = 0; i < n; i++)
    {
        scanf("%d", &arr[i]);
    }

    printf("Enter the element to search: ");
    scanf("%d", &search);

    for (int i = 0; i < n; i++)
    {
        if (arr[i] == search)
        {
            found = i;
            break;
        }
    }

    if (found != -1)
    {
        printf("Element found at position %d.\n", found + 1);
    }
    else
    {
        printf("Element not found.\n");
    }

    return 0;
}
```

## Sample Output

```text
Enter the number of elements: 6
Enter 6 elements:
10 25 30 45 50 60
Enter the element to search: 45
Element found at position 4.
```
