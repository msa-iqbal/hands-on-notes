# Typedef for Primitive Data Types

## Problem

Write a C program that uses `typedef` to create aliases for primitive data types.

## C Program

```c
#include <stdio.h>

typedef int Integer;
typedef float Decimal;
typedef char Character;

int main(void)
{
    Integer age = 20;
    Decimal marks = 85.50;
    Character grade = 'A';

    printf("Age = %d\n", age);
    printf("Marks = %.2f\n", marks);
    printf("Grade = %c\n", grade);

    return 0;
}
```

## Sample Output

```text
Age = 20
Marks = 85.50
Grade = A
```
