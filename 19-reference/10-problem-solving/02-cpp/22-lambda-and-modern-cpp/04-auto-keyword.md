# auto Keyword

The `auto` keyword allows the compiler to deduce a variable's type from its initializer.

## Basic Example

```cpp
#include <iostream>
using namespace std;

int main() {
    auto number = 10;
    auto price = 99.99;
    auto letter = 'A';

    cout << number << endl;
    cout << price << endl;
    cout << letter << endl;

    return 0;
}
```

### Output

```text
10
99.99
A
```

## Example 2: String

```cpp
#include <iostream>
#include <string>
using namespace std;

int main() {
    auto name = string("Alice");

    cout << name << endl;

    return 0;
}
```

## Example 3: Const auto

```cpp
#include <iostream>
using namespace std;

int main() {
    const auto number = 100;

    cout << number << endl;

    return 0;
}
```

## Example 4: auto With Iterators

```cpp
#include <iostream>
#include <vector>
using namespace std;

int main() {
    vector<int> numbers = {10, 20, 30, 40};

    for (auto it = numbers.begin(); it != numbers.end(); ++it) {
        cout << *it << " ";
    }

    return 0;
}
```

### Output

```text
10 20 30 40
```

## Example 5: auto With Lambda

Lambda types cannot normally be written explicitly.

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

## Example 6: auto Return Type

```cpp
#include <iostream>
using namespace std;

auto add(int a, int b) {
    return a + b;
}

int main() {
    cout << add(10, 20) << endl;

    return 0;
}
```

## Example 7: auto With References

```cpp
#include <iostream>
using namespace std;

int main() {
    int number = 10;

    auto& reference = number;

    reference = 50;

    cout << number << endl;

    return 0;
}
```

### Output

```text
50
```

## Example 8: const auto&

```cpp
#include <iostream>
#include <string>
using namespace std;

int main() {
    string name = "Alice";

    const auto& reference = name;

    cout << reference << endl;

    return 0;
}
```

## When to Use `auto`

Good use:

```cpp
auto iterator = numbers.begin();
auto lambda = [](int x) { return x * 2; };
auto value = calculate();
```

Avoid it when the explicit type makes the code substantially clearer:

```cpp
auto value = getSomething();
```

If the type is important to understanding the code, an explicit type may be preferable.

## Key Points

- `auto` performs compile-time type deduction.

- It does not mean dynamic typing.

- The type is determined during compilation.

- `auto` is especially useful with iterators and lambdas.

- `auto` can be combined with `const`, `&`, and `*`.
