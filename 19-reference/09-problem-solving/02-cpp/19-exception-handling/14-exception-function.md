# Exception Handling in Functions

A function can throw an exception and let the caller handle it.

## Example

```cpp
#include <iostream>
#include <stdexcept>

using namespace std;

double divide(double a, double b) {
    if (b == 0) {
        throw runtime_error("Cannot divide by zero");
    }

    return a / b;
}

int main() {
    try {
        double result = divide(20, 0);

        cout << result << endl;
    }
    catch (const runtime_error& error) {
        cout << "Error: " << error.what() << endl;
    }

    return 0;
}
```

## Expected Output

```text
Error: Cannot divide by zero
```
