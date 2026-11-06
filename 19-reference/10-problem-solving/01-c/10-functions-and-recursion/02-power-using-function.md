# Power of a Number Using Function

## Problem

Write a C program to calculate the power of a number using a user-defined function.

## C Program

```c
#include <stdio.h>

long long power(int base, int exponent)
{
    long long result = 1;

    for (int i = 1; i <= exponent; i++)
    {
        result *= base;
    }

    return result;
}

int main(void)
{
    int base, exponent;

    printf("Enter base: ");
    scanf("%d", &base);

    printf("Enter exponent: ");
    scanf("%d", &exponent);

    if (exponent < 0)
    {
        printf("Exponent must be non-negative.\n");
        return 0;
    }

    printf("%d^%d = %lld\n", base, exponent, power(base, exponent));

    return 0;
}
```

## Sample Output

```text
Enter base: 2
Enter exponent: 5
2^5 = 32
```
