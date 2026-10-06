# min_element() and max_element()

The `min_element()` algorithm finds the smallest element, while `max_element()` finds the largest element.

## Example

```cpp
#include <iostream>
#include <vector>
#include <algorithm>

using namespace std;

int main() {
    vector<int> numbers = {45, 12, 78, 23, 9, 56};

    auto minimum = min_element(numbers.begin(), numbers.end());
    auto maximum = max_element(numbers.begin(), numbers.end());

    cout << "Minimum: " << *minimum << endl;
    cout << "Maximum: " << *maximum << endl;

    return 0;
}
```

## Expected Output

```text
Minimum: 9
Maximum: 78
```
