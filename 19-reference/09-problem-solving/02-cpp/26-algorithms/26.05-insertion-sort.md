# Insertion Sort

Insertion sort builds the sorted portion of an array one element at a time.

It works similarly to arranging playing cards in your hand.

Example:

```text
5 3 8 1 2

5
3 5
3 5 8
1 3 5 8
1 2 3 5 8
```

## Basic Example

```cpp
#include <iostream>
using namespace std;

void insertionSort(int numbers[], int size) {
    for (int i = 1; i < size; i++) {
        int key = numbers[i];
        int j = i - 1;

        while (j >= 0 && numbers[j] > key) {
            numbers[j + 1] = numbers[j];
            j--;
        }

        numbers[j + 1] = key;
    }
}

int main() {
    int numbers[] = {5, 3, 8, 1, 2};

    insertionSort(numbers, 5);

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

Initial array:

```text
5 3 8 1 2
```

Take `3`:

```text
3 5 8 1 2
```

Take `8`:

```text
3 5 8 1 2
```

Take `1`:

```text
1 3 5 8 2
```

Take `2`:

```text
1 2 3 5 8
```

## Descending Order

```cpp
#include <iostream>
using namespace std;

void insertionSortDescending(int numbers[], int size) {
    for (int i = 1; i < size; i++) {
        int key = numbers[i];
        int j = i - 1;

        while (j >= 0 && numbers[j] < key) {
            numbers[j + 1] = numbers[j];
            j--;
        }

        numbers[j + 1] = key;
    }
}

int main() {
    int numbers[] = {5, 3, 8, 1, 2};

    insertionSortDescending(numbers, 5);

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

## Nearly Sorted Data

Insertion sort can perform well when the input is already nearly sorted.

Example:

```text
1 2 3 5 4 6 7
```

Only a small amount of movement is required to put `4` into its correct position.

## Complexity

|Case|Complexity|
|---|--:|
|Best|O(n)|
|Average|O(n²)|
|Worst|O(n²)|
|Space|O(1)|

## Properties

- Simple implementation.
- In-place.
- Stable.
- Efficient for small arrays.
- Efficient for nearly sorted data.
- Usually inefficient for large unsorted datasets.
