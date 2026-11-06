# Structure Comparison

## Problem

Write a C program to compare the members of two structure variables.

## C Program

```c
#include <stdio.h>
#include <string.h>

struct Student
{
    char name[50];
    int age;
    float marks;
};

int main(void)
{
    struct Student student1;
    struct Student student2;

    printf("Enter first student name: ");
    scanf(" %49[^\n]", student1.name);

    printf("Enter first student age: ");
    scanf("%d", &student1.age);

    printf("Enter first student marks: ");
    scanf("%f", &student1.marks);

    printf("Enter second student name: ");
    scanf(" %49[^\n]", student2.name);

    printf("Enter second student age: ");
    scanf("%d", &student2.age);

    printf("Enter second student marks: ");
    scanf("%f", &student2.marks);

    if (strcmp(student1.name, student2.name) == 0 &&
        student1.age == student2.age &&
        student1.marks == student2.marks)
    {
        printf("The structures are equal.\n");
    }
    else
    {
        printf("The structures are different.\n");
    }

    return 0;
}
```

## Sample Output

```text
Enter first student name: Rahim
Enter first student age: 20
Enter first student marks: 85
Enter second student name: Rahim
Enter second student age: 20
Enter second student marks: 85
The structures are equal.
```
