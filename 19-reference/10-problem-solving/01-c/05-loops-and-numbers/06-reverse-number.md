# Reverse Number

Write a C program to reverse the digits of an integer.

## Program

```c
#include <stdio.h>

int main(void)
{
    int number, digit, reverse = 0;

    printf("Enter an integer: ");
    scanf("%d", &number);

    while (number != 0)
    {
        digit = number % 10;
        reverse = reverse * 10 + digit;
        number /= 10;
    }

    printf("Reversed number = %d\n", reverse);

    return 0;
}
```

## Output

```text
Enter an integer: 12345
Reversed number = 54321
```
