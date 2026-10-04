# noexcept

`noexcept` specifies that a function is not expected to throw exceptions.

## Example

```cpp
#include <iostream>

using namespace std;

void display() noexcept {
    cout << "This function does not throw exceptions." << endl;
}

int main() {
    display();

    return 0;
}
```

## Expected Output

```text
This function does not throw exceptions.
```

## Checking noexcept

You can use the `noexcept` operator to determine whether an expression is declared non-throwing.

```cpp
#include <iostream>

using namespace std;

void safeFunction() noexcept {
}

void normalFunction() {
}

int main() {
    cout << boolalpha;

    cout << noexcept(safeFunction()) << endl;
    cout << noexcept(normalFunction()) << endl;

    return 0;
}
```

## Expected Output

```text
true
false
```
