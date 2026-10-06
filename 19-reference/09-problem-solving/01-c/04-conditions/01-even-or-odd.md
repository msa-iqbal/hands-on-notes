# Even or Odd

Write a C program to check whether a given integer is even or odd.

## Program

```c
#include <stdio.h>

int main(void)
{
    int number;

    printf("Enter an integer: ");
    scanf("%d", &number);

    if (number % 2 == 0)
        printf("%d is even.\n", number);
    else
        printf("%d is odd.\n", number);

    return 0;
}
```

## Output

```text
Enter an integer: 25
25 is odd.
```
