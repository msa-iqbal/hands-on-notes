# #define Preprocessor

## Problem

Write a C program using the `#define` preprocessor directive to define constants and use them in calculations.

## C Program

```c
#include <stdio.h>

#define PI 3.141592653589793
#define MAX_VALUE 100

int main(void)
{
    float radius;
    float area;

    printf("Enter radius: ");
    scanf("%f", &radius);

    area = PI * radius * radius;

    printf("Area of circle = %.2f\n", area);
    printf("Maximum value = %d\n", MAX_VALUE);

    return 0;
}
```

## Sample Output

```text
Enter radius: 5
Area of circle = 78.54
Maximum value = 100
```
