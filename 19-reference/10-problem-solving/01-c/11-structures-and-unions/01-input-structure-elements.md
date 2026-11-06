# Input Structure Elements

## Problem

Write a C program to define a structure and take input for its members from the user.

## C Program

```c
#include <stdio.h>

struct Student
{
    char name[100];
    int age;
    float marks;
};

int main(void)
{
    struct Student student;

    printf("Enter student name: ");
    scanf(" %99[^\n]", student.name);

    printf("Enter age: ");
    scanf("%d", &student.age);

    printf("Enter marks: ");
    scanf("%f", &student.marks);

    printf("\nStudent Details:\n");
    printf("Name: %s\n", student.name);
    printf("Age: %d\n", student.age);
    printf("Marks: %.2f\n", student.marks);

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
