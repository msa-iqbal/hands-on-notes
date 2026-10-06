# unique_ptr

`std::unique_ptr` provides exclusive ownership of a dynamically allocated object.

Include:

```cpp
#include <memory>
```

## Basic Example

```cpp
#include <iostream>
#include <memory>

using namespace std;

int main() {
    unique_ptr<int> number =
        make_unique<int>(100);

    cout << "Value: " << *number << endl;

    return 0;
}
```

## Expected Output

```text
Value: 100
```

## Accessing Members

```cpp
#include <iostream>
#include <memory>

using namespace std;

class Student {
public:
    string name;

    void display() {
        cout << name << endl;
    }
};

int main() {
    auto student =
        make_unique<Student>();

    student->name = "Muhammad";

    student->display();

    return 0;
}
```

## Expected Output

```text
Muhammad
```

## unique_ptr Cannot Be Copied

This is not allowed:

```cpp
auto first = make_unique<int>(100);

// auto second = first;  // Error
```

Instead, ownership can be moved:

```cpp
auto first = make_unique<int>(100);

auto second = std::move(first);
```

Now `second` owns the object.

## Example

```cpp
#include <iostream>
#include <memory>

using namespace std;

int main() {
    auto first = make_unique<int>(100);

    auto second = move(first);

    cout << *second << endl;

    return 0;
}
```

## Expected Output

```text
100
```

After the move, `first` no longer owns the object.

## Key Property

```text
One object
    │
    └── one unique owner
```

Use `unique_ptr` when one owner should control the lifetime of an object.
