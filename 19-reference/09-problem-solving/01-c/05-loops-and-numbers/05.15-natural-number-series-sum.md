# Natural Number Series Sum

Write a C program to calculate the sum of the first `n` natural numbers.

The series is:

`1 + 2 + 3 + ... + n`

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
        sum += i;
    }

    printf("Sum = %d\n", sum);

    return 0;
}
```

## Output

```text
Enter the number of terms: 10
Sum = 55
```
