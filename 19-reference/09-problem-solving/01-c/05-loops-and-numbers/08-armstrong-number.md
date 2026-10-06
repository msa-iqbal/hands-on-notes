# Armstrong Number

Write a C program to check whether a number is an Armstrong number.

For a three-digit number, an Armstrong number is a number equal to the sum of the cubes of its digits.

## Program

```c
#include <stdio.h>

int main(void)
{
    int number, original, digit, sum = 0;

    printf("Enter a three-digit integer: ");
    scanf("%d", &number);

    original = number;

    while (number != 0)
    {
        digit = number % 10;
        sum += digit * digit * digit;
        number /= 10;
    }

    if (original == sum)
        printf("%d is an Armstrong number.\n", original);
    else
        printf("%d is not an Armstrong number.\n", original);

    return 0;
}
```

## Output

```text
Enter a three-digit integer: 153
153 is an Armstrong number.
```
