# Passing String to Function

## Problem

Write a C program to pass a string to a function and display the string from that function.

## C Program

```c
#include <stdio.h>

void displayString(char str[])
{
    printf("String: %s\n", str);
}

int main(void)
{
    char str[100];

    printf("Enter a string: ");
    fgets(str, sizeof(str), stdin);

    displayString(str);

    return 0;
}
```

## Sample Output

```text
Enter a string: Hello World
String: Hello World
```
