# Linear Search

Linear search checks each element one by one until the target value is found or the end of the collection is reached.

## Basic Example

```cpp
#include <iostream>
using namespace std;

int main() {
    int numbers[] = {10, 20, 30, 40, 50};
    int size = 5;
    int target = 30;

    int index = -1;

    for (int i = 0; i < size; i++) {
        if (numbers[i] == target) {
            index = i;
            break;
        }
    }

    if (index != -1) {
        cout << "Found at index: " << index << endl;
    } else {
        cout << "Not Found" << endl;
    }

    return 0;
}
```

### Output

```text
Found at index: 2
```

## Using a Function

```cpp
#include <iostream>
using namespace std;

int linearSearch(int numbers[], int size, int target) {
    for (int i = 0; i < size; i++) {
        if (numbers[i] == target) {
            return i;
        }
    }

    return -1;
}

int main() {
    int numbers[] = {5, 15, 25, 35, 45};

    int result = linearSearch(numbers, 5, 35);

    if (result != -1) {
        cout << "Found at index: " << result << endl;
    } else {
        cout << "Not Found" << endl;
    }

    return 0;
}
```

### Output

```text
Found at index: 3
```

## Search for All Occurrences

```cpp
#include <iostream>
using namespace std;

int main() {
    int numbers[] = {10, 20, 10, 30, 10};
    int size = 5;
    int target = 10;

    for (int i = 0; i < size; i++) {
        if (numbers[i] == target) {
            cout << "Found at index: " << i << endl;
        }
    }

    return 0;
}
```

### Output

```text
Found at index: 0
Found at index: 2
Found at index: 4
```

## Complexity

|Case|Complexity|
|---|--:|
|Best|O(1)|
|Average|O(n)|
|Worst|O(n)|
|Space|O(1)|

## Key Points

- Works on sorted and unsorted data.
- Very simple to implement.
- No preprocessing is required.
- Inefficient for large datasets compared with suitable faster search algorithms.
