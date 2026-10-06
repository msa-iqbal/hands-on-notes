# Bubble Sort

Bubble sort repeatedly compares adjacent elements and swaps them when they are in the wrong order.

Example:

```text
5 3 8 1

5 > 3
3 5 8 1

5 < 8
3 5 8 1

8 > 1
3 5 1 8
```

After repeated passes:

```text
1 3 5 8
```

## Basic Example

```cpp
#include <iostream>
using namespace std;

void bubbleSort(int numbers[], int size) {
    for (int i = 0; i < size - 1; i++) {
        for (int j = 0; j < size - i - 1; j++) {
            if (numbers[j] > numbers[j + 1]) {
                int temp = numbers[j];

                numbers[j] = numbers[j + 1];
                numbers[j + 1] = temp;
            }
        }
    }
}

int main() {
    int numbers[] = {5, 3, 8, 1, 2};

    bubbleSort(numbers, 5);

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

## Optimized Bubble Sort

If no swaps happen during a pass, the array is already sorted.

```cpp
#include <iostream>
using namespace std;

void bubbleSort(int numbers[], int size) {
    for (int i = 0; i < size - 1; i++) {
        bool swapped = false;

        for (int j = 0; j < size - i - 1; j++) {
            if (numbers[j] > numbers[j + 1]) {
                swap(numbers[j], numbers[j + 1]);
                swapped = true;
            }
        }

        if (!swapped) {
            break;
        }
    }
}

int main() {
    int numbers[] = {1, 2, 3, 4, 5};

    bubbleSort(numbers, 5);

    for (int number : numbers) {
        cout << number << " ";
    }

    return 0;
}
```

### Output

```text
1 2 3 4 5
```

## Descending Order

Change:

```cpp
if (numbers[j] > numbers[j + 1])
```

to:

```cpp
if (numbers[j] < numbers[j + 1])
```

Complete example:

```cpp
#include <iostream>
using namespace std;

int main() {
    int numbers[] = {5, 2, 8, 1, 3};
    int size = 5;

    for (int i = 0; i < size - 1; i++) {
        for (int j = 0; j < size - i - 1; j++) {
            if (numbers[j] < numbers[j + 1]) {
                swap(numbers[j], numbers[j + 1]);
            }
        }
    }

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
|Best — optimized|O(n)|
|Average|O(n²)|
|Worst|O(n²)|
|Space|O(1)|

## Properties

- Simple
- In-place
- Stable when implemented with adjacent swaps
- Not efficient for large datasets
- Useful for learning sorting fundamentals
