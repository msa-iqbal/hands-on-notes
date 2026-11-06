# count()

The `count()` algorithm returns the number of times a value appears in a range.

## Example

```cpp
#include <iostream>
#include <vector>
#include <algorithm>

using namespace std;

int main() {
    vector<int> numbers = {10, 20, 10, 30, 10, 40};

    int result = count(numbers.begin(), numbers.end(), 10);

    cout << "Count: " << result << endl;

    return 0;
}
```

## Expected Output

```text
Count: 3
```
