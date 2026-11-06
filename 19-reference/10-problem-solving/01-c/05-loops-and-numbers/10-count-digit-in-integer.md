# Count Digits in Integer

Write a C program to count the number of digits in an integer.

## Program

```c
#include <stdio.h>

int main(void)
{
    int number, count = 0;

    printf("Enter an integer: ");
    scanf("%d", &number);

    if (number == 0)
    {
        count = 1;
    }
    else
    {
        if (number < 0)
            number = -number;

        while (number != 0)
        {
            number /= 10;
            count++;
        }
    }

    printf("Number of digits = %d\n", count);

    return 0;
}
```

## Output

```text
Enter an integer: 123456
Number of digits = 6
```
