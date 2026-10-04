# Divide and Conquer

**Divide and conquer** is an algorithmic strategy that breaks a problem into smaller subproblems, solves them, and combines their results.

The general pattern is:

```text
Divide
  ↓
Solve
  ↓
Combine
```

## Three Steps

### 1. Divide

Break the original problem into smaller problems.

### 2. Conquer

Solve each smaller problem, usually recursively.

### 3. Combine

Combine the solutions to produce the final result.

## Example

Merge sort follows this pattern:

```text
                Array
                  |
                Divide
              /       \
          Left         Right
           |             |
        Divide         Divide
           |             |
         Sort           Sort
              \       /
                Merge
                  |
              Sorted Array
```

## Maximum Element Using Divide and Conquer

```cpp
#include <iostream>
using namespace std;

int findMax(
    int numbers[],
    int left,
    int right
) {
    if (left == right) {
        return numbers[left];
    }

    int middle = left + (right - left) / 2;

    int leftMax = findMax(
        numbers,
        left,
        middle
    );

    int rightMax = findMax(
        numbers,
        middle + 1,
        right
    );

    return max(leftMax, rightMax);
}

int main() {
    int numbers[] = {
        10, 50, 20, 80, 30
    };

    cout << findMax(
        numbers,
        0,
        4
    ) << endl;

    return 0;
}
```

### Output

```text
80
```

## Binary Search as Divide and Conquer

Binary search divides the search space into two parts.

```text
[10 20 30 40 50 60 70]

             ↓

        [10 20 30]
             or
        [50 60 70]
```

Only one half needs to be searched.

## Merge Sort

Merge sort:

1. Divides the array.
2. Recursively sorts both halves.
3. Merges the sorted halves.

```cpp
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

    mergeSort(
        numbers,
        middle + 1,
        right
    );

    // Merge the two sorted halves.
}
```

## Quick Sort

Quick sort also uses divide and conquer.

```text
Array
  ↓
Choose Pivot
  ↓
Partition
 /       \
Left     Right
 ↓         ↓
Sort      Sort
```

## General Template

```cpp
void solve(int left, int right) {
    if (left >= right) {
        return;
    }

    int middle = left + (right - left) / 2;

    solve(left, middle);
    solve(middle + 1, right);

    // Combine results.
}
```

## Common Divide-and-Conquer Algorithms

|Algorithm|Divide|Combine|
|---|---|---|
|Binary Search|Search range|Usually none|
|Merge Sort|Array|Merge|
|Quick Sort|Partition|Usually implicit|
|Closest Pair|Point set|Combine closest results|
|Fast Exponentiation|Exponent|Multiply results|

## Complexity Idea

If an algorithm divides a problem into two roughly equal parts and performs linear work while combining, its recurrence may look like:

```text
T(n) = 2T(n/2) + O(n)
```

This leads to:

```text
O(n log n)
```

which is the complexity commonly associated with merge sort.

## Key Points

- Divide and conquer breaks large problems into smaller problems.
- Recursion is commonly used.
- The subproblems are usually independent.
- A combine step may be required.
- Binary search, merge sort, and quick sort are classic examples.
