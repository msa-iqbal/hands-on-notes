# Largest of Three Numbers

Write a C program to find the largest among three numbers.

## Program

```c
#include <stdio.h>

int main(void)
{
    int a, b, c, largest;

    printf("Enter three numbers: ");
    scanf("%d %d %d", &a, &b, &c);

    largest = a;

    if (b > largest)
        largest = b;

    if (c > largest)
        largest = c;

    printf("Largest number: %d\n", largest);

    return 0;
}
```

## Output

```text
Enter three numbers: 25 78 42
Largest number: 78
```
