# out_of_range

`out_of_range` is commonly used when an index or value is outside an allowed range.

## Example

```cpp
#include <iostream>
#include <vector>
#include <stdexcept>

using namespace std;

int main() {
    vector<int> numbers = {10, 20, 30};

    try {
        cout << numbers.at(10) << endl;
    }
    catch (const out_of_range& error) {
        cout << "Error: " << error.what() << endl;
    }

    return 0;
}
```

## Expected Output

```text
Error: vector::_M_range_check
```

The exact error message may vary between C++ standard library implementations.
