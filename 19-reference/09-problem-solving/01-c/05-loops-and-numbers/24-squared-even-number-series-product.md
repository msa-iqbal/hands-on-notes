# Squared Even Series Product

Write a C program to calculate the product of the squares of the first `n` even numbers.

The series is:

`2² × 4² × 6² × ...`

## Program

```c
#include <stdio.h>

int main(void)
{
    int n, i, even;
    unsigned long long product = 1;

    printf("Enter the number of terms: ");
    scanf("%d", &n);

    for (i = 1; i <= n; i++)
    {
        even = 2 * i;
        product *= (unsigned long long)even * even;
    }

    printf("Product = %llu\n", product);

    return 0;
}
```

## Output

```text
Enter the number of terms: 3
Product = 2304
```
