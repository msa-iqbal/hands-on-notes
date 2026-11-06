# Array of Structures

## Problem

Write a C program to store and display the details of multiple students using an array of structures.

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
    int n;

    printf("Enter number of students: ");
    scanf("%d", &n);

    struct Student students[n];

    for (int i = 0; i < n; i++)
    {
        printf("\nStudent %d\n", i + 1);

        printf("Enter name: ");
        scanf(" %49[^\n]", students[i].name);

        printf("Enter age: ");
        scanf("%d", &students[i].age);

        printf("Enter marks: ");
        scanf("%f", &students[i].marks);
    }

    printf("\nStudent Details:\n");

    for (int i = 0; i < n; i++)
    {
        printf("\nStudent %d\n", i + 1);
        printf("Name: %s\n", students[i].name);
        printf("Age: %d\n", students[i].age);
        printf("Marks: %.2f\n", students[i].marks);
    }

    return 0;
}
```

## Sample Output

```text
Enter number of students: 2

Student 1
Enter name: Rahim
Enter age: 20
Enter marks: 85

Student 2
Enter name: Karim
Enter age: 21
Enter marks: 90

Student Details:

Student 1
Name: Rahim
Age: 20
Marks: 85.00

Student 2
Name: Karim
Age: 21
Marks: 90.00
```
