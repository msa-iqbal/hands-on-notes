# Product Series

Write a C program to calculate the product of the natural number series:

`1 × 2 × 3 × ... × n`

## Program

```c
#include <stdio.h>

int main(void)
{
    int n, i;
    unsigned long long product = 1;

    printf("Enter the number of terms: ");
    scanf("%d", &n);

    for (i = 1; i <= n; i++)
    {
        product *= i;
    }

    printf("Product = %llu\n", product);

    return 0;
}
```

## Output

```text
Enter the number of terms: 5
Product = 120
```
