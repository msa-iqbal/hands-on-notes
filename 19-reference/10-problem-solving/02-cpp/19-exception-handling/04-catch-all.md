# Catch-All Handler

The `catch (...)` syntax can catch exceptions of any type.

## Example

```cpp
#include <iostream>

using namespace std;

int main() {
    try {
        throw 100;
    }
    catch (...) {
        cout << "Unknown exception caught" << endl;
    }

    return 0;
}
```

## Expected Output

```text
Unknown exception caught
```
