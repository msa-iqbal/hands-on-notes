# Left-Angle Symbol Triangle 02

## Problem

Write a C program to print a right-aligned triangle using `#`.

### C Program

```c
#include <stdio.h>

int main(void)
{
    int n;

    printf("Enter number of rows: ");
    scanf("%d", &n);

    for (int i = 1; i <= n; i++)
    {
        for (int j = 1; j <= n - i; j++)
        {
            printf(" ");
        }

        for (int j = 1; j <= i; j++)
        {
            printf("#");
        }

        printf("\n");
    }

    return 0;
}
```

### Sample Output

```text
Enter number of rows: 5
    #
   ##
  ###
 ####
#####
```
