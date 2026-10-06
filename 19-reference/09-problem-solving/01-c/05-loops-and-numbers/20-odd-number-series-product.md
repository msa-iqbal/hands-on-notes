# Odd Number Series Product

Write a C program to calculate the product of the first `n` odd numbers.

The series is:

`1 × 3 × 5 × ...`

## Program

```c
#include <stdio.h>

int main(void)
{
    int n, i, odd;
    unsigned long long product = 1;

    printf("Enter the number of terms: ");
    scanf("%d", &n);

    for (i = 1; i <= n; i++)
    {
        odd = 2 * i - 1;
        product *= odd;
    }

    printf("Product = %llu\n", product);

    return 0;
}
```

## Output

```text
Enter the number of terms: 5
Product = 945
```
