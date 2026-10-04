# Even Number Series Product

Write a C program to calculate the product of the first `n` even numbers.

The series is:

`2 × 4 × 6 × ...`

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
        product *= even;
    }

    printf("Product = %llu\n", product);

    return 0;
}
```

## Output

```text
Enter the number of terms: 5
Product = 3840
```
