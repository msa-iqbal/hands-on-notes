# Factorial

Write a C program to find the factorial of a given non-negative integer.

## Program

```c
#include <stdio.h>

int main(void)
{
    int n, i;
    unsigned long long factorial = 1;

    printf("Enter a non-negative integer: ");
    scanf("%d", &n);

    if (n < 0)
    {
        printf("Factorial is not defined for negative numbers.\n");
    }
    else
    {
        for (i = 1; i <= n; i++)
        {
            factorial *= i;
        }

        printf("%d! = %llu\n", n, factorial);
    }

    return 0;
}
```

## Output

```text
Enter a non-negative integer: 5
5! = 120
```
