# String Swapping

## Problem

Write a C program to swap the contents of two strings.

## C Program

```c
#include <stdio.h>
#include <string.h>

int main(void)
{
    char first[100];
    char second[100];
    char temp[100];

    printf("Enter first string: ");
    fgets(first, sizeof(first), stdin);

    printf("Enter second string: ");
    fgets(second, sizeof(second), stdin);

    first[strcspn(first, "\n")] = '\0';
    second[strcspn(second, "\n")] = '\0';

    strcpy(temp, first);
    strcpy(first, second);
    strcpy(second, temp);

    printf("After swapping:\n");
    printf("First string: %s\n", first);
    printf("Second string: %s\n", second);

    return 0;
}
```

## Sample Output

```text
Enter first string: Hello
Enter second string: World
After swapping:
First string: World
Second string: Hello
```
