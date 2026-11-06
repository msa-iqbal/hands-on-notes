# Perfect Forwarding

**Perfect forwarding** allows a function template to preserve whether an argument was originally an lvalue or rvalue.

It commonly uses:

- forwarding references
- `std::forward`
- templates

## Example 1: lvalue and rvalue Overloads

```cpp
#include <iostream>
using namespace std;

void process(int& value) {
    cout << "lvalue" << endl;
}

void process(int&& value) {
    cout << "rvalue" << endl;
}

int main() {
    int number = 10;

    process(number);
    process(20);

    return 0;
}
```

### Output

```text
lvalue
rvalue
```

## Example 2: Forwarding Reference

```cpp
#include <iostream>
using namespace std;

template <typename T>
void wrapper(T&& value) {
    cout << value << endl;
}

int main() {
    int number = 10;

    wrapper(number);
    wrapper(20);

    return 0;
}
```

For a deduced `T&&` in this context, the parameter is a **forwarding reference**.

## Example 3: std::forward

```cpp
#include <iostream>
#include <utility>
using namespace std;

void process(int& value) {
    cout << "lvalue" << endl;
}

void process(int&& value) {
    cout << "rvalue" << endl;
}

template <typename T>
void wrapper(T&& value) {
    process(forward<T>(value));
}

int main() {
    int number = 10;

    wrapper(number);
    wrapper(20);

    return 0;
}
```

### Output

```text
lvalue
rvalue
```

## Example 4: String Forwarding

```cpp
#include <iostream>
#include <string>
#include <utility>
using namespace std;

void process(const string& value) {
    cout << "lvalue/string reference: " << value << endl;
}

void process(string&& value) {
    cout << "rvalue/string: " << value << endl;
}

template <typename T>
void wrapper(T&& value) {
    process(forward<T>(value));
}

int main() {
    string name = "Alice";

    wrapper(name);
    wrapper(string("Bob"));

    return 0;
}
```

## Example 5: Factory Function

```cpp
#include <iostream>
#include <string>
#include <utility>

using namespace std;

class Person {
public:
    Person(string name, int age) {
        cout << name << " " << age << endl;
    }
};

template <typename T, typename... Args>
T create(Args&&... args) {
    return T(forward<Args>(args)...);
}

int main() {
    auto person = create<Person>("Alice", 25);

    return 0;
}
```

### Output

```text
Alice 25
```

## Why `std::forward`?

Without forwarding:

```cpp
process(value);
```

`value` is a named variable inside the function and therefore behaves as an lvalue expression.

With forwarding:

```cpp
process(std::forward<T>(value));
```

the original value category can be preserved.

## Key Points

- Perfect forwarding preserves lvalue/rvalue category.
- Forwarding references commonly use `T&&` with type deduction.
- `std::forward<T>()` performs conditional forwarding.
- Perfect forwarding is widely used in generic libraries and factory functions.
