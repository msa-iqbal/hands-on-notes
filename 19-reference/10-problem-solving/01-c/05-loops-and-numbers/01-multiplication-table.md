# Multiplication Table

Write a C program to print the multiplication table of a given number.

## Program

```c
#include <stdio.h>

int main(void)
{
    int number, i;

    printf("Enter a number: ");
    scanf("%d", &number);

    for (i = 1; i <= 10; i++)
    {
        printf("%d x %d = %d\n", number, i, number * i);
    }

    return 0;
}
```

## Output

```text
Enter a number: 7
7 x 1 = 7
7 x 2 = 14
7 x 3 = 21
7 x 4 = 28
7 x 5 = 35
7 x 6 = 42
7 x 7 = 49
7 x 8 = 56
7 x 9 = 63
7 x 10 = 70
```
