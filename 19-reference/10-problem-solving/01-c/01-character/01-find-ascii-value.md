### Problem

Write a C program to find the ASCII value of a given character.

### C Program

```c
#include <stdio.h>

int main(void)
{
    char ch;

    printf("Enter a character: ");
    scanf("%c", &ch);

    printf("ASCII value of '%c' = %d\n", ch, ch);

    return 0;
}
```

### Sample Output

```text
Enter a character: A
ASCII value of 'A' = 65
```
