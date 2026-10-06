# Write to a File Using fputs()

## Problem

Write a C program to write a string to a file using the `fputs()` function.

## C Program

```c
#include <stdio.h>

int main(void)
{
    FILE *file;

    file = fopen("example.txt", "w");

    if (file == NULL)
    {
        printf("Unable to open the file.\n");
        return 1;
    }

    fputs("Hello, World!\n", file);
    fputs("This text is written using fputs().\n", file);

    fclose(file);

    printf("Data written to file successfully.\n");

    return 0;
}
```

## Sample Output

```text
Data written to file successfully.
```

The contents of `example.txt` will be:

```text
Hello, World!
This text is written using fputs().
```
