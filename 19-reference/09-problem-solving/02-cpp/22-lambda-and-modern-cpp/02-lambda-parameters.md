# Lambda Parameters

Lambda functions can accept parameters just like normal functions.

## Basic Syntax

```cpp
auto lambda = [](type parameter) {
    // code
};
```

## Example 1: One Parameter

```cpp
#include <iostream>
using namespace std;

int main() {
    auto square = [](int n) {
        return n * n;
    };

    cout << square(6) << endl;

    return 0;
}
```

### Output

```text
36
```

## Example 2: Multiple Parameters

```cpp
#include <iostream>
using namespace std;

int main() {
    auto add = [](int a, int b) {
        return a + b;
    };

    cout << add(10, 20) << endl;

    return 0;
}
```

### Output

```text
30
```

## Example 3: Three Parameters

```cpp
#include <iostream>
using namespace std;

int main() {
    auto calculate = [](int a, int b, int c) {
        return a + b + c;
    };

    cout << calculate(10, 20, 30) << endl;

    return 0;
}
```

## Example 4: String Parameter

```cpp
#include <iostream>
#include <string>
using namespace std;

int main() {
    auto greet = [](const string& name) {
        cout << "Hello, " << name << endl;
    };

    greet("Alice");
    greet("Bob");

    return 0;
}
```

### Output

```text
Hello, Alice
Hello, Bob
```

## Example 5: Explicit Return Type

```cpp
#include <iostream>
using namespace std;

int main() {
    auto average = [](int a, int b) -> double {
        return (a + b) / 2.0;
    };

    cout << average(10, 15) << endl;

    return 0;
}
```

### Output

```text
12.5
```

## Example 6: Generic Lambda

C++14 allows lambda parameters to use `auto`.

```cpp
#include <iostream>
using namespace std;

int main() {
    auto print = [](auto value) {
        cout << value << endl;
    };

    print(100);
    print(3.14);
    print("Hello");

    return 0;
}
```

### Output

```text
100
3.14
Hello
```

## Example 7: Generic Addition

```cpp
#include <iostream>
using namespace std;

int main() {
    auto add = [](auto a, auto b) {
        return a + b;
    };

    cout << add(10, 20) << endl;
    cout << add(2.5, 3.5) << endl;

    return 0;
}
```

### Output

```text
30
6
```

## Key Points

- Lambda parameters are placed inside `()`.

- Multiple parameters are separated by commas.

- `auto` parameters create a generic lambda.

- `const string&` avoids unnecessary string copying.

- The return type can be explicitly specified using `->`.
