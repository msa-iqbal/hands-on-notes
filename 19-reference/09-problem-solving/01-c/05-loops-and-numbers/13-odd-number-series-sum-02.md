# Odd Number Series Sum - 02

Write a C program to calculate the sum of an odd-number series from `1` to a given limit.

## Program

```c
#include <stdio.h>

int main(void)
{
    int limit, i, sum = 0;

    printf("Enter the limit: ");
    scanf("%d", &limit);

    for (i = 1; i <= limit; i += 2)
    {
        sum += i;
    }

    printf("Sum of odd numbers = %d\n", sum);

    return 0;
}
```

## Output

```text
Enter the limit: 10
Sum of odd numbers = 25
```
