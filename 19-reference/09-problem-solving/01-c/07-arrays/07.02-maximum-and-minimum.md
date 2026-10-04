# Maximum and Minimum Element in an Array

## Problem

Write a C program to find the maximum and minimum elements in an array.

## C Program

```c
#include <stdio.h>

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

    int maximum = arr[0];
    int minimum = arr[0];

    for (int i = 1; i < n; i++)
    {
        if (arr[i] > maximum)
            maximum = arr[i];

        if (arr[i] < minimum)
            minimum = arr[i];
    }

    printf("Maximum = %d\n", maximum);
    printf("Minimum = %d\n", minimum);

    return 0;
}
```

## Sample Output

```text
Enter the number of elements: 6
Enter 6 elements:
25 10 45 5 30 20
Maximum = 45
Minimum = 5
```
