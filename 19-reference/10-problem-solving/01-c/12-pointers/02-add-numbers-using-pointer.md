# Add Two Numbers Using Pointer

## Problem

Write a C program to add two numbers using pointers.

## C Program

```c
#include <stdio.h>

int main(void)
{
    int first, second;
    int *pFirst, *pSecond;

    printf("Enter first number: ");
    scanf("%d", &first);

    printf("Enter second number: ");
    scanf("%d", &second);

    pFirst = &first;
    pSecond = &second;

    printf("Sum = %d\n", *pFirst + *pSecond);

    return 0;
}
```

## Sample Output

```text
Enter first number: 10
Enter second number: 20
Sum = 30
```
