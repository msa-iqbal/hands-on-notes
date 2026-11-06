# Structure with Array Within Structure

## Problem

Write a C program to create a structure containing an array and display the stored values.

## C Program

```c
#include <stdio.h>

struct Student
{
    char name[50];
    int marks[5];
};

int main(void)
{
    struct Student student;

    printf("Enter student name: ");
    scanf(" %49[^\n]", student.name);

    printf("Enter marks for 5 subjects:\n");

    for (int i = 0; i < 5; i++)
    {
        scanf("%d", &student.marks[i]);
    }

    printf("\nStudent Details:\n");
    printf("Name: %s\n", student.name);
    printf("Marks: ");

    for (int i = 0; i < 5; i++)
    {
        printf("%d", student.marks[i]);

        if (i < 4)
            printf(" ");
    }

    printf("\n");

    return 0;
}
```

## Sample Output

```text
Enter student name: Rahim
Enter marks for 5 subjects:
80 85 90 75 88

Student Details:
Name: Rahim
Marks: 80 85 90 75 88
```
