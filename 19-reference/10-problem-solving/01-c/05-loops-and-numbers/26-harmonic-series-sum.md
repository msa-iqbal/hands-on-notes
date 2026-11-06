# Harmonic Series Sum

Write a C program to calculate the sum of the first `n` terms of the harmonic series.

The series is:

`1 + 1/2 + 1/3 + ... + 1/n`

## Program

```c
#include <stdio.h>

int main(void)
{
    int n, i;
    double sum = 0.0;

    printf("Enter the number of terms: ");
    scanf("%d", &n);

    for (i = 1; i <= n; i++)
    {
        sum += 1.0 / i;
    }

    printf("Harmonic sum = %.6f\n", sum);

    return 0;
}
```

## Output

```text
Enter the number of terms: 5
Harmonic sum = 2.283333
```
