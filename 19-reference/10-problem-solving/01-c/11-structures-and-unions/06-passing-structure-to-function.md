# Passing Structure to Function

## Problem

Write a C program to pass a structure variable to a function and display its members.

## C Program

```c
#include <stdio.h>

struct Student
{
    char name[50];
    int age;
    float marks;
};

void displayStudent(struct Student student)
{
    printf("Name: %s\n", student.name);
    printf("Age: %d\n", student.age);
    printf("Marks: %.2f\n", student.marks);
}

int main(void)
{
    struct Student student;

    printf("Enter student name: ");
    scanf(" %49[^\n]", student.name);

    printf("Enter age: ");
    scanf("%d", &student.age);

    printf("Enter marks: ");
    scanf("%f", &student.marks);

    printf("\nStudent Details:\n");
    displayStudent(student);

    return 0;
}
```

## Sample Output

```text
Enter student name: Rahim
Enter age: 20
Enter marks: 85.5

Student Details:
Name: Rahim
Age: 20
Marks: 85.50
```
