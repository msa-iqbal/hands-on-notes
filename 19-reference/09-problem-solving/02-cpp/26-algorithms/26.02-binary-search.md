# Binary Search

Binary search repeatedly divides a **sorted** search range into two halves.

```text
[10, 20, 30, 40, 50, 60, 70]

Search: 60

middle = 40
60 > 40
search right half

[50, 60, 70]

middle = 60
found
```

## Basic Example

```cpp
#include <iostream>
using namespace std;

int binarySearch(int numbers[], int size, int target) {
    int left = 0;
    int right = size - 1;

    while (left <= right) {
        int middle = left + (right - left) / 2;

        if (numbers[middle] == target) {
            return middle;
        }

        if (numbers[middle] < target) {
            left = middle + 1;
        } else {
            right = middle - 1;
        }
    }

    return -1;
}

int main() {
    int numbers[] = {10, 20, 30, 40, 50, 60, 70};

    int result = binarySearch(numbers, 7, 60);

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
Found at index: 5
```

## Recursive Binary Search

```cpp
#include <iostream>
using namespace std;

int binarySearch(
    int numbers[],
    int left,
    int right,
    int target
) {
    if (left > right) {
        return -1;
    }

    int middle = left + (right - left) / 2;

    if (numbers[middle] == target) {
        return middle;
    }

    if (numbers[middle] < target) {
        return binarySearch(
            numbers,
            middle + 1,
            right,
            target
        );
    }

    return binarySearch(
        numbers,
        left,
        middle - 1,
        target
    );
}

int main() {
    int numbers[] = {
        10, 20, 30, 40, 50, 60, 70
    };

    int result = binarySearch(
        numbers,
        0,
        6,
        40
    );

    cout << result << endl;

    return 0;
}
```

### Output

```text
3
```

## Using `std::binary_search`

```cpp
#include <algorithm>
#include <iostream>
using namespace std;

int main() {
    int numbers[] = {
        10, 20, 30, 40, 50
    };

    bool found = binary_search(
        numbers,
        numbers + 5,
        30
    );

    cout << boolalpha << found << endl;

    return 0;
}
```

### Output

```text
true
```

## Important Requirement

The input must be sorted for ordinary binary search.

Incorrect:

```text
40 10 50 20 30
```

Correct:

```text
10 20 30 40 50
```

## Complexity

|Case|Complexity|
|---|--:|
|Best|O(1)|
|Average|O(log n)|
|Worst|O(log n)|
|Space — iterative|O(1)|
|Space — recursive|O(log n)|

## Linear Search vs Binary Search

| Feature              | Linear Search  | Binary Search |
| -------------------- | -------------- | ------------- |
| Sorted data required | No             | Yes           |
| Worst case           | O(n)           | O(log n)      |
| Implementation       | Simple         | More involved |
| Large sorted data    | Less efficient | Efficient     |
