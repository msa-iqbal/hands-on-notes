# Merge Sort

Merge sort is a **divide-and-conquer** sorting algorithm.

It:

1. Divides the array into smaller parts.
2. Recursively sorts each part.
3. Merges the sorted parts.

Example:

```text
8 3 5 1 4 2

       ↓ divide

8 3 5       1 4 2

       ↓ divide

8 3    5    1 4    2

       ↓ sort

3 8    5    1 4    2

       ↓ merge

3 5 8       1 2 4

       ↓ merge

1 2 3 4 5 8
```

## Basic Implementation

```cpp
#include <iostream>
using namespace std;

void merge(
    int numbers[],
    int left,
    int middle,
    int right
) {
    int leftSize = middle - left + 1;
    int rightSize = right - middle;

    int* leftArray = new int[leftSize];
    int* rightArray = new int[rightSize];

    for (int i = 0; i < leftSize; i++) {
        leftArray[i] = numbers[left + i];
    }

    for (int i = 0; i < rightSize; i++) {
        rightArray[i] = numbers[middle + 1 + i];
    }

    int i = 0;
    int j = 0;
    int k = left;

    while (i < leftSize && j < rightSize) {
        if (leftArray[i] <= rightArray[j]) {
            numbers[k] = leftArray[i];
            i++;
        } else {
            numbers[k] = rightArray[j];
            j++;
        }

        k++;
    }

    while (i < leftSize) {
        numbers[k] = leftArray[i];
        i++;
        k++;
    }

    while (j < rightSize) {
        numbers[k] = rightArray[j];
        j++;
        k++;
    }

    delete[] leftArray;
    delete[] rightArray;
}

void mergeSort(
    int numbers[],
    int left,
    int right
) {
    if (left >= right) {
        return;
    }

    int middle = left + (right - left) / 2;

    mergeSort(numbers, left, middle);
    mergeSort(numbers, middle + 1, right);

    merge(numbers, left, middle, right);
}

int main() {
    int numbers[] = {
        8, 3, 5, 1, 4, 2
    };

    int size = 6;

    mergeSort(numbers, 0, size - 1);

    for (int number : numbers) {
        cout << number << " ";
    }

    return 0;
}
```

### Output

```text
1 2 3 4 5 8
```

## Merge Operation

Suppose we have two sorted arrays:

```text
Left:  2 5 8
Right: 1 4 7
```

Compare their first elements:

```text
2 vs 1 → choose 1
2 vs 4 → choose 2
5 vs 4 → choose 4
5 vs 7 → choose 5
8 vs 7 → choose 7
```

Result:

```text
1 2 4 5 7 8
```

## Using `vector`

A modern C++ implementation can use `vector`.

```cpp
#include <iostream>
#include <vector>
using namespace std;

void merge(
    vector<int>& numbers,
    int left,
    int middle,
    int right
) {
    vector<int> temp;

    int i = left;
    int j = middle + 1;

    while (i <= middle && j <= right) {
        if (numbers[i] <= numbers[j]) {
            temp.push_back(numbers[i]);
            i++;
        } else {
            temp.push_back(numbers[j]);
            j++;
        }
    }

    while (i <= middle) {
        temp.push_back(numbers[i]);
        i++;
    }

    while (j <= right) {
        temp.push_back(numbers[j]);
        j++;
    }

    for (int k = 0; k < static_cast<int>(temp.size()); k++) {
        numbers[left + k] = temp[k];
    }
}

void mergeSort(
    vector<int>& numbers,
    int left,
    int right
) {
    if (left >= right) {
        return;
    }

    int middle = left + (right - left) / 2;

    mergeSort(numbers, left, middle);
    mergeSort(numbers, middle + 1, right);

    merge(numbers, left, middle, right);
}

int main() {
    vector<int> numbers = {
        9, 4, 7, 3, 1, 8, 2
    };

    mergeSort(numbers, 0, numbers.size() - 1);

    for (int number : numbers) {
        cout << number << " ";
    }

    return 0;
}
```

### Output

```text
1 2 3 4 7 8 9
```

## Complexity

|Case|Complexity|
|---|--:|
|Best|O(n log n)|
|Average|O(n log n)|
|Worst|O(n log n)|
|Extra space|O(n)|

## Properties

- Divide-and-conquer algorithm.
- Predictable O(n log n) time.
- Stable when implemented appropriately.
- Requires additional memory for the merge operation.
- Useful for large datasets.
