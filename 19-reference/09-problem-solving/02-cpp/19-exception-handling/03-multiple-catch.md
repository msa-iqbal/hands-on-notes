# Multiple catch Blocks

A single `try` block can have multiple `catch` blocks for handling different exception types.

## Example

```cpp
#include <iostream>

using namespace std;

int main() {
    try {
        throw 10.5;
    }
    catch (int error) {
        cout << "Integer exception: " << error << endl;
    }
    catch (double error) {
        cout << "Double exception: " << error << endl;
    }
    catch (const char* error) {
        cout << "String exception: " << error << endl;
    }

    return 0;
}
```

## Expected Output

```text
Double exception: 10.5
```
