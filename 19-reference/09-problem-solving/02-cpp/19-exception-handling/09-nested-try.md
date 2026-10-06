# Nested try Blocks

A `try` block can be placed inside another `try` block.

## Example

```cpp
#include <iostream>

using namespace std;

int main() {
    try {
        try {
            throw 100;
        }
        catch (int error) {
            cout << "Inner catch: " << error << endl;

            throw;
        }
    }
    catch (int error) {
        cout << "Outer catch: " << error << endl;
    }

    return 0;
}
```

## Expected Output

```text
Inner catch: 100
Outer catch: 100
```
