# Leap Year

Write a C program to check whether a given year is a leap year.

A year is a leap year if it is divisible by 400, or if it is divisible by 4 but not divisible by 100.

## Program

```c
#include <stdio.h>

int main(void)
{
    int year;

    printf("Enter a year: ");
    scanf("%d", &year);

    if ((year % 400 == 0) || (year % 4 == 0 && year % 100 != 0))
        printf("%d is a leap year.\n", year);
    else
        printf("%d is not a leap year.\n", year);

    return 0;
}
```

## Output

```text
Enter a year: 2024
2024 is a leap year.
```
