# Read a File Using fgetc()

## Problem

Write a C program to read and display the contents of a file character by character using `fgetc()`.

## C Program

```c
#include <stdio.h>

int main(void)
{
    FILE *file;
    int character;

    file = fopen("example.txt", "r");

    if (file == NULL)
    {
        printf("Unable to open the file.\n");
        return 1;
    }

    printf("File contents:\n");

    while ((character = fgetc(file)) != EOF)
    {
        putchar(character);
    }

    fclose(file);

    return 0;
}
```

## Sample Output

Assuming `example.txt` contains:

```text
Hello, World!
Welcome to C programming.
```

Output:

```text
File contents:
Hello, World!
Welcome to C programming.
```
