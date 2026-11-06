# String Palindrome

## Problem

Write a C program to check whether a string is a palindrome.

A palindrome reads the same from left to right and right to left.

## C Program

```c
#include <stdio.h>
#include <string.h>

int main(void)
{
    char str[100];
    int left = 0;
    int right;
    int isPalindrome = 1;

    printf("Enter a string: ");
    fgets(str, sizeof(str), stdin);

    str[strcspn(str, "\n")] = '\0';

    right = strlen(str) - 1;

    while (left < right)
    {
        if (str[left] != str[right])
        {
            isPalindrome = 0;
            break;
        }

        left++;
        right--;
    }

    if (isPalindrome)
        printf("The string is a palindrome.\n");
    else
        printf("The string is not a palindrome.\n");

    return 0;
}
```

## Sample Output

```text
Enter a string: madam
The string is a palindrome.
```
