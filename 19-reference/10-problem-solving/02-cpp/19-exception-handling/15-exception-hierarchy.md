# Exception Hierarchy

C++ standard exceptions are organized into a class hierarchy.

A simplified structure is:

```text
std::exception
│
├── std::logic_error
│   ├── std::invalid_argument
│   ├── std::domain_error
│   ├── std::length_error
│   └── std::out_of_range
│
└── std::runtime_error
    ├── std::range_error
    ├── std::overflow_error
    └── std::underflow_error
```

## Example

```cpp
#include <iostream>
#include <stdexcept>

using namespace std;

int main() {
    try {
        throw invalid_argument("Invalid value");
    }
    catch (const logic_error& error) {
        cout << "Logic error: " << error.what() << endl;
    }
    catch (const exception& error) {
        cout << "General exception: " << error.what() << endl;
    }

    return 0;
}
```

## Expected Output

```text
Logic error: Invalid value
```

Because `invalid_argument` derives from `logic_error`, the `logic_error` handler can catch it.
