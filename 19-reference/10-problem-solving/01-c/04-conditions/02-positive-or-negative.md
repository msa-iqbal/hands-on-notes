# Positive or Negative

Write a C program to check whether a given number is positive, negative, or zero.

## Program

```c
#include <stdio.h>

int main(void)
{
    double number;

    printf("Enter a number: ");
    scanf("%lf", &number);

    if (number > 0)
        printf("%.2f is positive.\n", number);
    else if (number < 0)
        printf("%.2f is negative.\n", number);
    else
        printf("The number is zero.\n");

    return 0;
}
```

## Output

```text
Enter a number: -15.5
-15.50 is negative.
```
