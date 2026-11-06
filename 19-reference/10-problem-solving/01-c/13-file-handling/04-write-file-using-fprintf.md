# Write to a File Using fprintf()

## Problem

Write a C program to write formatted data to a file using the `fprintf()` function.

## C Program

```c
#include <stdio.h>

int main(void)
{
    FILE *file;

    file = fopen("student.txt", "w");

    if (file == NULL)
    {
        printf("Unable to open the file.\n");
        return 1;
    }

    fprintf(file, "Name: Rahim\n");
    fprintf(file, "Age: %d\n", 20);
    fprintf(file, "Marks: %.2f\n", 85.50);

    fclose(file);

    printf("Formatted data written successfully.\n");

    return 0;
}
```

## Sample Output

```text
Formatted data written successfully.
```

The contents of `student.txt` will be:

```text
Name: Rahim
Age: 20
Marks: 85.50
```
