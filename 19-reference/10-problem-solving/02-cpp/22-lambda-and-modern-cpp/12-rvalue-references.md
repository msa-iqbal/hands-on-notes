# Rvalue References

An **rvalue reference** is declared using `&&`.

```cpp
Type&& variable;
```

Rvalue references are a fundamental part of move semantics and perfect forwarding.

## Example 1: Basic Rvalue Reference

```cpp
#include <iostream>
using namespace std;

int main() {
    int&& value = 100;

    cout << value << endl;

    return 0;
}
```

### Output

```text
100
```

## Example 2: Cannot Bind Normal lvalue

```cpp
int number = 100;

// int&& value = number; // Error
```

An ordinary lvalue cannot directly bind to a non-const rvalue reference.

## Example 3: std::move

```cpp
#include <iostream>
#include <utility>
using namespace std;

int main() {
    int number = 100;

    int&& value = move(number);

    cout << value << endl;

    return 0;
}
```

## Example 4: Function Overloading

```cpp
#include <iostream>
using namespace std;

void show(int& value) {
    cout << "lvalue" << endl;
}

void show(int&& value) {
    cout << "rvalue" << endl;
}

int main() {
    int number = 10;

    show(number);
    show(20);

    return 0;
}
```

### Output

```text
lvalue
rvalue
```

## Example 5: Temporary Object

```cpp
#include <iostream>
#include <string>
using namespace std;

void show(string&& text) {
    cout << text << endl;
}

int main() {
    show("Hello");

    return 0;
}
```

### Output

```text
Hello
```

## Example 6: Class Move Constructor

```cpp
#include <iostream>
#include <string>
#include <utility>
using namespace std;

class Person {
private:
    string name;

public:
    Person(string name)
        : name(move(name)) {}

    Person(Person&& other) noexcept
        : name(move(other.name)) {}

    void show() const {
        cout << name << endl;
    }
};

int main() {
    Person first("Alice");

    Person second(move(first));

    second.show();

    return 0;
}
```

## lvalue vs rvalue

|Expression|Category|
|---|---|
|Named variable|lvalue|
|`10`|rvalue|
|`x`|lvalue|
|`x + y`|rvalue|
|`std::move(x)`|xvalue|

## Key Points

- `T&&` declares an rvalue reference.
- Rvalue references can bind to temporary values.
- They are used heavily by move constructors and move assignment.
- `std::move()` can convert an expression into an xvalue.
- Rvalue references are also important for perfect forwarding.
