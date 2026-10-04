# Number Conversions Using Switch

Write a C program to perform decimal, octal, and hexadecimal number conversions using a `switch` statement.

## Program

```c
#include <stdio.h>

int main(void)
{
    int choice;
    int number;

    printf("Number Conversion\n");
    printf("1. Decimal to Octal\n");
    printf("2. Decimal to Hexadecimal\n");
    printf("3. Octal to Decimal\n");
    printf("4. Hexadecimal to Decimal\n");

    printf("Enter your choice: ");
    scanf("%d", &choice);

    switch (choice)
    {
        case 1:
            printf("Enter a decimal number: ");
            scanf("%d", &number);

            printf("Octal = %o\n", number);
            break;

        case 2:
            printf("Enter a decimal number: ");
            scanf("%d", &number);

            printf("Hexadecimal = %X\n", number);
            break;

        case 3:
            printf("Enter an octal number: ");
            scanf("%o", &number);

            printf("Decimal = %d\n", number);
            break;

        case 4:
            printf("Enter a hexadecimal number: ");
            scanf("%x", &number);

            printf("Decimal = %d\n", number);
            break;

        default:
            printf("Invalid choice.\n");
    }

    return 0;
}
```

## Output

```text
Number Conversion
1. Decimal to Octal
2. Decimal to Hexadecimal
3. Octal to Decimal
4. Hexadecimal to Decimal
Enter your choice: 2
Enter a decimal number: 255
Hexadecimal = FF
```
