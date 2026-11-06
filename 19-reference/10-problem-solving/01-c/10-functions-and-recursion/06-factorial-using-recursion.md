# Factorial Using Recursion

## Problem

Write a C program to calculate the factorial of a number using recursion.

The factorial of `n` is:

```text
n! = n × (n - 1) × (n - 2) × ... × 1
```

And:

```text
0! = 1
```

## C Program

```c
#include <stdio.h>

unsigned long long factorial(int n)
{
    if (n == 0 || n == 1)
        return 1;

    return n * factorial(n - 1);
}

int main(void)
{
    int n;

    printf("Enter a non-negative integer: ");
    scanf("%d", &n);

    if (n < 0)
    {
        printf("Factorial is not defined for negative numbers.\n");
        return 0;
    }

    printf("%d! = %llu\n", n, factorial(n));

    return 0;
}
```

## Sample Output

```text
Enter a non-negative integer: 5
5! = 120
```
