# Maximum Array Value Using Function

## Problem

Write a C program to find the maximum value in an array using a user-defined function.

## C Program

```c
#include <stdio.h>

int findMaximum(int arr[], int size)
{
    int maximum = arr[0];

    for (int i = 1; i < size; i++)
    {
        if (arr[i] > maximum)
        {
            maximum = arr[i];
        }
    }

    return maximum;
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

    printf("Maximum value = %d\n", findMaximum(arr, n));

    return 0;
}
```

## Sample Output

```text
Enter the number of elements: 6
Enter 6 elements:
12 45 23 67 34 10
Maximum value = 67
```
