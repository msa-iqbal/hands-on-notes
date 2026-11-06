# Calculator Using Switch

Write a C program to perform addition, subtraction, multiplication, and division using a `switch` statement.

## Program

```c
#include <stdio.h>

int main(void)
{
    double num1, num2, result;
    char operator;

    printf("Enter an expression (e.g. 10 + 5): ");
    scanf("%lf %c %lf", &num1, &operator, &num2);

    switch (operator)
    {
        case '+':
            result = num1 + num2;
            printf("Result: %.2f\n", result);
            break;

        case '-':
            result = num1 - num2;
            printf("Result: %.2f\n", result);
            break;

        case '*':
            result = num1 * num2;
            printf("Result: %.2f\n", result);
            break;

        case '/':
            if (num2 == 0)
            {
                printf("Error: Division by zero is not allowed.\n");
            }
            else
            {
                result = num1 / num2;
                printf("Result: %.2f\n", result);
            }
            break;

        default:
            printf("Invalid operator.\n");
    }

    return 0;
}
```

## Output

```text
Enter an expression (e.g. 10 + 5): 20 * 5
Result: 100.00
```
