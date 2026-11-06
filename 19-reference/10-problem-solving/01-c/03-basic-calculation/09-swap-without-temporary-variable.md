# Swap without Temporary Variable

Write a C program to swap two integers without using a temporary variable.

## Program

```c
#include <stdio.h>

int main(void)
{
    int a, b;

    printf("Enter two numbers: ");
    scanf("%d %d", &a, &b);

    printf("Before swapping: a = %d, b = %d\n", a, b);

    a = a + b;
    b = a - b;
    a = a - b;

    printf("After swapping:  a = %d, b = %d\n", a, b);

    return 0;
}
```

## Output

```text
Enter two numbers: 10 20
Before swapping: a = 10, b = 20
After swapping:  a = 20, b = 10
```

> This arithmetic method can overflow for sufficiently large integers. In production code, a temporary variable is generally safer and clearer.
