# Odd Number Series Sum - 03

Write a C program to calculate the sum of the squares of the first `n` odd numbers.

The series is:

`1² + 3² + 5² + ...`

## Program

```c
#include <stdio.h>

int main(void)
{
    int n, i, odd;
    long long sum = 0;

    printf("Enter the number of terms: ");
    scanf("%d", &n);

    for (i = 1; i <= n; i++)
    {
        odd = 2 * i - 1;
        sum += (long long)odd * odd;
    }

    printf("Sum = %lld\n", sum);

    return 0;
}
```

## Output

```text
Enter the number of terms: 5
Sum = 165
```
