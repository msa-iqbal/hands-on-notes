# Create and Close a File

## Problem

Write a C program to create a file and close it using `fopen()` and `fclose()`.

## C Program

```c
#include <stdio.h>

int main(void)
{
    FILE *file;

    file = fopen("example.txt", "w");

    if (file == NULL)
    {
        printf("Unable to create the file.\n");
        return 1;
    }

    printf("File created successfully.\n");

    fclose(file);

    printf("File closed successfully.\n");

    return 0;
}
```

## Sample Output

```text
File created successfully.
File closed successfully.
```
