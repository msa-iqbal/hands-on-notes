# Convert String to Uppercase and Lowercase

## Problem

Write a C program to convert a string to uppercase and lowercase.

## C Program

```c
#include <stdio.h>
#include <ctype.h>
#include <string.h>

int main(void)
{
    char str[100];
    char upper[100];
    char lower[100];

    printf("Enter a string: ");
    fgets(str, sizeof(str), stdin);

    str[strcspn(str, "\n")] = '\0';

    int i = 0;

    while (str[i] != '\0')
    {
        upper[i] = toupper((unsigned char)str[i]);
        lower[i] = tolower((unsigned char)str[i]);
        i++;
    }

    upper[i] = '\0';
    lower[i] = '\0';

    printf("Uppercase: %s\n", upper);
    printf("Lowercase: %s\n", lower);

    return 0;
}
```

## Sample Output

```text
Enter a string: Hello World
Uppercase: HELLO WORLD
Lowercase: hello world
```
