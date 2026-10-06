# Alphabetic Pyramid Pattern 22

## Problem

Write a C program to print a centered pyramid using continuous alphabets.

## C Program

```c
#include <stdio.h>

int main(void)
{
    int n;
    char ch = 'A';

    printf("Enter number of rows: ");
    scanf("%d", &n);

    for (int i = 1; i <= n; i++)
    {
        for (int j = 1; j <= n - i; j++)
            printf(" ");

        for (int j = 1; j <= 2 * i - 1; j++)
        {
            printf("%c", ch);

            ch++;

            if (ch > 'Z')
                ch = 'A';
        }

        printf("\n");
    }

    return 0;
}
```

## Sample Output

```text
Enter number of rows: 5
    A
   BCD
  EFGHI
 JKLMNOP
QRSTUVWXY
```
