# Strong Number

Write a C program to check whether a number is a strong number.

A strong number is a number whose value is equal to the sum of the factorials of its digits.

For example, `145 = 1! + 4! + 5!`.

## Program

```c
#include <stdio.h>

int main(void)
{
    int number, original, digit;
    int sum = 0;

    printf("Enter an integer: ");
    scanf("%d", &number);

    original = number;

    while (number != 0)
    {
        int factorial = 1;
        int i;

        digit = number % 10;

        for (i = 1; i <= digit; i++)
        {
            factorial *= i;
        }

        sum += factorial;
        number /= 10;
    }

    if (sum == original)
        printf("%d is a strong number.\n", original);
    else
        printf("%d is not a strong number.\n", original);

    return 0;
}
```

## Output

```text
Enter an integer: 145
145 is a strong number.
```
