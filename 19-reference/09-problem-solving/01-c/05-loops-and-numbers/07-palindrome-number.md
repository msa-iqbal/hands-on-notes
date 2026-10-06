# Palindrome Number

Write a C program to check whether an integer is a palindrome number.

A palindrome number remains the same when its digits are reversed.

## Program

```c
#include <stdio.h>

int main(void)
{
    int number, original, digit, reverse = 0;

    printf("Enter an integer: ");
    scanf("%d", &number);

    original = number;

    while (number != 0)
    {
        digit = number % 10;
        reverse = reverse * 10 + digit;
        number /= 10;
    }

    if (original == reverse)
        printf("%d is a palindrome number.\n", original);
    else
        printf("%d is not a palindrome number.\n", original);

    return 0;
}
```

## Output

```text
Enter an integer: 1221
1221 is a palindrome number.
```
