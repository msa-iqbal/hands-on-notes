# Summation and Average

Write a C program to calculate the sum and average of numbers.

## Program

```c
#include <stdio.h>

int main(void)
{
    int n, i;
    double number, sum = 0.0, average;

    printf("Enter the number of values: ");
    scanf("%d", &n);

    for (i = 1; i <= n; i++)
    {
        printf("Enter value %d: ", i);
        scanf("%lf", &number);

        sum += number;
    }

    average = sum / n;

    printf("Sum = %.2f\n", sum);
    printf("Average = %.2f\n", average);

    return 0;
}
```

## Output

```text
Enter the number of values: 5
Enter value 1: 10
Enter value 2: 20
Enter value 3: 30
Enter value 4: 40
Enter value 5: 50
Sum = 150.00
Average = 30.00
```
