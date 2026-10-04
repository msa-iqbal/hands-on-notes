# std::any

`std::any` can store a value of almost any copyable type.

It was introduced in **C++17**.

```cpp
#include <any>
```

## Example 1: Basic any

```cpp
#include <iostream>
#include <any>
using namespace std;

int main() {
    any value = 100;

    cout << any_cast<int>(value) << endl;

    return 0;
}
```

### Output

```text
100
```

## Example 2: Change Stored Type

```cpp
#include <iostream>
#include <any>
#include <string>
using namespace std;

int main() {
    any value = 100;

    cout << any_cast<int>(value) << endl;

    value = string("Hello");

    cout << any_cast<string>(value) << endl;

    return 0;
}
```

### Output

```text
100
Hello
```

## Example 3: has_value()

```cpp
#include <iostream>
#include <any>
using namespace std;

int main() {
    any value = 100;

    if (value.has_value()) {
        cout << "Value exists" << endl;
    }

    return 0;
}
```

## Example 4: Empty any

```cpp
#include <iostream>
#include <any>
using namespace std;

int main() {
    any value;

    if (!value.has_value()) {
        cout << "Empty" << endl;
    }

    return 0;
}
```

### Output

```text
Empty
```

## Example 5: reset()

```cpp
#include <iostream>
#include <any>
using namespace std;

int main() {
    any value = 100;

    cout << any_cast<int>(value) << endl;

    value.reset();

    cout << boolalpha << value.has_value() << endl;

    return 0;
}
```

### Output

```text
100
false
```

## Example 6: String

```cpp
#include <iostream>
#include <any>
#include <string>
using namespace std;

int main() {
    any value = string("C++ Programming");

    cout << any_cast<string>(value) << endl;

    return 0;
}
```

## Example 7: Wrong Type

The requested type must match the stored type.

```cpp
#include <iostream>
#include <any>
using namespace std;

int main() {
    any value = 100;

    try {
        cout << any_cast<double>(value) << endl;
    }
    catch (const bad_any_cast& e) {
        cout << "Invalid type" << endl;
    }

    return 0;
}
```

### Output

```text
Invalid type
```

## Key Points

- `std::any` can hold values of different types.
- The actual stored type is checked at runtime.
- `any_cast<T>()` retrieves the stored value.
- `has_value()` checks whether a value exists.
- `reset()` removes the stored value.
- Use `variant` when the allowed types are known and fixed.
