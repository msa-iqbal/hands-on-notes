# Right Angle Number Triangle 03

## Problem

Print consecutive numbers in each row:

```text
1
23
456
789
```

## C Program

```c
#include <stdio.h>

int main(void)
{
    int i, j, n;
    int number = 1;

    printf("Enter number of rows: ");
    scanf("%d", &n);

    for (i = 1; i <= n; i++)
    {
        for (j = 1; j <= i; j++)
        {
            printf("%d", number);
            number++;
        }

        printf("\n");
    }

    return 0;
}
```

## Example Output

```text
Enter number of rows: 4

1
23
456
789
```
