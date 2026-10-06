# sort()

The `sort()` algorithm arranges elements in ascending order by default.

## Example

```cpp
#include <iostream>
#include <vector>
#include <algorithm>

using namespace std;

int main() {
    vector<int> numbers = {50, 20, 40, 10, 30};

    sort(numbers.begin(), numbers.end());

    for (int number : numbers) {
        cout << number << " ";
    }

    return 0;
}
```

## Expected Output

```text
10 20 30 40 50
```
