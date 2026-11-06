# Prime Number

Write a C program to check whether a given integer is a prime number.

## Program

```c
#include <stdio.h>

int main(void)
{
    int n, i, is_prime = 1;

    printf("Enter an integer: ");
    scanf("%d", &n);

    if (n < 2)
    {
        is_prime = 0;
    }
    else
    {
        for (i = 2; i <= n / i; i++)
        {
            if (n % i == 0)
            {
                is_prime = 0;
                break;
            }
        }
    }

    if (is_prime)
        printf("%d is a prime number.\n", n);
    else
        printf("%d is not a prime number.\n", n);

    return 0;
}
```

## Output

```text
Enter an integer: 29
29 is a prime number.
```
