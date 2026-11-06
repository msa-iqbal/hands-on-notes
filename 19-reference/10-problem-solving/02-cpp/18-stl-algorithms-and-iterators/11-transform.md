# transform()

The `transform()` algorithm applies an operation to every element in a range.

## Example

```cpp
#include <iostream>
#include <vector>
#include <algorithm>

using namespace std;

int main() {
    vector<int> numbers = {1, 2, 3, 4, 5};
    vector<int> doubled(numbers.size());

    transform(
        numbers.begin(),
        numbers.end(),
        doubled.begin(),
        [](int number) {
            return number * 2;
        }
    );

    for (int number : doubled) {
        cout << number << " ";
    }

    return 0;
}
```

## Expected Output

```text
2 4 6 8 10
```
