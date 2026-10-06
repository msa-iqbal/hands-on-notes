# Fibonacci Series

Write a C program to print the first `n` terms of the Fibonacci series.

The Fibonacci series starts with `0` and `1`, and every following term is the sum of the previous two terms.

## Program

```c
#include <stdio.h>

int main(void)
{
    int n, i;
    unsigned long long first = 0, second = 1, next;

    printf("Enter the number of terms: ");
    scanf("%d", &n);

    printf("Fibonacci series: ");

    for (i = 1; i <= n; i++)
    {
        printf("%llu", first);

        if (i < n)
            printf(" ");

        next = first + second;
        first = second;
        second = next;
    }

    printf("\n");

    return 0;
}
```

## Output

```text
Enter the number of terms: 10
Fibonacci series: 0 1 1 2 3 5 8 13 21 34
```
