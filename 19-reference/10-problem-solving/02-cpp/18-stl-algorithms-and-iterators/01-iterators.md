# Iterators

Iterators are objects used to traverse elements of STL containers.

## Example

```cpp
#include <iostream>
#include <vector>

using namespace std;

int main() {
    vector<int> numbers = {10, 20, 30, 40, 50};

    vector<int>::iterator it;

    for (it = numbers.begin(); it != numbers.end(); ++it) {
        cout << *it << " ";
    }

    return 0;
}
```

## Expected Output

```text
10 20 30 40 50
```
