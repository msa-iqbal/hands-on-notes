# Odd Number Series Sum - 01

Write a C program to find the sum of the first `n` odd numbers.

The series is:

`1 + 3 + 5 + 7 + ...`

## Program

```c
#include <stdio.h>

int main(void)
{
    int n, i, sum = 0;

    printf("Enter the number of terms: ");
    scanf("%d", &n);

    for (i = 1; i <= n; i++)
    {
        sum += 2 * i - 1;
    }

    printf("Sum = %d\n", sum);

    return 0;
}
```

## Output

```text
Enter the number of terms: 5
Sum = 25
```
