# Rethrowing Exceptions

The `throw;` statement inside a `catch` block can rethrow the currently handled exception.

## Example

```cpp
#include <iostream>

using namespace std;

void process() {
    try {
        throw 404;
    }
    catch (int error) {
        cout << "Handled locally: " << error << endl;

        throw;
    }
}

int main() {
    try {
        process();
    }
    catch (int error) {
        cout << "Handled in main: " << error << endl;
    }

    return 0;
}
```

## Expected Output

```text
Handled locally: 404
Handled in main: 404
```
