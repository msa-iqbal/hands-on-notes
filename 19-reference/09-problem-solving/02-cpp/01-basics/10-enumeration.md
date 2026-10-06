# Enumeration

Write a C++ program to demonstrate an enumeration using the `enum` keyword.

## C++ Program

```cpp
#include <iostream>
using namespace std;

enum Day {
    Monday,
    Tuesday,
    Wednesday,
    Thursday,
    Friday,
    Saturday,
    Sunday
};

int main() {
    Day today = Wednesday;

    cout << "Today is day number: "
         << static_cast<int>(today) + 1 << endl;

    if (today == Wednesday) {
        cout << "Today is Wednesday." << endl;
    }

    return 0;
}
```

## Sample Output

```text
Today is day number: 3
Today is Wednesday.
```

> By default, the first enumerator has the value `0`, the next has `1`, and so on.
