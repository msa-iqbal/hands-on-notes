# constexpr

`constexpr` tells the compiler that a variable or function can be evaluated at compile time when its arguments and context allow it.

## Example 1: constexpr Variable

```cpp
#include <iostream>
using namespace std;

int main() {
    constexpr int size = 10;

    cout << size << endl;

    return 0;
}
```

## Example 2: constexpr Function

```cpp
#include <iostream>
using namespace std;

constexpr int square(int n) {
    return n * n;
}

int main() {
    constexpr int result = square(5);

    cout << result << endl;

    return 0;
}
```

### Output

```text
25
```

## Example 3: Runtime Argument

A `constexpr` function can also be called with a value that is not known at compile time.

```cpp
#include <iostream>
using namespace std;

constexpr int square(int n) {
    return n * n;
}

int main() {
    int number;

    cin >> number;

    cout << square(number) << endl;

    return 0;
}
```

The compiler can evaluate `square(number)` at runtime when `number` is runtime data.

## Example 4: constexpr Array Size

```cpp
#include <iostream>
using namespace std;

constexpr int getSize() {
    return 5;
}

int main() {
    int numbers[getSize()] = {1, 2, 3, 4, 5};

    for (int number : numbers) {
        cout << number << " ";
    }

    return 0;
}
```

## Example 5: constexpr Object

```cpp
#include <iostream>
using namespace std;

struct Point {
    int x;
    int y;

    constexpr Point(int x, int y)
        : x(x), y(y) {}
};

int main() {
    constexpr Point point(10, 20);

    cout << point.x << " " << point.y << endl;

    return 0;
}
```

## Example 6: constexpr Conditional

```cpp
#include <iostream>
using namespace std;

constexpr int absoluteValue(int value) {
    return value < 0 ? -value : value;
}

int main() {
    constexpr int result = absoluteValue(-25);

    cout << result << endl;

    return 0;
}
```

### Output

```text
25
```

## `const` vs `constexpr`

| Keyword     | Meaning                                 |
| ----------- | --------------------------------------- |
| `const`     | Cannot be modified after initialization |
| `constexpr` | Can be evaluated at compile time        |

Example:

```cpp
const int a = 10;
constexpr int b = 20;
```

## Key Points

- `constexpr` is used for compile-time evaluation.
- `constexpr` variables must be initialized with constant expressions.
- A `constexpr` function can also be used at runtime.
- `constexpr` is useful for constants and compile-time computations.
