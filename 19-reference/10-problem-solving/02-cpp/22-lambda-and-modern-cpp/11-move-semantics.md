# Move Semantics

**Move semantics** allow resources owned by one object to be transferred to another object instead of being copied.

Move semantics were introduced in **C++11**.

## Copy vs Move

Copy:

```text
Object A
   |
   | copy
   v
Object B
```

Move:

```text
Object A
   |
   | transfer resource
   v
Object B
```

## Example 1: Moving a String

```cpp
#include <iostream>
#include <string>
#include <utility>
using namespace std;

int main() {
    string first = "Hello";

    string second = move(first);

    cout << second << endl;

    return 0;
}
```

### Output

```text
Hello
```

After the move, `first` remains a valid object, but its exact contents should not be relied upon.

## Example 2: Copy

```cpp
#include <iostream>
#include <string>
using namespace std;

int main() {
    string first = "Hello";

    string second = first;

    cout << first << endl;
    cout << second << endl;

    return 0;
}
```

Both strings contain their own copy.

## Example 3: Move

```cpp
#include <iostream>
#include <string>
#include <utility>
using namespace std;

int main() {
    string first = "Hello";

    string second = move(first);

    cout << "Second: " << second << endl;

    return 0;
}
```

## Example 4: Vector Move

```cpp
#include <iostream>
#include <vector>
#include <utility>
using namespace std;

int main() {
    vector<int> first = {1, 2, 3, 4, 5};

    vector<int> second = move(first);

    cout << "Second: ";

    for (int value : second) {
        cout << value << " ";
    }

    return 0;
}
```

### Output

```text
Second: 1 2 3 4 5
```

## Example 5: Move Constructor

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

### Output

```text
Alice
```

## Example 6: Move Assignment

```cpp
#include <iostream>
#include <string>
#include <utility>
using namespace std;

int main() {
    string first = "Hello";
    string second = "World";

    second = move(first);

    cout << second << endl;

    return 0;
}
```

### Output

```text
Hello
```

## std::move

`std::move()` does not itself move the object.

It converts an expression into an rvalue expression, allowing move operations to be selected when available.

```cpp
string second = std::move(first);
```

## Important Rule

Do not assume the exact value of a moved-from object.

The moved-from object remains valid, but its state is generally unspecified unless the type documents more specific behavior.

## Key Points

- Move semantics avoid unnecessary resource copying.
- Introduced in C++11.
- `std::move()` enables move operations by casting to an rvalue.
- Move constructors use `Type&&`.
- Move assignment operators use `operator=(Type&&)`.
- A moved-from object remains valid but should not be assumed to contain its old value.
