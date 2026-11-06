# Dynamic Array

A dynamic array is allocated at runtime using `new[]`.

It must be released using `delete[]`.

## Example

```cpp
#include <iostream>

using namespace std;

int main() {
    int size;

    cout << "Enter array size: ";
    cin >> size;

    int* numbers = new int[size];

    for (int i = 0; i < size; ++i) {
        numbers[i] = (i + 1) * 10;
    }

    cout << "Array: ";

    for (int i = 0; i < size; ++i) {
        cout << numbers[i] << " ";
    }

    cout << endl;

    delete[] numbers;
    numbers = nullptr;

    return 0;
}
```

## Example Input

```text
5
```

## Expected Output

```text
Enter array size: Array: 10 20 30 40 50
```

## Dynamic Array with Initialization

```cpp
int* numbers = new int[5]{10, 20, 30, 40, 50};

for (int i = 0; i < 5; ++i) {
    cout << numbers[i] << " ";
}

delete[] numbers;
```

## Important

For:

```cpp
new int;
```

use:

```cpp
delete pointer;
```

For:

```cpp
new int[size];
```

use:

```cpp
delete[] pointer;
```

Do not mix them.
