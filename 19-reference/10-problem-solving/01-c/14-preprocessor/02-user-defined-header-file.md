# User-Defined Header File

## Problem

Write a C program using a user-defined header file.

Create a header file containing a function declaration, then include that header file in the main C program.

## Header File

Create a file named `math_utils.h`:

```c
#ifndef MATH_UTILS_H
#define MATH_UTILS_H

int add(int a, int b);

#endif
```

## Source File

Create a file named `math_utils.c`:

```c
#include "math_utils.h"

int add(int a, int b)
{
    return a + b;
}
```

## Main Program

Create a file named `main.c`:

```c
#include <stdio.h>
#include "math_utils.h"

int main(void)
{
    int first, second;

    printf("Enter first number: ");
    scanf("%d", &first);

    printf("Enter second number: ");
    scanf("%d", &second);

    printf("Sum = %d\n", add(first, second));

    return 0;
}
```

## Sample Output

```text
Enter first number: 10
Enter second number: 20
Sum = 30
```
