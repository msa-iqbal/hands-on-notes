# Flow Pattern 05

## Problem

Write a C program to print a continuous alphabet pattern in reverse order within each row.

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
        char start = ch + i - 1;

        for (int j = i; j >= 1; j--)
        {
            printf("%c ", start);
            start--;
        }

        ch += i;

        if (ch > 'Z')
            ch = 'A';

        printf("\n");
    }

    return 0;
}
```

### Sample Output

```text
Enter number of rows: 5
A
C B
F E D
J I H G
O N M L K
```
