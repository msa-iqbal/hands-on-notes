# Student Grade System

Write a C program to calculate a student's grade from their marks.

The grading system is:

|Marks|Grade|
|--:|:-:|
|80–100|A+|
|70–79|A|
|60–69|A-|
|50–59|B|
|40–49|C|
|33–39|D|
|0–32|F|

## Program

```c
#include <stdio.h>

int main(void)
{
    float marks;

    printf("Enter marks: ");
    scanf("%f", &marks);

    if (marks < 0 || marks > 100)
    {
        printf("Invalid marks.\n");
    }
    else if (marks >= 80)
    {
        printf("Grade: A+\n");
    }
    else if (marks >= 70)
    {
        printf("Grade: A\n");
    }
    else if (marks >= 60)
    {
        printf("Grade: A-\n");
    }
    else if (marks >= 50)
    {
        printf("Grade: B\n");
    }
    else if (marks >= 40)
    {
        printf("Grade: C\n");
    }
    else if (marks >= 33)
    {
        printf("Grade: D\n");
    }
    else
    {
        printf("Grade: F\n");
    }

    return 0;
}
```

## Output

```text
Enter marks: 85
Grade: A+
```
