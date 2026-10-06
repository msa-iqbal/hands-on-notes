# Count Vowels, Consonants, Digits, and Words

## Problem

Write a C program to count the number of vowels, consonants, digits, and words in a string.

## C Program

```c
#include <stdio.h>
#include <ctype.h>

int main(void)
{
    char str[200];
    int vowels = 0;
    int consonants = 0;
    int digits = 0;
    int words = 0;
    int inWord = 0;

    printf("Enter a string: ");
    fgets(str, sizeof(str), stdin);

    for (int i = 0; str[i] != '\0'; i++)
    {
        unsigned char ch = (unsigned char)str[i];

        if (isalpha(ch))
        {
            if (ch == 'a' || ch == 'e' || ch == 'i' ||
                ch == 'o' || ch == 'u' ||
                ch == 'A' || ch == 'E' || ch == 'I' ||
                ch == 'O' || ch == 'U')
            {
                vowels++;
            }
            else
            {
                consonants++;
            }
        }
        else if (isdigit(ch))
        {
            digits++;
        }

        if (!isspace(ch) && !inWord)
        {
            words++;
            inWord = 1;
        }
        else if (isspace(ch))
        {
            inWord = 0;
        }
    }

    printf("Vowels = %d\n", vowels);
    printf("Consonants = %d\n", consonants);
    printf("Digits = %d\n", digits);
    printf("Words = %d\n", words);

    return 0;
}
```

## Sample Output

```text
Enter a string: Hello World 123
Vowels = 3
Consonants = 7
Digits = 3
Words = 3
```
