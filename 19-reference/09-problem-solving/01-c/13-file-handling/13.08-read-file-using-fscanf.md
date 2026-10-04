# Read Data from a File Using fscanf()

## Problem

Write a C program to read formatted data from a file using `fscanf()`.

Assume the file contains a student's name, age, and marks.

## C Program

```c
#include <stdio.h>

int main(void)
{
    FILE *file;

    char name[100];
    int age;
    float marks;

    file = fopen("student.txt", "r");

    if (file == NULL)
    {
        printf("Unable to open the file.\n");
        return 1;
    }

    fscanf(file, "%99s %d %f", name, &age, &marks);

    fclose(file);

    printf("Student Details:\n");
    printf("Name: %s\n", name);
    printf("Age: %d\n", age);
    printf("Marks: %.2f\n", marks);

    return 0;
}
```

## Sample File

`student.txt`:

```text
Rahim 20 85.50
```

## Sample Output

```text
Student Details:
Name: Rahim
Age: 20
Marks: 85.50
```
