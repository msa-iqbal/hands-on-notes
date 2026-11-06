# lower_bound()

The `lower_bound()` algorithm returns an iterator pointing to the first element that is **greater than or equal to** a given value.

## Example

```cpp
#include <iostream>
#include <vector>
#include <algorithm>

using namespace std;

int main() {
    vector<int> numbers = {10, 20, 20, 30, 40, 50};

    auto result = lower_bound(numbers.begin(), numbers.end(), 20);

    cout << "Position: " << result - numbers.begin() << endl;
    cout << "Value: " << *result << endl;

    return 0;
}
```

## Expected Output

```text
Position: 1
Value: 20
```
