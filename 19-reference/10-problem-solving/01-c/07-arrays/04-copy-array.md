# Copy One Array to Another

## Problem

Write a C program to copy all elements from one array into another array.

## C Program

```c
#include <stdio.h>

int main(void)
{
    int n;

    printf("Enter the number of elements: ");
    scanf("%d", &n);

    int source[n];
    int destination[n];

    printf("Enter %d elements:\n", n);

    for (int i = 0; i < n; i++)
    {
        scanf("%d", &source[i]);
    }

    for (int i = 0; i < n; i++)
    {
        destination[i] = source[i];
    }

    printf("Copied array: ");

    for (int i = 0; i < n; i++)
    {
        printf("%d", destination[i]);

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
Copied array: 10 20 30 40 50
```
