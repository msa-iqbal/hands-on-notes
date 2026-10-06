# accumulate()

The `accumulate()` algorithm calculates the sum of elements in a range.

It is provided by the `<numeric>` header.

## Example

```cpp
#include <iostream>
#include <vector>
#include <numeric>

using namespace std;

int main() {
    vector<int> numbers = {10, 20, 30, 40, 50};

    int sum = accumulate(numbers.begin(), numbers.end(), 0);

    cout << "Sum: " << sum << endl;

    return 0;
}
```

## Expected Output

```text
Sum: 150
```
