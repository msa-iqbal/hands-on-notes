# Area of Triangle

Write a C program to calculate the area of a triangle using its base and height.

## Program

```c
#include <stdio.h>

int main(void)
{
    double base, height, area;

    printf("Enter base: ");
    scanf("%lf", &base);

    printf("Enter height: ");
    scanf("%lf", &height);

    area = 0.5 * base * height;

    printf("Area of triangle = %.2f\n", area);

    return 0;
}
```

## Output

```text
Enter base: 10
Enter height: 6
Area of triangle = 30.00
```
