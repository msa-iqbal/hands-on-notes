# Capture List

The **capture list** allows a lambda to access variables from its surrounding scope.

## Basic Syntax

```cpp
[capture-list](parameters) {
    // body
};
```

## Example 1: Empty Capture List

```cpp
#include <iostream>
using namespace std;

int main() {
    int number = 10;

    auto show = []() {
        cout << "Lambda" << endl;
    };

    show();

    return 0;
}
```

A lambda with `[]` cannot directly access local variables from the surrounding scope.

## Example 2: Capture by Value

```cpp
#include <iostream>
using namespace std;

int main() {
    int number = 10;

    auto show = [number]() {
        cout << number << endl;
    };

    show();

    return 0;
}
```

### Output

```text
10
```

The lambda receives its own copy of `number`.

## Example 3: Capture by Reference

```cpp
#include <iostream>
using namespace std;

int main() {
    int number = 10;

    auto change = [&number]() {
        number = 50;
    };

    change();

    cout << number << endl;

    return 0;
}
```

### Output

```text
50
```

## Example 4: Capture Everything by Value

```cpp
#include <iostream>
using namespace std;

int main() {
    int a = 10;
    int b = 20;

    auto show = [=]() {
        cout << a + b << endl;
    };

    show();

    return 0;
}
```

### Output

```text
30
```

## Example 5: Capture Everything by Reference

```cpp
#include <iostream>
using namespace std;

int main() {
    int a = 10;
    int b = 20;

    auto change = [&]() {
        a = 100;
        b = 200;
    };

    change();

    cout << a << " " << b << endl;

    return 0;
}
```

### Output

```text
100 200
```

## Example 6: Mixed Capture

```cpp
#include <iostream>
using namespace std;

int main() {
    int a = 10;
    int b = 20;

    auto test = [a, &b]() {
        cout << a << endl;
        b = 100;
    };

    test();

    cout << b << endl;

    return 0;
}
```

### Output

```text
10
100
```

## Example 7: Mutable Lambda

A value capture is normally treated as `const` inside the lambda. `mutable` allows modification of the lambda's captured copy.

```cpp
#include <iostream>
using namespace std;

int main() {
    int number = 10;

    auto change = [number]() mutable {
        number = 50;
        cout << number << endl;
    };

    change();

    cout << number << endl;

    return 0;
}
```

### Output

```text
50
10
```

The original `number` is unchanged.

## Capture List Summary

|Capture|Meaning|
|---|---|
|`[]`|Capture nothing|
|`[x]`|Capture `x` by value|
|`[&x]`|Capture `x` by reference|
|`[=]`|Capture used local variables by value|
|`[&]`|Capture used local variables by reference|
|`[x, &y]`|Mixed capture|

## Key Points

- Capture by value creates a copy.

- Capture by reference accesses the original variable.

- `[=]` captures by value.

- `[&]` captures by reference.

- `mutable` allows modification of captured-by-value copies.

- Be careful when capturing references whose variables may go out of scope.
