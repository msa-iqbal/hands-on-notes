# std::variant

`std::variant` is a type-safe union that can store one value from a predefined set of types.

It was introduced in **C++17**.

```cpp
#include <variant>
```

## Example 1: Basic variant

```cpp
#include <iostream>
#include <variant>
using namespace std;

int main() {
    variant<int, string> value;

    value = 100;

    cout << get<int>(value) << endl;

    value = "Hello";

    cout << get<string>(value) << endl;

    return 0;
}
```

### Output

```text
100
Hello
```

## Example 2: holds_alternative()

```cpp
#include <iostream>
#include <variant>
using namespace std;

int main() {
    variant<int, string> value = 100;

    if (holds_alternative<int>(value)) {
        cout << "Integer value" << endl;
    }

    return 0;
}
```

### Output

```text
Integer value
```

## Example 3: get_if()

```cpp
#include <iostream>
#include <variant>
using namespace std;

int main() {
    variant<int, string> value = "Hello";

    if (auto ptr = get_if<string>(&value)) {
        cout << *ptr << endl;
    }

    return 0;
}
```

## Example 4: variant with Multiple Types

```cpp
#include <iostream>
#include <variant>
using namespace std;

int main() {
    variant<int, double, string> value;

    value = 10;
    cout << get<int>(value) << endl;

    value = 3.14;
    cout << get<double>(value) << endl;

    value = "C++";
    cout << get<string>(value) << endl;

    return 0;
}
```

## Example 5: std::visit()

`std::visit()` applies a callable to the currently active variant value.

```cpp
#include <iostream>
#include <variant>
using namespace std;

int main() {
    variant<int, string> value = 100;

    visit([](const auto& item) {
        cout << item << endl;
    }, value);

    value = "Hello";

    visit([](const auto& item) {
        cout << item << endl;
    }, value);

    return 0;
}
```

### Output

```text
100
Hello
```

## Example 6: Visitor With Different Behavior

```cpp
#include <iostream>
#include <variant>
using namespace std;

int main() {
    variant<int, string> value = 50;

    visit([](const auto& item) {
        cout << "Value: " << item << endl;
    }, value);

    return 0;
}
```

## Key Points

- `std::variant` stores exactly one active alternative.
- It is type-safe.
- `std::get<T>()` retrieves a value by type.
- `std::get_if<T>()` safely checks for a type.
- `std::holds_alternative<T>()` checks the active type.
- `std::visit()` applies a callable to the active value.
