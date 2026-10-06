# Typedef for User-Defined Data Types

## Problem

Write a C program to create a type alias for a user-defined structure using `typedef`.

## C Program

```c
#include <stdio.h>
#include <string.h>

typedef struct
{
    char name[50];
    int age;
    float marks;
} Student;

int main(void)
{
    Student student;

    strcpy(student.name, "Rahim");
    student.age = 20;
    student.marks = 85.50;

    printf("Student Details:\n");
    printf("Name: %s\n", student.name);
    printf("Age: %d\n", student.age);
    printf("Marks: %.2f\n", student.marks);

    return 0;
}
```

## Sample Output

```text
Student Details:
Name: Rahim
Age: 20
Marks: 85.50
```
