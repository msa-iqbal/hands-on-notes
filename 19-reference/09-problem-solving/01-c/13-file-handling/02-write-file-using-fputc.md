# Write to a File Using fputc()

## Problem

Write a C program to write characters to a file using the `fputc()` function.

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

    fputc('H', file);
    fputc('e', file);
    fputc('l', file);
    fputc('l', file);
    fputc('o', file);
    fputc('\n', file);

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
Hello
```
