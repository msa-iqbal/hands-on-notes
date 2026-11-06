# Left Angle Number Triangle 03

## Problem

Print consecutive numbers in a right-aligned triangle:

```text
    1
   23
  456
 7890
12345
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
        for (j = 1; j <= n - i; j++)
        {
            printf(" ");
        }

        for (j = 1; j <= i; j++)
        {
            printf("%d", number % 10);
            number++;
        }

        printf("\n");
    }

    return 0;
}
```

## Example Output

```text
Enter number of rows: 5

    1
   23
  456
 789
01234
```
