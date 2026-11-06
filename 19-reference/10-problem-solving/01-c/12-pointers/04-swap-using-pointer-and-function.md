# Swap Two Numbers Using Pointer and Function

## Problem

Write a C program to swap two numbers by passing their addresses to a function.

## C Program

```c
#include <stdio.h>

void swap(int *a, int *b)
{
    int temp = *a;

    *a = *b;
    *b = temp;
}

int main(void)
{
    int first, second;

    printf("Enter first number: ");
    scanf("%d", &first);

    printf("Enter second number: ");
    scanf("%d", &second);

    printf("Before swapping: %d %d\n", first, second);

    swap(&first, &second);

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
