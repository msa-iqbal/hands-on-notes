# Area of Triangle from Three Values

Write a C program to calculate the area of a triangle when its three sides are given.

## Program

```c
#include <stdio.h>
#include <math.h>

int main(void)
{
    double a, b, c;
    double s, area;

    printf("Enter three sides: ");
    scanf("%lf %lf %lf", &a, &b, &c);

    if (a + b <= c || a + c <= b || b + c <= a)
    {
        printf("Invalid triangle.\n");
        return 0;
    }

    s = (a + b + c) / 2.0;
    area = sqrt(s * (s - a) * (s - b) * (s - c));

    printf("Area of triangle = %.2f\n", area);

    return 0;
}
```

## Output

```text
Enter three sides: 3 4 5
Area of triangle = 6.00
```

> Compile with the math library when required: `gcc program.c -lm`
