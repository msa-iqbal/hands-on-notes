# Swap with Temporary Variable

Write a C program to swap two numbers using a temporary variable.

## Program

```c
#include <stdio.h>

int main(void)
{
    int a, b, temp;

    printf("Enter two numbers: ");
    scanf("%d %d", &a, &b);

    printf("Before swapping: a = %d, b = %d\n", a, b);

    temp = a;
    a = b;
    b = temp;

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
