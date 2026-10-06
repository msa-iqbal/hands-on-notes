# shared_ptr

`std::shared_ptr` provides shared ownership.

Multiple `shared_ptr` objects can own the same object.

## Basic Example

```cpp
#include <iostream>
#include <memory>

using namespace std;

int main() {
    auto first =
        make_shared<int>(100);

    auto second = first;

    cout << "Value: " << *first << endl;
    cout << "Value: " << *second << endl;

    cout << "Owners: "
         << first.use_count()
         << endl;

    return 0;
}
```

## Expected Output

```text
Value: 100
Value: 100
Owners: 2
```

## Reference Counting

A `shared_ptr` maintains a reference count representing the number of owning `shared_ptr` instances.

```cpp
auto first = make_shared<int>(100);

cout << first.use_count() << endl;

{
    auto second = first;

    cout << first.use_count() << endl;
}

cout << first.use_count() << endl;
```

## Expected Output

```text
1
2
1
```

When the last owning `shared_ptr` is destroyed, the managed object is destroyed.

## Shared Ownership

```text
        ┌──────────────┐
        │   Object     │
        └──────────────┘
          ↑          ↑
          │          │
      shared_ptr  shared_ptr
```

## Important

Do not use `shared_ptr` simply because it is convenient.

Use it when shared ownership is actually part of the design.

For exclusive ownership, prefer:

```cpp
unique_ptr
```
