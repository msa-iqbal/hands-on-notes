# Smart Pointer Basics

Smart pointers are objects that manage dynamically allocated memory automatically.

They are provided by:

```cpp
#include <memory>
```

The main smart pointers are:

- `unique_ptr`

- `shared_ptr`

- `weak_ptr`

## unique_ptr

Owns an object exclusively.

```cpp
std::unique_ptr<int> number =
    std::make_unique<int>(100);
```

## shared_ptr

Allows multiple smart pointers to share ownership.

```cpp
std::shared_ptr<int> number =
    std::make_shared<int>(100);
```

## weak_ptr

Provides a non-owning reference to an object managed by `shared_ptr`.

```cpp
std::weak_ptr<int> observer;
```

## Basic Example

```cpp
#include <iostream>
#include <memory>

using namespace std;

int main() {
    unique_ptr<int> number =
        make_unique<int>(100);

    cout << *number << endl;

    return 0;
}
```

## Expected Output

```text
100
```

No explicit `delete` is required.

## Why Smart Pointers?

Traditional:

```cpp
int* number = new int(100);

delete number;
```

Modern:

```cpp
auto number = make_unique<int>(100);
```

The second approach uses RAII to manage the lifetime automatically.
