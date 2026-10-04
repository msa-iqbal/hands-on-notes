# reverse()

The `reverse()` algorithm reverses the order of elements in a range.

## Example

```cpp
#include <iostream>
#include <vector>
#include <algorithm>

using namespace std;

int main() {
    vector<int> numbers = {10, 20, 30, 40, 50};

    reverse(numbers.begin(), numbers.end());

    for (int number : numbers) {
        cout << number << " ";
    }

    return 0;
}
```

## Expected Output

```text
50 40 30 20 10
```
