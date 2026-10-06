# Fibonacci Series Using Array

## Problem

Write a C program to generate the Fibonacci series using an array.

The Fibonacci series starts with `0` and `1`. Each subsequent number is the sum of the previous two numbers.

## C Program

```c
#include <stdio.h>

int main(void)
{
    int n;

    printf("Enter the number of terms: ");
    scanf("%d", &n);

    int fibonacci[n];

    if (n >= 1)
        fibonacci[0] = 0;

    if (n >= 2)
        fibonacci[1] = 1;

    for (int i = 2; i < n; i++)
    {
        fibonacci[i] = fibonacci[i - 1] + fibonacci[i - 2];
    }

    printf("Fibonacci Series: ");

    for (int i = 0; i < n; i++)
    {
        printf("%d", fibonacci[i]);

        if (i < n - 1)
            printf(" ");
    }

    printf("\n");

    return 0;
}
```

## Sample Output

```text
Enter the number of terms: 10
Fibonacci Series: 0 1 1 2 3 5 8 13 21 34
```
