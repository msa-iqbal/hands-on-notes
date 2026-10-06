# Area of Rectangle

Write a C program to calculate the area of a rectangle using its length and width.

## Program

```c
#include <stdio.h>

int main(void)
{
    double length, width, area;

    printf("Enter length: ");
    scanf("%lf", &length);

    printf("Enter width: ");
    scanf("%lf", &width);

    area = length * width;

    printf("Area of rectangle = %.2f\n", area);

    return 0;
}
```

## Output

```text
Enter length: 10
Enter width: 5
Area of rectangle = 50.00
```
