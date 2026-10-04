# Vowel or Consonant

Write a C program to determine whether a given character is a vowel or consonant.

## Program

```c
#include <stdio.h>

int main(void)
{
    char ch;

    printf("Enter an alphabet: ");
    scanf("%c", &ch);

    if ((ch >= 'A' && ch <= 'Z') ||
        (ch >= 'a' && ch <= 'z'))
    {
        if (ch == 'A' || ch == 'E' || ch == 'I' ||
            ch == 'O' || ch == 'U' ||
            ch == 'a' || ch == 'e' || ch == 'i' ||
            ch == 'o' || ch == 'u')
        {
            printf("%c is a vowel.\n", ch);
        }
        else
        {
            printf("%c is a consonant.\n", ch);
        }
    }
    else
    {
        printf("Invalid input.\n");
    }

    return 0;
}
```

## Output

```text
Enter an alphabet: E
E is a vowel.
```
