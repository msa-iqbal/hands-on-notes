# Quick Sort

Quick sort is a divide-and-conquer sorting algorithm that selects a **pivot** and partitions the array around it.

Conceptually:

```text
5 2 8 1 7 3

Pivot = 3

values smaller than 3
        ↓
2 1

Pivot
 ↓
3

values larger than 3
        ↓
5 8 7
```

Then each partition is sorted recursively.

## Basic Implementation

```cpp
#include <iostream>
using namespace std;

int partitionArray(
    int numbers[],
    int low,
    int high
) {
    int pivot = numbers[high];

    int i = low - 1;

    for (int j = low; j < high; j++) {
        if (numbers[j] < pivot) {
            i++;

            swap(numbers[i], numbers[j]);
        }
    }

    swap(numbers[i + 1], numbers[high]);

    return i + 1;
}

void quickSort(
    int numbers[],
    int low,
    int high
) {
    if (low >= high) {
        return;
    }

    int pivotIndex = partitionArray(
        numbers,
        low,
        high
    );

    quickSort(numbers, low, pivotIndex - 1);
    quickSort(numbers, pivotIndex + 1, high);
}

int main() {
    int numbers[] = {
        5, 2, 8, 1, 7, 3
    };

    quickSort(numbers, 0, 5);

    for (int number : numbers) {
        cout << number << " ";
    }

    return 0;
}
```

### Output

```text
1 2 3 5 7 8
```

## Partition

Given:

```text
5 2 8 1 7 3
```

Pivot:

```text
3
```

After partitioning, the pivot is placed into its correct position:

```text
2 1 3 5 7 8
```

Then quick sort recursively processes:

```text
2 1
```

and:

```text
5 7 8
```

## Complexity

|Case|Complexity|
|---|--:|
|Best|O(n log n)|
|Average|O(n log n)|
|Worst|O(n²)|
|Average auxiliary space|O(log n)|
|Worst recursive depth|O(n)|

The worst case can occur when the pivot selection repeatedly produces highly unbalanced partitions.

## Properties

- Divide-and-conquer.
- Usually in-place apart from recursion/implementation details.
- Often fast in practice.
- Performance depends heavily on pivot selection.
- Standard simple quick sort is not stable.
