# runtime_error

`runtime_error` represents an error detected during program execution.

## Example

```cpp
#include <iostream>
#include <stdexcept>

using namespace std;

int divide(int a, int b) {
    if (b == 0) {
        throw runtime_error("Division by zero");
    }

    return a / b;
}

int main() {
    try {
        cout << divide(10, 0) << endl;
    }
    catch (const runtime_error& error) {
        cout << "Error: " << error.what() << endl;
    }

    return 0;
}
```

## Expected Output

```text
Error: Division by zero
```
