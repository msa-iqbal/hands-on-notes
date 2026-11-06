# Flow Pattern 02

## Problem

Write a C program to print a continuous alphabet pattern where letters continue from one row to the next.

### C Program

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
        for (int j = 1; j <= i; j++)
        {
            printf("%c ", ch);

            ch++;

            if (ch > 'Z')
                ch = 'A';
        }

        printf("\n");
    }

    return 0;
}
```

### Sample Output

```text
Enter number of rows: 5
A
B C
D E F
G H I J
K L M N O
```
