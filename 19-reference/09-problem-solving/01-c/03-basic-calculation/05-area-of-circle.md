# Area of Circle

Write a C program to calculate the area of a circle using its radius.

## Program

```c
#include <stdio.h>

#define PI 3.141592653589793

int main(void)
{
    double radius, area;

    printf("Enter radius: ");
    scanf("%lf", &radius);

    area = PI * radius * radius;

    printf("Area of circle = %.2f\n", area);

    return 0;
}
```

## Output

```text
Enter radius: 5
Area of circle = 78.54
```
