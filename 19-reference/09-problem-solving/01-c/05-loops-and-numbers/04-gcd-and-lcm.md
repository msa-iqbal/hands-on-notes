# GCD and LCM

Write a C program to find the Greatest Common Divisor (GCD) and Least Common Multiple (LCM) of two integers.

## Program

```c
#include <stdio.h>

int main(void)
{
    int a, b, x, y, remainder, gcd, lcm;

    printf("Enter two positive integers: ");
    scanf("%d %d", &a, &b);

    x = a;
    y = b;

    while (y != 0)
    {
        remainder = x % y;
        x = y;
        y = remainder;
    }

    gcd = x;
    lcm = (a / gcd) * b;

    printf("GCD = %d\n", gcd);
    printf("LCM = %d\n", lcm);

    return 0;
}
```

## Output

```text
Enter two positive integers: 12 18
GCD = 6
LCM = 36
```
