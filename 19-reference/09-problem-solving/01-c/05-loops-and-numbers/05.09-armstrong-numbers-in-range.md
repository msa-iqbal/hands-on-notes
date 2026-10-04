# Armstrong Numbers in Range

Write a C program to print all three-digit Armstrong numbers within a given range.

## Program

```c
#include <stdio.h>

int main(void)
{
    int start, end, number, digit, sum;

    printf("Enter the starting value: ");
    scanf("%d", &start);

    printf("Enter the ending value: ");
    scanf("%d", &end);

    printf("Armstrong numbers: ");

    for (number = start; number <= end; number++)
    {
        int original = number;
        sum = 0;

        while (number != 0)
        {
            digit = number % 10;
            sum += digit * digit * digit;
            number /= 10;
        }

        number = original;

        if (sum == original)
            printf("%d ", original);
    }

    printf("\n");

    return 0;
}
```

## Output

```text
Enter the starting value: 100
Enter the ending value: 500
Armstrong numbers: 153 370 371 407
```
