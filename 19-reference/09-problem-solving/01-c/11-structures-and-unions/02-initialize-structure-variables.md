# Initialize Structure Variables

## Problem

Write a C program to initialize a structure variable and display its values.

## C Program

```c
#include <stdio.h>

struct Student
{
    char name[50];
    int age;
    float marks;
};

int main(void)
{
    struct Student student = {"Rahim", 20, 85.50};

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
