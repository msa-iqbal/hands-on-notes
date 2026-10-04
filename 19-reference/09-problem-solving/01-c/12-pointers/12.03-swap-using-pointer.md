# Swap Two Numbers Using Pointer

## Problem

Write a C program to swap two numbers using pointers.

## C Program

```c
#include <stdio.h>

int main(void)
{
    int first, second;
    int *pFirst, *pSecond;
    int temp;

    printf("Enter first number: ");
    scanf("%d", &first);

    printf("Enter second number: ");
    scanf("%d", &second);

    pFirst = &first;
    pSecond = &second;

    printf("Before swapping: %d %d\n", first, second);

    temp = *pFirst;
    *pFirst = *pSecond;
    *pSecond = temp;

    printf("After swapping: %d %d\n", first, second);

    return 0;
}
```

## Sample Output

```text
Enter first number: 10
Enter second number: 20
Before swapping: 10 20
After swapping: 20 10
```
