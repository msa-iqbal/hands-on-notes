# Lambda Function

A **lambda function** is an anonymous function that can be created directly where it is needed.

## Basic Syntax

```cpp
[capture-list](parameters) -> return-type {
    // function body
};
```

The return type can usually be omitted because C++ can deduce it.

## Example 1: Basic Lambda

```cpp
#include <iostream>
using namespace std;

int main() {
    auto greet = []() {
        cout << "Hello, C++!" << endl;
    };

    greet();

    return 0;
}
```

### Output

```text
Hello, C++!
```

## Example 2: Lambda Without Parameters

```cpp
#include <iostream>
using namespace std;

int main() {
    auto message = []() {
        cout << "Learning Lambda Functions" << endl;
    };

    message();

    return 0;
}
```

## Example 3: Lambda With Return Value

```cpp
#include <iostream>
using namespace std;

int main() {
    auto add = []() {
        return 10 + 20;
    };

    cout << add() << endl;

    return 0;
}
```

### Output

```text
30
```

## Example 4: Explicit Return Type

```cpp
#include <iostream>
using namespace std;

int main() {
    auto divide = [](int a, int b) -> double {
        return static_cast<double>(a) / b;
    };

    cout << divide(10, 4) << endl;

    return 0;
}
```

### Output

```text
2.5
```

## Example 5: Lambda Used Immediately

```cpp
#include <iostream>
using namespace std;

int main() {
    []() {
        cout << "Lambda executed!" << endl;
    }();

    return 0;
}
```

## Example 6: Lambda as a Calculation

```cpp
#include <iostream>
using namespace std;

int main() {
    auto square = [](int n) {
        return n * n;
    };

    cout << square(5) << endl;
    cout << square(10) << endl;

    return 0;
}
```

### Output

```text
25
100
```

## Key Points

- Lambda functions are anonymous functions.
- `[]` is the capture list.
- `()` contains parameters.
- `{}` contains the function body.
- `auto` is commonly used to store a lambda.
- A lambda can return a value.
- A lambda can be passed to other functions.

## Syntax Summary

```cpp
auto name = [](parameters) {
    // code
};
```
