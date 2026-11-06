# Summation and Average of Array Elements

## Problem

Write a C program to find the sum and average of all elements in an array.

## C Program

```c
#include <stdio.h>

int main(void)
{
    int n;
    int sum = 0;
    float average;

    printf("Enter the number of elements: ");
    scanf("%d", &n);

    int arr[n];

    printf("Enter %d elements:\n", n);

    for (int i = 0; i < n; i++)
    {
        scanf("%d", &arr[i]);
        sum += arr[i];
    }

    average = (float)sum / n;

    printf("Sum = %d\n", sum);
    printf("Average = %.2f\n", average);

    return 0;
}
```

## Sample Output

```text
Enter the number of elements: 5
Enter 5 elements:
10 20 30 40 50
Sum = 150
Average = 30.00
```
