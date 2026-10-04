# Size of Union and Structure

## Problem

Write a C program to find and compare the memory sizes of a structure and a union.

## C Program

```c
#include <stdio.h>

struct Data
{
    int integer;
    float decimal;
    char character;
};

union DataUnion
{
    int integer;
    float decimal;
    char character;
};

int main(void)
{
    printf("Size of structure = %zu bytes\n", sizeof(struct Data));
    printf("Size of union = %zu bytes\n", sizeof(union DataUnion));

    return 0;
}
```

## Sample Output

```text
Size of structure = 12 bytes
Size of union = 4 bytes
```

> The exact structure size can vary depending on compiler, architecture, alignment, and padding.
