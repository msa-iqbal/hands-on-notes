# Selection Sort

Selection sort repeatedly finds the smallest element from the unsorted portion and places it at the beginning.

Example:

```text
5 3 8 1 2
↓
1 3 8 5 2
↓
1 2 8 5 3
↓
1 2 3 5 8
```

## Basic Example

```cpp
#include <iostream>
using namespace std;

void selectionSort(int numbers[], int size) {
    for (int i = 0; i < size - 1; i++) {
        int minIndex = i;

        for (int j = i + 1; j < size; j++) {
            if (numbers[j] < numbers[minIndex]) {
                minIndex = j;
            }
        }

        swap(numbers[i], numbers[minIndex]);
    }
}

int main() {
    int numbers[] = {5, 3, 8, 1, 2};

    selectionSort(numbers, 5);

    for (int number : numbers) {
        cout << number << " ";
    }

    return 0;
}
```

### Output

```text
1 2 3 5 8
```

## Step-by-Step

Given:

```text
5 3 8 1 2
```

### Pass 1

Smallest value:

```text
1
```

Swap `5` and `1`:

```text
1 3 8 5 2
```

### Pass 2

Remaining:

```text
3 8 5 2
```

Smallest:

```text
2
```

Result:

```text
1 2 8 5 3
```

### Pass 3

Remaining:

```text
8 5 3
```

Smallest:

```text
3
```

Result:

```text
1 2 3 5 8
```

## Descending Order

```cpp
#include <iostream>
using namespace std;

void selectionSortDescending(int numbers[], int size) {
    for (int i = 0; i < size - 1; i++) {
        int maxIndex = i;

        for (int j = i + 1; j < size; j++) {
            if (numbers[j] > numbers[maxIndex]) {
                maxIndex = j;
            }
        }

        swap(numbers[i], numbers[maxIndex]);
    }
}

int main() {
    int numbers[] = {5, 3, 8, 1, 2};

    selectionSortDescending(numbers, 5);

    for (int number : numbers) {
        cout << number << " ";
    }

    return 0;
}
```

### Output

```text
8 5 3 2 1
```

## Complexity

|Case|Complexity|
|---|--:|
|Best|O(n²)|
|Average|O(n²)|
|Worst|O(n²)|
|Space|O(1)|

## Properties

- Simple to implement.
- Works without requiring sorted input.
- Uses constant extra space.
- Performs relatively few swaps.
- Standard selection sort is not stable.
- Inefficient for large datasets.
