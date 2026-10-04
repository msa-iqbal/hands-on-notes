# find()

The `find()` algorithm searches for a specific value in a range.

## Example

```cpp
#include <iostream>
#include <vector>
#include <algorithm>

using namespace std;

int main() {
    vector<int> numbers = {10, 20, 30, 40, 50};

    auto result = find(numbers.begin(), numbers.end(), 30);

    if (result != numbers.end()) {
        cout << "Found: " << *result << endl;
    } else {
        cout << "Not Found" << endl;
    }

    return 0;
}
```

## Expected Output

```text
Found: 30
```
