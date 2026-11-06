# Area of Triangle Using Function

## Problem

Write a C program to calculate the area of a triangle using a user-defined function.

Formula:

```text
Area = 1/2 × base × height
```

## C Program

```c
#include <stdio.h>

float triangleArea(float base, float height)
{
    return 0.5f * base * height;
}

int main(void)
{
    float base, height, area;

    printf("Enter base: ");
    scanf("%f", &base);

    printf("Enter height: ");
    scanf("%f", &height);

    area = triangleArea(base, height);

    printf("Area of triangle = %.2f\n", area);

    return 0;
}
```

## Sample Output

```text
Enter base: 10
Enter height: 5
Area of triangle = 25.00
```
