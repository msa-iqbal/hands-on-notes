# unique()

The `unique()` algorithm removes consecutive duplicate elements from a range.

It is commonly used with `erase()` to actually remove the duplicated elements from a container.

## Example

```cpp
#include <iostream>
#include <vector>
#include <algorithm>

using namespace std;

int main() {
    vector<int> numbers = {
        10, 10, 20, 20, 20, 30, 30, 40
    };

    auto newEnd = unique(numbers.begin(), numbers.end());

    numbers.erase(newEnd, numbers.end());

    for (int number : numbers) {
        cout << number << " ";
    }

    return 0;
}
```

## Expected Output

```text
10 20 30 40
```
