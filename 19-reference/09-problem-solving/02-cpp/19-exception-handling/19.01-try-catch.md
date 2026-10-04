# try-catch

C++ uses `try` and `catch` blocks to handle exceptions.

## Example

```cpp
#include <iostream>

using namespace std;

int main() {
    try {
        throw 10;
    }
    catch (int error) {
        cout << "Exception caught: " << error << endl;
    }

    return 0;
}
```

## Expected Output

```text
Exception caught: 10
```
