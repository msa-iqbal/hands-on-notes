# String Palindrome

Write a C++ program to check whether a string is a palindrome.

## Program

```cpp
#include <iostream>
#include <string>
using namespace std;

int main() {
    string text;

    cout << "Enter a string: ";
    getline(cin, text);

    bool palindrome = true;

    int left = 0;
    int right = static_cast<int>(text.length()) - 1;

    while (left < right) {
        if (text[left] != text[right]) {
            palindrome = false;
            break;
        }

        left++;
        right--;
    }

    if (palindrome) {
        cout << "The string is a palindrome." << endl;
    } else {
        cout << "The string is not a palindrome." << endl;
    }

    return 0;
}
```

## Sample Output

```text
Enter a string: madam
The string is a palindrome.
```
