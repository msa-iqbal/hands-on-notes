# weak_ptr

`std::weak_ptr` is a non-owning smart pointer that observes an object managed by `std::shared_ptr`.

A `weak_ptr` does not increase the `shared_ptr` reference count.

## Basic Example

```cpp
#include <iostream>
#include <memory>

using namespace std;

int main() {
    auto shared =
        make_shared<int>(100);

    weak_ptr<int> weak = shared;

    cout << "Shared owners: "
         << shared.use_count()
         << endl;

    return 0;
}
```

## Expected Output

```text
Shared owners: 1
```

The `weak_ptr` does not become an additional owner.

## Accessing the Object

A `weak_ptr` cannot be directly dereferenced.

Use `lock()` to obtain a temporary `shared_ptr`.

```cpp
#include <iostream>
#include <memory>

using namespace std;

int main() {
    auto shared =
        make_shared<int>(100);

    weak_ptr<int> weak = shared;

    if (auto locked = weak.lock()) {
        cout << "Value: " << *locked << endl;
    }

    return 0;
}
```

## Expected Output

```text
Value: 100
```

## Expired Object

```cpp
#include <iostream>
#include <memory>

using namespace std;

int main() {
    weak_ptr<int> weak;

    {
        auto shared =
            make_shared<int>(100);

        weak = shared;

        cout << boolalpha
             << weak.expired()
             << endl;
    }

    cout << boolalpha
         << weak.expired()
         << endl;

    return 0;
}
```

## Expected Output

```text
false
true
```

## Common Use

`weak_ptr` is especially useful for breaking ownership cycles between `shared_ptr` objects.

```text
shared_ptr
    │
    ▼
  Object
    ▲
    │
weak_ptr
```

The weak reference observes the object without keeping it alive.
