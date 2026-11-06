# Read a File Using fgets()

## Problem

Write a C program to read a file line by line using the `fgets()` function.

## C Program

```c
#include <stdio.h>

int main(void)
{
    FILE *file;
    char line[200];

    file = fopen("example.txt", "r");

    if (file == NULL)
    {
        printf("Unable to open the file.\n");
        return 1;
    }

    printf("File contents:\n");

    while (fgets(line, sizeof(line), file) != NULL)
    {
        printf("%s", line);
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
File handling is useful.
```

Output:

```text
File contents:
Hello, World!
Welcome to C programming.
File handling is useful.
```
