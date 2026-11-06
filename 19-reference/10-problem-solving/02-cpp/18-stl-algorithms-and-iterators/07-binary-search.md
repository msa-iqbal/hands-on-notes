# binary_search()

The `binary_search()` algorithm checks whether a value exists in a **sorted** range.

## Example

```cpp
#include <iostream>
#include <vector>
#include <algorithm>

using namespace std;

int main() {
    vector<int> numbers = {10, 20, 30, 40, 50};

    bool found = binary_search(numbers.begin(), numbers.end(), 30);

    if (found) {
        cout << "Element found" << endl;
    } else {
        cout << "Element not found" << endl;
    }

    return 0;
}
```

## Expected Output

```text
Element found
```
