# Alternating Series Sum

Write a C program to calculate the sum of the alternating series:

`1 - 2 + 3 - 4 + 5 - ...`

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
        if (i % 2 == 0)
            sum -= i;
        else
            sum += i;
    }

    printf("Sum = %lld\n", sum);

    return 0;
}
```

## Output

```text
Enter the number of terms: 6
Sum = -3
```
