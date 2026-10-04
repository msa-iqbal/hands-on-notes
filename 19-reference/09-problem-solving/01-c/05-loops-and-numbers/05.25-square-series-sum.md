# Square Series Sum

Write a C program to calculate the sum of the squares of the first `n` natural numbers.

The series is:

`1² + 2² + 3² + ... + n²`

## Program

```c
#include <stdio.h>

int main(void)
{
    int n, i;
    long long sum = 0;

    printf("Enter the number of terms: ");
    scanf("%d", &n);

    for (i = 1; i <= n; i++)
    {
        sum += (long long)i * i;
    }

    printf("Sum = %lld\n", sum);

    return 0;
}
```

## Output

```text
Enter the number of terms: 5
Sum = 55
```
