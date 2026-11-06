# Pointer to Different Variables

## Problem

Write a C program to demonstrate pointers by storing the addresses of different variables and accessing their values through pointers.

## C Program

```c
#include <stdio.h>

int main(void)
{
    int integerValue = 10;
    float floatValue = 20.5f;
    char characterValue = 'A';

    int *integerPointer = &integerValue;
    float *floatPointer = &floatValue;
    char *characterPointer = &characterValue;

    printf("Integer value = %d\n", *integerPointer);
    printf("Float value = %.2f\n", *floatPointer);
    printf("Character value = %c\n", *characterPointer);

    return 0;
}
```

## Sample Output

```text
Integer value = 10
Float value = 20.50
Character value = A
```
