# Even Number Series Sum

Write a C program to calculate the sum of the first `n` even numbers.

The series is:

`2 + 4 + 6 + ...`

## Program

```c
#include <stdio.h>

int main(void)
{
    int n, i, even;
    int sum = 0;

    printf("Enter the number of terms: ");
    scanf("%d", &n);

    for (i = 1; i <= n; i++)
    {
        even = 2 * i;
        sum += even;
    }

    printf("Sum = %d\n", sum);

    return 0;
}
```

## Output

```text
Enter the number of terms: 5
Sum = 30
```
