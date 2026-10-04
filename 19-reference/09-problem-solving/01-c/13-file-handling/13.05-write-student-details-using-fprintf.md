# Write Student Details Using fprintf()

## Problem

Write a C program to take student details from the user and save them to a file using `fprintf()`.

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
    FILE *file;

    printf("Enter student name: ");
    scanf(" %99[^\n]", student.name);

    printf("Enter age: ");
    scanf("%d", &student.age);

    printf("Enter marks: ");
    scanf("%f", &student.marks);

    file = fopen("student.txt", "w");

    if (file == NULL)
    {
        printf("Unable to open the file.\n");
        return 1;
    }

    fprintf(file, "Student Details\n");
    fprintf(file, "Name: %s\n", student.name);
    fprintf(file, "Age: %d\n", student.age);
    fprintf(file, "Marks: %.2f\n", student.marks);

    fclose(file);

    printf("Student details saved successfully.\n");

    return 0;
}
```

## Sample Output

```text
Enter student name: Rahim
Enter age: 20
Enter marks: 85.5
Student details saved successfully.
```

The contents of `student.txt` will be:

```text
Student Details
Name: Rahim
Age: 20
Marks: 85.50
```
