# Standard Exceptions

C++ provides standard exception classes through the `<stdexcept>` header.

Common classes include:

- `exception`
- `runtime_error`
- `logic_error`
- `invalid_argument`
- `out_of_range`
- `length_error`

## Example

```cpp
#include <iostream>
#include <stdexcept>

using namespace std;

int main() {
    try {
        throw runtime_error("Something went wrong");
    }
    catch (const runtime_error& error) {
        cout << "Error: " << error.what() << endl;
    }

    return 0;
}
```

## Expected Output

```text
Error: Something went wrong
```
