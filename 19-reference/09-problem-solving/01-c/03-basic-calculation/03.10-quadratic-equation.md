# Quadratic Equation

Write a C program to find the roots of a quadratic equation:

```text
ax² + bx + c = 0
```

## Program

```c
#include <stdio.h>
#include <math.h>

int main(void)
{
    double a, b, c;
    double discriminant;
    double root1, root2;
    double realPart, imaginaryPart;

    printf("Enter a, b and c: ");
    scanf("%lf %lf %lf", &a, &b, &c);

    if (a == 0)
    {
        printf("This is not a quadratic equation.\n");
        return 0;
    }

    discriminant = b * b - 4.0 * a * c;

    if (discriminant > 0)
    {
        root1 = (-b + sqrt(discriminant)) / (2.0 * a);
        root2 = (-b - sqrt(discriminant)) / (2.0 * a);

        printf("Root 1 = %.2f\n", root1);
        printf("Root 2 = %.2f\n", root2);
    }
    else if (discriminant == 0)
    {
        root1 = -b / (2.0 * a);

        printf("Both roots = %.2f\n", root1);
    }
    else
    {
        realPart = -b / (2.0 * a);
        imaginaryPart = sqrt(-discriminant) / (2.0 * a);

        printf("Root 1 = %.2f + %.2fi\n",
               realPart, imaginaryPart);

        printf("Root 2 = %.2f - %.2fi\n",
               realPart, imaginaryPart);
    }

    return 0;
}
```

## Output

```text
Enter a, b and c: 1 -5 6
Root 1 = 3.00
Root 2 = 2.00
```

> Compile with the math library when required: `gcc program.c -lm`
