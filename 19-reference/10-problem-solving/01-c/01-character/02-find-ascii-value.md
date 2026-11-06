# Find ASCII Value

Write a C program to find the character from a given ASCII value.

## Program

```c
#include <stdio.h>

int main(void)
{
    int ascii;

    printf("Enter ASCII value: ");
    scanf("%d", &ascii);

    printf("Character = %c\n", ascii);

    return 0;
}
```

## Output

```text
Enter ASCII value: 65
Character = A
```
