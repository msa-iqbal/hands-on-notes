# std::optional

`std::optional` represents a value that may or may not exist.

It was introduced in **C++17**.

Include:

```cpp
#include <optional>
```

## Example 1: Basic optional

```cpp
#include <iostream>
#include <optional>
using namespace std;

int main() {
    optional<int> value = 100;

    cout << *value << endl;

    return 0;
}
```

### Output

```text
100
```

## Example 2: Empty optional

```cpp
#include <iostream>
#include <optional>
using namespace std;

int main() {
    optional<int> value;

    if (value.has_value()) {
        cout << *value << endl;
    } else {
        cout << "No value" << endl;
    }

    return 0;
}
```

### Output

```text
No value
```

## Example 3: nullopt

```cpp
#include <iostream>
#include <optional>
using namespace std;

int main() {
    optional<int> value = nullopt;

    if (!value) {
        cout << "Value is missing" << endl;
    }

    return 0;
}
```

## Example 4: value_or()

```cpp
#include <iostream>
#include <optional>
using namespace std;

int main() {
    optional<int> value;

    cout << value.value_or(100) << endl;

    return 0;
}
```

### Output

```text
100
```

## Example 5: Function Returning optional

```cpp
#include <iostream>
#include <optional>
using namespace std;

optional<int> divide(int a, int b) {
    if (b == 0) {
        return nullopt;
    }

    return a / b;
}

int main() {
    auto result = divide(10, 2);

    if (result) {
        cout << *result << endl;
    } else {
        cout << "Cannot divide by zero" << endl;
    }

    return 0;
}
```

### Output

```text
5
```

## Example 6: Missing Result

```cpp
#include <iostream>
#include <optional>
using namespace std;

optional<int> divide(int a, int b) {
    if (b == 0) {
        return nullopt;
    }

    return a / b;
}

int main() {
    auto result = divide(10, 0);

    cout << result.value_or(-1) << endl;

    return 0;
}
```

### Output

```text
-1
```

## Example 7: String Optional

```cpp
#include <iostream>
#include <optional>
#include <string>
using namespace std;

optional<string> findName(bool found) {
    if (found) {
        return "Alice";
    }

    return nullopt;
}

int main() {
    auto name = findName(true);

    if (name) {
        cout << *name << endl;
    }

    return 0;
}
```

## Key Points

- `std::optional<T>` may contain a `T` or no value.
- Use `has_value()` or boolean conversion to check it.
- `value()` accesses the contained value but throws if empty.
- `value_or()` provides a fallback.
- `std::nullopt` represents an empty optional.
